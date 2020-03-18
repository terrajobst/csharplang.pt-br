---
ms.openlocfilehash: 1457c1eb018e12af30ce5b38be704bf8851d4b25
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "79485010"
---
# <a name="efficient-params-and-string-formatting"></a>Parâmetros eficientes e formatação de cadeia de caracteres

## <a name="summary"></a>Resumo
Essa combinação de recursos aumentará a eficiência da formatação `string` valores e a passagem de argumentos de estilo de `params`.

## <a name="motivation"></a>Motivação
A sobrecarga de alocação da formatação `string` valores pode dominar o desempenho de muitos aplicativos baseados em texto: da penalidade de Boxing dos tipos de `struct`, a alocação de `object[]` para `params` e as alocações de `string` intermediárias durante chamadas de `string.Format`. Para manter a eficiência, esses aplicativos geralmente precisam abandonar os recursos de produtividade, como `params` e `string` interpolação e mudar para soluções codificadas não padrão. 

Considere o MSBuild como exemplo. Isso é escrito usando vários recursos modernos C# por desenvolvedores que estão preocupados com o desempenho. No entanto, em um modelo de compilação de exemplo, o MSBuild gerará 262MB de alocação de `string` usando o mínimo de detalhes. Desse 1/2 das alocações estão as alocações de vida curta dentro de `string.Format`. Esses recursos poderiam remover grande parte disso do .NET desktop e colocá-lo em quase zero no .NET Core devido à disponibilidade do `Span<T>`

O conjunto de recursos de linguagem descritos aqui permitirá que os aplicativos continuem usando esses recursos, com pouca ou nenhuma variação na base de código do aplicativo, ao mesmo tempo que remove a sobrecarga de alocação não intencional na maioria dos casos.

## <a name="detailed-design"></a>Design detalhado 
Há um conjunto de recursos que serão usados aqui para obter esses resultados:

- Expandir `params` para dar suporte a um conjunto mais amplo de tipos de coleção.
- Permitir que os desenvolvedores personalizem como `string` interpolação é obtida. 
- Permitir que o `string` interpolado se associe a sobrecargas de `string.Format` mais eficientes.

### <a name="extending-params"></a>Estendendo parâmetros
O idioma permitirá `params` em uma assinatura de método para ter os tipos `Span<T>`, `ReadOnlySpan<T>` e `IEnumerable<T>`. As mesmas regras para invocação serão aplicadas a esses novos tipos que se aplicam a `params T[]`:

- Não é possível sobrecarregar onde a única diferença é uma palavra-chave `params`.
- Pode invocar passando uma série de argumentos que são conversíveis implicitamente para `T` ou um único `Span<T>` / 
`ReadOnlySpan<T>` / argumento `IEnumerable<T>`.
- Deve ser o último parâmetro em uma assinatura de método.
- Etc... 

As variantes `Span<T>` e `ReadOnlySpan<T>` serão referidas como `Span<T>` abaixo para manter a simplicidade. Nos casos em que o comportamento de `ReadOnlySpan<T>` difere, ele será explicitamente chamado. 

A vantagem do `Span<T>` variantes do `params` fornece isso, proporciona ao compilador uma grande flexibilidade na forma como aloca o armazenamento de backup para o valor de `Span<T>`. Com um `params T[]` o compilador deve alocar um novo `T[]` para cada invocação de um método `params`. Não é possível reutilizar porque ele deve assumir o receptor armazenado e reutilizado o parâmetro. Isso pode levar a uma grande ineficiência em métodos com muitas invocações de `params`.

Dadas `Span<T>` variantes são `ref struct` o receptor não pode armazenar o argumento. Portanto, o compilador pode otimizar os sites de chamada executando ações como reutilizar o argumento. Isso pode tornar as invocações repetidas muito eficientes em comparação com `T[]`. No entanto, o idioma não fará nenhuma garantia específica sobre como esses CallSites são otimizados. Observe apenas que o compilador é livre para usar valores diferentes de `T[]` ao invocar um método de `params Span<T>`. 

Uma dessas implementações potenciais é a seguinte. Considere a invocação de todos os `params` em um corpo de método. O compilador poderia alocar uma matriz que tem um tamanho igual à maior invocação de `params` e usá-la para todas as invocações criando instâncias de `Span<T>` de tamanho adequado na matriz. Por exemplo:

``` csharp
static class OneAllocation {
    static void Use(params Span<string> spans) {
        ...
    }

    static void Go() {
        Use("jaredpar");
        Use("hello", "world");
        Use("a", "longer", "set");
    }
}
```

O compilador pode optar por emitir o corpo de `Go` da seguinte maneira:

``` csharp
    static void Go() {
        var args = new string[3];
        args[0] = "jaredpar";
        Use(new Span<string>(args, start: 0, length: 1));

        args[0] = "hello";
        args[1] = "world";
        Use(new Span<string>(args, start: 0, length: 2));

        args[0] = "a";
        args[1] = "longer";
        args[2] = "set";
        Use(new Span<string>(args, start: 0, length: 3));
   }
```

Isso pode reduzir significativamente o número de matrizes alocadas em um aplicativo. As alocações podem ser ainda mais reduzidas se o tempo de execução fornecer utilitários para alocação de pilha mais inteligente de matrizes.

No entanto, essa otimização nem sempre pode ser aplicada. Embora o receptor não possa capturar o argumento `params`, ele ainda pode ser capturado no chamador quando há um `ref` ou um parâmetro `out / ref` que, por sua vez, é um tipo `ref struct`. 

``` csharp
static class SneakyCapture {
    static ref int M(params Span<T> span) => ref span[0];

    static void Oops() {
        // This now holds onto the memory backing the Span<T> 
        ref int r = ref M(42);
    }
}
```

No entanto, esses casos são detectáveis estaticamente. Ele potencialmente ocorre sempre que há um `ref` retorno ou um parâmetro `ref struct` passado por `out` ou `ref`. Nesse caso, o compilador deve alocar uma `T[]` atualizada para cada invocação. 

Várias outras estratégias de otimização potenciais são discutidas no final deste documento.

A variante de `IEnumerable<T>` é meramente uma sobrecarga de conveniência. Ele é útil em cenários que têm usos frequentes de `IEnumerable<T>`, mas também têm muito uso `params`. Quando invocado no `T` argumento, o armazenamento de backup será alocado como um `T[]` assim que `params T[]` for feito hoje.

### <a name="params-overload-resolution-changes"></a>alterações de resolução de sobrecarga de params
Essa proposta significa que a linguagem agora tem quatro variantes de `params` em que antes dela tinha uma. É sensato que os métodos definam sobrecargas de métodos que diferem apenas no tipo de declarações de `params`. 

Considere que `StringBuilder.AppendFormat` certamente adicionaria uma sobrecarga de `params ReadOnlySpan<object>` além da `params object[]`. Isso permitiria que ele melhorasse substancialmente o desempenho, reduzindo as alocações de coleta sem exigir nenhuma alteração no código de chamada. 

Para facilitar isso, a linguagem apresentará a seguinte regra de quebra de empate de resolução de sobrecarga. Quando os métodos candidatos diferem somente pelo parâmetro `params`, os candidatos serão preferidos na seguinte ordem:

1. `ReadOnlySpan<T>`
1. `Span<T>`
1. `T[]`
1. `IEnumerable<T>`

Essa ordem é a mais do que o menos eficiente para o caso geral.

### <a name="variant"></a>Variante
CoreFX está criando um protótipo de um novo tipo gerenciado chamado [Variant](https://github.com/dotnet/corefxlab/pull/2595). Esse tipo destina-se a ser usado em APIs que esperam valores heterogêneos, mas que não desejam a sobrecarga trazida usando `object` como o parâmetro. O tipo de `Variant` fornece armazenamento universal, mas evita a alocação Boxing para os tipos usados com mais frequência. Usar esse tipo em APIs como `string.Format` pode eliminar a sobrecarga de Boxing na maioria dos casos.

Esse tipo em si não é necessariamente especial para o idioma. Ele está sendo apresentado neste documento separadamente, embora se torne um detalhe de implementação de outras partes da proposta. 

### <a name="efficient-interpolated-strings"></a>Cadeias de caracteres interpoladas eficientes
As cadeias de caracteres interpoladas são um recurso popular C#, mas ineficiente, no. A sintaxe mais comum, usando um `string` interpolado como um `string`, se traduz em uma chamada de `string.Format(string, params object[])`. Isso incorrerá em alocações de Boxing para todos os tipos de valor, as alocações de `string` intermediárias como a implementação usa amplamente `object.ToString` para formatação, bem como as alocações de matriz, uma vez que o número de argumentos excede a quantidade de parâmetros nas sobrecargas "rápidas" de `string.Format`. 

O idioma alterará sua interpolação abaixo para considerar sobrecargas alternativas de `string.Format`. Ele considerará todas as formas de `string.Format(string, params)` e escolherá a sobrecarga "melhor", que satisfaz os tipos de argumentos.
A sobrecarga de `params` "melhor" será determinada pelas regras discutidas acima. Isso significa que os `string` interpolados agora podem ser associados a sobrecargas muito eficientes, como `string.Format(string format, params ReadOnlySpan<Variant> args)`. Em muitos casos, isso removerá todas as alocações intermediárias.

### <a name="customizable-interpolated-strings"></a>Cadeias de caracteres interpoladas personalizáveis
Os desenvolvedores podem personalizar o comportamento de cadeias de caracteres interpoladas com `FormattableString`. Ele contém os dados que entram em uma cadeia de caracteres interpolada: o formato `string` e os argumentos como uma matriz. Embora ainda tenha a conversão boxing e a matriz de argumentos, bem como a alocação para `FormattableString` (é um `abstract class`). Portanto, é de pouco uso para aplicativos que são de alocação pesada na formatação `string`.

Para tornar a formatação de cadeia de caracteres interpolada eficiente, a linguagem reconhecerá um novo tipo: `System.ValueFormattableString`. Todas as cadeias de caracteres interpoladas terão uma conversão de tipo de destino para esse tipo. Isso será implementado pela conversão da cadeia de caracteres interpolada na chamada `ValueFormattableString.Create` exatamente como é feito para `FormattableString.Create` hoje. O idioma dará suporte a todas as opções de `params` descritas neste documento ao procurar o método de `ValueFormattableString.Create` mais adequado. 

``` csharp
readonly struct ValueFormattableString {
    public static ValueFormattableString Create(Variant v) { ... } 
    public static ValueFormattableString Create(string s) { ... } 
    public static ValueFormattableString Create(string s, params ReadOnlySpan<Variant> collection) { ... } 
}

class ConsoleEx { 
    static void Write(ValueFormattableString f) { ... }
}

class Program { 
    static void Main() { 
        ConsoleEx.Write(42);
        ConsoleEx.Write($"hello {DateTime.UtcNow}");

        // Translates into 
        ConsoleEx.Write(ValueFormattableString.Create((Variant)42));
        ConsoleEx.Write(ValueFormattableString.Create(
            "hello {0}", 
            new Variant(DateTime.UtcNow));
    }
}
```

As regras de resolução de sobrecarga serão alteradas para preferir `ValueFormattableString` sobre `string` quando o argumento for uma cadeia de caracteres interpolada. Isso significa que será valioso ter sobrecargas que diferem apenas em `string` e `ValueFormattableString`. Atualmente, essa sobrecarga com `FormattableString` não é valiosa, pois o compilador sempre preferirá a versão `string` (a menos que o desenvolvedor Use uma conversão explícita). 

## <a name="open-issues"></a>Problemas em aberto

### <a name="valueformattablestring-breaking-change"></a>ValueFormattableString alteração significativa
A alteração para preferir `ValueFormattableString` durante a resolução de sobrecarga sobre `string` é uma alteração significativa. É possível que um desenvolvedor tenha definido um tipo chamado `ValueFormattableString` hoje e usá-lo em sobrecargas de método com `string`. Essa alteração proposta faria com que o compilador selecionasse uma sobrecarga diferente depois que esse conjunto de recursos fosse implementado. 

A possibilidade de isso parece razoavelmente baixa. O tipo precisaria do nome completo `System.ValueFormattableString` e precisaria ter `static` métodos chamados `Create`. Considerando que os desenvolvedores são altamente desencorajados de definir qualquer tipo no namespace de `System`, essa interrupção parece um comprometimento razoável.

### <a name="expanding-to-more-types"></a>Expandindo para mais tipos
Dado que estamos na área que devemos considerar adicionar `IList<T>`, `ICollection<T>` e `IReadOnlyList<T>` ao conjunto de coleções para os quais `params` tem suporte. Em termos de implementação, ele custará uma pequena quantia em relação ao outro trabalho aqui.

O LDM precisa decidir se a complicação do idioma vale a pena. A adição de `IEnumerable<T>` remove um ponto de conflito muito específico. Sem esse `params` solução, muitos clientes foram forçados a alocar `T[]` de um `IEnumerable<T>` ao chamar um método `params`. No entanto, a adição de `IEnumerable<T>` corrige isso. Não há um ponto de conflito específico que as outras interfaces corrigem aqui. 

## <a name="considerations"></a>Considerações

### <a name="variant2-and-variant3"></a>Variant2 e Variant3
A equipe do CoreFX também tem um conjunto de tipos de armazenamento não alocados para até três argumentos `Variant`. Esses são um único `Variant`, `Variant2` e `Variant3`. Todos têm um par de métodos para obter uma alocação livre `Span<Variant>` deles: `CreateSpan` e `KeepAlive`. Isso significa que para um `params Span<Variant>` de até três argumentos, o site de chamada pode ser totalmente livre de alocação.

``` csharp
static class ZeroAllocation {
    static void Use(params Span<Variant> spans) {
        ...
    }

    static void Go() {
        Use("hello", "world");
    }
}
```

O método `Go` pode ser reduzido para o seguinte:

``` csharp
static class ZeroAllocation {
    static void Go() {
        Variant2 _v;
        _v.Variant1 = new Variant("hello");
        _v.Variant2 = new Variant("word");
        Use(_v.CreateSpan());
        _v.KeepAlive();
    }
}
```

Isso requer muito pouco trabalho sobre a proposta para reutilizar `T[]` entre chamadas `params Span<T>`. O compilador já precisa gerenciar um temporário por chamada e fazer a limpeza do trabalho após (mesmo se, em um caso, estiver apenas marcando um temp interno como livre). 

Observação: a função `KeepAlive` só é necessária na área de trabalho. No .NET Core, o método não estará disponível e, portanto, o compilador não emitirá uma chamada para ele.

### <a name="clr-stack-allocation-helpers"></a>Auxiliares de alocação de pilha do CLR
O CLR só fornece [localloc](https://docs.microsoft.com/en-us/dotnet/api/system.reflection.emit.opcodes.localloc?redirectedfrom=MSDN&view=netframework-4.7.2) para alocação de pilha de memória contígua. Essa instrução é limitada, pois funciona apenas para tipos de `unmanaged`. Isso significa que ele não pode ser usado como uma solução universal para alocar com eficiência o armazenamento de backup para `params 
 Span<T>`. 

Essa limitação não é uma restrição fundamental, mas, em vez disso, mais um artefato de histórico. O CLR pode optar por adicionar novos códigos de op/intrínsecos que fornecem alocação de pilha universal. Eles podem então ser usados para alocar o armazenamento de backup para a maioria das chamadas `params Span<T>`.

``` csharp
static class BetterAllocation {
    static void Use(params Span<string> spans) {
        ...
    }

    static void Go() {
        Use("hello", "world");
    }
}
```

O método `Go` pode ser reduzido para o seguinte:

``` csharp
static class ZeroAllocation {
    static void Go() {
        Span<T> span = RuntimeIntrinsic.StackAlloc<string>(length: 2);
        span[0] = "hello";
        span[1] = "world";
        Use(span);
    }
}
```

Embora essa abordagem seja muito eficiente de heap, isso causa um uso de pilha extra. Em um algoritmo que tem uma pilha profunda e muitos `params` uso, é possível que isso possa fazer com que uma `StackOverflowException` seja gerada onde uma simples alocação de `T[]` seria bem sucedido. 

Infelizmente C# , não está configurado para o tipo de análise entre métodos, onde poderia fazer uma determinação informada de se a chamada deve ou não usar a alocação de pilha ou heap de `params`. Ele só pode realmente considerar cada chamada por conta própria.

O CLR é a melhor configuração para fazer esse tipo de determinação no tempo de execução. Portanto, provavelmente, o tempo de execução forneceria dois métodos para a alocação de pilha universal:

1. `Span<T> StackAlloc<T>(int length)`: isso tem os mesmos comportamentos e limitações do `localloc`, exceto que ele pode funcionar em qualquer tipo `T`. 
1. `Span<T> MaybeStackAlloc<T>(int length)`: esse tempo de execução pode optar por implementar isso fazendo uma alocação de pilha ou heap. Em seguida, o tempo de execução pode usar o contexto de execução no qual é chamado para determinar como o `Span<T>` é alocado. O chamador, no entanto, sempre o tratará como se fosse uma pilha alocada.

Para casos muito simples, como um para dois argumentos, o C# compilador sempre poderia usar `StackAlloc<T>` variante. Isso é improvável de contribuir significativamente para esgotamento de pilha na maioria dos casos. Para outros casos, o compilador pode optar por usar `MaybeStackAlloc<T>` em vez disso e permitir que o tempo de execução faça a chamada.

A forma como escolheremos provavelmente exigirá uma investigação mais profunda e o exame de aplicativos do mundo real. Mas se esses novos intrínsecos estiverem disponíveis, eles fornecerão esse tipo de flexibilidade.

### <a name="why-not-varargs"></a>Por que não é varargs? 
O recurso de [varargs](https://docs.microsoft.com/en-us/cpp/windows/variable-argument-lists-dot-dot-dot-cpp-cli?view=vs-2017) existente foi considerado como uma solução possível. No entanto, esse recurso destina C++-se principalmente a cenários de/CLI e tem buracos conhecidos para outros cenários. Além disso, há um custo significativo na portabilidade para o UNIX. Portanto, ele não foi visto como uma solução viável.

## <a name="related-issues"></a>Problemas relacionados
Esta especificação está relacionada aos seguintes problemas: 

- https://github.com/dotnet/csharplang/issues/1757
- https://github.com/dotnet/csharplang/issues/179
- https://github.com/dotnet/corefxlab/pull/2595

