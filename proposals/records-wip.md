---
ms.openlocfilehash: 258ae6865c5b2c3103a0cdf7e1e5a2cdee11e740
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281951"
---
# <a name="records-work-in-progress"></a>Registros de trabalho em andamento

Ao contrário das outras propostas de registros, essa não é uma proposta em si, mas um trabalho em andamento é projetado para registrar decisões de design de consenso para o recurso de registros. Os detalhes de especificação serão adicionados conforme necessário para resolver perguntas.

A sintaxe de um registro é proposta para ser adicionada da seguinte maneira:

```antlr
class_declaration
    : attributes? class_modifiers? 'partial'? 'class' identifier type_parameter_list?
      parameter_list? type_parameter_constraints_clauses? class_body
    ;

struct_declaration
    : attributes? struct_modifiers? 'partial'? 'struct' identifier type_parameter_list?
      parameter_list? struct_interfaces? type_parameter_constraints_clauses? struct_body
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

O `attributes` não terminal também permitirá um novo atributo contextual, `data`.

Uma classe (struct) declarada com uma lista de parâmetros ou `data` modificador é chamada de classe de registro (struct de registro), ou seja, um tipo de registro.

É um erro declarar um tipo de registro sem uma lista de parâmetros e o modificador de `data`.

## <a name="members-of-a-record-type"></a>Membros de um tipo de registro

Além dos membros declarados no corpo da classe, um tipo de registro tem os seguintes membros adicionais:

### <a name="primary-constructor"></a>Construtor principal

Um tipo de registro tem um construtor público cuja assinatura corresponde aos parâmetros de valor da declaração de tipo. Isso é chamado de Construtor principal para o tipo e faz com que o construtor padrão declarado implicitamente seja suprimido. É um erro ter um construtor primário e um construtor com a mesma assinatura já presentes na classe.
Em tempo de execução, o construtor primário 

1. executa os inicializadores de campo de instância que aparecem no corpo da classe; e, em seguida, invoca o construtor da classe base sem argumentos.

1. Inicializa os campos de backup gerados pelo compilador para as propriedades correspondentes aos parâmetros de valor (se essas propriedades forem fornecidas pelo compilador; consulte [Propriedades sintetizadas](#Synthesized Properties))


[] TODO: Adicionar sintaxe de chamada base e especificação sobre como escolher o construtor base por meio da resolução de sobrecarga

### <a name="properties"></a>Propriedades

Para cada parâmetro de registro de uma declaração de tipo de registro, há um membro de propriedade pública correspondente cujo nome e tipo são tirados da declaração de parâmetro de valor. Se nenhuma propriedade concreta (ou seja, não abstrata) com um acessador get e com esse nome e tipo for explicitamente declarada ou herdada, ela será produzida pelo compilador da seguinte maneira:

Para uma estrutura de registro ou uma classe de registro:

* Uma propriedade automática somente obtenção pública é criada. Seu valor é inicializado durante a construção com o valor do parâmetro de construtor primário correspondente. Cada acessador get herdado de propriedade abstrata "correspondente" é substituído.

### <a name="equality-members"></a>Membros de igualdade

Os tipos de registro produzem implementações sintetizadas para os seguintes métodos:

* `object.GetHashCode()` substituir, a menos que seja lacrado ou fornecido pelo usuário
* `object.Equals(object)` substituir, a menos que seja lacrado ou fornecido pelo usuário
* `T Equals(T)` método, em que `T` é o tipo atual

`T Equals(T)` é especificado para executar a igualdade de valor, comparando a propriedade com o mesmo nome de cada parâmetro de construtor primário para a propriedade correspondente do outro tipo.
`object.Equals` executa o equivalente de

```C#
override Equals(object o) => Equals(o as T);
```

## <a name="with-expression"></a>Expressão `with`

Uma expressão `with` é uma nova expressão usando a sintaxe a seguir.

```antlr
with_expression
    : switch_expression
    | switch_expression 'with' anonymous_object_initializer
```

Uma expressão `with` permite a "mutação não destrutiva", projetada para produzir uma cópia da expressão do destinatário com modificações nas propriedades listadas na `anonymous_object_initializer`.

Uma expressão de `with` válida tem um receptor com um tipo não void. O tipo de receptor deve conter um método de instância acessível chamado `With` com os parâmetros e o tipo de retorno apropriados. Ocorrerá um erro se houver vários métodos de `With` não substituição. Se houver várias substituições `With`, deverá haver um método `With` não substituir, que é o método de destino. Caso contrário, deve haver exatamente um método `With`.

No lado direito da `with` expressão é um `anonymous_object_initializer` com uma sequência de atribuições com um campo ou Propriedade do destinatário no lado esquerdo da atribuição, e uma expressão arbitrária no lado direito, que é implicitamente conversível para o tipo do lado esquerdo da ordem de entrega.

Dado um método de `With` de destino, o tipo de retorno deve ser o tipo do tipo de expressão do receptor ou um tipo de base dele. Para cada parâmetro do método `With`, deve haver um campo de instância correspondente acessível ou uma propriedade legível no tipo de receptor com o mesmo nome e o mesmo tipo. Cada propriedade ou campo no lado direito da expressão with também deve corresponder a um parâmetro de mesmo nome no método `With`.

Dado um método de `With` válido, a avaliação de uma expressão `with` é equivalente a chamar o método `With` com as expressões no `anonymous_object_initializer` substituído para o parâmetro do mesmo nome que a propriedade no lado esquerdo. Se não houver nenhuma propriedade correspondente para um determinado parâmetro na `anonymous_object_initializer`, o argumento será a avaliação do campo ou da Propriedade do mesmo nome no receptor.

A ordem de avaliação de efeitos colaterais é a seguinte, com cada expressão avaliada exatamente uma vez:

1. Expressão do destinatário

2. Expressões no `anonymous_object_initializer`, em ordem lexical

3. A avaliação de todas as propriedades que correspondem aos parâmetros do método `With`, em ordem de definição dos parâmetros do método `With`.