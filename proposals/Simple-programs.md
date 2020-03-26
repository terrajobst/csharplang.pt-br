---
ms.openlocfilehash: 54ae4ffabde6dca49b7e6bfb626d65837eabc8f5
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281938"
---
# <a name="simple-programs"></a>Programas simples

* [x] proposta
* [x] protótipo: iniciado
* [] Implementação: não iniciada
* [] Especificação: não iniciada

## <a name="summary"></a>Resumo
[summary]: #summary

Permita que uma sequência de *instruções* ocorra logo antes da *namespace_member_declaration*s de um *compilation_unit* (ou seja, o arquivo de origem).

A semântica é que se tal sequência de *instruções* estiver presente, a seguinte declaração de tipo, módulo, o nome do tipo real e o nome do método, seria emitido:

``` c#
static class Program
{
    static async Task Main()
    {
        // statements
    }
}
```

Consulte também https://github.com/dotnet/csharplang/issues/3117.

## <a name="motivation"></a>Motivação
[motivation]: #motivation

Há uma certa quantidade de clichê em torno mesmo dos programas mais simples, devido à necessidade de um método `Main` explícito. Isso parece chegar à forma de aprendizado de idioma e clareza do programa. O objetivo principal do recurso é, portanto, permitir C# programas sem texto clichê desnecessário em relação a eles, para fins de aprendizes e a clareza do código.

## <a name="detailed-design"></a>Design detalhado
[design]: #detailed-design

### <a name="syntax"></a>Sintaxe

A única sintaxe adicional é permitir uma sequência de *instruções*s em uma unidade de compilação, logo antes do *namespace_member_declaration*s:

``` antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? statement* namespace_member_declaration*
    ;
```

Somente um *compilation_unit* pode ter a *instrução*s. 

Exemplo:

``` c#
if (System.Environment.CommandLine.Length == 0
    || !int.TryParse(System.Environment.CommandLine, out int n)
    || n < 0) return;
Console.WriteLine(Fib(n).curr);

(int curr, int prev) Fib(int i)
{
    if (i == 0) return (1, 0);
    var (curr, prev) = Fib(i - 1);
    return (curr + prev, curr);
}
```

### <a name="semantics"></a>Semântica

Se quaisquer instruções de nível superior estiverem presentes em qualquer unidade de compilação do programa, o significado será como se elas fossem combinadas no corpo do bloco de um método `Main` de uma classe `Program` no namespace global, da seguinte maneira:

``` c#
static class Program
{
    static async Task Main()
    {
        // statements
    }
}
```

Observe que os nomes "Program" e "Main" são usados apenas para fins ilustrativos, os nomes reais usados pelo compilador são dependentes de implementação e nem o tipo, nem o método pode ser referenciado pelo nome do código-fonte.

O método é designado como o ponto de entrada do programa. Métodos explicitamente declarados que por convenção podem ser considerados como candidatos de ponto de entrada são ignorados. Um aviso é relatado quando isso acontece. É um erro especificar `-main:<type>` switch de compilador quando há instruções de nível superior.

Operações assíncronas são permitidas em instruções de nível superior para o grau em que são permitidas em instruções dentro de um método de ponto de entrada assíncrono regular. No entanto, eles não são necessários, se `await` expressões e outras operações assíncronas forem omitidas, nenhum aviso será produzido. Em vez disso, a assinatura do método de ponto de entrada gerado é equivalente a 
``` c#
    static void Main()
```

O exemplo acima produziria a seguinte declaração de método de `$Main`:

``` c#
static class $Program
{
    static void $Main()
    {
        if (System.Environment.CommandLine.Length == 0
            || !int.TryParse(System.Environment.CommandLine, out int n)
            || n < 0) return;
        Console.WriteLine(Fib(n).curr);
        
        (int curr, int prev) Fib(int i)
        {
            if (i == 0) return (1, 0);
            var (curr, prev) = Fib(i - 1);
            return (curr + prev, curr);
        }
    }
}
```

Ao mesmo tempo, um exemplo como este:
``` c#
await System.Threading.Tasks.Task.Delay(1000);
System.Console.WriteLine("Hi!");
```

produziria:
``` c#
static class $Program
{
    static async Task $Main()
    {
        await System.Threading.Tasks.Task.Delay(1000);
        System.Console.WriteLine("Hi!");
    }
}
```

### <a name="scope-of-top-level-local-variables-and-local-functions"></a>Escopo de variáveis locais de nível superior e funções locais

Embora as variáveis e funções locais de nível superior sejam "encapsuladas" no método de ponto de entrada gerado, elas ainda devem estar no escopo em todo o programa em cada unidade de compilação.
Para fins de avaliação de nome simples, depois que o namespace global for atingido:
- Primeiro, é feita uma tentativa de avaliar o nome dentro do método de ponto de entrada gerado e somente se essa tentativa falhar 
- A avaliação "regular" dentro da declaração de namespace global é executada. 

Isso pode levar ao nome sombreamento de namespaces e tipos declarados dentro do namespace global, bem como para sombreamento de nomes importados.

Se a avaliação de nome simples ocorrer fora das instruções de nível superior e a avaliação gerar uma variável ou função local de nível superior, isso deverá levar a um erro.

Dessa forma, protegemos nossa capacidade futura de abordar melhor as "funções de nível superior" (cenário 2 em https://github.com/dotnet/csharplang/issues/3117)e são capazes de fornecer diagnósticos úteis aos usuários que, por engano, acreditam que eles tenham suporte.

