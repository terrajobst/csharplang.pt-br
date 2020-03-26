---
ms.openlocfilehash: 258ae6865c5b2c3103a0cdf7e1e5a2cdee11e740
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281951"
---
# <a name="records-work-in-progress"></a><span data-ttu-id="3f3b0-101">Registros de trabalho em andamento</span><span class="sxs-lookup"><span data-stu-id="3f3b0-101">Records Work-in-Progress</span></span>

<span data-ttu-id="3f3b0-102">Ao contrário das outras propostas de registros, essa não é uma proposta em si, mas um trabalho em andamento é projetado para registrar decisões de design de consenso para o recurso de registros.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-102">Unlike the other records proposals, this is not a proposal in itself, but a work-in-progress designed to record consensus design decisions for the records feature.</span></span> <span data-ttu-id="3f3b0-103">Os detalhes de especificação serão adicionados conforme necessário para resolver perguntas.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-103">Specification detail will be added as necessary to resolve questions.</span></span>

<span data-ttu-id="3f3b0-104">A sintaxe de um registro é proposta para ser adicionada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="3f3b0-104">The syntax for a record is proposed to be added as follows:</span></span>

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

<span data-ttu-id="3f3b0-105">O `attributes` não terminal também permitirá um novo atributo contextual, `data`.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-105">The `attributes` non-terminal will also permit a new contextual attribute, `data`.</span></span>

<span data-ttu-id="3f3b0-106">Uma classe (struct) declarada com uma lista de parâmetros ou `data` modificador é chamada de classe de registro (struct de registro), ou seja, um tipo de registro.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-106">A class (struct) declared with a parameter list or `data` modifier is called a record class (record struct), either of which is a record type.</span></span>

<span data-ttu-id="3f3b0-107">É um erro declarar um tipo de registro sem uma lista de parâmetros e o modificador de `data`.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-107">It is an error to declare a record type without both a parameter list and the `data` modifier.</span></span>

## <a name="members-of-a-record-type"></a><span data-ttu-id="3f3b0-108">Membros de um tipo de registro</span><span class="sxs-lookup"><span data-stu-id="3f3b0-108">Members of a record type</span></span>

<span data-ttu-id="3f3b0-109">Além dos membros declarados no corpo da classe, um tipo de registro tem os seguintes membros adicionais:</span><span class="sxs-lookup"><span data-stu-id="3f3b0-109">In addition to the members declared in the class-body, a record type has the following additional members:</span></span>

### <a name="primary-constructor"></a><span data-ttu-id="3f3b0-110">Construtor principal</span><span class="sxs-lookup"><span data-stu-id="3f3b0-110">Primary Constructor</span></span>

<span data-ttu-id="3f3b0-111">Um tipo de registro tem um construtor público cuja assinatura corresponde aos parâmetros de valor da declaração de tipo.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-111">A record type has a public constructor whose signature corresponds to the value parameters of the type declaration.</span></span> <span data-ttu-id="3f3b0-112">Isso é chamado de Construtor principal para o tipo e faz com que o construtor padrão declarado implicitamente seja suprimido.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-112">This is called the primary constructor for the type, and causes the implicitly declared default constructor to be suppressed.</span></span> <span data-ttu-id="3f3b0-113">É um erro ter um construtor primário e um construtor com a mesma assinatura já presentes na classe.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-113">It is an error to have a primary constructor and a constructor with the same signature already present in the class.</span></span>
<span data-ttu-id="3f3b0-114">Em tempo de execução, o construtor primário</span><span class="sxs-lookup"><span data-stu-id="3f3b0-114">At runtime the primary constructor</span></span> 

1. <span data-ttu-id="3f3b0-115">executa os inicializadores de campo de instância que aparecem no corpo da classe; e, em seguida, invoca o construtor da classe base sem argumentos.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-115">executes the instance field initializers appearing in the class-body; and then  invokes the base class constructor with no arguments.</span></span>

1. <span data-ttu-id="3f3b0-116">Inicializa os campos de backup gerados pelo compilador para as propriedades correspondentes aos parâmetros de valor (se essas propriedades forem fornecidas pelo compilador; consulte [Propriedades sintetizadas](#Synthesized Properties))</span><span class="sxs-lookup"><span data-stu-id="3f3b0-116">initializes compiler-generated backing fields for the properties corresponding to the value parameters (if these properties are compiler-provided; see [Synthesized properties](#Synthesized Properties))</span></span>


<span data-ttu-id="3f3b0-117">[] TODO: Adicionar sintaxe de chamada base e especificação sobre como escolher o construtor base por meio da resolução de sobrecarga</span><span class="sxs-lookup"><span data-stu-id="3f3b0-117">[ ] TODO: add base call syntax and specification about choosing base constructor through overload resolution</span></span>

### <a name="properties"></a><span data-ttu-id="3f3b0-118">Propriedades</span><span class="sxs-lookup"><span data-stu-id="3f3b0-118">Properties</span></span>

<span data-ttu-id="3f3b0-119">Para cada parâmetro de registro de uma declaração de tipo de registro, há um membro de propriedade pública correspondente cujo nome e tipo são tirados da declaração de parâmetro de valor.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-119">For each record parameter of a record type declaration there is a corresponding public property member whose name and type are taken from the value parameter declaration.</span></span> <span data-ttu-id="3f3b0-120">Se nenhuma propriedade concreta (ou seja, não abstrata) com um acessador get e com esse nome e tipo for explicitamente declarada ou herdada, ela será produzida pelo compilador da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="3f3b0-120">If no concrete (i.e. non-abstract) property with a get accessor and with this name and type is explicitly declared or inherited, it is produced by the compiler as follows:</span></span>

<span data-ttu-id="3f3b0-121">Para uma estrutura de registro ou uma classe de registro:</span><span class="sxs-lookup"><span data-stu-id="3f3b0-121">For a record struct or a record class:</span></span>

* <span data-ttu-id="3f3b0-122">Uma propriedade automática somente obtenção pública é criada.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-122">A public get-only auto-property is created.</span></span> <span data-ttu-id="3f3b0-123">Seu valor é inicializado durante a construção com o valor do parâmetro de construtor primário correspondente.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-123">Its value is initialized during construction with the value of the corresponding primary constructor parameter.</span></span> <span data-ttu-id="3f3b0-124">Cada acessador get herdado de propriedade abstrata "correspondente" é substituído.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-124">Each "matching" inherited abstract property's get accessor is overridden.</span></span>

### <a name="equality-members"></a><span data-ttu-id="3f3b0-125">Membros de igualdade</span><span class="sxs-lookup"><span data-stu-id="3f3b0-125">Equality members</span></span>

<span data-ttu-id="3f3b0-126">Os tipos de registro produzem implementações sintetizadas para os seguintes métodos:</span><span class="sxs-lookup"><span data-stu-id="3f3b0-126">Record types produce synthesized implementations for the following methods:</span></span>

* <span data-ttu-id="3f3b0-127">`object.GetHashCode()` substituir, a menos que seja lacrado ou fornecido pelo usuário</span><span class="sxs-lookup"><span data-stu-id="3f3b0-127">`object.GetHashCode()` override, unless it is sealed or user provided</span></span>
* <span data-ttu-id="3f3b0-128">`object.Equals(object)` substituir, a menos que seja lacrado ou fornecido pelo usuário</span><span class="sxs-lookup"><span data-stu-id="3f3b0-128">`object.Equals(object)` override, unless it is sealed or user provided</span></span>
* <span data-ttu-id="3f3b0-129">`T Equals(T)` método, em que `T` é o tipo atual</span><span class="sxs-lookup"><span data-stu-id="3f3b0-129">`T Equals(T)` method, where `T` is the current type</span></span>

<span data-ttu-id="3f3b0-130">`T Equals(T)` é especificado para executar a igualdade de valor, comparando a propriedade com o mesmo nome de cada parâmetro de construtor primário para a propriedade correspondente do outro tipo.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-130">`T Equals(T)` is specified to perform value equality, comparing the property with same name as each primary constructor parameter to the corresponding property of the other type.</span></span>
<span data-ttu-id="3f3b0-131">`object.Equals` executa o equivalente de</span><span class="sxs-lookup"><span data-stu-id="3f3b0-131">`object.Equals` performs the equivalent of</span></span>

```C#
override Equals(object o) => Equals(o as T);
```

## <a name="with-expression"></a><span data-ttu-id="3f3b0-132">Expressão `with`</span><span class="sxs-lookup"><span data-stu-id="3f3b0-132">`with` expression</span></span>

<span data-ttu-id="3f3b0-133">Uma expressão `with` é uma nova expressão usando a sintaxe a seguir.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-133">A `with` expression is a new expression using the following syntax.</span></span>

```antlr
with_expression
    : switch_expression
    | switch_expression 'with' anonymous_object_initializer
```

<span data-ttu-id="3f3b0-134">Uma expressão `with` permite a "mutação não destrutiva", projetada para produzir uma cópia da expressão do destinatário com modificações nas propriedades listadas na `anonymous_object_initializer`.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-134">A `with` expression allows for "non-destructive mutation", designed to produce a copy of the receiver expression with modifications to properties listed in the `anonymous_object_initializer`.</span></span>

<span data-ttu-id="3f3b0-135">Uma expressão de `with` válida tem um receptor com um tipo não void.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-135">A valid `with` expression has a receiver with a non-void type.</span></span> <span data-ttu-id="3f3b0-136">O tipo de receptor deve conter um método de instância acessível chamado `With` com os parâmetros e o tipo de retorno apropriados.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-136">The receiver type must contain an accessible instance method called `With` with the appropriate parameters and return type.</span></span> <span data-ttu-id="3f3b0-137">Ocorrerá um erro se houver vários métodos de `With` não substituição.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-137">It is an error if there are multiple non-override `With` methods.</span></span> <span data-ttu-id="3f3b0-138">Se houver várias substituições `With`, deverá haver um método `With` não substituir, que é o método de destino.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-138">If there are multiple `With` overrides, there must be a non-override `With` method, which is the target method.</span></span> <span data-ttu-id="3f3b0-139">Caso contrário, deve haver exatamente um método `With`.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-139">Otherwise, there must be exactly one `With` method.</span></span>

<span data-ttu-id="3f3b0-140">No lado direito da `with` expressão é um `anonymous_object_initializer` com uma sequência de atribuições com um campo ou Propriedade do destinatário no lado esquerdo da atribuição, e uma expressão arbitrária no lado direito, que é implicitamente conversível para o tipo do lado esquerdo da ordem de entrega.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-140">On the right hand side of the `with` expression is an `anonymous_object_initializer` with a sequence of assignments with a field or property of the receiver on the left-hand side of the assignment, and an arbitrary expression on the right-hand side which is implicitly convertible to the type of the left-hand side.</span></span>

<span data-ttu-id="3f3b0-141">Dado um método de `With` de destino, o tipo de retorno deve ser o tipo do tipo de expressão do receptor ou um tipo de base dele.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-141">Given a target `With` method, the return type must be the type of the receiver expression type, or a base type thereof.</span></span> <span data-ttu-id="3f3b0-142">Para cada parâmetro do método `With`, deve haver um campo de instância correspondente acessível ou uma propriedade legível no tipo de receptor com o mesmo nome e o mesmo tipo.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-142">For each parameter of the `With` method, there must be an accessible corresponding instance field or readable property on the receiver type with the same name and the same type.</span></span> <span data-ttu-id="3f3b0-143">Cada propriedade ou campo no lado direito da expressão with também deve corresponder a um parâmetro de mesmo nome no método `With`.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-143">Each property or field in the right-hand side of the With expression must also correspond to a parameter of the same name in the `With` method.</span></span>

<span data-ttu-id="3f3b0-144">Dado um método de `With` válido, a avaliação de uma expressão `with` é equivalente a chamar o método `With` com as expressões no `anonymous_object_initializer` substituído para o parâmetro do mesmo nome que a propriedade no lado esquerdo.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-144">Given a valid `With` method, the evaluation of a `with` expression is equivalent to calling the `With` method with the expressions in the `anonymous_object_initializer` substituted for the parameter of the same name as the property on the left hand side.</span></span> <span data-ttu-id="3f3b0-145">Se não houver nenhuma propriedade correspondente para um determinado parâmetro na `anonymous_object_initializer`, o argumento será a avaliação do campo ou da Propriedade do mesmo nome no receptor.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-145">If there is no matching property for a given parameter in the `anonymous_object_initializer`, the argument is the evaluation of the field or property of the same name on the receiver.</span></span>

<span data-ttu-id="3f3b0-146">A ordem de avaliação de efeitos colaterais é a seguinte, com cada expressão avaliada exatamente uma vez:</span><span class="sxs-lookup"><span data-stu-id="3f3b0-146">The order of evaluation of side effects is as follows, with each expression evaluated exactly once:</span></span>

1. <span data-ttu-id="3f3b0-147">Expressão do destinatário</span><span class="sxs-lookup"><span data-stu-id="3f3b0-147">Receiver expression</span></span>

2. <span data-ttu-id="3f3b0-148">Expressões no `anonymous_object_initializer`, em ordem lexical</span><span class="sxs-lookup"><span data-stu-id="3f3b0-148">Expressions in the `anonymous_object_initializer`, in lexical order</span></span>

3. <span data-ttu-id="3f3b0-149">A avaliação de todas as propriedades que correspondem aos parâmetros do método `With`, em ordem de definição dos parâmetros do método `With`.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-149">The evaluation of any properties matching the `With` method parameters, in order of definition of the `With` method parameters.</span></span>