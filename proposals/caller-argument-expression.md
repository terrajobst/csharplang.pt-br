---
ms.openlocfilehash: fa3326bf69c83b6042b1db7b5567fd5c28d6f81a
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484520"
---
# <a name="callerargumentexpression"></a>CallerArgumentExpression

* [x] proposta
* [] Protótipo: não iniciado
* [] Implementação: não iniciada
* [] Especificação: não iniciada

## <a name="summary"></a>Resumo
[summary]: #summary

Permitir que os desenvolvedores capturem as expressões passadas para um método, a fim de permitir melhores mensagens de erro em APIs de diagnóstico/teste e reduzir pressionamentos de teclas.

## <a name="motivation"></a>Motivação
[motivation]: #motivation

Quando uma validação de asserção ou argumento falha, o desenvolvedor deseja saber o máximo possível sobre onde e por que ele falhou. No entanto, as APIs de diagnóstico atuais não facilitam totalmente isso. Considere o seguinte método:

```csharp
T Single<T>(this T[] array)
{
    Debug.Assert(array != null);
    Debug.Assert(array.Length == 1);

    return array[0];
}
```

Quando uma das declarações falhar, somente o nome do arquivo, o número da linha e o do método serão fornecidos no rastreamento de pilha. O desenvolvedor não conseguirá informar qual declaração falhou nessas informações--(s) ele terá que abrir o arquivo e navegar até o número de linha fornecido para ver o que deu errado.

Isso também é o motivo pelo qual as estruturas de teste precisam fornecer uma variedade de métodos Assert. Com xUnit, `Assert.True` e `Assert.False` não são frequentemente usados porque não fornecem contexto suficiente sobre o que falhou.

Embora a situação seja um pouco melhor para a validação de argumento porque os nomes de argumentos inválidos são mostrados para o desenvolvedor, o desenvolvedor deve passar esses nomes para exceções manualmente. Se o exemplo acima tiver sido reescrito para usar a validação de argumento tradicional em vez de `Debug.Assert`, ele ficaria como

```csharp
T Single<T>(this T[] array)
{
    if (array == null)
    {
        throw new ArgumentNullException(nameof(array));
    }

    if (array.Length != 1)
    {
        throw new ArgumentException("Array must contain a single element.", nameof(array));
    }

    return array[0];
}
```

Observe que `nameof(array)` deve ser passado para cada exceção, embora já esteja claro do contexto qual argumento é inválido.

## <a name="detailed-design"></a>Design detalhado
[design]: #detailed-design

Nos exemplos acima, incluindo a cadeia de caracteres `"array != null"` ou `"array.Length == 1"` na mensagem Assert ajudaria o desenvolvedor a determinar o que falhou. Insira `CallerArgumentExpression`: é um atributo que a estrutura pode usar para obter a cadeia de caracteres associada a um argumento de método específico. Vamos adicioná-lo a `Debug.Assert` como

```csharp
public static class Debug
{
    public static void Assert(bool condition, [CallerArgumentExpression("condition")] string message = null);
}
```

O código-fonte no exemplo acima permaneceria o mesmo. No entanto, o código que o compilador realmente emite corresponde a

```csharp
T Single<T>(this T[] array)
{
    Debug.Assert(array != null, "array != null");
    Debug.Assert(array.Length == 1, "array.Length == 1");

    return array[0];
}
```

O compilador reconhece especialmente o atributo em `Debug.Assert`. Ele passa a cadeia de caracteres associada ao argumento referido no construtor do atributo (nesse caso, `condition`) no site de chamada. Quando a declaração falha, o desenvolvedor mostrará a condição que foi falsa e saberá qual delas falhou.

Para validação de argumento, o atributo não pode ser usado diretamente, mas pode ser usado por meio de uma classe auxiliar:

```csharp
public static class Verify
{
    public static void Argument(bool condition, string message, [CallerArgumentExpression("condition")] string conditionExpression = null)
    {
        if (!condition) throw new ArgumentException(message: message, paramName: conditionExpression);
    }

    public static void InRange(int argument, int low, int high,
        [CallerArgumentExpression("argument")] string argumentExpression = null,
        [CallerArgumentExpression("low")] string lowExpression = null,
        [CallerArgumentExpression("high")] string highExpression = null)
    {
        if (argument < low)
        {
            throw new ArgumentOutOfRangeException(paramName: argumentExpression,
                message: $"{argumentExpression} ({argument}) cannot be less than {lowExpression} ({low}).");
        }

        if (argument > high)
        {
            throw new ArgumentOutOfRangeException(paramName: argumentExpression,
                message: $"{argumentExpression} ({argument}) cannot be greater than {highExpression} ({high}).");
        }
    }

    public static void NotNull<T>(T argument, [CallerArgumentExpression("argument")] string argumentExpression = null)
        where T : class
    {
        if (argument == null) throw new ArgumentNullException(paramName: argumentExpression);
    }
}

T Single<T>(this T[] array)
{
    Verify.NotNull(array); // paramName: "array"
    Verify.Argument(array.Length == 1, "Array must contain a single element."); // paramName: "array.Length == 1"

    return array[0];
}

T ElementAt(this T[] array, int index)
{
    Verify.NotNull(array); // paramName: "array"
    // paramName: "index"
    // message: "index (-1) cannot be less than 0 (0).", or
    //          "index (6) cannot be greater than array.Length - 1 (5)."
    Verify.InRange(index, 0, array.Length - 1);

    return array[index];
}
```

Uma proposta para adicionar tal classe auxiliar à estrutura está em andamento em https://github.com/dotnet/corefx/issues/17068. Se esse recurso de idioma foi implementado, a proposta poderá ser atualizada para aproveitar esse recurso.

### <a name="extension-methods"></a>Métodos de extensão

O parâmetro `this` em um método de extensão pode ser referenciado por `CallerArgumentExpression`. Por exemplo:

```csharp
public static void ShouldBe<T>(this T @this, T expected, [CallerArgumentExpression("this")] string thisExpression = null) {}

contestant.Points.ShouldBe(1337); // thisExpression: "contestant.Points"
```

`thisExpression` receberá a expressão correspondente ao objeto antes do ponto. Se for chamado com a sintaxe do método estático, por exemplo, `Ext.ShouldBe(contestant.Points, 1337)`, ele se comportará como se o primeiro parâmetro não estivesse marcado como `this`.

Sempre deve haver uma expressão correspondente ao parâmetro `this`. Mesmo que uma instância de uma classe chame um método de extensão por si só, por exemplo, `this.Single()` de dentro de um tipo de coleção, a `this` é obrigatória pelo compilador para que `"this"` seja passado. Se essa regra for alterada no futuro, podemos considerar a possibilidade de passar `null` ou a cadeia de caracteres vazia.

### <a name="extra-details"></a>Detalhes adicionais

- Assim como os outros atributos de `Caller*`, como `CallerMemberName`, esse atributo só pode ser usado em parâmetros com valores padrão.
- Vários parâmetros marcados com `CallerArgumentExpression` são permitidos, conforme mostrado acima.
- O namespace do atributo será `System.Runtime.CompilerServices`.
- Se `null` ou uma cadeia de caracteres que não seja um nome de parâmetro (por exemplo, `"notAParameterName"`) for fornecida, o compilador passará uma cadeia de caracteres vazia.

## <a name="drawbacks"></a>Desvantagens
[drawbacks]: #drawbacks

- As pessoas que sabem como usar descompiladores poderão ver parte do código-fonte em sites de chamada para métodos marcados com esse atributo. Isso pode ser indesejável/inesperado para software de código-fonte fechado.

- Embora isso não seja uma falha no próprio recurso, uma fonte de preocupação pode ser que exista uma API de `Debug.Assert` hoje que usa apenas um `bool`. Mesmo que a sobrecarga que pega uma mensagem tivesse seu segundo parâmetro marcado com esse atributo e tenha tornado opcional, o compilador ainda escolheria o não-Message na resolução de sobrecarga. Portanto, a sobrecarga de não mensagem teria de ser removida para tirar proveito desse recurso, que seria uma alteração significativa de quebra de binário (embora não de origem).

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

- Se for capaz de ver o código-fonte em sites de chamada para métodos que usam esse atributo se comprovem como um problema, podemos fazer a aceitação dos efeitos do atributo. Os desenvolvedores irão habilitá-lo por meio de um atributo `[assembly: EnableCallerArgumentExpression]` em todo o assembly inseridos `AssemblyInfo.cs`.
  - No caso de os efeitos do atributo não serem habilitados, os métodos de chamada marcados com o atributo não seriam um erro, para permitir que os métodos existentes usem o atributo e mantenham a compatibilidade de origem. No entanto, o atributo seria ignorado e o método seria chamado com qualquer valor padrão fornecido.

```csharp
// Assembly1

void Foo(string bar); // V1
void Foo(string bar, string barExpression = "not provided"); // V2
void Foo(string bar, [CallerArgumentExpression("bar")] string barExpression = "not provided"); // V3

// Assembly2

Foo(a); // V1: Compiles to Foo(a), V2, V3: Compiles to Foo(a, "not provided")
Foo(a, "provided"); // V2, V3: Compiles to Foo(a, "provided")

// Assembly3

[assembly: EnableCallerArgumentExpression]

Foo(a); // V1: Compiles to Foo(a), V2: Compiles to Foo(a, "not provided"), V3: Compiles to Foo(a, "a")
Foo(a, "provided"); // V2, V3: Compiles to Foo(a, "provided")
```

- Para evitar que o [problema] [ drawbacks] de compatibilidade binária ocorra toda vez que desejarmos adicionar novas informações do chamador ao `Debug.Assert`, uma solução alternativa seria adicionar um struct `CallerInfo` à estrutura que contém todas as informações necessárias sobre o chamador.

```csharp
struct CallerInfo
{
    public string MemberName { get; set; }
    public string TypeName { get; set; }
    public string Namespace { get; set; }
    public string FullTypeName { get; set; }
    public string FilePath { get; set; }
    public int LineNumber { get; set; }
    public int ColumnNumber { get; set; }
    public Type Type { get; set; }
    public MethodBase Method { get; set; }
    public string[] ArgumentExpressions { get; set; }
}

[Flags]
enum CallerInfoOptions
{
    MemberName = 1, TypeName = 2, ...
}

public static class Debug
{
    public static void Assert(bool condition,
        // If a flag is not set here, the corresponding CallerInfo member is not populated by the caller, so it's
        // pay-for-play friendly.
        [CallerInfo(CallerInfoOptions.FilePath | CallerInfoOptions.Method | CallerInfoOptions.ArgumentExpressions)] CallerInfo callerInfo = default(CallerInfo))
    {
        string filePath = callerInfo.FilePath;
        MethodBase method = callerInfo.Method;
        string conditionExpression = callerInfo.ArgumentExpressions[0];
        ...
    }
}

class Bar
{
    void Foo()
    {
        Debug.Assert(false);

        // Translates to:

        var callerInfo = new CallerInfo();
        callerInfo.FilePath = @"C:\Bar.cs";
        callerInfo.Method = MethodBase.GetCurrentMethod();
        callerInfo.ArgumentExpressions = new string[] { "false" };
        Debug.Assert(false, callerInfo);
    }
}
```

Isso foi proposto originalmente em https://github.com/dotnet/csharplang/issues/87.

Há algumas desvantagens dessa abordagem:

- Apesar de ser amigável de pagamento por reprodução, permitindo que você especifique quais propriedades são necessárias, ele ainda pode prejudicar o desempenho alocando uma matriz para as expressões/chamando `MethodBase.GetCurrentMethod` mesmo quando a declaração é aprovada.

- Além disso, embora a passagem de um novo sinalizador para o atributo `CallerInfo` não seja uma alteração significativa, `Debug.Assert` não terá a garantia de que realmente receba esse novo parâmetro de sites de chamada que foram compilados em uma versão antiga do método.

## <a name="unresolved-questions"></a>Perguntas não resolvidas
[unresolved]: #unresolved-questions

TBD

## <a name="design-meetings"></a>Criar reuniões

{1&gt;N/A&lt;1}
