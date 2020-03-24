---
ms.openlocfilehash: 50f2bd2d0a84064cfe35fe65b9e5c59c052d19ac
ms.sourcegitcommit: 1dbb8e82bed5012a58a3a035bf2c3737ed570d07
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80122944"
---
# <a name="ranges"></a><span data-ttu-id="4f469-101">Intervalos</span><span class="sxs-lookup"><span data-stu-id="4f469-101">Ranges</span></span>

## <a name="summary"></a><span data-ttu-id="4f469-102">Resumo</span><span class="sxs-lookup"><span data-stu-id="4f469-102">Summary</span></span>

<span data-ttu-id="4f469-103">Esse recurso está prestes a fornecer dois novos operadores que permitem construir objetos `System.Index` e `System.Range` e usá-los para indexar/dividir as coleções em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="4f469-103">This feature is about delivering two new operators that allow constructing `System.Index` and `System.Range` objects, and using them to index/slice collections at runtime.</span></span>

## <a name="overview"></a><span data-ttu-id="4f469-104">Visão geral</span><span class="sxs-lookup"><span data-stu-id="4f469-104">Overview</span></span>

### <a name="well-known-types-and-members"></a><span data-ttu-id="4f469-105">Membros e tipos bem conhecidos</span><span class="sxs-lookup"><span data-stu-id="4f469-105">Well-known types and members</span></span>

<span data-ttu-id="4f469-106">Para usar as novas formas sintáticas para `System.Index` e `System.Range`, novos tipos e membros bem conhecidos podem ser necessários, dependendo de quais formulários sintáticas são usados.</span><span class="sxs-lookup"><span data-stu-id="4f469-106">To use the new syntactic forms for `System.Index` and `System.Range`, new well-known types and members may be necessary, depending on which syntactic forms are used.</span></span>

<span data-ttu-id="4f469-107">Para usar o operador "Hat" (`^`), é necessário o seguinte</span><span class="sxs-lookup"><span data-stu-id="4f469-107">To use the "hat" operator (`^`), the following is required</span></span>

```csharp
namespace System
{
    public readonly struct Index
    {
        public Index(int value, bool fromEnd);
    }
}
```

<span data-ttu-id="4f469-108">Para usar o tipo de `System.Index` como um argumento em um acesso de elemento de matriz, o membro a seguir é necessário:</span><span class="sxs-lookup"><span data-stu-id="4f469-108">To use the `System.Index` type as an argument in an array element access, the following member is required:</span></span>

```csharp
int System.Index.GetOffset(int length);
```

<span data-ttu-id="4f469-109">A sintaxe de `..` para `System.Range` exigirá o tipo de `System.Range`, bem como um ou mais dos seguintes membros:</span><span class="sxs-lookup"><span data-stu-id="4f469-109">The `..` syntax for `System.Range` will require the `System.Range` type, as well as one or more of the following members:</span></span>

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

<span data-ttu-id="4f469-110">A sintaxe `..` permite que ambos ou nenhum dos seus argumentos estejam ausentes.</span><span class="sxs-lookup"><span data-stu-id="4f469-110">The `..` syntax allows for either, both, or none of its arguments to be absent.</span></span> <span data-ttu-id="4f469-111">Independentemente do número de argumentos, o construtor de `Range` é sempre suficiente para usar a sintaxe `Range`.</span><span class="sxs-lookup"><span data-stu-id="4f469-111">Regardless of the number of arguments, the `Range` constructor is always sufficient for using the `Range` syntax.</span></span> <span data-ttu-id="4f469-112">No entanto, se algum dos outros membros estiver presente e um ou mais dos argumentos de `..` estiverem ausentes, o membro apropriado poderá ser substituído.</span><span class="sxs-lookup"><span data-stu-id="4f469-112">However, if any of the other members are present and one or more of the `..` arguments are missing, the appropriate member may be substituted.</span></span>

<span data-ttu-id="4f469-113">Por fim, para um valor do tipo `System.Range` a ser usado em uma expressão de acesso de elemento de matriz, o seguinte membro deve estar presente:</span><span class="sxs-lookup"><span data-stu-id="4f469-113">Finally, for a value of type `System.Range` to be used in an array element access expression, the following member must be present:</span></span>

```csharp
namespace System.Runtime.CompilerServices
{
    public static class RuntimeHelpers
    {
        public static T[] GetSubArray<T>(T[] array, System.Range range);
    }
}
```

### <a name="systemindex"></a><span data-ttu-id="4f469-114">System. index</span><span class="sxs-lookup"><span data-stu-id="4f469-114">System.Index</span></span>

<span data-ttu-id="4f469-115">C#Não tem como indexar uma coleção a partir do final, mas sim a maioria dos indexadores usam a noção "de início" ou uma expressão de "comprimento i".</span><span class="sxs-lookup"><span data-stu-id="4f469-115">C# has no way of indexing a collection from the end, but rather most indexers use the "from start" notion, or do a "length - i" expression.</span></span> <span data-ttu-id="4f469-116">Apresentamos uma nova expressão de índice que significa "do final".</span><span class="sxs-lookup"><span data-stu-id="4f469-116">We introduce a new Index expression that means "from the end".</span></span> <span data-ttu-id="4f469-117">O recurso apresentará um novo operador unário "Hat".</span><span class="sxs-lookup"><span data-stu-id="4f469-117">The feature will introduce a new unary prefix "hat" operator.</span></span> <span data-ttu-id="4f469-118">Seu único operando deve ser conversível para `System.Int32`.</span><span class="sxs-lookup"><span data-stu-id="4f469-118">Its single operand must be convertible to `System.Int32`.</span></span> <span data-ttu-id="4f469-119">Ele será reduzido para a chamada de método de fábrica de `System.Index` apropriada.</span><span class="sxs-lookup"><span data-stu-id="4f469-119">It will be lowered into the appropriate `System.Index` factory method call.</span></span>

<span data-ttu-id="4f469-120">Aumentamos a gramática para *unary_expression* com o seguinte formulário de sintaxe adicional:</span><span class="sxs-lookup"><span data-stu-id="4f469-120">We augment the grammar for *unary_expression* with the following additional syntax form:</span></span>

```antlr
unary_expression
    : '^' unary_expression
    ;
```

<span data-ttu-id="4f469-121">Chamamos isso de *índice do operador end* .</span><span class="sxs-lookup"><span data-stu-id="4f469-121">We call this the *index from end* operator.</span></span> <span data-ttu-id="4f469-122">O índice predefinido *dos operadores end* é o seguinte:</span><span class="sxs-lookup"><span data-stu-id="4f469-122">The predefined *index from end* operators are as follows:</span></span>

```csharp
    System.Index operator ^(int fromEnd);
```

<span data-ttu-id="4f469-123">O comportamento desse operador é definido apenas para valores de entrada maiores ou iguais a zero.</span><span class="sxs-lookup"><span data-stu-id="4f469-123">The behavior of this operator is only defined for input values greater than or equal to zero.</span></span>

<span data-ttu-id="4f469-124">Exemplos:</span><span class="sxs-lookup"><span data-stu-id="4f469-124">Examples:</span></span>

```csharp
var thirdItem = list[2];                // list[2]
var lastItem = list[^1];                // list[Index.CreateFromEnd(1)]

var multiDimensional = list[3, ^2]      // list[3, Index.CreateFromEnd(2)]
```

#### <a name="systemrange"></a><span data-ttu-id="4f469-125">System. Range</span><span class="sxs-lookup"><span data-stu-id="4f469-125">System.Range</span></span>

<span data-ttu-id="4f469-126">C#Não tem uma forma sintática de acessar "intervalos" ou "fatias" de coleções.</span><span class="sxs-lookup"><span data-stu-id="4f469-126">C# has no syntactic way to access "ranges" or "slices" of collections.</span></span> <span data-ttu-id="4f469-127">Normalmente, os usuários são forçados a implementar estruturas complexas para filtrar/operar em fatias de memória ou recorrer a métodos LINQ como `list.Skip(5).Take(2)`.</span><span class="sxs-lookup"><span data-stu-id="4f469-127">Usually users are forced to implement complex structures to filter/operate on slices of memory, or resort to LINQ methods like `list.Skip(5).Take(2)`.</span></span> <span data-ttu-id="4f469-128">Com a adição de `System.Span<T>` e outros tipos semelhantes, torna-se mais importante ter esse tipo de operação com suporte em um nível mais profundo no idioma/tempo de execução e ter a interface unificada.</span><span class="sxs-lookup"><span data-stu-id="4f469-128">With the addition of `System.Span<T>` and other similar types, it becomes more important to have this kind of operation supported on a deeper level in the language/runtime, and have the interface unified.</span></span>

<span data-ttu-id="4f469-129">A linguagem introduzirá um novo operador Range `x..y`.</span><span class="sxs-lookup"><span data-stu-id="4f469-129">The language will introduce a new range operator `x..y`.</span></span> <span data-ttu-id="4f469-130">É um operador binário infixo que aceita duas expressões.</span><span class="sxs-lookup"><span data-stu-id="4f469-130">It is a binary infix operator that accepts two expressions.</span></span> <span data-ttu-id="4f469-131">Ambos os operandos podem ser omitidos (exemplos abaixo) e devem ser conversíveis para `System.Index`.</span><span class="sxs-lookup"><span data-stu-id="4f469-131">Either operand can be omitted (examples below), and they have to be convertible to `System.Index`.</span></span> <span data-ttu-id="4f469-132">Ele será reduzido para a chamada de método de fábrica de `System.Range` apropriada.</span><span class="sxs-lookup"><span data-stu-id="4f469-132">It will be lowered to the appropriate `System.Range` factory method call.</span></span>

<span data-ttu-id="4f469-133">Substituimos as C# regras de gramática por *multiplicative_expression* pelo seguinte (para introduzir um novo nível de precedência):</span><span class="sxs-lookup"><span data-stu-id="4f469-133">We replace the C# grammar rules for *multiplicative_expression* with the following (in order to introduce a new precedence level):</span></span>

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

<span data-ttu-id="4f469-134">Todas as formas do *operador Range* têm a mesma precedência.</span><span class="sxs-lookup"><span data-stu-id="4f469-134">All forms of the *range operator* have the same precedence.</span></span> <span data-ttu-id="4f469-135">Esse novo grupo de precedência é menor do que os *operadores unários* e superiores aos *operadores aritméticos mulitiplicative*.</span><span class="sxs-lookup"><span data-stu-id="4f469-135">This new precedence group is lower than the *unary operators* and higher than the *mulitiplicative arithmetic operators*.</span></span>

<span data-ttu-id="4f469-136">Chamamos o operador de `..` do *operador Range*.</span><span class="sxs-lookup"><span data-stu-id="4f469-136">We call the `..` operator the *range operator*.</span></span> <span data-ttu-id="4f469-137">O operador de intervalo interno pode aproximadamente ser compreendido para corresponder à invocação de um operador interno deste formulário:</span><span class="sxs-lookup"><span data-stu-id="4f469-137">The built-in range operator can roughly be understood to correspond to the invocation of a built-in operator of this form:</span></span>

```csharp
    System.Range operator ..(Index start = 0, Index end = ^0);
```

<span data-ttu-id="4f469-138">Exemplos:</span><span class="sxs-lookup"><span data-stu-id="4f469-138">Examples:</span></span>

```csharp
var slice1 = list[2..^3];               // list[Range.Create(2, Index.CreateFromEnd(3))]
var slice2 = list[..^3];                // list[Range.ToEnd(Index.CreateFromEnd(3))]
var slice3 = list[2..];                 // list[Range.FromStart(2)]
var slice4 = list[..];                  // list[Range.All]

var multiDimensional = list[1..2, ..]   // list[Range.Create(1, 2), Range.All]
```

<span data-ttu-id="4f469-139">Além disso, `System.Index` deve ter uma conversão implícita de `System.Int32`, a fim de não precisar sobrecarregar para misturar inteiros e índices em assinaturas multidimensionais.</span><span class="sxs-lookup"><span data-stu-id="4f469-139">Moreover, `System.Index` should have an implicit conversion from `System.Int32`, in order not to need to overload for mixing integers and indexes over multi-dimensional signatures.</span></span>

## <a name="adding-index-and-range-support-to-existing-library-types"></a><span data-ttu-id="4f469-140">Adicionando suporte a índice e intervalo a tipos de biblioteca existentes</span><span class="sxs-lookup"><span data-stu-id="4f469-140">Adding Index and Range support to existing library types</span></span>

### <a name="implicit-index-support"></a><span data-ttu-id="4f469-141">Suporte a índice implícito</span><span class="sxs-lookup"><span data-stu-id="4f469-141">Implicit Index support</span></span>

<span data-ttu-id="4f469-142">O idioma fornecerá um membro do indexador de instância com um único parâmetro do tipo `Index` para tipos que atendam aos seguintes critérios:</span><span class="sxs-lookup"><span data-stu-id="4f469-142">The language will provide an instance indexer member with a single parameter of type `Index` for types which meet the following criteria:</span></span>

- <span data-ttu-id="4f469-143">O tipo é contável.</span><span class="sxs-lookup"><span data-stu-id="4f469-143">The type is Countable.</span></span>
- <span data-ttu-id="4f469-144">O tipo tem um indexador de instância acessível que usa um único `int` como o argumento.</span><span class="sxs-lookup"><span data-stu-id="4f469-144">The type has an accessible instance indexer which takes a single `int` as the argument.</span></span>
- <span data-ttu-id="4f469-145">O tipo não tem um indexador de instância acessível que usa um `Index` como o primeiro parâmetro.</span><span class="sxs-lookup"><span data-stu-id="4f469-145">The type does not have an accessible instance indexer which takes an `Index` as the first parameter.</span></span> <span data-ttu-id="4f469-146">O `Index` deve ser o único parâmetro ou os parâmetros restantes devem ser opcionais.</span><span class="sxs-lookup"><span data-stu-id="4f469-146">The `Index` must be the only parameter or the remaining parameters must be optional.</span></span>

<span data-ttu-id="4f469-147">Um tipo é ***contável*** se tiver uma propriedade chamada `Length` ou `Count` com um getter acessível e um tipo de retorno de `int`.</span><span class="sxs-lookup"><span data-stu-id="4f469-147">A type is ***Countable*** if it  has a property named `Length` or `Count` with an accessible getter and a return type of `int`.</span></span> <span data-ttu-id="4f469-148">O idioma pode fazer uso dessa propriedade para converter uma expressão do tipo `Index` em um `int` no ponto da expressão sem a necessidade de usar o tipo `Index` de forma alguma.</span><span class="sxs-lookup"><span data-stu-id="4f469-148">The language can make use of this property to convert an expression of type `Index` into an `int` at the point of the expression without the need to use the type `Index` at all.</span></span> <span data-ttu-id="4f469-149">Caso ambas `Length` e `Count` estejam presentes, `Length` será preferível.</span><span class="sxs-lookup"><span data-stu-id="4f469-149">In case both `Length` and `Count` are present, `Length` will be preferred.</span></span> <span data-ttu-id="4f469-150">Para simplificar o futuro, a proposta usará o nome `Length` para representar `Count` ou `Length`.</span><span class="sxs-lookup"><span data-stu-id="4f469-150">For simplicity going forward, the proposal will use the name `Length` to represent `Count` or `Length`.</span></span>

<span data-ttu-id="4f469-151">Para esses tipos, a linguagem agirá como se houver um membro do indexador no formato `T this[Index index]` em que `T` é o tipo de retorno do indexador baseado em `int`, incluindo quaisquer anotações de estilo `ref`.</span><span class="sxs-lookup"><span data-stu-id="4f469-151">For such types, the language will act as if there is an indexer member of the form `T this[Index index]` where `T` is the return type of the `int` based indexer including any `ref` style annotations.</span></span> <span data-ttu-id="4f469-152">O novo membro terá o mesmo `get` e `set` Membros com a acessibilidade correspondente como o indexador de `int`.</span><span class="sxs-lookup"><span data-stu-id="4f469-152">The new member will have the same `get` and `set` members with matching accessibility as the `int` indexer.</span></span> 

<span data-ttu-id="4f469-153">O novo indexador será implementado convertendo o argumento do tipo `Index` em um `int` e emitindo uma chamada para o indexador baseado em `int`.</span><span class="sxs-lookup"><span data-stu-id="4f469-153">The new indexer will be implemented by converting the argument of type `Index` into an `int` and emitting a call to the `int` based indexer.</span></span> <span data-ttu-id="4f469-154">Para fins de discussão, vamos usar o exemplo de `receiver[expr]`.</span><span class="sxs-lookup"><span data-stu-id="4f469-154">For discussion purposes, let's use the example of `receiver[expr]`.</span></span> <span data-ttu-id="4f469-155">A conversão de `expr` para `int` ocorrerá da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="4f469-155">The conversion of `expr` to `int` will occur as follows:</span></span>

- <span data-ttu-id="4f469-156">Quando o argumento estiver no formato `^expr2` e o tipo de `expr2` for `int`, ele será convertido em `receiver.Length - expr2`.</span><span class="sxs-lookup"><span data-stu-id="4f469-156">When the argument is of the form `^expr2` and the type of `expr2` is `int`, it will be translated to `receiver.Length - expr2`.</span></span>
- <span data-ttu-id="4f469-157">Caso contrário, ele será traduzido como `expr.GetOffset(receiver.Length)`.</span><span class="sxs-lookup"><span data-stu-id="4f469-157">Otherwise, it will be translated as `expr.GetOffset(receiver.Length)`.</span></span>

<span data-ttu-id="4f469-158">Isso permite que os desenvolvedores usem o recurso de `Index` em tipos existentes sem a necessidade de modificação.</span><span class="sxs-lookup"><span data-stu-id="4f469-158">This allows for developers to use the `Index` feature on existing types without the need for modification.</span></span> <span data-ttu-id="4f469-159">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="4f469-159">For example:</span></span>

``` csharp
List<char> list = ...;
var value = list[^1]; 

// Gets translated to 
var value = list[list.Count - 1]; 
```

<span data-ttu-id="4f469-160">As expressões `receiver` e `Length` serão despejadas conforme apropriado para garantir que os efeitos colaterais sejam executados apenas uma vez.</span><span class="sxs-lookup"><span data-stu-id="4f469-160">The `receiver` and `Length` expressions will be spilled as appropriate to ensure any side effects are only executed once.</span></span> <span data-ttu-id="4f469-161">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="4f469-161">For example:</span></span>

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

<span data-ttu-id="4f469-162">Esse código imprimirá "obter tamanho 3".</span><span class="sxs-lookup"><span data-stu-id="4f469-162">This code will print "Get Length 3".</span></span>

### <a name="implicit-range-support"></a><span data-ttu-id="4f469-163">Suporte a intervalo implícito</span><span class="sxs-lookup"><span data-stu-id="4f469-163">Implicit Range support</span></span>

<span data-ttu-id="4f469-164">O idioma fornecerá um membro do indexador de instância com um único parâmetro do tipo `Range` para tipos que atendam aos seguintes critérios:</span><span class="sxs-lookup"><span data-stu-id="4f469-164">The language will provide an instance indexer member with a single parameter of type `Range` for types which meet the following criteria:</span></span>

- <span data-ttu-id="4f469-165">O tipo é contável.</span><span class="sxs-lookup"><span data-stu-id="4f469-165">The type is Countable.</span></span>
- <span data-ttu-id="4f469-166">O tipo tem um membro acessível chamado `Slice` que tem dois parâmetros do tipo `int`.</span><span class="sxs-lookup"><span data-stu-id="4f469-166">The type has an accessible member named `Slice` which has two parameters of type `int`.</span></span>
- <span data-ttu-id="4f469-167">O tipo não tem um indexador de instância que usa um único `Range` como o primeiro parâmetro.</span><span class="sxs-lookup"><span data-stu-id="4f469-167">The type does not have an instance indexer which takes a single `Range` as the first parameter.</span></span> <span data-ttu-id="4f469-168">O `Range` deve ser o único parâmetro ou os parâmetros restantes devem ser opcionais.</span><span class="sxs-lookup"><span data-stu-id="4f469-168">The `Range` must be the only parameter or the remaining parameters must be optional.</span></span>

<span data-ttu-id="4f469-169">Para esses tipos, a linguagem será vinculada como se houver um membro do indexador no formato `T this[Range range]` em que `T` é o tipo de retorno do método `Slice`, incluindo quaisquer anotações de estilo `ref`.</span><span class="sxs-lookup"><span data-stu-id="4f469-169">For such types, the language will bind as if there is an indexer member of the form `T this[Range range]` where `T` is the return type of the `Slice` method including any `ref` style annotations.</span></span> <span data-ttu-id="4f469-170">O novo membro também terá a acessibilidade correspondente com o `Slice`.</span><span class="sxs-lookup"><span data-stu-id="4f469-170">The new member will also have matching accessibility with `Slice`.</span></span> 

<span data-ttu-id="4f469-171">Quando o indexador baseado em `Range` está associado em uma expressão chamada `receiver`, ele será reduzido convertendo a expressão `Range` em dois valores que são passados para o método `Slice`.</span><span class="sxs-lookup"><span data-stu-id="4f469-171">When the `Range` based indexer is bound on an expression named `receiver`, it will be lowered by converting the `Range` expression into two values that are then passed to the `Slice` method.</span></span> <span data-ttu-id="4f469-172">Para fins de discussão, vamos usar o exemplo de `receiver[expr]`.</span><span class="sxs-lookup"><span data-stu-id="4f469-172">For discussion purposes, let's use the example of `receiver[expr]`.</span></span>

<span data-ttu-id="4f469-173">O primeiro argumento de `Slice` será obtido convertendo a expressão com tipo de intervalo da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="4f469-173">The first argument of `Slice` will be obtained by converting the range typed expression in the following way:</span></span>

- <span data-ttu-id="4f469-174">Quando `expr` estiver no formato `expr1..expr2` (onde `expr2` pode ser omitido) e `expr1` tiver o tipo `int`, ele será emitido como `expr1`.</span><span class="sxs-lookup"><span data-stu-id="4f469-174">When `expr` is of the form `expr1..expr2` (where `expr2` can be omitted) and `expr1` has type `int`, then it will be emitted as `expr1`.</span></span>
- <span data-ttu-id="4f469-175">Quando `expr` estiver no formato `^expr1..expr2` (onde `expr2` pode ser omitido), ele será emitido como `receiver.Length - expr1`.</span><span class="sxs-lookup"><span data-stu-id="4f469-175">When `expr` is of the form `^expr1..expr2` (where `expr2` can be omitted), then it will be emitted as `receiver.Length - expr1`.</span></span>
- <span data-ttu-id="4f469-176">Quando `expr` estiver no formato `..expr2` (onde `expr2` pode ser omitido), ele será emitido como `0`.</span><span class="sxs-lookup"><span data-stu-id="4f469-176">When `expr` is of the form `..expr2` (where `expr2` can be omitted), then it will be emitted as `0`.</span></span>
- <span data-ttu-id="4f469-177">Caso contrário, ele será emitido como `expr.Start.GetOffset(receiver.Length)`.</span><span class="sxs-lookup"><span data-stu-id="4f469-177">Otherwise, it will be emitted as `expr.Start.GetOffset(receiver.Length)`.</span></span>

<span data-ttu-id="4f469-178">Esse valor será reutilizado no cálculo do segundo argumento `Slice`.</span><span class="sxs-lookup"><span data-stu-id="4f469-178">This value will be re-used in the calculation of the second `Slice` argument.</span></span> <span data-ttu-id="4f469-179">Ao fazer isso, ele será chamado de `start`.</span><span class="sxs-lookup"><span data-stu-id="4f469-179">When doing so it will be referred to as `start`.</span></span> <span data-ttu-id="4f469-180">O segundo argumento de `Slice` será obtido convertendo a expressão com tipo de intervalo da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="4f469-180">The second argument of `Slice` will be obtained by converting the range typed expression in the following way:</span></span>

- <span data-ttu-id="4f469-181">Quando `expr` estiver no formato `expr1..expr2` (onde `expr1` pode ser omitido) e `expr2` tiver o tipo `int`, ele será emitido como `expr2 - start`.</span><span class="sxs-lookup"><span data-stu-id="4f469-181">When `expr` is of the form `expr1..expr2` (where `expr1` can be omitted) and `expr2` has type `int`, then it will be emitted as `expr2 - start`.</span></span>
- <span data-ttu-id="4f469-182">Quando `expr` estiver no formato `expr1..^expr2` (onde `expr1` pode ser omitido), ele será emitido como `(receiver.Length - expr2) - start`.</span><span class="sxs-lookup"><span data-stu-id="4f469-182">When `expr` is of the form `expr1..^expr2` (where `expr1` can be omitted), then it will be emitted as `(receiver.Length - expr2) - start`.</span></span>
- <span data-ttu-id="4f469-183">Quando `expr` estiver no formato `expr1..` (onde `expr1` pode ser omitido), ele será emitido como `receiver.Length - start`.</span><span class="sxs-lookup"><span data-stu-id="4f469-183">When `expr` is of the form `expr1..` (where `expr1` can be omitted), then it will be emitted as `receiver.Length - start`.</span></span>
- <span data-ttu-id="4f469-184">Caso contrário, ele será emitido como `expr.End.GetOffset(receiver.Length) - start`.</span><span class="sxs-lookup"><span data-stu-id="4f469-184">Otherwise, it will be emitted as `expr.End.GetOffset(receiver.Length) - start`.</span></span>

<span data-ttu-id="4f469-185">As expressões `receiver`, `Length`e `expr` serão despejadas conforme apropriado para garantir que os efeitos colaterais sejam executados apenas uma vez.</span><span class="sxs-lookup"><span data-stu-id="4f469-185">The `receiver`, `Length`, and `expr` expressions will be spilled as appropriate to ensure any side effects are only executed once.</span></span> <span data-ttu-id="4f469-186">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="4f469-186">For example:</span></span>

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

<span data-ttu-id="4f469-187">Esse código imprimirá "obter comprimento 2".</span><span class="sxs-lookup"><span data-stu-id="4f469-187">This code will print "Get Length 2".</span></span>

<span data-ttu-id="4f469-188">O idioma fará o caso especial dos seguintes tipos conhecidos:</span><span class="sxs-lookup"><span data-stu-id="4f469-188">The language will special case the following known types:</span></span> 

- <span data-ttu-id="4f469-189">`string`: o método `Substring` será usado em vez de `Slice`.</span><span class="sxs-lookup"><span data-stu-id="4f469-189">`string`: the method `Substring` will be used instead of `Slice`.</span></span>
- <span data-ttu-id="4f469-190">`array`: o método `System.Reflection.CompilerServices.GetSubArray` será usado em vez de `Slice`.</span><span class="sxs-lookup"><span data-stu-id="4f469-190">`array`: the method `System.Reflection.CompilerServices.GetSubArray` will be used instead of `Slice`.</span></span>

## <a name="alternatives"></a><span data-ttu-id="4f469-191">Alternativas</span><span class="sxs-lookup"><span data-stu-id="4f469-191">Alternatives</span></span>

<span data-ttu-id="4f469-192">Os novos operadores (`^` e `..`) são uma simplificação sintática.</span><span class="sxs-lookup"><span data-stu-id="4f469-192">The new operators (`^` and `..`) are syntactic sugar.</span></span> <span data-ttu-id="4f469-193">A funcionalidade pode ser implementada por chamadas explícitas para `System.Index` e `System.Range` métodos de fábrica, mas isso resultará em muito mais código clichê, e a experiência será não intuitiva.</span><span class="sxs-lookup"><span data-stu-id="4f469-193">The functionality can be implemented by explicit calls to `System.Index` and `System.Range` factory methods, but it will result in a lot more boilerplate code, and the experience will be unintuitive.</span></span>

## <a name="il-representation"></a><span data-ttu-id="4f469-194">Representação de IL</span><span class="sxs-lookup"><span data-stu-id="4f469-194">IL Representation</span></span>

<span data-ttu-id="4f469-195">Esses dois operadores serão reduzidos para chamadas regulares de indexador/método, sem alteração nas camadas subsequentes do compilador.</span><span class="sxs-lookup"><span data-stu-id="4f469-195">These two operators will be lowered to regular indexer/method calls, with no change in subsequent compiler layers.</span></span>

## <a name="runtime-behavior"></a><span data-ttu-id="4f469-196">Comportamento do tempo de execução</span><span class="sxs-lookup"><span data-stu-id="4f469-196">Runtime behavior</span></span>

- <span data-ttu-id="4f469-197">O compilador pode otimizar indexadores para tipos internos, como matrizes e cadeias de caracteres, e reduzir a indexação para os métodos existentes apropriados.</span><span class="sxs-lookup"><span data-stu-id="4f469-197">Compiler can optimize indexers for built-in types like arrays and strings, and lower the indexing to the appropriate existing methods.</span></span>
- <span data-ttu-id="4f469-198">`System.Index` será lançada se construída com um valor negativo.</span><span class="sxs-lookup"><span data-stu-id="4f469-198">`System.Index` will throw if constructed with a negative value.</span></span>
- <span data-ttu-id="4f469-199">o `^0` não lança, mas se traduz no comprimento da coleção/enumerável para o qual ele é fornecido.</span><span class="sxs-lookup"><span data-stu-id="4f469-199">`^0` does not throw, but it translates to the length of the collection/enumerable it is supplied to.</span></span>
- <span data-ttu-id="4f469-200">`Range.All` é semanticamente equivalente a `0..^0`e pode ser desconstruído para esses índices.</span><span class="sxs-lookup"><span data-stu-id="4f469-200">`Range.All` is semantically equivalent to `0..^0`, and can be deconstructed to these indices.</span></span>

## <a name="considerations"></a><span data-ttu-id="4f469-201">Considerações</span><span class="sxs-lookup"><span data-stu-id="4f469-201">Considerations</span></span>

### <a name="detect-indexable-based-on-icollection"></a><span data-ttu-id="4f469-202">Detectar indexável com base em ICollection</span><span class="sxs-lookup"><span data-stu-id="4f469-202">Detect Indexable based on ICollection</span></span>

<span data-ttu-id="4f469-203">A inspiração para esse comportamento era inicializadores de coleção.</span><span class="sxs-lookup"><span data-stu-id="4f469-203">The inspiration for this behavior was collection initializers.</span></span> <span data-ttu-id="4f469-204">Usando a estrutura de um tipo para transmitir que ele tinha optado por um recurso.</span><span class="sxs-lookup"><span data-stu-id="4f469-204">Using the structure of a type to convey that it had opted into a feature.</span></span> <span data-ttu-id="4f469-205">No caso de tipos de inicializadores de coleção podem optar pelo recurso implementando a interface `IEnumerable` (não genérico).</span><span class="sxs-lookup"><span data-stu-id="4f469-205">In the case of collection initializers types can opt into the feature by implementing the interface `IEnumerable` (non generic).</span></span>

<span data-ttu-id="4f469-206">Inicialmente, essa proposta exigia que os tipos implementassem `ICollection` para se qualificarem como indexáveis.</span><span class="sxs-lookup"><span data-stu-id="4f469-206">This proposal initially required that types implement `ICollection` in order to qualify as Indexable.</span></span> <span data-ttu-id="4f469-207">Isso exigia vários casos especiais:</span><span class="sxs-lookup"><span data-stu-id="4f469-207">That required a number of special cases though:</span></span>

- <span data-ttu-id="4f469-208">`ref struct`: eles não podem implementar interfaces, mas tipos como `Span<T>` são ideais para suporte de índice/intervalo.</span><span class="sxs-lookup"><span data-stu-id="4f469-208">`ref struct`: these cannot implement interfaces yet types like `Span<T>` are ideal for index / range support.</span></span> 
- <span data-ttu-id="4f469-209">`string`: o não implementa `ICollection` e a adição de `interface` tem um grande custo.</span><span class="sxs-lookup"><span data-stu-id="4f469-209">`string`: does not implement `ICollection` and adding that `interface` has a large cost.</span></span>

<span data-ttu-id="4f469-210">Isso significa que o suporte a maiúsculas e minúsculas em tipos de chave já é necessário.</span><span class="sxs-lookup"><span data-stu-id="4f469-210">This means to support key types special casing is already needed.</span></span> <span data-ttu-id="4f469-211">A capitalização especial de `string` é menos interessante, pois a linguagem faz isso em outras áreas (`foreach` redução, constantes, etc...). A capitalização especial de `ref struct` é mais relacionada à medida que é especial em toda a classe de tipos.</span><span class="sxs-lookup"><span data-stu-id="4f469-211">The special casing of `string` is less interesting as the language does this in other areas (`foreach` lowering, constants, etc ...). The special casing of `ref struct` is more concerning as it's special casing an entire class of types.</span></span> <span data-ttu-id="4f469-212">Eles são rotulados como indexáveis se simplesmente tiverem uma propriedade chamada `Count` com um tipo de retorno de `int`.</span><span class="sxs-lookup"><span data-stu-id="4f469-212">They get labeled as Indexable if they simply have a property named `Count` with a return type of `int`.</span></span> 

<span data-ttu-id="4f469-213">Depois de considerar que o design foi normalizado para dizer que qualquer tipo que tenha uma propriedade `Count` / `Length` com um tipo de retorno de `int` é indexável.</span><span class="sxs-lookup"><span data-stu-id="4f469-213">After consideration the design was normalized to say that any type which has a property `Count` / `Length` with a return type of `int` is Indexable.</span></span> <span data-ttu-id="4f469-214">Isso remove todas as maiúsculas e minúsculas especiais, mesmo para `string` e matrizes.</span><span class="sxs-lookup"><span data-stu-id="4f469-214">That removes all special casing, even for `string` and arrays.</span></span>

### <a name="detect-just-count"></a><span data-ttu-id="4f469-215">Detectar apenas contagem</span><span class="sxs-lookup"><span data-stu-id="4f469-215">Detect just Count</span></span>

<span data-ttu-id="4f469-216">Detectar os nomes de propriedade `Count` ou `Length` complica o design um pouco.</span><span class="sxs-lookup"><span data-stu-id="4f469-216">Detecting on the property names `Count` or `Length` does complicate the design a bit.</span></span> <span data-ttu-id="4f469-217">Escolher apenas um para padronizar não é suficiente, pois acaba excluindo um grande número de tipos:</span><span class="sxs-lookup"><span data-stu-id="4f469-217">Picking just one to standardize though is not sufficient as it ends up excluding a large number of types:</span></span>

- <span data-ttu-id="4f469-218">Usar `Length`: exclui praticamente todas as coleções em System. Collections e subnamespaces.</span><span class="sxs-lookup"><span data-stu-id="4f469-218">Use `Length`: excludes pretty much every collection in System.Collections and sub-namespaces.</span></span> <span data-ttu-id="4f469-219">Eles tendem a derivar de `ICollection` e, portanto, preferem `Count` comprimento.</span><span class="sxs-lookup"><span data-stu-id="4f469-219">Those tend to derive from `ICollection` and hence prefer `Count` over length.</span></span>
- <span data-ttu-id="4f469-220">Usar `Count`: exclui `string`, matrizes, `Span<T>` e tipos baseados na maior `ref struct`</span><span class="sxs-lookup"><span data-stu-id="4f469-220">Use `Count`: excludes `string`, arrays, `Span<T>` and most `ref struct` based types</span></span>

<span data-ttu-id="4f469-221">A complicação extra na detecção inicial de tipos indexáveis é avaliada por sua simplificação em outros aspectos.</span><span class="sxs-lookup"><span data-stu-id="4f469-221">The extra complication on the initial detection of Indexable types is outweighed by its simplification in other aspects.</span></span>

### <a name="choice-of-slice-as-a-name"></a><span data-ttu-id="4f469-222">Opção de fatia como um nome</span><span class="sxs-lookup"><span data-stu-id="4f469-222">Choice of Slice as a name</span></span>

<span data-ttu-id="4f469-223">O nome `Slice` foi escolhido como é o nome padrão real para operações de estilo de fatia no .NET.</span><span class="sxs-lookup"><span data-stu-id="4f469-223">The name `Slice` was chosen as it's the de-facto standard name for slice style operations in .NET.</span></span> <span data-ttu-id="4f469-224">A partir do netcoreapp 2.1, todos os tipos de estilo de span usam o nome `Slice` para operações de divisão.</span><span class="sxs-lookup"><span data-stu-id="4f469-224">Starting with netcoreapp2.1 all span style types use the name `Slice` for slicing operations.</span></span> <span data-ttu-id="4f469-225">Antes do netcoreapp 2.1, realmente não há exemplos de fatias para procurar por um exemplo.</span><span class="sxs-lookup"><span data-stu-id="4f469-225">Prior to netcoreapp2.1 there really aren't any examples of slicing to look to for an example.</span></span> <span data-ttu-id="4f469-226">Os tipos como `List<T>`, `ArraySegment<T>`, `SortedList<T>` seriam ideais para fatiar, mas o conceito não existia quando os tipos foram adicionados.</span><span class="sxs-lookup"><span data-stu-id="4f469-226">Types like `List<T>`, `ArraySegment<T>`, `SortedList<T>` would've been ideal for slicing but the concept didn't exist when types were added.</span></span> 

<span data-ttu-id="4f469-227">Portanto, `Slice` sendo o único exemplo, ele foi escolhido como o nome.</span><span class="sxs-lookup"><span data-stu-id="4f469-227">Thus, `Slice` being the sole example, it was chosen as the name.</span></span>

### <a name="index-target-type-conversion"></a><span data-ttu-id="4f469-228">Conversão de tipo de destino de índice</span><span class="sxs-lookup"><span data-stu-id="4f469-228">Index target type conversion</span></span>

<span data-ttu-id="4f469-229">Outra maneira de exibir a transformação `Index` em uma expressão de indexador é como uma conversão de tipo de destino.</span><span class="sxs-lookup"><span data-stu-id="4f469-229">Another way to view the `Index` transformation in an indexer expression is as a target type conversion.</span></span> <span data-ttu-id="4f469-230">Em vez de associar como se houver um membro do formulário `return_type this[Index]`, a linguagem atribuirá uma conversão de tipo de destino a `int`.</span><span class="sxs-lookup"><span data-stu-id="4f469-230">Instead of binding as if there is a member of the form `return_type this[Index]`, the language instead assigns a target typed conversion to `int`.</span></span> 

<span data-ttu-id="4f469-231">Esse conceito pode ser generalizado para todos os acessos de membros em tipos de contagem.</span><span class="sxs-lookup"><span data-stu-id="4f469-231">This concept could be generalized to all member access on Countable types.</span></span> <span data-ttu-id="4f469-232">Sempre que uma expressão com o tipo `Index` é usada como um argumento para uma invocação de membro de instância e o receptor é contável, a expressão terá uma conversão de tipo de destino para `int`.</span><span class="sxs-lookup"><span data-stu-id="4f469-232">Whenever an expression with type `Index` is used as an argument to an instance member invocation and the receiver is Countable then the expression will have a target type conversion to `int`.</span></span> <span data-ttu-id="4f469-233">As invocações de membro aplicáveis a essa conversão incluem métodos, indexadores, propriedades, métodos de extensão, etc... Somente os construtores são excluídos, pois não têm nenhum receptor.</span><span class="sxs-lookup"><span data-stu-id="4f469-233">The member invocations applicable for this conversion include methods, indexers, properties, extension methods, etc ... Only constructors are excluded as they have no receiver.</span></span> 

<span data-ttu-id="4f469-234">A conversão de tipo de destino será implementada da seguinte maneira para qualquer expressão que tenha um tipo de `Index`.</span><span class="sxs-lookup"><span data-stu-id="4f469-234">The target type conversion will be implemented as follows for any expression which has a type of `Index`.</span></span> <span data-ttu-id="4f469-235">Para fins de discussão, vamos usar o exemplo de `receiver[expr]`:</span><span class="sxs-lookup"><span data-stu-id="4f469-235">For discussion purposes lets use the example of `receiver[expr]`:</span></span>

- <span data-ttu-id="4f469-236">Quando `expr` estiver no formato `^expr2` e o tipo de `expr2` for `int`, ele será convertido em `receiver.Length - expr2`.</span><span class="sxs-lookup"><span data-stu-id="4f469-236">When `expr` is of the form `^expr2` and the type of `expr2` is `int`, it will be translated to `receiver.Length - expr2`.</span></span>
- <span data-ttu-id="4f469-237">Caso contrário, ele será traduzido como `expr.GetOffset(receiver.Length)`.</span><span class="sxs-lookup"><span data-stu-id="4f469-237">Otherwise, it will be translated as `expr.GetOffset(receiver.Length)`.</span></span>

<span data-ttu-id="4f469-238">As expressões `receiver` e `Length` serão despejadas conforme apropriado para garantir que os efeitos colaterais sejam executados apenas uma vez.</span><span class="sxs-lookup"><span data-stu-id="4f469-238">The `receiver` and `Length` expressions will be spilled as appropriate to ensure any side effects are only executed once.</span></span> <span data-ttu-id="4f469-239">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="4f469-239">For example:</span></span>

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

<span data-ttu-id="4f469-240">Esse código imprimirá "obter tamanho 3".</span><span class="sxs-lookup"><span data-stu-id="4f469-240">This code will print "Get Length 3".</span></span> 

<span data-ttu-id="4f469-241">Esse recurso seria benéfico para qualquer membro que tivesse um parâmetro que representasse um índice.</span><span class="sxs-lookup"><span data-stu-id="4f469-241">This feature would be beneficial to any member which had a parameter that represented an index.</span></span> <span data-ttu-id="4f469-242">Por exemplo `List<T>.InsertAt`.</span><span class="sxs-lookup"><span data-stu-id="4f469-242">For example `List<T>.InsertAt`.</span></span> <span data-ttu-id="4f469-243">Isso também tem o potencial de confusão, pois a linguagem não pode fornecer orientações sobre se uma expressão é ou não destinada à indexação.</span><span class="sxs-lookup"><span data-stu-id="4f469-243">This also has the potential for confusion as the language can't give any guidance as to whether or not an expression is meant for indexing.</span></span> <span data-ttu-id="4f469-244">Tudo o que ele pode fazer é converter qualquer expressão de `Index` para `int` ao invocar um membro em um tipo de contagem.</span><span class="sxs-lookup"><span data-stu-id="4f469-244">All it can do is convert any `Index` expression to `int` when invoking a member on a Countable type.</span></span> 

<span data-ttu-id="4f469-245">Restrições:</span><span class="sxs-lookup"><span data-stu-id="4f469-245">Restrictions:</span></span>

- <span data-ttu-id="4f469-246">Essa conversão só é aplicável quando a expressão com o tipo `Index` é diretamente um argumento para o membro.</span><span class="sxs-lookup"><span data-stu-id="4f469-246">This conversion is only applicable when the expression with type `Index` is directly an argument to the member.</span></span> <span data-ttu-id="4f469-247">Ele não se aplicaria a nenhuma expressão aninhada.</span><span class="sxs-lookup"><span data-stu-id="4f469-247">It would not apply to any nested expressions.</span></span>

## <a name="decisions-made-during-implementation"></a><span data-ttu-id="4f469-248">Decisões tomadas durante a implementação</span><span class="sxs-lookup"><span data-stu-id="4f469-248">Decisions made during implementation</span></span>

- <span data-ttu-id="4f469-249">Todos os membros no padrão devem ser membros da instância</span><span class="sxs-lookup"><span data-stu-id="4f469-249">All members in the pattern must be instance members</span></span>
- <span data-ttu-id="4f469-250">Se um método de comprimento for encontrado, mas tiver o tipo de retorno incorreto, continue procurando por contagem</span><span class="sxs-lookup"><span data-stu-id="4f469-250">If a Length method is found but it has the wrong return type, continue looking for Count</span></span>
- <span data-ttu-id="4f469-251">O indexador usado para o padrão de índice deve ter exatamente um parâmetro int</span><span class="sxs-lookup"><span data-stu-id="4f469-251">The indexer used for the Index pattern must have exactly one int parameter</span></span>
- <span data-ttu-id="4f469-252">O método de fatia usado para o padrão de intervalo deve ter exatamente dois parâmetros int</span><span class="sxs-lookup"><span data-stu-id="4f469-252">The Slice method used for the Range pattern must have exactly two int parameters</span></span>
- <span data-ttu-id="4f469-253">Ao procurar os membros padrão, procuramos definições originais, não membros construídos</span><span class="sxs-lookup"><span data-stu-id="4f469-253">When looking for the pattern members, we look for original definitions, not constructed members</span></span>

## <a name="design-meetings"></a><span data-ttu-id="4f469-254">Criar reuniões</span><span class="sxs-lookup"><span data-stu-id="4f469-254">Design Meetings</span></span>

- https://github.com/dotnet/csharplang/blob/master/meetings/2019/LDM-2019-04-01.md

## <a name="questions"></a><span data-ttu-id="4f469-255">Perguntas</span><span class="sxs-lookup"><span data-stu-id="4f469-255">Questions</span></span>
