---
ms.openlocfilehash: 75fcd5b00ea5cac218a9f7809c53b179df97825c
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488948"
---
# <a name="exceptions"></a>Exceções

Exceções em c# fornecem uma maneira estruturada, uniforme e fortemente tipada de lidar com o nível de sistema e o nível de aplicativo condições de erro. O mecanismo de exceção no c# é muito semelhante do C++, com algumas diferenças importantes:

*  No c#, todas as exceções devem ser representadas por uma instância de um tipo de classe derivado de `System.Exception`. No C++, qualquer valor de qualquer tipo pode ser usado para representar uma exceção.
*  No c#, um bloco finally ([a instrução try](statements.md#the-try-statement)) pode ser usado para gravar o código de finalização que é executada em execução normal e condições excepcionais. Esse código é difícil escrever em C++ sem duplicação de código.
*  No c#, as exceções de nível de sistema, como estouro, divisão por zero e null desreferencia também definir classes de exceção em são juntamente com as condições de erro de nível de aplicativo.

## <a name="causes-of-exceptions"></a>Causas de exceções

Exceção pode ser gerada de duas maneiras diferentes.

*  Um `throw` instrução ([a instrução throw](statements.md#the-throw-statement)) gera uma exceção imediatamente e incondicionalmente. Controle nunca atinge a instrução imediatamente após o `throw`.
*  Determinadas condições excepcionais que ocorrem durante o processamento de instruções em c# e expressão causam uma exceção em determinadas circunstâncias, quando a operação não pode ser concluída normalmente. Por exemplo, uma operação de divisão de inteiro ([operador de divisão](expressions.md#division-operator)) gera um `System.DivideByZeroException` se o denominador for zero. Ver [Classes de exceção comuns](exceptions.md#common-exception-classes) para obter uma lista das várias exceções que podem ocorrer dessa maneira.

## <a name="the-systemexception-class"></a>A classe System. Exception

O `System.Exception` classe é o tipo base de todas as exceções. Essa classe tem algumas propriedades importantes que compartilham todas as exceções:

*  `Message` é uma propriedade somente leitura do tipo `string` que contém uma descrição legível do motivo da exceção.
*  `InnerException` é uma propriedade somente leitura do tipo `Exception`. Se seu valor não for nulo, ele se refere à exceção que causou a exceção atual — ou seja, a exceção atual foi gerada em um bloco catch de tratamento de `InnerException`. Caso contrário, seu valor é nulo, indicando que essa exceção não foi causada por outra exceção. O número de objetos de exceção encadeadas dessa maneira pode ser arbitrário.

O valor dessas propriedades pode ser especificado em chamadas para o construtor de instância para `System.Exception`.

## <a name="how-exceptions-are-handled"></a>Como as exceções são tratadas

As exceções são tratadas por um `try` instrução ([a instrução try](statements.md#the-try-statement)).

Quando ocorre uma exceção, o sistema procura por mais próximo `catch` cláusula que pode manipular a exceção, conforme determinado pelo tipo da exceção de tempo de execução. Primeiro, o método atual é procurado por um delimitador lexicalmente `try` instrução e as cláusulas catch associado da instrução try são considerados na ordem. Se isso falhar, o método que chamou o método atual é procurado por um delimitador lexicalmente `try` instrução que inclui o ponto da chamada para o método atual. Essa pesquisa continua até que um `catch` cláusula for encontrada, que pode lidar com a exceção atual, nomeando uma classe de exceção que é da mesma classe ou uma classe base, do tipo de tempo de execução da exceção sendo lançada. Um `catch` cláusula que não nomeie uma classe de exceção pode lidar com qualquer exceção.

Depois que uma cláusula catch correspondente for encontrada, o sistema se prepara para transferir o controle para a primeira instrução da cláusula catch. Antes de inicia a execução da cláusula catch, o sistema primeiro executará, em ordem, qualquer `finally` cláusulas que estavam associadas a mais de instruções try aninhados que daquela que capturou a exceção.

Se nenhuma cláusula catch correspondente for encontrada, ocorrerá uma das duas coisas:

*  Se a pesquisa por uma cláusula catch correspondente atingir um construtor estático ([construtores estáticos](classes.md#static-constructors)) ou inicializador de campo estático, em seguida, um `System.TypeInitializationException` é lançada no ponto em que disparou a invocação do construtor estático. A exceção interna do `System.TypeInitializationException` contém a exceção que foi originalmente lançada.
*  Se a pesquisa para correspondência de cláusulas catch atingir o código que inicialmente iniciou o thread, a execução do thread é encerrada. O impacto de tal rescisão é definido pela implementação.

As exceções que ocorrem durante a execução do destruidor estão merece atenção especial. Se ocorrer uma exceção durante a execução do destruidor e essa exceção não é capturada, em seguida, a execução do que o destruidor é terminada e o destruidor da classe base (se houver) é chamado. Se não houver nenhuma classe base (como no caso do `object` tipo) ou se não há nenhum destruidor de classe base, a exceção é descartada.

## <a name="common-exception-classes"></a>Classes de exceção comuns

As seguintes exceções são geradas por determinadas operações de c#.

|                                      |                |
|--------------------------------------|----------------|
| `System.ArithmeticException`         | Uma classe base para exceções que ocorrem durante operações aritméticas, tais como `System.DivideByZeroException` e `System.OverflowException`. | 
| `System.ArrayTypeMismatchException`  | Gerada quando um armazenamento em uma matriz falha porque o tipo real do elemento armazenado é incompatível com o tipo real da matriz. | 
| `System.DivideByZeroException`       | Gerada quando ocorre uma tentativa de dividir um valor inteiro por zero. | 
| `System.IndexOutOfRangeException`    | Gerada quando uma tentativa de indexar uma matriz por meio de um índice que é menor que zero ou fora dos limites da matriz. | 
| `System.InvalidCastException`        | Gerada quando uma conversão explícita de um tipo base ou interface para um tipo derivado falha em tempo de execução. | 
| `System.NullReferenceException`      | Lançada quando um `null` referência é usada em uma forma que faz com que o objeto referenciado a ser necessária. | 
| `System.OutOfMemoryException`        | Gerada quando uma tentativa de alocar memória (via `new`) falhará. | 
| `System.OverflowException`           | Lançada quando uma operação aritmética em um contexto `checked` estoura. | 
| `System.StackOverflowException`      | Gerada quando a pilha de execução acaba tendo muitas chamadas de método pendente; normalmente um indicativo da recursão muito profunda ou irrestrita. | 
| `System.TypeInitializationException` | Gerada quando um construtor estático lança uma exceção e não `catch` cláusulas existe para capturá-la. | 
