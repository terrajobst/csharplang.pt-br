---
ms.openlocfilehash: 63dfdfee9ea6c16e162f483aa1298feed297daef
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484695"
---
# <a name="conditional-ref-expressions"></a>Expressões de referência condicional

O padrão de associação de uma variável ref a uma ou outra expressão condicionalmente não é expresso no C#momento no.

A solução alternativa típica é introduzir um método como:

```csharp
ref T Choice(bool condition, ref T consequence, ref T alternative)
{
    if (condition)
    {
         return ref consequence;
    }
    else
    {
         return ref alternative;
    }
}
```

Observe que isso não é uma substituição exata de um ternário, já que todos os argumentos devem ser avaliados no site de chamada.

O seguinte não funcionará conforme o esperado:

```csharp
       // will crash with NRE because 'arr[0]' will be executed unconditionally
      ref var r = ref Choice(arr != null, ref arr[0], ref otherArr[0]);
```

A sintaxe proposta teria a seguinte aparência:

```csharp
     <condition> ? ref <consequence> : ref <alternative>;
```

A tentativa acima com "Choice" pode ser gravada _corretamente_ usando ref ternário como:

```csharp
     ref var r = ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

A diferença da escolha é que a consequência e as expressões alternativas são acessadas de maneira _realmente_ condicional, portanto, não vemos uma falha se ```arr == null```

A referência ternário é apenas um ternário em que tanto a alternativa quanto a consequência são refs. Naturalmente, isso exigirá que os operandos/conseqüências alternativos sejam LValue. Ele também exigirá que a consequência e a alternativa tenham tipos que são conversíveis de identidade entre si.

O tipo da expressão será computado de forma semelhante à do ternário regular. I.E. caso a consequência e a alternativa tenham identidade conversível, mas tipos diferentes, as regras de mesclagem de tipos existentes serão aplicadas.

A segurança a ser retornada será presumida de forma conservadora dos operandos condicionais. Se não for seguro retornar, não será seguro retornar o item inteiro.

Ref ternário é um LValue e, como tal, pode ser passado/atribuído/retornado por referência;

```csharp
     // pass by reference
     foo(ref (arr != null ? ref arr[0]: ref otherArr[0]));

     // return by reference
     return ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

Sendo um LValue, ele também pode ser atribuído a. 

```csharp
    // assign to
    (arr != null ? ref arr[0]: ref otherArr[0]) = 1;
```

Ref ternário também pode ser usado em um contexto regular (não ref). Embora isso não seja comum, já que você poderia usar também um ternário regular.

```csharp
     int x = (arr != null ? ref arr[0]: ref otherArr[0]);
```


___

Notas de implementação: 

A complexidade da implementação parece ser o tamanho de uma correção de bug moderada para grande. -I. E não é muito caro.
Não acho que precisamos de nenhuma alteração na sintaxe ou análise.
Não há nenhum efeito nos metadados ou na interoperabilidade. O recurso é baseado em expressão completamente.
Nenhum efeito na depuração/PDB pode ser
