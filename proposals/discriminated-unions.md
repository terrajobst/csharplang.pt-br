---
ms.openlocfilehash: 2e4a3a32def6900797c151264c984378b09b4988
ms.sourcegitcommit: 5983461e05be62f39c77383cb7857539518cb04f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2019
ms.locfileid: "79485122"
---

# <a name="discriminated-unions--enum-class"></a>Uniões discriminadas/`enum class`

os `enum class`es são um novo tipo de declaração de tipo, às vezes chamados de uniões discriminadas, em que cada instância possível do tipo é listada e cada instância não se sobrepõe.

Um `enum class` é definido usando a seguinte sintaxe:

```antlr
enum_class
    : 'partial'? 'enum class' identifier type_parameter_list? type_parameter_constraints_clause* 
      '{' enum_class_body '}'
    ;

enum_class_body
    : enum_class_cases?
    | enum_class_cases ','
    ;

enum_class_cases
    : enum_class_case
    | enum_class_case ',' enum_class_cases
    ;

enum_class_case
    : enum_class
    | class_declaration
    | identifier type_parameter_list? '(' formal_parameter_list? ')'
    | identifier
    ;

```

Sintaxe de exemplo:

```C#
enum class Shape
{
    Rectangle(float Width, float Length),
    Circle(float Radius),
}
```

## <a name="semantics"></a>Semântica

Uma definição de `enum class` define um tipo de raiz, que é uma classe abstrata de mesmo nome que a declaração de `enum class` e um conjunto de membros, cada um com um tipo que é um subtipo do tipo raiz. Se houver várias definições de `partial enum class`, todos os membros serão considerados membros da definição de classe de enumeração. Ao contrário de uma definição de classe abstrata definida pelo usuário, o tipo de raiz `enum class` é parcial por padrão e definido para ter um construtor padrão sem parâmetros *privados* .

Observe que, como o tipo de raiz é definido para ser uma classe abstrata parcial, as definições parciais do *tipo raiz* também podem ser adicionadas, onde são permitidas formas de sintaxe padrão para um corpo de classe.
No entanto, nenhum tipo pode herdar diretamente do tipo raiz em qualquer declaração, exceto aqueles especificados como membros `enum class`. Além disso, nenhum construtor definido pelo usuário é permitido para o tipo de raiz.

Há quatro tipos de declarações de membro `enum class`:

* Membros de classe simples

* Membros de classe complexa

* Membros do `enum class`

* Membros de valor.

### <a name="simple-class-members"></a>Membros de classe simples

Uma declaração de membro de classe simples define uma nova classe "registro" aninhada (intencionalmente deixada indefinida neste documento) com o mesmo nome. A classe aninhada herda do tipo raiz.

Dado o código de exemplo acima,

```C#
enum class Shape
{
    Rectangle(float Width, float Length),
    Circle(float Radius)
}
```

a declaração de `enum class` tem semântica equivalente à declaração a seguir

```C#
partial abstract class Shape
{
    public data class Rectangle(float Width, float Length) : Shape,
    public data class Circle(float Radius) : Shape
}
```

### <a name="complex-class-members"></a>Membros de classe complexa

Você também pode aninhar uma declaração de classe inteira abaixo de uma declaração de `enum class`. Ele será tratado como uma classe aninhada do tipo raiz. A sintaxe permite qualquer declaração de classe, mas é necessária para que o membro de classe complexa herde da declaração direta contendo `enum class`. 

### <a name="enum-class-members"></a>Membros do `enum class`

`enum classes` pode ser aninhada entre si, por exemplo,

```C#
enum class Expr
{
    enum class Binary
    {
        Addition(Expr left, Expr right),
        Multiplication(Expr left, Expr right)
    }
}
```

Isso é quase idêntico à semântica de um `enum class`de nível superior, exceto pelo fato de que a classe enum aninhada define um tipo raiz aninhado, e tudo abaixo da classe enum aninhada é um subtipo do tipo raiz aninhado, em vez do tipo raiz de nível superior.

```C#
partial abstract class Expr
{
    partial abstract class Binary : Expr
    {
        public data class Addition(Expr left, Expr right) : Binary,
        public data class Multiplication(Expr left, Expr right) : Binary
    }
}
```

### <a name="value-members"></a>Membros de valor

`enum classes` também pode conter membros de valor. Membros de valor definem propriedades estáticas públicas somente obtenção no tipo raiz que também retornam o tipo raiz, por exemplo,

```C#
enum class Color
{
    Red,
    Green
}
```

tem propriedades equivalentes a

```C#
partial abstract class Color
{
    public static Color Red => ...;
    public static Color Green => ...;
}
```

A semântica completa é considerada um detalhe de implementação, mas é garantido que uma instância exclusiva será retornada para cada propriedade e a mesma instância será retornada em invocações repetidas.


### <a name="switch-expression-and-patterns"></a>Alternar expressão e padrões

Há alguns ajustes propostos na correspondência de padrões e na expressão switch para manipular `enum classes`. Expressões de switch já podem corresponder tipos por meio do padrão variável, mas, para os tipos de referência, no momento, nenhum conjunto de braços de switch na expressão switch é considerado concluído, exceto para correspondência com o tipo estático do argumento ou um subtipo.

As expressões switches seriam alteradas de modo que, se o tipo raiz de um `enum class` for o tipo estático do argumento para a expressão switch, e houver um conjunto de padrões correspondentes a todos os membros da enumeração, a opção será considerada exaustiva.

Como os membros de valor não são constantes e não definem novos tipos estáticos, eles atualmente não podem ser correspondidos por padrão. Para tornar isso possível, um novo padrão usando a sintaxe de padrão constante será adicionado para permitir a correspondência em relação aos membros `enum class` valor. A correspondência é definida para ser bem sucedido se e somente se o argumento para o padrão corresponder e o valor retornado pelo membro de `enum class` valor for igual ao mesmo, embora a implementação não seja necessária para executar essa verificação.


## <a name="open-questions"></a>Perguntas abertas

- [] O que o algoritmo de tipo comum diz sobre `enum class` Membros? Este código é válido?
    * `var x = b ? new Shape.Rectangle(...) : new Shape.Circle(...)`

- [] A adição de um novo padrão apenas para membros de valor parece pesada. Existe uma construção de versão mais geral que faz sentido?
    - [] Os membros de valor também não mapeiam bem para uma construção de classe aninhada paralela por causa disso

- [] Está alternando para um argumento com um `enum class` tipo estático garantido como tempo constante?

- [] Deve haver uma maneira de fazer `enum class`es não ser considerada concluída na expressão switch? Prefixar com `virtual`?

- [] Quais modificadores devem ser permitidos em `enum class`?