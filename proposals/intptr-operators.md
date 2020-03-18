---
ms.openlocfilehash: 340a1dc5a2eac653458d7d29f74551e5fe28b6d5
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484555"
---
# <a name="operators-should-be-exposed-for-systemintptr-and-systemuintptr"></a>Os operadores devem ser expostos para `System.IntPtr` e `System.UIntPtr`

* [x] proposta
* [] Protótipo: não iniciado
* [] Implementação: não iniciada
* [] Especificação: não iniciada

## <a name="summary"></a>Resumo
[summary]: #summary

O CLR dá suporte a um conjunto de operadores para os tipos de `System.IntPtr` e `System.UIntPtr` (`native int`). Esses operadores podem ser vistos em `III.1.5` da especificação de Common Language Infrastructure (`ECMA-335`). No entanto, esses operadores não são C#suportados pelo.

O suporte a idiomas deve ser fornecido para o conjunto completo de operadores com suporte pelo `System.IntPtr` e `System.UIntPtr`. Esses operadores são: `Add`, `Divide`, `Multiply`, `Remainder`, `Subtract`, `Negate`, `Equals`, `Compare`, `And`, `Not`, `Or`, `XOr`, `ShiftLeft`, `ShiftRight`.

## <a name="motivation"></a>Motivação
[motivation]: #motivation

Hoje, os usuários podem facilmente C# escrever aplicativos direcionados a várias plataformas usando várias ferramentas e estruturas, como: `Xamarin`, `.NET Core`, `Mono`, etc...

Ao escrever código de plataforma cruzada, muitas vezes é necessário escrever código de interoperabilidade que interaja com uma plataforma de destino específica de uma maneira específica. Isso pode incluir a escrita de código de gráficos, a chamada de alguma API do sistema ou a interação com uma biblioteca nativa existente.

Esse código de interoperabilidade geralmente precisa lidar com identificadores, memória não gerenciada ou até apenas números inteiros específicos da plataforma.

O tempo de execução fornece suporte para isso definindo um conjunto de operadores que podem ser usados nos tipos primitivos `native int` (`System.IntPtr`) e `native unsigned int` (`System.UIntPtr`).

C#o nunca tem suporte para esses operadores e, portanto, os usuários precisam contornar o problema. Isso geralmente aumenta a complexidade do código e reduz a facilidade de manutenção do código.

Como tal, a linguagem deve começar a dar suporte a esses operadores para ajudar a avançar a linguagem para dar melhor suporte a esses requisitos.

## <a name="detailed-design"></a>Design detalhado
[design]: #detailed-design

O conjunto completo de operadores com suporte é definido em `III.1.5` da especificação de Common Language Infrastructure (`ECMA-335`). A especificação está disponível aqui: [https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf).

* Um resumo dos operadores é fornecido abaixo para sua conveniência.
* Os operadores não verificáveis definidos pela especificação da CLI não estão listados e não fazem parte desta proposta no momento (embora também possa valer a pena considerar isso).
* Fornecer uma palavra-chave (como `nint` e `nuint`) nem fornecer uma maneira de permitir que os literais sejam declarados para `System.IntPtr` e `System.UIntPtr` (como 0N) não fazem parte dessa proposta (embora possa valer a pena considerar isso também).

### <a name="unary-plus-operator"></a>Operador de adição unário

```csharp
System.IntPtr operator +(System.IntPtr)
```

```csharp
System.UIntPtr operator +(System.UIntPtr)
```

### <a name="unary-minus-operator"></a>Operador de subtração unário

```csharp
System.IntPtr operator -(System.IntPtr)
```

### <a name="bitwise-complement-operator"></a>Operador de complemento de bits

```csharp
System.IntPtr operator ~(System.IntPtr)
```

```csharp
System.UIntPtr operator ~(System.UIntPtr)
```

### <a name="cast-operators"></a>Operadores cast

```csharp
explicit operator sbyte(System.IntPtr)               // Truncate
explicit operator short(System.IntPtr)               // Truncate
explicit operator int(System.IntPtr)                 // Truncate
explicit operator long(System.IntPtr)                // Sign Extend

explicit operator byte(System.IntPtr)                // Truncate
explicit operator ushort(System.IntPtr)              // Truncate
explicit operator uint(System.IntPtr)                // Truncate
explicit operator ulong(System.IntPtr)               // Zero Extend

explicit operator System.IntPtr(int)                 // Sign Extend
explicit operator System.IntPtr(long)                // Truncate

explicit operator System.IntPtr(uint)                // Sign Extend
explicit operator System.IntPtr(ulong)               // Truncate

explicit operator System.IntPtr(System.IntPtr)
explicit operator System.IntPtr(System.UIntPtr)
```

```csharp
explicit operator sbyte(System.UIntPtr)               // Truncate
explicit operator short(System.UIntPtr)               // Truncate
explicit operator int(System.UIntPtr)                 // Truncate
explicit operator long(System.UIntPtr)                // Sign Extend

explicit operator byte(System.UIntPtr)                // Truncate
explicit operator ushort(System.UIntPtr)              // Truncate
explicit operator uint(System.UIntPtr)                // Truncate
explicit operator ulong(System.UIntPtr)               // Zero Extend

explicit operator System.UIntPtr(int)                 // Zero Extend
explicit operator System.UIntPtr(long)                // Truncate

explicit operator System.UIntPtr(uint)                // Zero Extend
explicit operator System.UIntPtr(ulong)               // Truncate

explicit operator System.UIntPtr(System.IntPtr)
explicit operator System.UIntPtr(System.UIntPtr)
```

### <a name="multiplication-operator"></a>Operador de multiplicação

```csharp
System.IntPtr operator *(int, System.IntPtr)
System.IntPtr operator *(System.IntPtr, int)
System.IntPtr operator *(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator *(uint, System.UIntPtr)
System.UIntPtr operator *(System.UIntPtr, uint)
System.UIntPtr operator *(System.UIntPtr, System.UIntPtr)
```

### <a name="division-operator"></a>Operador de divisão

```csharp
System.IntPtr operator /(int, System.IntPtr)
System.IntPtr operator /(System.IntPtr, int)
System.IntPtr operator /(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator /(uint, System.UIntPtr)
System.UIntPtr operator /(System.UIntPtr, uint)
System.UIntPtr operator /(System.UIntPtr, System.UIntPtr)
```

### <a name="remainder-operator"></a>Operador restante

```csharp
System.IntPtr operator %(int, System.IntPtr)
System.IntPtr operator %(System.IntPtr, int)
System.IntPtr operator %(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator %(uint, System.UIntPtr)
System.UIntPtr operator %(System.UIntPtr, uint)
System.UIntPtr operator %(System.UIntPtr, System.UIntPtr)
```

### <a name="addition-operator"></a>Operador de adição

```csharp
System.IntPtr operator +(int, System.IntPtr)
System.IntPtr operator +(System.IntPtr, int)
System.IntPtr operator +(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator +(uint, System.UIntPtr)
System.UIntPtr operator +(System.UIntPtr, uint)
System.UIntPtr operator +(System.UIntPtr, System.UIntPtr)
```

### <a name="subtraction-operator"></a>Operador de subtração

```csharp
System.IntPtr operator -(int, System.IntPtr)
System.IntPtr operator -(System.IntPtr, int)
System.IntPtr operator -(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator -(uint, System.UIntPtr)
System.UIntPtr operator -(System.UIntPtr, uint)
System.UIntPtr operator -(System.UIntPtr, System.UIntPtr)
```

### <a name="shift-operators"></a>Operadores Shift

```csharp
System.IntPtr operator <<(System.IntPtr, int)
System.IntPtr operator >>(System.IntPtr, int)
```

```csharp
System.UIntPtr operator <<(System.UIntPtr, int)
System.UIntPtr operator >>(System.UIntPtr, int)
```

### <a name="integer-comparison-operators"></a>Operadores de comparação de inteiros

```csharp
bool operator ==(int, System.IntPtr)
bool operator ==(System.IntPtr, int)
bool operator ==(System.IntPtr, System.IntPtr)

bool operator !=(int, System.IntPtr)
bool operator !=(System.IntPtr, int)
bool operator !=(System.IntPtr, System.IntPtr)

bool operator  <(int, System.IntPtr)
bool operator  <(System.IntPtr, int)
bool operator  <(System.IntPtr, System.IntPtr)

bool operator  >(int, System.IntPtr)
bool operator  >(System.IntPtr, int)
bool operator  >(System.IntPtr, System.IntPtr)

bool operator <=(int, System.IntPtr)
bool operator <=(System.IntPtr, int)
bool operator <=(System.IntPtr, System.IntPtr)

bool operator >=(int, System.IntPtr)
bool operator >=(System.IntPtr, int)
bool operator >=(System.IntPtr, System.IntPtr)
```

```csharp
bool operator ==(uint, System.UIntPtr)
bool operator ==(System.UIntPtr, uint)
bool operator ==(System.UIntPtr, System.UIntPtr)

bool operator !=(uint, System.UIntPtr)
bool operator !=(System.UIntPtr, uint)
bool operator !=(System.UIntPtr, System.UIntPtr)

bool operator  <(uint, System.UIntPtr)
bool operator  <(System.UIntPtr, uint)
bool operator  <(System.UIntPtr, System.UIntPtr)

bool operator  >(uint, System.UIntPtr)
bool operator  >(System.UIntPtr, uint)
bool operator  >(System.UIntPtr, System.UIntPtr)

bool operator <=(uint, System.UIntPtr)
bool operator <=(System.UIntPtr, uint)
bool operator <=(System.UIntPtr, System.UIntPtr)

bool operator >=(uint, System.UIntPtr)
bool operator >=(System.UIntPtr, uint)
bool operator >=(System.UIntPtr, System.UIntPtr)
```

### <a name="integer-logical-operators"></a>Operadores lógicos de inteiros

```csharp
System.IntPtr operator &(int, System.IntPtr)
System.IntPtr operator &(System.IntPtr, int)
System.IntPtr operator &(System.IntPtr, System.IntPtr)

System.IntPtr operator |(int, System.IntPtr)
System.IntPtr operator |(System.IntPtr, int)
System.IntPtr operator |(System.IntPtr, System.IntPtr)

System.IntPtr operator ^(int, System.IntPtr)
System.IntPtr operator ^(System.IntPtr, int)
System.IntPtr operator ^(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator &(uint, System.UIntPtr)
System.UIntPtr operator &(System.UIntPtr, uint)
System.UIntPtr operator &(System.UIntPtr, System.UIntPtr)

System.UIntPtr operator |(uint, System.UIntPtr)
System.UIntPtr operator |(System.UIntPtr, uint)
System.UIntPtr operator |(System.UIntPtr, System.UIntPtr)

System.UIntPtr operator ^(uint, System.UIntPtr)
System.UIntPtr operator ^(System.UIntPtr, uint)
System.UIntPtr operator ^(System.UIntPtr, System.UIntPtr)
```

## <a name="drawbacks"></a>Desvantagens
[drawbacks]: #drawbacks

O uso real desses operadores pode ser pequeno e limitado aos usuários finais que estão escrevendo bibliotecas de nível inferior ou código de interoperabilidade. A maioria dos usuários finais provavelmente estavam consumindo essas bibliotecas de nível inferior, o que teria os números inteiros, identificadores e código de interoperabilidade nativos, abstratos. Dessa forma, eles não precisariam dos operadores propriamente ditos.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

Faça com que a estrutura implemente os operadores necessários escrevendo-os diretamente no IL. Além disso, o tempo de execução pode fornecer suporte intrínseco para os operadores definidos pela estrutura, para otimizar melhor o desempenho final.

## <a name="unresolved-questions"></a>Perguntas não resolvidas
[unresolved]: #unresolved-questions

Quais partes do design ainda são TBD?

## <a name="design-meetings"></a>Criar reuniões

Vincule a anotações de design que afetam essa proposta e descreva em uma frase para cada alteração que elas levaram.