---
ms.openlocfilehash: a8822137c85f449444ed675c6f2912315c041691
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484471"
---
# <a name="static-delegates"></a>Delegados estáticos

* [x] proposta
* [] Protótipo: não iniciado
* [] Implementação: não iniciada
* [] Especificação: não iniciada

## <a name="summary"></a>Resumo
[summary]: #summary

Forneça um recurso de retorno de chamada leve de uso geral C# para a linguagem.

## <a name="motivation"></a>Motivação
[motivation]: #motivation

Hoje, os usuários têm a capacidade de criar retornos de chamada usando o tipo de `System.Delegate`. No entanto, elas são bastante pesadas (como exigir uma alocação de heap e sempre ter tratamento para encadear retornos de chamada juntos).

Além disso, `System.Delegate` não fornece a melhor interoperabilidade com ponteiros de função não gerenciados, ou seja, por ser não-blittable e exigir Marshalling a qualquer momento, ele cruza o limite gerenciado/não gerenciado.

Com alguns pequenos ajustes, poderíamos fornecer um novo tipo de delegado que seja leve, de uso geral e de interoperabilidade bem com código nativo.

## <a name="detailed-design"></a>Design detalhado
[design]: #detailed-design

Um declararia um delegado estático por meio do seguinte:

```C#
static delegate int Func()
```

Além disso, é possível atribuir a declaração com algo semelhante a `System.Runtime.InteropServices.UnmanagedFunctionPointer` para que a Convenção de chamada, o Marshalling de cadeia de caracteres e o comportamento de definir o último erro sejam controlados. Observação: usar `System.Runtime.InteropServices.UnmanagedFunctionPointer` em si não funcionará, pois ele só pode ser usado em delegados.

A declaração seria convertida em uma representação interna pelo compilador que é semelhante ao seguinte

```C#
struct <Func>e__StaticDelegate
{
    IntPtr pFunction;

    static int WellKnownCompilerName();
}
```

Isso significa que ele é representado internamente por uma struct que tem um único membro do tipo `IntPtr` (tal struct é blittable e não incorre em nenhuma alocação de heap):
* O membro contém o endereço da função que deve ser o retorno de chamada.
* O tipo declara um método que corresponde à assinatura do método do retorno de chamada.
* O nome da estrutura não deve ser User-constructible (como fazemos com outras estruturas de backup geradas internamente).
 * Por exemplo: buffers de tamanho fixo geram uma struct com um nome no formato de `<name>e__FixedBuffer` (`<` e `>` fazem parte do identificador e tornam o identificador não constructibledo C#, mas ainda utilizáveis em Il).
* O nome da declaração do método deve ser um nome bem conhecido usado em todos os tipos delegados estáticos (isso permite que o compilador saiba o nome a ser pesquisado ao determinar a assinatura).

O valor do delegado estático só pode ser associado a um método estático que corresponda à assinatura do retorno de chamada.

Não há suporte para o encadeamento de retornos de chamada juntos.

A invocação do retorno de chamada seria implementada pela instrução `calli`.

## <a name="drawbacks"></a>Desvantagens
[drawbacks]: #drawbacks

Delegados estáticos não funcionariam com APIs existentes que usam delegados regulares (seria necessário encapsular um delegado estático em um delegado regular da mesma assinatura).
* Considerando que `System.Delegate` é representado internamente como um conjunto de campos de `object` e `IntPtr` (http://source.dot.net/#System.Private.CoreLib/src/System/Delegate.cs), seria possível permitir a conversão implícita de um delegado estático em um `System.Delegate` que tenha uma assinatura de método correspondente. Também seria possível fornecer uma conversão explícita na direção oposta, desde que o `System.Delegate` esteja em conformidade com todos os requisitos de ser um delegado estático.

Um trabalho adicional seria necessário para tornar o delegado estático prontamente utilizável na estrutura principal.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

TBD

## <a name="unresolved-questions"></a>Perguntas não resolvidas
[unresolved]: #unresolved-questions

Quais partes do design ainda são TBD?

## <a name="design-meetings"></a>Criar reuniões

Vincule a anotações de design que afetam essa proposta e descreva em uma frase para cada alteração que elas levaram.


