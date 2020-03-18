---
ms.openlocfilehash: 392d932459ff0a4cb0d6d32c1606f73f9b913c68
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484450"
---
# <a name="covariant-return-types"></a>tipos de retorno covariantes

* [x] proposta
* [] Protótipo: não iniciado
* [] Implementação: não iniciada
* [] Especificação: não iniciada

## <a name="summary"></a>Resumo
[summary]: #summary

Suporte a _tipos de retorno covariantes_. Especificamente, permitir que um método de substituição tenha um tipo de referência mais derivado do que o método que ele substitui.

## <a name="motivation"></a>Motivação
[motivation]: #motivation

É um padrão comum no código que nomes de métodos diferentes precisam ser inventados para contornar a restrição de idioma que as substituições devem retornar o mesmo tipo do método substituído. Veja abaixo um exemplo da base de código Roslyn.

## <a name="detailed-design"></a>Design detalhado
[design]: #detailed-design

Suporte a _tipos de retorno covariantes_. Especificamente, permitir que um método de substituição tenha um tipo de referência mais derivado do que o método que ele substitui. Isso se aplicaria a métodos e propriedades e terá suporte em classes e interfaces.

Isso seria útil no padrão de fábrica. Por exemplo, na base de código Roslyn, teríamos

``` cs
class Compilation ...
{
    virtual Compilation WithOptions(Options options)...
}
```

``` cs
class CSharpCompilation : Compilation
{
    override CSharpCompilation WithOptions(Options options)...
}
```

A implementação disso seria para o compilador emitir o método de substituição como um método virtual "New" que oculta o método de classe base, junto com um método de _ponte_ que implementa o método de classe base com uma chamada para o método de classe derivada.

## <a name="drawbacks"></a>Desvantagens
[drawbacks]: #drawbacks

- [] Cada alteração de idioma deve pagar por si mesma.
- [] Devemos garantir que o desempenho seja razoável, mesmo no caso de hierarquias de herança profundas
- [] Devemos garantir que os artefatos da estratégia de tradução não afetem a semântica da linguagem, mesmo ao consumir o novo IL de compiladores antigos.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

Poderíamos relaxar ligeiramente as regras de linguagem para permitir, na origem,

```csharp
abstract class Cloneable
{
    public abstract Cloneable Clone();
}

class Digit : Cloneable
{
    public override Cloneable Clone()
    {
        return this.Clone();
    }

    public new Digit Clone() // Error: 'Digit' already defines a member called 'Clone' with the same parameter types
    {
        return this;
    }
}
```

## <a name="unresolved-questions"></a>Perguntas não resolvidas
[unresolved]: #unresolved-questions

- [] Como as APIs que foram compiladas para usar esse recurso funcionam em versões mais antigas do idioma?

## <a name="design-meetings"></a>Criar reuniões

Nenhum ainda. Houve alguma discussão em <https://github.com/dotnet/roslyn/issues/357>.