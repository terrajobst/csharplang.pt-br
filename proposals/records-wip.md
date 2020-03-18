---
ms.openlocfilehash: 25756c1811d5e6dc97512ce70f99ab7fefa91c4a
ms.sourcegitcommit: 2a6dffb60718065ece95df75e1cc7eb509e48a8d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2020
ms.locfileid: "79485234"
---
# <a name="records-work-in-progress"></a><span data-ttu-id="22223-101">Registros de trabalho em andamento</span><span class="sxs-lookup"><span data-stu-id="22223-101">Records Work-in-Progress</span></span>

<span data-ttu-id="22223-102">Ao contrário das outras propostas de registros, essa não é uma proposta em si, mas um trabalho em andamento é projetado para registrar decisões de design de consenso para o recurso de registros.</span><span class="sxs-lookup"><span data-stu-id="22223-102">Unlike the other records proposals, this is not a proposal in itself, but a work-in-progress designed to record consensus design decisions for the records feature.</span></span> <span data-ttu-id="22223-103">Os detalhes de especificação serão adicionados conforme necessário para resolver perguntas.</span><span class="sxs-lookup"><span data-stu-id="22223-103">Specification detail will be added as necessary to resolve questions.</span></span>

<span data-ttu-id="22223-104">A sintaxe de um registro é proposta para ser adicionada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="22223-104">The syntax for a record is proposed to be added as follows:</span></span>

```antlr
class_declaration
    : attributes? class_modifiers? 'partial'? 'class' identifier type_parameter_list?
      parameter_list? type_parameter_constraints_clauses? class_body
    ;

struct_declaration
    : attributes? struct_modifiers? 'partial'? 'struct' identifier type_parameter_list?
      parameter_list? struct_interfaces? type_parameter_constraints_clauses? struct_body
    ;

class_body
    : '{' class_member_declarations? '}'
    | ';'
    ;

struct_body
    : '{' struct_members_declarations? '}'
    | ';'
    ;
```

<span data-ttu-id="22223-105">O `attributes` não terminal também permitirá um novo atributo contextual, `data`.</span><span class="sxs-lookup"><span data-stu-id="22223-105">The `attributes` non-terminal will also permit a new contextual attribute, `data`.</span></span>

<span data-ttu-id="22223-106">Uma classe (struct) declarada com uma lista de parâmetros ou `data` modificador é chamada de classe de registro (struct de registro), ou seja, um tipo de registro.</span><span class="sxs-lookup"><span data-stu-id="22223-106">A class (struct) declared with a parameter list or `data` modifier is called a record class (record struct), either of which is a record type.</span></span>

<span data-ttu-id="22223-107">É um erro declarar um tipo de registro sem uma lista de parâmetros e o modificador de `data`.</span><span class="sxs-lookup"><span data-stu-id="22223-107">It is an error to declare a record type without both a parameter list and the `data` modifier.</span></span>

## <a name="members-of-a-record-type"></a><span data-ttu-id="22223-108">Membros de um tipo de registro</span><span class="sxs-lookup"><span data-stu-id="22223-108">Members of a record type</span></span>

<span data-ttu-id="22223-109">Além dos membros declarados no corpo da classe, um tipo de registro tem os seguintes membros adicionais:</span><span class="sxs-lookup"><span data-stu-id="22223-109">In addition to the members declared in the class-body, a record type has the following additional members:</span></span>

### <a name="primary-constructor"></a><span data-ttu-id="22223-110">Construtor principal</span><span class="sxs-lookup"><span data-stu-id="22223-110">Primary Constructor</span></span>

<span data-ttu-id="22223-111">Um tipo de registro tem um construtor público cuja assinatura corresponde aos parâmetros de valor da declaração de tipo.</span><span class="sxs-lookup"><span data-stu-id="22223-111">A record type has a public constructor whose signature corresponds to the value parameters of the type declaration.</span></span> <span data-ttu-id="22223-112">Isso é chamado de Construtor principal para o tipo e faz com que o construtor padrão declarado implicitamente seja suprimido.</span><span class="sxs-lookup"><span data-stu-id="22223-112">This is called the primary constructor for the type, and causes the implicitly declared default constructor to be suppressed.</span></span> <span data-ttu-id="22223-113">É um erro ter um construtor primário e um construtor com a mesma assinatura já presentes na classe.</span><span class="sxs-lookup"><span data-stu-id="22223-113">It is an error to have a primary constructor and a constructor with the same signature already present in the class.</span></span>
<span data-ttu-id="22223-114">Em tempo de execução, o construtor primário</span><span class="sxs-lookup"><span data-stu-id="22223-114">At runtime the primary constructor</span></span> 

1. <span data-ttu-id="22223-115">executa os inicializadores de campo de instância que aparecem no corpo da classe; e, em seguida, invoca o construtor da classe base sem argumentos.</span><span class="sxs-lookup"><span data-stu-id="22223-115">executes the instance field initializers appearing in the class-body; and then  invokes the base class constructor with no arguments.</span></span>

1. <span data-ttu-id="22223-116">Inicializa os campos de backup gerados pelo compilador para as propriedades correspondentes aos parâmetros de valor (se essas propriedades forem fornecidas pelo compilador; consulte [Propriedades sintetizadas](#Synthesized Properties))</span><span class="sxs-lookup"><span data-stu-id="22223-116">initializes compiler-generated backing fields for the properties corresponding to the value parameters (if these properties are compiler-provided; see [Synthesized properties](#Synthesized Properties))</span></span>


<span data-ttu-id="22223-117">[] TODO: Adicionar sintaxe de chamada base e especificação sobre como escolher o construtor base por meio da resolução de sobrecarga</span><span class="sxs-lookup"><span data-stu-id="22223-117">[ ] TODO: add base call syntax and specification about choosing base constructor through overload resolution</span></span>

### <a name="properties"></a><span data-ttu-id="22223-118">{1&gt;Propriedades&lt;1}</span><span class="sxs-lookup"><span data-stu-id="22223-118">Properties</span></span>

<span data-ttu-id="22223-119">Para cada parâmetro de registro de uma declaração de tipo de registro, há um membro de propriedade pública correspondente cujo nome e tipo são tirados da declaração de parâmetro de valor.</span><span class="sxs-lookup"><span data-stu-id="22223-119">For each record parameter of a record type declaration there is a corresponding public property member whose name and type are taken from the value parameter declaration.</span></span> <span data-ttu-id="22223-120">Se nenhuma propriedade concreta (ou seja, não abstrata) com um acessador get e com esse nome e tipo for explicitamente declarada ou herdada, ela será produzida pelo compilador da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="22223-120">If no concrete (i.e. non-abstract) property with a get accessor and with this name and type is explicitly declared or inherited, it is produced by the compiler as follows:</span></span>

<span data-ttu-id="22223-121">Para uma estrutura de registro ou uma classe de registro:</span><span class="sxs-lookup"><span data-stu-id="22223-121">For a record struct or a record class:</span></span>

* <span data-ttu-id="22223-122">Uma propriedade automática somente obtenção pública é criada.</span><span class="sxs-lookup"><span data-stu-id="22223-122">A public get-only auto-property is created.</span></span> <span data-ttu-id="22223-123">Seu valor é inicializado durante a construção com o valor do parâmetro de construtor primário correspondente.</span><span class="sxs-lookup"><span data-stu-id="22223-123">Its value is initialized during construction with the value of the corresponding primary constructor parameter.</span></span> <span data-ttu-id="22223-124">Cada acessador get herdado de propriedade abstrata "correspondente" é substituído.</span><span class="sxs-lookup"><span data-stu-id="22223-124">Each "matching" inherited abstract property's get accessor is overridden.</span></span>

### <a name="equality-members"></a><span data-ttu-id="22223-125">Membros de igualdade</span><span class="sxs-lookup"><span data-stu-id="22223-125">Equality members</span></span>

<span data-ttu-id="22223-126">Os tipos de registro produzem implementações sintetizadas para os seguintes métodos:</span><span class="sxs-lookup"><span data-stu-id="22223-126">Record types produce synthesized implementations for the following methods:</span></span>

* <span data-ttu-id="22223-127">`object.GetHashCode()` substituir, a menos que seja lacrado ou fornecido pelo usuário</span><span class="sxs-lookup"><span data-stu-id="22223-127">`object.GetHashCode()` override, unless it is sealed or user provided</span></span>
* <span data-ttu-id="22223-128">`object.Equals(object)` substituir, a menos que seja lacrado ou fornecido pelo usuário</span><span class="sxs-lookup"><span data-stu-id="22223-128">`object.Equals(object)` override, unless it is sealed or user provided</span></span>
* <span data-ttu-id="22223-129">`T Equals(T)` método, em que `T` é o tipo atual</span><span class="sxs-lookup"><span data-stu-id="22223-129">`T Equals(T)` method, where `T` is the current type</span></span>

<span data-ttu-id="22223-130">`T Equals(T)` é especificado para executar a igualdade de valor, comparando a propriedade com o mesmo nome de cada parâmetro de construtor primário para a propriedade correspondente do outro tipo.</span><span class="sxs-lookup"><span data-stu-id="22223-130">`T Equals(T)` is specified to perform value equality, comparing the property with same name as each primary constructor parameter to the corresponding property of the other type.</span></span>
<span data-ttu-id="22223-131">`object.Equals` executa o equivalente de</span><span class="sxs-lookup"><span data-stu-id="22223-131">`object.Equals` performs the equivalent of</span></span>

```C#
override Equals(object o) => Equals(o as T);
```
