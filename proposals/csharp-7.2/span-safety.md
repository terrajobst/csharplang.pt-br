---
ms.openlocfilehash: 6088a5cd41926d828013f1b8e5736fd2b7939e44
ms.sourcegitcommit: da452002c3f472165a0e1fa7759f494cc703ae31
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "79485038"
---
# <a name="compile-time-enforcement-of-safety-for-ref-like-types"></a>Tempo de compilação imposição de segurança para tipos semelhantes a ref

## <a name="introduction"></a>Introdução

O principal motivo para as regras de segurança adicionais ao lidar com tipos como `Span<T>` e `ReadOnlySpan<T>` é que esses tipos devem ser confinados para a pilha de execução.
 
Há dois motivos pelos quais `Span<T>` e tipos semelhantes devem ser tipos somente de pilha.

1. `Span<T>` é semanticamente uma struct que contém uma referência e um `(ref T data, int length)`de intervalo. Independentemente da implementação real, as gravações em tal struct não seriam atômicas. A "divisão" simultânea dessa estrutura levaria à possibilidade de `length` não corresponder à `data`, causando acessos fora do intervalo e violações de segurança de tipo, o que, por fim, pode resultar na corrupção de heap de GC no código "seguro" aparentemente.
2. Algumas implementações de `Span<T>` literalmente contêm um ponteiro gerenciado em um de seus campos. Não há suporte para ponteiros gerenciados, pois campos de objetos de heap e código que gerencia para colocar um ponteiro gerenciado no heap de GC normalmente falham no momento do JIT.

Todos os problemas acima seriam aliviados se instâncias de `Span<T>` estiverem restritas a existir somente na pilha de execução. 

Um problema adicional surge devido à composição. Geralmente, seria desejável criar tipos de dados mais complexos que incorporaram `Span<T>` e `ReadOnlySpan<T>` instâncias. Esses tipos compostos teriam de ser structos e compartilhariam todos os riscos e requisitos de `Span<T>`. Como resultado, as regras de segurança descritas aqui devem ser exibidas como aplicáveis a todo o intervalo de **_tipos de referência_** .

A [especificação de idioma de rascunho](#draft-language-specification) destina-se a garantir que os valores de um tipo de referência ocorra somente na pilha.

## <a name="generalized-ref-like-types-in-source-code"></a>Tipos de `ref-like` generalizados no código-fonte

`ref-like` structs são explicitamente marcadas no código-fonte usando o modificador `ref`:

```csharp
ref struct TwoSpans<T>
{
    // can have ref-like instance fields
    public Span<T> first;
    public Span<T> second;
} 

// error: arrays of ref-like types are not allowed. 
TwoSpans<T>[] arr = null;

```

A designação de uma struct como ref permitirá que a estrutura tenha campos de instância semelhantes a ref e também fará com que todos os requisitos de tipos de referência sejam aplicáveis à estrutura. 

## <a name="metadata-representation-or-ref-like-structs"></a>Representação de metadados ou estruturas semelhantes a ref

As estruturas semelhantes a ref serão marcadas com o atributo **System. Runtime. CompilerServices. IsRefLikeAttribute** .

O atributo será adicionado às bibliotecas base comuns, como `mscorlib`. Em um caso, se o atributo não estiver disponível, o compilador irá gerar um interno de forma semelhante a outros atributos incorporados sob demanda, como `IsReadOnlyAttribute`.

Uma medida adicional será tomada para evitar o uso de structs de referência em compiladores que não estejam familiarizados com as regras de segurança ( C# isso inclui compiladores anteriores ao em que esse recurso é implementado). 

Não tendo outras boas alternativas que funcionem em compiladores antigos sem manutenção, um atributo `Obsolete` com uma cadeia de caracteres conhecida será adicionado a todas as estruturas semelhantes a ref. Os compiladores que sabem como usar tipos de referência irão ignorar essa forma específica de `Obsolete`.

Uma representação de metadados típica:

```csharp
    [IsRefLike]
    [Obsolete("Types with embedded references are not supported in this version of your compiler.")]
    public struct TwoSpans<T>
    {
       // . . . .
    }
```

Observação: não é o objetivo de fazê-lo para que qualquer uso de tipos de referência em compiladores antigos falhe em 100%. Isso é difícil de atingir e não é estritamente necessário. Por exemplo, sempre há uma maneira de contornar o `Obsolete` usando código dinâmico ou, por exemplo, criando uma matriz de tipos de referência por meio de reflexão.

Em particular, se o usuário quiser colocar um atributo `Obsolete` ou `Deprecated` em um tipo de referência, não teremos nenhuma outra opção além de não emitir o predefinido, uma vez que `Obsolete` atributo não pode ser aplicado mais de uma vez.  

## <a name="examples"></a>Exemplos:

```csharp
SpanLikeType M1(ref SpanLikeType x, Span<byte> y)
{
    // this is all valid, unconcerned with stack-referring stuff
    var local = new SpanLikeType(y);
    x = local;
    return x;
}

void Test1(ref SpanLikeType param1, Span<byte> param2)
{
    Span<byte> stackReferring1 = stackalloc byte[10];
    var stackReferring2 = new SpanLikeType(stackReferring1);

    // this is allowed
    stackReferring2 = M1(ref stackReferring2, stackReferring1);

    // this is NOT allowed
    stackReferring2 = M1(ref param1, stackReferring1);

    // this is NOT allowed
    param1 = M1(ref stackReferring2, stackReferring1);

    // this is NOT allowed
    param2 = stackReferring1.Slice(10);

    // this is allowed
    param1 = new SpanLikeType(param2);

    // this is allowed
    stackReferring2 = param1;
}

ref SpanLikeType M2(ref SpanLikeType x)
{
    return ref x;
}

ref SpanLikeType Test2(ref SpanLikeType param1, Span<byte> param2)
{
    Span<byte> stackReferring1 = stackalloc byte[10];
    var stackReferring2 = new SpanLikeType(stackReferring1);

    ref var stackReferring3 = M2(ref stackReferring2);

    // this is allowed
    stackReferring3 = M1(ref stackReferring2, stackReferring1);

    // this is allowed
    M2(ref stackReferring3) = stackReferring2;

    // this is NOT allowed
    M1(ref param1) = stackReferring2;

    // this is NOT allowed
    param1 = stackReferring3;

    // this is NOT allowed
    return ref stackReferring3;

    // this is allowed
    return ref param1;
}

```

----------------

## <a name="draft-language-specification"></a>Especificação da linguagem de rascunho

Abaixo, descrevemos um conjunto de regras de segurança para tipos do tipo ref (`ref struct`s) para garantir que os valores desses tipos ocorram somente na pilha. Um conjunto de regras de segurança diferente e mais simples seria possível se os locais não puderem ser passados por referência. Essa especificação também permitiria a reatribuição segura de locais de referência.

### <a name="overview"></a>Visão geral

Nós associamos a cada expressão em tempo de compilação o conceito de qual escopo a expressão tem permissão para escapar, "seguro para escapar". Da mesma forma, para cada lvalue, mantemos um conceito de qual escopo uma referência a ele tem permissão para escapar, "ref-safe-to-escape". Para uma determinada expressão lvalue, elas podem ser diferentes.

Eles são análogos ao "seguro para retornar" do recurso de locais de referência, mas é mais refinado. Onde o "seguro-para-retornar" de uma expressão registra apenas se (ou não) ele pode escapar do método delimitador como um todo, os registros seguros para o escape para os quais o escopo pode escapar (em qual escopo ele pode não escapar). O mecanismo de segurança básico é imposto da seguinte maneira. Devido a uma atribuição de uma expressão E1 com um escopo de segurança para escape S1, para uma expressão (lvalue) E2 com escopo seguro para escape S2, será um erro se S2 for um escopo maior do que S1. Por construção, os dois escopos S1 e S2 estão em uma relação de aninhamento, porque uma expressão legal sempre é segura para retornar de algum escopo delimitando a expressão.

Por enquanto, é o suficiente para a finalidade da análise, dar suporte a apenas dois escopos – externos ao método e ao escopo de nível superior do método. Isso ocorre porque os valores like ref com escopos internos não podem ser criados e os locais de referência não dão suporte à reatribuição. As regras, no entanto, podem dar suporte a mais de dois níveis de escopo.

As regras precisas para a computação do status de *segurança para retorno* de uma expressão e as regras que regem a legalidade de expressões, seguem.

### <a name="ref-safe-to-escape"></a>ref-safe-to-escape

O *ref-safe-to-escape* é um escopo, colocando uma expressão lvalue, à qual é seguro para uma ref ao lvalue para escapar. Se esse escopo for o método inteiro, dizemos que uma referência ao lvalue é *segura para retornar* do método.

### <a name="safe-to-escape"></a>seguro para escapar

O *seguro para escapar* é um escopo, delimitando uma expressão, à qual é seguro para o valor escapar. Se esse escopo for o método inteiro, dizemos que um valor é *seguro para retornar* do método.

Uma expressão cujo tipo não é um tipo de `ref struct` é *seguro para retornar* de todo o método delimitador. Caso contrário, nos referimos às regras abaixo.

#### <a name="parameters"></a>Parâmetros

Um lvalue que designa um parâmetro formal é *ref-safe-to-escape* (por referência) da seguinte maneira:
- Se o parâmetro for um parâmetro `ref`, `out`ou `in`, ele será de *ref-safe-to-escape* de todo o método (por exemplo, por uma instrução `return ref`); ,
- Se o parâmetro for o parâmetro `this` de um tipo struct, ele será de *referência segura a escape* para o escopo de nível superior do método (mas não de todo o próprio método); [Exemplo](#struct-this-escape) de
- Caso contrário, o parâmetro é um parâmetro de valor e ele é de *referência segura para escape* para o escopo de nível superior do método (mas não do próprio método).

Uma expressão que é um Rvalue que designa o uso de um parâmetro formal é *segura para escapar* (por valor) do método inteiro (por exemplo, por uma instrução `return`). Isso se aplica ao parâmetro `this` também.

#### <a name="locals"></a>Locais

Um lvalue que designa uma variável local é *ref-safe-to-escape* (por referência) da seguinte maneira:
- Se a variável for uma variável `ref`, seu *ref-safe-to-escape* será obtido do *ref-safe-to-escape* de sua expressão de inicialização; ,
- A variável é *ref-safe-to-escape* no escopo no qual ele foi declarado.

Uma expressão que é um Rvalue que designa o uso de uma variável local é *segura para escapar* (por valor) da seguinte maneira:
- Mas a regra geral acima, um local cujo tipo não é um tipo de `ref struct` é *seguro para retornar* de todo o método delimitador.
- Se a variável for uma variável de iteração de um loop de `foreach`, o escopo de *segurança para escape* da variável será o mesmo que o *seguro para escapar* da expressão do loop de `foreach`.
- Um local de `ref struct` tipo e não inicializado no ponto de declaração é *seguro para retornar* de todo o método delimitador.
- Caso contrário, o tipo da variável é um tipo de `ref struct`, e a declaração da variável requer um inicializador. O escopo de *segurança para escape* da variável é o mesmo que o de *segurança para escapar* de seu inicializador.

#### <a name="field-reference"></a>Referência de campo

Um lvalue que designa uma referência a um campo `e.F`é *ref-safe-to-escape* (por referência) da seguinte maneira:
- Se `e` for de um tipo de referência, ele será de *ref-safe-to-escape* de todo o método; ,
- Se `e` for de um tipo de valor, seu *ref-safe-to-escape* será tirado da *referência-safe-to-escape* de `e`.

Um Rvalue que designa uma referência a um campo, `e.F`, tem um escopo *seguro para escapar* , que é o mesmo que o *seguro para escapar* do `e`.

#### <a name="operators-including-"></a>Operadores, incluindo `?:`

O aplicativo de um operador definido pelo usuário é tratado como uma invocação de método.

Para um operador que produz um Rvalue, como `e1 + e2` ou `c ? e1 : e2`, o seguro- *para-escape* do resultado é o escopo mais estreito entre os operandos de *segurança para escape* do operador.  Em consequência disso, para um operador unário que produz um Rvalue, como `+e`, o *seguro para escapar* do resultado é o *seguro para escapar* do operando.

Para um operador que produz um lvalue, como `c ? ref e1 : ref e2`
- a *ref-safe-to-escape* do resultado é o escopo mais estreito entre o *ref-safe-to-escape* dos operandos do operador.
- o *seguro para escapar* dos operandos deve concordar, e isso é o *seguro para escapar* do lvalue resultante.

#### <a name="method-invocation"></a>Invocação de método

Um lvalue resultante de uma invocação de método de retorno de referência `e1.M(e2, ...)` é de *referência segura para escapar* do menor dos seguintes escopos:
- O método de circunscrição inteiro
- o *ref-safe-to-escape* de todas as expressões de argumento `ref` e `out` (excluindo o receptor)
- Para cada parâmetro de `in` do método, se houver uma expressão correspondente que seja um lvalue, sua *ref-safe-to-escape*, caso contrário, o escopo de delimitação mais próximo
- o *seguro para escapar* de todas as expressões de argumento (incluindo o receptor)

> Observação: o último marcador é necessário para manipular código como
> ```csharp
> var sp = new Span(...)
> return ref sp[0];
> ```
> ou
> ```csharp
> return ref M(sp, 0);
> ```

Um Rvalue resultante de uma invocação de método `e1.M(e2, ...)` é *seguro para escapar* do menor dos seguintes escopos:
- O método de circunscrição inteiro
- o *seguro para escapar* de todas as expressões de argumento (incluindo o receptor)

#### <a name="an-rvalue"></a>Um Rvalue
Um Rvalue é *ref-safe-to-escape* do escopo de delimitação mais próximo. Isso ocorre, por exemplo, em uma invocação, como `M(ref d.Length)` em que `d` é do tipo `dynamic`. Ele também é consistente com (e talvez incorporou) nossa manipulação de argumentos correspondentes a parâmetros de `in`. *

#### <a name="property-invocations"></a>Invocações de propriedade

Uma invocação de propriedade (`get` ou `set`) tratada como uma invocação de método do método subjacente pelas regras acima.

#### `stackalloc`

Uma expressão stackalloc é um Rvalue que é *seguro para escapar* do escopo de nível superior do método (mas não de todo o próprio método).

#### <a name="constructor-invocations"></a>Invocações de Construtor

Uma `new` expressão que invoca um Construtor obedece às mesmas regras que uma invocação de método que é considerada para retornar o tipo que está sendo construído.

Além disso, o *safe-to-escape* não é mais largo do que o menor de *todos os argumentos* /operandos das expressões de inicializador de objeto, recursivamente, se o inicializador estiver presente. 

#### <a name="span-constructor"></a>Construtor de span
O idioma depende de `Span<T>` não ter um construtor da seguinte forma:

```csharp
void Example(ref int x)
{
    // Create a span of length one
    var span = new Span<int>(ref x); 
}
```

Esse construtor faz `Span<T>` que são usados como campos indistinguíveis de um campo `ref`. As regras de segurança descritas neste documento dependem de `ref` campos que não são uma construção C#válida no ou no .net.

#### <a name="default-expressions"></a>Expressões `default`

Uma expressão de `default` é *segura para escapar* de todo o método delimitador.

## <a name="language-constraints"></a>Restrições de idioma

Gostaríamos de garantir que nenhuma variável local `ref` e nenhuma variável de `ref struct` tipo, se refere à memória de pilha ou a variáveis que não estão mais ativas. Portanto, temos as seguintes restrições de idioma:

- Nem um parâmetro ref, nem um local ref, nem um parâmetro ou local de um tipo de `ref struct` podem ser divididos em uma função lambda ou local.

- Nem um parâmetro ref nem um parâmetro de um tipo `ref struct` pode ser um argumento em um método Iterator ou um método `async`.

- Nem um local de referência, nem um local de um tipo de `ref struct` pode estar no escopo no ponto de uma instrução `yield return` ou uma expressão `await`.

- Um tipo de `ref struct` não pode ser usado como um argumento de tipo ou como um tipo de elemento em um tipo de tupla.

- Um tipo de `ref struct` não pode ser o tipo declarado de um campo, exceto que ele pode ser o tipo declarado de um campo de instância de outro `ref struct`.

- Um tipo de `ref struct` não pode ser o tipo de elemento de uma matriz.

- Um valor de um tipo de `ref struct` não pode ser Boxed:
  - Não há nenhuma conversão de um tipo de `ref struct` para o tipo `object` ou o tipo `System.ValueType`.
  - Um tipo de `ref struct` não pode ser declarado para implementar qualquer interface
  - Nenhum método de instância declarado em `object` ou em `System.ValueType`, mas não substituído em um tipo `ref struct` pode ser chamado com um destinatário desse tipo de `ref struct`.
  - Nenhum método de instância de um tipo de `ref struct` pode ser capturado pela conversão de método para um tipo delegado.

- Para uma `ref e1 = ref e2`de reatribuição de referência, o *ref-safe-to-escape* de `e2` deve ser pelo menos tão amplo quanto o escopo de *ref-safe-to-escape* de `e1`.

- Para uma instrução de retorno de ref `return ref e1`, o *ref-safe-to-escape* de `e1` deve ser *ref-safe-to-escape* de todo o método. (TODO: também precisamos de uma regra que `e1` deve ser *segura para escapar* do método inteiro ou que seja redundante?)

- Para uma instrução de retorno `return e1`, a *segurança* de `e1` deve ser *segura para escapar* do método inteiro.

- Para uma atribuição `e1 = e2`, se o tipo de `e1` for um tipo de `ref struct`, o *seguro para escape* de `e2` deve ser pelo menos tão amplo quanto o escopo da `e1`de *segurança para escape* .

- Para uma invocação de método se houver um argumento `ref` ou `out` de um tipo de `ref struct` (incluindo o receptor), com E1 de *segurança para escape* , nenhum argumento (incluindo o receptor) poderá ter um limite *mais* estreito do que o E1. [Amostra](#method-arguments-must-match)

- Uma função local ou anônima pode não se referir a um local ou parâmetro do tipo `ref struct` declarado em um escopo delimitador.

> ***Abrir problema:*** Precisamos de alguma regra que nos permita produzir um erro quando precisar despejar um valor de pilha de um tipo de `ref struct` em uma expressão Await, por exemplo, no código
> ```csharp
> Foo(new Span<int>(...), await e2);
> ```

## <a name="explanations"></a>Sobre
Essas explicações e exemplos ajudam a explicar por que muitas das regras de segurança acima existem

### <a name="method-arguments-must-match"></a>Argumentos de método devem corresponder
Ao invocar um método em que há um `out`, `ref` parâmetro que é um `ref struct` incluindo o destinatário, todas as `ref struct` precisam ter o mesmo tempo de vida. Isso é necessário porque C# o deve tomar todas as decisões sobre a segurança do tempo de vida com base nas informações disponíveis na assinatura do método e no tempo de vida dos valores no site de chamada. 

Quando há `ref` parâmetros que são `ref struct`, há o possibilidade que podem alternar em seu conteúdo. Portanto, no local de chamada, devemos garantir que todas essas trocas **potenciais** sejam compatíveis. Se o idioma não tiver aplicado isso, ele permitirá um código inadequado como o seguinte.

```csharp
void M1(ref Span<int> s1)
{
    Span<int> s2 = stackalloc int[1];
    Swap(ref s1, ref s2);
}

void Swap(ref Span<int> x, ref int Span<int> y)
{
    // This will effectively assign the stackalloc to the s1 parameter and allow it
    // to escape to the caller of M1
    ref x = ref y; 
}
```

A restrição no receptor é necessária porque, embora nenhum dos seus conteúdos seja ref-safe-to-escape, ele pode armazenar os valores fornecidos. Isso significa que, com tempos de vida incompatíveis, você poderia criar um buraco de segurança de tipo da seguinte maneira:

```csharp
ref struct S
{
    public Span<int> Span;

    public void Set(Span<int> span)
    {
        Span = span;
    }
}

void Broken(ref S s)
{
    Span<int> span = stackalloc int[1];

    // The result of a stackalloc is now stored in s.Span and escaped to the caller
    // of Broken
    s.Set(span); 
}
```

### <a name="struct-this-escape"></a>Estruturar este escape
Quando se trata de estender regras de segurança, o valor de `this` em um membro de instância é modelado como um parâmetro para o membro. Agora, para um `struct` o tipo de `this` é, na verdade, `ref S` onde em uma `class` é simplesmente `S` (para membros de um `class / struct` chamado S). 

Ainda `this` tem diferentes regras de escape do que outros parâmetros de `ref`. Especificamente, não é válido para ref-safe-to-escape enquanto outros parâmetros são:

```csharp
ref struct S
{ 
    int Field;

    // Illegal because this isn't safe to escape as ref
    ref int Get() => ref Field;

    // Legal
    ref int GetParam(ref int p) => ref p;
}
```

A razão para essa restrição realmente tem pouco a ver com `struct` invocação de membro. Há algumas regras que precisam ser configuradas em relação à invocação de membro em `struct` Membros em que o receptor é um Rvalue. Mas isso é muito acessível. 

A razão para essa restrição é, na verdade, sobre a invocação de interface. Especificamente, isso se resume se o exemplo a seguir deve ou não ser compilado;

```csharp
interface I1
{
    ref int Get();
}

ref int Use<T>(T p)
    where T : I1
{
    return ref p.Get();
}
```

Considere o caso em que `T` é instanciado como um `struct`. Se o parâmetro `this` for ref-safe-to-escape, o retorno de `p.Get` poderá apontar para a pilha (especificamente, poderia ser um campo dentro do tipo instanciado de `T`). Isso significa que o idioma não pôde permitir a compilação deste exemplo, pois ele poderia retornar um `ref` para um local de pilha. Por outro lado, se `this` não for ref-safe-to-escape, `p.Get` não poderá se referir à pilha e, portanto, será seguro retornar. 

É por isso que a saída de `this` em uma `struct` é, na verdade, todas as interfaces. Ele pode ser absolutamente feito para funcionar, mas tem uma compensação. O design eventualmente surgiu em favor de tornar as interfaces mais flexíveis. 

No entanto, é possível relaxar isso no futuro. 

## <a name="future-considerations"></a>Considerações futuras

### <a name="length-one-spant-over-ref-values"></a>Comprimento um\<T > sobre valores de referência
Embora não seja legal hoje, há casos em que criar um comprimento `Span<T>` instância em um valor seria benéfico:

```csharp
void RefExample()
{
    int x = ...;

    // Today creating a length one Span<int> requires a stackalloc and a new 
    // local
    Span<int> span1 = stackalloc [] { x };
    Use(span1);
    x = span1[0]; 

    // Simpler to just allow length one span
    var span2 = new Span<int>(ref x);
    Use(span2);
}
```

Esse recurso será mais atraente se compararmos as restrições em [buffers de tamanho fixo](https://github.com/dotnet/csharplang/blob/master/proposals/fixed-sized-buffers.md) , pois permitiria `Span<T>` instâncias de comprimento ainda maior. 

Se houver alguma necessidade de reduzir esse caminho, o idioma poderá acomodar isso, garantindo que instâncias de `Span<T>` estavam apenas para frente. Isso é que, no momento *,* eles eram seguros para o escopo no qual foram criados. Isso garante que o idioma nunca tenha que considerar um valor `ref` escapar de um método por meio de um `ref struct` retorno ou campo de `ref struct`. Isso provavelmente também exigiria outras alterações para reconhecer esses construtores como capturar um parâmetro de `ref` desse modo.
