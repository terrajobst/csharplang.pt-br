---
ms.openlocfilehash: 2e054c629f71ae37b112300905c3106f9ce977e8
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484660"
---
# <a name="auto-implemented-property-field-targeted-attributes"></a><span data-ttu-id="1a40f-101">Campo de propriedade autoimplementada – atributos direcionados</span><span class="sxs-lookup"><span data-stu-id="1a40f-101">Auto-Implemented Property Field-Targeted Attributes</span></span>

## <a name="summary"></a><span data-ttu-id="1a40f-102">Resumo</span><span class="sxs-lookup"><span data-stu-id="1a40f-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="1a40f-103">Esse recurso pretende permitir que os desenvolvedores apliquem atributos diretamente aos campos de backup das propriedades implementadas automaticamente.</span><span class="sxs-lookup"><span data-stu-id="1a40f-103">This feature intends to allow developers to apply attributes directly to the backing fields of auto-implemented properties.</span></span>

## <a name="motivation"></a><span data-ttu-id="1a40f-104">Motivação</span><span class="sxs-lookup"><span data-stu-id="1a40f-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="1a40f-105">Atualmente, não é possível aplicar atributos aos campos de backup das propriedades implementadas automaticamente.</span><span class="sxs-lookup"><span data-stu-id="1a40f-105">Currently it is not possible to apply attributes to the backing fields of auto-implemented properties.</span></span>  <span data-ttu-id="1a40f-106">Nesses casos em que o desenvolvedor deve usar um atributo de direcionamento de campo, ele é forçado a declarar o campo manualmente e usar a sintaxe de propriedade mais detalhada.</span><span class="sxs-lookup"><span data-stu-id="1a40f-106">In those cases where the developer must use a field-targeting attribute they are forced to declare the field manually and use the more verbose property syntax.</span></span>  <span data-ttu-id="1a40f-107">Considerando que C# sempre há suporte para atributos direcionados a campo no campo de apoio gerado para eventos, faz sentido estender a mesma funcionalidade para sua propriedade parentes.</span><span class="sxs-lookup"><span data-stu-id="1a40f-107">Given that C# has always supported field-targeted attributes on the generated backing field for events it makes sense to extend the same functionality to their property kin.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="1a40f-108">Design detalhado</span><span class="sxs-lookup"><span data-stu-id="1a40f-108">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="1a40f-109">Em suma, o seguinte seria legal C# e não produziria um aviso:</span><span class="sxs-lookup"><span data-stu-id="1a40f-109">In short, the following would be legal C# and not produce a warning:</span></span>

```csharp
[Serializable]
public class Foo 
{
    [field: NonSerialized]
    public string MySecret { get; set; }
}
```

<span data-ttu-id="1a40f-110">Isso resultaria na aplicação dos atributos direcionados por campo ao campo de backup gerado pelo compilador:</span><span class="sxs-lookup"><span data-stu-id="1a40f-110">This would result in the field-targeted attributes being applied to the compiler-generated backing field:</span></span>

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

<span data-ttu-id="1a40f-111">Como mencionado, isso traz a paridade com a sintaxe C# de evento de 1,0, pois o seguinte já é legal e se comporta conforme o esperado:</span><span class="sxs-lookup"><span data-stu-id="1a40f-111">As mentioned, this brings parity with event syntax from C# 1.0 as the following is already legal and behaves as expected:</span></span>

```csharp
[Serializable]
public class Foo
{
    [field: NonSerialized]
    public event EventHandler MyEvent;
}
```

## <a name="drawbacks"></a><span data-ttu-id="1a40f-112">Desvantagens</span><span class="sxs-lookup"><span data-stu-id="1a40f-112">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="1a40f-113">Há duas desvantagens em potencial para implementar essa alteração:</span><span class="sxs-lookup"><span data-stu-id="1a40f-113">There are two potential drawbacks to implementing this change:</span></span>

1. <span data-ttu-id="1a40f-114">A tentativa de aplicar um atributo ao campo de uma propriedade implementada automaticamente produz um aviso do compilador de que os atributos nesse bloco serão ignorados.</span><span class="sxs-lookup"><span data-stu-id="1a40f-114">Attempting to apply an attribute to the field of an auto-implemented property produces a compiler warning that the attributes in that block will be ignored.</span></span>  <span data-ttu-id="1a40f-115">Se o compilador foi alterado para dar suporte a esses atributos, eles seriam aplicados ao campo de backup em uma recompilação subsequente, o que poderia alterar o comportamento do programa em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="1a40f-115">If the compiler were changed to support those attributes they would be applied to the backing field on a subsequent recompilation which could alter the behavior of the program at runtime.</span></span>
1. <span data-ttu-id="1a40f-116">Atualmente, o compilador não valida os destinos AttributeUsage dos atributos ao tentar aplicá-los ao campo da propriedade implementada automaticamente.</span><span class="sxs-lookup"><span data-stu-id="1a40f-116">The compiler does not currently validate the AttributeUsage targets of the attributes when attempting to apply them to the field of the auto-implemented property.</span></span>  <span data-ttu-id="1a40f-117">Se o compilador tiver sido alterado para dar suporte a atributos direcionados a campo e o atributo em questão não puder ser aplicado a um campo, o compilador emitiria um erro em vez de um aviso, dividindo a compilação.</span><span class="sxs-lookup"><span data-stu-id="1a40f-117">If the compiler were changed to support field-targeted attributes and the attribute in question cannot be applied to a field the compiler would emit an error instead of a warning, breaking the build.</span></span>

## <a name="alternatives"></a><span data-ttu-id="1a40f-118">Alternativas</span><span class="sxs-lookup"><span data-stu-id="1a40f-118">Alternatives</span></span>
[alternatives]: #alternatives

## <a name="unresolved-questions"></a><span data-ttu-id="1a40f-119">Perguntas não resolvidas</span><span class="sxs-lookup"><span data-stu-id="1a40f-119">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a><span data-ttu-id="1a40f-120">Criar reuniões</span><span class="sxs-lookup"><span data-stu-id="1a40f-120">Design meetings</span></span>
