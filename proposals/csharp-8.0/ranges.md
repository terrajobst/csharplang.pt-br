---
ms.openlocfilehash: d6519ff57b4a98c4eec8ccbf310303432ac3255e
ms.sourcegitcommit: 65ea1e6dc02853e37e7f2088e2b6cc08d01d1044
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "79485024"
---
# <a name="ranges"></a>Intervalos

## <a name="summary"></a>Resumo

Esse recurso está prestes a fornecer dois novos operadores que permitem construir objetos `System.Index` e `System.Range` e usá-los para indexar/dividir as coleções em tempo de execução.

## <a name="overview"></a>Visão geral

### <a name="well-known-types-and-members"></a>Membros e tipos bem conhecidos

Para usar as novas formas sintáticas para `System.Index` e `System.Range`, novos tipos e membros bem conhecidos podem ser necessários, dependendo de quais formulários sintáticas são usados.

Para usar o operador "Hat" (`^`), é necessário o seguinte

```csharp
namespace System
{
    public readonly struct Index
    {
        public Index(int value, bool fromEnd);
    }
}
```

Para usar o tipo de `System.Index` como um argumento em um acesso de elemento de matriz, o membro a seguir é necessário:

```csharp
int System.Index.GetOffset(int length);
```

A sintaxe de `..` para `System.Range` exigirá o tipo de `System.Range`, bem como um ou mais dos seguintes membros:

```csharp
namespace System
{
    public readonly struct Range
    {
        public Range(System.Index start, System.Index end);
        public static Range StartAt(System.Index start);
        public static Range EndAt(System.Index end);
        public static Range All { get; }
    }
}
```

A sintaxe `..` permite que ambos ou nenhum dos seus argumentos estejam ausentes. Independentemente do número de argumentos, o construtor de `Range` é sempre suficiente para usar a sintaxe `Range`. No entanto, se algum dos outros membros estiver presente e um ou mais dos argumentos de `..` estiverem ausentes, o membro apropriado poderá ser substituído.

Por fim, para um valor do tipo `System.Range` a ser usado em uma expressão de acesso de elemento de matriz, o seguinte membro deve estar presente:

```csharp
namespace System.Runtime.CompilerServices
{
    public static class RuntimeHelpers
    {
        public static T[] GetSubArray<T>(T[] array, System.Range range);
    }
}
```

### <a name="systemindex"></a>System. index

C#Não tem como indexar uma coleção a partir do final, mas sim a maioria dos indexadores usam a noção "de início" ou uma expressão de "comprimento i". Apresentamos uma nova expressão de índice que significa "do final". O recurso apresentará um novo operador unário "Hat". Seu único operando deve ser conversível para `System.Int32`. Ele será reduzido para a chamada de método de fábrica de `System.Index` apropriada.

Aumentamos a gramática para *unary_expression* com o seguinte formulário de sintaxe adicional:

```antlr
unary_expression
    : '^' unary_expression
    ;
```

Chamamos isso de *índice do operador end* . O índice predefinido *dos operadores end* é o seguinte:

```csharp
    System.Index operator ^(int fromEnd);
```

O comportamento desse operador é definido apenas para valores de entrada maiores ou iguais a zero.

Exemplos:

```csharp
var thirdItem = list[2];                // list[2]
var lastItem = list[^1];                // list[Index.CreateFromEnd(1)]

var multiDimensional = list[3, ^2]      // list[3, Index.CreateFromEnd(2)]
```

#### <a name="systemrange"></a>System. Range

C#Não tem uma forma sintática de acessar "intervalos" ou "fatias" de coleções. Normalmente, os usuários são forçados a implementar estruturas complexas para filtrar/operar em fatias de memória ou recorrer a métodos LINQ como `list.Skip(5).Take(2)`. Com a adição de `System.Span<T>` e outros tipos semelhantes, torna-se mais importante ter esse tipo de operação com suporte em um nível mais profundo no idioma/tempo de execução e ter a interface unificada.

A linguagem introduzirá um novo operador Range `x..y`. É um operador binário infixo que aceita duas expressões. Ambos os operandos podem ser omitidos (exemplos abaixo) e devem ser conversíveis para `System.Index`. Ele será reduzido para a chamada de método de fábrica de `System.Range` apropriada.

-Substituimos as C# regras de gramática por *multiplicative_expression* pelo seguinte (para introduzir um novo nível de precedência):

```antlr
range_expression
    : unary_expression
    | range_expression? '..' range_expression?
    ;

multiplicative_expression
    : range_expression
    | multiplicative_expression '*' range_expression
    | multiplicative_expression '/' range_expression
    | multiplicative_expression '%' range_expression
    ;
```

Todas as formas do *operador Range* têm a mesma precedência. Esse novo grupo de precedência é menor do que os *operadores unários* e superiores aos *operadores aritméticos mulitiplicative*.

Chamamos o operador de `..` do *operador Range*. O operador de intervalo interno pode aproximadamente ser compreendido para corresponder à invocação de um operador interno deste formulário:

```csharp
    System.Range operator ..(Index start = 0, Index end = ^0);
```

Exemplos:

```csharp
var slice1 = list[2..^3];               // list[Range.Create(2, Index.CreateFromEnd(3))]
var slice2 = list[..^3];                // list[Range.ToEnd(Index.CreateFromEnd(3))]
var slice3 = list[2..];                 // list[Range.FromStart(2)]
var slice4 = list[..];                  // list[Range.All]

var multiDimensional = list[1..2, ..]   // list[Range.Create(1, 2), Range.All]
```

Além disso, `System.Index` deve ter uma conversão implícita de `System.Int32`, a fim de não precisar sobrecarregar para misturar inteiros e índices em assinaturas multidimensionais.

## <a name="adding-index-and-range-support-to-existing-library-types"></a>Adicionando suporte a índice e intervalo a tipos de biblioteca existentes

### <a name="implicit-index-support"></a>Suporte a índice implícito

O idioma fornecerá um membro do indexador de instância com um único parâmetro do tipo `Index` para tipos que atendam aos seguintes critérios:

- O tipo é contável.
- O tipo tem um indexador de instância acessível que usa um único `int` como o argumento.
- O tipo não tem um indexador de instância acessível que usa um `Index` como o primeiro parâmetro. O `Index` deve ser o único parâmetro ou os parâmetros restantes devem ser opcionais.

Um tipo é ***contável*** se tiver uma propriedade chamada `Length` ou `Count` com um getter acessível e um tipo de retorno de `int`. O idioma pode fazer uso dessa propriedade para converter uma expressão do tipo `Index` em um `int` no ponto da expressão sem a necessidade de usar o tipo `Index` de forma alguma. Caso ambas `Length` e `Count` estejam presentes, `Length` será preferível. Para simplificar o futuro, a proposta usará o nome `Length` para representar `Count` ou `Length`.

Para esses tipos, a linguagem funcionará como se houver um membro de índice do formulário `T this[Index index]` em que `T` é o tipo de retorno do indexador baseado em `int`, incluindo quaisquer anotações de estilo `ref`. O novo membro terá o mesmo `get` e `set` Membros com a acessibilidade correspondente como o indexador de `int`. 

O novo indexador será implementado convertendo o argumento do tipo `Index` em um `int` e emitindo uma chamada para o indexador baseado em `int`. Para fins de discussão, vamos usar o exemplo de `receiver[expr]`. A conversão de `expr` para `int` ocorrerá da seguinte maneira:

- Quando o argumento estiver no formato `^expr2` e o tipo de `expr2` for `int`, ele será convertido em `receiver.Length - expr2`.
- Caso contrário, ele será traduzido como `expr.GetOffset(receiver.Length)`.

Isso permite que os desenvolvedores usem o recurso de `Index` em tipos existentes sem a necessidade de modificação. Por exemplo:

``` csharp
List<char> list = ...;
var value = list[^1]; 

// Gets translated to 
var value = list[list.Count - 1]; 
```

As expressões `receiver` e `Length` serão despejadas conforme apropriado para garantir que os efeitos colaterais sejam executados apenas uma vez. Por exemplo:

``` csharp
class Collection {
    private int[] _array = new[] { 1, 2, 3 };

    int Length {
        get {
            Console.Write("Length ");
            return _array.Length;
        }
    }

    int this[int index] => _array[index];
}

class SideEffect {
    Collection Get() {
        Console.Write("Get ");
        return new Collection();
    }

    void Use() { 
        int i = Get()[^1];
        Console.WriteLine(i);
    }
}
```

Esse código imprimirá "obter tamanho 3".

### <a name="implicit-range-support"></a>Suporte a intervalo implícito

O idioma fornecerá um membro do indexador de instância com um único parâmetro do tipo `Range` para tipos que atendam aos seguintes critérios:

- O tipo é contável.
- O tipo tem um membro acessível chamado `Slice` que tem dois parâmetros do tipo `int`.
- O tipo não tem um indexador de instância que usa um único `Range` como o primeiro parâmetro. O `Range` deve ser o único parâmetro ou os parâmetros restantes devem ser opcionais.

Para esses tipos, a linguagem será vinculada como se houver um membro de índice do formulário `T this[Range range]` em que `T` é o tipo de retorno do método `Slice`, incluindo quaisquer anotações de estilo `ref`. O novo membro também terá a acessibilidade correspondente com o `Slice`. 

Quando o indexador baseado em `Range` está associado em uma expressão chamada `receiver`, ele será reduzido convertendo a expressão `Range` em dois valores que são passados para o método `Slice`. Para fins de discussão, vamos usar o exemplo de `receiver[expr]`.

O primeiro argumento de `Slice` será obtido convertendo a expressão tipada da seguinte maneira:

- Quando `expr` estiver no formato `expr1..expr2` (onde `expr2` pode ser omitido) e `expr1` tiver o tipo `int`, ele será emitido como `expr1`.
- Quando `expr` estiver no formato `^expr1..expr2` (onde `expr2` pode ser omitido), ele será emitido como `receiver.Length - expr1`.
- Quando `expr` estiver no formato `..expr2` (onde `expr2` pode ser omitido), ele será emitido como `0`.
- Caso contrário, ele será emitido como `expr.Start.GetOffset(receiver.Length)`.

Esse valor será reutilizado no cálculo do segundo argumento `Slice`. Ao fazer isso, ele será chamado de `start`. O segundo argumento de `Slice` será obtido convertendo a expressão com tipo de intervalo da seguinte maneira:

- Quando `expr` estiver no formato `expr1..expr2` (onde `expr1` pode ser omitido) e `expr2` tiver o tipo `int`, ele será emitido como `expr2 - start`.
- Quando `expr` estiver no formato `expr1..^expr2` (onde `expr1` pode ser omitido), ele será emitido como `(receiver.Length - expr2) - start`.
- Quando `expr` estiver no formato `expr1..` (onde `expr1` pode ser omitido), ele será emitido como `receiver.Length - start`.
- Caso contrário, ele será emitido como `expr.End.GetOffset(receiver.Length) - start`.

As expressões `receiver`, `Length` e `expr` serão despejadas conforme apropriado para garantir que os efeitos colaterais sejam executados apenas uma vez. Por exemplo:

``` csharp
class Collection {
    private int[] _array = new[] { 1, 2, 3 };

    int Length {
        get {
            Console.Write("Length ");
            return _array.Length;
        }
    }

    int[] Slice(int start, int length) { 
        var slice = new int[length];
        Array.Copy(_array, start, slice, 0, length);
        return slice;
    }
}

class SideEffect {
    Collection Get() {
        Console.Write("Get ");
        return new Collection();
    }

    void Use() { 
        var array = Get()[0..2];
        Console.WriteLine(array.length);
    }
}
```

Esse código imprimirá "obter comprimento 2".

O idioma fará o caso especial dos seguintes tipos conhecidos: 

- `string`: o método `Substring` será usado em vez de `Slice`.
- `array`: o método `System.Reflection.CompilerServices.GetSubArray` será usado em vez de `Slice`.

## <a name="alternatives"></a>Alternativas

Os novos operadores (`^` e `..`) são uma simplificação sintática. A funcionalidade pode ser implementada por chamadas explícitas para `System.Index` e `System.Range` métodos de fábrica, mas isso resultará em muito mais código clichê, e a experiência será não intuitiva.

## <a name="il-representation"></a>Representação de IL

Esses dois operadores serão reduzidos para chamadas regulares de indexador/método, sem alteração nas camadas subsequentes do compilador.

## <a name="runtime-behavior"></a>Comportamento do tempo de execução

- O compilador pode otimizar indexadores para tipos internos, como matrizes e cadeias de caracteres, e reduzir a indexação para os métodos existentes apropriados.
- `System.Index` será lançada se construída com um valor negativo.
- o `^0` não lança, mas se traduz no comprimento da coleção/enumerável para o qual ele é fornecido.
- `Range.All` é semanticamente equivalente a `0..^0`e pode ser desconstruído para esses índices.

## <a name="considerations"></a>Considerações

### <a name="detect-indexable-based-on-icollection"></a>Detectar indexável com base em ICollection

A inspiração para esse comportamento era inicializadores de coleção. Usando a estrutura de um tipo para transmitir que ele tinha optado por um recurso. No caso de tipos de inicializadores de coleção podem optar pelo recurso implementando a interface `IEnumerable` (não genérico).

Inicialmente, essa proposta exigia que os tipos implementassem `ICollection` para se qualificarem como indexáveis. Isso exigia vários casos especiais:

- `ref struct`: eles não podem implementar interfaces, mas tipos como `Span<T>` são ideais para suporte de índice/intervalo. 
- `string`: o não implementa `ICollection` e a adição de `interface` tem um grande custo.

Isso significa que o suporte a maiúsculas e minúsculas em tipos de chave já é necessário. A capitalização especial de `string` é menos interessante, pois a linguagem faz isso em outras áreas (`foreach` redução, constantes, etc...). A capitalização especial de `ref struct` é mais relacionada à medida que é especial em toda a classe de tipos. Eles são rotulados como indexáveis se simplesmente tiverem uma propriedade chamada `Count` com um tipo de retorno de `int`. 

Depois de considerar que o design foi normalizado para dizer que qualquer tipo que tenha uma propriedade `Count` / `Length` com um tipo de retorno de `int` é indexável. Isso remove todas as maiúsculas e minúsculas especiais, mesmo para `string` e matrizes.

### <a name="detect-just-count"></a>Detectar apenas contagem

Detectar os nomes de propriedade `Count` ou `Length` complica o design um pouco. Escolher apenas um para padronizar não é suficiente, pois acaba excluindo um grande número de tipos:

- Usar `Length`: exclui praticamente todas as coleções em System. Collections e subnamespaces. Eles tendem a derivar de `ICollection` e, portanto, preferem `Count` comprimento.
- Usar `Count`: exclui `string`, matrizes, `Span<T>` e tipos baseados na maior `ref struct`

A complicação extra na detecção inicial de tipos indexáveis é avaliada por sua simplificação em outros aspectos.

### <a name="choice-of-slice-as-a-name"></a>Opção de fatia como um nome

O nome `Slice` foi escolhido como é o nome padrão real para operações de estilo de fatia no .NET. A partir do netcoreapp 2.1, todos os tipos de estilo de span usam o nome `Slice` para operações de divisão. Antes do netcoreapp 2.1, realmente não há exemplos de fatias para procurar por um exemplo. Os tipos como `List<T>`, `ArraySegment<T>`, `SortedList<T>` seriam ideais para fatiar, mas o conceito não existia quando os tipos foram adicionados. 

Portanto, `Slice` sendo o único exemplo, ele foi escolhido como o nome.

### <a name="index-target-type-conversion"></a>Conversão de tipo de destino de índice

Outra maneira de exibir a transformação `Index` em uma expressão de indexador é como uma conversão de tipo de destino. Em vez de associar como se houver um membro do formulário `return_type this[Index]`, a linguagem atribuirá uma conversão de tipo de destino a `int`. 

Esse conceito pode ser generalizado para todos os acessos de membros em tipos de contagem. Sempre que uma expressão com o tipo `Index` é usada como um argumento para uma invocação de membro de instância e o receptor é contável, a expressão terá uma conversão de tipo de destino para `int`. As invocações de membro aplicáveis a essa conversão incluem métodos, indexadores, propriedades, métodos de extensão, etc... Somente os construtores são excluídos, pois não têm nenhum receptor. 

A conversão de tipo de destino será implementada da seguinte maneira para qualquer expressão que tenha um tipo de `Index`. Para fins de discussão, vamos usar o exemplo de `receiver[expr]`:

- Quando `expr` estiver no formato `^expr2` e o tipo de `expr2` for `int`, ele será convertido em `receiver.Length - expr2`.
- Caso contrário, ele será traduzido como `expr.GetOffset(receiver.Length)`.

As expressões `receiver` e `Length` serão despejadas conforme apropriado para garantir que os efeitos colaterais sejam executados apenas uma vez. Por exemplo:

``` csharp
class Collection {
    private int[] _array = new[] { 1, 2, 3 };

    int Length {
        get {
            Console.Write("Length ");
            return _array.Length;
        }
    }

    int GetAt(int index) => _array[index];
}

class SideEffect {
    Collection Get() {
        Console.Write("Get ");
        return new Collection();
    }

    void Use() { 
        int i = Get().GetAt(^1);
        Console.WriteLine(i);
    }
}
```

Esse código imprimirá "obter tamanho 3". 

Esse recurso seria benéfico para qualquer membro que tivesse um parâmetro que representasse um índice. Por exemplo `List<T>.InsertAt`. Isso também tem o potencial de confusão, pois a linguagem não pode fornecer orientações sobre se uma expressão é ou não destinada à indexação. Tudo o que ele pode fazer é converter qualquer expressão de `Index` para `int` ao invocar um membro em um tipo de contagem. 

Restrições:

- Essa conversão só é aplicável quando a expressão com o tipo `Index` é diretamente um argumento para o membro. Ele não se aplicaria a nenhuma expressão aninhada.

## <a name="decisions-made-during-implementation"></a>Decisões tomadas durante a implementação

- Todos os membros no padrão devem ser membros da instância
- Se um método de comprimento for encontrado, mas tiver o tipo de retorno incorreto, continue procurando por contagem
- O indexador usado para o padrão de índice deve ter exatamente um parâmetro int
- O método de fatia usado para o padrão de intervalo deve ter exatamente dois parâmetros int
- Ao procurar os membros padrão, procuramos definições originais, não membros construídos

## <a name="design-meetings"></a>Criar reuniões

- https://github.com/dotnet/csharplang/blob/master/meetings/2019/LDM-2019-04-01.md

## <a name="questions"></a>Perguntas
