---
ms.openlocfilehash: ac4c8760e3b6a0934f01ae634f666af60aa1c0fe
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484562"
---
# <a name="compiler-intrinsics"></a>Intrínsecos do compilador

## <a name="summary"></a>Resumo

Esta proposta fornece construções de linguagem que expõem opcodes IL de nível baixo que atualmente não podem ser acessados com eficiência, ou em todos: `ldftn`, `ldvirtftn`, `ldtoken` e `calli`. Esses opcodes de baixo nível podem ser importantes em código de alto desempenho e os desenvolvedores precisam de uma maneira eficiente para acessá-los.

## <a name="motivation"></a>Motivação

As motivações e o plano de fundo desse recurso são descritos no seguinte problema (como é uma implementação potencial do recurso): 

https://github.com/dotnet/csharplang/issues/191

Essa proposta de design alternativa vem depois de revisar uma implementação de protótipo da proposta original por @msjabby, bem como o uso em uma base de código significativa. Esse design foi feito com uma entrada significativa do @mjsabby, @tmat e @jkotas.

## <a name="detailed-design"></a>Design detalhado 

### <a name="allow-address-of-to-target-methods"></a>Permitir endereço de métodos de destino

Os grupos de métodos agora serão permitidos como argumentos para uma expressão de endereço. O tipo dessa expressão será `void*`. 

``` csharp
class Util { 
    public static void Log() { } 
}

// ldftn Util.Log
void* ptr = &Util.Log; 
```

Considerando que não há nenhuma conversão de delegado aqui, o único mecanismo para filtrar Membros no grupo de métodos é por acesso estático/de instância. Se não for possível distinguir os membros, ocorrerá um erro de tempo de compilação.

``` csharp
class Util { 
    public void Log() { } 
    public void Log(string p1) { } 
    public static void Log(int i) { };
}

unsafe {
    // Error: Method group Log has more than one applicable candidate.
    void* ptr1 = &Log; 

    // Okay: only one static member to consider here.
    void* ptr2 = &Util.Log;
}
```

A expressão AddressOf neste contexto será implementada da seguinte maneira:

- Ldftn: quando o método é não virtual.
- ldvirtftn: quando o método é virtual.

Restrições deste recurso:

- Os métodos de instância só podem ser especificados ao usar uma expressão de invocação em um valor
- As funções locais não podem ser usadas em `&`. Os detalhes de implementação desses métodos são deliberadamente não especificados pelo idioma. Isso inclui se eles são estáticos versus instância ou exatamente em qual assinatura eles são emitidos.

### <a name="handleof"></a>handleof

O `handleof` palavra-chave contextual converterá um campo, membro ou tipo em seu tipo equivalente de `RuntimeHandle` usando a instrução `ldtoken`. O tipo exato da expressão dependerá do tipo de nome em `handleof`:

- campo: `RuntimeFieldHandle`
- Tipo: `RuntimeTypeHandle`
- método: `RuntimeMethodHandle`

Os argumentos para `handleof` são idênticos aos `nameof`. Ele deve ser um nome simples, um nome qualificado, um acesso de membro, um acesso de base com um membro especificado ou esse acesso com um membro especificado. A expressão de argumento identifica uma definição de código, mas ela nunca é avaliada.

A expressão de `handleof` é avaliada em tempo de execução e tem um tipo de retorno de `RuntimeHandle`. Isso pode ser executado em código seguro, bem como não seguro. 

``` 
RuntimeHandle stringHandle = handleof(string);
```

Restrições deste recurso:

- As propriedades não podem ser usadas em uma expressão `handleof`.
- A expressão de `handleof` não pode ser usada quando há um nome de `handleof` existente no escopo. Por exemplo, um tipo, namespace, etc...

### <a name="calli"></a>Calli

O compilador adicionará suporte para um novo tipo de `extern` função que se traduz com eficiência em uma instrução `.calli`. O atributo externo será marcado com um atributo da seguinte forma:

``` csharp
[AttributeUsage(AttributeTargets.Method)]
public sealed class CallIndirectAttribute : Attribute
{
    public CallingConvention CallingConvention { get; }
    public CallIndirectAttribute(CallingConvention callingConvention)
    {
        CallingConvention = callingConvention;
    }
}
```

Isso permite que os desenvolvedores definam métodos no seguinte formato:

``` csharp
[CallIndirect(CallingConvention.Cdecl)]
static extern int MapValue(string s, void *ptr);

unsafe {
    var i = MapValue("42", &int.Parse);
    Console.WriteLine(i);
}
```

Restrições no método que tem o atributo `CallIndirect` aplicado:

- Não é possível ter um atributo `DllImport`.
- Não pode ser genérico.

## <a name="open-issues"></a>Problemas em aberto

### <a name="callingconvention"></a>CallingConvention

O `CallIndirectAttribute` como projetado usa o `CallingConvention` enum que não tem uma entrada para convenções de chamada gerenciadas. A enumeração precisa ser estendida para incluir essa Convenção de chamada ou o atributo precisa usar uma abordagem diferente.

## <a name="considerations"></a>Considerações

### <a name="disambiguating-method-groups"></a>Grupos de métodos de desambiguidade

Havia algumas discussões sobre os recursos que tornaria mais fácil a ambiguidade dos grupos de métodos passados para uma expressão de endereço. Por exemplo, potencialmente adicionando elementos de assinatura à sintaxe:

``` csharp
class Util {
    public static void Log() { ... }
    public static void Log(string) { ... }
}

unsafe {
    // Error: ambiguous Log
    void *ptr1 = &Util.Log;

    // Use Util.Log();
    void *ptr2 = &Util.Log();
}
```

Isso foi rejeitado porque um caso convincente não pôde ser feito nem uma sintaxe simples pode ser configurada aqui. Além disso, há um trabalho bastante simples: defina um outro método que não seja ambíguo e use C# o código para chamar a função desejada. 

``` csharp
class Workaround {
    public static void LocalLog() => Util.Log();
}
unsafe { 
    void* ptr = &Workaround.LocalLog;
}
```

Isso se torna ainda mais simples se `static` funções locais entram no idioma. Em seguida, a solução alternativa pode ser definida na mesma função que usou a operação de endereço ambíguo:

``` csharp
unsafe { 
    static void LocalLog() => Util.Log();
    void* ptr = &Workaround.LocalLog;
}
```

### <a name="loadtypetokenint32"></a>LoadTypeTokenInt32

A proposta original permitida para que os tokens de metadados sejam carregados como `int` valores em tempo de compilação. Basicamente, tem `tokenof` que tem os mesmos argumentos que `handleof`, mas é avaliado em tempo de compilação para uma constante `int`. 

Isso foi rejeitado, pois causa um problema significativo para regravações de IL (das quais o .NET tem muitos). Esses regravadores geralmente manipulam as tabelas de metadados de uma maneira que pode invalidar esses valores. Não há nenhuma maneira razoável para que esses regravadores atualizem esses valores quando são armazenados como valores `int` simples.

A ideia subjacente de ter um identificador opaco para entradas de metadados continuará a ser explorada pela equipe de tempo de execução. 

## <a name="future-considerations"></a>Considerações futuras

### <a name="static-local-functions"></a>funções locais estáticas

Isso se refere à [proposta](https://github.com/dotnet/csharplang/issues/1565) para permitir o modificador de `static` em funções locais. Essa função seria garantida para ser emitida como `static` e com a assinatura exata especificada no código-fonte. Essa função deve ser um argumento válido para `&`, pois ela contém nenhum dos problemas que as funções locais têm hoje.

### <a name="nativecallableattribute"></a>NativeCallableAttribute

O CLR tem um recurso que permite que métodos gerenciados sejam emitidos de forma que eles sejam diretamente chamáveis de código nativo. Isso é feito adicionando o `NativeCallableAttribute` aos métodos. Esse método só pode ser chamado de código nativo e, portanto, deve conter apenas tipos blittable na assinatura. A chamada a partir de código gerenciado resulta em um erro de tempo de execução. 

Esse recurso seria bem padronizado com essa proposta, como seria permitido:

- Passar uma função definida em código gerenciado para código nativo como um ponteiro de função (por endereço) sem sobrecarga no código gerenciado ou nativo. 
- O tempo de execução pode introduzir o uso de erros de site para essas funções no código gerenciado para impedir que eles sejam invocados no momento da compilação.




