---
ms.openlocfilehash: 91afbc3e3412049cd183c36c8035f1862c520413
ms.sourcegitcommit: da1180f7eacdd5067b32d291a76e6764159e00fe
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2019
ms.locfileid: "79485052"
---
# <a name="pattern-based-using-and-using-declarations"></a>"baseado em padrões usando" e "usando declarações"

## <a name="summary"></a>Resumo

A linguagem adicionará dois novos recursos em relação à instrução de `using` para tornar o gerenciamento de recursos mais simples: `using` deve reconhecer um padrão descartável além de `IDisposable` e adicionar uma declaração de `using` ao idioma.

## <a name="motivation"></a>Motivação

A instrução `using` é uma ferramenta eficaz para o gerenciamento de recursos hoje, mas requer um pouco de cerimônia. Os métodos que têm um número de recursos para gerenciar podem ser sobrecargados sintaticamente com uma série de instruções de `using`. Essa sobrecarga de sintaxe é suficiente para que a maioria das diretrizes de estilo de codificação tenha explicitamente uma exceção em chaves para esse cenário. 

A declaração de `using` remove grande parte da cerimônia aqui e C# se obtém em comparação com outras linguagens que incluem blocos de gerenciamento de recursos. Além disso, a `using` baseada em padrões permite que os desenvolvedores expandam o conjunto de tipos que podem participar aqui. Em muitos casos, remover a necessidade de criar tipos de wrapper que existem apenas para permitir um uso de valores em uma instrução `using`. 

Juntos, esses recursos permitem que os desenvolvedores simplifiquem e expandam os cenários em que `using` podem ser aplicados.

## <a name="detailed-design"></a>Design detalhado 

### <a name="using-declaration"></a>usando declaração

O idioma permitirá que `using` sejam adicionados a uma declaração de variável local. Tal declaração terá o mesmo efeito que declarar a variável em uma instrução `using` no mesmo local.

```csharp
if (...) 
{ 
   using FileStream f = new FileStream(@"C:\users\jaredpar\using.md");
   // statements
}

// Equivalent to 
if (...) 
{ 
   using (FileStream f = new FileStream(@"C:\users\jaredpar\using.md")) 
   {
    // statements
   }
}
```

O tempo de vida de um `using` local será estendido para o final do escopo no qual ele é declarado. O `using` locais será Descartado na ordem inversa na qual eles são declarados. 

```csharp
{ 
    using var f1 = new FileStream("...");
    using var f2 = new FileStream("..."), f3 = new FileStream("...");
    ...
    // Dispose f3
    // Dispose f2 
    // Dispose f1
}
```

Não há restrições em relação à `goto`ou a qualquer outra construção de fluxo de controle diante de uma declaração de `using`. Em vez disso, o código funciona exatamente como seria para a instrução `using` equivalente:

```csharp
{
    using var f1 = new FileStream("...");
  target:
    using var f2 = new FileStream("...");
    if (someCondition) 
    {
        // Causes f2 to be disposed but has no effect on f1
        goto target;
    }
}
```

Um local declarado em uma `using` declaração local será implicitamente somente leitura. Isso corresponde ao comportamento de locais declarados em uma instrução `using`. 

A gramática de idioma para declarações de `using` será a seguinte:

```antlr
local-using-declaration:
  using type using-declarators

using-declarators:
  using-declarator
  using-declarators , using-declarator
  
using-declarator:
  identifier = expression
```

Restrições em relação à declaração de `using`:

- Pode não aparecer diretamente dentro de um rótulo de `case`, mas deve estar dentro de um bloco dentro do rótulo de `case`.
- Pode não aparecer como parte de uma declaração de variável `out`. 
- Deve ter um inicializador para cada Declarador.
- O tipo local deve ser implicitamente conversível para `IDisposable` ou para atender ao padrão de `using`.

### <a name="pattern-based-using"></a>baseado em padrões usando

A linguagem adicionará a noção de um padrão descartável: que é um tipo que tem um método de instância `Dispose` acessível. Os tipos que se ajustam ao padrão descartável podem participar de uma declaração ou instrução `using` sem precisar implementar `IDisposable`. 

```csharp
class Resource
{ 
    public void Dispose() { ... }
}

using (var r = new Resource())
{
    // statements
}
```

Isso permitirá que os desenvolvedores aproveitem `using` em vários cenários novos:

- `ref struct`: esses tipos não podem implementar interfaces hoje e, portanto, não podem participar de instruções `using`.
- Os métodos de extensão permitirão que os desenvolvedores ampliem tipos em outros assemblies para participarem de `using` instruções.

Na situação em que um tipo pode ser convertido implicitamente em `IDisposable` e também se adapta ao padrão descartável, `IDisposable` será preferível. Embora isso assuma a abordagem oposta de `foreach` (o padrão é preferencial na interface), é necessário para compatibilidade com versões anteriores.

As mesmas restrições de uma instrução de `using` tradicional também se aplicam aqui: variáveis locais declaradas no `using` são somente leitura, um valor de `null` não fará com que uma exceção seja gerada, etc... A geração de código só será diferente, pois não haverá uma conversão para `IDisposable` antes de chamar Dispose:

```csharp
{
      Resource r = new Resource();
      try {
            // statements
      }
      finally {
            if (resource != null) resource.Dispose();
      }
}
```

Para ajustar o padrão descartável, o método `Dispose` deve ser acessível, sem parâmetros e ter um tipo de retorno `void`. Não há nenhuma outra restrição. Isso significa explicitamente que os métodos de extensão podem ser usados aqui.

## <a name="considerations"></a>Considerações

### <a name="case-labels-without-blocks"></a>rótulos de caso sem blocos

Um `using declaration` é ilegal diretamente dentro de um rótulo de `case` devido a complicações em relação ao seu tempo de vida real. Uma solução potencial é simplesmente dar a ela o mesmo tempo de vida como um `out var` no mesmo local. Foi considerada a complexidade extra para a implementação do recurso e a facilidade do trabalho (basta adicionar um bloco ao rótulo de `case`) não justificava a execução dessa rota.

## <a name="future-expansions"></a>Expansões futuras

### <a name="fixed-locals"></a>locais fixos

Uma instrução `fixed` tem todas as propriedades de instruções `using` que motivavam a capacidade de ter `using` locais. A consideração deve ser fornecida para estender esse recurso para `fixed` locais também. As regras de tempo de vida e ordenação devem se aplicar igualmente bem ao `using` e `fixed` aqui.
