---
ms.openlocfilehash: d2064ec1637e50962262c9380281abd5e1711402
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484576"
---
# <a name="declaration-expressions"></a>Expressões de declaração

Suporte a atribuições de declaração como expressões.

## <a name="motivation"></a>Motivação
[motivation]: #motivation

Permita a inicialização no ponto de declaração em mais casos, simplificando o código e permitindo que `var` seja usado.

```csharp
SpecialType ReferenceType =>
    (var st = _type.SpecialType).IsValueType() ? SpecialType.None : st;
```

Permitir declarações para argumentos de `ref`, semelhante a `out var`.

```csharp
Convert(source, destination, ref List<Diagnostic> diagnostics = null);
```

## <a name="detailed-design"></a>Design detalhado
[design]: #detailed-design

As expressões são estendidas para incluir a atribuição de declaração. A precedência é igual à atribuição.

```antlr
expression
    : non_assignment_expression
    | assignment
    | declaration_assignment_expression // new
    ;
declaration_assignment_expression // new
    : declaration_expression '=' local_variable_initializer
    ;
declaration_expression // C# 7.0
    | type variable_designation
    ;
```

A atribuição de declaração é de um único local.

O tipo de uma expressão de atribuição de declaração é o tipo da declaração.
Se o tipo for `var`, o tipo inferido será o tipo da expressão de inicialização. 

A expressão de atribuição de declaração pode ser um valor l, para `ref` valores de argumento em particular.

Se a expressão de atribuição de declaração declarar um tipo de valor e a expressão for um valor de r, o valor da expressão será uma cópia.

A expressão de atribuição de declaração pode declarar um `ref` local.
Há uma ambiguidade quando `ref` é usada para uma expressão de declaração em um argumento `ref`.
O inicializador da variável local determina se a declaração é um `ref` local.

```csharp
F(ref int x = IntFunc());    // int x;
F(ref int y = RefIntFunc()); // ref int y;
```

O escopo de locais declarados em expressões de atribuição de declaração é o mesmo escopo das expressões de declaração correspondentes do C # 7.0.

É um erro de tempo de compilação para fazer referência a um local no texto que precede a expressão de declaração.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives
Nenhuma alteração. Esse recurso é apenas uma abreviação sintática depois de tudo.

Expressões de sequência mais gerais: consulte [#377](https://github.com/dotnet/csharplang/issues/377).

Para permitir o uso de `var` em mais casos, permita uma declaração e atribuição separada de `var` locais e inferir o tipo de atribuições de todos os caminhos de código.

## <a name="see-also"></a>Consulte também
[see-also]: #see-also
Consulte expressão de declaração básica no [#595](https://github.com/dotnet/csharplang/issues/595).

Consulte declaração de desconstrução no recurso de [desconstrução](https://github.com/dotnet/roslyn/blob/master/docs/features/deconstruction.md) .
