---
ms.openlocfilehash: 8bf3a18dc42e225e64bd3ccda2106aed89b421ed
ms.sourcegitcommit: 9aa177443b83116fe1be2ab28e2c7291947fe32d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/21/2020
ms.locfileid: "80108383"
---
# <a name="function-pointers"></a>Ponteiros de função

## <a name="summary"></a>Resumo

Esta proposta fornece construções de linguagem que expõem opcodes IL que não podem ser acessados no momento com eficiência C# ou, atualmente, em hoje: `ldftn` e `calli`. Esses opcodes de IL podem ser importantes em código de alto desempenho e os desenvolvedores precisam de uma maneira eficiente para acessá-los.

## <a name="motivation"></a>Motivação

As motivações e o plano de fundo desse recurso são descritos no seguinte problema (como é uma implementação potencial do recurso):

https://github.com/dotnet/csharplang/issues/191

Esta é uma proposta de design alternativa para [intrínsecos do compilador](https://github.com/dotnet/csharplang/blob/master/proposals/intrinsics.md)

## <a name="detailed-design"></a>Design detalhado

### <a name="function-pointers"></a>Ponteiros de função

O idioma permitirá a declaração de ponteiros de função usando a sintaxe `delegate*`. A sintaxe completa é descrita detalhadamente na próxima seção, mas ela se destina a se assemelhar à sintaxe usada pelas declarações de tipo `Func` e `Action`.

``` csharp
unsafe class Example {
    void Example(Action<int> a, delegate*<int, void> f) {
        a(42);
        f(42);
    }
}
```

Esses tipos são representados usando o tipo de ponteiro de função, conforme descrito em ECMA-335. Isso significa que a invocação de um `delegate*` usará `calli` em que a invocação de um `delegate` usará `callvirt` no método `Invoke`.
Sintaticamente, embora a invocação seja idêntica para ambas as construções.

A definição ECMA-335 dos ponteiros de método inclui a Convenção de chamada como parte da assinatura de tipo (seção 7,1).
A Convenção de chamada padrão será `managed`. Formulários alternativos podem ser especificados adicionando o modificador apropriado após a sintaxe de `delegate*`: `managed`, `cdecl`, `stdcall`, `thiscall`ou `unmanaged`. Exemplo:

``` csharp
// This method will be invoked using the cdecl calling convention
delegate* cdecl<int, int>;

// This method will be invoked using the stdcall calling convention
delegate* stdcall<int, int>;
```

As conversões entre os tipos de `delegate*` são feitas com base em sua assinatura, incluindo a Convenção de chamada.

``` csharp
unsafe class Example {
    void Conversions() {
        delegate*<int, int, int> p1 = ...;
        delegate* managed<int, int, int> p2 = ...;
        delegate* cdecl<int, int, int> p3 = ...;

        p1 = p2; // okay p1 and p2 have compatible signatures
        Console.WriteLine(p2 == p1); // True
        p2 = p3; // error: calling conventions are incompatible
    }
}
```

Um tipo de `delegate*` é um tipo de ponteiro que significa que ele tem todos os recursos e restrições de um tipo de ponteiro padrão:

- Válido somente em um contexto de `unsafe`.
- Os métodos que contêm um parâmetro `delegate*` ou tipo de retorno só podem ser chamados de um contexto `unsafe`.
- Não pode ser convertido em `object`.
- Não pode ser usado como um argumento genérico.
- Pode converter implicitamente `delegate*` em `void*`.
- Pode converter explicitamente de `void*` para `delegate*`.

Restrições:

- Atributos personalizados não podem ser aplicados a um `delegate*` ou a qualquer um de seus elementos.
- Um parâmetro `delegate*` não pode ser marcado como `params`
- Um tipo de `delegate*` tem todas as restrições de um tipo de ponteiro normal.

### <a name="function-pointer-syntax"></a>Sintaxe de ponteiro de função

A sintaxe de ponteiro de função completa é representada pela seguinte gramática:

```antlr
pointer_type
    : ...
    | funcptr_type
    ;

funcptr_type
    : 'delegate' '*' calling_convention? '<' (funcptr_parameter_modifier? type ',')* funcptr_return_modifier? return_type '>'
    ;

calling_convention
    : 'cdecl'
    | 'managed'
    | 'stdcall'
    | 'thiscall'
    | 'unmanaged'
    ;

funcptr_parameter_modifier
    : 'ref'
    | 'out'
    | 'in'
    ;

funcptr_return_modifier
    : 'ref'
    | 'ref readonly'
    ;
```

A Convenção de chamada `unmanaged` representa a Convenção de chamada padrão para código nativo na plataforma atual e é codificada como Winapi tenha.
Todas as `calling_convention`s são palavras-chave contextuais quando precedidas por um `delegate*`.

``` csharp
delegate int Func1(string s);
delegate Func1 Func2(Func1 f);

// Function pointer equivalent without calling convention
delegate*<string, int>;
delegate*<delegate*<string, int>, delegate*<string, int>>;

// Function pointer equivalent with calling convention
delegate* managed<string, int>;
delegate*<delegate* managed<string, int>, delegate*<string, int>>;
```

### <a name="function-pointer-conversions"></a>Conversões de ponteiro de função

Em um contexto sem segurança, o conjunto de conversões implícitas disponíveis (conversões implícitas) é estendido para incluir as seguintes conversões de ponteiro implícitas:
- [_Conversões existentes_](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#pointer-conversions)
- Em _funcptr\_tipo_ `F0` a outro _\_tipo_ `F1`, desde que todas as seguintes opções sejam verdadeiras:
    - `F0` e `F1` têm o mesmo número de parâmetros, e cada `D0n` de parâmetro no `F0` tem os mesmos `ref`, `out`ou `in` modificadores que o parâmetro correspondente `D1n` em `F1`.
    - Para cada parâmetro de valor (um parâmetro sem `ref`, `out`ou modificador de `in`), uma conversão de identidade, conversão de referência implícita ou conversão de ponteiro implícita existe do tipo de parâmetro em `F0` para o tipo de parâmetro correspondente no `F1`.
    - Para cada parâmetro `ref`, `out`ou `in`, o tipo de parâmetro em `F0` é o mesmo que o tipo de parâmetro correspondente em `F1`.
    - Se o tipo de retorno for por valor (sem `ref` ou `ref readonly`), uma identidade, referência implícita ou conversão implícita do ponteiro existirá do tipo de retorno de `F1` para o tipo de retorno de `F0`.
    - Se o tipo de retorno for por referência (`ref` ou `ref readonly`), o tipo de retorno e os modificadores de `ref` de `F1` serão os mesmos do tipo de retorno e os modificadores de `ref` de `F0`.
    - A Convenção de chamada de `F0` é igual à Convenção de chamada de `F1`.

### <a name="allow-address-of-to-target-methods"></a>Permitir endereço para métodos de destino

Os grupos de métodos agora serão permitidos como argumentos para uma expressão de endereço. O tipo de tal expressão será um `delegate*` que tem a assinatura equivalente do método de destino e uma Convenção de chamada gerenciada:

``` csharp
unsafe class Util {
    public static void Log() { }

    void Use() {
        delegate*<void> ptr1 = &Util.Log;

        // Error: type "delegate*<void>" not compatible with "delegate*<int>";
        delegate*<int> ptr2 = &Util.Log;

        // Okay. Conversion to void* is always allowed.
        void* v = &Util.Log;
   }
}
```

Em um contexto sem segurança, um método `M` é compatível com um tipo de ponteiro de função `F` se todas as seguintes opções forem verdadeiras:
- `M` e `F` têm o mesmo número de parâmetros, e cada parâmetro em `D` tem os mesmos `ref`, `out`ou modificadores de `in` como o parâmetro correspondente no `F`.
- Para cada parâmetro de valor (um parâmetro sem `ref`, `out`ou modificador de `in`), uma conversão de identidade, conversão de referência implícita ou conversão de ponteiro implícita existe do tipo de parâmetro em `M` para o tipo de parâmetro correspondente no `F`.
- Para cada parâmetro `ref`, `out`ou `in`, o tipo de parâmetro em `M` é o mesmo que o tipo de parâmetro correspondente em `F`.
- Se o tipo de retorno for por valor (sem `ref` ou `ref readonly`), uma identidade, referência implícita ou conversão implícita do ponteiro existirá do tipo de retorno de `F` para o tipo de retorno de `M`.
- Se o tipo de retorno for por referência (`ref` ou `ref readonly`), o tipo de retorno e os modificadores de `ref` de `F` serão os mesmos do tipo de retorno e os modificadores de `ref` de `M`.
- A Convenção de chamada de `M` é igual à Convenção de chamada de `F`.
- `M` é um método estático.

Em um contexto não seguro, existe uma conversão implícita de uma expressão de endereço cujo destino é um grupo de métodos `E` a um tipo de ponteiro de função compatível `F` se `E` contiver pelo menos um método aplicável em seu formato normal a uma lista de argumentos construída pelo uso dos tipos de parâmetro e modificadores de `F`, conforme descrito no seguinte.
- Um único método `M` é selecionado correspondente a uma invocação de método do formulário `E(A)` com as seguintes modificações:
   - A lista de argumentos `A` é uma lista de expressões, cada uma classificada como uma variável e com o tipo e o modificador (`ref`, `out`ou `in`) do _parâmetro\_formal correspondente\_lista_ de `D`.
   - Os métodos candidatos são apenas os métodos que são aplicáveis em seu formato normal, não aqueles aplicáveis em sua forma expandida.
   - Os métodos candidatos são apenas os métodos que são estáticos.
- Se o algoritmo das invocações de método produzir um erro, ocorrerá um erro de tempo de compilação. Caso contrário, o algoritmo produz um único método melhor `M` ter o mesmo número de parâmetros que `F` e a conversão é considerada como existe.
- O método selecionado `M` deve ser compatível (conforme definido acima) com o tipo de ponteiro de função `F`. Caso contrário, ocorrerá um erro de tempo de compilação.
- O resultado da conversão é um ponteiro de função do tipo `F`.

Há uma conversão implícita de uma expressão de endereço cujo destino é um grupo de métodos `E` para `void*` se houver apenas um método estático `M` em `E`.
Se houver um método estático, o melhor método de `E` será `M`.
Caso contrário, ocorrerá um erro de tempo de compilação.

Isso significa que os desenvolvedores podem depender das regras de resolução de sobrecarga para trabalhar em conjunto com o operador address-of:

``` csharp
unsafe class Util {
    public static void Log() { }
    public static void Log(string p1) { }
    public static void Log(int i) { };

    void Use() {
        delegate*<void> a1 = &Log; // Log()
        delegate*<int, void> a2 = &Log; // Log(int i)

        // Error: ambiguous conversion from method group Log to "void*"
        void* v = &Log;
    }
```

O operador address-of será implementado usando a instrução `ldftn`.

Restrições deste recurso:

- Aplica-se somente a métodos marcados como `static`.
- Funções locais não`static` não podem ser usadas em `&`. Os detalhes de implementação desses métodos são deliberadamente não especificados pelo idioma. Isso inclui se eles são estáticos versus instância ou exatamente em qual assinatura eles são emitidos.

### <a name="better-function-member"></a>Melhor membro da função

A melhor especificação de membro de função será alterada para incluir a seguinte linha:

> Uma `delegate*` é mais específica do que `void*`

Isso significa que é possível sobrecarregar `void*` e uma `delegate*` e ainda facilmente usar o operador address-of.

## <a name="open-issues"></a>Problemas em aberto

### <a name="nativecallableattribute"></a>NativeCallableAttribute

Esse é um atributo usado pelo CLR para evitar o prólogo gerenciado para nativo ao invocar. Os métodos marcados por este atributo só podem ser chamados de código nativo, não gerenciado (não é possível chamar métodos, criar um delegado, etc...). O atributo não é especial para mscorlib; o tempo de execução tratará qualquer atributo com esse nome com a mesma semântica.

É possível que o tempo de execução e a linguagem funcionem juntos para dar suporte total a isso. O idioma pode optar por tratar os membros de `static` de endereço com um atributo `NativeCallable` como um `delegate*` com a Convenção de chamada especificada.

``` csharp
unsafe class NativeCallableExample {
    [NativeCallable(CallingConvention.CDecl)]
    static void CloseHandle(IntPtr p) => Marshal.FreeHGlobal(p);

    void Use() {
        delegate*<IntPtr, void> p1 = &CloseHandle; // Error: Invalid calling convention

        delegate* cdecl<IntPtr, void> p2 = &CloseHandle; // Okay
    }
}

```

Além disso, a linguagem provavelmente também desejaria:

- Sinalizar todas as chamadas gerenciadas para um método marcado com `NativeCallable` como um erro. Considerando que a função não pode ser invocada do código gerenciado, o compilador deve impedir que os desenvolvedores tentem essa invocação.
- Impeça conversões de grupo de métodos para `delegate` quando o método é marcado com `NativeCallable`.

No entanto, isso não é necessário para dar suporte a `NativeCallable`. O compilador pode dar suporte ao atributo `NativeCallable` como está usando a sintaxe existente. O programa simplesmente precisaria ser convertido em `void*` antes da conversão para a assinatura de `delegate*` correta. Isso não seria pior do que o suporte atualmente.

``` csharp
void* v = &CloseHandle;
delegate* cdecl<IntPtr, bool> f1 = (delegate* cdecl<IntPtr, bool>)v;
```

### <a name="extensible-set-of-unmanaged-calling-conventions"></a>Conjunto extensível de convenções de chamada não gerenciadas

O conjunto de convenções de chamada não gerenciadas com suporte das codificações ECMA-335 atuais está desatualizado. Vimos solicitações para adicionar suporte para mais convenções de chamada não gerenciadas, por exemplo:

- https://github.com/dotnet/coreclr/issues/12120 [vectorcall](https://docs.microsoft.com/cpp/cpp/vectorcall)
- StdCall com este https://github.com/dotnet/coreclr/pull/23974#issuecomment-482991750 explícito

O design desse recurso deve permitir a extensão do conjunto de convenções de chamada não gerenciadas conforme necessário no futuro. Os problemas incluem espaço limitado para codificar convenções de chamada (12 de 16 valores são obtidos em `IMAGE_CEE_CS_CALLCONV_MASK`) e número de casas que precisam ser tocadas para adicionar uma nova Convenção de chamada. Uma solução potencial é apresentar uma nova codificação que representa a Convenção de chamada usando [`System.Runtime.InteropServices.CallingConvention`](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.interopservices.callingconvention) enumeração.

Para referência, https://github.com/llvm/llvm-project/blob/master/llvm/include/llvm/IR/CallingConv.h tem a lista de convenções de chamada com suporte do LLVM. Embora seja improvável que o .NET precise dar suporte a todos eles, ele demonstra que o espaço das convenções de chamada é muito avançado.

## <a name="considerations"></a>Considerações

### <a name="allow-instance-methods"></a>Permitir métodos de instância

A proposta pode ser estendida para dar suporte a métodos de instância aproveitando a Convenção de chamada da CLI `EXPLICITTHIS` C# (chamada `instance` no código). Essa forma de ponteiros de função da CLI coloca o parâmetro `this` como um primeiro parâmetro explícito da sintaxe do ponteiro de função.

``` csharp
unsafe class Instance {
    void Use() {
        delegate* instance<Instance, string> f = &ToString;
        f(this);
    }
}
```

Isso é bom, mas adiciona certa complicação à proposta. Particularmente, como os ponteiros de função que diferem pela Convenção de chamada `instance` e `managed` seriam incompatíveis, embora ambos os casos sejam usados para invocar métodos gerenciados com a mesma C# assinatura. Além disso, em cada caso, considerado como seria valioso ter uma solução simples: Use um `static` função local.

``` csharp
unsafe class Instance {
    void Use() {
        static string toString(Instance i) = i.ToString();
        delgate*<Instance, string> f = &toString;
        f(this);
    }
}
```

### <a name="dont-require-unsafe-at-declaration"></a>Não requer segurança na declaração

Em vez de exigir `unsafe` em cada uso de um `delegate*`, só é necessário no ponto em que um grupo de métodos é convertido em um `delegate*`. É aí que os problemas de segurança principal entram em cena (sabendo que o assembly recipiente não pode ser descarregado enquanto o valor estiver ativo). Exigir `unsafe` em outros locais pode ser visto como excessivo.

É assim que o design foi originalmente planejado. Mas as regras de linguagem resultantes pareciam muito estranhas. É impossível ocultar o fato de que esse é um valor de ponteiro e que ele continua exibindo até mesmo sem a palavra-chave `unsafe`. Por exemplo, a conversão em `object` não pode ser permitida, não pode ser um membro de um `class`, etc... O C# design é exigir `unsafe` para todos os usos de ponteiro e, portanto, esse design é o seguinte.

Os desenvolvedores ainda poderão apresentar um wrapper _seguro_ sobre `delegate*` valores da mesma maneira que fazem para tipos de ponteiros normais hoje em dia. Considere:

``` csharp
unsafe struct Action {
    delegate*<void> _ptr;

    Action(delegate*<void> ptr) => _ptr = ptr;
    public void Invoke() => _ptr();
}
```

### <a name="using-delegates"></a>Usando delegados

Em vez de usar um novo elemento de sintaxe, `delegate*`, basta usar tipos de `delegate` existentes com um `*` seguindo o tipo:

``` csharp
Func<object, object, bool>* ptr = &object.ReferenceEquals;
```

A manipulação de Convenção de chamada pode ser feita anotando os tipos de `delegate` com um atributo que especifica um valor de `CallingConvention`. A falta de um atributo significaria a Convenção de chamada gerenciada.

Codificar isso no IL é problemático. O valor subjacente precisa ser representado como um ponteiro, ainda que ele também deva:

1. Ter um tipo exclusivo para permitir sobrecargas com tipos diferentes de ponteiro de função.
1. Ser equivalente para fins de OHI em limites de assembly.

O último ponto é particularmente problemático. Isso significa que cada assembly que usa `Func<int>*` deve codificar um tipo equivalente em metadados, mesmo que `Func<int>*` seja definido em um assembly, embora não controle.
Além disso, qualquer outro tipo que é definido com o nome `System.Func<T>` em um assembly que não seja mscorlib deve ser diferente da versão definida em mscorlib.

Uma opção que foi explorada foi emitir um ponteiro como `mod_req(Func<int>) void*`. Embora isso não funcione como um `mod_req` não pode se associar a um `TypeSpec` e, portanto, não pode direcionar instanciações genéricas.

### <a name="named-function-pointers"></a>Ponteiros de função nomeados

A sintaxe do ponteiro de função pode ser complicada, particularmente em casos complexos, como ponteiros de funções aninhadas. Em vez de os desenvolvedores digitarem a assinatura sempre que o idioma puder permitir declarações nomeadas de ponteiros de função, como é feito com `delegate`.

``` csharp
func* void Action();

unsafe class NamedExample {
    void M(Action a) {
        a();
    }
}
```

Parte do problema aqui é que o primitivo de CLI subjacente não tem nomes, portanto, isso seria C# puramente uma invenção e requer um pouco de trabalho de metadados para habilitar. Isso é factível, mas é um grande respeito do trabalho. Basicamente, é C# necessário ter um complemento para a tabela de tipo def puramente para esses nomes.

Além disso, quando os argumentos dos ponteiros de função nomeados foram examinados, eles poderiam ser aplicados igualmente bem a vários outros cenários. Por exemplo, seria conveniente declarar tuplas nomeadas para reduzir a necessidade de digitar a assinatura completa em todos os casos.

``` csharp
(int x, int y) Point;

class NamedTupleExample {
    void M(Point p) {
        Console.WriteLine(p.x);
    }
}
```

Após a discussão, decidimos não permitir a declaração nomeada de tipos de `delegate*`. Se encontrarmos uma necessidade significativa para isso com base nos comentários de uso do cliente, investigaremos uma solução de nomenclatura que funciona para ponteiros de função, tuplas, genéricos, etc... Isso provavelmente será semelhante em forma de outras sugestões, como suporte completo a `typedef` no idioma.

## <a name="future-considerations"></a>Considerações futuras

### <a name="static-local-functions"></a>funções locais estáticas

Isso se refere à [proposta](https://github.com/dotnet/csharplang/issues/1565) para permitir o modificador de `static` em funções locais. Essa função seria garantida para ser emitida como `static` e com a assinatura exata especificada no código-fonte. Essa função deve ser um argumento válido para `&`, pois ela contém nenhum dos problemas que as funções locais têm hoje

### <a name="static-delegates"></a>delegados estáticos

Isso se refere à [proposta](https://github.com/dotnet/csharplang/issues/302) para permitir a declaração de tipos de `delegate` que só podem se referir a membros `static`. A vantagem é que essas instâncias de `delegate` podem ser de alocação gratuita e melhor em cenários de desempenho.

Se o recurso de ponteiro de função for implementado, a proposta de `static delegate` provavelmente será fechada. A vantagem proposta desse recurso é a natureza livre de alocação. No entanto, foram encontradas investigações recentes que não podem ser obtidas devido ao descarregamento do assembly. Deve haver um identificador forte do `static delegate` ao método ao qual se refere para impedir que o assembly seja descarregado de dentro dele.

Para manter cada instância de `static delegate` seria necessário alocar um novo identificador que executa um contador para as metas da proposta. Havia alguns designs em que a alocação poderia ser amortizada para uma única alocação por site de chamada, mas isso era um pouco complexo e não parece que vale a pena compensar.

Isso significa que os desenvolvedores têm, essencialmente, decidir entre as seguintes compensações:

1. Segurança diante do descarregamento do assembly: isso requer alocações e, portanto, `delegate` já é uma opção suficiente.
1. Não há segurança em face de descarregamento de assembly: Use um `delegate*`. Isso pode ser encapsulado em um `struct` para permitir o uso fora de um contexto de `unsafe` no restante do código.
