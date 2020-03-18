---
ms.openlocfilehash: f238a711e710bbac7f5b7400fa938bd85dec00c6
ms.sourcegitcommit: 5278336b61519956240a6f7d83bcb4322019e032
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/10/2020
ms.locfileid: "79485248"
---
# <a name="support-for--and--on-tuple-types"></a>Suporte para = = e! = em tipos de tupla

Permitir expressões `t1 == t2` em que `t1` e `t2` são tipos de tupla ou separadoras anuláveis da mesma cardinalidade e os avaliam aproximadamente como `temp1.Item1 == temp2.Item1 && temp1.Item2 == temp2.Item2` (supondo `var temp1 = t1; var temp2 = t2;`).

Por outro lado, ele permitiria `t1 != t2` e avaliá-lo como `temp1.Item1 != temp2.Item1 || temp1.Item2 != temp2.Item2`.

No caso anulável, são usadas verificações adicionais para `temp1.HasValue` e `temp2.HasValue`. Por exemplo, `nullableT1 == nullableT2` é avaliada como `temp1.HasValue == temp2.HasValue ? (temp1.HasValue ? ... : true) : false`.

Quando uma comparação por elemento retorna um resultado não bool (por exemplo, quando um `operator ==` definido pelo usuário não booliano ou `operator !=` é usado, ou em uma comparação dinâmica), esse resultado será convertido em `bool` ou executado por `operator true` ou `operator false` para obter uma `bool`. A comparação de tupla sempre acaba retornando um `bool`.

A partir C# de 7,2, esse código produz um erro (`error CS0019: Operator '==' cannot be applied to operands of type '(...)' and '(...)'`), a menos que haja um `operator==`definido pelo usuário.

## <a name="details"></a>Detalhes

Ao vincular o operador `==` (ou `!=`), as regras existentes são: (1) caso dinâmico, (2) resolução de sobrecarga e (3) falham.
Essa proposta adiciona um caso de tupla entre (1) e (2): se ambos os operandos de um operador de comparação são tuplas (têm tipos de tupla ou literais de tupla) e têm cardinalidade correspondente, a comparação é executada com um elemento-Wise. Essa igualdade de tupla também é levantada em tuplas anuláveis.

Ambos os operandos (e, no caso de literais de tupla, seus elementos) são avaliados na ordem da esquerda para a direita. Cada par de elementos é usado como operandos para associar o operador `==` (ou `!=`), recursivamente. Qualquer elemento com o tipo de tempo de compilação `dynamic` causa um erro. Os resultados dessas comparações por elemento são usados como operandos em uma cadeia de operadores condicionais e (or ou).

Por exemplo, no contexto de `(int, (int, int)) t1, t2;`, `t1 == (1, (2, 3))` será avaliada como `temp1.Item1 == temp2.Item1 && temp1.Item2.Item1 == temp2.Item2.Item1 && temp2.Item2.Item2 == temp2.Item2.Item2`.

Quando um literal de tupla é usado como operando (em qualquer lado), ele recebe um tipo de tupla convertido formado pelas conversões de elemento-Wise que são introduzidas ao ligar o elemento `==` (ou `!=`) de operador. 

Por exemplo, em `(1L, 2, "hello") == (1, 2L, null)`, o tipo convertido para os literais de tupla é `(long, long, string)` e o segundo literal não tem nenhum tipo natural.


### <a name="deconstruction-and-conversions-to-tuple"></a>Desconstrução e conversões para tupla
Em `(a, b) == x`, o fato de que `x` pode desconstruir em dois elementos não desempenha uma função. Isso poderia estar em uma futura proposta, embora isso gere dúvidas sobre `x == y` (é uma comparação simples ou uma comparação de elemento, e se estiver usando qual cardinalidade?).
Da mesma forma, as conversões para a tupla não desempenham nenhuma função.

### <a name="tuple-element-names"></a>Nomes de elementos de tupla

Ao converter um literal de tupla, avisamos quando um nome de elemento de tupla explícito era fornecido no literal, mas ele não corresponde ao nome do elemento de tupla de destino.
Usamos a mesma regra na comparação de tuplas, de modo que supondo `(int a, int b) t` avisamos sobre `d` no `t == (c, d: 0)`.

### <a name="non-bool-element-wise-comparison-results"></a>Resultados de comparação de elemento não bool

Se uma comparação por elemento for dinâmica em uma igualdade de tupla, usamos uma invocação dinâmica do operador `false` e a desnegarei para obter um `bool` e continuar com outras comparações com elemento. 

Se uma comparação por elemento retornar algum outro tipo não bool em uma igualdade de tupla, haverá dois casos:
- Se o tipo não bool for convertido em `bool`, aplicamos essa conversão,
- Se não houver tal conversão, mas o tipo tiver um operador `false`, usaremos isso e negarei o resultado.

Em uma desigualdade de tupla, as mesmas regras se aplicam, exceto pelo fato de usarmos o operador `true` (sem negação) em vez da `false`do operador.

Essas regras são semelhantes às regras envolvidas para usar um tipo não bool em uma instrução `if` e alguns outros contextos existentes.

## <a name="evaluation-order-and-special-cases"></a>Ordem de avaliação e casos especiais
O valor do lado esquerdo é avaliado primeiro, depois o valor do lado direito e, em seguida, as comparações por elemento da esquerda para a direita (incluindo conversões e com a saída inicial com base em regras existentes para operadores condicionais e/ou).

Por exemplo, se houver uma conversão do tipo `A` para o tipo `B` e um método `(A, A) GetTuple()`, avaliar `(new A(1), (new B(2), new B(3))) == (new B(4), GetTuple())` significa:
- `new A(1)`
- `new B(2)`
- `new B(3)`
- `new B(4)`
- `GetTuple()`
- em seguida, as conversões e comparações e a lógica condicional do elemento são avaliadas (converta `new A(1)` no tipo `B`, compare-o com `new B(4)`e assim por diante).

### <a name="comparing-null-to-null"></a>Comparando `null` a `null`

Esse é um caso especial de comparações regulares, que são transferidas para comparações de tupla. A comparação de `null == null` é permitida e os literais de `null` não obtêm nenhum tipo.
Na igualdade de tupla, isso significa que `(0, null) == (0, null)` também é permitido e os literais `null` e tupla não obtêm um tipo.

### <a name="comparing-a-nullable-struct-to-null-without-operator"></a>Comparando uma struct anulável com `null` sem `operator==`

Esse é outro caso especial de comparações regulares, que são transferidas para comparações de tupla.
Se você tiver um `struct S` sem `operator==`, a comparação de `(S?)x == null` será permitida e será interpretada como `((S?).x).HasValue`.
Na igualdade de tupla, a mesma regra é aplicada, portanto `(0, (S?)x) == (0, null)` é permitido.

## <a name="compatibility"></a>Compatibilidade

Se alguém escreveu seus próprios tipos de `ValueTuple` com uma implementação do operador de comparação, ele teria sido previamente selecionado pela resolução de sobrecarga. Mas como o novo caso de tupla vem antes da resolução de sobrecarga, tratamos desse caso com a comparação de tupla em vez de depender da comparação definida pelo usuário.

----

Relaciona-se aos [operadores de teste relacional e de tipo](../../spec/expressions.md#relational-and-type-testing-operators) relacionados a [#190](https://github.com/dotnet/csharplang/issues/190)
