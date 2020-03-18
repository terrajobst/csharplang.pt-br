---
ms.openlocfilehash: 2e054c629f71ae37b112300905c3106f9ce977e8
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484660"
---
# <a name="auto-implemented-property-field-targeted-attributes"></a>Campo de propriedade autoimplementada – atributos direcionados

## <a name="summary"></a>Resumo
[summary]: #summary

Esse recurso pretende permitir que os desenvolvedores apliquem atributos diretamente aos campos de backup das propriedades implementadas automaticamente.

## <a name="motivation"></a>Motivação
[motivation]: #motivation

Atualmente, não é possível aplicar atributos aos campos de backup das propriedades implementadas automaticamente.  Nesses casos em que o desenvolvedor deve usar um atributo de direcionamento de campo, ele é forçado a declarar o campo manualmente e usar a sintaxe de propriedade mais detalhada.  Considerando que C# sempre há suporte para atributos direcionados a campo no campo de apoio gerado para eventos, faz sentido estender a mesma funcionalidade para sua propriedade parentes.

## <a name="detailed-design"></a>Design detalhado
[design]: #detailed-design

Em suma, o seguinte seria legal C# e não produziria um aviso:

```csharp
[Serializable]
public class Foo 
{
    [field: NonSerialized]
    public string MySecret { get; set; }
}
```

Isso resultaria na aplicação dos atributos direcionados por campo ao campo de backup gerado pelo compilador:

```csharp
[Serializable]
public class Foo 
{
    [NonSerialized]
    private string _mySecretBackingField;
    
    public string MySecret
    {
        get { return _mySecretBackingField; }
        set { _mySecretBackingField = value; }
    }
}
```

Como mencionado, isso traz a paridade com a sintaxe C# de evento de 1,0, pois o seguinte já é legal e se comporta conforme o esperado:

```csharp
[Serializable]
public class Foo
{
    [field: NonSerialized]
    public event EventHandler MyEvent;
}
```

## <a name="drawbacks"></a>Desvantagens
[drawbacks]: #drawbacks

Há duas desvantagens em potencial para implementar essa alteração:

1. A tentativa de aplicar um atributo ao campo de uma propriedade implementada automaticamente produz um aviso do compilador de que os atributos nesse bloco serão ignorados.  Se o compilador foi alterado para dar suporte a esses atributos, eles seriam aplicados ao campo de backup em uma recompilação subsequente, o que poderia alterar o comportamento do programa em tempo de execução.
1. Atualmente, o compilador não valida os destinos AttributeUsage dos atributos ao tentar aplicá-los ao campo da propriedade implementada automaticamente.  Se o compilador tiver sido alterado para dar suporte a atributos direcionados a campo e o atributo em questão não puder ser aplicado a um campo, o compilador emitiria um erro em vez de um aviso, dividindo a compilação.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

## <a name="unresolved-questions"></a>Perguntas não resolvidas
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a>Criar reuniões
