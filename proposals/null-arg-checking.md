---
ms.openlocfilehash: 76065293f652979ab395e131d657e44899c5a65b
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484548"
---
# <a name="simplified-null-argument-checking"></a>Verificação de argumento nulo simplificado

## <a name="summary"></a>Resumo
Esta proposta fornece uma sintaxe simplificada para validar argumentos de método não é `null` e gerar `ArgumentNullException` adequadamente.

## <a name="motivation"></a>Motivação
O trabalho de design de tipos de referência anuláveis nos fez examinar o código necessário para `null` validação de argumento. Considerando que o NRT não afeta a execução de código, os desenvolvedores ainda devem adicionar `if (arg is null) throw` código de placa mais para os projetos que são totalmente `null` limpos. Isso nos forneceu o desejo de explorar uma sintaxe mínima para o argumento `null` validação no idioma. 

Embora seja esperado que essa `null` sintaxe de validação de parâmetro seja emparelhada com frequência com NRT, a proposta é totalmente independente dela. A sintaxe pode ser usada independentemente das diretivas de `#nullable`.

## <a name="detailed-design"></a>Design detalhado 

### <a name="null-validation-parameter-syntax"></a>Sintaxe de parâmetro de validação nula
O operador Bang, `!`, pode ser posicionado após um nome de parâmetro em uma lista de parâmetros e isso C# fará com que o compilador emita o código de verificação de `null` padrão para esse parâmetro. Isso é conhecido como `null` sintaxe de parâmetro de validação. Por exemplo:

``` csharp
void M(string name!) {
    ...
}
```

Será convertido em:

``` csharp
void M(string name) {
    if (name is null) {
        throw new ArgumentNullException(nameof(name));
    }
    ...
}
```

A verificação `null` gerada ocorrerá antes de qualquer código de desenvolvedor criado no método. Quando vários parâmetros contiverem o operador `!`, as verificações ocorrerão na mesma ordem em que os parâmetros são declarados.

``` csharp
void M(string p1, string p2) {
    if (p1 is null) {
        throw new ArgumentNullException(nameof(p1));
    }
    if (p2 is null) {
        throw new ArgumentNullException(nameof(p2));
    }
    ...
}
```

A verificação será especificamente para a igualdade de referência para `null`, ela não invoca `==` nem quaisquer operadores definidos pelo usuário. Isso também significa que o operador de `!` só pode ser adicionado a parâmetros cujo tipo pode ser testado para igualdade em relação a `null`. Isso significa que ele não pode ser usado em um parâmetro cujo tipo é conhecido como um tipo de valor.

``` csharp
// Error: Cannot use ! on parameters who types derive from System.ValueType
void G<T>(T arg!) where T : struct {

}
```

No caso de um construtor, a validação de `null` ocorrerá antes de qualquer outro código no construtor. Isso inclui: 

- Encadeamento com outros construtores com `this` ou `base` 
- Inicializadores de campo que ocorrem implicitamente no Construtor

Por exemplo:

``` csharp
class C {
    string field = GetString();
    C(string name!): this(name) {
        ...
    }
}
```

Será traduzido aproximadamente para o seguinte:

``` csharp
class C {
    C(string name)
        if (name is null) {
            throw new ArgumentNullException(nameof(name));
        }
        field = GetString();
        :this(name);
        ...
}
```

Observação: esse não é um C# código legal, mas sim apenas uma aproximação do que a implementação faz. 

A sintaxe do parâmetro de validação `null` também será válida em listas de parâmetros lambda. Isso é válido mesmo na sintaxe de parâmetro único que não tem parênteses.

``` csharp
void G() {
    // An identity lambda which throws on a null input
    Func<string, string> s = x! => x;
}
```

A sintaxe também é válida em parâmetros para métodos iteradores. Ao contrário de outro código no iterador, a validação de `null` ocorrerá quando o método do iterador for invocado, não quando o enumerador subjacente for movimentado. Isso é verdadeiro para iteradores tradicionais ou `async`s.

``` csharp
class Iterators {
    IEnumerable<char> GetCharacters(string s!) {
        foreach (var c in s) {
            yield return c;
        }
    }

    void Use() {
        // The invocation of GetCharacters will throw
        IEnumerable<char> e = GetCharacters(null);
    }
}
```

O operador `!` só pode ser usado para listas de parâmetros que têm um corpo de método associado. Isso significa que ele não pode ser usado em um método `abstract`, `interface`, `delegate` ou `partial` definição de método.

### <a name="extending-is-null"></a>A extensão é nula
Os tipos para os quais a expressão `is null` é válida serão estendidos para incluir parâmetros de tipo irrestrito. Isso permitirá que ele preencha a intenção de verificar `null` em todos os tipos que uma verificação de `null` é válida. Especificamente, esses são tipos que não são definitivamente conhecidos como tipos de valor. Por exemplo, parâmetros de tipo que são restritos a `struct` não podem ser usados com essa sintaxe.

``` csharp
void NullCheck<T1, T2>(T1 p1, T2 p2) where T2 : struct {
    // Okay: T1 could be a class or struct here.
    if (p1 is null) {
        ...
    }

    // Error 
    if (p2 is null) { 
        ...
    }
}
```

O comportamento de `is null` em um parâmetro de tipo será o mesmo que `== null` hoje. Nos casos em que o parâmetro de tipo é instanciado como um tipo de valor, o código será avaliado como `false`. Para casos em que é um tipo de referência, o código fará uma verificação de `is null` adequada.

### <a name="intersection-with-nullable-reference-types"></a>Interseção com tipos de referência anuláveis
Qualquer parâmetro que tenha um operador de `!` aplicado ao nome será iniciado com o estado anulável não `null`. Isso é verdadeiro mesmo que o tipo do parâmetro em si seja potencialmente `null`. Isso pode ocorrer com um tipo anulável explicitamente, como digamos `string?`ou com um parâmetro de tipo irrestrito. 

Quando uma sintaxe de `!` em parâmetros é combinada com um tipo explicitamente anulável no parâmetro, um aviso será emitido pelo compilador:

``` csharp
void WarnCase<T>(
    string? name!, // Warning: combining explicit null checking with a nullable type
    T value1 // Okay
)
```

## <a name="open-issues"></a>Problemas em aberto
Nenhum

## <a name="considerations"></a>Considerações

### <a name="constructors"></a>{1&gt;Construtores&lt;1}
A geração de código para construtores significa que há uma alteração de comportamento pequena, mas observável, ao mover da validação de `null` padrão hoje e a `null` sintaxe de parâmetro de validação (`!`). A `null` verificação na validação padrão ocorre depois de ambos os inicializadores de campo e de qualquer `base` ou `this` chamadas. Isso significa que um desenvolvedor não pode necessariamente migrar 100% de sua validação de `null` para a nova sintaxe. Os construtores exigem, pelo menos, alguma inspeção.

Após a discussão, foi decidido que isso é muito improvável de causar problemas significativos de adoção. É mais lógico que a `null` verificação seja executada antes de qualquer lógica no construtor. Pode revisitar se forem descobertos problemas de compatibilidade significativos.

### <a name="warning-when-mixing--and-"></a>Aviso ao misturar? e!
Houve uma discussão longa sobre se um aviso deve ou não ser emitido quando a sintaxe de `!` é aplicada a um parâmetro que é explicitamente digitado para um tipo anulável. Na superfície, ela parece uma declaração não-atestada pelo desenvolvedor, mas há casos em que as hierarquias de tipos poderiam forçar os desenvolvedores nessa situação. 

Considere a seguinte hierarquia de classe em uma série de assemblies (supondo que todos sejam compilados com `null` verificação habilitada):

``` csharp
// Assembly1
abstract class C1 {
    protected abstract void M(object o); 
}

// Assembly2
abstract class C2 : C1 {

}

// Assembly3
abstract class C3 : C2 { 
    protected override void M(object o!) {
        ...
    }
}
```

Aqui, o autor de `C3` decidiu adicionar `null` validação ao parâmetro `o`. Isso está totalmente em linha com o objetivo do recurso ser usado.

Agora imagine em uma data posterior o autor de Assembly2 decide adicionar a seguinte substituição:

``` csharp
// Assembly2
abstract class C2 : C1 {
   protected override void M(object? o) { 
       ...
   }
}
```

Isso é permitido por tipos de referência anuláveis, pois é legal tornar o contrato mais flexível para posições de entrada. O recurso NRT em geral permite co/contravariância razoáveis em parâmetro/retorno de nulidade. No entanto, o idioma faz a verificação de co/contravariância com base na substituição mais específica, não na declaração original. Isso significa que o autor de Assembly3 receberá um aviso sobre o tipo de `o` não correspondente e precisará alterar a assinatura para o seguinte para eliminá-la: 

``` csharp
// Assembly3
abstract class C3 : C2 { 
    protected override void M(object? o!) {
        ...
    }
}
```

Neste ponto, o autor do Assembly3 tem algumas opções:

- Eles podem aceitar/suprimir o aviso sobre `object?` e `object` incompatibilidade.
- Eles podem aceitar/suprimir o aviso sobre `object?` e `!` incompatibilidade.
- Eles podem apenas remover a verificação de validação de `null` (excluir `!` e fazer a verificação explícita)

Esse é um cenário real, mas, por enquanto, a ideia é avançar com o aviso. Se o aviso ocorrer com mais frequência do que antecipamos, podemos removê-lo mais tarde (o inverso não é verdadeiro).

### <a name="implicit-property-setter-arguments"></a>Argumentos de setter de propriedade implícita
O argumento `value` de um parâmetro é implícito e não aparece em nenhuma lista de parâmetros. Isso significa que ele não pode ser um destino desse recurso. A sintaxe setter de propriedade pode ser estendida para incluir uma lista de parâmetros para permitir que o operador de `!` seja aplicado. Mas isso recorta a ideia desse recurso, tornando `null` a validação mais simples. Assim, o argumento de `value` implícito simplesmente não funcionará com esse recurso.

## <a name="future-considerations"></a>Considerações futuras
Nenhum
