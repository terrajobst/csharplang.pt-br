---
ms.openlocfilehash: b0d0fa70d90f7493c6c23be576155a77cec36cf8
ms.sourcegitcommit: f61a06970fa0562d2e40363fae3948eb168624ca
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/14/2020
ms.locfileid: "79485178"
---
# <a name="default-interface-methods"></a>métodos de interface padrão

* [x] proposta
* [] Protótipo: [em andamento](https://github.com/dotnet/roslyn/blob/master/docs/features/DefaultInterfaceImplementation.md)
* [] Implementação: nenhuma
* [] Especificação: em andamento, abaixo

## <a name="summary"></a>Resumo
[summary]: #summary

Adicionar suporte para _métodos de extensão virtual_ – métodos em interfaces com implementações concretas. Uma classe ou estrutura que implementa tal interface é necessária para ter uma única implementação _mais específica_ para o método de interface, implementada pela classe ou struct, ou herdada de suas classes ou interfaces base. Os métodos de extensão virtual permitem que um autor de API adicione métodos a uma interface em versões futuras sem perder a origem ou a compatibilidade binária com as implementações existentes dessa interface.

Eles são semelhantes aos ["métodos padrão"](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html)do Java.

(Com base na provável técnica de implementação), esse recurso requer suporte correspondente no CLI/CLR. Os programas que tiram proveito desse recurso não podem ser executados em versões anteriores da plataforma.

## <a name="motivation"></a>Motivação
[motivation]: #motivation

As principais motivações para esse recurso são

- Os métodos de interface padrão permitem que um autor de API adicione métodos a uma interface em versões futuras sem perder a origem ou a compatibilidade binária com as implementações existentes dessa interface.
- O recurso permite C# interoperar com APIs voltadas para [Android (Java)](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html) e [Ios (Swift)](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-ID267), que dão suporte a recursos semelhantes.
- Como acontece, adicionar implementações de interface padrão fornece os elementos do recurso de linguagem "características" (<https://en.wikipedia.org/wiki/Trait_(computer_programming)>). As características comprovaram ser uma técnica de programação avançada (<http://scg.unibe.ch/archive/papers/Scha03aTraits.pdf>).

## <a name="detailed-design"></a>Design detalhado
[design]: #detailed-design

A sintaxe de uma interface é estendida para permitir

- declarações de membro que declaram constantes, operadores, construtores estáticos e tipos aninhados;
- um *corpo* para um método, um indexador, uma propriedade ou um acessador de evento (ou seja, uma implementação "padrão");
- declarações de membro que declaram campos estáticos, métodos, propriedades, indexadores e eventos;
- declarações de membro usando a sintaxe de implementação de interface explícita; e
- Modificadores de acesso explícitos (o acesso padrão é `public`).

Membros com corpos permitem que a interface forneça uma implementação "padrão" para o método em classes e estruturas que não fornecem uma implementação de substituição.

As interfaces não podem conter o estado da instância. Embora os campos estáticos agora sejam permitidos, os campos de instância não são permitidos em interfaces. Não há suporte para propriedades automáticas de instância em interfaces, pois elas declarariam implicitamente um campo oculto.

Os métodos estáticos e privados permitem a refatoração útil e a organização do código usado para implementar a API pública da interface.

Uma substituição de método em uma interface deve usar a sintaxe de implementação de interface explícita.

É um erro declarar um tipo de classe, tipo de struct ou tipo de enumeração dentro do escopo de um parâmetro de tipo que foi declarado com um *variance_annotation*.  Por exemplo, a declaração de `C` abaixo é um erro.

```csharp
interface IOuter<out T>
{
    class C { } // error: class declaration within the scope of variant type parameter 'T'
}
```

### <a name="concrete-methods-in-interfaces"></a>Métodos concretos em interfaces

A forma mais simples desse recurso é a capacidade de declarar um *método concreto* em uma interface, que é um método com um corpo.

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
```

Uma classe que implementa essa interface não precisa implementar seu método concreto.

```csharp
class C : IA { } // OK

IA i = new C();
i.M(); // prints "IA.M"
```

A substituição final para `IA.M` na classe `C` é o método concreto `M` declarado em `IA`. Observe que uma classe não herda membros de suas interfaces; Isso não é alterado por esse recurso:

```csharp
new C().M(); // error: class 'C' does not contain a member 'M'
```

Dentro de um membro de instância de uma interface, `this` tem o tipo da interface delimitadora.

### <a name="modifiers-in-interfaces"></a>Modificadores em interfaces

A sintaxe de uma interface é relaxada para permitir modificadores em seus membros. Os itens a seguir são permitidos: `private`, `protected`, `internal`, `public`, `virtual`, `abstract`, `sealed`, `static`, `extern`e `partial`.

> ***Todo***: Verifique quais outros modificadores existem.

Um membro de interface cuja declaração inclui um corpo é um membro `virtual`, a menos que o modificador `sealed` ou `private` seja usado. O modificador de `virtual` pode ser usado em um membro de função que, de outra forma, seria implicitamente `virtual`. Da mesma forma, embora `abstract` seja o padrão em membros de interface sem corpos, esse modificador pode ser fornecido explicitamente. Um membro não virtual pode ser declarado usando a palavra-chave `sealed`.

É um erro para um membro de função `private` ou `sealed` de uma interface não ter corpo. Um membro da função `private` pode não ter o modificador `sealed`.

Os modificadores de acesso podem ser usados em membros de interface de todos os tipos de membros que são permitidos. O nível de acesso `public` é o padrão, mas pode ser fornecido explicitamente.

> ***Abrir problema:*** Precisamos especificar o significado preciso dos modificadores de acesso, como `protected` e `internal`, e quais declarações fazem e não os substituem (em uma interface derivada) ou os implementam (em uma classe que implementa a interface).

As interfaces podem declarar `static` Membros, incluindo tipos aninhados, métodos, indexadores, propriedades, eventos e construtores estáticos. O nível de acesso padrão para todos os membros da interface é `public`.

As interfaces não podem declarar construtores, destruidores ou campos de instância.

> ***Problema fechado:*** As declarações de operador devem ser permitidas em uma interface? Provavelmente não há operadores de conversão, mas e quanto a outros? ***Decisão***: operadores são permitidos *, exceto* para operadores de conversão, igualdade e desigualdade.

> ***Problema fechado:*** Deve `new` ser permitido em declarações de membro de interface que ocultam membros de interfaces base? ***Decisão***: Sim.

> ***Problema fechado:*** No momento, não permitimos `partial` em uma interface ou seus membros. Isso exigiria uma proposta separada. ***Decisão***: Sim. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>

### <a name="overrides-in-interfaces"></a>Substituições em interfaces

As declarações de substituição (ou seja, aquelas que contêm o modificador de `override`) permitem que o programador forneça uma implementação mais específica de um membro virtual em uma interface em que o compilador ou tempo de execução não teria, de outra forma, encontrar um. Ele também permite transformar um membro abstrato de uma superinterface em um membro padrão em uma interface derivada. Uma declaração de substituição tem permissão para substituir *explicitamente* um método de interface base específico qualificando a declaração com o nome da interface (nenhum modificador de acesso é permitido nesse caso). Substituições implícitas não são permitidas.

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    override void IA.M() { WriteLine("IB.M"); } // explicitly named
}
interface IC : IA
{
    override void M() { WriteLine("IC.M"); } // implicitly named
}
```

Declarações de substituição em interfaces não podem ser declaradas `sealed`.

Os membros da função pública `virtual` em uma interface podem ser substituídos em uma interface derivada explicitamente (qualificando o nome na declaração de substituição com o tipo de interface que declarou originalmente o método e omitindo um modificador de acesso).

`virtual` membros de função em uma interface só podem ser substituídos explicitamente (não implicitamente) em interfaces derivadas e os membros que não são `public` só podem ser implementados em uma classe ou estrutura explicitamente (não implicitamente). Em ambos os casos, o membro substituído ou implementado deve estar *acessível* onde é substituído.

### <a name="reabstraction"></a>Reabstração

Um método virtual (concreto) declarado em uma interface pode ser substituído para ser abstrato em uma interface derivada

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    abstract void IA.M();
}
class C : IB { } // error: class 'C' does not implement 'IA.M'.
```

O modificador de `abstract` não é necessário na declaração de `IB.M` (que é o padrão nas interfaces), mas provavelmente é uma boa prática ser explícita em uma declaração de substituição.

Isso é útil em interfaces derivadas em que a implementação padrão de um método é inadequada e uma implementação mais apropriada deve ser fornecida pela implementação de classes.

> ***Abrir problema:*** A reabstração deve ser permitida?

### <a name="the-most-specific-override-rule"></a>A regra de substituição mais específica

Exigimos que cada interface e classe tenham uma *substituição mais específica* para cada membro virtual entre as substituições que aparecem no tipo ou suas interfaces diretas e indiretas. A *substituição mais específica* é uma substituição exclusiva que é mais específica do que todas as outras substituições. Se não houver nenhuma substituição, o membro em si será considerado a substituição mais específica.

Uma substituição `M1` é considerada *mais específica* do que outra `M2` de substituição se `M1` é declarado no tipo `T1`, `M2` é declarado no tipo `T2`e qualquer um

1. `T1` contém `T2` entre suas interfaces diretas ou indiretas, ou
2. `T2` é um tipo de interface, mas `T1` não é um tipo de interface.

Por exemplo:

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    void IA.M() { WriteLine("IB.M"); }
}
interface IC : IA
{
    void IA.M() { WriteLine("IC.M"); }
}
interface ID : IB, IC { } // error: no most specific override for 'IA.M'
abstract class C : IB, IC { } // error: no most specific override for 'IA.M'
abstract class D : IA, IB, IC // ok
{
    public abstract void M();
}

```

A regra de substituição mais específica garante que um conflito (ou seja, uma ambiguidade resultante da herança de losango) seja resolvido explicitamente pelo programador no ponto em que surge o conflito.

Como damos suporte a substituições abstratas explícitas em interfaces, poderíamos fazer isso em classes também

```csharp
abstract class E : IA, IB, IC // ok
{
    abstract void IA.M();
}
```

> ***Problema aberto***: devemos dar suporte a substituições abstratas de interface explícita em classes?

Além disso, é um erro se, em uma declaração de classe, a substituição mais específica de algum método de interface for uma substituição abstrata que foi declarada em uma interface. Esta é uma regra existente renovada usando a nova terminologia.

```csharp
interface IF
{
    void M();
}
abstract class F : IF { } // error: 'F' does not implement 'IF.M'
```

É possível que uma propriedade virtual declarada em uma interface tenha uma substituição mais específica para seu acessador de `get` em uma interface e uma substituição mais específica para seu acessador de `set` em uma interface diferente. Isso é considerado uma violação da regra de *substituição mais específica* .

### <a name="static-and-private-methods"></a>Métodos `static` e `private`

Como as interfaces agora podem conter código executável, é útil abstrair o código comum em métodos privados e estáticos. Agora, permitimos isso em interfaces.

> ***Problema fechado***: devemos dar suporte a métodos privados? Devemos dar suporte a métodos estáticos? **Decisão: Sim**

> ***Problema de abertura***: devemos permitir que os métodos de interface sejam `protected` ou `internal` ou outro acesso? Em caso afirmativo, quais são as semânticas? Eles são `virtual` por padrão? Nesse caso, há uma maneira de torná-los não virtuais?

> ***Problema aberto***: se damos suporte a métodos estáticos, devemos dar suporte a operadores (estáticos)?

### <a name="base-interface-invocations"></a>Invocações de interface base

O código em um tipo derivado de uma interface com um método padrão pode invocar explicitamente a implementação de "base" dessa interface.

```csharp
interface I0
{
   void M() { Console.WriteLine("I0"); }
}
interface I1 : I0
{
   override void M() { Console.WriteLine("I1"); }
}
interface I2 : I0
{
   override void M() { Console.WriteLine("I2"); }
}
interface I3 : I1, I2
{
   // an explicit override that invoke's a base interface's default method
   void I0.M() { I2.base.M(); }
}

```

Um método de instância (não estático) é permitido para invocar a implementação de um método de instância acessível em uma interface base direta não virtualmente, nomeando-a usando a sintaxe `base(Type).M`. Isso é útil quando uma substituição necessária para ser fornecida devido à herança de losango é resolvida pela delegação a uma implementação de base específica.

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    override void IA.M() { WriteLine("IB.M"); }
}
interface IC : IA
{
    override void IA.M() { WriteLine("IC.M"); }
}

class D : IA, IB, IC
{
    void IA.M() { base(IB).M(); }
}
```

Quando um membro de `virtual` ou `abstract` é acessado usando a sintaxe `base(Type).M`, é necessário que `Type` contenha uma *substituição exclusiva mais específica* para `M`.

### <a name="binding-base-clauses"></a>Vinculando cláusulas base

Agora, as interfaces contêm tipos.  Esses tipos podem ser usados na cláusula base como interfaces base.  Ao ligar uma cláusula base, talvez seja necessário saber o conjunto de interfaces base para associar esses tipos (por exemplo, para pesquisar neles e resolver o acesso protegido).  O significado da cláusula base de uma interface é, portanto, definido circularmente.  Para interromper o ciclo, adicionamos novas regras de idioma correspondentes a uma regra semelhante já em vigor para classes.

Ao determinar o significado da *interface_base* de uma interface, as interfaces base são temporariamente consideradas vazias. Intuitivamente, isso garante que o significado de uma cláusula base não possa ser recursivamente dependente de si mesma. 

**Nós usamos as seguintes regras:**

"Quando uma classe B deriva de uma classe A, é um erro de tempo de compilação para um para depender de B. Uma classe **depende diretamente** de sua classe base direta (se houver) e **depende diretamente** da ~~**classe**~~ na qual ela é imediatamente aninhada (se houver). Dada essa definição, o conjunto completo de ~~**classes**~~ sobre as quais uma classe depende é o fechamento reflexivo e transitivo da relação **depende diretamente** de.

É um erro de tempo de compilação para uma interface herdar direta ou indiretamente de si mesma.
As **interfaces base** de uma interface são as interfaces base explícitas e suas interfaces base. Em outras palavras, o conjunto de interfaces base é o fechamento transitivo completo das interfaces base explícitas, suas interfaces base explícitas e assim por diante.

**Estamos ajustando-os da seguinte maneira:**

Quando uma classe B deriva de uma classe A, é um erro de tempo de compilação para um para depender de B. Uma classe **depende diretamente** de sua classe base direta (se houver) e **depende diretamente** do _**tipo**_ no qual ele é imediatamente aninhado (se houver).

Quando uma interface IB estende uma interface IA, é um erro de tempo de compilação para IA depender da IB. Uma interface **depende diretamente** de suas interfaces base diretas (se houver) e **depende diretamente** do tipo no qual ele é imediatamente aninhado (se houver).

Dadas essas definições, o conjunto completo de **tipos** sobre os quais um tipo depende é o fechamento reflexivo e transitivo da relação **depende diretamente** de.

### <a name="effect-on-existing-programs"></a>Efeito em programas existentes

As regras apresentadas aqui destinam-se a não afetar o significado dos programas existentes.

Exemplo 1:

```csharp
interface IA
{
    void M();
}
class C: IA // Error: IA.M has no concrete most specific override in C
{
    public static void M() { } // method unrelated to 'IA.M' because static
}
```

Exemplo 2:

```csharp
interface IA
{
    void M();
}
class Base: IA
{
    void IA.M() { }
}
class Derived: Base, IA // OK, all interface members have a concrete most specific override
{
    private void M() { } // method unrelated to 'IA.M' because private
}
```

As mesmas regras fornecem resultados semelhantes à situação análoga que envolve métodos de interface padrão:

```csharp
interface IA
{
    void M() { }
}
class Derived: IA // OK, all interface members have a concrete most specific override
{
    private void M() { } // method unrelated to 'IA.M' because private
}
```

> ***Problema fechado***: Confirme se essa é uma conseqüência pretendida da especificação. **Decisão: Sim**

### <a name="runtime-method-resolution"></a>Resolução do método de tempo de execução

> ***Problema fechado:*** A especificação deve descrever o algoritmo de resolução do método de tempo de execução na face dos métodos padrão de interface. Precisamos garantir que a semântica seja consistente com a semântica da linguagem, por exemplo, quais métodos declarados fazem e não substituem ou implementam um método `internal`.

### <a name="clr-support-api"></a>API de suporte CLR

Para que os compiladores detectem quando estão compilando para um tempo de execução que dá suporte a esse recurso, as bibliotecas para tais tempos de execução são modificadas para anunciar esse fato por meio da API discutida em <https://github.com/dotnet/corefx/issues/17116>. Adicionamos

```csharp
namespace System.Runtime.CompilerServices
{
    public static class RuntimeFeature
    {
        // Presence of the field indicates runtime support
        public const string DefaultInterfaceImplementation = nameof(DefaultInterfaceImplementation);
    }
}
```

> ***Abrir problema***: é o melhor nome para o recurso *CLR* ? O recurso CLR faz muito mais do que apenas isso (por exemplo, libera as restrições de proteção, dá suporte a substituições em interfaces, etc). Talvez ele deva ser chamado de algo como "métodos concretos em interfaces" ou "características"?

### <a name="further-areas-to-be-specified"></a>Outras áreas a serem especificadas

- [] Seria útil catalogar os tipos de efeitos de compatibilidade binária e de origem causados pela adição de métodos de interface padrão e substituições a interfaces existentes.

## <a name="drawbacks"></a>Desvantagens
[drawbacks]: #drawbacks

Essa proposta requer uma atualização coordenada para a especificação do CLR (para dar suporte a métodos concretos em interfaces e resolução de métodos). Portanto, é razoavelmente "caro" e pode valer a pena fazer isso em combinação com outros recursos que também antecipamos a necessidade de alterações no CLR.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

None.

## <a name="unresolved-questions"></a>Perguntas não resolvidas
[unresolved]: #unresolved-questions

- As perguntas abertas são chamadas em toda a proposta, acima.
- Consulte também <https://github.com/dotnet/csharplang/issues/406> para obter uma lista de perguntas abertas.
- A especificação detalhada deve descrever o mecanismo de resolução usado em tempo de execução para selecionar o método preciso a ser invocado.
- A interação dos metadados produzidos por novos compiladores e consumidos por compiladores mais antigos precisa ser realizada em detalhes. Por exemplo, precisamos garantir que a representação de metadados que usamos não cause a adição de uma implementação padrão em uma interface para interromper uma classe existente que implementa essa interface quando compilada por um compilador mais antigo. Isso pode afetar a representação de metadados que podemos usar.
- O design deve considerar a interoperação com outras linguagens e compiladores existentes para outras linguagens.

## <a name="resolved-questions"></a>Perguntas resolvidas

### <a name="abstract-override"></a>Substituição abstrata

A especificação de rascunho anterior continha a capacidade de "reabstrair" um método herdado:

```csharp
interface IA
{
    void M();
}
interface IB : IA
{
    override void M() { }
}
interface IC : IB
{
    override void M(); // make it abstract again
}
```

Minhas notas para 2017-03-20 mostraram que decidimos não permitir isso. No entanto, há pelo menos dois casos de uso para ele:

1. As APIs do Java, com as quais alguns usuários desse recurso esperamos interoperar dependem dessa instalação.
2. A programação com *características* se beneficia disso. A reabstração é um dos elementos do recurso de linguagem "características" (https://en.wikipedia.org/wiki/Trait_(computer_programming)). O seguinte é permitido com classes:

```csharp
public abstract class Base
{
    public abstract void M();
}
public abstract class A : Base
{
    public override void M() { }
}
public abstract class B : A
{
    public override abstract void M(); // reabstract Base.M
}
```

Infelizmente, esse código não pode ser refatorado como um conjunto de interfaces (características), a menos que isso seja permitido. Pelo *princípio de Jared de ganância*, ele deve ser permitido.

> ***Problema fechado:*** A reabstração deve ser permitida? Ok Minhas anotações estavam erradas. As [observações do LDM](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md) dizem que a reabstração é permitida em uma interface. Não está em uma classe.

### <a name="virtual-modifier-vs-sealed-modifier"></a>Modificador e modificador virtual vs lacrado

De [Aleksey Tsingauz](https://github.com/AlekseyTs):

> Decidimos permitir que modificadores explicitamente declarados em membros de interface, a menos que haja um motivo para não permitir alguns deles. Isso traz uma pergunta interessante sobre o modificador virtual. Eles devem ser necessários em membros com implementação padrão?
>
> Poderíamos dizer que:
>
> - Se não houver nenhuma implementação e nenhum virtual, nem lacrado for especificado, supomos que o membro é abstrato.
> - Se houver uma implementação e nenhum resumo, nem lacrado for especificado, supomos que o membro é virtual.
> - o modificador lacrado é necessário para tornar um método não virtual nem abstrato.
>
> Como alternativa, poderíamos dizer que o modificador virtual é necessário para um membro virtual. Ou seja, se houver um membro com implementação não explicitamente marcado com modificador virtual, ele não será virtual nem abstrato. Essa abordagem pode proporcionar uma melhor experiência quando um método é movido de uma classe para uma interface:
>
> - um método abstrato permanece abstrato.
> - um método virtual permanece virtual.
> - um método sem nenhum modificador não permanece virtual nem abstract.
> - o modificador lacrado não pode ser aplicado a um método que não seja uma substituição.
>
> O que você acha?

> ***Problema fechado:*** Um método concreto (com implementação) deve ser implicitamente `virtual`? Ok

***Decisões:*** Feitas no LDM 2017-04-05:

1. Não virtual deve ser expresso explicitamente por meio de `sealed` ou `private`.
2. `sealed` é a palavra-chave para tornar membros da instância da interface com corpos não virtuais
3. Queremos permitir todos os modificadores em interfaces  
4. A acessibilidade padrão para membros de interface é pública, incluindo tipos aninhados
5. Membros de função privada em interfaces são lacrados implicitamente e `sealed` não são permitidos neles.
6. As classes privadas (em interfaces) são permitidas e podem ser seladas, e isso significa lacrado na classe sensação de sealed.
7. Uma boa proposta está ausente, parcial ainda não é permitida em interfaces ou seus membros.

### <a name="binary-compatibility-1"></a>Compatibilidade binária 1

Quando uma biblioteca fornece uma implementação padrão

```csharp
interface I1
{
    void M() { Impl1 }
}
interface I2 : I1
{
}
class C : I2
{
}
```

Entendemos que a implementação de `I1.M` no `C` é `I1.M`. E se o assembly que contém `I2` for alterado da seguinte maneira e recompilada

```csharp
interface I2 : I1
{
    override void M() { Impl2 }
}
```

Mas `C` não é recompilado. O que acontece quando o programa é executado? Uma invocação de `(C as I1).M()`

1. Executa `I1.M`
2. Executa `I2.M`
3. Gera algum tipo de erro de tempo de execução

***Decisão:*** Tornou-se 2017-04-11: executa `I2.M`, que é a substituição mais específica, sem ambigüidade, no tempo de execução.

### <a name="event-accessors-closed"></a>Acessadores de evento (fechados)

> ***Problema fechado:*** Um evento pode ser substituído "piecewise"?

Considere este caso:

```csharp
public interface I1
{
    event T e1;
}
public interface I2 : I1
{
    override event T
    {
        add { }
        // error: "remove" accessor missing
    }
}
```

Essa implementação "parcial" do evento não é permitida porque, como em uma classe, a sintaxe de uma declaração de evento não permite apenas um acessador; ambos (ou nenhum) devem ser fornecidos. Você poderia realizar a mesma coisa permitindo que o acessador remove abstract na sintaxe seja implicitamente abstrato pela ausência de um corpo:

```csharp
public interface I1
{
    event T e1;
}
public interface I2 : I1
{
    override event T
    {
        add { }
        remove; // implicitly abstract
    }
}
```

Observe que *essa é uma sintaxe nova (proposta)* . Na gramática atual, os acessadores de evento têm um corpo obrigatório.

> ***Problema fechado:*** Um acessador de evento pode ser (implicitamente) abstrato pela omissão de um corpo, da mesma forma que os métodos em interfaces e acessadores de propriedade são (implicitamente) abstratos pela omissão de um corpo?

***Decisão:*** (2017-04-18) não, as declarações de evento exigem tanto acessadores concretos (ou nenhum).

### <a name="reabstraction-in-a-class-closed"></a>Reabstração em uma classe (fechada)

***Problema fechado:*** Devemos confirmar que isso é permitido (caso contrário, adicionar uma implementação padrão seria uma alteração significativa):

```csharp
interface I1
{
    void M() { }
}
abstract class C : I1
{
    public abstract void M(); // implement I1.M with an abstract method in C
}
```

***Decisão:*** (2017-04-18) Sim, a adição de um corpo a uma declaração de membro de interface não deve quebrar C.

### <a name="sealed-override-closed"></a>Substituição lacrada (fechada)

A pergunta anterior pressupõe implicitamente que o modificador de `sealed` possa ser aplicado a um `override` em uma interface. Isso contradiz a especificação de rascunho. Queremos permitir o lacre de uma substituição? Devem ser considerados efeitos de compatibilidade de origem e binário de lacre.

> ***Problema fechado:*** Devemos permitir lacrar uma substituição?

***Decisão:*** (2017-04-18) não é permitido `sealed` em substituições em interfaces. O único uso de `sealed` em membros de interface é torná-los não virtuais em sua declaração inicial.

### <a name="diamond-inheritance-and-classes-closed"></a>Herança de diamante e classes (fechadas)

O rascunho da proposta prefere substituições de classe a substituições de interface em cenários de herança em losango:

> Exigimos que cada interface e classe tenham uma *substituição mais específica* para cada método de interface entre as substituições que aparecem no tipo ou suas interfaces diretas e indiretas. A *substituição mais específica* é uma substituição exclusiva que é mais específica do que todas as outras substituições. Se não houver nenhuma substituição, o próprio método será considerado a substituição mais específica.
>
> Uma substituição `M1` é considerada *mais específica* do que outra `M2` de substituição se `M1` é declarado no tipo `T1`, `M2` é declarado no tipo `T2`e qualquer um
>
> 1. `T1` contém `T2` entre suas interfaces diretas ou indiretas, ou
> 2. `T2` é um tipo de interface, mas `T1` não é um tipo de interface.

O cenário é este

```csharp
interface IA
{
    void M();
}
interface IB : IA
{
    override void M() { WriteLine("IB"); }
}
class Base : IA
{
    void IA.M() { WriteLine("Base"); }
}
class Derived : Base, IB // allowed?
{
    static void Main()
    {
        Ia a = new Derived();
        a.M();           // what does it do?
    }
}
```

Devemos confirmar esse comportamento (ou decidir de outra forma)

> ***Problema fechado:*** Confirme a especificação de rascunho, acima, para a *substituição mais específica* , pois ela se aplica a classes e interfaces mistas (uma classe tem prioridade sobre uma interface). Consulte <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#diamonds-with-classes>.

### <a name="interface-methods-vs-structs-closed"></a>Métodos de interface vs structs (fechados)

Há algumas interações de infeliz entre as estruturas e os métodos de interface padrão.

```csharp
interface IA
{
    public void M() { }
}
struct S : IA
{
}
```

Observe que os membros da interface não são herdados:

```csharp
var s = default(S);
s.M(); // error: 'S' does not contain a member 'M'
```

Consequentemente, o cliente deve caixar a estrutura para invocar métodos de interface

```csharp
IA s = default(S); // an S, boxed
s.M(); // ok
```

A Boxing dessa forma derrota os principais benefícios de um tipo de `struct`. Além disso, qualquer método de mutação não terá efeito aparente, pois eles estão operando em uma *cópia em caixa* da estrutura:

```csharp
interface IB
{
    public void Increment() { P += 1; }
    public int P { get; set; }
}
struct T : IB
{
    public int P { get; set; } // auto-property
}

T t = default(T);
Console.WriteLine(t.P); // prints 0
(t as IB).Increment();
Console.WriteLine(t.P); // prints 0
```

> ***Problema fechado:*** O que podemos fazer a respeito:
>
> 1. Proíba uma `struct` de herdar uma implementação padrão. Todos os métodos de interface seriam tratados como abstratos em um `struct`. Em seguida, poderemos levar tempo mais tarde para decidir como fazê-lo funcionar melhor.
> 2. Surgirão com algum tipo de estratégia de geração de código que evita boxing. Dentro de um método como `IB.Increment`, o tipo de `this` talvez seria semelhante a um parâmetro de tipo restrito a `IB`. Em conjunto com isso, para evitar boxing no chamador, os métodos não abstratos seriam herdados de interfaces. Isso pode aumentar o trabalho de implementação do compilador e do CLR.
> 3. Não se preocupe e simplesmente deixe-o como um imperfeição.
> 4. Outras ideias?

***Decisão:*** Não se preocupe e simplesmente deixe-o como um imperfeição. Consulte <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#structs-and-default-implementations>.

### <a name="base-interface-invocations-closed"></a>Invocações de interface base (fechadas)

A especificação de rascunho sugere uma sintaxe para invocações de interface base inspiradas por Java: `Interface.base.M()`. Precisamos selecionar uma sintaxe, pelo menos para o protótipo inicial. Meu favorito é `base<Interface>.M()`.

> ***Problema fechado:*** Qual é a sintaxe para uma invocação de membro base?

***Decisão:*** A sintaxe é `base(Interface).M()`. Consulte <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>. A interface, portanto, denominada deve ser uma interface base, mas não precisa ser uma interface base direta.

> ***Abrir problema:*** As invocações de interface base devem ser permitidas em membros de classe?

***Decisão***: Sim. <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>

### <a name="overriding-non-public-interface-members-closed"></a>Substituindo membros de interface não-pública (fechado)

Em uma interface, membros não públicos de interfaces base são substituídos usando o modificador de `override`. Se for uma substituição "explícita" que nomeia a interface que contém o membro, o modificador de acesso será omitido.

> ***Problema fechado:*** Se for uma substituição "implícita" que não nomeie a interface, o modificador de acesso precisará corresponder?

***Decisão:*** Somente membros públicos podem ser substituídos implicitamente e o acesso deve corresponder. Consulte <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.

> ***Abrir problema:*** O modificador de acesso é necessário, opcional ou omitido em uma substituição explícita, como `override void IB.M() {}`?

> ***Abrir problema:*** É `override` necessário, opcional ou omitido em uma substituição explícita, como `void IB.M() {}`?

Como uma implementação de um membro de interface não-pública em uma classe? Talvez ele deva ser feito explicitamente?

```csharp
interface IA
{
    internal void MI();
    protected void MP();
}
class C : IA
{
    // are these implementations?
    internal void MI() {}
    protected void MP() {}
}
```

> ***Problema fechado:*** Como uma implementação de um membro de interface não-pública em uma classe?

***Decisão:*** Você só pode implementar membros de interface não pública explicitamente. Consulte <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.

***Decisão***: nenhuma palavra-chave `override` permitida em membros de interface. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>

### <a name="binary-compatibility-2-closed"></a>Compatibilidade binária 2 (fechado)

Considere o seguinte código no qual cada tipo está em um assembly separado

```csharp
interface I1
{
    void M() { Impl1 }
}
interface I2 : I1
{
    override void M() { Impl2 }
}
interface I3 : I1
{
}
class C : I2, I3
{
}
```

Entendemos que a implementação de `I1.M` no `C` é `I2.M`. E se o assembly que contém `I3` for alterado da seguinte maneira e recompilada

```csharp
interface I3 : I1
{
    override void M() { Impl3 }
}
```

Mas `C` não é recompilado. O que acontece quando o programa é executado? Uma invocação de `(C as I1).M()`

1. Executa `I1.M`
2. Executa `I2.M`
3. Executa `I3.M`
4. 2 ou 3, de forma determinista
5. Gera algum tipo de exceção de tempo de execução

***Decisão***: lançar uma exceção (5). Consulte <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#issues-in-default-interface-methods>.

### <a name="permit-partial-in-interface-closed"></a>Permitir `partial` na interface? Legenda

Considerando que as interfaces podem ser usadas de maneiras análogas à forma como as classes abstratas são usadas, pode ser útil declará-las `partial`. Isso seria particularmente útil na face de geradores.

> ***Proposta:*** Remova a restrição de idioma que as interfaces e os membros das interfaces não podem ser declarados `partial`.

***Decisão***: Sim. Consulte <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>.

### <a name="main-in-an-interface-closed"></a>`Main` em uma interface? Legenda

> ***Abrir problema:*** Um método `static Main` em uma interface que é candidato a ser o ponto de entrada do programa?

***Decisão***: Sim. Consulte <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#main-in-an-interface>.

### <a name="confirm-intent-to-support-public-non-virtual-methods-closed"></a>Confirmar a intenção de dar suporte a métodos não virtuais públicos (fechados)

Podemos confirmar (ou reverter) nossa decisão de permitir métodos públicos não virtuais em uma interface?

```csharp
interface IA
{
    public sealed void M() { }
}
```

> ***Problema semifechado:*** (2017-04-18) achamos que ele será útil, mas voltará a ele. Esse é um bloco mental de blocos de modelo.

***Decisão***: Sim. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#confirm-that-we-support-public-non-virtual-methods>

### <a name="does-an-override-in-an-interface-introduce-a-new-member-closed"></a>Um `override` em uma interface introduz um novo membro? Legenda

Há algumas maneiras de observar se uma declaração de substituição introduz um novo membro ou não.

```csharp
interface IA
{
    void M(int x) { }
}
interface IB : IA
{
    override void M(int y) { }
}
interface IC : IB
{
    static void M2()
    {
        M(y: 3); // permitted?
    }
    override void IB.M(int z) { } // permitted? What does it override?
}
```

> ***Abrir problema:*** Uma declaração de substituição em uma interface introduz um novo membro? Legenda

Em uma classe, um método de substituição é "visível" em alguns sentidos. Por exemplo, os nomes de seus parâmetros têm precedência sobre os nomes dos parâmetros no método substituído. Pode ser possível duplicar esse comportamento em interfaces, pois sempre há uma substituição mais específica. Mas queremos duplicar esse comportamento?

Além disso, é possível "substituir" um método de substituição? Sentido

***Decisão***: nenhuma palavra-chave `override` permitida em membros de interface. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>

### <a name="properties-with-a-private-accessor-closed"></a>Propriedades com um acessador privado (fechado)

Dizemos que os membros privados não são virtuais e a combinação de virtual e privada não é permitida. Mas e quanto a uma propriedade com um acessador particular?

```csharp
interface IA
{
    public virtual int P
    {
        get => 3;
        private set => { }
    }
}
```

Isso é permitido? O acessador de `set` é `virtual` ou não? Ele pode ser substituído onde estiver acessível? O seguinte implementa implicitamente o acessador `get`?

```csharp
class C : IA
{
    public int P
    {
        get => 4;
        set { }
    }
}
```

O seguinte é supostamente um erro porque IA. P. set não é virtual e também porque não está acessível?

```csharp
class C : IA
{
    int IA.P
    {
        get => 4;
        set { }
    }
}
```

***Decisão***: o primeiro exemplo é válido, enquanto o último não. Isso é resolvido de forma análoga em como ele já funciona C#no. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#properties-with-a-private-accessor>

### <a name="base-interface-invocations-round-2-closed"></a>Invocações de interface base, redondo 2 (fechado)

Nossa "resolução" anterior de como lidar com invocações de base não oferece, na verdade, uma expressividade suficiente. Acontece que, no C# e no CLR, ao contrário do Java, você precisa especificar a interface que contém a declaração do método e o local da implementação que você deseja invocar.

Eu propondo a sintaxe a seguir para chamadas base em interfaces. Não estou de amor, mas ilustra o que qualquer sintaxe deve ser capaz de expressar:

```csharp
interface I1 { void M(); }
interface I2 { void M(); }
interface I3 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I4 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I5 : I3, I4
{
    void I1.M()
    {
        base<I3>(I1).M(); // calls I3's implementation of I1.M
        base<I4>(I1).M(); // calls I4's implementation of I1.M
    }
    void I2.M()
    {
        base<I3>(I2).M(); // calls I3's implementation of I2.M
        base<I4>(I2).M(); // calls I4's implementation of I2.M
    }
}
```

Se não houver nenhuma ambiguidade, você poderá escrevê-la mais simplesmente

```csharp
interface I1 { void M(); }
interface I3 : I1 { void I1.M() { } }
interface I4 : I1 { void I1.M() { } }
interface I5 : I3, I4
{
    void I1.M()
    {
        base<I3>.M(); // calls I3's implementation of I1.M
        base<I4>.M(); // calls I4's implementation of I1.M
    }
}
```

Ou

```csharp
interface I1 { void M(); }
interface I2 { void M(); }
interface I3 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I5 : I3
{
    void I1.M()
    {
        base(I1).M(); // calls I3's implementation of I1.M
    }
    void I2.M()
    {
        base(I2).M(); // calls I3's implementation of I2.M
    }
}
```

Ou

```csharp
interface I1 { void M(); }
interface I3 : I1 { void I1.M() { } }
interface I5 : I3
{
    void I1.M()
    {
        base.M(); // calls I3's implementation of I1.M
    }
}
```

***Decisão***: decidida em `base(N.I1<T>).M(s)`, convinculando que, se tivermos uma associação de invocação, pode haver um problema aqui mais tarde. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md#default-interface-implementations>

### <a name="warning-for-struct-not-implementing-default-method-closed"></a>Aviso para struct não implementar o método padrão? Legenda

@vancem afirma que devemos considerar seriamente a criação de um aviso se uma declaração de tipo de valor não substituir algum método de interface, mesmo que herde uma implementação desse método de uma interface. Porque faz com que a boxing e a subminam chamadas restritas.

***Decisão***: isso parece algo mais adequado para um analisador. Também parece que esse aviso pode ser barulhento, pois ele será disparado mesmo se o método de interface padrão nunca for chamado e nenhuma Boxing ocorrerá. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#warning-for-struct-not-implementing-default-method>

### <a name="interface-static-constructors-closed"></a>Construtores estáticos de interface (fechados)

Quando os construtores estáticos da interface são executados?  O rascunho da CLI atual propõe que ele ocorre quando o primeiro método ou campo estático é acessado. Se não houver nenhuma delas, ela pode nunca ser executada?

[2018-10-09 a equipe do CLR propõe "vai espelhar o que fazemos para valuetypes (verificação de cctor no acesso a cada método de instância)"]

***Decisão***: construtores estáticos também são executados na entrada para métodos de instância, se o construtor estático não tiver sido `beforefieldinit`, caso em que construtores estáticos são executados antes do acesso ao primeiro campo estático. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#when-are-interface-static-constructors-run>

## <a name="design-meetings"></a>Criar reuniões

[2017-03-08 notas de reunião do ldm](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-08.md)
[2017-03-21 as notas de reunião do LDM](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md)
[2017-03-23 Meeting "comportamento CLR para métodos de Interface padrão"](https://github.com/dotnet/csharplang/blob/master/meetings/2017/CLR-2017-03-23.md)
[2017-04-05
LDM](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md) notas de reunião
2017-04-11 as notas de reunião do LDM [2017-05-17 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-17.md)
2017-04-18 as notas de [reunião](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md) do LDM
2017-04-19 [2017-04-11 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-11.md) [2017-04-18 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md) [2017-04-19 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md) [LDM Notas de reunião](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-06-14.md)
[2018-10-17 de reuniões LDM](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
[notas de reunião do LDM 2018-11-14](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md)


