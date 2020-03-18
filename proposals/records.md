---
ms.openlocfilehash: 49720d62c496ff904eacacb31ae29ef97a5aefaa
ms.sourcegitcommit: 8152182f0a477cb3082e625b607262cc459a17f3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/23/2019
ms.locfileid: "79484940"
---
# <a name="records"></a>registros

* [x] proposta
* [] Protótipo: [concluído](https://github.com/PROTOTYPE_OWNER/roslyn/BRANCH_NAME)
* [] Implementação: [em andamento](https://github.com/dotnet/roslyn/BRANCH_NAME)
* [] Especificação: especificação de rascunho incluída

## <a name="summary"></a>Resumo
[summary]: #summary

Os registros são um formulário de declaração novo e C# simplificado para tipos de classe e struct que combinam os benefícios de uma série de recursos mais simples. Descrevemos os novos recursos (parâmetros do receptor do chamador e *com-Expression*s), fornecemos a sintaxe e a semântica para declarações de registro e, em seguida, fornecem alguns exemplos.


## <a name="motivation"></a>Motivação
[motivation]: #motivation

Um número significativo de declarações de tipo C# no é muito mais do que as coleções agregadas de dados digitados. Infelizmente, a declaração desses tipos requer uma grande quantidade de código clichê. Os *registros* fornecem um mecanismo para declarar um tipo de dados, descrevendo os membros da agregação junto com código ou desvios adicionais do texto clichê comum, se houver.

Consulte os [exemplos](#examples)abaixo.

## <a name="detailed-design"></a>Design detalhado
[design]: #detailed-design

### <a name="caller-receiver-parameters"></a>chamador-parâmetros do receptor

No momento *, o argumento padrão* de um parâmetro de método deve ser 
- uma *expressão de constante*; or
- uma expressão do formulário `new S()` onde `S` é um tipo de valor; or
- uma expressão do formulário `default(S)` onde `S` é um tipo de valor

Estendemos isso para adicionar o seguinte
- uma expressão do formulário `this.Identifier`

Esse novo formulário é chamado de *argumento-padrão do receptor do chamador*e só é permitido se todos os itens a seguir forem atendidos
- O método no qual ele aparece é um método de instância; e
- A expressão `this.Identifier` é associada a um membro de instância do tipo delimitador, que deve ser um campo ou uma propriedade; e
- O membro ao qual ele é associado (e o acessador de `get`, se for uma propriedade) é pelo menos tão acessível quanto o método; e
- O tipo de `this.Identifier` é implicitamente conversível por uma identidade ou conversão anulável para o tipo do parâmetro (essa é uma restrição existente no *argumento padrão*).

Quando um argumento é omitido de uma invocação de um membro de função para um parâmetro opcional correspondente com um *argumento-padrão de receptor de chamador*, o valor do membro do destinatário é passado implicitamente. 

> **Observações de design**: o principal motivo para o parâmetro do receptor do chamador é dar suporte a *com a expressão with*. A ideia é que você pode declarar um método como este
> ```cs
> class Point
> {
>     public readonly int X;
>     public readonly int Y;
>     public Point With(int x = this.X, int y = this.Y) => new Point(x, y);
>     // etc
> }
> ```
> e, em seguida, usá-lo como este
> ```cs
>     Point p = new Point(3, 4);
>     p = p.With(x: 1);
> ```
> Para criar um novo `Point` assim como um `Point` existente, mas com o valor de `X` alterado.
> 
> É uma pergunta aberta se a forma sintática da *expressão with* deve ser adicionada assim que tivermos suporte para parâmetros do receptor do chamador, portanto, é possível que possamos fazer isso em *vez de* *em* vez de *com a expressão with*.

- [] **Abrir problema**: Qual é a ordem na qual um *argumento-padrão do receptor do chamador* é avaliado com relação a outros argumentos? Devemos dizer que ele não está especificado?

### <a name="with-expressions"></a>com expressões

Um novo formulário de expressão é proposto:

```antlr
primary_expression
    : with_expression
    ;

with_expression
    : primary_expression 'with' '{' with_initializer_list '}'
    ;

with_initializer_list
    : with_initializer
    | with_initializer ',' with_initializer_list
    ;

with_initializer
    : identifier '=' expression
    ;
```

O token `with` é uma nova palavra-chave sensível ao contexto.

Cada *identificador* à esquerda de um *with_initializer* deve ser associado a um campo de instância acessível ou à propriedade do tipo do *primary_expression* do *with_expression*. Pode não haver nenhum nome duplicado entre esses identificadores de um determinado *with_expression*.

Uma *with_expression* do formulário

> *identificador* de `{` de `with` *E1* = *e2*,... `}`

é tratado como uma invocação do formulário

> *e1*`.With(`*identifier2*`:` *e2*,...`)`

Em que, para cada método chamado `With` que é um membro de instância acessível de *E1*, selecionamos *identifier2* como o nome do primeiro parâmetro nesse método que tem um parâmetro receptor de chamador que é o mesmo membro que o campo de instância ou a propriedade associada ao *identificador*. Se esse parâmetro não puder ser identificado, o método será eliminado da consideração. O método a ser invocado é selecionado entre os candidatos restantes pela resolução de sobrecarga.

> **Observações de design**: os parâmetros do receptor de chamador fornecidos, muitos dos benefícios da *expressão with* estão disponíveis sem esse formulário de sintaxe especial. Portanto, estamos considerando se é necessário ou não. Seu principal benefício é permitir que um programe em termos de nomes de campos e propriedades, e não em termos de nomes de parâmetros. Dessa forma, melhoramos a legibilidade e a qualidade das ferramentas (por exemplo, ir para a definição no identificador de um *with_expression* navegar para a propriedade em vez de um parâmetro de método).

- [] **Abrir problema**: esta descrição deve ser modificada para dar suporte a métodos de extensão.

### <a name="pattern-matching"></a>correspondência padrão

Consulte a [especificação de correspondência de padrões](csharp-8.0/patterns.md#positional-pattern) para uma especificação de `Deconstruct` e sua relação com a correspondência de padrões.

> **Observações de design**: em virtude do `Deconstruct` gerado pelo compilador conforme especificado aqui, e a especificação para correspondência de padrões, uma declaração de registro
> ```cs
> public class Point(int X, int Y);
> ```
> oferecerá suporte à correspondência de padrão posicional da seguinte maneira
> ```cs
> Point p = new Point(3, 4);
> if (p is Point(3, var y)) { // if X is 3
>     Console.WriteLine(y);   // print Y
> }
> ```

### <a name="record-type-declarations"></a>declarações de tipo de registro

A sintaxe para uma declaração de `class` ou `struct` é estendida para dar suporte a parâmetros de valor; os parâmetros tornam-se propriedades do tipo:

```antlr
class_declaration
    : attributes? class_modifiers? 'partial'? 'class' identifier type_parameter_list?
      record_parameters? record_class_base? type_parameter_constraints_clauses? class_body
    ;

struct_declaration
    : attributes? struct_modifiers? 'partial'? 'struct' identifier type_parameter_list?
      record_parameters? struct_interfaces? type_parameter_constraints_clauses? struct_body
    ;

record_class_base
    : class_type record_base_arguments?
    | interface_type_list
    | class_type record_base_arguments? ',' interface_type_list
    ;

record_base_arguments
    : '(' argument_list? ')'
    ;

record_parameters
    : '(' record_parameter_list? ')'
    ;

record_parameter_list
    : record_parameter
    | record_parameter ',' record_parameter_list
    ;

record_parameter
    : attributes? type identifier record_property_name? default_argument?
    ;

record_property_name
    : ':' identifier
    ;

class_body
    : '{' class_member_declarations? '}'
    | ';'
    ;

struct_body
    : '{' struct_members_declarations? '}'
    | ';'
    ;
```

> **Observações de design**: como os tipos de registro geralmente são úteis sem a necessidade de quaisquer membros explicitamente declarados em um corpo de classe, modificamos a sintaxe da declaração para permitir que um corpo seja simplesmente um ponto e vírgula.

Uma classe (struct) declarada com os *parâmetros de registro* é chamada de *classe de registro* (struct de*registro*), ou seja, um *tipo de registro*.

- [] **Abrir problema**: precisamos incluir *primary_constructor_body* na gramática para que ela possa aparecer dentro de uma declaração de tipo de registro.
- [] **Abrir problema**: quais são as regras de conflito de nome para os nomes de parâmetro? Supostamente, um não tem permissão para entrar em conflito com um parâmetro de tipo ou outro *parâmetro de registro*.
- [] **Abrir problema**: precisamos especificar o escopo dos parâmetros de registro. Onde eles podem ser usados? Presumivelmente, em inicializadores de campo de instância e *primary_constructor_body* pelo menos.
- [] **Abrir problema**: uma declaração de tipo de registro pode ser parcial? Nesse caso, os parâmetros devem ser repetidos em cada parte?

#### <a name="members-of-a-record-type"></a>Membros de um tipo de registro

Além dos membros declarados no *corpo da classe*, um tipo de registro tem os seguintes membros adicionais:

##### <a name="primary-constructor"></a>Construtor principal

Um tipo de registro tem um Construtor `public` cuja assinatura corresponde aos parâmetros de valor da declaração de tipo. Isso é chamado de *Construtor principal* para o tipo e faz com que o *construtor padrão* declarado implicitamente seja suprimido.

Em tempo de execução, o construtor primário

* Inicializa os campos de backup gerados pelo compilador para as propriedades correspondentes aos parâmetros de valor (se essas propriedades forem fornecidas pelo compilador; [consulte 1.1.2](#1.1.2)); Clique
* executa os inicializadores de campo de instância que aparecem no *corpo da classe*; e, em seguida,
* invoca um construtor de classe base:
    * Se houver argumentos na *record_base_arguments*, um construtor base selecionado pela resolução de sobrecarga com esses argumentos será invocado;
    * Caso contrário, um construtor base será invocado sem argumentos.
* executa o corpo de cada *primary_constructor_body*, se houver, na ordem de origem.

- [] **Abrir problema**: precisamos especificar essa ordem, particularmente em unidades de compilação para parciais.
- [] **Abrir problema**: precisamos especificar que cada Construtor declarado explicitamente deve encadear o construtor primário.
- [] **Abrir problema**: deve ter permissão para alterar o modificador de acesso no construtor primário?
- [] **Abrir problema**: em um struct de registro, é um erro para não haver nenhum parâmetro de registro?

##### <a name="primary-constructor-body"></a>Corpo do construtor primário

```antlr
primary_constructor_body
    : attributes? constructor_modifiers? identifier block
    ;
```

Uma *primary_constructor_body* só pode ser usada dentro de uma declaração de tipo de registro. O *identificador* de um *primary_constructor_body* deve nomear o tipo de registro no qual ele é declarado.

O *primary_constructor_body* não declara um membro por conta própria, mas é uma maneira para o programador fornecer atributos para e especificar o acesso de, um construtor principal de tipo de registro. Ele também permite que o programador forneça código adicional que será executado quando uma instância do tipo de registro for construída.

- [] **Abrir problema**: devemos observar que um construtor padrão de struct ignora isso.
- [] **Abrir problema**: devemos especificar a ordem de execução da inicialização.
- [] **Abrir problema**: devemos permitir algo como um *primary_constructor_body* (presumivelmente sem atributos e modificadores) em uma declaração de tipo sem registro e tratá-lo como seria o código de um inicializador de campo de instância?

##### <a name="properties"></a>{1&gt;Propriedades&lt;1}

Para cada parâmetro de registro de uma declaração de tipo de registro, há um membro de propriedade de `public` correspondente cujo nome e tipo são tirados da declaração de parâmetro de valor. Seu nome é o *identificador* do *record_property_name*, se presente, ou o *identificador* do *record_parameter* caso contrário. Se nenhuma propriedade pública concreta (ou seja, não abstrata) com um acessador `get` e com esse nome e tipo for explicitamente declarada ou herdada, ela será produzida pelo compilador da seguinte maneira:

* Para uma *estrutura de registro* ou uma *classe de registro*de `sealed`:
 * Um campo de `readonly` `private` é produzido como um campo de apoio para uma propriedade `readonly`. Seu valor é inicializado durante a construção com o valor do parâmetro de construtor primário correspondente.
 * O acessador de `get` da propriedade é implementado para retornar o valor do campo de backup.
 * Cada acessador de `get` de propriedade virtual herdada "correspondente" é substituído.

> **Observações de design**: em outras palavras, se você estender uma classe base ou implementar uma interface que declare uma propriedade abstrata pública com o mesmo nome e tipo como um parâmetro de registro, essa propriedade será substituída ou implementada.

- [] **Abrir problema**: deve ser possível alterar o modificador de acesso em uma propriedade quando declarado explicitamente?
- [] **Abrir problema**: deve ser possível substituir um campo por uma propriedade?

##### <a name="object-methods"></a>Métodos de objeto

Para uma *estrutura de registro* ou uma `sealed` *classe de registro*, as implementações dos métodos `object.GetHashCode()` e `object.Equals(object)` são produzidas pelo compilador, a menos que sejam fornecidas pelo usuário.

- [] **Abrir problema**: devemos especificar precisamente sua implementação.
- [] **Abrir problema**: também devemos adicionar a interface `IEquatable<T>` para o tipo de registro e especificar que as implementações são fornecidas.
- [] **Abrir problema**: também devemos especificar que implementamos cada `IEquatable<T>.Equals`.
- [] **Abrir problema**: devemos especificar precisamente como resolvemos o problema de igual à herança de registro: especificamente, como geramos métodos de igualdade, de forma que eles sejam simétricos, transitivos, reflexivo, etc.
- [] **Abrir problema**: foi proposto que implementamos `operator ==` e `operator !=` para tipos de registro.
- [] **Abrir problema**: devemos gerar automaticamente uma implementação de `object.ToString`?

##### `Deconstruct`

Um tipo de registro tem um método de `public` gerado pelo compilador `void Deconstruct`, a menos que um com qualquer assinatura seja fornecido pelo usuário. Cada parâmetro é um parâmetro `out` de mesmo nome e tipo que o parâmetro correspondente do tipo de registro. A implementação fornecida pelo compilador desse método deve atribuir cada parâmetro de `out` com o valor da propriedade correspondente.

Consulte [a especificação de correspondência de padrões](csharp-8.0/patterns.md#positional-pattern) para a semântica de `Deconstruct`.

##### <a name="with-method"></a>Método `With`

A menos que haja um membro declarado pelo usuário chamado `With` declarado, um tipo de registro tem um método fornecido pelo compilador chamado `With` cujo tipo de retorno é o próprio tipo de registro e que contém um parâmetro de valor correspondente a cada *parâmetro de registro* na mesma ordem em que esses parâmetros aparecem na declaração de tipo de registro. Cada parâmetro deve ter um *argumento de emissor-padrão do receptor* da propriedade correspondente.

Em uma classe de registro `abstract`, o método `With` fornecido pelo compilador é abstrato. Em uma estrutura de registro, ou uma classe de registro lacrado, o método de `With` fornecido pelo compilador é `sealed`. Caso contrário, o método de `With` fornecido pelo compilador será ' virtual e sua implementação deverá retornar uma nova instância produzida invocando o Construtor principal com os parâmetros como argumentos para criar uma nova instância a partir dos parâmetros e retornar essa nova instância.

- [] **Abrir problema**: também devemos especificar em quais condições substituimos ou implementamos métodos de `With` virtual herdados ou métodos de `With` de interfaces implementadas.
- [] **Abrir problema**: devemos dizer o que acontece quando herdamos um método de `With` não virtual.

> **Observações de design**: como os tipos de registro são, por padrão, imutáveis, o método `With` fornece uma maneira de criar uma nova instância que seja igual a uma instância existente, mas com as propriedades selecionadas, dadas novos valores. Por exemplo, dada
> ```cs
> public class Point(int X, int Y);
> ```
> Há um membro fornecido pelo compilador
> ```cs
>     public virtual Point With(int X = this.X, int Y = this.Y) => new Point(X, Y);
> ```
> Que habilita uma variável do tipo de registro
> ```cs
> var p = new Point(3, 4);
> ```
> para ser substituído por uma instância que tenha uma ou mais propriedades diferentes
> ```cs
>     p = p.With(X: 5);
> ```
> Isso também pode ser expresso usando o *with_expression*:
> ```cs
>     p = p with { X = 5 };
> ```

### <a name="examples"></a>Exemplos

#### <a name="compatibility-of-record-types"></a>Compatibilidade dos tipos de registro

Como o programador pode adicionar membros a uma declaração de tipo de registro, muitas vezes é possível alterar o conjunto de elementos de registro sem afetar os clientes existentes. Por exemplo, dada uma versão inicial de um tipo de registro

```cs
// v1
public class Person(string Name, DateTime DateOfBirth);
```

Um novo elemento do tipo de registro pode ser dos adicionado na próxima revisão do tipo sem afetar a compatibilidade binária ou de origem:

```cs
// v2
public class Person(string Name, DateTime DateOfBirth, string HomeTown)
{
    // Note: below operations added to retain binary compatibility with v1
    public Person(string Name, DateTime DateOfBirth) : this(Name, DateOfBirth, string.Empty) {}
    public static void operator is(Person self, out string Name, out DateTime DateOfBirth)
        { Name = self.Name; DateOfBirth = self.DateOfBirth; }
    public Person With(string Name, DateTime DateOfBirth) => new Person(Name, DateOfBirth);
}
```

#### <a name="record-struct-example"></a>exemplo de struct do registro

Este struct de registro

```cs
public struct Pair(object First, object Second);
```

é traduzido para este código

```cs
public struct Pair : IEquatable<Pair>
{
    public object First { get; }
    public object Second { get; }
    public Pair(object First, object Second)
    {
        this.First = First;
        this.Second = Second;
    }
    public bool Equals(Pair other) // for IEquatable<Pair>
    {
        return Equals(First, other.First) && Equals(Second, other.Second);
    }
    public override bool Equals(object other)
    {
        return (other as Pair)?.Equals(this) == true;
    }
    public override int GetHashCode()
    {
        return (First?.GetHashCode()*17 + Second?.GetHashCode()).GetValueOrDefault();
    }
    public Pair With(object First = this.First, object Second = this.Second) => new Pair(First, Second);
    public void Deconstruct(out object First, out object Second)
    {
        First = self.First;
        Second = self.Second;
    }
}
```

- [] **Abrir problema**: a implementação de Equals (par outro) é um membro público do par?
- [] **Abrir problema**: essa implementação de `Equals` não é simétrica em face de herança.
- [] **Abrir problema**: um registro deve declarar `operator ==` e `operator !=`?

> **Observações de design**: como um tipo de registro pode herdar de outro, e essa implementação de `Equals` não seria simétrica nesse caso, ela não está correta. Sugerimos implementar igualdade dessa maneira:
> ```cs
>     public bool Equals(Pair other) // for IEquatable<Pair>
>     {
>         return other != null && EqualityContract == other.EqualityContract &&
>             Equals(First, other.First) && Equals(Second, other.Second);
>     }
>     protected virtual Type EqualityContract => typeof(Pair);
> ```
> Os registros derivados `override EqualityContract`. A alternativa menos atraente é restringir a herança.

#### <a name="sealed-record-example"></a>exemplo de registro lacrado

Esta classe de registro lacrado

```cs
public sealed class Student(string Name, decimal Gpa);
```

é traduzido para este código

```cs
public sealed class Student : IEquatable<Student>
{
    public string Name { get; }
    public decimal Gpa { get; }
    public Student(string Name, decimal Gpa)
    {
        this.Name = Name;
        this.Gpa = Gpa;
    }
    public bool Equals(Student other) // for IEquatable<Student>
    {
        return other != null && Equals(Name, other.Name) && Equals(Gpa, other.Gpa);
    }
    public override bool Equals(object other)
    {
        return this.Equals(other as Student);
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()*17 + Gpa?.GetHashCode()).GetValueOrDefault();
    }
    public Student With(string Name = this.Name, decimal Gpa = this.Gpa) => new Student(Name, Gpa);
    public void Deconstruct(out string Name, out decimal Gpa)
    {
        Name = self.Name;
        Gpa = self.Gpa;
    }
}
```

#### <a name="abstract-record-class-example"></a>exemplo de classe de registro abstract

Essa classe de registro abstrato

```cs
public abstract class Person(string Name);
```

é traduzido para este código

```cs
public abstract class Person : IEquatable<Person>
{
    public string Name { get; }
    public Person(string Name)
    {
        this.Name = Name;
    }
    public bool Equals(Person other)
    {
        return other != null && Equals(Name, other.Name);
    }
    public override bool Equals(object other)
    {
        return Equals(other as Person);
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()).GetValueOrDefault();
    }
    public abstract Person With(string Name = this.Name);
    public void Deconstruct(Person self, out string Name)
    {
        Name = self.Name;
    }
}
```

#### <a name="combining-abstract-and-sealed-records"></a>combinando registros abstratos e lacrados

Dada a classe de registro abstrato `Person` acima, essa classe de registro lacrado

```cs
public sealed class Student(string Name, decimal Gpa) : Person(Name);
```

é traduzido para este código

```cs
public sealed class Student : Person, IEquatable<Student>
{
    public override string Name { get; }
    public decimal Gpa { get; }
    public Student(string Name, decimal Gpa) : base(Name)
    {
        this.Name = Name;
        this.Gpa = Gpa;
    }
    public override bool Equals(Student other) // for IEquatable<Student>
    {
        return Equals(Name, other.Name) && Equals(Gpa, other.Gpa);
    }
    public bool Equals(Person other) // for IEquatable<Person>
    {
        return (other as Student)?.Equals(this) == true;
    }
    public override bool Equals(object other)
    {
        return (other as Student)?.Equals(this) == true;
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()*17 + Gpa.GetHashCode()).GetValueOrDefault();
    }
    public Student With(string Name = this.Name, decimal Gpa = this.Gpa) => new Student(Name, Gpa);
    public override Person With(string Name = this.Name) => new Student(Name, Gpa);
    public void Deconstruct(Student self, out string Name, out decimal Gpa)
    {
        Name = self.Name;
        Gpa = self.Gpa;
    }
}
```

## <a name="drawbacks"></a>Desvantagens
[drawbacks]: #drawbacks

Como com qualquer recurso de linguagem, devemos questionar se a complexidade adicional para o idioma é repagada na maior clareza oferecida ao corpo de C# programas que se beneficiariam do recurso.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

Consideramos a adição de *construtores primários* em C# 6. Embora eles ocupem a mesma superfície sintática que essa proposta, descobrimos que eles se resumiram das vantagens oferecidas pelos registros.

## <a name="unresolved-questions"></a>Perguntas não resolvidas
[unresolved]: #unresolved-questions

As perguntas abertas são exibidas no corpo da proposta.

## <a name="design-meetings"></a>Criar reuniões

TBD
