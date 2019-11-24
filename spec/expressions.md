---
ms.openlocfilehash: f61039abd6bd557ac0ea625e6aac1c8bafa57b02
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704084"
---
# <a name="expressions"></a><span data-ttu-id="dd693-101">{1&gt;Expressões&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-101">Expressions</span></span>

<span data-ttu-id="dd693-102">Uma expressão é uma sequência de operadores e operandos.</span><span class="sxs-lookup"><span data-stu-id="dd693-102">An expression is a sequence of operators and operands.</span></span> <span data-ttu-id="dd693-103">Este capítulo define a sintaxe, a ordem de avaliação de operandos e operadores e o significado de expressões.</span><span class="sxs-lookup"><span data-stu-id="dd693-103">This chapter defines the syntax, order of evaluation of operands and operators, and meaning of expressions.</span></span>

## <a name="expression-classifications"></a><span data-ttu-id="dd693-104">Classificações de expressão</span><span class="sxs-lookup"><span data-stu-id="dd693-104">Expression classifications</span></span>

<span data-ttu-id="dd693-105">Uma expressão é classificada como uma das seguintes:</span><span class="sxs-lookup"><span data-stu-id="dd693-105">An expression is classified as one of the following:</span></span>

*  <span data-ttu-id="dd693-106">Um valor.</span><span class="sxs-lookup"><span data-stu-id="dd693-106">A value.</span></span> <span data-ttu-id="dd693-107">Cada valor tem um tipo associado.</span><span class="sxs-lookup"><span data-stu-id="dd693-107">Every value has an associated type.</span></span>
*  <span data-ttu-id="dd693-108">Uma variável.</span><span class="sxs-lookup"><span data-stu-id="dd693-108">A variable.</span></span> <span data-ttu-id="dd693-109">Cada variável tem um tipo associado, ou seja, o tipo declarado da variável.</span><span class="sxs-lookup"><span data-stu-id="dd693-109">Every variable has an associated type, namely the declared type of the variable.</span></span>
*  <span data-ttu-id="dd693-110">Um namespace.</span><span class="sxs-lookup"><span data-stu-id="dd693-110">A namespace.</span></span> <span data-ttu-id="dd693-111">Uma expressão com essa classificação só pode aparecer como o lado esquerdo de uma *member_access* ([acesso de membro](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="dd693-111">An expression with this classification can only appear as the left hand side of a *member_access* ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="dd693-112">Em qualquer outro contexto, uma expressão classificada como um namespace causa um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-112">In any other context, an expression classified as a namespace causes a compile-time error.</span></span>
*  <span data-ttu-id="dd693-113">Um tipo.</span><span class="sxs-lookup"><span data-stu-id="dd693-113">A type.</span></span> <span data-ttu-id="dd693-114">Uma expressão com essa classificação só pode aparecer como o lado esquerdo de uma *member_access* ([acesso de membro](expressions.md#member-access)) ou como um operando para o operador de `as` ([o operador as](expressions.md#the-as-operator)), o operador de `is` ([o operador is](expressions.md#the-is-operator)) ou o operador `typeof` ([o operador typeof](expressions.md#the-typeof-operator)).</span><span class="sxs-lookup"><span data-stu-id="dd693-114">An expression with this classification can only appear as the left hand side of a *member_access* ([Member access](expressions.md#member-access)), or as an operand for the `as` operator ([The as operator](expressions.md#the-as-operator)), the `is` operator ([The is operator](expressions.md#the-is-operator)), or the `typeof` operator ([The typeof operator](expressions.md#the-typeof-operator)).</span></span> <span data-ttu-id="dd693-115">Em qualquer outro contexto, uma expressão classificada como um tipo causa um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-115">In any other context, an expression classified as a type causes a compile-time error.</span></span>
*  <span data-ttu-id="dd693-116">Um grupo de métodos, que é um conjunto de métodos sobrecarregados resultante de uma pesquisa de membro ([pesquisa de membro](expressions.md#member-lookup)).</span><span class="sxs-lookup"><span data-stu-id="dd693-116">A method group, which is a set of overloaded methods resulting from a member lookup ([Member lookup](expressions.md#member-lookup)).</span></span> <span data-ttu-id="dd693-117">Um grupo de métodos pode ter uma expressão de instância associada e uma lista de argumentos de tipo associado.</span><span class="sxs-lookup"><span data-stu-id="dd693-117">A method group may have an associated instance expression and an associated type argument list.</span></span> <span data-ttu-id="dd693-118">Quando um método de instância é invocado, o resultado da avaliação da expressão de instância se torna a instância representada por `this` ([esse acesso](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="dd693-118">When an instance method is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)).</span></span> <span data-ttu-id="dd693-119">Um grupo de métodos é permitido em *um invocation_expression* ([expressões de invocação](expressions.md#invocation-expressions)), um *delegate_creation_expression* (expressões de[criação de delegado](expressions.md#delegate-creation-expressions)) e como o lado esquerdo de um operador is e pode ser convertido implicitamente em um tipo de delegado compatível ([conversões de grupo de métodos](conversions.md#method-group-conversions)).</span><span class="sxs-lookup"><span data-stu-id="dd693-119">A method group is permitted in an *invocation_expression* ([Invocation expressions](expressions.md#invocation-expressions)) , a *delegate_creation_expression* ([Delegate creation expressions](expressions.md#delegate-creation-expressions)) and as the left hand side of an is operator, and can be implicitly converted to a compatible delegate type ([Method group conversions](conversions.md#method-group-conversions)).</span></span> <span data-ttu-id="dd693-120">Em qualquer outro contexto, uma expressão classificada como um grupo de métodos causa um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-120">In any other context, an expression classified as a method group causes a compile-time error.</span></span>
*  <span data-ttu-id="dd693-121">Um literal nulo.</span><span class="sxs-lookup"><span data-stu-id="dd693-121">A null literal.</span></span> <span data-ttu-id="dd693-122">Uma expressão com essa classificação pode ser convertida implicitamente em um tipo de referência ou tipo anulável.</span><span class="sxs-lookup"><span data-stu-id="dd693-122">An expression with this classification can be implicitly converted to a reference type or nullable type.</span></span>
*  <span data-ttu-id="dd693-123">Uma função anônima.</span><span class="sxs-lookup"><span data-stu-id="dd693-123">An anonymous function.</span></span> <span data-ttu-id="dd693-124">Uma expressão com essa classificação pode ser convertida implicitamente em um tipo de representante ou tipo de árvore de expressão compatível.</span><span class="sxs-lookup"><span data-stu-id="dd693-124">An expression with this classification can be implicitly converted to a compatible delegate type or expression tree type.</span></span>
*  <span data-ttu-id="dd693-125">Um acesso de propriedade.</span><span class="sxs-lookup"><span data-stu-id="dd693-125">A property access.</span></span> <span data-ttu-id="dd693-126">Cada acesso de propriedade tem um tipo associado, ou seja, o tipo da propriedade.</span><span class="sxs-lookup"><span data-stu-id="dd693-126">Every property access has an associated type, namely the type of the property.</span></span> <span data-ttu-id="dd693-127">Além disso, um acesso de propriedade pode ter uma expressão de instância associada.</span><span class="sxs-lookup"><span data-stu-id="dd693-127">Furthermore, a property access may have an associated instance expression.</span></span> <span data-ttu-id="dd693-128">Quando um acessador (o `get` ou bloco de `set`) de um acesso de propriedade de instância é invocado, o resultado da avaliação da expressão de instância se torna a instância representada por `this` ([esse acesso](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="dd693-128">When an accessor (the `get` or `set` block) of an instance property access is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)).</span></span>
*  <span data-ttu-id="dd693-129">Um acesso de evento.</span><span class="sxs-lookup"><span data-stu-id="dd693-129">An event access.</span></span> <span data-ttu-id="dd693-130">Cada acesso de evento tem um tipo associado, ou seja, o tipo do evento.</span><span class="sxs-lookup"><span data-stu-id="dd693-130">Every event access has an associated type, namely the type of the event.</span></span> <span data-ttu-id="dd693-131">Além disso, um acesso de evento pode ter uma expressão de instância associada.</span><span class="sxs-lookup"><span data-stu-id="dd693-131">Furthermore, an event access may have an associated instance expression.</span></span> <span data-ttu-id="dd693-132">Um acesso de evento pode aparecer como o operando à esquerda dos operadores de `+=` e de `-=` ([atribuição de evento](expressions.md#event-assignment)).</span><span class="sxs-lookup"><span data-stu-id="dd693-132">An event access may appear as the left hand operand of the `+=` and `-=` operators ([Event assignment](expressions.md#event-assignment)).</span></span> <span data-ttu-id="dd693-133">Em qualquer outro contexto, uma expressão classificada como um acesso de evento causa um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-133">In any other context, an expression classified as an event access causes a compile-time error.</span></span>
*  <span data-ttu-id="dd693-134">Um acesso ao indexador.</span><span class="sxs-lookup"><span data-stu-id="dd693-134">An indexer access.</span></span> <span data-ttu-id="dd693-135">Cada acesso ao indexador tem um tipo associado, ou seja, o tipo de elemento do indexador.</span><span class="sxs-lookup"><span data-stu-id="dd693-135">Every indexer access has an associated type, namely the element type of the indexer.</span></span> <span data-ttu-id="dd693-136">Além disso, um acesso ao indexador tem uma expressão de instância associada e uma lista de argumentos associados.</span><span class="sxs-lookup"><span data-stu-id="dd693-136">Furthermore, an indexer access has an associated instance expression and an associated argument list.</span></span> <span data-ttu-id="dd693-137">Quando um acessador (o `get` ou bloco de `set`) de um acesso do indexador é invocado, o resultado da avaliação da expressão de instância se torna a instância representada por `this` ([esse acesso](expressions.md#this-access)) e o resultado da avaliação da lista de argumentos se torna a lista de parâmetros da invocação.</span><span class="sxs-lookup"><span data-stu-id="dd693-137">When an accessor (the `get` or `set` block) of an indexer access is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)), and the result of evaluating the argument list becomes the parameter list of the invocation.</span></span>
*  <span data-ttu-id="dd693-138">Nada.</span><span class="sxs-lookup"><span data-stu-id="dd693-138">Nothing.</span></span> <span data-ttu-id="dd693-139">Isso ocorre quando a expressão é uma invocação de um método com um tipo de retorno de `void`.</span><span class="sxs-lookup"><span data-stu-id="dd693-139">This occurs when the expression is an invocation of a method with a return type of `void`.</span></span> <span data-ttu-id="dd693-140">Uma expressão classificada como Nothing só é válida no contexto de uma *statement_expression* ([instruções de expressão](statements.md#expression-statements)).</span><span class="sxs-lookup"><span data-stu-id="dd693-140">An expression classified as nothing is only valid in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>

<span data-ttu-id="dd693-141">O resultado final de uma expressão nunca é um namespace, tipo, grupo de métodos ou acesso de evento.</span><span class="sxs-lookup"><span data-stu-id="dd693-141">The final result of an expression is never a namespace, type, method group, or event access.</span></span> <span data-ttu-id="dd693-142">Em vez disso, conforme observado acima, essas categorias de expressões são construções intermediárias que só são permitidas em determinados contextos.</span><span class="sxs-lookup"><span data-stu-id="dd693-142">Rather, as noted above, these categories of expressions are intermediate constructs that are only permitted in certain contexts.</span></span>

<span data-ttu-id="dd693-143">Um acesso de propriedade ou de indexador é sempre reclassificado como um valor executando uma invocação do *acessador get* ou o *acessador set*.</span><span class="sxs-lookup"><span data-stu-id="dd693-143">A property access or indexer access is always reclassified as a value by performing an invocation of the *get accessor* or the *set accessor*.</span></span> <span data-ttu-id="dd693-144">O acessador específico é determinado pelo contexto da propriedade ou do acesso do indexador: se o acesso for o destino de uma atribuição, o *acessador set* será invocado para atribuir um novo valor ([atribuição simples](expressions.md#simple-assignment)).</span><span class="sxs-lookup"><span data-stu-id="dd693-144">The particular accessor is determined by the context of the property or indexer access: If the access is the target of an assignment, the *set accessor* is invoked to assign a new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="dd693-145">Caso contrário, o *acessador get* é invocado para obter o valor atual ([valores de expressões](expressions.md#values-of-expressions)).</span><span class="sxs-lookup"><span data-stu-id="dd693-145">Otherwise, the *get accessor* is invoked to obtain the current value ([Values of expressions](expressions.md#values-of-expressions)).</span></span>

### <a name="values-of-expressions"></a><span data-ttu-id="dd693-146">Valores de expressões</span><span class="sxs-lookup"><span data-stu-id="dd693-146">Values of expressions</span></span>

<span data-ttu-id="dd693-147">A maioria das construções que envolvem uma expressão, por fim, exige que a expressão denotasse um ***valor***.</span><span class="sxs-lookup"><span data-stu-id="dd693-147">Most of the constructs that involve an expression ultimately require the expression to denote a ***value***.</span></span> <span data-ttu-id="dd693-148">Nesses casos, se a expressão real denota um namespace, um tipo, um grupo de métodos ou nada, ocorrerá um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-148">In such cases, if the actual expression denotes a namespace, a type, a method group, or nothing, a compile-time error occurs.</span></span> <span data-ttu-id="dd693-149">No entanto, se a expressão denota um acesso de propriedade, um acesso de indexador ou uma variável, o valor da propriedade, do indexador ou da variável é substituído implicitamente:</span><span class="sxs-lookup"><span data-stu-id="dd693-149">However, if the expression denotes a property access, an indexer access, or a variable, the value of the property, indexer, or variable is implicitly substituted:</span></span>

*  <span data-ttu-id="dd693-150">O valor de uma variável é simplesmente o valor atualmente armazenado no local de armazenamento identificado pela variável.</span><span class="sxs-lookup"><span data-stu-id="dd693-150">The value of a variable is simply the value currently stored in the storage location identified by the variable.</span></span> <span data-ttu-id="dd693-151">Uma variável deve ser considerada definitivamente atribuída ([atribuição definitiva](variables.md#definite-assignment)) antes que seu valor possa ser obtido ou, caso contrário, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-151">A variable must be considered definitely assigned ([Definite assignment](variables.md#definite-assignment)) before its value can be obtained, or otherwise a compile-time error occurs.</span></span>
*  <span data-ttu-id="dd693-152">O valor de uma expressão de acesso à propriedade é obtido invocando o *acessador get* da propriedade.</span><span class="sxs-lookup"><span data-stu-id="dd693-152">The value of a property access expression is obtained by invoking the *get accessor* of the property.</span></span> <span data-ttu-id="dd693-153">Se a propriedade não tiver nenhum *acessador get*, ocorrerá um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-153">If the property has no *get accessor*, a compile-time error occurs.</span></span> <span data-ttu-id="dd693-154">Caso contrário, uma invocação de membro[de função (verificação de tempo de compilação da resolução dinâmica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) será executada e o resultado da invocação se tornará o valor da expressão de acesso à propriedade.</span><span class="sxs-lookup"><span data-stu-id="dd693-154">Otherwise, a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is performed, and the result of the invocation becomes the value of the property access expression.</span></span>
*  <span data-ttu-id="dd693-155">O valor de uma expressão de acesso do indexador é obtido invocando o *acessador get* do indexador.</span><span class="sxs-lookup"><span data-stu-id="dd693-155">The value of an indexer access expression is obtained by invoking the *get accessor* of the indexer.</span></span> <span data-ttu-id="dd693-156">Se o indexador não tiver nenhum *acessador get*, ocorrerá um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-156">If the indexer has no *get accessor*, a compile-time error occurs.</span></span> <span data-ttu-id="dd693-157">Caso contrário, uma invocação de membro[de função (verificação de tempo de compilação da resolução dinâmica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) é executada com a lista de argumentos associada à expressão de acesso do indexador e o resultado da invocação se torna o valor da expressão de acesso do indexador.</span><span class="sxs-lookup"><span data-stu-id="dd693-157">Otherwise, a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is performed with the argument list associated with the indexer access expression, and the result of the invocation becomes the value of the indexer access expression.</span></span>

## <a name="static-and-dynamic-binding"></a><span data-ttu-id="dd693-158">Associação estática e dinâmica</span><span class="sxs-lookup"><span data-stu-id="dd693-158">Static and Dynamic Binding</span></span>

<span data-ttu-id="dd693-159">O processo de determinar o significado de uma operação com base no tipo ou valor de expressões constituintes (argumentos, operandos, receptores) é geralmente conhecido como ***Associação***.</span><span class="sxs-lookup"><span data-stu-id="dd693-159">The process of determining the meaning of an operation based on the type or value of constituent expressions (arguments, operands, receivers) is often referred to as ***binding***.</span></span> <span data-ttu-id="dd693-160">Por exemplo, o significado de uma chamada de método é determinado com base no tipo de receptor e argumentos.</span><span class="sxs-lookup"><span data-stu-id="dd693-160">For instance the meaning of a method call is determined based on the type of the receiver and arguments.</span></span> <span data-ttu-id="dd693-161">O significado de um operador é determinado com base no tipo de seus operandos.</span><span class="sxs-lookup"><span data-stu-id="dd693-161">The meaning of an operator is determined based on the type of its operands.</span></span>

<span data-ttu-id="dd693-162">No C# sentido de uma operação geralmente é determinado em tempo de compilação, com base no tipo de tempo de compilação de suas expressões constituintes.</span><span class="sxs-lookup"><span data-stu-id="dd693-162">In C# the meaning of an operation is usually determined at compile-time, based on the compile-time type of its constituent expressions.</span></span> <span data-ttu-id="dd693-163">Da mesma forma, se uma expressão contiver um erro, o erro será detectado e relatado pelo compilador.</span><span class="sxs-lookup"><span data-stu-id="dd693-163">Likewise, if an expression contains an error, the error is detected and reported by the compiler.</span></span> <span data-ttu-id="dd693-164">Essa abordagem é conhecida como ***associação estática***.</span><span class="sxs-lookup"><span data-stu-id="dd693-164">This approach is known as ***static binding***.</span></span>

<span data-ttu-id="dd693-165">No entanto, se uma expressão for uma expressão dinâmica (ou seja, tiver o tipo `dynamic`) isso indica que qualquer associação na qual ela participa deve ser baseada em seu tipo de tempo de execução (ou seja, o tipo real do objeto que ele denota em tempo de execução) em vez do tipo que ele tem em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-165">However, if an expression is a dynamic expression (i.e. has the type `dynamic`) this indicates that any binding that it participates in should be based on its run-time type (i.e. the actual type of the object it denotes at run-time) rather than the type it has at compile-time.</span></span> <span data-ttu-id="dd693-166">A associação de tal operação é, portanto, adiada até o momento em que a operação deve ser executada durante a execução do programa.</span><span class="sxs-lookup"><span data-stu-id="dd693-166">The binding of such an operation is therefore deferred until the time where the operation is to be executed during the running of the program.</span></span> <span data-ttu-id="dd693-167">Isso é conhecido como ***associação dinâmica***.</span><span class="sxs-lookup"><span data-stu-id="dd693-167">This is referred to as ***dynamic binding***.</span></span>

<span data-ttu-id="dd693-168">Quando uma operação é vinculada dinamicamente, pouca ou nenhuma verificação é executada pelo compilador.</span><span class="sxs-lookup"><span data-stu-id="dd693-168">When an operation is dynamically bound, little or no checking is performed by the compiler.</span></span> <span data-ttu-id="dd693-169">Em vez disso, se a associação de tempo de execução falhar, os erros serão relatados como exceções em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="dd693-169">Instead if the run-time binding fails, errors are reported as exceptions at run-time.</span></span>

<span data-ttu-id="dd693-170">As seguintes operações no C# estão sujeitas à associação:</span><span class="sxs-lookup"><span data-stu-id="dd693-170">The following operations in C# are subject to binding:</span></span>

*  <span data-ttu-id="dd693-171">Acesso de membro: `e.M`</span><span class="sxs-lookup"><span data-stu-id="dd693-171">Member access: `e.M`</span></span>
*  <span data-ttu-id="dd693-172">Invocação de método: `e.M(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="dd693-172">Method invocation: `e.M(e1, ..., eN)`</span></span>
*  <span data-ttu-id="dd693-173">Invocação de delegado:`e(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="dd693-173">Delegate invocation:`e(e1, ..., eN)`</span></span>
*  <span data-ttu-id="dd693-174">Acesso ao elemento: `e[e1, ..., eN]`</span><span class="sxs-lookup"><span data-stu-id="dd693-174">Element access: `e[e1, ..., eN]`</span></span>
*  <span data-ttu-id="dd693-175">Criação de objeto: `new C(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="dd693-175">Object creation: `new C(e1, ..., eN)`</span></span>
*  <span data-ttu-id="dd693-176">Operadores unários sobrecarregados: `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`</span><span class="sxs-lookup"><span data-stu-id="dd693-176">Overloaded unary operators: `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`</span></span>
*  <span data-ttu-id="dd693-177">Operadores binários sobrecarregados: `+`, `-`, `*`, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, `^`, `<<`, `>>`, `==`,`!=`, `>`, `<`, `>=`, `<=`</span><span class="sxs-lookup"><span data-stu-id="dd693-177">Overloaded binary operators: `+`, `-`, `*`, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, `^`, `<<`, `>>`, `==`,`!=`, `>`, `<`, `>=`, `<=`</span></span>
*  <span data-ttu-id="dd693-178">Operadores de atribuição: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, `>>=`</span><span class="sxs-lookup"><span data-stu-id="dd693-178">Assignment operators: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, `>>=`</span></span>
*  <span data-ttu-id="dd693-179">Conversões implícitas e explícitas</span><span class="sxs-lookup"><span data-stu-id="dd693-179">Implicit and explicit conversions</span></span>

<span data-ttu-id="dd693-180">Quando não há expressões dinâmicas envolvidas C# , o padrão é a associação estática, o que significa que os tipos de tempo de compilação de expressões constituintes são usados no processo de seleção.</span><span class="sxs-lookup"><span data-stu-id="dd693-180">When no dynamic expressions are involved, C# defaults to static binding, which means that the compile-time types of constituent expressions are used in the selection process.</span></span> <span data-ttu-id="dd693-181">No entanto, quando uma das expressões constituintes nas operações listadas acima é uma expressão dinâmica, a operação é vinculada dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="dd693-181">However, when one of the constituent expressions in the operations listed above is a dynamic expression, the operation is instead dynamically bound.</span></span>

### <a name="binding-time"></a><span data-ttu-id="dd693-182">Tempo de associação</span><span class="sxs-lookup"><span data-stu-id="dd693-182">Binding-time</span></span>

<span data-ttu-id="dd693-183">A associação estática ocorre no tempo de compilação, enquanto a associação dinâmica ocorre em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="dd693-183">Static binding takes place at compile-time, whereas dynamic binding takes place at run-time.</span></span> <span data-ttu-id="dd693-184">Nas seções a seguir, o termo ***Associação de tempo refere-*** se ao tempo de compilação ou tempo de execução, dependendo de quando a associação ocorre.</span><span class="sxs-lookup"><span data-stu-id="dd693-184">In the following sections, the term ***binding-time*** refers to either compile-time or run-time, depending on when the binding takes place.</span></span>

<span data-ttu-id="dd693-185">O exemplo a seguir ilustra as noções de associação estática e dinâmica e de tempo de vinculação:</span><span class="sxs-lookup"><span data-stu-id="dd693-185">The following example illustrates the notions of static and dynamic binding and of binding-time:</span></span>
```csharp
object  o = 5;
dynamic d = 5;

Console.WriteLine(5);  // static  binding to Console.WriteLine(int)
Console.WriteLine(o);  // static  binding to Console.WriteLine(object)
Console.WriteLine(d);  // dynamic binding to Console.WriteLine(int)
```

<span data-ttu-id="dd693-186">As duas primeiras chamadas são vinculadas estaticamente: a sobrecarga de `Console.WriteLine` é escolhida com base no tipo de tempo de compilação de seu argumento.</span><span class="sxs-lookup"><span data-stu-id="dd693-186">The first two calls are statically bound: the overload of `Console.WriteLine` is picked based on the compile-time type of their argument.</span></span> <span data-ttu-id="dd693-187">Portanto, o tempo de associação é de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-187">Thus, the binding-time is compile-time.</span></span>

<span data-ttu-id="dd693-188">A terceira chamada é vinculada dinamicamente: a sobrecarga de `Console.WriteLine` é escolhida com base no tipo de tempo de execução de seu argumento.</span><span class="sxs-lookup"><span data-stu-id="dd693-188">The third call is dynamically bound: the overload of `Console.WriteLine` is picked based on the run-time type of its argument.</span></span> <span data-ttu-id="dd693-189">Isso acontece porque o argumento é uma expressão dinâmica – seu tipo de tempo de compilação é `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="dd693-189">This happens because the argument is a dynamic expression -- its compile-time type is `dynamic`.</span></span> <span data-ttu-id="dd693-190">Portanto, o tempo de associação para a terceira chamada é tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="dd693-190">Thus, the binding-time for the third call is run-time.</span></span>

### <a name="dynamic-binding"></a><span data-ttu-id="dd693-191">Associação dinâmica</span><span class="sxs-lookup"><span data-stu-id="dd693-191">Dynamic binding</span></span>

<span data-ttu-id="dd693-192">A finalidade da vinculação dinâmica é permitir C# que os programas interajam com ***objetos dinâmicos***, ou seja, objetos que não seguem as regras normais C# do sistema de tipos.</span><span class="sxs-lookup"><span data-stu-id="dd693-192">The purpose of dynamic binding is to allow C# programs to interact with ***dynamic objects***, i.e. objects that do not follow the normal rules of the C# type system.</span></span> <span data-ttu-id="dd693-193">Objetos dinâmicos podem ser objetos de outras linguagens de programação com sistemas de tipos diferentes, ou podem ser objetos que são configurados programaticamente para implementar sua própria semântica de ligação para operações diferentes.</span><span class="sxs-lookup"><span data-stu-id="dd693-193">Dynamic objects may be objects from other programming languages with different types systems, or they may be objects that are programmatically setup to implement their own binding semantics for different operations.</span></span>

<span data-ttu-id="dd693-194">O mecanismo pelo qual um objeto dinâmico implementa sua própria semântica é a implementação definida.</span><span class="sxs-lookup"><span data-stu-id="dd693-194">The mechanism by which a dynamic object implements its own semantics is implementation defined.</span></span> <span data-ttu-id="dd693-195">Uma determinada interface – novamente definida--é implementada por objetos dinâmicos para sinalizar para C# o tempo de execução que eles têm semântica especial.</span><span class="sxs-lookup"><span data-stu-id="dd693-195">A given interface -- again implementation defined -- is implemented by dynamic objects to signal to the C# run-time that they have special semantics.</span></span> <span data-ttu-id="dd693-196">Portanto, sempre que as operações em um objeto dinâmico forem vinculadas dinamicamente, sua própria semântica de ligação, em C# vez das especificadas neste documento, assumirá.</span><span class="sxs-lookup"><span data-stu-id="dd693-196">Thus, whenever operations on a dynamic object are dynamically bound, their own binding semantics, rather than those of C# as specified in this document, take over.</span></span>

<span data-ttu-id="dd693-197">Embora a finalidade da vinculação dinâmica seja permitir a interoperação com objetos dinâmicos C# , o permite a vinculação dinâmica em todos os objetos, sejam eles dinâmicos ou não.</span><span class="sxs-lookup"><span data-stu-id="dd693-197">While the purpose of dynamic binding is to allow interoperation with dynamic objects, C# allows dynamic binding on all objects, whether they are dynamic or not.</span></span> <span data-ttu-id="dd693-198">Isso permite uma integração mais suave de objetos dinâmicos, pois os resultados das operações neles não podem ser objetos dinâmicos, mas ainda são de um tipo desconhecido para o programador em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-198">This allows for a smoother integration of dynamic objects, as the results of operations on them may not themselves be dynamic objects, but are still of a type unknown to the programmer at compile-time.</span></span> <span data-ttu-id="dd693-199">Além disso, a ligação dinâmica pode ajudar a eliminar códigos baseados em reflexão propensos a erros, mesmo quando nenhum objeto envolvido é um objeto dinâmico.</span><span class="sxs-lookup"><span data-stu-id="dd693-199">Also dynamic binding can help eliminate error-prone reflection-based code even when no objects involved are dynamic objects.</span></span>

<span data-ttu-id="dd693-200">As seções a seguir descrevem cada construção no idioma exatamente quando a vinculação dinâmica é aplicada, qual é a verificação de tempo de compilação – se houver, se houver, e qual será o resultado de tempo de compilação e a classificação de expressão.</span><span class="sxs-lookup"><span data-stu-id="dd693-200">The following sections describe for each construct in the language exactly when dynamic binding is applied, what compile time checking -- if any -- is applied, and what the compile-time result and expression classification is.</span></span>

### <a name="types-of-constituent-expressions"></a><span data-ttu-id="dd693-201">Tipos de expressões constituintes</span><span class="sxs-lookup"><span data-stu-id="dd693-201">Types of constituent expressions</span></span>

<span data-ttu-id="dd693-202">Quando uma operação é vinculada estaticamente, o tipo de uma expressão constituinte (por exemplo, um receptor, um argumento, um índice ou um operando) é sempre considerado o tipo de tempo de compilação dessa expressão.</span><span class="sxs-lookup"><span data-stu-id="dd693-202">When an operation is statically bound, the type of a constituent expression (e.g. a receiver, an argument, an index or an operand) is always considered to be the compile-time type of that expression.</span></span>

<span data-ttu-id="dd693-203">Quando uma operação é vinculada dinamicamente, o tipo de uma expressão constituinte é determinado de maneiras diferentes, dependendo do tipo de tempo de compilação da expressão constituinte:</span><span class="sxs-lookup"><span data-stu-id="dd693-203">When an operation is dynamically bound, the type of a constituent expression is determined in different ways depending on the compile-time type of the constituent expression:</span></span>

*  <span data-ttu-id="dd693-204">Uma expressão constituinte de tipo de tempo de compilação `dynamic` é considerada como tendo o tipo do valor real que a expressão avalia em tempo de execução</span><span class="sxs-lookup"><span data-stu-id="dd693-204">A constituent expression of compile-time type `dynamic` is considered to have the type of the actual value that the expression evaluates to at runtime</span></span>
*  <span data-ttu-id="dd693-205">Uma expressão constituinte cujo tipo de tempo de compilação é um parâmetro de tipo é considerado como tendo o tipo ao qual o parâmetro de tipo está associado em tempo de execução</span><span class="sxs-lookup"><span data-stu-id="dd693-205">A constituent expression whose compile-time type is a type parameter is considered to have the type which the type parameter is bound to at runtime</span></span>
*  <span data-ttu-id="dd693-206">Caso contrário, a expressão constituinte será considerada com seu tipo de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-206">Otherwise the constituent expression is considered to have its compile-time type.</span></span>

## <a name="operators"></a><span data-ttu-id="dd693-207">Operadores</span><span class="sxs-lookup"><span data-stu-id="dd693-207">Operators</span></span>

<span data-ttu-id="dd693-208">As expressões são construídas a partir de ***operandos*** e ***operadores***.</span><span class="sxs-lookup"><span data-stu-id="dd693-208">Expressions are constructed from ***operands*** and ***operators***.</span></span> <span data-ttu-id="dd693-209">Os operadores de uma expressão indicam quais operações devem ser aplicadas aos operandos.</span><span class="sxs-lookup"><span data-stu-id="dd693-209">The operators of an expression indicate which operations to apply to the operands.</span></span> <span data-ttu-id="dd693-210">Exemplos de operadores incluem `+`, `-`, `*`, `/` e `new`.</span><span class="sxs-lookup"><span data-stu-id="dd693-210">Examples of operators include `+`, `-`, `*`, `/`, and `new`.</span></span> <span data-ttu-id="dd693-211">Exemplos de operandos incluem literais, campos, variáveis locais e expressões.</span><span class="sxs-lookup"><span data-stu-id="dd693-211">Examples of operands include literals, fields, local variables, and expressions.</span></span>

<span data-ttu-id="dd693-212">Há três tipos de operadores:</span><span class="sxs-lookup"><span data-stu-id="dd693-212">There are three kinds of operators:</span></span>

*  <span data-ttu-id="dd693-213">Operadores unários.</span><span class="sxs-lookup"><span data-stu-id="dd693-213">Unary operators.</span></span> <span data-ttu-id="dd693-214">Os operadores unários usam um operando e usam a notação de prefixo (como `--x`) ou a notação de sufixo (como `x++`).</span><span class="sxs-lookup"><span data-stu-id="dd693-214">The unary operators take one operand and use either prefix notation (such as `--x`) or postfix notation (such as `x++`).</span></span>
*  <span data-ttu-id="dd693-215">Operadores binários.</span><span class="sxs-lookup"><span data-stu-id="dd693-215">Binary operators.</span></span> <span data-ttu-id="dd693-216">Os operadores binários usam dois operandos e todos usam notação de infixo (como `x + y`).</span><span class="sxs-lookup"><span data-stu-id="dd693-216">The binary operators take two operands and all use infix notation (such as `x + y`).</span></span>
*  <span data-ttu-id="dd693-217">Operador ternário.</span><span class="sxs-lookup"><span data-stu-id="dd693-217">Ternary operator.</span></span> <span data-ttu-id="dd693-218">Somente um operador ternário, `?:`, existe; Ele usa três operandos e usa a notação de infixo (`c ? x : y`).</span><span class="sxs-lookup"><span data-stu-id="dd693-218">Only one ternary operator, `?:`, exists; it takes three operands and uses infix notation (`c ? x : y`).</span></span>

<span data-ttu-id="dd693-219">A ordem de avaliação de operadores em uma expressão é determinada pela ***precedência*** e ***Associação*** dos operadores ([precedência de operador e Associação](expressions.md#operator-precedence-and-associativity)).</span><span class="sxs-lookup"><span data-stu-id="dd693-219">The order of evaluation of operators in an expression is determined by the ***precedence*** and ***associativity*** of the operators ([Operator precedence and associativity](expressions.md#operator-precedence-and-associativity)).</span></span>

<span data-ttu-id="dd693-220">Os operandos em uma expressão são avaliados da esquerda para a direita.</span><span class="sxs-lookup"><span data-stu-id="dd693-220">Operands in an expression are evaluated from left to right.</span></span> <span data-ttu-id="dd693-221">Por exemplo, em `F(i) + G(i++) * H(i)`, o método `F` é chamado usando o valor antigo de `i`, então o método `G` é chamado com o valor antigo de `i`e, finalmente, o método `H` é chamado com o novo valor de `i`.</span><span class="sxs-lookup"><span data-stu-id="dd693-221">For example, in `F(i) + G(i++) * H(i)`, method `F` is called using the old value of `i`, then method `G` is called with the old value of `i`, and, finally, method `H` is called with the new value of `i`.</span></span> <span data-ttu-id="dd693-222">Isso é separado de e não relacionado à precedência de operador.</span><span class="sxs-lookup"><span data-stu-id="dd693-222">This is separate from and unrelated to operator precedence.</span></span>

<span data-ttu-id="dd693-223">Determinados operadores podem ser ***sobrecarregados***.</span><span class="sxs-lookup"><span data-stu-id="dd693-223">Certain operators can be ***overloaded***.</span></span> <span data-ttu-id="dd693-224">A sobrecarga de operador permite que as implementações de operador definidas pelo usuário sejam especificadas para operações em que um ou ambos os operandos são de uma classe definida pelo usuário ou tipo struct ([sobrecarga de operador](expressions.md#operator-overloading)).</span><span class="sxs-lookup"><span data-stu-id="dd693-224">Operator overloading permits user-defined operator implementations to be specified for operations where one or both of the operands are of a user-defined class or struct type ([Operator overloading](expressions.md#operator-overloading)).</span></span>

### <a name="operator-precedence-and-associativity"></a><span data-ttu-id="dd693-225">Precedência e associatividade do operador</span><span class="sxs-lookup"><span data-stu-id="dd693-225">Operator precedence and associativity</span></span>

<span data-ttu-id="dd693-226">Quando uma expressão contiver vários operadores, a ***precedência*** dos operadores controla a ordem na qual os operadores individuais são avaliados.</span><span class="sxs-lookup"><span data-stu-id="dd693-226">When an expression contains multiple operators, the ***precedence*** of the operators controls the order in which the individual operators are evaluated.</span></span> <span data-ttu-id="dd693-227">Por exemplo, a expressão `x + y * z` é avaliada como `x + (y * z)` porque o operador de `*` tem precedência maior do que o operador binário `+`.</span><span class="sxs-lookup"><span data-stu-id="dd693-227">For example, the expression `x + y * z` is evaluated as `x + (y * z)` because the `*` operator has higher precedence than the binary `+` operator.</span></span> <span data-ttu-id="dd693-228">A precedência de um operador é estabelecida pela definição de sua produção de gramática associada.</span><span class="sxs-lookup"><span data-stu-id="dd693-228">The precedence of an operator is established by the definition of its associated grammar production.</span></span> <span data-ttu-id="dd693-229">Por exemplo, um *additive_expression* consiste em uma sequência de *multiplicative_expression*s separados por `+` ou operadores de `-`, permitindo que os operadores `+` e `-` diminuam a precedência dos operadores `*`, `/`e `%`.</span><span class="sxs-lookup"><span data-stu-id="dd693-229">For example, an *additive_expression* consists of a sequence of *multiplicative_expression*s separated by `+` or `-` operators, thus giving the `+` and `-` operators lower precedence than the `*`, `/`, and `%` operators.</span></span>

<span data-ttu-id="dd693-230">A tabela a seguir resume todos os operadores em ordem de precedência do mais alto para o mais baixo:</span><span class="sxs-lookup"><span data-stu-id="dd693-230">The following table summarizes all operators in order of precedence from highest to lowest:</span></span>

| <span data-ttu-id="dd693-231">__Section__</span><span class="sxs-lookup"><span data-stu-id="dd693-231">__Section__</span></span>                                                                                   | <span data-ttu-id="dd693-232">__Categoria__</span><span class="sxs-lookup"><span data-stu-id="dd693-232">__Category__</span></span>                | <span data-ttu-id="dd693-233">__Operadores__</span><span class="sxs-lookup"><span data-stu-id="dd693-233">__Operators__</span></span> | 
|-----------------------------------------------------------------------------------------------|-----------------------------|---------------|
| [<span data-ttu-id="dd693-234">Expressões primárias</span><span class="sxs-lookup"><span data-stu-id="dd693-234">Primary expressions</span></span>](expressions.md#primary-expressions)                                     | <span data-ttu-id="dd693-235">Primária</span><span class="sxs-lookup"><span data-stu-id="dd693-235">Primary</span></span>                     | <span data-ttu-id="dd693-236">`x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate`</span><span class="sxs-lookup"><span data-stu-id="dd693-236">`x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate`</span></span> | 
| [<span data-ttu-id="dd693-237">Operadores unários</span><span class="sxs-lookup"><span data-stu-id="dd693-237">Unary operators</span></span>](expressions.md#unary-operators)                                             | <span data-ttu-id="dd693-238">Unário</span><span class="sxs-lookup"><span data-stu-id="dd693-238">Unary</span></span>                       | <span data-ttu-id="dd693-239">`+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x`</span><span class="sxs-lookup"><span data-stu-id="dd693-239">`+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x`</span></span> | 
| [<span data-ttu-id="dd693-240">Operadores aritméticos</span><span class="sxs-lookup"><span data-stu-id="dd693-240">Arithmetic operators</span></span>](expressions.md#arithmetic-operators)                                   | <span data-ttu-id="dd693-241">Multiplicativo</span><span class="sxs-lookup"><span data-stu-id="dd693-241">Multiplicative</span></span>              | <span data-ttu-id="dd693-242">`*`  `/`  `%`</span><span class="sxs-lookup"><span data-stu-id="dd693-242">`*`  `/`  `%`</span></span> | 
| [<span data-ttu-id="dd693-243">Operadores aritméticos</span><span class="sxs-lookup"><span data-stu-id="dd693-243">Arithmetic operators</span></span>](expressions.md#arithmetic-operators)                                   | <span data-ttu-id="dd693-244">Aditivo</span><span class="sxs-lookup"><span data-stu-id="dd693-244">Additive</span></span>                    | <span data-ttu-id="dd693-245">`+`  `-`</span><span class="sxs-lookup"><span data-stu-id="dd693-245">`+`  `-`</span></span>      | 
| [<span data-ttu-id="dd693-246">Operadores shift</span><span class="sxs-lookup"><span data-stu-id="dd693-246">Shift operators</span></span>](expressions.md#shift-operators)                                             | <span data-ttu-id="dd693-247">Shift</span><span class="sxs-lookup"><span data-stu-id="dd693-247">Shift</span></span>                       | <span data-ttu-id="dd693-248">`<<`  `>>`</span><span class="sxs-lookup"><span data-stu-id="dd693-248">`<<`  `>>`</span></span>    | 
| [<span data-ttu-id="dd693-249">Operadores de teste de tipo e relacional</span><span class="sxs-lookup"><span data-stu-id="dd693-249">Relational and type-testing operators</span></span>](expressions.md#relational-and-type-testing-operators) | <span data-ttu-id="dd693-250">Teste de tipo e relacional</span><span class="sxs-lookup"><span data-stu-id="dd693-250">Relational and type testing</span></span> | <span data-ttu-id="dd693-251">`<`  `>`  `<=`  `>=`  `is`  `as`</span><span class="sxs-lookup"><span data-stu-id="dd693-251">`<`  `>`  `<=`  `>=`  `is`  `as`</span></span> | 
| [<span data-ttu-id="dd693-252">Operadores de teste de tipo e relacional</span><span class="sxs-lookup"><span data-stu-id="dd693-252">Relational and type-testing operators</span></span>](expressions.md#relational-and-type-testing-operators) | <span data-ttu-id="dd693-253">Igualdade</span><span class="sxs-lookup"><span data-stu-id="dd693-253">Equality</span></span>                    | <span data-ttu-id="dd693-254">`==`  `!=`</span><span class="sxs-lookup"><span data-stu-id="dd693-254">`==`  `!=`</span></span>    | 
| [<span data-ttu-id="dd693-255">Operadores lógicos</span><span class="sxs-lookup"><span data-stu-id="dd693-255">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="dd693-256">AND lógico</span><span class="sxs-lookup"><span data-stu-id="dd693-256">Logical AND</span></span>                 | `&`           | 
| [<span data-ttu-id="dd693-257">Operadores lógicos</span><span class="sxs-lookup"><span data-stu-id="dd693-257">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="dd693-258">XOR lógico</span><span class="sxs-lookup"><span data-stu-id="dd693-258">Logical XOR</span></span>                 | `^`           | 
| [<span data-ttu-id="dd693-259">Operadores lógicos</span><span class="sxs-lookup"><span data-stu-id="dd693-259">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="dd693-260">OR lógico</span><span class="sxs-lookup"><span data-stu-id="dd693-260">Logical OR</span></span>                  | <code>&#124;</code>           |
| [<span data-ttu-id="dd693-261">Operadores lógicos condicionais</span><span class="sxs-lookup"><span data-stu-id="dd693-261">Conditional logical operators</span></span>](expressions.md#conditional-logical-operators)                 | <span data-ttu-id="dd693-262">AND condicional</span><span class="sxs-lookup"><span data-stu-id="dd693-262">Conditional AND</span></span>             | `&&`          | 
| [<span data-ttu-id="dd693-263">Operadores lógicos condicionais</span><span class="sxs-lookup"><span data-stu-id="dd693-263">Conditional logical operators</span></span>](expressions.md#conditional-logical-operators)                 | <span data-ttu-id="dd693-264">OR condicional</span><span class="sxs-lookup"><span data-stu-id="dd693-264">Conditional OR</span></span>              | <code>&#124;&#124;</code>          | 
| [<span data-ttu-id="dd693-265">O operador de coalescência nula</span><span class="sxs-lookup"><span data-stu-id="dd693-265">The null coalescing operator</span></span>](expressions.md#the-null-coalescing-operator)                   | <span data-ttu-id="dd693-266">Coalescência nula</span><span class="sxs-lookup"><span data-stu-id="dd693-266">Null coalescing</span></span>             | `??`          | 
| [<span data-ttu-id="dd693-267">Operador condicional</span><span class="sxs-lookup"><span data-stu-id="dd693-267">Conditional operator</span></span>](expressions.md#conditional-operator)                                   | <span data-ttu-id="dd693-268">Condicional</span><span class="sxs-lookup"><span data-stu-id="dd693-268">Conditional</span></span>                 | `?:`          | 
| <span data-ttu-id="dd693-269">[Operadores de atribuição](expressions.md#assignment-operators), [expressões de função anônimas](expressions.md#anonymous-function-expressions)</span><span class="sxs-lookup"><span data-stu-id="dd693-269">[Assignment operators](expressions.md#assignment-operators), [Anonymous function expressions](expressions.md#anonymous-function-expressions)</span></span>  | <span data-ttu-id="dd693-270">Atribuição e expressão lambda</span><span class="sxs-lookup"><span data-stu-id="dd693-270">Assignment and lambda expression</span></span> | <span data-ttu-id="dd693-271">`=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>`</span><span class="sxs-lookup"><span data-stu-id="dd693-271">`=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>`</span></span> | 

<span data-ttu-id="dd693-272">Quando um operando ocorre entre dois operadores com a mesma precedência, a Associação dos operadores controla a ordem na qual as operações são executadas:</span><span class="sxs-lookup"><span data-stu-id="dd693-272">When an operand occurs between two operators with the same precedence, the associativity of the operators controls the order in which the operations are performed:</span></span>

*  <span data-ttu-id="dd693-273">Exceto para os operadores de atribuição e o operador de União nulo, todos os operadores binários são ***associativos à esquerda***, o que significa que as operações são executadas da esquerda para a direita.</span><span class="sxs-lookup"><span data-stu-id="dd693-273">Except for the assignment operators and the null coalescing operator, all binary operators are ***left-associative***, meaning that operations are performed from left to right.</span></span> <span data-ttu-id="dd693-274">Por exemplo, `x + y + z` é avaliado como `(x + y) + z`.</span><span class="sxs-lookup"><span data-stu-id="dd693-274">For example, `x + y + z` is evaluated as `(x + y) + z`.</span></span>
*  <span data-ttu-id="dd693-275">Os operadores de atribuição, o operador de União nulo e o operador condicional (`?:`) são ***associativos à direita***, o que significa que as operações são executadas da direita para a esquerda.</span><span class="sxs-lookup"><span data-stu-id="dd693-275">The assignment operators, the null coalescing operator and the conditional operator (`?:`) are ***right-associative***, meaning that operations are performed from right to left.</span></span> <span data-ttu-id="dd693-276">Por exemplo, `x = y = z` é avaliado como `x = (y = z)`.</span><span class="sxs-lookup"><span data-stu-id="dd693-276">For example, `x = y = z` is evaluated as `x = (y = z)`.</span></span>

<span data-ttu-id="dd693-277">Precedência e associatividade podem ser controladas usando parênteses.</span><span class="sxs-lookup"><span data-stu-id="dd693-277">Precedence and associativity can be controlled using parentheses.</span></span> <span data-ttu-id="dd693-278">Por exemplo, `x + y * z` primeiro multiplica `y` por `z` e, em seguida, adiciona o resultado a `x`, mas `(x + y) * z` primeiro adiciona `x` e `y` e, em seguida, multiplica o resultado por `z`.</span><span class="sxs-lookup"><span data-stu-id="dd693-278">For example, `x + y * z` first multiplies `y` by `z` and then adds the result to `x`, but `(x + y) * z` first adds `x` and `y` and then multiplies the result by `z`.</span></span>

### <a name="operator-overloading"></a><span data-ttu-id="dd693-279">Sobrecarga de operador</span><span class="sxs-lookup"><span data-stu-id="dd693-279">Operator overloading</span></span>

<span data-ttu-id="dd693-280">Todos os operadores unários e binários têm implementações predefinidas que estão automaticamente disponíveis em qualquer expressão.</span><span class="sxs-lookup"><span data-stu-id="dd693-280">All unary and binary operators have predefined implementations that are automatically available in any expression.</span></span> <span data-ttu-id="dd693-281">Além das implementações predefinidas, as implementações definidas pelo usuário podem ser introduzidas incluindo `operator` declarações em classes e Structs ([operadores](classes.md#operators)).</span><span class="sxs-lookup"><span data-stu-id="dd693-281">In addition to the predefined implementations, user-defined implementations can be introduced by including `operator` declarations in classes and structs ([Operators](classes.md#operators)).</span></span> <span data-ttu-id="dd693-282">Implementações de operador definidas pelo usuário sempre têm precedência sobre implementações de operador predefinidas: somente quando não houver nenhuma implementação de operador definida pelo usuário aplicável as implementações de operador predefinidas serão consideradas, conforme descrito em resolução de [sobrecarga de operador unário](expressions.md#unary-operator-overload-resolution) e [resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="dd693-282">User-defined operator implementations always take precedence over predefined operator implementations: Only when no applicable user-defined operator implementations exist will the predefined operator implementations be considered, as described in [Unary operator overload resolution](expressions.md#unary-operator-overload-resolution) and [Binary operator overload resolution](expressions.md#binary-operator-overload-resolution).</span></span>

<span data-ttu-id="dd693-283">Os ***operadores unários sobrecarregados*** são:</span><span class="sxs-lookup"><span data-stu-id="dd693-283">The ***overloadable unary operators*** are:</span></span>
```csharp
+   -   !   ~   ++   --   true   false
```

<span data-ttu-id="dd693-284">Embora `true` e `false` não sejam usados explicitamente em expressões (e, portanto, não estejam incluídos na tabela de precedência em [precedência de operador e Associação](expressions.md#operator-precedence-and-associativity)), eles são considerados operadores porque são invocados em vários contextos de expressão: expressões boolianas ([expressões booleanas](expressions.md#boolean-expressions)) e expressões que envolvem o condicional ([operador condicional](expressions.md#conditional-operator)) e operadores lógicos condicionais ([operadores lógicos condicional](expressions.md#conditional-logical-operators)).</span><span class="sxs-lookup"><span data-stu-id="dd693-284">Although `true` and `false` are not used explicitly in expressions (and therefore are not included in the precedence table in [Operator precedence and associativity](expressions.md#operator-precedence-and-associativity)), they are considered operators because they are invoked in several expression contexts: boolean expressions ([Boolean expressions](expressions.md#boolean-expressions)) and expressions involving the conditional ([Conditional operator](expressions.md#conditional-operator)), and conditional logical operators ([Conditional logical operators](expressions.md#conditional-logical-operators)).</span></span>

<span data-ttu-id="dd693-285">Os ***operadores binários sobrecarregados*** são:</span><span class="sxs-lookup"><span data-stu-id="dd693-285">The ***overloadable binary operators*** are:</span></span>
```csharp
+   -   *   /   %   &   |   ^   <<   >>   ==   !=   >   <   >=   <=
```

<span data-ttu-id="dd693-286">Somente os operadores listados acima podem ser sobrecarregados.</span><span class="sxs-lookup"><span data-stu-id="dd693-286">Only the operators listed above can be overloaded.</span></span> <span data-ttu-id="dd693-287">Em particular, não é possível sobrecarregar o acesso de membro, a invocação de método ou os operadores `=`, `&&`, `||`, `??`, `?:`, `=>`, `checked`, `unchecked`, `new`, `typeof`, `default`, `as`e `is`.</span><span class="sxs-lookup"><span data-stu-id="dd693-287">In particular, it is not possible to overload member access, method invocation, or the `=`, `&&`, `||`, `??`, `?:`, `=>`, `checked`, `unchecked`, `new`, `typeof`, `default`, `as`, and `is` operators.</span></span>

<span data-ttu-id="dd693-288">Quando um operador binário está sobrecarregado, o operador de atribuição correspondente, se houver, também estará implicitamente sobrecarregado.</span><span class="sxs-lookup"><span data-stu-id="dd693-288">When a binary operator is overloaded, the corresponding assignment operator, if any, is also implicitly overloaded.</span></span> <span data-ttu-id="dd693-289">Por exemplo, uma sobrecarga de operador `*` também é uma sobrecarga de operador `*=`.</span><span class="sxs-lookup"><span data-stu-id="dd693-289">For example, an overload of operator `*` is also an overload of operator `*=`.</span></span> <span data-ttu-id="dd693-290">Isso é descrito mais detalhadamente na [atribuição composta](expressions.md#compound-assignment).</span><span class="sxs-lookup"><span data-stu-id="dd693-290">This is described further in [Compound assignment](expressions.md#compound-assignment).</span></span> <span data-ttu-id="dd693-291">Observe que o próprio operador de atribuição (`=`) não pode ser sobrecarregado.</span><span class="sxs-lookup"><span data-stu-id="dd693-291">Note that the assignment operator itself (`=`) cannot be overloaded.</span></span> <span data-ttu-id="dd693-292">Uma atribuição sempre executa uma cópia de bits simples de um valor em uma variável.</span><span class="sxs-lookup"><span data-stu-id="dd693-292">An assignment always performs a simple bit-wise copy of a value into a variable.</span></span>

<span data-ttu-id="dd693-293">As operações de conversão, como `(T)x`, são sobrecarregadas fornecendo conversões definidas pelo usuário ([conversões definidas pelo usuário](conversions.md#user-defined-conversions)).</span><span class="sxs-lookup"><span data-stu-id="dd693-293">Cast operations, such as `(T)x`, are overloaded by providing user-defined conversions ([User-defined conversions](conversions.md#user-defined-conversions)).</span></span>

<span data-ttu-id="dd693-294">O acesso ao elemento, como `a[x]`, não é considerado um operador sobrecarregado.</span><span class="sxs-lookup"><span data-stu-id="dd693-294">Element access, such as `a[x]`, is not considered an overloadable operator.</span></span> <span data-ttu-id="dd693-295">Em vez disso, há suporte para a indexação definida pelo usuário por meio de indexadores ([indexadores](classes.md#indexers)).</span><span class="sxs-lookup"><span data-stu-id="dd693-295">Instead, user-defined indexing is supported through indexers ([Indexers](classes.md#indexers)).</span></span>

<span data-ttu-id="dd693-296">Em expressões, os operadores são referenciados usando a notação de operador e, em declarações, os operadores são referenciados usando a notação funcional.</span><span class="sxs-lookup"><span data-stu-id="dd693-296">In expressions, operators are referenced using operator notation, and in declarations, operators are referenced using functional notation.</span></span> <span data-ttu-id="dd693-297">A tabela a seguir mostra a relação entre as notações de operador e funcional para operadores unários e binários.</span><span class="sxs-lookup"><span data-stu-id="dd693-297">The following table shows the relationship between operator and functional notations for unary and binary operators.</span></span> <span data-ttu-id="dd693-298">Na primeira entrada, *op* denota qualquer operador de prefixo unário sobrecarregado.</span><span class="sxs-lookup"><span data-stu-id="dd693-298">In the first entry, *op* denotes any overloadable unary prefix operator.</span></span> <span data-ttu-id="dd693-299">Na segunda entrada, *op* denota o sufixo unário `++` e os operadores de `--`.</span><span class="sxs-lookup"><span data-stu-id="dd693-299">In the second entry, *op* denotes the unary postfix `++` and `--` operators.</span></span> <span data-ttu-id="dd693-300">Na terceira entrada, *op* denota qualquer operador binário sobrecarregado.</span><span class="sxs-lookup"><span data-stu-id="dd693-300">In the third entry, *op* denotes any overloadable binary operator.</span></span>


| <span data-ttu-id="dd693-301">__Notação de operador__</span><span class="sxs-lookup"><span data-stu-id="dd693-301">__Operator notation__</span></span> | <span data-ttu-id="dd693-302">__Notação funcional__</span><span class="sxs-lookup"><span data-stu-id="dd693-302">__Functional notation__</span></span> |
|-----------------------|-------------------------|
| `op x`                | `operator op(x)`        | 
| `x op`                | `operator op(x)`        | 
| `x op y`              | `operator op(x,y)`      | 

<span data-ttu-id="dd693-303">As declarações de operador definidas pelo usuário sempre exigem que pelo menos um dos parâmetros seja do tipo de classe ou struct que contém a declaração do operador.</span><span class="sxs-lookup"><span data-stu-id="dd693-303">User-defined operator declarations always require at least one of the parameters to be of the class or struct type that contains the operator declaration.</span></span> <span data-ttu-id="dd693-304">Portanto, não é possível que um operador definido pelo usuário tenha a mesma assinatura que um operador predefinido.</span><span class="sxs-lookup"><span data-stu-id="dd693-304">Thus, it is not possible for a user-defined operator to have the same signature as a predefined operator.</span></span>

<span data-ttu-id="dd693-305">As declarações do operador definidas pelo usuário não podem modificar a sintaxe, a precedência ou a associação de um operador.</span><span class="sxs-lookup"><span data-stu-id="dd693-305">User-defined operator declarations cannot modify the syntax, precedence, or associativity of an operator.</span></span> <span data-ttu-id="dd693-306">Por exemplo, o operador de `/` é sempre um operador binário, sempre tem o nível de precedência especificado em [precedência de operador e Associação](expressions.md#operator-precedence-and-associativity), e é sempre associativo à esquerda.</span><span class="sxs-lookup"><span data-stu-id="dd693-306">For example, the `/` operator is always a binary operator, always has the precedence level specified in [Operator precedence and associativity](expressions.md#operator-precedence-and-associativity), and is always left-associative.</span></span>

<span data-ttu-id="dd693-307">Embora seja possível que um operador definido pelo usuário execute qualquer computação, as implementações que produzem resultados diferentes daqueles que são intuitivos esperados são altamente desencorajadas.</span><span class="sxs-lookup"><span data-stu-id="dd693-307">While it is possible for a user-defined operator to perform any computation it pleases, implementations that produce results other than those that are intuitively expected are strongly discouraged.</span></span> <span data-ttu-id="dd693-308">Por exemplo, uma implementação de `operator ==` deve comparar os dois operandos para igualdade e retornar um resultado de `bool` apropriado.</span><span class="sxs-lookup"><span data-stu-id="dd693-308">For example, an implementation of `operator ==` should compare the two operands for equality and return an appropriate `bool` result.</span></span>

<span data-ttu-id="dd693-309">As descrições de operadores individuais em [expressões primárias](expressions.md#primary-expressions) por meio de [operadores lógicos condicionais](expressions.md#conditional-logical-operators) especificam as implementações predefinidas dos operadores e quaisquer regras adicionais que se aplicam a cada operador.</span><span class="sxs-lookup"><span data-stu-id="dd693-309">The descriptions of individual operators in [Primary expressions](expressions.md#primary-expressions) through [Conditional logical operators](expressions.md#conditional-logical-operators) specify the predefined implementations of the operators and any additional rules that apply to each operator.</span></span> <span data-ttu-id="dd693-310">As descrições fazem uso dos termos ***resolução de sobrecarga de operador unário***, ***resolução de sobrecarga de operador binário***e ***promoção numérica***, definições das quais são encontradas nas seções a seguir.</span><span class="sxs-lookup"><span data-stu-id="dd693-310">The descriptions make use of the terms ***unary operator overload resolution***, ***binary operator overload resolution***, and ***numeric promotion***, definitions of which are found in the following sections.</span></span>

### <a name="unary-operator-overload-resolution"></a><span data-ttu-id="dd693-311">Resolução de sobrecarga de operador unário</span><span class="sxs-lookup"><span data-stu-id="dd693-311">Unary operator overload resolution</span></span>

<span data-ttu-id="dd693-312">Uma operação do formulário `op x` ou `x op`, em que `op` é um operador unário sobrecarregado e `x` é uma expressão do tipo `X`, é processada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="dd693-312">An operation of the form `op x` or `x op`, where `op` is an overloadable unary operator, and `x` is an expression of type `X`, is processed as follows:</span></span>

*  <span data-ttu-id="dd693-313">O conjunto de operadores candidatos definidos pelo usuário fornecido pelo `X` para a operação `operator op(x)` é determinado usando as regras de [operadores do candidato definidos pelo usuário](expressions.md#candidate-user-defined-operators).</span><span class="sxs-lookup"><span data-stu-id="dd693-313">The set of candidate user-defined operators provided by `X` for the operation `operator op(x)` is determined using the rules of [Candidate user-defined operators](expressions.md#candidate-user-defined-operators).</span></span>
*  <span data-ttu-id="dd693-314">Se o conjunto de operadores candidatos definidos pelo usuário não estiver vazio, isso se tornará o conjunto de operadores candidatos para a operação.</span><span class="sxs-lookup"><span data-stu-id="dd693-314">If the set of candidate user-defined operators is not empty, then this becomes the set of candidate operators for the operation.</span></span> <span data-ttu-id="dd693-315">Caso contrário, as implementações de `operator op` unário predefinidas, incluindo suas formas comparadas, se tornarão o conjunto de operadores candidatos para a operação.</span><span class="sxs-lookup"><span data-stu-id="dd693-315">Otherwise, the predefined unary `operator op` implementations, including their lifted forms, become the set of candidate operators for the operation.</span></span> <span data-ttu-id="dd693-316">As implementações predefinidas de um determinado operador são especificadas na descrição do operador ([expressões primárias](expressions.md#primary-expressions) e [operadores unários](expressions.md#unary-operators)).</span><span class="sxs-lookup"><span data-stu-id="dd693-316">The predefined implementations of a given operator are specified in the description of the operator ([Primary expressions](expressions.md#primary-expressions) and [Unary operators](expressions.md#unary-operators)).</span></span>
*  <span data-ttu-id="dd693-317">As regras de resolução de sobrecarga da [resolução de sobrecarga](expressions.md#overload-resolution) são aplicadas ao conjunto de operadores candidatos para selecionar o melhor operador em relação à lista de argumentos `(x)`, e esse operador se torna o resultado do processo de resolução de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="dd693-317">The overload resolution rules of [Overload resolution](expressions.md#overload-resolution) are applied to the set of candidate operators to select the best operator with respect to the argument list `(x)`, and this operator becomes the result of the overload resolution process.</span></span> <span data-ttu-id="dd693-318">Se a resolução de sobrecarga falhar ao selecionar um único operador melhor, ocorrerá um erro de tempo de associação.</span><span class="sxs-lookup"><span data-stu-id="dd693-318">If overload resolution fails to select a single best operator, a binding-time error occurs.</span></span>

### <a name="binary-operator-overload-resolution"></a><span data-ttu-id="dd693-319">Resolução de sobrecarga de operador binário</span><span class="sxs-lookup"><span data-stu-id="dd693-319">Binary operator overload resolution</span></span>

<span data-ttu-id="dd693-320">Uma operação do formulário `x op y`, em que `op` é um operador binário sobrecarregável, `x` é uma expressão do tipo `X`e `y` é uma expressão do tipo `Y`, é processada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="dd693-320">An operation of the form `x op y`, where `op` is an overloadable binary operator, `x` is an expression of type `X`, and `y` is an expression of type `Y`, is processed as follows:</span></span>

*  <span data-ttu-id="dd693-321">O conjunto de operadores candidatos definidos pelo usuário fornecido pelo `X` e `Y` para a operação `operator op(x,y)` é determinado.</span><span class="sxs-lookup"><span data-stu-id="dd693-321">The set of candidate user-defined operators provided by `X` and `Y` for the operation `operator op(x,y)` is determined.</span></span> <span data-ttu-id="dd693-322">O conjunto consiste na União dos operadores candidatos fornecidos por `X` e nos operadores candidatos fornecidos pelo `Y`, cada um determinado usando as regras de [operadores definidos pelo usuário candidatos](expressions.md#candidate-user-defined-operators).</span><span class="sxs-lookup"><span data-stu-id="dd693-322">The set consists of the union of the candidate operators provided by `X` and the candidate operators provided by `Y`, each determined using the rules of [Candidate user-defined operators](expressions.md#candidate-user-defined-operators).</span></span> <span data-ttu-id="dd693-323">Se `X` e `Y` forem do mesmo tipo, ou se `X` e `Y` forem derivados de um tipo base comum, os operadores de candidato compartilhados só ocorrerão no conjunto combinado uma vez.</span><span class="sxs-lookup"><span data-stu-id="dd693-323">If `X` and `Y` are the same type, or if `X` and `Y` are derived from a common base type, then shared candidate operators only occur in the combined set once.</span></span>
*  <span data-ttu-id="dd693-324">Se o conjunto de operadores candidatos definidos pelo usuário não estiver vazio, isso se tornará o conjunto de operadores candidatos para a operação.</span><span class="sxs-lookup"><span data-stu-id="dd693-324">If the set of candidate user-defined operators is not empty, then this becomes the set of candidate operators for the operation.</span></span> <span data-ttu-id="dd693-325">Caso contrário, o binário predefinido `operator op` implementações, incluindo seus formulários levantados, se tornará o conjunto de operadores candidatos para a operação.</span><span class="sxs-lookup"><span data-stu-id="dd693-325">Otherwise, the predefined binary `operator op` implementations, including their lifted forms,  become the set of candidate operators for the operation.</span></span> <span data-ttu-id="dd693-326">As implementações predefinidas de um determinado operador são especificadas na descrição do operador ([operadores aritméticos](expressions.md#arithmetic-operators) por meio de [operadores lógicos condicionais](expressions.md#conditional-logical-operators)).</span><span class="sxs-lookup"><span data-stu-id="dd693-326">The predefined implementations of a given operator are specified in the description of the operator ([Arithmetic operators](expressions.md#arithmetic-operators) through [Conditional logical operators](expressions.md#conditional-logical-operators)).</span></span> <span data-ttu-id="dd693-327">Para operadores enum e delegate predefinidos, os únicos operadores considerados são aqueles definidos por um tipo de enumeração ou de delegado que é o tipo de tempo de associação de um dos operandos.</span><span class="sxs-lookup"><span data-stu-id="dd693-327">For predefined enum and delegate operators, the only operators considered are those defined by an enum or delegate type that is the binding-time type of one of the operands.</span></span>
*  <span data-ttu-id="dd693-328">As regras de resolução de sobrecarga da [resolução de sobrecarga](expressions.md#overload-resolution) são aplicadas ao conjunto de operadores candidatos para selecionar o melhor operador em relação à lista de argumentos `(x,y)`, e esse operador se torna o resultado do processo de resolução de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="dd693-328">The overload resolution rules of [Overload resolution](expressions.md#overload-resolution) are applied to the set of candidate operators to select the best operator with respect to the argument list `(x,y)`, and this operator becomes the result of the overload resolution process.</span></span> <span data-ttu-id="dd693-329">Se a resolução de sobrecarga falhar ao selecionar um único operador melhor, ocorrerá um erro de tempo de associação.</span><span class="sxs-lookup"><span data-stu-id="dd693-329">If overload resolution fails to select a single best operator, a binding-time error occurs.</span></span>

### <a name="candidate-user-defined-operators"></a><span data-ttu-id="dd693-330">Operadores definidos pelo usuário candidatos</span><span class="sxs-lookup"><span data-stu-id="dd693-330">Candidate user-defined operators</span></span>

<span data-ttu-id="dd693-331">Dado um tipo `T` e uma operação `operator op(A)`, em que `op` é um operador que está sendo sobrecarregado e `A` é uma lista de argumentos, o conjunto de operadores candidatos definidos pelo usuário fornecido pelo `T` para `operator op(A)` é determinado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="dd693-331">Given a type `T` and an operation `operator op(A)`, where `op` is an overloadable operator and `A` is an argument list, the set of candidate user-defined operators provided by `T` for `operator op(A)` is determined as follows:</span></span>

*  <span data-ttu-id="dd693-332">Determine o tipo `T0`.</span><span class="sxs-lookup"><span data-stu-id="dd693-332">Determine the type `T0`.</span></span> <span data-ttu-id="dd693-333">Se `T` for um tipo anulável, `T0` será seu tipo subjacente, caso contrário `T0` será igual a `T`.</span><span class="sxs-lookup"><span data-stu-id="dd693-333">If `T` is a nullable type, `T0` is its underlying type, otherwise `T0` is equal to `T`.</span></span>
*  <span data-ttu-id="dd693-334">Para todas as declarações de `operator op` em `T0` e todas as formas levantadas desses operadores, se pelo menos um operador for aplicável ([membro de função aplicável](expressions.md#applicable-function-member)) em relação à lista de argumentos `A`, o conjunto de operadores candidatos consiste em todos esses operadores aplicáveis no `T0`.</span><span class="sxs-lookup"><span data-stu-id="dd693-334">For all `operator op` declarations in `T0` and all lifted forms of such operators, if at least one operator is applicable ([Applicable function member](expressions.md#applicable-function-member)) with respect to the argument list `A`, then the set of candidate operators consists of all such applicable operators in `T0`.</span></span>
*  <span data-ttu-id="dd693-335">Caso contrário, se `T0` for `object`, o conjunto de operadores candidatos estará vazio.</span><span class="sxs-lookup"><span data-stu-id="dd693-335">Otherwise, if `T0` is `object`, the set of candidate operators is empty.</span></span>
*  <span data-ttu-id="dd693-336">Caso contrário, o conjunto de operadores candidatos fornecidos pelo `T0` é o conjunto de operadores candidatos fornecidos pela classe base direta de `T0`ou a classe base efetiva de `T0` se `T0` for um parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="dd693-336">Otherwise, the set of candidate operators provided by `T0` is the set of candidate operators provided by the direct base class of `T0`, or the effective base class of `T0` if `T0` is a type parameter.</span></span>

### <a name="numeric-promotions"></a><span data-ttu-id="dd693-337">Promoções numéricas</span><span class="sxs-lookup"><span data-stu-id="dd693-337">Numeric promotions</span></span>

<span data-ttu-id="dd693-338">A promoção numérica consiste em executar automaticamente determinadas conversões implícitas dos operandos dos operadores numéricos unários e binários predefinidos.</span><span class="sxs-lookup"><span data-stu-id="dd693-338">Numeric promotion consists of automatically performing certain implicit conversions of the operands of the predefined unary and binary numeric operators.</span></span> <span data-ttu-id="dd693-339">A promoção numérica não é um mecanismo distinto, mas sim um efeito de aplicar a resolução de sobrecarga aos operadores predefinidos.</span><span class="sxs-lookup"><span data-stu-id="dd693-339">Numeric promotion is not a distinct mechanism, but rather an effect of applying overload resolution to the predefined operators.</span></span> <span data-ttu-id="dd693-340">A promoção numérica especificamente não afeta a avaliação de operadores definidos pelo usuário, embora os operadores definidos pelo usuário possam ser implementados para exibir efeitos semelhantes.</span><span class="sxs-lookup"><span data-stu-id="dd693-340">Numeric promotion specifically does not affect evaluation of user-defined operators, although user-defined operators can be implemented to exhibit similar effects.</span></span>

<span data-ttu-id="dd693-341">Como exemplo de promoção numérica, considere as implementações predefinidas do operador binário `*`:</span><span class="sxs-lookup"><span data-stu-id="dd693-341">As an example of numeric promotion, consider the predefined implementations of the binary `*` operator:</span></span>

```csharp
int operator *(int x, int y);
uint operator *(uint x, uint y);
long operator *(long x, long y);
ulong operator *(ulong x, ulong y);
float operator *(float x, float y);
double operator *(double x, double y);
decimal operator *(decimal x, decimal y);
```

<span data-ttu-id="dd693-342">Quando as regras de resolução de sobrecarga ([resolução de sobrecarga](expressions.md#overload-resolution)) são aplicadas a esse conjunto de operadores, o efeito é selecionar o primeiro dos operadores para os quais conversões implícitas existem nos tipos de operando.</span><span class="sxs-lookup"><span data-stu-id="dd693-342">When overload resolution rules ([Overload resolution](expressions.md#overload-resolution)) are applied to this set of operators, the effect is to select the first of the operators for which implicit conversions exist from the operand types.</span></span> <span data-ttu-id="dd693-343">Por exemplo, para a operação `b * s`, em que `b` é um `byte` e `s` é um `short`, a resolução de sobrecarga seleciona `operator *(int,int)` como o melhor operador.</span><span class="sxs-lookup"><span data-stu-id="dd693-343">For example, for the operation `b * s`, where `b` is a `byte` and `s` is a `short`, overload resolution selects `operator *(int,int)` as the best operator.</span></span> <span data-ttu-id="dd693-344">Assim, o efeito é que `b` e `s` são convertidos em `int`, e o tipo do resultado é `int`.</span><span class="sxs-lookup"><span data-stu-id="dd693-344">Thus, the effect is that `b` and `s` are converted to `int`, and the type of the result is `int`.</span></span> <span data-ttu-id="dd693-345">Da mesma forma, para a operação `i * d`, em que `i` é um `int` e `d` é um `double`, a resolução de sobrecarga seleciona `operator *(double,double)` como o melhor operador.</span><span class="sxs-lookup"><span data-stu-id="dd693-345">Likewise, for the operation `i * d`, where `i` is an `int` and `d` is a `double`, overload resolution selects `operator *(double,double)` as the best operator.</span></span>

#### <a name="unary-numeric-promotions"></a><span data-ttu-id="dd693-346">Promoções numéricas unários</span><span class="sxs-lookup"><span data-stu-id="dd693-346">Unary numeric promotions</span></span>

<span data-ttu-id="dd693-347">A promoção numérica unário ocorre para os operandos dos operadores predefinidos `+`, `-`e `~` unários.</span><span class="sxs-lookup"><span data-stu-id="dd693-347">Unary numeric promotion occurs for the operands of the predefined `+`, `-`, and `~` unary operators.</span></span> <span data-ttu-id="dd693-348">A promoção numérica unário simplesmente consiste na conversão de operandos do tipo `sbyte`, `byte`, `short`, `ushort`ou `char` para o tipo `int`.</span><span class="sxs-lookup"><span data-stu-id="dd693-348">Unary numeric promotion simply consists of converting operands of type `sbyte`, `byte`, `short`, `ushort`, or `char` to type `int`.</span></span> <span data-ttu-id="dd693-349">Além disso, para o operador de `-` unário, a promoção numérica unário converte os operandos do tipo `uint` para o tipo `long`.</span><span class="sxs-lookup"><span data-stu-id="dd693-349">Additionally, for the unary `-` operator, unary numeric promotion converts operands of type `uint` to type `long`.</span></span>

#### <a name="binary-numeric-promotions"></a><span data-ttu-id="dd693-350">Promoções numéricas binárias</span><span class="sxs-lookup"><span data-stu-id="dd693-350">Binary numeric promotions</span></span>

<span data-ttu-id="dd693-351">A promoção numérica binária ocorre para os operandos dos operadores predefinidos `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `==`, `!=`, `>`, `<`, `>=`e `<=` binários.</span><span class="sxs-lookup"><span data-stu-id="dd693-351">Binary numeric promotion occurs for the operands of the predefined `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `==`, `!=`, `>`, `<`, `>=`, and `<=` binary operators.</span></span> <span data-ttu-id="dd693-352">A promoção numérica binária converte implicitamente os dois operandos em um tipo comum que, no caso dos operadores não relacionais, também se torna o tipo de resultado da operação.</span><span class="sxs-lookup"><span data-stu-id="dd693-352">Binary numeric promotion implicitly converts both operands to a common type which, in case of the non-relational operators, also becomes the result type of the operation.</span></span> <span data-ttu-id="dd693-353">A promoção numérica binária consiste na aplicação das seguintes regras, na ordem em que aparecem aqui:</span><span class="sxs-lookup"><span data-stu-id="dd693-353">Binary numeric promotion consists of applying the following rules, in the order they appear here:</span></span>

*  <span data-ttu-id="dd693-354">Se um dos operandos for do tipo `decimal`, o outro operando será convertido no tipo `decimal`ou ocorrerá um erro de tempo de ligação se o outro operando for do tipo `float` ou `double`.</span><span class="sxs-lookup"><span data-stu-id="dd693-354">If either operand is of type `decimal`, the other operand is converted to type `decimal`, or a binding-time error occurs if the other operand is of type `float` or `double`.</span></span>
*  <span data-ttu-id="dd693-355">Caso contrário, se um dos operandos for do tipo `double`, o outro operando será convertido no tipo `double`.</span><span class="sxs-lookup"><span data-stu-id="dd693-355">Otherwise, if either operand is of type `double`, the other operand is converted to type `double`.</span></span>
*  <span data-ttu-id="dd693-356">Caso contrário, se um dos operandos for do tipo `float`, o outro operando será convertido no tipo `float`.</span><span class="sxs-lookup"><span data-stu-id="dd693-356">Otherwise, if either operand is of type `float`, the other operand is converted to type `float`.</span></span>
*  <span data-ttu-id="dd693-357">Caso contrário, se qualquer operando for do tipo `ulong`, o outro operando será convertido no tipo `ulong`, ou ocorrerá um erro de tempo de associação se o outro operando for do tipo `sbyte`, `short`, `int`ou `long`.</span><span class="sxs-lookup"><span data-stu-id="dd693-357">Otherwise, if either operand is of type `ulong`, the other operand is converted to type `ulong`, or a binding-time error occurs if the other operand is of type `sbyte`, `short`, `int`, or `long`.</span></span>
*  <span data-ttu-id="dd693-358">Caso contrário, se um dos operandos for do tipo `long`, o outro operando será convertido no tipo `long`.</span><span class="sxs-lookup"><span data-stu-id="dd693-358">Otherwise, if either operand is of type `long`, the other operand is converted to type `long`.</span></span>
*  <span data-ttu-id="dd693-359">Caso contrário, se um dos operandos for do tipo `uint` e o outro operando for do tipo `sbyte`, `short`ou `int`, ambos os operandos serão convertidos no tipo `long`.</span><span class="sxs-lookup"><span data-stu-id="dd693-359">Otherwise, if either operand is of type `uint` and the other operand is of type `sbyte`, `short`, or `int`, both operands are converted to type `long`.</span></span>
*  <span data-ttu-id="dd693-360">Caso contrário, se um dos operandos for do tipo `uint`, o outro operando será convertido no tipo `uint`.</span><span class="sxs-lookup"><span data-stu-id="dd693-360">Otherwise, if either operand is of type `uint`, the other operand is converted to type `uint`.</span></span>
*  <span data-ttu-id="dd693-361">Caso contrário, ambos os operandos serão convertidos no tipo `int`.</span><span class="sxs-lookup"><span data-stu-id="dd693-361">Otherwise, both operands are converted to type `int`.</span></span>

<span data-ttu-id="dd693-362">Observe que a primeira regra não permite nenhuma operação que misture o tipo de `decimal` com os tipos `double` e `float`.</span><span class="sxs-lookup"><span data-stu-id="dd693-362">Note that the first rule disallows any operations that mix the `decimal` type with the `double` and `float` types.</span></span> <span data-ttu-id="dd693-363">A regra segue do fato de que não há conversões implícitas entre o tipo de `decimal` e os tipos de `double` e `float`.</span><span class="sxs-lookup"><span data-stu-id="dd693-363">The rule follows from the fact that there are no implicit conversions between the `decimal` type and the `double` and `float` types.</span></span>

<span data-ttu-id="dd693-364">Observe também que não é possível que um operando seja do tipo `ulong` quando o outro operando é de um tipo integral assinado.</span><span class="sxs-lookup"><span data-stu-id="dd693-364">Also note that it is not possible for an operand to be of type `ulong` when the other operand is of a signed integral type.</span></span> <span data-ttu-id="dd693-365">O motivo é que não existe nenhum tipo integral que possa representar o intervalo completo de `ulong`, bem como os tipos integrais assinados.</span><span class="sxs-lookup"><span data-stu-id="dd693-365">The reason is that no integral type exists that can represent the full range of `ulong` as well as the signed integral types.</span></span>

<span data-ttu-id="dd693-366">Em ambos os casos acima, uma expressão de conversão pode ser usada para converter explicitamente um operando em um tipo que seja compatível com o outro operando.</span><span class="sxs-lookup"><span data-stu-id="dd693-366">In both of the above cases, a cast expression can be used to explicitly convert one operand to a type that is compatible with the other operand.</span></span>

<span data-ttu-id="dd693-367">No exemplo</span><span class="sxs-lookup"><span data-stu-id="dd693-367">In the example</span></span>
```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (1.0 + percent / 100.0);
}
```
<span data-ttu-id="dd693-368">um erro de tempo de associação ocorre porque um `decimal` não pode ser multiplicado por um `double`.</span><span class="sxs-lookup"><span data-stu-id="dd693-368">a binding-time error occurs because a `decimal` cannot be multiplied by a `double`.</span></span> <span data-ttu-id="dd693-369">O erro é resolvido convertendo explicitamente o segundo operando para `decimal`, da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="dd693-369">The error is resolved by explicitly converting the second operand to `decimal`, as follows:</span></span>

```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (decimal)(1.0 + percent / 100.0);
}
```

### <a name="lifted-operators"></a><span data-ttu-id="dd693-370">Operadores levantados</span><span class="sxs-lookup"><span data-stu-id="dd693-370">Lifted operators</span></span>

<span data-ttu-id="dd693-371">***Operadores levantados*** permitem operadores predefinidos e definidos pelo usuário que operam em tipos de valores não anuláveis para também serem usados com formulários anuláveis desses tipos.</span><span class="sxs-lookup"><span data-stu-id="dd693-371">***Lifted operators*** permit predefined and user-defined operators that operate on non-nullable value types to also be used with nullable forms of those types.</span></span> <span data-ttu-id="dd693-372">Os operadores levantados são construídos a partir de operadores predefinidos e definidos pelo usuário que atendem a determinados requisitos, conforme descrito no seguinte:</span><span class="sxs-lookup"><span data-stu-id="dd693-372">Lifted operators are constructed from predefined and user-defined operators that meet certain requirements, as described in the following:</span></span>

*   <span data-ttu-id="dd693-373">Para os operadores unários</span><span class="sxs-lookup"><span data-stu-id="dd693-373">For the unary operators</span></span>

    ```csharp
    +  ++  -  --  !  ~
    ```

    <span data-ttu-id="dd693-374">uma forma levantada de um operador existirá se o operando e os tipos de resultado forem tipos de valor não anuláveis.</span><span class="sxs-lookup"><span data-stu-id="dd693-374">a lifted form of an operator exists if the operand and result types are both non-nullable value types.</span></span> <span data-ttu-id="dd693-375">O formulário levantado é construído com a adição de um único modificador de `?` para os tipos de operando e de resultado.</span><span class="sxs-lookup"><span data-stu-id="dd693-375">The lifted form is constructed by adding a single `?` modifier to the operand and result types.</span></span> <span data-ttu-id="dd693-376">O operador levantado produz um valor nulo se o operando for nulo.</span><span class="sxs-lookup"><span data-stu-id="dd693-376">The lifted operator produces a null value if the operand is null.</span></span> <span data-ttu-id="dd693-377">Caso contrário, o operador levantado desencapsula o operando, aplica o operador subjacente e encapsula o resultado.</span><span class="sxs-lookup"><span data-stu-id="dd693-377">Otherwise, the lifted operator unwraps the operand, applies the underlying operator, and wraps the result.</span></span>

*   <span data-ttu-id="dd693-378">Para os operadores binários</span><span class="sxs-lookup"><span data-stu-id="dd693-378">For the binary operators</span></span>

    ```csharp
    +  -  *  /  %  &  |  ^  <<  >>
    ```

    <span data-ttu-id="dd693-379">Existe uma forma levantada de um operador se os tipos de operando e de resultado são todos os tipos de valor não anuláveis.</span><span class="sxs-lookup"><span data-stu-id="dd693-379">a lifted form of an operator exists if the operand and result types are all non-nullable value types.</span></span> <span data-ttu-id="dd693-380">O formulário levantado é construído com a adição de um único modificador de `?` a cada operando e tipo de resultado.</span><span class="sxs-lookup"><span data-stu-id="dd693-380">The lifted form is constructed by adding a single `?` modifier to each operand and result type.</span></span> <span data-ttu-id="dd693-381">O operador levantado produz um valor nulo se um ou ambos os operandos forem nulos (uma exceção sendo os operadores `&` e `|` do tipo `bool?`, conforme descrito em [operadores lógicos boolianos](expressions.md#boolean-logical-operators)).</span><span class="sxs-lookup"><span data-stu-id="dd693-381">The lifted operator produces a null value if one or both operands are null (an exception being the `&` and `|` operators of the `bool?` type, as described in [Boolean logical operators](expressions.md#boolean-logical-operators)).</span></span> <span data-ttu-id="dd693-382">Caso contrário, o operador levantado desencapsula os operandos, aplica o operador subjacente e encapsula o resultado.</span><span class="sxs-lookup"><span data-stu-id="dd693-382">Otherwise, the lifted operator unwraps the operands, applies the underlying operator, and wraps the result.</span></span>

*   <span data-ttu-id="dd693-383">Para os operadores de igualdade</span><span class="sxs-lookup"><span data-stu-id="dd693-383">For the equality operators</span></span>

    ```csharp
    ==  !=
    ```

    <span data-ttu-id="dd693-384">Existe uma forma levantada de um operador se os tipos de operando forem tipos de valor não anuláveis e se o tipo de resultado for `bool`.</span><span class="sxs-lookup"><span data-stu-id="dd693-384">a lifted form of an operator exists if the operand types are both non-nullable value types and if the result type is `bool`.</span></span> <span data-ttu-id="dd693-385">O formulário levantado é construído com a adição de um único modificador de `?` a cada tipo de operando.</span><span class="sxs-lookup"><span data-stu-id="dd693-385">The lifted form is constructed by adding a single `?` modifier to each operand type.</span></span> <span data-ttu-id="dd693-386">O operador levantado considera dois valores nulos iguais e um valor nulo é diferente de qualquer valor não nulo.</span><span class="sxs-lookup"><span data-stu-id="dd693-386">The lifted operator considers two null values equal, and a null value unequal to any non-null value.</span></span> <span data-ttu-id="dd693-387">Se ambos os operandos forem não nulos, o operador levantado desencapsulará os operandos e aplicará o operador subjacente para produzir o resultado de `bool`.</span><span class="sxs-lookup"><span data-stu-id="dd693-387">If both operands are non-null, the lifted operator unwraps the operands and applies the underlying operator to produce the `bool` result.</span></span>

*   <span data-ttu-id="dd693-388">Para os operadores relacionais</span><span class="sxs-lookup"><span data-stu-id="dd693-388">For the relational operators</span></span>

    ```csharp
    <  >  <=  >=
    ```

    <span data-ttu-id="dd693-389">Existe uma forma levantada de um operador se os tipos de operando forem tipos de valor não anuláveis e se o tipo de resultado for `bool`.</span><span class="sxs-lookup"><span data-stu-id="dd693-389">a lifted form of an operator exists if the operand types are both non-nullable value types and if the result type is `bool`.</span></span> <span data-ttu-id="dd693-390">O formulário levantado é construído com a adição de um único modificador de `?` a cada tipo de operando.</span><span class="sxs-lookup"><span data-stu-id="dd693-390">The lifted form is constructed by adding a single `?` modifier to each operand type.</span></span> <span data-ttu-id="dd693-391">O operador levantado produz o valor `false` se um ou ambos os operandos forem nulos.</span><span class="sxs-lookup"><span data-stu-id="dd693-391">The lifted operator produces the value `false` if one or both operands are null.</span></span> <span data-ttu-id="dd693-392">Caso contrário, o operador levantado desencapsula os operandos e aplica o operador subjacente para produzir o resultado de `bool`.</span><span class="sxs-lookup"><span data-stu-id="dd693-392">Otherwise, the lifted operator unwraps the operands and applies the underlying operator to produce the `bool` result.</span></span>

## <a name="member-lookup"></a><span data-ttu-id="dd693-393">Pesquisa de membros</span><span class="sxs-lookup"><span data-stu-id="dd693-393">Member lookup</span></span>

<span data-ttu-id="dd693-394">Uma pesquisa de membro é o processo pelo qual o significado de um nome no contexto de um tipo é determinado.</span><span class="sxs-lookup"><span data-stu-id="dd693-394">A member lookup is the process whereby the meaning of a name in the context of a type is determined.</span></span> <span data-ttu-id="dd693-395">Uma pesquisa de membro pode ocorrer como parte da avaliação de um *Simple_name* ([nomes simples](expressions.md#simple-names)) ou uma *member_access* ([acesso de membro](expressions.md#member-access)) em uma expressão.</span><span class="sxs-lookup"><span data-stu-id="dd693-395">A member lookup can occur as part of evaluating a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)) in an expression.</span></span> <span data-ttu-id="dd693-396">Se o *Simple_name* ou *member_access* ocorrer como o *primary_expression* de um *invocation_expression* ([invocações de método](expressions.md#method-invocations)), o membro será chamado de chamada.</span><span class="sxs-lookup"><span data-stu-id="dd693-396">If the *simple_name* or *member_access* occurs as the *primary_expression* of an *invocation_expression* ([Method invocations](expressions.md#method-invocations)), the member is said to be invoked.</span></span>

<span data-ttu-id="dd693-397">Se um membro for um método ou evento, ou se for uma constante, campo ou propriedade de um tipo delegado ([delegados](delegates.md)) ou do tipo `dynamic` ([o tipo dinâmico](types.md#the-dynamic-type)), o membro será dito como *invocável*.</span><span class="sxs-lookup"><span data-stu-id="dd693-397">If a member is a method or event, or if it is a constant, field or property of either a delegate type ([Delegates](delegates.md)) or the type `dynamic` ([The dynamic type](types.md#the-dynamic-type)), then the member is said to be *invocable*.</span></span>

<span data-ttu-id="dd693-398">A pesquisa de membros considera não apenas o nome de um membro, mas também o número de parâmetros de tipo que o membro tem e se o membro está acessível.</span><span class="sxs-lookup"><span data-stu-id="dd693-398">Member lookup considers not only the name of a member but also the number of type parameters the member has and whether the member is accessible.</span></span> <span data-ttu-id="dd693-399">Para fins de pesquisa de membros, os métodos genéricos e os tipos genéricos aninhados têm o número de parâmetros de tipo indicados em suas respectivas declarações e todos os outros membros têm zero parâmetros de tipo.</span><span class="sxs-lookup"><span data-stu-id="dd693-399">For the purposes of member lookup, generic methods and nested generic types have the number of type parameters indicated in their respective declarations and all other members have zero type parameters.</span></span>

<span data-ttu-id="dd693-400">Uma pesquisa de membro de um nome `N` com `K`parâmetros de tipo de  em um tipo `T` é processada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="dd693-400">A member lookup of a name `N` with `K` type parameters in a type `T` is processed as follows:</span></span>

*  <span data-ttu-id="dd693-401">Primeiro, um conjunto de membros acessíveis chamado `N` é determinado:</span><span class="sxs-lookup"><span data-stu-id="dd693-401">First, a set of accessible members named `N` is determined:</span></span>
    * <span data-ttu-id="dd693-402">Se `T` for um parâmetro de tipo, o conjunto será a União dos conjuntos de membros acessíveis chamados `N` em cada um dos tipos especificados como uma restrição primária ou restrição secundária ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)) para `T`, juntamente com o conjunto de membros acessíveis chamados `N` em `object`.</span><span class="sxs-lookup"><span data-stu-id="dd693-402">If `T` is a type parameter, then the set is the union of the sets of accessible members named `N` in each of the types specified as a primary constraint or secondary constraint ([Type parameter constraints](classes.md#type-parameter-constraints)) for `T`, along with the set of accessible members named `N` in `object`.</span></span>
    * <span data-ttu-id="dd693-403">Caso contrário, o conjunto consiste em todos os membros acessíveis ([acesso de membro](basic-concepts.md#member-access)) chamados `N` em `T`, incluindo membros herdados e os membros acessíveis chamados `N` no `object`.</span><span class="sxs-lookup"><span data-stu-id="dd693-403">Otherwise, the set consists of all accessible ([Member access](basic-concepts.md#member-access)) members named `N` in `T`, including inherited members and the accessible members named `N` in `object`.</span></span> <span data-ttu-id="dd693-404">Se `T` for um tipo construído, o conjunto de membros será obtido substituindo argumentos de tipo conforme descrito em [membros de tipos construídos](classes.md#members-of-constructed-types).</span><span class="sxs-lookup"><span data-stu-id="dd693-404">If `T` is a constructed type, the set of members is obtained by substituting type arguments as described in [Members of constructed types](classes.md#members-of-constructed-types).</span></span> <span data-ttu-id="dd693-405">Os membros que incluem um modificador de `override` são excluídos do conjunto.</span><span class="sxs-lookup"><span data-stu-id="dd693-405">Members that include an `override` modifier are excluded from the set.</span></span>
*  <span data-ttu-id="dd693-406">Em seguida, se `K` for zero, todos os tipos aninhados cujas declarações incluem parâmetros de tipo serão removidos.</span><span class="sxs-lookup"><span data-stu-id="dd693-406">Next, if `K` is zero, all nested types whose declarations include type parameters are removed.</span></span> <span data-ttu-id="dd693-407">Se `K` não for zero, todos os membros com um número diferente de parâmetros de tipo serão removidos.</span><span class="sxs-lookup"><span data-stu-id="dd693-407">If `K` is not zero, all members with a different number of type parameters are removed.</span></span> <span data-ttu-id="dd693-408">Observe que quando `K` é zero, os métodos com parâmetros de tipo não são removidos, pois o processo de inferência de tipos ([inferência de tipos](expressions.md#type-inference)) pode ser capaz de inferir os argumentos de tipo.</span><span class="sxs-lookup"><span data-stu-id="dd693-408">Note that when `K` is zero, methods having type parameters are not removed, since the type inference process ([Type inference](expressions.md#type-inference)) might be able to infer the type arguments.</span></span>
*  <span data-ttu-id="dd693-409">Em seguida, se o membro for *invocado*, todos os membros não*invocáveis* serão removidos do conjunto.</span><span class="sxs-lookup"><span data-stu-id="dd693-409">Next, if the member is *invoked*, all non-*invocable* members are removed from the set.</span></span>
*  <span data-ttu-id="dd693-410">Em seguida, os membros que estão ocultos por outros membros são removidos do conjunto.</span><span class="sxs-lookup"><span data-stu-id="dd693-410">Next, members that are hidden by other members are removed from the set.</span></span> <span data-ttu-id="dd693-411">Para cada membro `S.M` no conjunto, em que `S` é o tipo no qual o membro `M` é declarado, as seguintes regras são aplicadas:</span><span class="sxs-lookup"><span data-stu-id="dd693-411">For every member `S.M` in the set, where `S` is the type in which the member `M` is declared, the following rules are applied:</span></span>
    * <span data-ttu-id="dd693-412">Se `M` for uma constante, campo, propriedade, evento ou membro de enumeração, todos os membros declarados em um tipo base de `S` serão removidos do conjunto.</span><span class="sxs-lookup"><span data-stu-id="dd693-412">If `M` is a constant, field, property, event, or enumeration member, then all members declared in a base type of `S` are removed from the set.</span></span>
    * <span data-ttu-id="dd693-413">Se `M` for uma declaração de tipo, todos os não tipos declarados em um tipo base de `S` serão removidos do conjunto, e todas as declarações de tipo com o mesmo número de parâmetros de tipo como `M` declarados em um tipo base de `S` serão removidas do conjunto.</span><span class="sxs-lookup"><span data-stu-id="dd693-413">If `M` is a type declaration, then all non-types declared in a base type of `S` are removed from the set, and all type declarations with the same number of type parameters as `M` declared in a base type of `S` are removed from the set.</span></span>
    * <span data-ttu-id="dd693-414">Se `M` for um método, todos os membros que não forem do método declarados em um tipo base de `S` serão removidos do conjunto.</span><span class="sxs-lookup"><span data-stu-id="dd693-414">If `M` is a method, then all non-method members declared in a base type of `S` are removed from the set.</span></span>
*  <span data-ttu-id="dd693-415">Em seguida, os membros de interface ocultos por membros de classe são removidos do conjunto.</span><span class="sxs-lookup"><span data-stu-id="dd693-415">Next, interface members that are hidden by class members are removed from the set.</span></span> <span data-ttu-id="dd693-416">Essa etapa só terá efeito se `T` for um parâmetro de tipo e `T` tiver uma classe base em vigor diferente de `object` e um conjunto de interfaces efetivas não vazias ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="dd693-416">This step only has an effect if `T` is a type parameter and `T` has both an effective base class other than `object` and a non-empty effective interface set ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="dd693-417">Para cada membro `S.M` no conjunto, em que `S` é o tipo no qual o membro `M` é declarado, as regras a seguir serão aplicadas se `S` for uma declaração de classe diferente de `object`:</span><span class="sxs-lookup"><span data-stu-id="dd693-417">For every member `S.M` in the set, where `S` is the type in which the member `M` is declared, the following rules are applied if `S` is a class declaration other than `object`:</span></span>
    * <span data-ttu-id="dd693-418">Se `M` for uma declaração constante, campo, propriedade, evento, membro de enumeração ou tipo, todos os membros declarados em uma declaração de interface serão removidos do conjunto.</span><span class="sxs-lookup"><span data-stu-id="dd693-418">If `M` is a constant, field, property, event, enumeration member, or type declaration, then all members declared in an interface declaration are removed from the set.</span></span>
    * <span data-ttu-id="dd693-419">Se `M` for um método, todos os membros não-método declarados em uma declaração de interface serão removidos do conjunto e todos os métodos com a mesma assinatura que `M` declarados em uma declaração de interface serão removidos do conjunto.</span><span class="sxs-lookup"><span data-stu-id="dd693-419">If `M` is a method, then all non-method members declared in an interface declaration are removed from the set, and all methods with the same signature as `M` declared in an interface declaration are removed from the set.</span></span>
*  <span data-ttu-id="dd693-420">Finalmente, ter removido Membros ocultos, o resultado da pesquisa é determinado:</span><span class="sxs-lookup"><span data-stu-id="dd693-420">Finally, having removed hidden members, the result of the lookup is determined:</span></span>
    * <span data-ttu-id="dd693-421">Se o conjunto consistir em um único membro que não seja um método, esse membro será o resultado da pesquisa.</span><span class="sxs-lookup"><span data-stu-id="dd693-421">If the set consists of a single member that is not a method, then this member is the result of the lookup.</span></span>
    * <span data-ttu-id="dd693-422">Caso contrário, se o conjunto contiver apenas métodos, esse grupo de métodos será o resultado da pesquisa.</span><span class="sxs-lookup"><span data-stu-id="dd693-422">Otherwise, if the set contains only methods, then this group of methods is the result of the lookup.</span></span>
    * <span data-ttu-id="dd693-423">Caso contrário, a pesquisa será ambígua e ocorrerá um erro de tempo de vinculação.</span><span class="sxs-lookup"><span data-stu-id="dd693-423">Otherwise, the lookup is ambiguous, and a binding-time error occurs.</span></span>

<span data-ttu-id="dd693-424">Para pesquisas de membros em tipos diferentes de parâmetros de tipo e interfaces, e pesquisas de membros em interfaces que são estritamente herança simples (cada interface na cadeia de herança tem exatamente zero ou uma interface de base direta), o efeito das regras de pesquisa é simplesmente, os membros derivados ocultam os membros base com o mesmo nome ou assinatura.</span><span class="sxs-lookup"><span data-stu-id="dd693-424">For member lookups in types other than type parameters and interfaces, and member lookups in interfaces that are strictly single-inheritance (each interface in the inheritance chain has exactly zero or one direct base interface), the effect of the lookup rules is simply that derived members hide base members with the same name or signature.</span></span> <span data-ttu-id="dd693-425">Essas pesquisas de herança única nunca são ambíguas.</span><span class="sxs-lookup"><span data-stu-id="dd693-425">Such single-inheritance lookups are never ambiguous.</span></span> <span data-ttu-id="dd693-426">As ambiguidades que podem surgir em relação a pesquisas de membros em interfaces de várias heranças são descritas em [acesso de membro de interface](interfaces.md#interface-member-access).</span><span class="sxs-lookup"><span data-stu-id="dd693-426">The ambiguities that can possibly arise from member lookups in multiple-inheritance interfaces are described in [Interface member access](interfaces.md#interface-member-access).</span></span>

### <a name="base-types"></a><span data-ttu-id="dd693-427">Tipos base</span><span class="sxs-lookup"><span data-stu-id="dd693-427">Base types</span></span>

<span data-ttu-id="dd693-428">Para fins de pesquisa de membros, um tipo `T` é considerado com os seguintes tipos base:</span><span class="sxs-lookup"><span data-stu-id="dd693-428">For purposes of member lookup, a type `T` is considered to have the following base types:</span></span>

*  <span data-ttu-id="dd693-429">Se `T` for `object`, `T` não terá nenhum tipo base.</span><span class="sxs-lookup"><span data-stu-id="dd693-429">If `T` is `object`, then `T` has no base type.</span></span>
*  <span data-ttu-id="dd693-430">Se `T` for uma *enum_type*, os tipos de base de `T` serão os tipos de classe `System.Enum`, `System.ValueType`e `object`.</span><span class="sxs-lookup"><span data-stu-id="dd693-430">If `T` is an *enum_type*, the base types of `T` are the class types `System.Enum`, `System.ValueType`, and `object`.</span></span>
*  <span data-ttu-id="dd693-431">Se `T` for uma *struct_type*, os tipos de base de `T` serão os tipos de classe `System.ValueType` e `object`.</span><span class="sxs-lookup"><span data-stu-id="dd693-431">If `T` is a *struct_type*, the base types of `T` are the class types `System.ValueType` and `object`.</span></span>
*  <span data-ttu-id="dd693-432">Se `T` for uma *class_type*, os tipos de base de `T` serão as classes base de `T`, incluindo o tipo de classe `object`.</span><span class="sxs-lookup"><span data-stu-id="dd693-432">If `T` is a *class_type*, the base types of `T` are the base classes of `T`, including the class type `object`.</span></span>
*  <span data-ttu-id="dd693-433">Se `T` for uma *interface_type*, os tipos de base de `T` serão as interfaces base de `T` e o tipo de classe `object`.</span><span class="sxs-lookup"><span data-stu-id="dd693-433">If `T` is an *interface_type*, the base types of `T` are the base interfaces of `T` and the class type `object`.</span></span>
*  <span data-ttu-id="dd693-434">Se `T` for um *array_type*, os tipos de base de `T` serão os tipos de classe `System.Array` e `object`.</span><span class="sxs-lookup"><span data-stu-id="dd693-434">If `T` is an *array_type*, the base types of `T` are the class types `System.Array` and `object`.</span></span>
*  <span data-ttu-id="dd693-435">Se `T` for uma *delegate_type*, os tipos de base de `T` serão os tipos de classe `System.Delegate` e `object`.</span><span class="sxs-lookup"><span data-stu-id="dd693-435">If `T` is a *delegate_type*, the base types of `T` are the class types `System.Delegate` and `object`.</span></span>

## <a name="function-members"></a><span data-ttu-id="dd693-436">Membros da função</span><span class="sxs-lookup"><span data-stu-id="dd693-436">Function members</span></span>

<span data-ttu-id="dd693-437">Membros de função são membros que contêm instruções Executáveis.</span><span class="sxs-lookup"><span data-stu-id="dd693-437">Function members are members that contain executable statements.</span></span> <span data-ttu-id="dd693-438">Membros de função são sempre membros de tipos e não podem ser membros de namespaces.</span><span class="sxs-lookup"><span data-stu-id="dd693-438">Function members are always members of types and cannot be members of namespaces.</span></span> <span data-ttu-id="dd693-439">C#define as seguintes categorias de membros de função:</span><span class="sxs-lookup"><span data-stu-id="dd693-439">C# defines the following categories of function members:</span></span>

*  <span data-ttu-id="dd693-440">{1&gt;Métodos&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-440">Methods</span></span>
*  <span data-ttu-id="dd693-441">{1&gt;Propriedades&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-441">Properties</span></span>
*  <span data-ttu-id="dd693-442">Eventos</span><span class="sxs-lookup"><span data-stu-id="dd693-442">Events</span></span>
*  <span data-ttu-id="dd693-443">Indexadores</span><span class="sxs-lookup"><span data-stu-id="dd693-443">Indexers</span></span>
*  <span data-ttu-id="dd693-444">Operadores definidos pelo usuário</span><span class="sxs-lookup"><span data-stu-id="dd693-444">User-defined operators</span></span>
*  <span data-ttu-id="dd693-445">Construtores de instância</span><span class="sxs-lookup"><span data-stu-id="dd693-445">Instance constructors</span></span>
*  <span data-ttu-id="dd693-446">Construtores estáticos</span><span class="sxs-lookup"><span data-stu-id="dd693-446">Static constructors</span></span>
*  <span data-ttu-id="dd693-447">Destruidores</span><span class="sxs-lookup"><span data-stu-id="dd693-447">Destructors</span></span>

<span data-ttu-id="dd693-448">Exceto para destruidores e construtores estáticos (que não podem ser invocados explicitamente), as instruções contidas nos membros da função são executadas por meio de invocações de membro de função.</span><span class="sxs-lookup"><span data-stu-id="dd693-448">Except for destructors and static constructors (which cannot be invoked explicitly), the statements contained in function members are executed through function member invocations.</span></span> <span data-ttu-id="dd693-449">A sintaxe real para gravar uma invocação de membro de função depende da categoria de membro de função específica.</span><span class="sxs-lookup"><span data-stu-id="dd693-449">The actual syntax for writing a function member invocation depends on the particular function member category.</span></span>

<span data-ttu-id="dd693-450">A lista de argumentos ([listas de argumentos](expressions.md#argument-lists)) de uma invocação de membro de função fornece valores reais ou referências de variáveis para os parâmetros do membro da função.</span><span class="sxs-lookup"><span data-stu-id="dd693-450">The argument list ([Argument lists](expressions.md#argument-lists)) of a function member invocation provides actual values or variable references for the parameters of the function member.</span></span>

<span data-ttu-id="dd693-451">Invocações de métodos genéricos podem empregar inferência de tipos para determinar o conjunto de argumentos de tipo a serem passados para o método.</span><span class="sxs-lookup"><span data-stu-id="dd693-451">Invocations of generic methods may employ type inference to determine the set of type arguments to pass to the method.</span></span> <span data-ttu-id="dd693-452">Esse processo é descrito em [inferência de tipos](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="dd693-452">This process is described in [Type inference](expressions.md#type-inference).</span></span>

<span data-ttu-id="dd693-453">Invocações de métodos, indexadores, operadores e construtores de instância empregam resolução de sobrecarga para determinar qual de um conjunto candidato de membros de função invocar.</span><span class="sxs-lookup"><span data-stu-id="dd693-453">Invocations of methods, indexers, operators and instance constructors employ overload resolution to determine which of a candidate set of function members to invoke.</span></span> <span data-ttu-id="dd693-454">Esse processo é descrito em [resolução de sobrecarga](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="dd693-454">This process is described in [Overload resolution](expressions.md#overload-resolution).</span></span>

<span data-ttu-id="dd693-455">Depois que um membro de função específico tiver sido identificado no tempo de vinculação, possivelmente por meio da resolução de sobrecarga, o processo real de tempo de execução de invocação do membro da função será descrito em [verificação de tempo de compilação da resolução dinâmica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="dd693-455">Once a particular function member has been identified at binding-time, possibly through overload resolution, the actual run-time process of invoking the function member is described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="dd693-456">A tabela a seguir resume o processamento que ocorre em construções envolvendo as seis categorias de membros de função que podem ser invocadas explicitamente.</span><span class="sxs-lookup"><span data-stu-id="dd693-456">The following table summarizes the processing that takes place in constructs involving the six categories of function members that can be explicitly invoked.</span></span> <span data-ttu-id="dd693-457">Na tabela, `e`, `x`, `y`e `value` indicam expressões classificadas como variáveis ou valores, `T` indica uma expressão classificada como um tipo, `F` é o nome simples de um método e `P` é o nome simples de uma propriedade.</span><span class="sxs-lookup"><span data-stu-id="dd693-457">In the table, `e`, `x`, `y`, and `value` indicate expressions classified as variables or values, `T` indicates an expression classified as a type, `F` is the simple name of a method, and `P` is the simple name of a property.</span></span>


| <span data-ttu-id="dd693-458">__Construir__</span><span class="sxs-lookup"><span data-stu-id="dd693-458">__Construct__</span></span>     | <span data-ttu-id="dd693-459">__Exemplo__</span><span class="sxs-lookup"><span data-stu-id="dd693-459">__Example__</span></span>    | <span data-ttu-id="dd693-460">__Descrição__</span><span class="sxs-lookup"><span data-stu-id="dd693-460">__Description__</span></span> |
|-------------------|----------------|-----------------|
| <span data-ttu-id="dd693-461">Invocação de método</span><span class="sxs-lookup"><span data-stu-id="dd693-461">Method invocation</span></span> | `F(x,y)`       | <span data-ttu-id="dd693-462">A resolução de sobrecarga é aplicada para selecionar o melhor método `F` na classe ou estrutura que o contém.</span><span class="sxs-lookup"><span data-stu-id="dd693-462">Overload resolution is applied to select the best method `F` in the containing class or struct.</span></span> <span data-ttu-id="dd693-463">O método é invocado com a lista de argumentos `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="dd693-463">The method is invoked with the argument list `(x,y)`.</span></span> <span data-ttu-id="dd693-464">Se o método não for `static`, a expressão de instância será `this`.</span><span class="sxs-lookup"><span data-stu-id="dd693-464">If the method is not `static`, the instance expression is `this`.</span></span> | 
|                   | `T.F(x,y)`     | <span data-ttu-id="dd693-465">A resolução de sobrecarga é aplicada para selecionar o melhor método `F` no `T`de classe ou struct.</span><span class="sxs-lookup"><span data-stu-id="dd693-465">Overload resolution is applied to select the best method `F` in the class or struct `T`.</span></span> <span data-ttu-id="dd693-466">Ocorrerá um erro de tempo de ligação se o método não for `static`.</span><span class="sxs-lookup"><span data-stu-id="dd693-466">A binding-time error occurs if the method is not `static`.</span></span> <span data-ttu-id="dd693-467">O método é invocado com a lista de argumentos `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="dd693-467">The method is invoked with the argument list `(x,y)`.</span></span> | 
|                   | `e.F(x,y)`     | <span data-ttu-id="dd693-468">A resolução de sobrecarga é aplicada para selecionar o melhor método F na classe, struct ou interface fornecida pelo tipo de `e`.</span><span class="sxs-lookup"><span data-stu-id="dd693-468">Overload resolution is applied to select the best method F in the class, struct, or interface given by the type of `e`.</span></span> <span data-ttu-id="dd693-469">Ocorrerá um erro de tempo de ligação se o método for `static`.</span><span class="sxs-lookup"><span data-stu-id="dd693-469">A binding-time error occurs if the method is `static`.</span></span> <span data-ttu-id="dd693-470">O método é invocado com a expressão de instância `e` e a lista de argumentos `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="dd693-470">The method is invoked with the instance expression `e` and the argument list `(x,y)`.</span></span> | 
| <span data-ttu-id="dd693-471">Acesso à propriedade</span><span class="sxs-lookup"><span data-stu-id="dd693-471">Property access</span></span>   | `P`            | <span data-ttu-id="dd693-472">O acessador de `get` da propriedade `P` na classe ou struct que a contém é invocado.</span><span class="sxs-lookup"><span data-stu-id="dd693-472">The `get` accessor of the property `P` in the containing class or struct is invoked.</span></span> <span data-ttu-id="dd693-473">Ocorrerá um erro em tempo de compilação se `P` for somente gravação.</span><span class="sxs-lookup"><span data-stu-id="dd693-473">A compile-time error occurs if `P` is write-only.</span></span> <span data-ttu-id="dd693-474">Se `P` não for `static`, a expressão de instância será `this`.</span><span class="sxs-lookup"><span data-stu-id="dd693-474">If `P` is not `static`, the instance expression is `this`.</span></span> | 
|                   | `P = value`    | <span data-ttu-id="dd693-475">O acessador de `set` da propriedade `P` na classe ou struct que o contém é invocado com a lista de argumentos `(value)`.</span><span class="sxs-lookup"><span data-stu-id="dd693-475">The `set` accessor of the property `P` in the containing class or struct is invoked with the argument list `(value)`.</span></span> <span data-ttu-id="dd693-476">Ocorrerá um erro em tempo de compilação se `P` for somente leitura.</span><span class="sxs-lookup"><span data-stu-id="dd693-476">A compile-time error occurs if `P` is read-only.</span></span> <span data-ttu-id="dd693-477">Se `P` não for `static`, a expressão de instância será `this`.</span><span class="sxs-lookup"><span data-stu-id="dd693-477">If `P` is not `static`, the instance expression is `this`.</span></span> | 
|                   | `T.P`          | <span data-ttu-id="dd693-478">O acessador de `get` da propriedade `P` no `T` de classe ou struct é invocado.</span><span class="sxs-lookup"><span data-stu-id="dd693-478">The `get` accessor of the property `P` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="dd693-479">Ocorrerá um erro de tempo de compilação se `P` não for `static` ou se `P` for somente gravação.</span><span class="sxs-lookup"><span data-stu-id="dd693-479">A compile-time error occurs if `P` is not `static` or if `P` is write-only.</span></span> | 
|                   | `T.P = value`  | <span data-ttu-id="dd693-480">O acessador de `set` da propriedade `P` no `T` de classe ou struct é invocado com a lista de argumentos `(value)`.</span><span class="sxs-lookup"><span data-stu-id="dd693-480">The `set` accessor of the property `P` in the class or struct `T` is invoked with the argument list `(value)`.</span></span> <span data-ttu-id="dd693-481">Ocorrerá um erro de tempo de compilação se `P` não for `static` ou se `P` for somente leitura.</span><span class="sxs-lookup"><span data-stu-id="dd693-481">A compile-time error occurs if `P` is not `static` or if `P` is read-only.</span></span> | 
|                   | `e.P`          | <span data-ttu-id="dd693-482">O acessador de `get` da propriedade `P` na classe, struct ou interface fornecida pelo tipo de `e` é invocado com a expressão de instância `e`.</span><span class="sxs-lookup"><span data-stu-id="dd693-482">The `get` accessor of the property `P` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="dd693-483">Ocorrerá um erro de tempo de associação se `P` for `static` ou se `P` for somente gravação.</span><span class="sxs-lookup"><span data-stu-id="dd693-483">A binding-time error occurs if `P` is `static` or if `P` is write-only.</span></span> | 
|                   | `e.P = value`  | <span data-ttu-id="dd693-484">O acessador de `set` da propriedade `P` na classe, struct ou interface fornecida pelo tipo de `e` é invocado com a expressão de instância `e` e a lista de argumentos `(value)`.</span><span class="sxs-lookup"><span data-stu-id="dd693-484">The `set` accessor of the property `P` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e` and the argument list `(value)`.</span></span> <span data-ttu-id="dd693-485">Ocorrerá um erro de tempo de associação se `P` for `static` ou se `P` for somente leitura.</span><span class="sxs-lookup"><span data-stu-id="dd693-485">A binding-time error occurs if `P` is `static` or if `P` is read-only.</span></span> | 
| <span data-ttu-id="dd693-486">Acesso a eventos</span><span class="sxs-lookup"><span data-stu-id="dd693-486">Event access</span></span>      | `E += value`   | <span data-ttu-id="dd693-487">O acessador de `add` do `E` de eventos na classe ou struct que o contém é invocado.</span><span class="sxs-lookup"><span data-stu-id="dd693-487">The `add` accessor of the event `E` in the containing class or struct is invoked.</span></span> <span data-ttu-id="dd693-488">Se `E` não for estático, a expressão de instância será `this`.</span><span class="sxs-lookup"><span data-stu-id="dd693-488">If `E` is not static, the instance expression is `this`.</span></span> | 
|                   | `E -= value`   | <span data-ttu-id="dd693-489">O acessador de `remove` do `E` de eventos na classe ou struct que o contém é invocado.</span><span class="sxs-lookup"><span data-stu-id="dd693-489">The `remove` accessor of the event `E` in the containing class or struct is invoked.</span></span> <span data-ttu-id="dd693-490">Se `E` não for estático, a expressão de instância será `this`.</span><span class="sxs-lookup"><span data-stu-id="dd693-490">If `E` is not static, the instance expression is `this`.</span></span> | 
|                   | `T.E += value` | <span data-ttu-id="dd693-491">O acessador de `add` do `E` de eventos na `T` de classe ou struct é invocado.</span><span class="sxs-lookup"><span data-stu-id="dd693-491">The `add` accessor of the event `E` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="dd693-492">Ocorrerá um erro de tempo de associação se `E` não for estático.</span><span class="sxs-lookup"><span data-stu-id="dd693-492">A binding-time error occurs if `E` is not static.</span></span> | 
|                   | `T.E -= value` | <span data-ttu-id="dd693-493">O acessador de `remove` do `E` de eventos na `T` de classe ou struct é invocado.</span><span class="sxs-lookup"><span data-stu-id="dd693-493">The `remove` accessor of the event `E` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="dd693-494">Ocorrerá um erro de tempo de associação se `E` não for estático.</span><span class="sxs-lookup"><span data-stu-id="dd693-494">A binding-time error occurs if `E` is not static.</span></span> | 
|                   | `e.E += value` | <span data-ttu-id="dd693-495">O acessador de `add` do `E` de eventos na classe, struct ou interface fornecida pelo tipo de `e` é invocado com a expressão de instância `e`.</span><span class="sxs-lookup"><span data-stu-id="dd693-495">The `add` accessor of the event `E` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="dd693-496">Ocorrerá um erro de tempo de associação se `E` for estático.</span><span class="sxs-lookup"><span data-stu-id="dd693-496">A binding-time error occurs if `E` is static.</span></span> | 
|                   | `e.E -= value` | <span data-ttu-id="dd693-497">O acessador de `remove` do `E` de eventos na classe, struct ou interface fornecida pelo tipo de `e` é invocado com a expressão de instância `e`.</span><span class="sxs-lookup"><span data-stu-id="dd693-497">The `remove` accessor of the event `E` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="dd693-498">Ocorrerá um erro de tempo de associação se `E` for estático.</span><span class="sxs-lookup"><span data-stu-id="dd693-498">A binding-time error occurs if `E` is static.</span></span> | 
| <span data-ttu-id="dd693-499">Acesso de indexador</span><span class="sxs-lookup"><span data-stu-id="dd693-499">Indexer access</span></span>    | `e[x,y]`       | <span data-ttu-id="dd693-500">A resolução de sobrecarga é aplicada para selecionar o melhor indexador na classe, struct ou interface fornecida pelo tipo de e.</span><span class="sxs-lookup"><span data-stu-id="dd693-500">Overload resolution is applied to select the best indexer in the class, struct, or interface given by the type of e.</span></span> <span data-ttu-id="dd693-501">O acessador de `get` do indexador é invocado com a expressão de instância `e` e a lista de argumentos `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="dd693-501">The `get` accessor of the indexer is invoked with the instance expression `e` and the argument list `(x,y)`.</span></span> <span data-ttu-id="dd693-502">Ocorrerá um erro de tempo de ligação se o indexador for somente gravação.</span><span class="sxs-lookup"><span data-stu-id="dd693-502">A binding-time error occurs if the indexer is write-only.</span></span> | 
|                   | `e[x,y] = value` | <span data-ttu-id="dd693-503">A resolução de sobrecarga é aplicada para selecionar o melhor indexador na classe, struct ou interface fornecida pelo tipo de `e`.</span><span class="sxs-lookup"><span data-stu-id="dd693-503">Overload resolution is applied to select the best indexer in the class, struct, or interface given by the type of `e`.</span></span> <span data-ttu-id="dd693-504">O acessador de `set` do indexador é invocado com a expressão de instância `e` e a lista de argumentos `(x,y,value)`.</span><span class="sxs-lookup"><span data-stu-id="dd693-504">The `set` accessor of the indexer is invoked with the instance expression `e` and the argument list `(x,y,value)`.</span></span> <span data-ttu-id="dd693-505">Ocorrerá um erro de tempo de ligação se o indexador for somente leitura.</span><span class="sxs-lookup"><span data-stu-id="dd693-505">A binding-time error occurs if the indexer is read-only.</span></span> | 
| <span data-ttu-id="dd693-506">Invocação de operador</span><span class="sxs-lookup"><span data-stu-id="dd693-506">Operator invocation</span></span> | `-x`         | <span data-ttu-id="dd693-507">A resolução de sobrecarga é aplicada para selecionar o melhor operador unário na classe ou struct fornecido pelo tipo de `x`.</span><span class="sxs-lookup"><span data-stu-id="dd693-507">Overload resolution is applied to select the best unary operator in the class or struct given by the type of `x`.</span></span> <span data-ttu-id="dd693-508">O operador selecionado é invocado com a lista de argumentos `(x)`.</span><span class="sxs-lookup"><span data-stu-id="dd693-508">The selected operator is invoked with the argument list `(x)`.</span></span> | 
|                     | `x + y`      | <span data-ttu-id="dd693-509">A resolução de sobrecarga é aplicada para selecionar o melhor operador binário nas classes ou estruturas dadas pelos tipos de `x` e `y`.</span><span class="sxs-lookup"><span data-stu-id="dd693-509">Overload resolution is applied to select the best binary operator in the classes or structs given by the types of `x` and `y`.</span></span> <span data-ttu-id="dd693-510">O operador selecionado é invocado com a lista de argumentos `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="dd693-510">The selected operator is invoked with the argument list `(x,y)`.</span></span> | 
| <span data-ttu-id="dd693-511">Invocação de construtor de instância</span><span class="sxs-lookup"><span data-stu-id="dd693-511">Instance constructor invocation</span></span> | `new T(x,y)` | <span data-ttu-id="dd693-512">A resolução de sobrecarga é aplicada para selecionar o melhor Construtor de instância no `T`de classe ou struct.</span><span class="sxs-lookup"><span data-stu-id="dd693-512">Overload resolution is applied to select the best instance constructor in the class or struct `T`.</span></span> <span data-ttu-id="dd693-513">O construtor de instância é invocado com a lista de argumentos `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="dd693-513">The instance constructor is invoked with the argument list `(x,y)`.</span></span> | 

### <a name="argument-lists"></a><span data-ttu-id="dd693-514">Listas de argumentos</span><span class="sxs-lookup"><span data-stu-id="dd693-514">Argument lists</span></span>

<span data-ttu-id="dd693-515">Cada membro de função e invocação de delegado inclui uma lista de argumentos que fornece valores reais ou referências de variáveis para os parâmetros do membro da função.</span><span class="sxs-lookup"><span data-stu-id="dd693-515">Every function member and delegate invocation includes an argument list which provides actual values or variable references for the parameters of the function member.</span></span> <span data-ttu-id="dd693-516">A sintaxe para especificar a lista de argumentos de uma invocação de membro de função depende da categoria de membro da função:</span><span class="sxs-lookup"><span data-stu-id="dd693-516">The syntax for specifying the argument list of a function member invocation depends on the function member category:</span></span>

*  <span data-ttu-id="dd693-517">Para construtores de instância, métodos, indexadores e delegados, os argumentos são especificados como um *argument_list*, conforme descrito abaixo.</span><span class="sxs-lookup"><span data-stu-id="dd693-517">For instance constructors, methods, indexers and delegates, the arguments are specified as an *argument_list*, as described below.</span></span> <span data-ttu-id="dd693-518">Para indexadores, ao invocar o acessador de `set`, a lista de argumentos também inclui a expressão especificada como o operando à direita do operador de atribuição.</span><span class="sxs-lookup"><span data-stu-id="dd693-518">For indexers, when invoking the `set` accessor, the argument list additionally includes the expression specified as the right operand of the assignment operator.</span></span>
*  <span data-ttu-id="dd693-519">Para propriedades, a lista de argumentos está vazia ao invocar o acessador de `get` e consiste na expressão especificada como o operando à direita do operador de atribuição ao invocar o acessador de `set`.</span><span class="sxs-lookup"><span data-stu-id="dd693-519">For properties, the argument list is empty when invoking the `get` accessor, and consists of the expression specified as the right operand of the assignment operator when invoking the `set` accessor.</span></span>
*  <span data-ttu-id="dd693-520">Para eventos, a lista de argumentos consiste na expressão especificada como o operando à direita do `+=` ou operador de `-=`.</span><span class="sxs-lookup"><span data-stu-id="dd693-520">For events, the argument list consists of the expression specified as the right operand of the `+=` or `-=` operator.</span></span>
*  <span data-ttu-id="dd693-521">Para operadores definidos pelo usuário, a lista de argumentos consiste no único operando do operador unário ou nos dois operandos do operador binário.</span><span class="sxs-lookup"><span data-stu-id="dd693-521">For user-defined operators, the argument list consists of the single operand of the unary operator or the two operands of the binary operator.</span></span>

<span data-ttu-id="dd693-522">Os argumentos de Propriedades ([Propriedades](classes.md#properties)), eventos ([eventos](classes.md#events)) e operadores definidos pelo usuário ([operadores](classes.md#operators)) são sempre passados como parâmetros de valor ([parâmetros de valor](classes.md#value-parameters)).</span><span class="sxs-lookup"><span data-stu-id="dd693-522">The arguments of properties ([Properties](classes.md#properties)), events ([Events](classes.md#events)), and user-defined operators ([Operators](classes.md#operators)) are always passed as value parameters ([Value parameters](classes.md#value-parameters)).</span></span> <span data-ttu-id="dd693-523">Os argumentos de indexadores ([indexadores](classes.md#indexers)) são sempre passados como parâmetros de valor ([parâmetros de valor](classes.md#value-parameters)) ou matrizes de parâmetro ([matrizes de parâmetro](classes.md#parameter-arrays)).</span><span class="sxs-lookup"><span data-stu-id="dd693-523">The arguments of indexers ([Indexers](classes.md#indexers)) are always passed as value parameters ([Value parameters](classes.md#value-parameters)) or parameter arrays ([Parameter arrays](classes.md#parameter-arrays)).</span></span> <span data-ttu-id="dd693-524">Parâmetros de referência e saída não têm suporte para essas categorias de membros de função.</span><span class="sxs-lookup"><span data-stu-id="dd693-524">Reference and output parameters are not supported for these categories of function members.</span></span>

<span data-ttu-id="dd693-525">Os argumentos de um construtor de instância, método, indexador ou invocação de delegado são especificados como um *argument_list*:</span><span class="sxs-lookup"><span data-stu-id="dd693-525">The arguments of an instance constructor, method, indexer or delegate invocation are specified as an *argument_list*:</span></span>

```antlr
argument_list
    : argument (',' argument)*
    ;

argument
    : argument_name? argument_value
    ;

argument_name
    : identifier ':'
    ;

argument_value
    : expression
    | 'ref' variable_reference
    | 'out' variable_reference
    ;
```

<span data-ttu-id="dd693-526">Um *argument_list* consiste em um ou mais s de *argumento*, separados por vírgulas.</span><span class="sxs-lookup"><span data-stu-id="dd693-526">An *argument_list* consists of one or more *argument*s, separated by commas.</span></span> <span data-ttu-id="dd693-527">Cada argumento consiste em um *argument_name* opcional seguido por um *argument_value*.</span><span class="sxs-lookup"><span data-stu-id="dd693-527">Each argument consists of an optional  *argument_name* followed by an *argument_value*.</span></span> <span data-ttu-id="dd693-528">Um *argumento* com um *argument_name* é conhecido como um ***argumento nomeado***, enquanto que um *argumento* sem um *argument_name* é um ***argumento posicional***.</span><span class="sxs-lookup"><span data-stu-id="dd693-528">An *argument* with an *argument_name* is referred to as a ***named argument***, whereas an *argument* without an *argument_name* is a ***positional argument***.</span></span> <span data-ttu-id="dd693-529">É um erro para que um argumento posicional apareça após um argumento nomeado em um *argument_list*.</span><span class="sxs-lookup"><span data-stu-id="dd693-529">It is an error for a positional argument to appear after a named argument in an *argument_list*.</span></span>

<span data-ttu-id="dd693-530">O *argument_value* pode ter um dos seguintes formatos:</span><span class="sxs-lookup"><span data-stu-id="dd693-530">The *argument_value* can take one of the following forms:</span></span>

*  <span data-ttu-id="dd693-531">Uma *expressão*, indicando que o argumento é passado como um parâmetro de valor ([parâmetros de valor](classes.md#value-parameters)).</span><span class="sxs-lookup"><span data-stu-id="dd693-531">An *expression*, indicating that the argument is passed as a value parameter ([Value parameters](classes.md#value-parameters)).</span></span>
*  <span data-ttu-id="dd693-532">A palavra-chave `ref` seguida por uma *variable_reference* ([referências de variáveis](variables.md#variable-references)), indicando que o argumento é passado como um parâmetro de referência ([parâmetros de referência](classes.md#reference-parameters)).</span><span class="sxs-lookup"><span data-stu-id="dd693-532">The keyword `ref` followed by a *variable_reference* ([Variable references](variables.md#variable-references)), indicating that the argument is passed as a reference parameter ([Reference parameters](classes.md#reference-parameters)).</span></span> <span data-ttu-id="dd693-533">Uma variável deve ser definitivamente atribuída ([atribuição definitiva](variables.md#definite-assignment)) antes de poder ser passada como um parâmetro de referência.</span><span class="sxs-lookup"><span data-stu-id="dd693-533">A variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) before it can be passed as a reference parameter.</span></span> <span data-ttu-id="dd693-534">A palavra-chave `out` seguida por uma *variable_reference* ([referências de variáveis](variables.md#variable-references)), indicando que o argumento é passado como um parâmetro de saída ([parâmetros de saída](classes.md#output-parameters)).</span><span class="sxs-lookup"><span data-stu-id="dd693-534">The keyword `out` followed by a *variable_reference* ([Variable references](variables.md#variable-references)), indicating that the argument is passed as an output parameter ([Output parameters](classes.md#output-parameters)).</span></span> <span data-ttu-id="dd693-535">Uma variável é considerada definitivamente atribuída ([atribuição definitiva](variables.md#definite-assignment)) após uma invocação de membro de função na qual a variável é passada como um parâmetro de saída.</span><span class="sxs-lookup"><span data-stu-id="dd693-535">A variable is considered definitely assigned ([Definite assignment](variables.md#definite-assignment)) following a function member invocation in which the variable is passed as an output parameter.</span></span>

#### <a name="corresponding-parameters"></a><span data-ttu-id="dd693-536">Parâmetros correspondentes</span><span class="sxs-lookup"><span data-stu-id="dd693-536">Corresponding parameters</span></span>

<span data-ttu-id="dd693-537">Para cada argumento em uma lista de argumentos, deve haver um parâmetro correspondente no membro da função ou no delegado que está sendo invocado.</span><span class="sxs-lookup"><span data-stu-id="dd693-537">For each argument in an argument list there has to be a corresponding parameter in the function member or delegate being invoked.</span></span>

<span data-ttu-id="dd693-538">A lista de parâmetros usada no seguinte é determinada como a seguir:</span><span class="sxs-lookup"><span data-stu-id="dd693-538">The parameter list used in the following is determined as follows:</span></span>

*  <span data-ttu-id="dd693-539">Para métodos virtuais e indexadores definidos em classes, a lista de parâmetros é escolhida da declaração mais específica ou substituição do membro da função, começando com o tipo estático do receptor e pesquisando suas classes base.</span><span class="sxs-lookup"><span data-stu-id="dd693-539">For virtual methods and indexers defined in classes, the parameter list is picked from the most specific declaration or override of the function member, starting with the static type of the receiver, and searching through its base classes.</span></span>
*  <span data-ttu-id="dd693-540">Para métodos de interface e indexadores, a lista de parâmetros é escolhida para formar a definição mais específica do membro, começando com o tipo de interface e pesquisando as interfaces base.</span><span class="sxs-lookup"><span data-stu-id="dd693-540">For interface methods and indexers, the parameter list is picked form the most specific definition of the member, starting with the interface type and searching through the base interfaces.</span></span> <span data-ttu-id="dd693-541">Se nenhuma lista de parâmetros exclusiva for encontrada, uma lista de parâmetros com nomes inacessíveis e nenhum parâmetro opcional será construído, de modo que as invocações não poderão usar parâmetros nomeados nem omitir argumentos opcionais.</span><span class="sxs-lookup"><span data-stu-id="dd693-541">If no unique parameter list is found, a parameter list with inaccessible names and no optional parameters is constructed, so that invocations cannot use named parameters or omit optional arguments.</span></span>
*  <span data-ttu-id="dd693-542">Para métodos parciais, a lista de parâmetros da declaração de método parcial de definição é usada.</span><span class="sxs-lookup"><span data-stu-id="dd693-542">For partial methods, the parameter list of the defining partial method declaration is used.</span></span>
*  <span data-ttu-id="dd693-543">Para todos os outros membros de função e delegados, há apenas uma única lista de parâmetros, que é a usada.</span><span class="sxs-lookup"><span data-stu-id="dd693-543">For all other function members and delegates there is only a single parameter list, which is the one used.</span></span>

<span data-ttu-id="dd693-544">A posição de um argumento ou parâmetro é definida como o número de argumentos ou parâmetros que o antecedem na lista de argumentos ou na lista de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="dd693-544">The position of an argument or parameter is defined as the number of arguments or parameters preceding it in the argument list or parameter list.</span></span>

<span data-ttu-id="dd693-545">Os parâmetros correspondentes para argumentos de membro de função são estabelecidos da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="dd693-545">The corresponding parameters for function member arguments are established as follows:</span></span>

*  <span data-ttu-id="dd693-546">Argumentos na *argument_list* de construtores de instância, métodos, indexadores e delegados:</span><span class="sxs-lookup"><span data-stu-id="dd693-546">Arguments in the *argument_list* of instance constructors, methods, indexers and delegates:</span></span>
    * <span data-ttu-id="dd693-547">Um argumento posicional em que um parâmetro fixo ocorre na mesma posição na lista de parâmetros corresponde a esse parâmetro.</span><span class="sxs-lookup"><span data-stu-id="dd693-547">A positional argument where a fixed parameter occurs at the same position in the parameter list corresponds to that parameter.</span></span>
    * <span data-ttu-id="dd693-548">Um argumento posicional de um membro de função com uma matriz de parâmetros invocada em seu formato normal corresponde à matriz de parâmetros, que deve ocorrer na mesma posição na lista de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="dd693-548">A positional argument of a function member with a parameter array invoked in its normal form corresponds to the parameter  array, which must occur at the same position in the parameter list.</span></span>
    * <span data-ttu-id="dd693-549">Um argumento posicional de um membro de função com uma matriz de parâmetro invocada em sua forma expandida, em que nenhum parâmetro fixo ocorre na mesma posição na lista de parâmetros, corresponde a um elemento na matriz de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="dd693-549">A positional argument of a function member with a parameter array invoked in its expanded form, where no fixed parameter occurs at the same position in the parameter list, corresponds to an element in the parameter array.</span></span>
    * <span data-ttu-id="dd693-550">Um argumento nomeado corresponde ao parâmetro de mesmo nome na lista de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="dd693-550">A named argument corresponds to the parameter of the same name in the parameter list.</span></span>
    * <span data-ttu-id="dd693-551">Para indexadores, ao invocar o acessador de `set`, a expressão especificada como o operando à direita do operador de atribuição corresponde ao parâmetro de `value` implícito da declaração de acessador de `set`.</span><span class="sxs-lookup"><span data-stu-id="dd693-551">For indexers, when invoking the `set` accessor, the expression specified as the right operand of the assignment operator corresponds to the implicit `value` parameter of the `set` accessor declaration.</span></span>
*  <span data-ttu-id="dd693-552">Para propriedades, ao invocar o acessador de `get`, não há argumentos.</span><span class="sxs-lookup"><span data-stu-id="dd693-552">For properties, when invoking the `get` accessor there are no arguments.</span></span> <span data-ttu-id="dd693-553">Ao invocar o acessador de `set`, a expressão especificada como o operando à direita do operador de atribuição corresponde ao parâmetro de `value` implícito da declaração de acessador de `set`.</span><span class="sxs-lookup"><span data-stu-id="dd693-553">When invoking the `set` accessor, the expression specified as the right operand of the assignment operator corresponds to the implicit `value` parameter of the `set` accessor declaration.</span></span>
*  <span data-ttu-id="dd693-554">Para operadores unários definidos pelo usuário (incluindo conversões), o único operando corresponde ao parâmetro único da declaração do operador.</span><span class="sxs-lookup"><span data-stu-id="dd693-554">For user-defined unary operators (including conversions), the single operand corresponds to the single parameter of the operator declaration.</span></span>
*  <span data-ttu-id="dd693-555">Para operadores binários definidos pelo usuário, o operando esquerdo corresponde ao primeiro parâmetro e o operando direito corresponde ao segundo parâmetro da declaração do operador.</span><span class="sxs-lookup"><span data-stu-id="dd693-555">For user-defined binary operators, the left operand corresponds to the first parameter, and the right operand corresponds to the second parameter of the operator declaration.</span></span>

#### <a name="run-time-evaluation-of-argument-lists"></a><span data-ttu-id="dd693-556">Avaliação de tempo de execução de listas de argumentos</span><span class="sxs-lookup"><span data-stu-id="dd693-556">Run-time evaluation of argument lists</span></span>

<span data-ttu-id="dd693-557">Durante o processamento de tempo de execução de uma invocação de membro de função ([verificação de tempo de compilação da resolução dinâmica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), as expressões ou referências de variáveis de uma lista de argumentos são avaliadas na ordem, da esquerda para a direita, da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="dd693-557">During the run-time processing of a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), the expressions or variable references of an argument list are evaluated in order, from left to right, as follows:</span></span>

*  <span data-ttu-id="dd693-558">Para um parâmetro de valor, a expressão de argumento é avaliada e uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)) para o tipo de parâmetro correspondente é executada.</span><span class="sxs-lookup"><span data-stu-id="dd693-558">For a value parameter, the argument expression is evaluated and an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to the corresponding parameter type is performed.</span></span> <span data-ttu-id="dd693-559">O valor resultante torna-se o valor inicial do parâmetro value na invocação de membro da função.</span><span class="sxs-lookup"><span data-stu-id="dd693-559">The resulting value becomes the initial value of the value parameter in the function member invocation.</span></span>
*  <span data-ttu-id="dd693-560">Para um parâmetro de referência ou saída, a referência de variável é avaliada e o local de armazenamento resultante torna-se o local de armazenamento representado pelo parâmetro na invocação de membro da função.</span><span class="sxs-lookup"><span data-stu-id="dd693-560">For a reference or output parameter, the variable reference is evaluated and the resulting storage location becomes the storage location represented by the parameter in the function member invocation.</span></span> <span data-ttu-id="dd693-561">Se a referência de variável fornecida como um parâmetro de referência ou de saída for um elemento de matriz de um *reference_type*, uma verificação de tempo de execução será executada para garantir que o tipo de elemento da matriz seja idêntico ao tipo do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="dd693-561">If the variable reference given as a reference or output parameter is an array element of a *reference_type*, a run-time check is performed to ensure that the element type of the array is identical to the type of the parameter.</span></span> <span data-ttu-id="dd693-562">Se essa verificação falhar, uma `System.ArrayTypeMismatchException` será lançada.</span><span class="sxs-lookup"><span data-stu-id="dd693-562">If this check fails, a `System.ArrayTypeMismatchException` is thrown.</span></span>

<span data-ttu-id="dd693-563">Métodos, indexadores e construtores de instância podem declarar seu parâmetro mais à direita para ser uma matriz de parâmetros ([matrizes de parâmetros](classes.md#parameter-arrays)).</span><span class="sxs-lookup"><span data-stu-id="dd693-563">Methods, indexers, and instance constructors may declare their right-most parameter to be a parameter array ([Parameter arrays](classes.md#parameter-arrays)).</span></span> <span data-ttu-id="dd693-564">Esses membros de função são invocados em seu formato normal ou em sua forma expandida, dependendo do que é aplicável ([membro de função aplicável](expressions.md#applicable-function-member)):</span><span class="sxs-lookup"><span data-stu-id="dd693-564">Such function members are invoked either in their normal form or in their expanded form depending on which is applicable ([Applicable function member](expressions.md#applicable-function-member)):</span></span>

*  <span data-ttu-id="dd693-565">Quando um membro de função com uma matriz de parâmetros é invocado em seu formato normal, o argumento fornecido para a matriz de parâmetros deve ser uma única expressão que é implicitamente conversível ([conversões implícitas](conversions.md#implicit-conversions)) para o tipo de matriz de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="dd693-565">When a function member with a parameter array is invoked in its normal form, the argument given for the parameter array must be a single expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the parameter array type.</span></span> <span data-ttu-id="dd693-566">Nesse caso, a matriz de parâmetros funciona precisamente como um parâmetro de valor.</span><span class="sxs-lookup"><span data-stu-id="dd693-566">In this case, the parameter array acts precisely like a value parameter.</span></span>
*  <span data-ttu-id="dd693-567">Quando um membro de função com uma matriz de parâmetros é invocado em seu formato expandido, a invocação deve especificar zero ou mais argumentos posicionais para a matriz de parâmetros, em que cada argumento é uma expressão que é implicitamente conversível ([conversões implícitas](conversions.md#implicit-conversions)) para o tipo de elemento da matriz de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="dd693-567">When a function member with a parameter array is invoked in its expanded form, the invocation must specify zero or more positional arguments for the parameter array, where each argument is an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the element type of the parameter array.</span></span> <span data-ttu-id="dd693-568">Nesse caso, a invocação cria uma instância do tipo de matriz de parâmetros com um comprimento correspondente ao número de argumentos, inicializa os elementos da instância de matriz com os valores de argumento fornecidos e usa a instância de matriz recém-criada como o real argumento.</span><span class="sxs-lookup"><span data-stu-id="dd693-568">In this case, the invocation creates an instance of the parameter array type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="dd693-569">As expressões de uma lista de argumentos são sempre avaliadas na ordem em que são gravadas.</span><span class="sxs-lookup"><span data-stu-id="dd693-569">The expressions of an argument list are always evaluated in the order they are written.</span></span> <span data-ttu-id="dd693-570">Portanto, o exemplo</span><span class="sxs-lookup"><span data-stu-id="dd693-570">Thus, the example</span></span>
```csharp
class Test
{
    static void F(int x, int y = -1, int z = -2) {
        System.Console.WriteLine("x = {0}, y = {1}, z = {2}", x, y, z);
    }

    static void Main() {
        int i = 0;
        F(i++, i++, i++);
        F(z: i++, x: i++);
    }
}
```
<span data-ttu-id="dd693-571">produz a saída</span><span class="sxs-lookup"><span data-stu-id="dd693-571">produces the output</span></span>
```console
x = 0, y = 1, z = 2
x = 4, y = -1, z = 3
```

<span data-ttu-id="dd693-572">As regras de covariância de matriz ([covariância de matriz](arrays.md#array-covariance)) permitem que um valor de um tipo de matriz `A[]` seja uma referência a uma instância de um tipo de matriz `B[]`, desde que exista uma conversão de referência implícita de `B` para `A`.</span><span class="sxs-lookup"><span data-stu-id="dd693-572">The array co-variance rules ([Array covariance](arrays.md#array-covariance)) permit a value of an array type `A[]` to be a reference to an instance of an array type `B[]`, provided an implicit reference conversion exists from `B` to `A`.</span></span> <span data-ttu-id="dd693-573">Devido a essas regras, quando um elemento de matriz de um *reference_type* é passado como um parâmetro de referência ou de saída, uma verificação de tempo de execução é necessária para garantir que o tipo de elemento real da matriz seja idêntico ao do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="dd693-573">Because of these rules, when an array element of a *reference_type* is passed as a reference or output parameter, a run-time check is required to ensure that the actual element type of the array is identical to that of the parameter.</span></span> <span data-ttu-id="dd693-574">No exemplo</span><span class="sxs-lookup"><span data-stu-id="dd693-574">In the example</span></span>
```csharp
class Test
{
    static void F(ref object x) {...}

    static void Main() {
        object[] a = new object[10];
        object[] b = new string[10];
        F(ref a[0]);        // Ok
        F(ref b[1]);        // ArrayTypeMismatchException
    }
}
```
<span data-ttu-id="dd693-575">a segunda invocação de `F` faz com que uma `System.ArrayTypeMismatchException` seja gerada porque o tipo de elemento real de `b` é `string` e não `object`.</span><span class="sxs-lookup"><span data-stu-id="dd693-575">the second invocation of `F` causes a `System.ArrayTypeMismatchException` to be thrown because the actual element type of `b` is `string` and not `object`.</span></span>

<span data-ttu-id="dd693-576">Quando um membro de função com uma matriz de parâmetros é invocado em seu formato expandido, a invocação é processada exatamente como se uma expressão de criação de matriz com um inicializador de matriz ([expressões de criação de matriz](expressions.md#array-creation-expressions)) foi inserida em volta dos parâmetros expandidos.</span><span class="sxs-lookup"><span data-stu-id="dd693-576">When a function member with a parameter array is invoked in its expanded form, the invocation is processed exactly as if an array creation expression with an array initializer ([Array creation expressions](expressions.md#array-creation-expressions)) was inserted around the expanded parameters.</span></span> <span data-ttu-id="dd693-577">Por exemplo, dada a declaração</span><span class="sxs-lookup"><span data-stu-id="dd693-577">For example, given the declaration</span></span>
```csharp
void F(int x, int y, params object[] args);
```
<span data-ttu-id="dd693-578">as seguintes invocações da forma expandida do método</span><span class="sxs-lookup"><span data-stu-id="dd693-578">the following invocations of the expanded form of the method</span></span>
```csharp
F(10, 20);
F(10, 20, 30, 40);
F(10, 20, 1, "hello", 3.0);
```
<span data-ttu-id="dd693-579">corresponder exatamente a</span><span class="sxs-lookup"><span data-stu-id="dd693-579">correspond exactly to</span></span>
```csharp
F(10, 20, new object[] {});
F(10, 20, new object[] {30, 40});
F(10, 20, new object[] {1, "hello", 3.0});
```

<span data-ttu-id="dd693-580">Em particular, observe que uma matriz vazia é criada quando há zero argumentos fornecidos para a matriz de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="dd693-580">In particular, note that an empty array is created when there are zero arguments given for the parameter array.</span></span>

<span data-ttu-id="dd693-581">Quando os argumentos são omitidos de um membro de função com parâmetros opcionais correspondentes, os argumentos padrão da declaração de membro de função são passados implicitamente.</span><span class="sxs-lookup"><span data-stu-id="dd693-581">When arguments are omitted from a function member with corresponding optional parameters, the default arguments of the function member declaration are implicitly passed.</span></span> <span data-ttu-id="dd693-582">Como elas são sempre constantes, sua avaliação não afetará a ordem de avaliação dos argumentos restantes.</span><span class="sxs-lookup"><span data-stu-id="dd693-582">Because these are always constant, their evaluation will not impact the evaluation order of the remaining arguments.</span></span>

### <a name="type-inference"></a><span data-ttu-id="dd693-583">Inferência de tipos</span><span class="sxs-lookup"><span data-stu-id="dd693-583">Type inference</span></span>

<span data-ttu-id="dd693-584">Quando um método genérico é chamado sem especificar argumentos de tipo, um processo de ***inferência de tipos*** tenta inferir argumentos de tipo para a chamada.</span><span class="sxs-lookup"><span data-stu-id="dd693-584">When a generic method is called without specifying type arguments, a ***type inference*** process attempts to infer type arguments for the call.</span></span> <span data-ttu-id="dd693-585">A presença da inferência de tipos permite que uma sintaxe mais conveniente seja usada para chamar um método genérico e permite que o programador Evite especificar informações de tipo redundante.</span><span class="sxs-lookup"><span data-stu-id="dd693-585">The presence of type inference allows a more convenient syntax to be used for calling a generic method, and allows the programmer to avoid specifying redundant type information.</span></span> <span data-ttu-id="dd693-586">Por exemplo, dada a declaração do método:</span><span class="sxs-lookup"><span data-stu-id="dd693-586">For example, given the method declaration:</span></span>
```csharp
class Chooser
{
    static Random rand = new Random();

    public static T Choose<T>(T first, T second) {
        return (rand.Next(2) == 0)? first: second;
    }
}
```
<span data-ttu-id="dd693-587">é possível invocar o método `Choose` sem especificar explicitamente um argumento de tipo:</span><span class="sxs-lookup"><span data-stu-id="dd693-587">it is possible to invoke the `Choose` method without explicitly specifying a type argument:</span></span>
```csharp
int i = Chooser.Choose(5, 213);                 // Calls Choose<int>

string s = Chooser.Choose("foo", "bar");        // Calls Choose<string>
```

<span data-ttu-id="dd693-588">Por meio da inferência de tipos, os argumentos de tipo `int` e `string` são determinados dos argumentos para o método.</span><span class="sxs-lookup"><span data-stu-id="dd693-588">Through type inference, the type arguments `int` and `string` are determined from the arguments to the method.</span></span>

<span data-ttu-id="dd693-589">A inferência de tipos ocorre como parte do processamento do tempo de associação de uma invocação de método ([invocações de método](expressions.md#method-invocations)) e ocorre antes da etapa de resolução de sobrecarga da invocação.</span><span class="sxs-lookup"><span data-stu-id="dd693-589">Type inference occurs as part of the binding-time processing of a method invocation ([Method invocations](expressions.md#method-invocations)) and takes place before the overload resolution step of the invocation.</span></span> <span data-ttu-id="dd693-590">Quando um grupo de métodos específico é especificado em uma invocação de método e nenhum argumento de tipo é especificado como parte da invocação de método, a inferência de tipos é aplicada a cada método genérico no grupo de métodos.</span><span class="sxs-lookup"><span data-stu-id="dd693-590">When a particular method group is specified in a method invocation, and no type arguments are specified as part of the method invocation, type inference is applied to each generic method in the method group.</span></span> <span data-ttu-id="dd693-591">Se a inferência de tipos for realizada com sucesso, os argumentos de tipo deduzidos serão usados para determinar os tipos de argumentos para resolução de sobrecarga subsequente.</span><span class="sxs-lookup"><span data-stu-id="dd693-591">If type inference succeeds, then the inferred type arguments are used to determine the types of arguments for subsequent overload resolution.</span></span> <span data-ttu-id="dd693-592">Se a resolução de sobrecarga escolher um método genérico como o para invocar, os argumentos de tipo inferido serão usados como os argumentos de tipo real para a invocação.</span><span class="sxs-lookup"><span data-stu-id="dd693-592">If overload resolution chooses a generic method as the one to invoke, then the inferred type arguments are used as the actual type arguments for the invocation.</span></span> <span data-ttu-id="dd693-593">Se a inferência de tipos para um determinado método falhar, esse método não participará da resolução de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="dd693-593">If type inference for a particular method fails, that method does not participate in overload resolution.</span></span> <span data-ttu-id="dd693-594">A falha da inferência de tipos, por si só, não causa um erro de tempo de ligação.</span><span class="sxs-lookup"><span data-stu-id="dd693-594">The failure of type inference, in and of itself, does not cause a binding-time error.</span></span> <span data-ttu-id="dd693-595">No entanto, geralmente leva a um erro de tempo de ligação quando a resolução de sobrecarga não consegue encontrar nenhum método aplicável.</span><span class="sxs-lookup"><span data-stu-id="dd693-595">However, it often leads to a binding-time error when overload resolution then fails to find any applicable methods.</span></span>

<span data-ttu-id="dd693-596">Se o número de argumentos fornecido for diferente do número de parâmetros no método, a inferência falhará imediatamente.</span><span class="sxs-lookup"><span data-stu-id="dd693-596">If the supplied number of arguments is different than the number of parameters in the method, then inference immediately fails.</span></span> <span data-ttu-id="dd693-597">Caso contrário, suponha que o método genérico tenha a seguinte assinatura:</span><span class="sxs-lookup"><span data-stu-id="dd693-597">Otherwise, assume that the generic method has the following signature:</span></span>
```csharp
Tr M<X1,...,Xn>(T1 x1, ..., Tm xm)
```

<span data-ttu-id="dd693-598">Com uma chamada de método do formulário `M(E1...Em)` a tarefa de tipo inferência é encontrar argumentos de tipo exclusivo `S1...Sn` para cada um dos parâmetros de tipo `X1...Xn` de forma que a chamada `M<S1...Sn>(E1...Em)` se torne válida.</span><span class="sxs-lookup"><span data-stu-id="dd693-598">With a method call of the form `M(E1...Em)` the task of type inference is to find unique type arguments `S1...Sn` for each of the type parameters `X1...Xn` so that the call `M<S1...Sn>(E1...Em)` becomes valid.</span></span>

<span data-ttu-id="dd693-599">Durante o processo de inferência, cada parâmetro de tipo `Xi` é *corrigido* para um tipo específico `Si` ou não *fixo* com um conjunto associado de *limites*.</span><span class="sxs-lookup"><span data-stu-id="dd693-599">During the process of inference each type parameter `Xi` is either *fixed* to a particular type `Si` or *unfixed* with an associated set of *bounds*.</span></span> <span data-ttu-id="dd693-600">Cada um dos limites é algum tipo `T`.</span><span class="sxs-lookup"><span data-stu-id="dd693-600">Each of the bounds is some type `T`.</span></span> <span data-ttu-id="dd693-601">Inicialmente, cada variável de tipo `Xi` é desfixada com um conjunto vazio de limites.</span><span class="sxs-lookup"><span data-stu-id="dd693-601">Initially each type variable `Xi` is unfixed with an empty set of bounds.</span></span>

<span data-ttu-id="dd693-602">A inferência de tipos ocorre em fases.</span><span class="sxs-lookup"><span data-stu-id="dd693-602">Type inference takes place in phases.</span></span> <span data-ttu-id="dd693-603">Cada fase tentará inferir argumentos de tipo para mais variáveis de tipo com base nas conclusões da fase anterior.</span><span class="sxs-lookup"><span data-stu-id="dd693-603">Each phase will try to infer type arguments for more type variables based on the findings of the previous phase.</span></span> <span data-ttu-id="dd693-604">A primeira fase faz algumas inferências iniciais de limites, enquanto a segunda fase corrige variáveis de tipo para tipos específicos e infere limites adicionais.</span><span class="sxs-lookup"><span data-stu-id="dd693-604">The first phase makes some initial inferences of bounds, whereas the second phase fixes type variables to specific types and infers further bounds.</span></span> <span data-ttu-id="dd693-605">A segunda fase pode ter que ser repetida várias vezes.</span><span class="sxs-lookup"><span data-stu-id="dd693-605">The second phase may have to be repeated a number of times.</span></span>

<span data-ttu-id="dd693-606">*Observação:* A inferência de tipos ocorre não apenas quando um método genérico é chamado.</span><span class="sxs-lookup"><span data-stu-id="dd693-606">*Note:* Type inference takes place not only when a generic method is called.</span></span> <span data-ttu-id="dd693-607">A inferência de tipos para a conversão de grupos de métodos é descrita na [inferência de tipos para a conversão de grupos de métodos](expressions.md#type-inference-for-conversion-of-method-groups) e a localização do melhor tipo comum de um conjunto de expressões é descrita na [localização do melhor tipo comum de um conjunto de expressões](expressions.md#finding-the-best-common-type-of-a-set-of-expressions).</span><span class="sxs-lookup"><span data-stu-id="dd693-607">Type inference for conversion of method groups is described in [Type inference for conversion of method groups](expressions.md#type-inference-for-conversion-of-method-groups) and finding the best common type of a set of expressions is described in [Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions).</span></span>

#### <a name="the-first-phase"></a><span data-ttu-id="dd693-608">A primeira fase</span><span class="sxs-lookup"><span data-stu-id="dd693-608">The first phase</span></span>

<span data-ttu-id="dd693-609">Para cada um dos argumentos do método `Ei`:</span><span class="sxs-lookup"><span data-stu-id="dd693-609">For each of the method arguments `Ei`:</span></span>

*   <span data-ttu-id="dd693-610">Se `Ei` for uma função anônima, uma *inferência de tipo de parâmetro explícita* ([induçãos de tipo de parâmetro explícita](expressions.md#explicit-parameter-type-inferences)) será feita de `Ei` para `Ti`</span><span class="sxs-lookup"><span data-stu-id="dd693-610">If `Ei` is an anonymous function, an *explicit parameter type inference* ([Explicit parameter type inferences](expressions.md#explicit-parameter-type-inferences)) is made from `Ei` to `Ti`</span></span>
*   <span data-ttu-id="dd693-611">Caso contrário, se `Ei` tiver um tipo `U` e `xi` for um parâmetro de valor, uma *inferência de limite inferior* será feita *de* `U` *para* `Ti`.</span><span class="sxs-lookup"><span data-stu-id="dd693-611">Otherwise, if `Ei` has a type `U` and `xi` is a value parameter then a *lower-bound inference* is made *from* `U` *to* `Ti`.</span></span>
*   <span data-ttu-id="dd693-612">Caso contrário, se `Ei` tiver um tipo `U` e `xi` for um parâmetro `ref` ou `out`, uma *inferência exata* será feita *de* `U` *para* `Ti`.</span><span class="sxs-lookup"><span data-stu-id="dd693-612">Otherwise, if `Ei` has a type `U` and `xi` is a `ref` or `out` parameter then an *exact inference* is made *from* `U` *to* `Ti`.</span></span>
*   <span data-ttu-id="dd693-613">Caso contrário, nenhuma inferência é feita para esse argumento.</span><span class="sxs-lookup"><span data-stu-id="dd693-613">Otherwise, no inference is made for this argument.</span></span>


#### <a name="the-second-phase"></a><span data-ttu-id="dd693-614">A segunda fase</span><span class="sxs-lookup"><span data-stu-id="dd693-614">The second phase</span></span>

<span data-ttu-id="dd693-615">A segunda fase continua da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="dd693-615">The second phase proceeds as follows:</span></span>

*   <span data-ttu-id="dd693-616">Todas as variáveis de tipo não *fixas* `Xi` que não *dependem* ([dependência](expressions.md#dependence)) quaisquer `Xj` são fixas ([corrigindo](expressions.md#fixing)).</span><span class="sxs-lookup"><span data-stu-id="dd693-616">All *unfixed* type variables `Xi` which do not *depend on* ([Dependence](expressions.md#dependence)) any `Xj` are fixed ([Fixing](expressions.md#fixing)).</span></span>
*   <span data-ttu-id="dd693-617">Se não existir nenhuma variável de tipo, todas as variáveis de tipo não *fixas* `Xi` serão *corrigidas* para cada uma das seguintes isenções:</span><span class="sxs-lookup"><span data-stu-id="dd693-617">If no such type variables exist, all *unfixed* type variables `Xi` are *fixed* for which all of the following hold:</span></span>
    *   <span data-ttu-id="dd693-618">Há pelo menos uma variável de tipo `Xj` que depende `Xi`</span><span class="sxs-lookup"><span data-stu-id="dd693-618">There is at least one type variable `Xj` that depends on `Xi`</span></span>
    *   <span data-ttu-id="dd693-619">`Xi` tem um conjunto não vazio de limites</span><span class="sxs-lookup"><span data-stu-id="dd693-619">`Xi` has a non-empty set of bounds</span></span>
*   <span data-ttu-id="dd693-620">Se nenhuma das variáveis de tipo existirem e ainda houver variáveis de tipo não *fixas* , a inferência de tipos falhará.</span><span class="sxs-lookup"><span data-stu-id="dd693-620">If no such type variables exist and there are still *unfixed* type variables, type inference fails.</span></span>
*   <span data-ttu-id="dd693-621">Caso contrário, se nenhuma variável de tipo não *fixa* existir, a inferência de tipos terá sucesso.</span><span class="sxs-lookup"><span data-stu-id="dd693-621">Otherwise, if no further *unfixed* type variables exist, type inference succeeds.</span></span>
*   <span data-ttu-id="dd693-622">Caso contrário, para todos os argumentos `Ei` com o tipo de parâmetro correspondente `Ti` em que os *tipos de saída* ([tipos de saída](expressions.md#output-types)) contêm variáveis de tipo não *fixos* `Xj` mas os *tipos de entrada* (tipos de[entrada](expressions.md#input-types)) não são, uma *inferência de tipo de saída* (inferências de[tipo de saída](expressions.md#output-type-inferences)) é feita *de* `Ei` *para* `Ti`.</span><span class="sxs-lookup"><span data-stu-id="dd693-622">Otherwise, for all arguments `Ei` with corresponding parameter type `Ti` where the *output types* ([Output types](expressions.md#output-types)) contain *unfixed* type variables `Xj` but the *input types* ([Input types](expressions.md#input-types)) do not, an *output type inference* ([Output type inferences](expressions.md#output-type-inferences)) is made *from* `Ei` *to* `Ti`.</span></span> <span data-ttu-id="dd693-623">Em seguida, a segunda fase é repetida.</span><span class="sxs-lookup"><span data-stu-id="dd693-623">Then the second phase is repeated.</span></span>

#### <a name="input-types"></a><span data-ttu-id="dd693-624">Tipos de entrada</span><span class="sxs-lookup"><span data-stu-id="dd693-624">Input types</span></span>

<span data-ttu-id="dd693-625">Se `E` for um grupo de métodos ou uma função anônima digitada implicitamente e `T` for um tipo delegado ou tipo de árvore de expressão, todos os tipos de parâmetro de `T` serão *tipos de entrada* de `E` *com o tipo* `T`.</span><span class="sxs-lookup"><span data-stu-id="dd693-625">If `E` is a method group or implicitly typed anonymous function and `T` is a delegate type or expression tree type then all the parameter types of `T` are *input types* of `E` *with type* `T`.</span></span>

####  <a name="output-types"></a><span data-ttu-id="dd693-626">Tipos de saída</span><span class="sxs-lookup"><span data-stu-id="dd693-626">Output types</span></span>

<span data-ttu-id="dd693-627">Se `E` for um grupo de métodos ou uma função anônima e `T` for um tipo delegado ou tipo de árvore de expressão, o tipo de retorno de `T` será um *tipo de saída de* `E` *com o tipo* `T`.</span><span class="sxs-lookup"><span data-stu-id="dd693-627">If `E` is a method group or an anonymous function and `T` is a delegate type or expression tree type then the return type of `T` is an *output type of* `E` *with type* `T`.</span></span>

#### <a name="dependence"></a><span data-ttu-id="dd693-628">Dependência</span><span class="sxs-lookup"><span data-stu-id="dd693-628">Dependence</span></span>

<span data-ttu-id="dd693-629">Uma variável de tipo não *fixa* `Xi` *depende diretamente de* uma variável de tipo não fixa `Xj` se for algum argumento `Ek` com o tipo `Tk` `Xj` ocorrer em um *tipo de entrada* de `Ek` com o tipo `Tk` e `Xi` ocorrer em um tipo de *saída* de `Ek` com o tipo `Tk`.</span><span class="sxs-lookup"><span data-stu-id="dd693-629">An *unfixed* type variable `Xi` *depends directly on* an unfixed type variable `Xj` if for some argument `Ek` with type `Tk` `Xj` occurs in an *input type* of `Ek` with type `Tk` and `Xi` occurs in an *output type* of `Ek` with type `Tk`.</span></span>

<span data-ttu-id="dd693-630">`Xj` *depende* `Xi` se `Xj` *depende diretamente de* `Xi` ou se `Xi` *depende diretamente de* `Xk` e `Xk` *depende do* `Xj`.</span><span class="sxs-lookup"><span data-stu-id="dd693-630">`Xj` *depends on* `Xi` if `Xj` *depends directly on* `Xi` or if `Xi` *depends directly on* `Xk` and `Xk` *depends on* `Xj`.</span></span> <span data-ttu-id="dd693-631">Assim, "depende" é o fechamento transitivo, mas não reflexivo, de "depende diretamente de".</span><span class="sxs-lookup"><span data-stu-id="dd693-631">Thus "depends on" is the transitive but not reflexive closure of "depends directly on".</span></span>

#### <a name="output-type-inferences"></a><span data-ttu-id="dd693-632">Inferências de tipo de saída</span><span class="sxs-lookup"><span data-stu-id="dd693-632">Output type inferences</span></span>

<span data-ttu-id="dd693-633">Uma *inferência de tipo de saída* é feita *de* uma expressão `E` *para* um tipo `T` da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="dd693-633">An *output type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>

*  <span data-ttu-id="dd693-634">Se `E` for uma função anônima com tipo de retorno inferido `U`[(tipo de retorno inferido](expressions.md#inferred-return-type)) e `T` for um tipo delegado ou tipo de árvore de expressão com tipo de retorno `Tb`, uma *inferência de limite inferior* ([inferências de limite inferior](expressions.md#lower-bound-inferences)) será feita *de* `U` *para* `Tb`.</span><span class="sxs-lookup"><span data-stu-id="dd693-634">If `E` is an anonymous function with inferred return type  `U` ([Inferred return type](expressions.md#inferred-return-type)) and `T` is a delegate type or expression tree type with return type `Tb`, then a *lower-bound inference* ([Lower-bound inferences](expressions.md#lower-bound-inferences)) is made *from* `U` *to* `Tb`.</span></span>
*  <span data-ttu-id="dd693-635">Caso contrário, se `E` for um grupo de métodos e `T` for um tipo delegado ou tipo de árvore de expressão com tipos de parâmetro `T1...Tk` e tipo de retorno `Tb`, e a resolução de sobrecarga de `E` com os tipos `T1...Tk` gerar um único método com o tipo de retorno `U`, uma *inferência de limite inferior* será feita *de* `U` *para* `Tb`.</span><span class="sxs-lookup"><span data-stu-id="dd693-635">Otherwise, if `E` is a method group and `T` is a delegate type or expression tree type with parameter types `T1...Tk` and return type `Tb`, and overload resolution of `E` with the types `T1...Tk` yields a single method with return type `U`, then a *lower-bound inference* is made *from* `U` *to* `Tb`.</span></span>
*  <span data-ttu-id="dd693-636">Caso contrário, se `E` for uma expressão com o tipo `U`, uma *inferência de limite inferior* será feita *de* `U` *para* `T`.</span><span class="sxs-lookup"><span data-stu-id="dd693-636">Otherwise, if `E` is an expression with type `U`, then a *lower-bound inference* is made *from* `U` *to* `T`.</span></span>
*  <span data-ttu-id="dd693-637">Caso contrário, nenhuma inferência será feita.</span><span class="sxs-lookup"><span data-stu-id="dd693-637">Otherwise, no inferences are made.</span></span>

#### <a name="explicit-parameter-type-inferences"></a><span data-ttu-id="dd693-638">Inferências de tipo de parâmetro explícito</span><span class="sxs-lookup"><span data-stu-id="dd693-638">Explicit parameter type inferences</span></span>

<span data-ttu-id="dd693-639">Uma *inferência de tipo de parâmetro explícita* é feita *de* uma expressão `E` *para* um tipo `T` da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="dd693-639">An *explicit parameter type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>

*  <span data-ttu-id="dd693-640">Se `E` é uma função anônima explicitamente tipada com tipos de parâmetro `U1...Uk` e `T` é um tipo delegado ou tipo de árvore de expressão com tipos de parâmetro `V1...Vk` para cada `Ui` uma *inferência exata* ([inferências exatas](expressions.md#exact-inferences)) é feita *de* `Ui` *para* o `Vi`correspondente.</span><span class="sxs-lookup"><span data-stu-id="dd693-640">If `E` is an explicitly typed anonymous function with parameter types `U1...Uk` and `T` is a delegate type or expression tree type with parameter types `V1...Vk` then for each `Ui` an *exact inference* ([Exact inferences](expressions.md#exact-inferences)) is made *from* `Ui` *to* the corresponding `Vi`.</span></span>

#### <a name="exact-inferences"></a><span data-ttu-id="dd693-641">Inferências exatas</span><span class="sxs-lookup"><span data-stu-id="dd693-641">Exact inferences</span></span>

<span data-ttu-id="dd693-642">Uma *inferência exata* *de* um tipo `U` *a* um tipo `V` é feita da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="dd693-642">An *exact inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="dd693-643">Se `V` for um dos `Xi` não *corrigidos* , `U` será adicionado ao conjunto de limites exatos para `Xi`.</span><span class="sxs-lookup"><span data-stu-id="dd693-643">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of exact bounds for `Xi`.</span></span>

*  <span data-ttu-id="dd693-644">Caso contrário, os conjuntos `V1...Vk` e `U1...Uk` são determinados verificando se algum dos casos a seguir se aplica:</span><span class="sxs-lookup"><span data-stu-id="dd693-644">Otherwise, sets `V1...Vk` and `U1...Uk` are determined by checking if any of the following cases apply:</span></span>

   *  <span data-ttu-id="dd693-645">`V` é um tipo de matriz `V1[...]` e `U` é um tipo de matriz `U1[...]` da mesma classificação</span><span class="sxs-lookup"><span data-stu-id="dd693-645">`V` is an array type `V1[...]` and `U` is an array type `U1[...]`  of the same rank</span></span>
   *  <span data-ttu-id="dd693-646">`V` é o tipo `V1?` e `U` é o tipo `U1?`</span><span class="sxs-lookup"><span data-stu-id="dd693-646">`V` is the type `V1?` and `U` is the type `U1?`</span></span>
   *  <span data-ttu-id="dd693-647">`V` é um tipo construído `C<V1...Vk>`e `U` é um tipo construído `C<U1...Uk>`</span><span class="sxs-lookup"><span data-stu-id="dd693-647">`V` is a constructed type `C<V1...Vk>`and `U` is a constructed type `C<U1...Uk>`</span></span>

   <span data-ttu-id="dd693-648">Se qualquer um desses casos se aplicar, uma *inferência exata* será feita *de* cada `Ui` *ao* `Vi`correspondente.</span><span class="sxs-lookup"><span data-stu-id="dd693-648">If any of these cases apply then an *exact inference* is made *from* each `Ui` *to* the corresponding `Vi`.</span></span>

*  <span data-ttu-id="dd693-649">Caso contrário, nenhuma inferência será feita.</span><span class="sxs-lookup"><span data-stu-id="dd693-649">Otherwise no inferences are made.</span></span>

#### <a name="lower-bound-inferences"></a><span data-ttu-id="dd693-650">Inferências de limite inferior</span><span class="sxs-lookup"><span data-stu-id="dd693-650">Lower-bound inferences</span></span>

<span data-ttu-id="dd693-651">Uma *inferência de limite inferior* *de* um tipo `U` *a* um tipo `V` é feita da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="dd693-651">A *lower-bound inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="dd693-652">Se `V` for um dos `Xi` não *corrigidos* , `U` será adicionado ao conjunto de limites inferiores para `Xi`.</span><span class="sxs-lookup"><span data-stu-id="dd693-652">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of lower bounds for `Xi`.</span></span>
*  <span data-ttu-id="dd693-653">Caso contrário, se `V` for o tipo `V1?`e `U` for o tipo `U1?`, uma inferência de limite inferior será feita de `U1` para `V1`.</span><span class="sxs-lookup"><span data-stu-id="dd693-653">Otherwise, if `V` is the type `V1?`and `U` is the type `U1?` then a lower bound inference is made from `U1` to `V1`.</span></span>
*  <span data-ttu-id="dd693-654">Caso contrário, os conjuntos `U1...Uk` e `V1...Vk` são determinados verificando se algum dos casos a seguir se aplica:</span><span class="sxs-lookup"><span data-stu-id="dd693-654">Otherwise, sets `U1...Uk` and `V1...Vk` are determined by checking if any of the following cases apply:</span></span>
   *  <span data-ttu-id="dd693-655">`V` é um tipo de matriz `V1[...]` e `U` é um tipo de matriz `U1[...]` (ou um parâmetro de tipo cujo tipo base efetivo é `U1[...]`) da mesma classificação</span><span class="sxs-lookup"><span data-stu-id="dd693-655">`V` is an array type `V1[...]` and `U` is an array type `U1[...]` (or a type parameter whose effective base type is `U1[...]`) of the same rank</span></span>
   *  <span data-ttu-id="dd693-656">`V` é uma das `IEnumerable<V1>`, `ICollection<V1>` ou `IList<V1>` e `U` é um tipo de matriz unidimensional `U1[]`(ou um parâmetro de tipo cujo tipo base efetivo é `U1[]`)</span><span class="sxs-lookup"><span data-stu-id="dd693-656">`V` is one of `IEnumerable<V1>`, `ICollection<V1>` or `IList<V1>` and `U` is a one-dimensional array type `U1[]`(or a type parameter whose effective base type is `U1[]`)</span></span>
   *  <span data-ttu-id="dd693-657">`V` é uma classe construída, struct, interface ou tipo delegado `C<V1...Vk>` e há um tipo exclusivo `C<U1...Uk>` de tal forma que `U` (ou, se `U` for um parâmetro de tipo, sua classe base efetiva ou qualquer membro de seu conjunto de interface eficaz) é idêntica a, herda de (direta ou indiretamente) ou implementa (direta ou indiretamente) `C<U1...Uk>`.</span><span class="sxs-lookup"><span data-stu-id="dd693-657">`V` is a constructed class, struct, interface or delegate type `C<V1...Vk>` and there is a unique type `C<U1...Uk>` such that `U` (or, if `U` is a type parameter, its effective base class or any member of its effective interface set) is identical to, inherits from (directly or indirectly), or implements (directly or indirectly) `C<U1...Uk>`.</span></span>

      <span data-ttu-id="dd693-658">(A restrição de "exclusividade" significa que, na interface de caso `C<T> {} class U: C<X>, C<Y> {}`, nenhuma inferência é feita ao se inferir de `U` para `C<T>` porque `U1` poderia ser `X` ou `Y`.)</span><span class="sxs-lookup"><span data-stu-id="dd693-658">(The "uniqueness" restriction means that in the case interface `C<T> {} class U: C<X>, C<Y> {}`, then no inference is made when inferring from `U` to `C<T>` because `U1` could be `X` or `Y`.)</span></span>

   <span data-ttu-id="dd693-659">Se qualquer um desses casos se aplicar, uma inferência será feita *de* cada `Ui` *ao* `Vi` correspondente da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="dd693-659">If any of these cases apply then an inference is made *from* each `Ui` *to* the corresponding `Vi` as follows:</span></span>

   *  <span data-ttu-id="dd693-660">Se `Ui` não for conhecido como um tipo de referência, uma *inferência exata* será feita</span><span class="sxs-lookup"><span data-stu-id="dd693-660">If `Ui` is not known to be a reference type then an *exact inference* is made</span></span>
   *  <span data-ttu-id="dd693-661">Caso contrário, se `U` for um tipo de matriz, uma *inferência de limite inferior* será feita</span><span class="sxs-lookup"><span data-stu-id="dd693-661">Otherwise, if `U` is an array type then a *lower-bound inference* is made</span></span>
   *  <span data-ttu-id="dd693-662">Caso contrário, se `V` for `C<V1...Vk>`, a inferência dependerá do parâmetro de tipo i-th de `C`:</span><span class="sxs-lookup"><span data-stu-id="dd693-662">Otherwise, if `V` is `C<V1...Vk>` then inference depends on the i-th type parameter of `C`:</span></span>
      *  <span data-ttu-id="dd693-663">Se for covariant, uma *inferência de limite inferior* será feita.</span><span class="sxs-lookup"><span data-stu-id="dd693-663">If it is covariant then a *lower-bound inference* is made.</span></span>
      *  <span data-ttu-id="dd693-664">Se for contravariant, uma *inferência de limite superior* será feita.</span><span class="sxs-lookup"><span data-stu-id="dd693-664">If it is contravariant then an *upper-bound inference* is made.</span></span>
      *  <span data-ttu-id="dd693-665">Se for invariável, uma *inferência exata* será feita.</span><span class="sxs-lookup"><span data-stu-id="dd693-665">If it is invariant then an *exact inference* is made.</span></span>
*  <span data-ttu-id="dd693-666">Caso contrário, nenhuma inferência será feita.</span><span class="sxs-lookup"><span data-stu-id="dd693-666">Otherwise, no inferences are made.</span></span>

#### <a name="upper-bound-inferences"></a><span data-ttu-id="dd693-667">Inferências de limite superior</span><span class="sxs-lookup"><span data-stu-id="dd693-667">Upper-bound inferences</span></span>

<span data-ttu-id="dd693-668">Uma *inferência de limite superior* *de* um tipo `U` *a* um tipo `V` é feita da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="dd693-668">An *upper-bound inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="dd693-669">Se `V` for um dos `Xi` não *corrigidos* , `U` será adicionado ao conjunto de limites superiores para `Xi`.</span><span class="sxs-lookup"><span data-stu-id="dd693-669">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of upper bounds for `Xi`.</span></span>
*  <span data-ttu-id="dd693-670">Caso contrário, os conjuntos `V1...Vk` e `U1...Uk` são determinados verificando se algum dos casos a seguir se aplica:</span><span class="sxs-lookup"><span data-stu-id="dd693-670">Otherwise, sets `V1...Vk` and `U1...Uk` are determined by checking if any of the following cases apply:</span></span>
   *  <span data-ttu-id="dd693-671">`U` é um tipo de matriz `U1[...]` e `V` é um tipo de matriz `V1[...]` da mesma classificação</span><span class="sxs-lookup"><span data-stu-id="dd693-671">`U` is an array type `U1[...]` and `V` is an array type `V1[...]` of the same rank</span></span>
   *  <span data-ttu-id="dd693-672">`U` é uma das `IEnumerable<Ue>`, `ICollection<Ue>` ou `IList<Ue>` e `V` é um tipo de matriz unidimensional `Ve[]`</span><span class="sxs-lookup"><span data-stu-id="dd693-672">`U` is one of `IEnumerable<Ue>`, `ICollection<Ue>` or `IList<Ue>` and `V` is a one-dimensional array type `Ve[]`</span></span>
   *  <span data-ttu-id="dd693-673">`U` é o tipo `U1?` e `V` é o tipo `V1?`</span><span class="sxs-lookup"><span data-stu-id="dd693-673">`U` is the type `U1?` and `V` is the type `V1?`</span></span>
   *  <span data-ttu-id="dd693-674">`U` é uma classe construída, struct, interface ou tipo delegado `C<U1...Uk>` e `V` é uma classe, struct, interface ou tipo delegado, que é idêntica a, herda de (direta ou indiretamente) ou implementa (direta ou indiretamente) um tipo exclusivo `C<V1...Vk>`</span><span class="sxs-lookup"><span data-stu-id="dd693-674">`U` is constructed class, struct, interface or delegate type `C<U1...Uk>` and `V` is a class, struct, interface or delegate type which is identical to, inherits from (directly or indirectly), or implements (directly or indirectly) a unique type `C<V1...Vk>`</span></span>

      <span data-ttu-id="dd693-675">(A restrição de "exclusividade" significa que, se tivermos `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`, nenhuma inferência será feita ao fazer referência de `C<U1>` para `V<Q>`.</span><span class="sxs-lookup"><span data-stu-id="dd693-675">(The "uniqueness" restriction means that if we have `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`, then no inference is made when inferring from `C<U1>` to `V<Q>`.</span></span> <span data-ttu-id="dd693-676">Não são feitas inferências de `U1` para `X<Q>` ou `Y<Q>`.)</span><span class="sxs-lookup"><span data-stu-id="dd693-676">Inferences are not made from `U1` to either `X<Q>` or `Y<Q>`.)</span></span>

   <span data-ttu-id="dd693-677">Se qualquer um desses casos se aplicar, uma inferência será feita *de* cada `Ui` *ao* `Vi` correspondente da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="dd693-677">If any of these cases apply then an inference is made *from* each `Ui` *to* the corresponding `Vi` as follows:</span></span>
   *  <span data-ttu-id="dd693-678">Se `Ui` não for conhecido como um tipo de referência, uma *inferência exata* será feita</span><span class="sxs-lookup"><span data-stu-id="dd693-678">If  `Ui` is not known to be a reference type then an *exact inference* is made</span></span>
   *  <span data-ttu-id="dd693-679">Caso contrário, se `V` for um tipo de matriz, uma *inferência de limite superior* será feita</span><span class="sxs-lookup"><span data-stu-id="dd693-679">Otherwise, if `V` is an array type then an *upper-bound inference* is made</span></span>
   *  <span data-ttu-id="dd693-680">Caso contrário, se `U` for `C<U1...Uk>`, a inferência dependerá do parâmetro de tipo i-th de `C`:</span><span class="sxs-lookup"><span data-stu-id="dd693-680">Otherwise, if `U` is `C<U1...Uk>` then inference depends on the i-th type parameter of `C`:</span></span>
      *  <span data-ttu-id="dd693-681">Se for covariant, uma *inferência de limite superior* será feita.</span><span class="sxs-lookup"><span data-stu-id="dd693-681">If it is covariant then an *upper-bound inference* is made.</span></span>
      *  <span data-ttu-id="dd693-682">Se for contravariant, uma *inferência de limite inferior* será feita.</span><span class="sxs-lookup"><span data-stu-id="dd693-682">If it is contravariant then a *lower-bound inference* is made.</span></span>
      *  <span data-ttu-id="dd693-683">Se for invariável, uma *inferência exata* será feita.</span><span class="sxs-lookup"><span data-stu-id="dd693-683">If it is invariant then an *exact inference* is made.</span></span>
*  <span data-ttu-id="dd693-684">Caso contrário, nenhuma inferência será feita.</span><span class="sxs-lookup"><span data-stu-id="dd693-684">Otherwise, no inferences are made.</span></span>   

#### <a name="fixing"></a><span data-ttu-id="dd693-685">Resolver</span><span class="sxs-lookup"><span data-stu-id="dd693-685">Fixing</span></span>

<span data-ttu-id="dd693-686">Uma variável de tipo não *fixa* `Xi` com um conjunto de limites é *fixa* da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="dd693-686">An *unfixed* type variable `Xi` with a set of bounds is *fixed* as follows:</span></span>

*  <span data-ttu-id="dd693-687">O conjunto de *tipos candidatos* `Uj` começa como o conjunto de todos os tipos no conjunto de limites para `Xi`.</span><span class="sxs-lookup"><span data-stu-id="dd693-687">The set of *candidate types* `Uj` starts out as the set of all types in the set of bounds for `Xi`.</span></span>
*  <span data-ttu-id="dd693-688">Em seguida, examinamos cada associado por `Xi` por sua vez: para cada `U` limite exato de `Xi` todos os tipos `Uj` que não são idênticos aos `U` são removidos do conjunto de candidatos.</span><span class="sxs-lookup"><span data-stu-id="dd693-688">We then examine each bound for `Xi` in turn: For each exact bound `U` of `Xi` all types `Uj` which are not identical to `U` are removed from the candidate set.</span></span> <span data-ttu-id="dd693-689">Para cada `U` limite inferior de `Xi` todos os tipos `Uj` para o qual *não* há uma conversão implícita de `U` são removidos do conjunto de candidatos.</span><span class="sxs-lookup"><span data-stu-id="dd693-689">For each lower bound `U` of `Xi` all types `Uj` to which there is *not* an implicit conversion from `U` are removed from the candidate set.</span></span> <span data-ttu-id="dd693-690">Para cada `U` limite superior de `Xi` todos os tipos `Uj` de onde *não* há uma conversão implícita para `U` são removidos do conjunto de candidatos.</span><span class="sxs-lookup"><span data-stu-id="dd693-690">For each upper bound `U` of `Xi` all types `Uj` from which there is *not* an implicit conversion to `U` are removed from the candidate set.</span></span>
*  <span data-ttu-id="dd693-691">Se entre os tipos candidatos restantes `Uj` houver um tipo exclusivo `V` do qual há uma conversão implícita para todos os outros tipos candidatos, `Xi` será corrigido para `V`.</span><span class="sxs-lookup"><span data-stu-id="dd693-691">If among the remaining candidate types `Uj` there is a unique type `V` from which there is an implicit conversion to all the other candidate types, then `Xi` is fixed to `V`.</span></span>
*  <span data-ttu-id="dd693-692">Caso contrário, a inferência de tipos falhará.</span><span class="sxs-lookup"><span data-stu-id="dd693-692">Otherwise, type inference fails.</span></span>

#### <a name="inferred-return-type"></a><span data-ttu-id="dd693-693">Tipo de retorno inferido</span><span class="sxs-lookup"><span data-stu-id="dd693-693">Inferred return type</span></span>

<span data-ttu-id="dd693-694">O tipo de retorno inferido de uma função anônima `F` é usado durante a inferência de tipos e resolução de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="dd693-694">The inferred return type of an anonymous function `F` is used during type inference and overload resolution.</span></span> <span data-ttu-id="dd693-695">O tipo de retorno inferido só pode ser determinado para uma função anônima em que todos os tipos de parâmetro são conhecidos, seja porque eles são explicitamente fornecidos, fornecidos por meio de uma conversão de função anônima ou inferido durante a inferência de tipos em um genérico delimitador invocação de método.</span><span class="sxs-lookup"><span data-stu-id="dd693-695">The inferred return type can only be determined for an anonymous function where all parameter types are known, either because they are explicitly given, provided through an anonymous function conversion or inferred during type inference on an enclosing generic method invocation.</span></span>

<span data-ttu-id="dd693-696">O ***tipo de resultado deduzido*** é determinado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="dd693-696">The ***inferred result type*** is determined as follows:</span></span>

*  <span data-ttu-id="dd693-697">Se o corpo de `F` for uma *expressão* que tem um tipo, o tipo de resultado inferido de `F` será o tipo dessa expressão.</span><span class="sxs-lookup"><span data-stu-id="dd693-697">If the body of `F` is an *expression* that has a type, then the inferred result type of `F` is the type of that expression.</span></span>
*  <span data-ttu-id="dd693-698">Se o corpo de `F` for um *bloco* e o conjunto de expressões nas instruções `return` do bloco tiver um tipo mais comum `T` ([encontrando o melhor tipo comum de um conjunto de expressões](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), o tipo de resultado inferido de `F` será `T`.</span><span class="sxs-lookup"><span data-stu-id="dd693-698">If the body of `F` is a *block* and the set of expressions in the block's `return` statements has a best common type `T` ([Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), then the inferred result type of `F` is `T`.</span></span>
*  <span data-ttu-id="dd693-699">Caso contrário, um tipo de resultado não pode ser inferido para `F`.</span><span class="sxs-lookup"><span data-stu-id="dd693-699">Otherwise, a result type cannot be inferred for `F`.</span></span>

<span data-ttu-id="dd693-700">O ***tipo de retorno inferido*** é determinado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="dd693-700">The ***inferred return type*** is determined as follows:</span></span>

*  <span data-ttu-id="dd693-701">Se `F` for Async e o corpo de `F` for uma expressão classificada como Nothing ([classificações de expressão](expressions.md#expression-classifications)) ou um bloco de instruções em que nenhuma instrução de retorno tem expressões, o tipo de retorno inferido será `System.Threading.Tasks.Task`</span><span class="sxs-lookup"><span data-stu-id="dd693-701">If `F` is async and the body of `F` is either an expression classified as nothing ([Expression classifications](expressions.md#expression-classifications)), or a statement block where no return statements have expressions, the inferred return type is `System.Threading.Tasks.Task`</span></span>
*  <span data-ttu-id="dd693-702">Se `F` for Async e tiver um tipo de resultado inferido `T`, o tipo de retorno inferido será `System.Threading.Tasks.Task<T>`.</span><span class="sxs-lookup"><span data-stu-id="dd693-702">If `F` is async and has an inferred result type `T`, the inferred return type is `System.Threading.Tasks.Task<T>`.</span></span>
*  <span data-ttu-id="dd693-703">Se `F` for não Async e tiver um tipo de resultado inferido `T`, o tipo de retorno inferido será `T`.</span><span class="sxs-lookup"><span data-stu-id="dd693-703">If `F` is non-async and has an inferred result type `T`, the inferred return type is `T`.</span></span>
*  <span data-ttu-id="dd693-704">Caso contrário, não será possível inferir um tipo de retorno para `F`.</span><span class="sxs-lookup"><span data-stu-id="dd693-704">Otherwise a return type cannot be inferred for `F`.</span></span>

<span data-ttu-id="dd693-705">Como exemplo de inferência de tipos envolvendo funções anônimas, considere o método de extensão `Select` declarado na classe `System.Linq.Enumerable`:</span><span class="sxs-lookup"><span data-stu-id="dd693-705">As an example of type inference involving anonymous functions, consider the `Select` extension method declared in the `System.Linq.Enumerable` class:</span></span>
```csharp
namespace System.Linq
{
    public static class Enumerable
    {
        public static IEnumerable<TResult> Select<TSource,TResult>(
            this IEnumerable<TSource> source,
            Func<TSource,TResult> selector)
        {
            foreach (TSource element in source) yield return selector(element);
        }
    }
}
```

<span data-ttu-id="dd693-706">Supondo que o namespace de `System.Linq` foi importado com uma cláusula `using` e dado uma classe `Customer` com uma propriedade `Name` do tipo `string`, o método `Select` pode ser usado para selecionar os nomes de uma lista de clientes:</span><span class="sxs-lookup"><span data-stu-id="dd693-706">Assuming the `System.Linq` namespace was imported with a `using` clause, and given a class `Customer` with a `Name` property of type `string`, the `Select` method can be used to select the names of a list of customers:</span></span>
```csharp
List<Customer> customers = GetCustomerList();
IEnumerable<string> names = customers.Select(c => c.Name);
```

<span data-ttu-id="dd693-707">A invocação do método de extensão ([invocações do método de extensão](expressions.md#extension-method-invocations)) de `Select` é processada regravando a invocação para uma invocação de método estático:</span><span class="sxs-lookup"><span data-stu-id="dd693-707">The extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)) of `Select` is processed by rewriting the invocation to a static method invocation:</span></span>
```csharp
IEnumerable<string> names = Enumerable.Select(customers, c => c.Name);
```

<span data-ttu-id="dd693-708">Como os argumentos de tipo não foram especificados explicitamente, a inferência de tipos é usada para inferir os argumentos de tipo.</span><span class="sxs-lookup"><span data-stu-id="dd693-708">Since type arguments were not explicitly specified, type inference is used to infer the type arguments.</span></span> <span data-ttu-id="dd693-709">Primeiro, o argumento `customers` está relacionado ao parâmetro `source`, inferindo `T` a ser `Customer`.</span><span class="sxs-lookup"><span data-stu-id="dd693-709">First, the `customers` argument is related to the `source` parameter, inferring `T` to be `Customer`.</span></span> <span data-ttu-id="dd693-710">Em seguida, usando o processo de inferência de tipo de função anônima descrito acima, `c` recebe o tipo `Customer`e a expressão `c.Name` está relacionada ao tipo de retorno do parâmetro `selector`, inferindo `S` a ser `string`.</span><span class="sxs-lookup"><span data-stu-id="dd693-710">Then, using the anonymous function type inference process described above, `c` is given type `Customer`, and the expression `c.Name` is related to the return type of the `selector` parameter, inferring `S` to be `string`.</span></span> <span data-ttu-id="dd693-711">Portanto, a invocação é equivalente a</span><span class="sxs-lookup"><span data-stu-id="dd693-711">Thus, the invocation is equivalent to</span></span>
```csharp
Sequence.Select<Customer,string>(customers, (Customer c) => c.Name)
```
<span data-ttu-id="dd693-712">e o resultado é do tipo `IEnumerable<string>`.</span><span class="sxs-lookup"><span data-stu-id="dd693-712">and the result is of type `IEnumerable<string>`.</span></span>

<span data-ttu-id="dd693-713">O exemplo a seguir demonstra como a inferência de tipo de função anônima permite que informações de tipo "Flow" Entrem argumentos em uma invocação de método genérico.</span><span class="sxs-lookup"><span data-stu-id="dd693-713">The following example demonstrates how anonymous function type inference allows type information to "flow" between arguments in a generic method invocation.</span></span> <span data-ttu-id="dd693-714">Dado o método:</span><span class="sxs-lookup"><span data-stu-id="dd693-714">Given the method:</span></span>
```csharp
static Z F<X,Y,Z>(X value, Func<X,Y> f1, Func<Y,Z> f2) {
    return f2(f1(value));
}
```

<span data-ttu-id="dd693-715">Inferência de tipos para a invocação:</span><span class="sxs-lookup"><span data-stu-id="dd693-715">Type inference for the invocation:</span></span>
```csharp
double seconds = F("1:15:30", s => TimeSpan.Parse(s), t => t.TotalSeconds);
```
<span data-ttu-id="dd693-716">continua da seguinte maneira: primeiro, o argumento `"1:15:30"` está relacionado ao parâmetro `value`, informando `X` a ser `string`.</span><span class="sxs-lookup"><span data-stu-id="dd693-716">proceeds as follows: First, the argument `"1:15:30"` is related to the `value` parameter, inferring `X` to be `string`.</span></span> <span data-ttu-id="dd693-717">Em seguida, o parâmetro da primeira função anônima, `s`, recebe o tipo inferido `string`e a expressão `TimeSpan.Parse(s)` está relacionada ao tipo de retorno de `f1`, inferindo `Y` a ser `System.TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="dd693-717">Then, the parameter of the first anonymous function, `s`, is given the inferred type `string`, and the expression `TimeSpan.Parse(s)` is related to the return type of `f1`, inferring `Y` to be `System.TimeSpan`.</span></span> <span data-ttu-id="dd693-718">Por fim, o parâmetro da segunda função anônima, `t`, recebe o tipo inferido `System.TimeSpan`e a expressão `t.TotalSeconds` está relacionada ao tipo de retorno de `f2`, inferindo `Z` a ser `double`.</span><span class="sxs-lookup"><span data-stu-id="dd693-718">Finally, the parameter of the second anonymous function, `t`, is given the inferred type `System.TimeSpan`, and the expression `t.TotalSeconds` is related to the return type of `f2`, inferring `Z` to be `double`.</span></span> <span data-ttu-id="dd693-719">Assim, o resultado da invocação é do tipo `double`.</span><span class="sxs-lookup"><span data-stu-id="dd693-719">Thus, the result of the invocation is of type `double`.</span></span>

#### <a name="type-inference-for-conversion-of-method-groups"></a><span data-ttu-id="dd693-720">Inferência de tipos para conversão de grupos de métodos</span><span class="sxs-lookup"><span data-stu-id="dd693-720">Type inference for conversion of method groups</span></span>

<span data-ttu-id="dd693-721">Semelhante às chamadas de métodos genéricos, a inferência de tipos também deve ser aplicada quando um grupo de método `M` contendo um método genérico é convertido em um determinado tipo de delegado `D` ([conversões de grupo de métodos](conversions.md#method-group-conversions)).</span><span class="sxs-lookup"><span data-stu-id="dd693-721">Similar to calls of generic methods, type inference must also be applied when a method group `M` containing a generic method is converted to a given delegate type `D` ([Method group conversions](conversions.md#method-group-conversions)).</span></span> <span data-ttu-id="dd693-722">Dado um método</span><span class="sxs-lookup"><span data-stu-id="dd693-722">Given a method</span></span>
```csharp
Tr M<X1...Xn>(T1 x1 ... Tm xm)
```
<span data-ttu-id="dd693-723">e o grupo de métodos `M` sendo atribuído ao tipo delegado `D` a tarefa da inferência de tipos é encontrar argumentos de tipo `S1...Sn` para que a expressão:</span><span class="sxs-lookup"><span data-stu-id="dd693-723">and the method group `M` being assigned to the delegate type `D` the task of type inference is to find type arguments `S1...Sn` so that the expression:</span></span>
```csharp
M<S1...Sn>
```
<span data-ttu-id="dd693-724">torna-se compatível ([delegar declarações](delegates.md#delegate-declarations)) com `D`.</span><span class="sxs-lookup"><span data-stu-id="dd693-724">becomes compatible ([Delegate declarations](delegates.md#delegate-declarations)) with `D`.</span></span>

<span data-ttu-id="dd693-725">Ao contrário do algoritmo de inferência de tipos para chamadas de método genérico, nesse caso, há apenas *tipos*de argumento, sem *expressões*de argumento.</span><span class="sxs-lookup"><span data-stu-id="dd693-725">Unlike the type inference algorithm for generic method calls, in this case there are only argument *types*, no argument *expressions*.</span></span> <span data-ttu-id="dd693-726">Em particular, não existem funções anônimas e, portanto, não há necessidade de várias fases de inferência.</span><span class="sxs-lookup"><span data-stu-id="dd693-726">In particular, there are no anonymous functions and hence no need for multiple phases of inference.</span></span>

<span data-ttu-id="dd693-727">Em vez disso, todos os `Xi` são considerados não *corrigidos*e uma *inferência de limite inferior* é feita *de* cada tipo de argumento `Uj` de `D` *para* o tipo de parâmetro correspondente `Tj` de `M`.</span><span class="sxs-lookup"><span data-stu-id="dd693-727">Instead, all `Xi` are considered *unfixed*, and a *lower-bound inference* is made *from* each argument type `Uj` of `D` *to* the corresponding parameter type `Tj` of `M`.</span></span> <span data-ttu-id="dd693-728">Se para qualquer um dos `Xi` nenhum limite for encontrado, a inferência de tipos falhará.</span><span class="sxs-lookup"><span data-stu-id="dd693-728">If for any of the `Xi` no bounds were found, type inference fails.</span></span> <span data-ttu-id="dd693-729">Caso contrário, todos os `Xi` são *corrigidos* para `Si`correspondentes, que são o resultado da inferência de tipos.</span><span class="sxs-lookup"><span data-stu-id="dd693-729">Otherwise, all `Xi` are *fixed* to corresponding `Si`, which are the result of type inference.</span></span>

#### <a name="finding-the-best-common-type-of-a-set-of-expressions"></a><span data-ttu-id="dd693-730">Encontrando o melhor tipo comum de um conjunto de expressões</span><span class="sxs-lookup"><span data-stu-id="dd693-730">Finding the best common type of a set of expressions</span></span>

<span data-ttu-id="dd693-731">Em alguns casos, um tipo comum precisa ser inferido para um conjunto de expressões.</span><span class="sxs-lookup"><span data-stu-id="dd693-731">In some cases, a common type needs to be inferred for a set of expressions.</span></span> <span data-ttu-id="dd693-732">Em particular, os tipos de elemento de matrizes tipadas implicitamente e os tipos de retorno de funções anônimas com corpos de *bloco* são encontrados dessa maneira.</span><span class="sxs-lookup"><span data-stu-id="dd693-732">In particular, the element types of implicitly typed arrays and the return types of anonymous functions with *block* bodies are found in this way.</span></span>

<span data-ttu-id="dd693-733">Intuitivamente, dado um conjunto de expressões `E1...Em` essa inferência deve ser equivalente à chamada de um método</span><span class="sxs-lookup"><span data-stu-id="dd693-733">Intuitively, given a set of expressions `E1...Em` this inference should be equivalent to calling a method</span></span>
```csharp
Tr M<X>(X x1 ... X xm)
```
<span data-ttu-id="dd693-734">com o `Ei` como argumentos.</span><span class="sxs-lookup"><span data-stu-id="dd693-734">with the `Ei` as arguments.</span></span>

<span data-ttu-id="dd693-735">Mais precisamente, a inferência começa com uma variável de tipo não *fixa* `X`.</span><span class="sxs-lookup"><span data-stu-id="dd693-735">More precisely, the inference starts out with an *unfixed* type variable `X`.</span></span> <span data-ttu-id="dd693-736">As *inferências de tipo de saída* são feitas *de* cada `Ei` *para* `X`.</span><span class="sxs-lookup"><span data-stu-id="dd693-736">*Output type inferences* are then made *from* each `Ei` *to* `X`.</span></span> <span data-ttu-id="dd693-737">Por fim, `X` é *fixo* e, se for bem-sucedido, o tipo resultante `S` é o melhor tipo comum resultante para as expressões.</span><span class="sxs-lookup"><span data-stu-id="dd693-737">Finally, `X` is *fixed* and, if successful, the resulting type `S` is the resulting best common type for the expressions.</span></span> <span data-ttu-id="dd693-738">Se não existir tal `S`, as expressões não terão o melhor tipo comum.</span><span class="sxs-lookup"><span data-stu-id="dd693-738">If no such `S` exists, the expressions have no best common type.</span></span>

### <a name="overload-resolution"></a><span data-ttu-id="dd693-739">Resolução de sobrecarga</span><span class="sxs-lookup"><span data-stu-id="dd693-739">Overload resolution</span></span>

<span data-ttu-id="dd693-740">A resolução de sobrecarga é um mecanismo de tempo de ligação para selecionar o melhor membro de função para invocar uma lista de argumentos e um conjunto de membros de funções candidatas.</span><span class="sxs-lookup"><span data-stu-id="dd693-740">Overload resolution is a binding-time mechanism for selecting the best function member to invoke given an argument list and a set of candidate function members.</span></span> <span data-ttu-id="dd693-741">Resolução de sobrecarga seleciona o membro da função a ser invocado nos seguintes contextos distintos em C#:</span><span class="sxs-lookup"><span data-stu-id="dd693-741">Overload resolution selects the function member to invoke in the following distinct contexts within C#:</span></span>

*  <span data-ttu-id="dd693-742">Invocação de um método chamado em um *invocation_expression* ([invocações de método](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="dd693-742">Invocation of a method named in an *invocation_expression* ([Method invocations](expressions.md#method-invocations)).</span></span>
*  <span data-ttu-id="dd693-743">Invocação de um construtor de instância chamado em um *object_creation_expression* ([expressões de criação de objeto](expressions.md#object-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="dd693-743">Invocation of an instance constructor named in an *object_creation_expression* ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span>
*  <span data-ttu-id="dd693-744">Invocação de um acessador de indexador por meio de um *element_access* ([acesso de elemento](expressions.md#element-access)).</span><span class="sxs-lookup"><span data-stu-id="dd693-744">Invocation of an indexer accessor through an *element_access* ([Element access](expressions.md#element-access)).</span></span>
*  <span data-ttu-id="dd693-745">Invocação de um operador predefinido ou definido pelo usuário referenciado em uma expressão (resolução de[sobrecarga de operador unário](expressions.md#unary-operator-overload-resolution) e [resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="dd693-745">Invocation of a predefined or user-defined operator referenced in an expression ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution) and [Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)).</span></span>

<span data-ttu-id="dd693-746">Cada um desses contextos define o conjunto de membros da função candidata e a lista de argumentos de forma exclusiva, conforme descrito em detalhes nas seções listadas acima.</span><span class="sxs-lookup"><span data-stu-id="dd693-746">Each of these contexts defines the set of candidate function members and the list of arguments in its own unique way, as described in detail in the sections listed above.</span></span> <span data-ttu-id="dd693-747">Por exemplo, o conjunto de candidatos para uma invocação de método não inclui métodos marcados `override` ([pesquisa de membro](expressions.md#member-lookup)) e os métodos em uma classe base não serão candidatos se qualquer método em uma classe derivada for aplicável ([invocações de método](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="dd693-747">For example, the set of candidates for a method invocation does not include methods marked `override` ([Member lookup](expressions.md#member-lookup)), and methods in a base class are not candidates if any method in a derived class is applicable ([Method invocations](expressions.md#method-invocations)).</span></span>

<span data-ttu-id="dd693-748">Depois que os membros da função candidata e a lista de argumentos tiverem sido identificados, a seleção do melhor membro de função será a mesma em todos os casos:</span><span class="sxs-lookup"><span data-stu-id="dd693-748">Once the candidate function members and the argument list have been identified, the selection of the best function member is the same in all cases:</span></span>

*  <span data-ttu-id="dd693-749">Dado o conjunto de membros da função candidata aplicável, o melhor membro da função nesse conjunto está localizado.</span><span class="sxs-lookup"><span data-stu-id="dd693-749">Given the set of applicable candidate function members, the best function member in that set is located.</span></span> <span data-ttu-id="dd693-750">Se o conjunto contiver apenas um membro de função, esse membro de função será o melhor membro de função.</span><span class="sxs-lookup"><span data-stu-id="dd693-750">If the set contains only one function member, then that function member is the best function member.</span></span> <span data-ttu-id="dd693-751">Caso contrário, o melhor membro da função é o membro de uma função que é melhor do que todos os outros membros da função em relação à lista de argumentos fornecida, desde que cada membro da função seja comparado com todos os outros membros da função usando as regras no [melhor membro da função](expressions.md#better-function-member).</span><span class="sxs-lookup"><span data-stu-id="dd693-751">Otherwise, the best function member is the one function member that is better than all other function members with respect to the given argument list, provided that each function member is compared to all other function members using the rules in [Better function member](expressions.md#better-function-member).</span></span> <span data-ttu-id="dd693-752">Se não houver exatamente um membro da função que seja melhor do que todos os outros membros da função, a invocação do membro da função será ambígua e ocorrerá um erro de tempo de vinculação.</span><span class="sxs-lookup"><span data-stu-id="dd693-752">If there is not exactly one function member that is better than all other function members, then the function member invocation is ambiguous and a binding-time error occurs.</span></span>

<span data-ttu-id="dd693-753">As seções a seguir definem os significados exatos do ***membro da função aplicável*** de termos e ***um melhor membro de função***.</span><span class="sxs-lookup"><span data-stu-id="dd693-753">The following sections define the exact meanings of the terms ***applicable function member*** and ***better function member***.</span></span>

#### <a name="applicable-function-member"></a><span data-ttu-id="dd693-754">Membro da função aplicável</span><span class="sxs-lookup"><span data-stu-id="dd693-754">Applicable function member</span></span>

<span data-ttu-id="dd693-755">Um membro de função é considerado um ***membro de função aplicável*** em relação a uma lista de argumentos `A` quando todas as seguintes são verdadeiras:</span><span class="sxs-lookup"><span data-stu-id="dd693-755">A function member is said to be an ***applicable function member*** with respect to an argument list `A` when all of the following are true:</span></span>

*  <span data-ttu-id="dd693-756">Cada argumento em `A` corresponde a um parâmetro na declaração de membro de função, conforme descrito em [parâmetros correspondentes](expressions.md#corresponding-parameters), e qualquer parâmetro ao qual nenhum argumento corresponde é um parâmetro opcional.</span><span class="sxs-lookup"><span data-stu-id="dd693-756">Each argument in `A` corresponds to a parameter in the function member declaration as described in [Corresponding parameters](expressions.md#corresponding-parameters), and any parameter to which no argument corresponds is an optional parameter.</span></span>
*  <span data-ttu-id="dd693-757">Para cada argumento em `A`, o modo de passagem de parâmetro do argumento (ou seja, value, `ref`ou `out`) é idêntico ao modo de passagem de parâmetro do parâmetro correspondente e</span><span class="sxs-lookup"><span data-stu-id="dd693-757">For each argument in `A`, the parameter passing mode of the argument (i.e., value, `ref`, or `out`) is identical to the parameter passing mode of the corresponding parameter, and</span></span>
   *  <span data-ttu-id="dd693-758">para um parâmetro de valor ou uma matriz de parâmetros, existe uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)) do argumento para o tipo do parâmetro correspondente ou</span><span class="sxs-lookup"><span data-stu-id="dd693-758">for a value parameter or a parameter array, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from the argument to the type of the corresponding parameter, or</span></span>
   *  <span data-ttu-id="dd693-759">para um parâmetro `ref` ou `out`, o tipo do argumento é idêntico ao tipo do parâmetro correspondente.</span><span class="sxs-lookup"><span data-stu-id="dd693-759">for a `ref` or `out` parameter, the type of the argument is identical to the type of the corresponding parameter.</span></span> <span data-ttu-id="dd693-760">Afinal, um parâmetro `ref` ou `out` é um alias para o argumento passado.</span><span class="sxs-lookup"><span data-stu-id="dd693-760">After all, a `ref` or `out` parameter is an alias for the argument passed.</span></span>

<span data-ttu-id="dd693-761">Para um membro de função que inclui uma matriz de parâmetros, se o membro da função for aplicável pelas regras acima, ele será considerado como aplicável em seu ***formato normal***.</span><span class="sxs-lookup"><span data-stu-id="dd693-761">For a function member that includes a parameter array, if the function member is applicable by the above rules, it is said to be applicable in its ***normal form***.</span></span> <span data-ttu-id="dd693-762">Se um membro de função que inclui uma matriz de parâmetros não for aplicável em seu formato normal, o membro da função poderá ser aplicável em seu ***formato expandido***:</span><span class="sxs-lookup"><span data-stu-id="dd693-762">If a function member that includes a parameter array is not applicable in its normal form, the function member may instead be applicable in its ***expanded form***:</span></span>

*  <span data-ttu-id="dd693-763">O formulário expandido é construído pela substituição da matriz de parâmetros na declaração de membro de função por zero ou mais parâmetros de valor do tipo de elemento da matriz de parâmetros, de modo que o número de argumentos na lista de argumentos `A` corresponda ao número total de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="dd693-763">The expanded form is constructed by replacing the parameter array in the function member declaration with zero or more value parameters of the element type of the parameter array such that the number of arguments in the argument list `A` matches the total number of parameters.</span></span> <span data-ttu-id="dd693-764">Se `A` tiver menos argumentos do que o número de parâmetros fixos na declaração de membro da função, a forma expandida do membro da função não poderá ser construída e, portanto, não será aplicável.</span><span class="sxs-lookup"><span data-stu-id="dd693-764">If `A` has fewer arguments than the number of fixed parameters in the function member declaration, the expanded form of the function member cannot be constructed and is thus not applicable.</span></span>
*  <span data-ttu-id="dd693-765">Caso contrário, o formulário expandido será aplicável se para cada argumento em `A` o modo de passagem de parâmetro do argumento for idêntico ao modo de passagem de parâmetro do parâmetro correspondente e</span><span class="sxs-lookup"><span data-stu-id="dd693-765">Otherwise, the expanded form is applicable if for each argument in `A` the parameter passing mode of the argument is identical to the parameter passing mode of the corresponding parameter, and</span></span>
   *  <span data-ttu-id="dd693-766">para um parâmetro de valor fixo ou um parâmetro de valor criado pela expansão, uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)) existe do tipo do argumento para o tipo do parâmetro correspondente ou</span><span class="sxs-lookup"><span data-stu-id="dd693-766">for a fixed value parameter or a value parameter created by the expansion, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from the type of the argument to the type of the corresponding parameter, or</span></span>
   *  <span data-ttu-id="dd693-767">para um parâmetro `ref` ou `out`, o tipo do argumento é idêntico ao tipo do parâmetro correspondente.</span><span class="sxs-lookup"><span data-stu-id="dd693-767">for a `ref` or `out` parameter, the type of the argument is identical to the type of the corresponding parameter.</span></span>

#### <a name="better-function-member"></a><span data-ttu-id="dd693-768">Melhor membro da função</span><span class="sxs-lookup"><span data-stu-id="dd693-768">Better function member</span></span>

<span data-ttu-id="dd693-769">Para fins de determinação do melhor membro de função, uma lista de argumentos desativados A é construída contendo apenas as expressões de argumento em si na ordem em que aparecem na lista de argumentos originais.</span><span class="sxs-lookup"><span data-stu-id="dd693-769">For the purposes of determining the better function member, a stripped-down argument list A is constructed containing just the argument expressions themselves in the order they appear in the original argument list.</span></span>

<span data-ttu-id="dd693-770">As listas de parâmetros para cada um dos membros da função candidata são construídos da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="dd693-770">Parameter lists for each of the candidate function members are constructed in the following way:</span></span>

*  <span data-ttu-id="dd693-771">O formulário expandido será usado se o membro da função for aplicável somente no formulário expandido.</span><span class="sxs-lookup"><span data-stu-id="dd693-771">The expanded form is used if the function member was applicable only in the expanded form.</span></span>
*  <span data-ttu-id="dd693-772">Parâmetros opcionais sem argumentos correspondentes são removidos da lista de parâmetros</span><span class="sxs-lookup"><span data-stu-id="dd693-772">Optional parameters with no corresponding arguments are removed from the parameter list</span></span>
*  <span data-ttu-id="dd693-773">Os parâmetros são reordenados para que eles ocorram na mesma posição que o argumento correspondente na lista de argumentos.</span><span class="sxs-lookup"><span data-stu-id="dd693-773">The parameters are reordered so that they occur at the same position as the corresponding argument in the argument list.</span></span>

<span data-ttu-id="dd693-774">Dado uma lista de argumentos `A` com um conjunto de expressões de argumento `{E1, E2, ..., En}` e dois membros de função aplicáveis `Mp` e `Mq` com tipos de parâmetro `{P1, P2, ..., Pn}` e `{Q1, Q2, ..., Qn}`, `Mp` é definido como um ***membro de função melhor*** do que `Mq` se</span><span class="sxs-lookup"><span data-stu-id="dd693-774">Given an argument list `A` with a set of argument expressions `{E1, E2, ..., En}` and two applicable function members `Mp` and `Mq` with parameter types `{P1, P2, ..., Pn}` and `{Q1, Q2, ..., Qn}`, `Mp` is defined to be a ***better function member*** than `Mq` if</span></span>

*  <span data-ttu-id="dd693-775">para cada argumento, a conversão implícita de `Ex` para `Qx` não é melhor do que a conversão implícita de `Ex` para `Px`e</span><span class="sxs-lookup"><span data-stu-id="dd693-775">for each argument, the implicit conversion from `Ex` to `Qx` is not better than the implicit conversion from `Ex` to `Px`, and</span></span>
*  <span data-ttu-id="dd693-776">para pelo menos um argumento, a conversão de `Ex` para `Px` é melhor do que a conversão de `Ex` para `Qx`.</span><span class="sxs-lookup"><span data-stu-id="dd693-776">for at least one argument, the conversion from `Ex` to `Px` is better than the conversion from `Ex` to `Qx`.</span></span>

<span data-ttu-id="dd693-777">Ao executar essa avaliação, se `Mp` ou `Mq` for aplicável em sua forma expandida, `Px` ou `Qx` se referirá a um parâmetro na forma expandida da lista de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="dd693-777">When performing this evaluation, if `Mp` or `Mq` is applicable in its expanded form, then `Px` or `Qx` refers to a parameter in the expanded form of the parameter list.</span></span>

<span data-ttu-id="dd693-778">Caso as sequências de tipo de parâmetro `{P1, P2, ..., Pn}` e `{Q1, Q2, ..., Qn}` sejam equivalentes (ou seja, cada `Pi` tem uma conversão de identidade para o `Qi`correspondente), as seguintes regras de quebra de empate são aplicadas, em ordem, para determinar o melhor membro da função.</span><span class="sxs-lookup"><span data-stu-id="dd693-778">In case the parameter type sequences `{P1, P2, ..., Pn}` and `{Q1, Q2, ..., Qn}` are equivalent (i.e. each `Pi` has an identity conversion to the corresponding `Qi`), the following tie-breaking rules are applied, in order, to determine the better function member.</span></span>

*  <span data-ttu-id="dd693-779">Se `Mp` for um método não genérico e `Mq` for um método genérico, `Mp` será melhor do que `Mq`.</span><span class="sxs-lookup"><span data-stu-id="dd693-779">If `Mp` is a non-generic method and `Mq` is a generic method, then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="dd693-780">Caso contrário, se `Mp` for aplicável em seu formato normal e `Mq` tiver uma matriz de `params` e for aplicável apenas em sua forma expandida, `Mp` será melhor do que `Mq`.</span><span class="sxs-lookup"><span data-stu-id="dd693-780">Otherwise, if `Mp` is applicable in its normal form and `Mq` has a `params` array and is applicable only in its expanded form, then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="dd693-781">Caso contrário, se `Mp` tiver mais parâmetros declarados que `Mq`, `Mp` será melhor do que `Mq`.</span><span class="sxs-lookup"><span data-stu-id="dd693-781">Otherwise, if `Mp` has more declared parameters than `Mq`, then `Mp` is better than `Mq`.</span></span> <span data-ttu-id="dd693-782">Isso pode ocorrer se ambos os métodos tiverem matrizes `params` e forem aplicáveis somente em suas formas expandidas.</span><span class="sxs-lookup"><span data-stu-id="dd693-782">This can occur if both methods have `params` arrays and are applicable only in their expanded forms.</span></span>
*  <span data-ttu-id="dd693-783">Caso contrário, se todos os parâmetros de `Mp` tiverem um argumento correspondente, enquanto os argumentos padrão precisarão ser substituídos por pelo menos um parâmetro opcional em `Mq`, `Mp` será melhor do que `Mq`.</span><span class="sxs-lookup"><span data-stu-id="dd693-783">Otherwise if all parameters of `Mp` have a corresponding argument whereas default arguments need to be substituted for at least one optional parameter in `Mq` then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="dd693-784">Caso contrário, se `Mp` tiver tipos de parâmetro mais específicos do que `Mq`, `Mp` será melhor do que `Mq`.</span><span class="sxs-lookup"><span data-stu-id="dd693-784">Otherwise, if `Mp` has more specific parameter types than `Mq`, then `Mp` is better than `Mq`.</span></span> <span data-ttu-id="dd693-785">Permita que `{R1, R2, ..., Rn}` e `{S1, S2, ..., Sn}` representem os tipos de parâmetros não instanciados e não expandidos de `Mp` e `Mq`.</span><span class="sxs-lookup"><span data-stu-id="dd693-785">Let `{R1, R2, ..., Rn}` and `{S1, S2, ..., Sn}` represent the uninstantiated and unexpanded parameter types of `Mp` and `Mq`.</span></span> <span data-ttu-id="dd693-786">os tipos de parâmetro de `Mp`são mais específicos do que `Mq`se, para cada parâmetro, `Rx` não for menos específico que `Sx`e, para pelo menos um parâmetro, `Rx` for mais específico que `Sx`:</span><span class="sxs-lookup"><span data-stu-id="dd693-786">`Mp`'s parameter types are more specific than `Mq`'s if, for each parameter, `Rx` is not less specific than `Sx`, and, for at least one parameter, `Rx` is more specific than `Sx`:</span></span>
   *  <span data-ttu-id="dd693-787">Um parâmetro de tipo é menos específico que um parâmetro sem tipo.</span><span class="sxs-lookup"><span data-stu-id="dd693-787">A type parameter is less specific than a non-type parameter.</span></span>
   *  <span data-ttu-id="dd693-788">Recursivamente, um tipo construído é mais específico do que outro tipo construído (com o mesmo número de argumentos de tipo) se pelo menos um argumento de tipo for mais específico e nenhum argumento de tipo for menos específico do que o argumento de tipo correspondente no outro.</span><span class="sxs-lookup"><span data-stu-id="dd693-788">Recursively, a constructed type is more specific than another constructed type (with the same number of type arguments) if at least one type argument is more specific and no type argument is less specific than the corresponding type argument in the other.</span></span>
   *  <span data-ttu-id="dd693-789">Um tipo de matriz é mais específico do que outro tipo de matriz (com o mesmo número de dimensões) se o tipo de elemento do primeiro for mais específico do que o tipo de elemento do segundo.</span><span class="sxs-lookup"><span data-stu-id="dd693-789">An array type is more specific than another array type (with the same number of dimensions) if the element type of the first is more specific than the element type of the second.</span></span>
*  <span data-ttu-id="dd693-790">Caso contrário, se um membro for um operador não-comparado e o outro for um operador de aumento, o que não foi levantado será melhor.</span><span class="sxs-lookup"><span data-stu-id="dd693-790">Otherwise if one member is a non-lifted operator and  the other is a lifted operator, the non-lifted one is better.</span></span>
*  <span data-ttu-id="dd693-791">Caso contrário, nenhum membro da função é melhor.</span><span class="sxs-lookup"><span data-stu-id="dd693-791">Otherwise, neither function member is better.</span></span>

#### <a name="better-conversion-from-expression"></a><span data-ttu-id="dd693-792">Melhor conversão de expressão</span><span class="sxs-lookup"><span data-stu-id="dd693-792">Better conversion from expression</span></span>

<span data-ttu-id="dd693-793">Dado um `C1` de conversão implícita que converte de uma expressão `E` para um tipo `T1`e um `C2` de conversão implícita que converte de uma expressão `E` em um tipo `T2`, `C1` é uma ***conversão melhor*** do que `C2` se `E` não corresponde exatamente a `T2` e pelo menos uma das seguintes isenções:</span><span class="sxs-lookup"><span data-stu-id="dd693-793">Given an implicit conversion `C1` that converts from an expression `E` to a type `T1`, and an implicit conversion `C2` that converts from an expression `E` to a type `T2`, `C1` is a ***better conversion*** than `C2` if `E` does not exactly match `T2` and at least one of the following holds:</span></span>

* <span data-ttu-id="dd693-794">`E` corresponde exatamente `T1` ([expressão exatamente correspondente](expressions.md#exactly-matching-expression))</span><span class="sxs-lookup"><span data-stu-id="dd693-794">`E` exactly matches `T1` ([Exactly matching Expression](expressions.md#exactly-matching-expression))</span></span>
* <span data-ttu-id="dd693-795">`T1` é um destino de conversão melhor do que `T2` ([melhor destino de conversão](expressions.md#better-conversion-target))</span><span class="sxs-lookup"><span data-stu-id="dd693-795">`T1` is a better conversion target than `T2` ([Better conversion target](expressions.md#better-conversion-target))</span></span>

#### <a name="exactly-matching-expression"></a><span data-ttu-id="dd693-796">Expressão exatamente correspondente</span><span class="sxs-lookup"><span data-stu-id="dd693-796">Exactly matching Expression</span></span>

<span data-ttu-id="dd693-797">Dado um `E` de expressão e um tipo `T`, `E` exatamente corresponde `T` se uma das seguintes isenções:</span><span class="sxs-lookup"><span data-stu-id="dd693-797">Given an expression `E` and a type `T`, `E` exactly matches `T` if one of the following holds:</span></span>

*  <span data-ttu-id="dd693-798">`E` tem um tipo `S`, e existe uma conversão de identidade de `S` para `T`</span><span class="sxs-lookup"><span data-stu-id="dd693-798">`E` has a type `S`, and an identity conversion exists from `S` to `T`</span></span>
*  <span data-ttu-id="dd693-799">`E` é uma função anônima, `T` é um tipo delegado `D` ou um tipo de árvore de expressão `Expression<D>` e uma das seguintes isenções:</span><span class="sxs-lookup"><span data-stu-id="dd693-799">`E` is an anonymous function, `T` is either a delegate type `D` or an expression tree type `Expression<D>` and one of the following holds:</span></span>
   *  <span data-ttu-id="dd693-800">Um tipo de retorno inferido `X` existe para `E` no contexto da lista de parâmetros de `D` ([tipo de retorno inferido](expressions.md#inferred-return-type)) e uma conversão de identidade existe de `X` para o tipo de retorno de `D`</span><span class="sxs-lookup"><span data-stu-id="dd693-800">An inferred return type `X` exists for `E` in the context of the parameter list of `D` ([Inferred return type](expressions.md#inferred-return-type)), and an identity conversion exists from `X` to the return type of `D`</span></span>
   *  <span data-ttu-id="dd693-801">O `E` é não Async e `D` tem um tipo de retorno `Y` ou `E` é Async e `D` tem um tipo de retorno `Task<Y>`e uma das seguintes isenções:</span><span class="sxs-lookup"><span data-stu-id="dd693-801">Either `E` is non-async and `D` has a return type `Y` or `E` is async and `D` has a return type `Task<Y>`, and one of the following holds:</span></span>
      * <span data-ttu-id="dd693-802">O corpo de `E` é uma expressão que corresponde exatamente `Y`</span><span class="sxs-lookup"><span data-stu-id="dd693-802">The body of `E` is an expression that exactly matches `Y`</span></span>
      * <span data-ttu-id="dd693-803">O corpo de `E` é um bloco de instruções em que cada instrução de retorno retorna uma expressão que corresponde exatamente a `Y`</span><span class="sxs-lookup"><span data-stu-id="dd693-803">The body of `E` is a statement block where every return statement returns an expression that exactly matches `Y`</span></span>

#### <a name="better-conversion-target"></a><span data-ttu-id="dd693-804">Melhor destino de conversão</span><span class="sxs-lookup"><span data-stu-id="dd693-804">Better conversion target</span></span>

<span data-ttu-id="dd693-805">Considerando dois tipos diferentes `T1` e `T2`, `T1` é um destino de conversão melhor do que `T2` se nenhuma conversão implícita de `T2` para `T1` existir e pelo menos uma das seguintes isenções:</span><span class="sxs-lookup"><span data-stu-id="dd693-805">Given two different types `T1` and `T2`, `T1` is a better conversion target than `T2` if no implicit conversion from `T2` to `T1` exists, and at least one of the following holds:</span></span>

*  <span data-ttu-id="dd693-806">Uma conversão implícita de `T1` para `T2` existe</span><span class="sxs-lookup"><span data-stu-id="dd693-806">An implicit conversion from `T1` to `T2` exists</span></span>
*  <span data-ttu-id="dd693-807">`T1` é um tipo delegado `D1` ou um tipo de árvore de expressão `Expression<D1>`, `T2` é um tipo delegado `D2` ou um tipo de árvore de expressão `Expression<D2>`, `D1` tem um tipo de retorno `S1` e uma das seguintes isenções:</span><span class="sxs-lookup"><span data-stu-id="dd693-807">`T1` is either a delegate type `D1` or an expression tree type `Expression<D1>`, `T2` is either a delegate type `D2` or an expression tree type `Expression<D2>`, `D1` has a return type `S1` and one of the following holds:</span></span>
   * <span data-ttu-id="dd693-808">a `D2` está sendo anulada retornando</span><span class="sxs-lookup"><span data-stu-id="dd693-808">`D2` is void returning</span></span>
   * <span data-ttu-id="dd693-809">`D2` tem um tipo de retorno `S2`e `S1` é um destino de conversão melhor do que o `S2`</span><span class="sxs-lookup"><span data-stu-id="dd693-809">`D2` has a return type `S2`, and `S1` is a better conversion target than `S2`</span></span>
*  <span data-ttu-id="dd693-810">`T1` é `Task<S1>`, `T2` é `Task<S2>`e `S1` é um destino de conversão melhor do que o `S2`</span><span class="sxs-lookup"><span data-stu-id="dd693-810">`T1` is `Task<S1>`, `T2` is `Task<S2>`, and `S1` is a better conversion target than `S2`</span></span>
*  <span data-ttu-id="dd693-811">`T1` é `S1` ou `S1?` onde `S1` é um tipo integral assinado e `T2` é `S2` ou `S2?` em que `S2` é um tipo integral não assinado.</span><span class="sxs-lookup"><span data-stu-id="dd693-811">`T1` is `S1` or `S1?` where `S1` is a signed integral type, and `T2` is `S2` or `S2?` where `S2` is an unsigned integral type.</span></span> <span data-ttu-id="dd693-812">Especificamente:</span><span class="sxs-lookup"><span data-stu-id="dd693-812">Specifically:</span></span>
   * <span data-ttu-id="dd693-813">`S1` é `sbyte` e `S2` é `byte`, `ushort`, `uint`ou `ulong`</span><span class="sxs-lookup"><span data-stu-id="dd693-813">`S1` is `sbyte` and `S2` is `byte`, `ushort`, `uint`, or `ulong`</span></span>
   * <span data-ttu-id="dd693-814">`S1` é `short` e `S2` é `ushort`, `uint`ou `ulong`</span><span class="sxs-lookup"><span data-stu-id="dd693-814">`S1` is `short` and `S2` is `ushort`, `uint`, or `ulong`</span></span>
   * <span data-ttu-id="dd693-815">`S1` é `int` e `S2` é `uint`ou `ulong`</span><span class="sxs-lookup"><span data-stu-id="dd693-815">`S1` is `int` and `S2` is `uint`, or `ulong`</span></span>
   * <span data-ttu-id="dd693-816">`S1` é `long` e `S2` é `ulong`</span><span class="sxs-lookup"><span data-stu-id="dd693-816">`S1` is `long` and `S2` is `ulong`</span></span>

#### <a name="overloading-in-generic-classes"></a><span data-ttu-id="dd693-817">Sobrecarregando em classes genéricas</span><span class="sxs-lookup"><span data-stu-id="dd693-817">Overloading in generic classes</span></span>

<span data-ttu-id="dd693-818">Embora as assinaturas como declaradas devam ser exclusivas, é possível que a substituição dos argumentos de tipo resulte em assinaturas idênticas.</span><span class="sxs-lookup"><span data-stu-id="dd693-818">While signatures as declared must be unique, it is possible that substitution of type arguments results in identical signatures.</span></span> <span data-ttu-id="dd693-819">Nesses casos, as regras de quebra de sobrecarga da resolução de sobrecarga acima escolherão o membro mais específico.</span><span class="sxs-lookup"><span data-stu-id="dd693-819">In such cases, the tie-breaking rules of overload resolution above will pick the most specific member.</span></span>

<span data-ttu-id="dd693-820">Os exemplos a seguir mostram sobrecargas que são válidas e inválidas de acordo com esta regra:</span><span class="sxs-lookup"><span data-stu-id="dd693-820">The following examples show overloads that are valid and invalid according to this rule:</span></span>

```csharp
interface I1<T> {...}

interface I2<T> {...}

class G1<U>
{
    int F1(U u);                  // Overload resolution for G<int>.F1
    int F1(int i);                // will pick non-generic

    void F2(I1<U> a);             // Valid overload
    void F2(I2<U> a);
}

class G2<U,V>
{
    void F3(U u, V v);            // Valid, but overload resolution for
    void F3(V v, U u);            // G2<int,int>.F3 will fail

    void F4(U u, I1<V> v);        // Valid, but overload resolution for    
    void F4(I1<V> v, U u);        // G2<I1<int>,int>.F4 will fail

    void F5(U u1, I1<V> v2);      // Valid overload
    void F5(V v1, U u2);

    void F6(ref U u);             // valid overload
    void F6(out V v);
}
```

### <a name="compile-time-checking-of-dynamic-overload-resolution"></a><span data-ttu-id="dd693-821">Verificação de tempo de compilação da resolução dinâmica de sobrecarga</span><span class="sxs-lookup"><span data-stu-id="dd693-821">Compile-time checking of dynamic overload resolution</span></span>

<span data-ttu-id="dd693-822">Para operações associadas mais dinamicamente, o conjunto de possíveis candidatos para resolução é desconhecido em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-822">For most dynamically bound operations the set of possible candidates for resolution is unknown at compile-time.</span></span> <span data-ttu-id="dd693-823">Em determinados casos, no entanto, o conjunto candidato é conhecido em tempo de compilação:</span><span class="sxs-lookup"><span data-stu-id="dd693-823">In certain cases, however the candidate set is known at compile-time:</span></span>

*  <span data-ttu-id="dd693-824">Chamadas de método estático com argumentos dinâmicos</span><span class="sxs-lookup"><span data-stu-id="dd693-824">Static method calls with dynamic arguments</span></span>
*  <span data-ttu-id="dd693-825">O método de instância chama onde o receptor não é uma expressão dinâmica</span><span class="sxs-lookup"><span data-stu-id="dd693-825">Instance method calls where the receiver is not a dynamic expression</span></span>
*  <span data-ttu-id="dd693-826">Chamadas do indexador onde o receptor não é uma expressão dinâmica</span><span class="sxs-lookup"><span data-stu-id="dd693-826">Indexer calls where the receiver is not a dynamic expression</span></span>
*  <span data-ttu-id="dd693-827">Chamadas de construtor com argumentos dinâmicos</span><span class="sxs-lookup"><span data-stu-id="dd693-827">Constructor calls with dynamic arguments</span></span>

<span data-ttu-id="dd693-828">Nesses casos, uma verificação limitada de tempo de compilação é executada para cada candidato para ver se qualquer uma delas poderia ser aplicada em tempo de execução. Essa verificação consiste nas seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="dd693-828">In these cases a limited compile-time check is performed for each candidate to see if any of them could possibly apply at run-time.This check consists of the following steps:</span></span>

*  <span data-ttu-id="dd693-829">Inferência de tipo parcial: qualquer argumento de tipo que não dependa direta ou indiretamente em um argumento do tipo `dynamic` é inferido usando as regras de [inferência de tipo](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="dd693-829">Partial type inference: Any type argument that does not depend directly or indirectly on an argument of type `dynamic` is inferred using the rules of [Type inference](expressions.md#type-inference).</span></span> <span data-ttu-id="dd693-830">Os argumentos de tipo restantes são desconhecidos.</span><span class="sxs-lookup"><span data-stu-id="dd693-830">The remaining type arguments are unknown.</span></span>
*  <span data-ttu-id="dd693-831">Verificação de aplicabilidade parcial: a aplicabilidade é verificada de acordo com o [membro de função aplicável](expressions.md#applicable-function-member), mas ignorando parâmetros cujos tipos são desconhecidos.</span><span class="sxs-lookup"><span data-stu-id="dd693-831">Partial applicability check: Applicability is checked according to [Applicable function member](expressions.md#applicable-function-member), but ignoring parameters whose types are unknown.</span></span>
*  <span data-ttu-id="dd693-832">Se nenhum candidato passar nesse teste, ocorrerá um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-832">If no candidate passes this test, a compile-time error occurs.</span></span>

### <a name="function-member-invocation"></a><span data-ttu-id="dd693-833">Invocação de membro de função</span><span class="sxs-lookup"><span data-stu-id="dd693-833">Function member invocation</span></span>

<span data-ttu-id="dd693-834">Esta seção descreve o processo que ocorre em tempo de execução para invocar um membro de função específico.</span><span class="sxs-lookup"><span data-stu-id="dd693-834">This section describes the process that takes place at run-time to invoke a particular function member.</span></span> <span data-ttu-id="dd693-835">Supõe-se que um processo de tempo de ligação já determinou o membro específico para invocar, possivelmente aplicando a resolução de sobrecarga a um conjunto de membros de funções candidatas.</span><span class="sxs-lookup"><span data-stu-id="dd693-835">It is assumed that a binding-time process has already determined the particular member to invoke, possibly by applying overload resolution to a set of candidate function members.</span></span>

<span data-ttu-id="dd693-836">Para fins de descrição do processo de invocação, os membros da função são divididos em duas categorias:</span><span class="sxs-lookup"><span data-stu-id="dd693-836">For purposes of describing the invocation process, function members are divided into two categories:</span></span>

*  <span data-ttu-id="dd693-837">Membros da função estática.</span><span class="sxs-lookup"><span data-stu-id="dd693-837">Static function members.</span></span> <span data-ttu-id="dd693-838">Esses são construtores de instância, métodos estáticos, acessadores de propriedade estática e operadores definidos pelo usuário.</span><span class="sxs-lookup"><span data-stu-id="dd693-838">These are instance constructors, static methods, static property accessors, and user-defined operators.</span></span> <span data-ttu-id="dd693-839">Membros de função estática são sempre não virtuais.</span><span class="sxs-lookup"><span data-stu-id="dd693-839">Static function members are always non-virtual.</span></span>
*  <span data-ttu-id="dd693-840">Membros da função de instância.</span><span class="sxs-lookup"><span data-stu-id="dd693-840">Instance function members.</span></span> <span data-ttu-id="dd693-841">Esses são métodos de instância, acessadores de propriedade de instância e acessadores do indexador.</span><span class="sxs-lookup"><span data-stu-id="dd693-841">These are instance methods, instance property accessors, and indexer accessors.</span></span> <span data-ttu-id="dd693-842">Os membros da função de instância são não virtuais ou virtuais e são sempre invocados em uma instância específica.</span><span class="sxs-lookup"><span data-stu-id="dd693-842">Instance function members are either non-virtual or virtual, and are always invoked on a particular instance.</span></span> <span data-ttu-id="dd693-843">A instância é computada por uma expressão de instância e torna-se acessível dentro do membro da função como `this` ([esse acesso](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="dd693-843">The instance is computed by an instance expression, and it becomes accessible within the function member as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="dd693-844">O processamento em tempo de execução de uma invocação de membro de função consiste nas etapas a seguir, em que `M` é o membro da função e, se `M` for um membro de instância, `E` será a expressão de instância:</span><span class="sxs-lookup"><span data-stu-id="dd693-844">The run-time processing of a function member invocation consists of the following steps, where `M` is the function member and, if `M` is an instance member, `E` is the instance expression:</span></span>

*  <span data-ttu-id="dd693-845">Se `M` for um membro de função estática:</span><span class="sxs-lookup"><span data-stu-id="dd693-845">If `M` is a static function member:</span></span>
   * <span data-ttu-id="dd693-846">A lista de argumentos é avaliada conforme descrito em [listas de argumentos](expressions.md#argument-lists).</span><span class="sxs-lookup"><span data-stu-id="dd693-846">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="dd693-847">`M` é invocado.</span><span class="sxs-lookup"><span data-stu-id="dd693-847">`M` is invoked.</span></span>

*  <span data-ttu-id="dd693-848">Se `M` for um membro de função de instância declarado em uma *value_type*:</span><span class="sxs-lookup"><span data-stu-id="dd693-848">If `M` is an instance function member declared in a *value_type*:</span></span>
   * <span data-ttu-id="dd693-849">`E` é avaliado.</span><span class="sxs-lookup"><span data-stu-id="dd693-849">`E` is evaluated.</span></span> <span data-ttu-id="dd693-850">Se essa avaliação causar uma exceção, nenhuma etapa adicional será executada.</span><span class="sxs-lookup"><span data-stu-id="dd693-850">If this evaluation causes an exception, then no further steps are executed.</span></span>
   * <span data-ttu-id="dd693-851">Se `E` não for classificada como uma variável, uma variável local temporária do tipo de `E`será criada e o valor de `E` será atribuído a essa variável.</span><span class="sxs-lookup"><span data-stu-id="dd693-851">If `E` is not classified as a variable, then a temporary local variable of `E`'s type is created and the value of `E` is assigned to that variable.</span></span> <span data-ttu-id="dd693-852">`E`, em seguida, é reclassificado como uma referência a essa variável local temporária.</span><span class="sxs-lookup"><span data-stu-id="dd693-852">`E` is then reclassified as a reference to that temporary local variable.</span></span> <span data-ttu-id="dd693-853">A variável temporária é acessível como `this` em `M`, mas não de nenhuma outra maneira.</span><span class="sxs-lookup"><span data-stu-id="dd693-853">The temporary variable is accessible as `this` within `M`, but not in any other way.</span></span> <span data-ttu-id="dd693-854">Assim, somente quando `E` é uma variável true é possível que o chamador Observe as alterações que `M` faz para `this`.</span><span class="sxs-lookup"><span data-stu-id="dd693-854">Thus, only when `E` is a true variable is it possible for the caller to observe the changes that `M` makes to `this`.</span></span>
   * <span data-ttu-id="dd693-855">A lista de argumentos é avaliada conforme descrito em [listas de argumentos](expressions.md#argument-lists).</span><span class="sxs-lookup"><span data-stu-id="dd693-855">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="dd693-856">`M` é invocado.</span><span class="sxs-lookup"><span data-stu-id="dd693-856">`M` is invoked.</span></span> <span data-ttu-id="dd693-857">A variável referenciada por `E` se torna a variável referenciada por `this`.</span><span class="sxs-lookup"><span data-stu-id="dd693-857">The variable referenced by `E` becomes the variable referenced by `this`.</span></span>

*  <span data-ttu-id="dd693-858">Se `M` for um membro de função de instância declarado em uma *reference_type*:</span><span class="sxs-lookup"><span data-stu-id="dd693-858">If `M` is an instance function member declared in a *reference_type*:</span></span>
   * <span data-ttu-id="dd693-859">`E` é avaliado.</span><span class="sxs-lookup"><span data-stu-id="dd693-859">`E` is evaluated.</span></span> <span data-ttu-id="dd693-860">Se essa avaliação causar uma exceção, nenhuma etapa adicional será executada.</span><span class="sxs-lookup"><span data-stu-id="dd693-860">If this evaluation causes an exception, then no further steps are executed.</span></span>
   * <span data-ttu-id="dd693-861">A lista de argumentos é avaliada conforme descrito em [listas de argumentos](expressions.md#argument-lists).</span><span class="sxs-lookup"><span data-stu-id="dd693-861">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="dd693-862">Se o tipo de `E` for uma *value_type*, uma conversão boxing ([conversões Boxing](types.md#boxing-conversions)) será executada para converter `E` para o tipo `object`e `E` será considerado como sendo do tipo `object` nas etapas a seguir.</span><span class="sxs-lookup"><span data-stu-id="dd693-862">If the type of `E` is a *value_type*, a boxing conversion ([Boxing conversions](types.md#boxing-conversions)) is performed to convert `E` to type `object`, and `E` is considered to be of type `object` in the following steps.</span></span> <span data-ttu-id="dd693-863">Nesse caso, `M` poderia ser apenas um membro de `System.Object`.</span><span class="sxs-lookup"><span data-stu-id="dd693-863">In this case, `M` could only be a member of `System.Object`.</span></span>
   * <span data-ttu-id="dd693-864">O valor de `E` é verificado para ser válido.</span><span class="sxs-lookup"><span data-stu-id="dd693-864">The value of `E` is checked to be valid.</span></span> <span data-ttu-id="dd693-865">Se o valor de `E` for `null`, um `System.NullReferenceException` será lançado e nenhuma etapa adicional será executada.</span><span class="sxs-lookup"><span data-stu-id="dd693-865">If the value of `E` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
   * <span data-ttu-id="dd693-866">A implementação do membro da função a ser invocada é determinada:</span><span class="sxs-lookup"><span data-stu-id="dd693-866">The function member implementation to invoke is determined:</span></span>
     * <span data-ttu-id="dd693-867">Se o tipo de tempo de associação de `E` for uma interface, o membro da função a ser invocado será a implementação de `M` fornecida pelo tipo de tempo de execução da instância referenciada por `E`.</span><span class="sxs-lookup"><span data-stu-id="dd693-867">If the binding-time type of `E` is an interface, the function member to invoke is the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span> <span data-ttu-id="dd693-868">Esse membro de função é determinado pela aplicação das regras de mapeamento de interface ([mapeamento de interface](interfaces.md#interface-mapping)) para determinar a implementação de `M` fornecida pelo tipo de tempo de execução da instância referenciada por `E`.</span><span class="sxs-lookup"><span data-stu-id="dd693-868">This function member is determined by applying the interface mapping rules ([Interface mapping](interfaces.md#interface-mapping)) to determine the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span>
     * <span data-ttu-id="dd693-869">Caso contrário, se `M` for um membro de função virtual, o membro da função a ser invocado será a implementação de `M` fornecida pelo tipo de tempo de execução da instância referenciada por `E`.</span><span class="sxs-lookup"><span data-stu-id="dd693-869">Otherwise, if `M` is a virtual function member, the function member to invoke is the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span> <span data-ttu-id="dd693-870">Esse membro de função é determinado pela aplicação das regras para determinar a implementação mais derivada ([métodos virtuais](classes.md#virtual-methods)) de `M` em relação ao tipo de tempo de execução da instância referenciada por `E`.</span><span class="sxs-lookup"><span data-stu-id="dd693-870">This function member is determined by applying the rules for determining the most derived implementation ([Virtual methods](classes.md#virtual-methods)) of `M` with respect to the run-time type of the instance referenced by `E`.</span></span>
     * <span data-ttu-id="dd693-871">Caso contrário, `M` é um membro de função não virtual e o membro da função a ser invocado é `M` ele mesmo.</span><span class="sxs-lookup"><span data-stu-id="dd693-871">Otherwise, `M` is a non-virtual function member, and the function member to invoke is `M` itself.</span></span>
   * <span data-ttu-id="dd693-872">A implementação do membro da função determinada na etapa acima é invocada.</span><span class="sxs-lookup"><span data-stu-id="dd693-872">The function member implementation determined in the step above is invoked.</span></span> <span data-ttu-id="dd693-873">O objeto referenciado por `E` se torna o objeto referenciado por `this`.</span><span class="sxs-lookup"><span data-stu-id="dd693-873">The object referenced by `E` becomes the object referenced by `this`.</span></span>

#### <a name="invocations-on-boxed-instances"></a><span data-ttu-id="dd693-874">Invocações em instâncias em caixas</span><span class="sxs-lookup"><span data-stu-id="dd693-874">Invocations on boxed instances</span></span>

<span data-ttu-id="dd693-875">Um membro de função implementado em um *value_type* pode ser invocado por meio de uma instância em caixa do que *value_type* nas seguintes situações:</span><span class="sxs-lookup"><span data-stu-id="dd693-875">A function member implemented in a *value_type* can be invoked through a boxed instance of that *value_type* in the following situations:</span></span>

*  <span data-ttu-id="dd693-876">Quando o membro da função é uma `override` de um método herdado do tipo `object` e é invocado por meio de uma expressão de instância do tipo `object`.</span><span class="sxs-lookup"><span data-stu-id="dd693-876">When the function member is an `override` of a method inherited from type `object` and is invoked through an instance expression of type `object`.</span></span>
*  <span data-ttu-id="dd693-877">Quando o membro da função é uma implementação de um membro da função de interface e é invocado por meio de uma expressão de instância de um *interface_type*.</span><span class="sxs-lookup"><span data-stu-id="dd693-877">When the function member is an implementation of an interface function member and is invoked through an instance expression of an *interface_type*.</span></span>
*  <span data-ttu-id="dd693-878">Quando o membro da função é invocado por meio de um delegado.</span><span class="sxs-lookup"><span data-stu-id="dd693-878">When the function member is invoked through a delegate.</span></span>

<span data-ttu-id="dd693-879">Nessas situações, a instância em caixa é considerada para conter uma variável do *value_type*, e essa variável se torna a variável referenciada por `this` na invocação de membro da função.</span><span class="sxs-lookup"><span data-stu-id="dd693-879">In these situations, the boxed instance is considered to contain a variable of the *value_type*, and this variable becomes the variable referenced by `this` within the function member invocation.</span></span> <span data-ttu-id="dd693-880">Em particular, isso significa que, quando um membro de função é invocado em uma instância em caixa, é possível que o membro da função modifique o valor contido na instância em caixa.</span><span class="sxs-lookup"><span data-stu-id="dd693-880">In particular, this means that when a function member is invoked on a boxed instance, it is possible for the function member to modify the value contained in the boxed instance.</span></span>

## <a name="primary-expressions"></a><span data-ttu-id="dd693-881">Expressões primárias</span><span class="sxs-lookup"><span data-stu-id="dd693-881">Primary expressions</span></span>

<span data-ttu-id="dd693-882">As expressões primárias incluem as formas mais simples de expressões.</span><span class="sxs-lookup"><span data-stu-id="dd693-882">Primary expressions include the simplest forms of expressions.</span></span>

```antlr
primary_expression
    : primary_no_array_creation_expression
    | array_creation_expression
    ;

primary_no_array_creation_expression
    : literal
    | interpolated_string_expression
    | simple_name
    | parenthesized_expression
    | member_access
    | invocation_expression
    | element_access
    | this_access
    | base_access
    | post_increment_expression
    | post_decrement_expression
    | object_creation_expression
    | delegate_creation_expression
    | anonymous_object_creation_expression
    | typeof_expression
    | checked_expression
    | unchecked_expression
    | default_value_expression
    | nameof_expression
    | anonymous_method_expression
    | primary_no_array_creation_expression_unsafe
    ;
```

<span data-ttu-id="dd693-883">As expressões primárias são divididas entre *array_creation_expression*s e *primary_no_array_creation_expression*s.</span><span class="sxs-lookup"><span data-stu-id="dd693-883">Primary expressions are divided between *array_creation_expression*s and *primary_no_array_creation_expression*s.</span></span> <span data-ttu-id="dd693-884">Tratar a expressão de criação de matriz dessa maneira, em vez de listá-la junto com outros formulários de expressão simples, permite que a gramática inpermita códigos potencialmente confusos, como</span><span class="sxs-lookup"><span data-stu-id="dd693-884">Treating array-creation-expression in this way, rather than listing it along with the other simple expression forms, enables the grammar to disallow potentially confusing code such as</span></span>
```csharp
object o = new int[3][1];
```
<span data-ttu-id="dd693-885">que, de outra forma, seriam interpretados como</span><span class="sxs-lookup"><span data-stu-id="dd693-885">which would otherwise be interpreted as</span></span>
```csharp
object o = (new int[3])[1];
```

### <a name="literals"></a><span data-ttu-id="dd693-886">{1&gt;Literais&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-886">Literals</span></span>

<span data-ttu-id="dd693-887">Um *primary_expression* que consiste em um *literal* ([literais](lexical-structure.md#literals)) é classificado como um valor.</span><span class="sxs-lookup"><span data-stu-id="dd693-887">A *primary_expression* that consists of a *literal* ([Literals](lexical-structure.md#literals)) is classified as a value.</span></span>


### <a name="interpolated-strings"></a><span data-ttu-id="dd693-888">Cadeias de caracteres interpoladas</span><span class="sxs-lookup"><span data-stu-id="dd693-888">Interpolated strings</span></span>

<span data-ttu-id="dd693-889">Um *interpolated_string_expression* consiste em um sinal de `$` seguido por um literal de cadeia de caracteres regular ou textual, no qual os buracos, delimitados por `{` e `}`, incluem expressões e especificações de formatação.</span><span class="sxs-lookup"><span data-stu-id="dd693-889">An *interpolated_string_expression* consists of a `$` sign followed by a regular or verbatim string literal, wherein holes, delimited by `{` and `}`, enclose expressions and formatting specifications.</span></span> <span data-ttu-id="dd693-890">Uma expressão de cadeia de caracteres interpolada é o resultado de um *interpolated_string_literal* que foi dividido em tokens individuais, conforme descrito em [literais de cadeia de caracteres interpolados](lexical-structure.md#interpolated-string-literals).</span><span class="sxs-lookup"><span data-stu-id="dd693-890">An interpolated string expression is the result of an *interpolated_string_literal* that has been broken up into individual tokens, as described in [Interpolated string literals](lexical-structure.md#interpolated-string-literals).</span></span>

```antlr
interpolated_string_expression
    : '$' interpolated_regular_string
    | '$' interpolated_verbatim_string
    ;

interpolated_regular_string
    : interpolated_regular_string_whole
    | interpolated_regular_string_start interpolated_regular_string_body interpolated_regular_string_end
    ;

interpolated_regular_string_body
    : interpolation (interpolated_regular_string_mid interpolation)*
    ;

interpolation
    : expression
    | expression ',' constant_expression
    ;

interpolated_verbatim_string
    : interpolated_verbatim_string_whole
    | interpolated_verbatim_string_start interpolated_verbatim_string_body interpolated_verbatim_string_end
    ;

interpolated_verbatim_string_body
    : interpolation (interpolated_verbatim_string_mid interpolation)+
    ;
```

<span data-ttu-id="dd693-891">O *constant_expression* em uma interpolação deve ter uma conversão implícita para `int`.</span><span class="sxs-lookup"><span data-stu-id="dd693-891">The *constant_expression* in an interpolation must have an implicit conversion to `int`.</span></span>

<span data-ttu-id="dd693-892">Um *interpolated_string_expression* é classificado como um valor.</span><span class="sxs-lookup"><span data-stu-id="dd693-892">An *interpolated_string_expression* is classified as a value.</span></span> <span data-ttu-id="dd693-893">Se ele for imediatamente convertido em `System.IFormattable` ou `System.FormattableString` com uma conversão de cadeia de caracteres interpolada implícita ([conversões de cadeia de caracteres interpoladas implícitas](conversions.md#implicit-interpolated-string-conversions)), a expressão de cadeia de caracteres interpolada terá esse tipo.</span><span class="sxs-lookup"><span data-stu-id="dd693-893">If it is immediately converted to `System.IFormattable` or `System.FormattableString` with an implicit interpolated string conversion ([Implicit interpolated string conversions](conversions.md#implicit-interpolated-string-conversions)), the interpolated string expression has that type.</span></span> <span data-ttu-id="dd693-894">Caso contrário, ele tem o tipo `string`.</span><span class="sxs-lookup"><span data-stu-id="dd693-894">Otherwise, it has the type `string`.</span></span>

<span data-ttu-id="dd693-895">Se o tipo de uma cadeia de caracteres interpolada for `System.IFormattable` ou `System.FormattableString`, o significado será uma chamada para `System.Runtime.CompilerServices.FormattableStringFactory.Create`.</span><span class="sxs-lookup"><span data-stu-id="dd693-895">If the type of an interpolated string is `System.IFormattable` or `System.FormattableString`, the meaning is a call to `System.Runtime.CompilerServices.FormattableStringFactory.Create`.</span></span> <span data-ttu-id="dd693-896">Se o tipo for `string`, o significado da expressão será uma chamada para `string.Format`.</span><span class="sxs-lookup"><span data-stu-id="dd693-896">If the type is `string`, the meaning of the expression is a call to `string.Format`.</span></span> <span data-ttu-id="dd693-897">Em ambos os casos, a lista de argumentos da chamada consiste em um literal de cadeia de caracteres de formato com espaços reservados para cada interpolação e um argumento para cada expressão correspondente aos detentores de lugar.</span><span class="sxs-lookup"><span data-stu-id="dd693-897">In both cases, the argument list of the call consists of a format string literal with placeholders for each interpolation, and an argument for each expression corresponding to the place holders.</span></span>

<span data-ttu-id="dd693-898">A cadeia de caracteres de formato literal é construída da seguinte maneira, em que `N` é o número de interpolações no *interpolated_string_expression*:</span><span class="sxs-lookup"><span data-stu-id="dd693-898">The format string literal is constructed as follows, where `N` is the number of interpolations in the *interpolated_string_expression*:</span></span>

*  <span data-ttu-id="dd693-899">Se um *interpolated_regular_string_whole* ou um *interpolated_verbatim_string_whole* seguir o sinal de `$`, a cadeia de caracteres de formato literal será esse token.</span><span class="sxs-lookup"><span data-stu-id="dd693-899">If an *interpolated_regular_string_whole* or an *interpolated_verbatim_string_whole* follows the `$` sign, then the format string literal is that token.</span></span>
*  <span data-ttu-id="dd693-900">Caso contrário, o literal de cadeia de caracteres de formato consiste em:</span><span class="sxs-lookup"><span data-stu-id="dd693-900">Otherwise, the format string literal consists of:</span></span> 
   *  <span data-ttu-id="dd693-901">Primeiro *interpolated_regular_string_start* ou *interpolated_verbatim_string_start*</span><span class="sxs-lookup"><span data-stu-id="dd693-901">First the *interpolated_regular_string_start* or *interpolated_verbatim_string_start*</span></span>
   *  <span data-ttu-id="dd693-902">Em seguida, para cada número `I` de `0` para `N-1`:</span><span class="sxs-lookup"><span data-stu-id="dd693-902">Then for each number `I` from `0` to `N-1`:</span></span> 
      * <span data-ttu-id="dd693-903">A representação decimal de `I`</span><span class="sxs-lookup"><span data-stu-id="dd693-903">The decimal representation of `I`</span></span>
      * <span data-ttu-id="dd693-904">Em seguida, se a *interpolação* correspondente tiver um *constant_expression*, uma `,` (vírgula) seguida da representação decimal do valor do *constant_expression*</span><span class="sxs-lookup"><span data-stu-id="dd693-904">Then, if the corresponding *interpolation* has a *constant_expression*, a `,` (comma) followed by the decimal representation of the value of the *constant_expression*</span></span>
      * <span data-ttu-id="dd693-905">Em seguida, o *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_mid* ou *interpolated_verbatim_string_end* imediatamente após a interpolação correspondente.</span><span class="sxs-lookup"><span data-stu-id="dd693-905">Then the *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_mid* or *interpolated_verbatim_string_end* immediately following the corresponding interpolation.</span></span>

<span data-ttu-id="dd693-906">Os argumentos subsequentes são simplesmente as *expressões* das *interpolações* (se houver), em ordem.</span><span class="sxs-lookup"><span data-stu-id="dd693-906">The subsequent arguments are simply the *expressions* from the *interpolations* (if any), in order.</span></span>

<span data-ttu-id="dd693-907">TODO: exemplos.</span><span class="sxs-lookup"><span data-stu-id="dd693-907">TODO: examples.</span></span>


### <a name="simple-names"></a><span data-ttu-id="dd693-908">Nomes simples</span><span class="sxs-lookup"><span data-stu-id="dd693-908">Simple names</span></span>

<span data-ttu-id="dd693-909">Um *Simple_name* consiste em um identificador, opcionalmente seguido por uma lista de argumentos de tipo:</span><span class="sxs-lookup"><span data-stu-id="dd693-909">A *simple_name* consists of an identifier, optionally followed by a type argument list:</span></span>

```antlr
simple_name
    : identifier type_argument_list?
    ;
```

<span data-ttu-id="dd693-910">Uma *Simple_name* está no formato `I` ou no formato `I<A1,...,Ak>`, em que `I` é um único identificador e `<A1,...,Ak>` é um *type_argument_list*opcional.</span><span class="sxs-lookup"><span data-stu-id="dd693-910">A *simple_name* is either of the form `I` or of the form `I<A1,...,Ak>`, where `I` is a single identifier and `<A1,...,Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="dd693-911">Quando nenhum *type_argument_list* for especificado, considere `K` como zero.</span><span class="sxs-lookup"><span data-stu-id="dd693-911">When no *type_argument_list* is specified, consider `K` to be zero.</span></span> <span data-ttu-id="dd693-912">A *Simple_name* é avaliada e classificada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="dd693-912">The *simple_name* is evaluated and classified as follows:</span></span>

*  <span data-ttu-id="dd693-913">Se `K` for zero e o *Simple_name* aparecer em um *bloco* e se o espaço da declaração local ([declarações](basic-concepts.md#declarations)) do *bloco*(ou um *bloco*delimitador) contiver uma variável local, um parâmetro ou uma constante com o nome `I`, o *Simple_name* se refere a essa variável local, parâmetro ou constante e é classificado como uma variável ou valor.</span><span class="sxs-lookup"><span data-stu-id="dd693-913">If `K` is zero and the *simple_name* appears within a *block* and if the *block*'s (or an enclosing *block*'s) local variable declaration space ([Declarations](basic-concepts.md#declarations)) contains a local variable, parameter or constant with name `I`, then the *simple_name* refers to that local variable, parameter or constant and is classified as a variable or value.</span></span>
*  <span data-ttu-id="dd693-914">Se `K` for zero e o *Simple_name* aparecer dentro do corpo de uma declaração de método genérico e se essa declaração incluir um parâmetro de tipo com o nome `I`, o *Simple_name* se referirá a esse parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="dd693-914">If `K` is zero and the *simple_name* appears within the body of a generic method declaration and if that declaration includes a type parameter with name `I`, then the *simple_name* refers to that type parameter.</span></span>
*  <span data-ttu-id="dd693-915">Caso contrário, para cada tipo de instância `T` ([o tipo de instância](classes.md#the-instance-type)), começando com o tipo de instância da declaração de tipo delimitadora imediatamente e continuando com o tipo de instância de cada declaração de classe ou struct de circunscrição (se houver):</span><span class="sxs-lookup"><span data-stu-id="dd693-915">Otherwise, for each instance type `T` ([The instance type](classes.md#the-instance-type)), starting with the instance type of the immediately enclosing type declaration and continuing with the instance type of each enclosing class or struct declaration (if any):</span></span>
   *  <span data-ttu-id="dd693-916">Se `K` for zero e a declaração de `T` incluir um parâmetro de tipo com o nome `I`, o *Simple_name* se referirá a esse parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="dd693-916">If `K` is zero and the declaration of `T` includes a type parameter with name `I`, then the *simple_name* refers to that type parameter.</span></span>
   *  <span data-ttu-id="dd693-917">Caso contrário, se uma pesquisa de membro ([pesquisa de membro](expressions.md#member-lookup)) de `I` em `T` com argumentos de tipo de  `K`produzir uma correspondência:</span><span class="sxs-lookup"><span data-stu-id="dd693-917">Otherwise, if a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `T` with `K` type arguments produces a match:</span></span>
      * <span data-ttu-id="dd693-918">Se `T` for o tipo de instância do tipo de classe ou struct de circunscrição imediatamente e a pesquisa identificar um ou mais métodos, o resultado será um grupo de métodos com uma expressão de instância associada de `this`.</span><span class="sxs-lookup"><span data-stu-id="dd693-918">If `T` is the instance type of the immediately enclosing class or struct type and the lookup identifies one or more methods, the result is a method group with an associated instance expression of `this`.</span></span> <span data-ttu-id="dd693-919">Se uma lista de argumentos de tipo tiver sido especificada, ela será usada na chamada de um método genérico ([invocações de método](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="dd693-919">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
      * <span data-ttu-id="dd693-920">Caso contrário, se `T` for o tipo de instância do tipo de classe ou struct imediatamente delimitadora, se a pesquisa identificar um membro de instância e se a referência ocorrer dentro do corpo de um construtor de instância, um método de instância ou um acessador de instância, o resultado será o mesmo que um acesso de membro ([acesso de membro](expressions.md#member-access)) do formulário `this.I`.</span><span class="sxs-lookup"><span data-stu-id="dd693-920">Otherwise, if `T` is the instance type of the immediately enclosing class or struct type, if the lookup identifies an instance member, and if the reference occurs within the body of an instance constructor, an instance method, or an instance accessor, the result is the same as a member access ([Member access](expressions.md#member-access)) of the form `this.I`.</span></span> <span data-ttu-id="dd693-921">Isso só pode acontecer quando `K` é zero.</span><span class="sxs-lookup"><span data-stu-id="dd693-921">This can only happen when `K` is zero.</span></span>
      * <span data-ttu-id="dd693-922">Caso contrário, o resultado será o mesmo que um acesso de membro ([acesso de membro](expressions.md#member-access)) do formulário `T.I` ou `T.I<A1,...,Ak>`.</span><span class="sxs-lookup"><span data-stu-id="dd693-922">Otherwise, the result is the same as a member access ([Member access](expressions.md#member-access)) of the form `T.I` or `T.I<A1,...,Ak>`.</span></span> <span data-ttu-id="dd693-923">Nesse caso, é um erro de tempo de associação para o *Simple_name* fazer referência a um membro de instância.</span><span class="sxs-lookup"><span data-stu-id="dd693-923">In this case, it is a binding-time error for the *simple_name* to refer to an instance member.</span></span>

*  <span data-ttu-id="dd693-924">Caso contrário, para cada namespace `N`, começando com o namespace no qual o *Simple_name* ocorre, continuando com cada namespace delimitador (se houver) e terminando com o namespace global, as etapas a seguir são avaliadas até que uma entidade seja localizada:</span><span class="sxs-lookup"><span data-stu-id="dd693-924">Otherwise, for each namespace `N`, starting with the namespace in which the *simple_name* occurs, continuing with each enclosing namespace (if any), and ending with the global namespace, the following steps are evaluated until an entity is located:</span></span>
   *  <span data-ttu-id="dd693-925">Se `K` for zero e `I` for o nome de um namespace no `N`, então:</span><span class="sxs-lookup"><span data-stu-id="dd693-925">If `K` is zero and `I` is the name of a namespace in `N`, then:</span></span>
      * <span data-ttu-id="dd693-926">Se o local em que a *Simple_name* ocorre estiver entre uma declaração de namespace para `N` e a declaração de namespace contiver um *extern_alias_directive* ou *using_alias_directive* que associa o nome `I` a um namespace ou tipo, a *Simple_name* será ambígua e ocorrerá um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-926">If the location where the *simple_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *simple_name* is ambiguous and a compile-time error occurs.</span></span>
      * <span data-ttu-id="dd693-927">Caso contrário, a *Simple_name* se refere ao namespace chamado `I` no `N`.</span><span class="sxs-lookup"><span data-stu-id="dd693-927">Otherwise, the *simple_name* refers to the namespace named `I` in `N`.</span></span>
   *  <span data-ttu-id="dd693-928">Caso contrário, se `N` contiver um tipo acessível com nome `I` e `K`parâmetros de tipo  , então:</span><span class="sxs-lookup"><span data-stu-id="dd693-928">Otherwise, if `N` contains an accessible type having name `I` and `K` type parameters, then:</span></span>
      * <span data-ttu-id="dd693-929">Se `K` for zero e o local onde o *Simple_name* ocorrer for incluído por uma declaração de namespace para `N` e a declaração de namespace contiver um *extern_alias_directive* ou *using_alias_directive* que associa o nome `I` a um namespace ou tipo, a *Simple_name* será ambígua e ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-929">If `K` is zero and the location where the *simple_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *simple_name* is ambiguous and a compile-time error occurs.</span></span>
      * <span data-ttu-id="dd693-930">Caso contrário, o *namespace_or_type_name* se refere ao tipo construído com os argumentos de tipo fornecidos.</span><span class="sxs-lookup"><span data-stu-id="dd693-930">Otherwise, the *namespace_or_type_name* refers to the type constructed with the given type arguments.</span></span>
   *  <span data-ttu-id="dd693-931">Caso contrário, se o local em que o *Simple_name* ocorrer for incluído por uma declaração de namespace para `N`:</span><span class="sxs-lookup"><span data-stu-id="dd693-931">Otherwise, if the location where the *simple_name* occurs is enclosed by a namespace declaration for `N`:</span></span>
      * <span data-ttu-id="dd693-932">Se `K` for zero e a declaração de namespace contiver um *extern_alias_directive* ou *using_alias_directive* que associa o nome `I` a um namespace ou tipo importado, a *Simple_name* se referirá a esse namespace ou tipo.</span><span class="sxs-lookup"><span data-stu-id="dd693-932">If `K` is zero and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with an imported namespace or type, then the *simple_name* refers to that namespace or type.</span></span>
      * <span data-ttu-id="dd693-933">Caso contrário, se os namespaces e as declarações de tipo importados pelo *using_namespace_directive*s e *using_static_directive*s da declaração de namespace contiverem exatamente um tipo acessível ou membro estático sem extensão com nome `I` e `K`parâmetros de tipo de  , o *Simple_name* se referirá a esse tipo ou membro construído com os argumentos de tipo fornecidos.</span><span class="sxs-lookup"><span data-stu-id="dd693-933">Otherwise, if the namespaces and type declarations imported by the *using_namespace_directive*s and *using_static_directive*s of the namespace declaration contain exactly one accessible type or non-extension static member having name `I` and `K` type parameters, then the *simple_name* refers to that type or member constructed with the given type arguments.</span></span>
      * <span data-ttu-id="dd693-934">Caso contrário, se os namespaces e os tipos importados pelo *using_namespace_directive*s da declaração de namespace contiverem mais de um tipo acessível ou membro estático de método não-extensão com nome `I` e `K`parâmetros de tipo de  , o *Simple_name* será ambíguo e ocorrerá um erro.</span><span class="sxs-lookup"><span data-stu-id="dd693-934">Otherwise, if the namespaces and types imported by the *using_namespace_directive*s of the namespace declaration contain more than one accessible type or non-extension-method static member having name `I` and `K` type parameters, then the *simple_name* is ambiguous and an error occurs.</span></span>

   <span data-ttu-id="dd693-935">Observe que essa etapa inteira é exatamente paralela à etapa correspondente no processamento de um *namespace_or_type_name* ([namespace e nomes de tipo](basic-concepts.md#namespace-and-type-names)).</span><span class="sxs-lookup"><span data-stu-id="dd693-935">Note that this entire step is exactly parallel to the corresponding step in the processing of a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)).</span></span>

*  <span data-ttu-id="dd693-936">Caso contrário, o *Simple_name* será indefinido e ocorrerá um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-936">Otherwise, the *simple_name* is undefined and a compile-time error occurs.</span></span>


### <a name="parenthesized-expressions"></a><span data-ttu-id="dd693-937">Expressões entre parênteses</span><span class="sxs-lookup"><span data-stu-id="dd693-937">Parenthesized expressions</span></span>

<span data-ttu-id="dd693-938">Uma *parenthesized_expression* consiste em uma *expressão* entre parênteses.</span><span class="sxs-lookup"><span data-stu-id="dd693-938">A *parenthesized_expression* consists of an *expression* enclosed in parentheses.</span></span>

```antlr
parenthesized_expression
    : '(' expression ')'
    ;
```

<span data-ttu-id="dd693-939">Uma *parenthesized_expression* é avaliada avaliando a *expressão* dentro dos parênteses.</span><span class="sxs-lookup"><span data-stu-id="dd693-939">A *parenthesized_expression* is evaluated by evaluating the *expression* within the parentheses.</span></span> <span data-ttu-id="dd693-940">Se a *expressão* dentro dos parênteses denota um namespace ou tipo, ocorre um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-940">If the *expression* within the parentheses denotes a namespace or type, a compile-time error occurs.</span></span> <span data-ttu-id="dd693-941">Caso contrário, o resultado da *parenthesized_expression* é o resultado da avaliação da *expressão*contida.</span><span class="sxs-lookup"><span data-stu-id="dd693-941">Otherwise, the result of the *parenthesized_expression* is the result of the evaluation of the contained *expression*.</span></span>

### <a name="member-access"></a><span data-ttu-id="dd693-942">Acesso de membros</span><span class="sxs-lookup"><span data-stu-id="dd693-942">Member access</span></span>

<span data-ttu-id="dd693-943">Um *member_access* consiste em um *primary_expression*, um *predefined_type*ou um *qualified_alias_member*, seguido por um token "`.`", seguido por um *identificador*, opcionalmente seguido por um *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="dd693-943">A *member_access* consists of a *primary_expression*, a *predefined_type*, or a *qualified_alias_member*, followed by a "`.`" token, followed by an *identifier*, optionally followed by a *type_argument_list*.</span></span>

```antlr
member_access
    : primary_expression '.' identifier type_argument_list?
    | predefined_type '.' identifier type_argument_list?
    | qualified_alias_member '.' identifier
    ;

predefined_type
    : 'bool'   | 'byte'  | 'char'  | 'decimal' | 'double' | 'float' | 'int' | 'long'
    | 'object' | 'sbyte' | 'short' | 'string'  | 'uint'   | 'ulong' | 'ushort'
    ;
```

<span data-ttu-id="dd693-944">A produção *qualified_alias_member* é definida em [qualificadores de alias de namespace](namespaces.md#namespace-alias-qualifiers).</span><span class="sxs-lookup"><span data-stu-id="dd693-944">The *qualified_alias_member* production is defined in [Namespace alias qualifiers](namespaces.md#namespace-alias-qualifiers).</span></span>

<span data-ttu-id="dd693-945">Uma *member_access* está no formato `E.I` ou no formato `E.I<A1, ..., Ak>`, em que `E` é uma expressão primária, `I` é um único identificador e `<A1, ..., Ak>` é um *type_argument_list*opcional.</span><span class="sxs-lookup"><span data-stu-id="dd693-945">A *member_access* is either of the form `E.I` or of the form `E.I<A1, ..., Ak>`, where `E` is a primary-expression, `I` is a single identifier and `<A1, ..., Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="dd693-946">Quando nenhum *type_argument_list* for especificado, considere `K` como zero.</span><span class="sxs-lookup"><span data-stu-id="dd693-946">When no *type_argument_list* is specified, consider `K` to be zero.</span></span>

<span data-ttu-id="dd693-947">Uma *member_access* com uma *primary_expression* do tipo `dynamic` é vinculada dinamicamente ([associação dinâmica](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="dd693-947">A *member_access* with a *primary_expression* of type `dynamic` is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="dd693-948">Nesse caso, o compilador classifica o acesso de membro como um acesso de Propriedade do tipo `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="dd693-948">In this case the compiler classifies the member access as a property access of type `dynamic`.</span></span> <span data-ttu-id="dd693-949">As regras abaixo para determinar o significado da *member_access* são então aplicadas em tempo de execução, usando o tipo de tempo de execução em vez do tipo de tempo de compilação do *primary_expression*.</span><span class="sxs-lookup"><span data-stu-id="dd693-949">The rules below to determine the meaning of the *member_access* are then applied at run-time, using the run-time type instead of the compile-time type of the *primary_expression*.</span></span> <span data-ttu-id="dd693-950">Se essa classificação de tempo de execução leva a um grupo de métodos, o acesso de membro deve ser o *primary_expression* de um *invocation_expression*.</span><span class="sxs-lookup"><span data-stu-id="dd693-950">If this run-time classification leads to a method group, then the member access must be the *primary_expression* of an *invocation_expression*.</span></span>

<span data-ttu-id="dd693-951">A *member_access* é avaliada e classificada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="dd693-951">The *member_access* is evaluated and classified as follows:</span></span>

*  <span data-ttu-id="dd693-952">Se `K` for zero e `E` for um namespace e `E` contiver um namespace aninhado com o nome `I`, o resultado será esse namespace.</span><span class="sxs-lookup"><span data-stu-id="dd693-952">If `K` is zero and `E` is a namespace and `E` contains a nested namespace with name `I`, then the result is that namespace.</span></span>
*  <span data-ttu-id="dd693-953">Caso contrário, se `E` for um namespace e `E` contiver um tipo acessível com nome `I` e `K`parâmetros de tipo de  , o resultado será aquele tipo construído com os argumentos de tipo fornecidos.</span><span class="sxs-lookup"><span data-stu-id="dd693-953">Otherwise, if `E` is a namespace and `E` contains an accessible type having name `I` and `K` type parameters, then the result is that type constructed with the given type arguments.</span></span>
*  <span data-ttu-id="dd693-954">Se `E` for uma *predefined_type* ou uma *primary_expression* classificada como um tipo, se `E` não for um parâmetro de tipo e se uma pesquisa de membro ([pesquisa de membro](expressions.md#member-lookup)) de `I` em `E` com parâmetros de tipo `K` produzir uma correspondência, `E.I` será avaliado e classificado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="dd693-954">If `E` is a *predefined_type* or a *primary_expression* classified as a type, if `E` is not a type parameter, and if a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `E` with `K` type parameters produces a match, then `E.I` is evaluated and classified as follows:</span></span>
   *  <span data-ttu-id="dd693-955">Se `I` identificar um tipo, o resultado será aquele tipo construído com os argumentos de tipo fornecidos.</span><span class="sxs-lookup"><span data-stu-id="dd693-955">If `I` identifies a type, then the result is that type constructed with the given type arguments.</span></span>
   *  <span data-ttu-id="dd693-956">Se `I` identificar um ou mais métodos, o resultado será um grupo de métodos sem nenhuma expressão de instância associada.</span><span class="sxs-lookup"><span data-stu-id="dd693-956">If `I` identifies one or more methods, then the result is a method group with no associated instance expression.</span></span> <span data-ttu-id="dd693-957">Se uma lista de argumentos de tipo tiver sido especificada, ela será usada na chamada de um método genérico ([invocações de método](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="dd693-957">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
   *  <span data-ttu-id="dd693-958">Se `I` identificar uma propriedade `static`, o resultado será um acesso de propriedade sem nenhuma expressão de instância associada.</span><span class="sxs-lookup"><span data-stu-id="dd693-958">If `I` identifies a `static` property, then the result is a property access with no associated instance expression.</span></span>
   *  <span data-ttu-id="dd693-959">Se `I` identificar um campo de `static`:</span><span class="sxs-lookup"><span data-stu-id="dd693-959">If `I` identifies a `static` field:</span></span>
      * <span data-ttu-id="dd693-960">Se o campo for `readonly` e a referência ocorrer fora do construtor estático da classe ou struct no qual o campo é declarado, o resultado será um valor, ou seja, o valor do campo estático `I` em `E`.</span><span class="sxs-lookup"><span data-stu-id="dd693-960">If the field is `readonly` and the reference occurs outside the static constructor of the class or struct in which the field is declared, then the result is a value, namely the value of the static field `I` in `E`.</span></span>
      * <span data-ttu-id="dd693-961">Caso contrário, o resultado será uma variável, ou seja, o campo estático `I` em `E`.</span><span class="sxs-lookup"><span data-stu-id="dd693-961">Otherwise, the result is a variable, namely the static field `I` in `E`.</span></span>
   *  <span data-ttu-id="dd693-962">Se `I` identificar um evento de `static`:</span><span class="sxs-lookup"><span data-stu-id="dd693-962">If `I` identifies a `static` event:</span></span>
      * <span data-ttu-id="dd693-963">Se a referência ocorrer dentro da classe ou struct na qual o evento é declarado e o evento tiver sido declarado sem *event_accessor_declarations* ([eventos](classes.md#events)), `E.I` será processado exatamente como se `I` fosse um campo estático.</span><span class="sxs-lookup"><span data-stu-id="dd693-963">If the reference occurs within the class or struct in which the event is declared, and the event was declared without *event_accessor_declarations* ([Events](classes.md#events)), then `E.I` is processed exactly as if `I` were a static field.</span></span>
      * <span data-ttu-id="dd693-964">Caso contrário, o resultado será um acesso de evento sem nenhuma expressão de instância associada.</span><span class="sxs-lookup"><span data-stu-id="dd693-964">Otherwise, the result is an event access with no associated instance expression.</span></span>
   *  <span data-ttu-id="dd693-965">Se `I` identificar uma constante, o resultado será um valor, ou seja, o valor dessa constante.</span><span class="sxs-lookup"><span data-stu-id="dd693-965">If `I` identifies a constant, then the result is a value, namely the value of that constant.</span></span>
    * <span data-ttu-id="dd693-966">Se `I` identificar um membro de enumeração, o resultado será um valor, ou seja, o valor desse membro de enumeração.</span><span class="sxs-lookup"><span data-stu-id="dd693-966">If `I` identifies an enumeration member, then the result is a value, namely the value of that enumeration member.</span></span>
    * <span data-ttu-id="dd693-967">Caso contrário, `E.I` é uma referência de membro inválida e ocorre um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-967">Otherwise, `E.I` is an invalid member reference, and a compile-time error occurs.</span></span>
*  <span data-ttu-id="dd693-968">Se `E` for um acesso de propriedade, acesso ao indexador, variável ou valor, o tipo de que é `T`e uma pesquisa de membro ([pesquisa de membro](expressions.md#member-lookup)) de `I` em `T` com argumentos de tipo `K` produz uma correspondência, então `E.I` é avaliado e classificado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="dd693-968">If `E` is a property access, indexer access, variable, or value, the type of which is `T`, and a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `T` with `K` type arguments produces a match, then `E.I` is evaluated and classified as follows:</span></span>
   *  <span data-ttu-id="dd693-969">Primeiro, se `E` for um acesso de propriedade ou indexador, o valor da propriedade ou do acesso do indexador será obtido ([valores de expressões](expressions.md#values-of-expressions)) e `E` será reclassificado como um valor.</span><span class="sxs-lookup"><span data-stu-id="dd693-969">First, if `E` is a property or indexer access, then the value of the property or indexer access is obtained ([Values of expressions](expressions.md#values-of-expressions)) and `E` is reclassified as a value.</span></span>
   *  <span data-ttu-id="dd693-970">Se `I` identificar um ou mais métodos, o resultado será um grupo de métodos com uma expressão de instância associada de `E`.</span><span class="sxs-lookup"><span data-stu-id="dd693-970">If `I` identifies one or more methods, then the result is a method group with an associated instance expression of `E`.</span></span> <span data-ttu-id="dd693-971">Se uma lista de argumentos de tipo tiver sido especificada, ela será usada na chamada de um método genérico ([invocações de método](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="dd693-971">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
   *  <span data-ttu-id="dd693-972">Se `I` identificar uma propriedade de instância,</span><span class="sxs-lookup"><span data-stu-id="dd693-972">If `I` identifies an instance property,</span></span>
      * <span data-ttu-id="dd693-973">Se `E` for `this`, `I` identificará uma propriedade implementada automaticamente ([Propriedades implementadas automaticamente](classes.md#automatically-implemented-properties)) sem um setter, e a referência ocorrerá dentro de um construtor de instância para um tipo de classe ou struct `T`, o resultado será uma variável, ou seja, o campo de apoio oculto para a propriedade automática fornecida pelo `I` na instância de `T` fornecida pelo `this`.</span><span class="sxs-lookup"><span data-stu-id="dd693-973">If `E` is `this`, `I` identifies an automatically implemented property ([Automatically implemented properties](classes.md#automatically-implemented-properties)) without a setter, and the reference occurs within an instance constructor for a class or struct type `T`, then the result is a variable, namely the hidden backing field for the auto-property given by `I` in the instance of `T` given by `this`.</span></span>
      * <span data-ttu-id="dd693-974">Caso contrário, o resultado é um acesso de propriedade com uma expressão de instância associada de `E`.</span><span class="sxs-lookup"><span data-stu-id="dd693-974">Otherwise, the result is a property access with an associated instance expression of `E`.</span></span>
   *  <span data-ttu-id="dd693-975">Se `T` for um *class_type* e `I` identificar um campo de instância desse *class_type*:</span><span class="sxs-lookup"><span data-stu-id="dd693-975">If `T` is a *class_type* and `I` identifies an instance field of that *class_type*:</span></span>
      * <span data-ttu-id="dd693-976">Se o valor de `E` for `null`, um `System.NullReferenceException` será gerado.</span><span class="sxs-lookup"><span data-stu-id="dd693-976">If the value of `E` is `null`, then a `System.NullReferenceException` is thrown.</span></span>
      * <span data-ttu-id="dd693-977">Caso contrário, se o campo for `readonly` e a referência ocorrer fora de um construtor de instância da classe na qual o campo é declarado, o resultado será um valor, ou seja, o valor do campo `I` no objeto referenciado por `E`.</span><span class="sxs-lookup"><span data-stu-id="dd693-977">Otherwise, if the field is `readonly` and the reference occurs outside an instance constructor of the class in which the field is declared, then the result is a value, namely the value of the field `I` in the object referenced by `E`.</span></span>
      * <span data-ttu-id="dd693-978">Caso contrário, o resultado será uma variável, ou seja, o campo `I` no objeto referenciado por `E`.</span><span class="sxs-lookup"><span data-stu-id="dd693-978">Otherwise, the result is a variable, namely the field `I` in the object referenced by `E`.</span></span>
   *  <span data-ttu-id="dd693-979">Se `T` for um *struct_type* e `I` identificar um campo de instância desse *struct_type*:</span><span class="sxs-lookup"><span data-stu-id="dd693-979">If `T` is a *struct_type* and `I` identifies an instance field of that *struct_type*:</span></span>
      * <span data-ttu-id="dd693-980">Se `E` for um valor ou se o campo for `readonly` e a referência ocorrer fora de um construtor de instância do struct no qual o campo é declarado, o resultado será um valor, ou seja, o valor do campo `I` na instância de struct fornecida pelo `E`.</span><span class="sxs-lookup"><span data-stu-id="dd693-980">If `E` is a value, or if the field is `readonly` and the reference occurs outside an instance constructor of the struct in which the field is declared, then the result is a value, namely the value of the field `I` in the struct instance given by `E`.</span></span>
      * <span data-ttu-id="dd693-981">Caso contrário, o resultado será uma variável, ou seja, o campo `I` na instância de struct fornecida por `E`.</span><span class="sxs-lookup"><span data-stu-id="dd693-981">Otherwise, the result is a variable, namely the field `I` in the struct instance given by `E`.</span></span>
   *  <span data-ttu-id="dd693-982">Se `I` identificar um evento de instância:</span><span class="sxs-lookup"><span data-stu-id="dd693-982">If `I` identifies an instance event:</span></span>
      * <span data-ttu-id="dd693-983">Se a referência ocorrer dentro da classe ou estrutura na qual o evento é declarado e o evento tiver sido declarado sem *event_accessor_declarations* ([eventos](classes.md#events)) e a referência não ocorrer como o lado esquerdo de um operador `+=` ou `-=`, `E.I` será processada exatamente como se `I` fosse um campo de instância.</span><span class="sxs-lookup"><span data-stu-id="dd693-983">If the reference occurs within the class or struct in which the event is declared, and the event was declared without *event_accessor_declarations* ([Events](classes.md#events)), and the reference does not occur as the left-hand side of a `+=` or `-=` operator, then `E.I` is processed exactly as if `I` was an instance field.</span></span>
      * <span data-ttu-id="dd693-984">Caso contrário, o resultado é um acesso de evento com uma expressão de instância associada de `E`.</span><span class="sxs-lookup"><span data-stu-id="dd693-984">Otherwise, the result is an event access with an associated instance expression of `E`.</span></span>
*  <span data-ttu-id="dd693-985">Caso contrário, será feita uma tentativa de processar `E.I` como uma invocação de método de extensão ([invocações de método de extensão](expressions.md#extension-method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="dd693-985">Otherwise, an attempt is made to process `E.I` as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="dd693-986">Se isso falhar, `E.I` será uma referência de membro inválida e ocorrerá um erro de tempo de vinculação.</span><span class="sxs-lookup"><span data-stu-id="dd693-986">If this fails, `E.I` is an invalid member reference, and a binding-time error occurs.</span></span>

#### <a name="identical-simple-names-and-type-names"></a><span data-ttu-id="dd693-987">Nomes de tipos e nomes simples idênticos</span><span class="sxs-lookup"><span data-stu-id="dd693-987">Identical simple names and type names</span></span>

<span data-ttu-id="dd693-988">Em um acesso de membro do formulário `E.I`, se `E` for um único identificador, e se o significado de `E` como um *Simple_name* ([nomes simples](expressions.md#simple-names)) for uma constante, campo, propriedade, variável local ou parâmetro com o mesmo tipo que o significado de `E` como um *type_name* ([nomes de namespace e tipo](basic-concepts.md#namespace-and-type-names)), então ambos os significados possíveis de `E` são permitidos.</span><span class="sxs-lookup"><span data-stu-id="dd693-988">In a member access of the form `E.I`, if `E` is a single identifier, and if the meaning of `E` as a *simple_name* ([Simple names](expressions.md#simple-names)) is a constant, field, property, local variable, or parameter with the same type as the meaning of `E` as a *type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)), then both possible meanings of `E` are permitted.</span></span> <span data-ttu-id="dd693-989">Os dois significados possíveis de `E.I` nunca são ambíguos, pois `I` deve necessariamente ser um membro do tipo `E` em ambos os casos.</span><span class="sxs-lookup"><span data-stu-id="dd693-989">The two possible meanings of `E.I` are never ambiguous, since `I` must necessarily be a member of the type `E` in both cases.</span></span> <span data-ttu-id="dd693-990">Em outras palavras, a regra simplesmente permite o acesso aos membros estáticos e tipos aninhados de `E` em que, caso contrário, ocorreria um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-990">In other words, the rule simply permits access to the static members and nested types of `E` where a compile-time error would otherwise have occurred.</span></span> <span data-ttu-id="dd693-991">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="dd693-991">For example:</span></span>
```csharp
struct Color
{
    public static readonly Color White = new Color(...);
    public static readonly Color Black = new Color(...);

    public Color Complement() {...}
}

class A
{
    public Color Color;                // Field Color of type Color

    void F() {
        Color = Color.Black;           // References Color.Black static member
        Color = Color.Complement();    // Invokes Complement() on Color field
    }

    static void G() {
        Color c = Color.White;         // References Color.White static member
    }
}
```

#### <a name="grammar-ambiguities"></a><span data-ttu-id="dd693-992">Ambiguidades gramaticais</span><span class="sxs-lookup"><span data-stu-id="dd693-992">Grammar ambiguities</span></span>

<span data-ttu-id="dd693-993">As produções para *Simple_name* ([nomes simples](expressions.md#simple-names)) e *member_access* ([acesso de membro](expressions.md#member-access)) podem dar ao aumento das ambiguidades na gramática para expressões.</span><span class="sxs-lookup"><span data-stu-id="dd693-993">The productions for *simple_name* ([Simple names](expressions.md#simple-names)) and *member_access* ([Member access](expressions.md#member-access)) can give rise to ambiguities in the grammar for expressions.</span></span> <span data-ttu-id="dd693-994">Por exemplo, a instrução:</span><span class="sxs-lookup"><span data-stu-id="dd693-994">For example, the statement:</span></span>
```csharp
F(G<A,B>(7));
```
<span data-ttu-id="dd693-995">pode ser interpretado como uma chamada para `F` com dois argumentos, `G < A` e `B > (7)`.</span><span class="sxs-lookup"><span data-stu-id="dd693-995">could be interpreted as a call to `F` with two arguments, `G < A` and `B > (7)`.</span></span> <span data-ttu-id="dd693-996">Como alternativa, ele pode ser interpretado como uma chamada para `F` com um argumento, que é uma chamada para um método genérico `G` com dois argumentos de tipo e um argumento regular.</span><span class="sxs-lookup"><span data-stu-id="dd693-996">Alternatively, it could be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span>

<span data-ttu-id="dd693-997">Se uma sequência de tokens puder ser analisada (no contexto) como *um Simple_name* ([nomes simples](expressions.md#simple-names)), *member_access* ([acesso de membro](expressions.md#member-access)) ou *pointer_member_access* (acesso de[membro de ponteiro](unsafe-code.md#pointer-member-access)) terminando com um *type_argument_list* ([argumentos de tipo](types.md#type-arguments)), o token imediatamente após o `>` token de fechamento será examinado.</span><span class="sxs-lookup"><span data-stu-id="dd693-997">If a sequence of tokens can be parsed (in context) as a *simple_name* ([Simple names](expressions.md#simple-names)), *member_access* ([Member access](expressions.md#member-access)), or *pointer_member_access* ([Pointer member access](unsafe-code.md#pointer-member-access)) ending with a *type_argument_list* ([Type arguments](types.md#type-arguments)), the token immediately following the closing `>` token is examined.</span></span> <span data-ttu-id="dd693-998">Se for um de</span><span class="sxs-lookup"><span data-stu-id="dd693-998">If it is one of</span></span>
```csharp
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```
<span data-ttu-id="dd693-999">em seguida, a *type_argument_list* é mantida como parte da *Simple_name*, *member_access* ou *pointer_member_access* e qualquer outra análise possível da sequência de tokens é descartada.</span><span class="sxs-lookup"><span data-stu-id="dd693-999">then the *type_argument_list* is retained as part of the *simple_name*, *member_access* or *pointer_member_access* and any other possible parse of the sequence of tokens is discarded.</span></span> <span data-ttu-id="dd693-1000">Caso contrário, o *type_argument_list* não será considerado como parte do *Simple_name*, *member_access* ou *pointer_member_access*, mesmo que não haja outra análise possível da sequência de tokens.</span><span class="sxs-lookup"><span data-stu-id="dd693-1000">Otherwise, the *type_argument_list* is not considered to be part of the *simple_name*, *member_access* or *pointer_member_access*, even if there is no other possible parse of the sequence of tokens.</span></span> <span data-ttu-id="dd693-1001">Observe que essas regras não são aplicadas ao analisar um *type_argument_list* em um *namespace_or_type_name* ([namespace e nomes de tipo](basic-concepts.md#namespace-and-type-names)).</span><span class="sxs-lookup"><span data-stu-id="dd693-1001">Note that these rules are not applied when parsing a *type_argument_list* in a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)).</span></span> <span data-ttu-id="dd693-1002">A instrução</span><span class="sxs-lookup"><span data-stu-id="dd693-1002">The statement</span></span>
```csharp
F(G<A,B>(7));
```
<span data-ttu-id="dd693-1003">de acordo com essa regra, será interpretado como uma chamada para `F` com um argumento, que é uma chamada para um método genérico `G` com dois argumentos de tipo e um argumento regular.</span><span class="sxs-lookup"><span data-stu-id="dd693-1003">will, according to this rule, be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span> <span data-ttu-id="dd693-1004">As instruções</span><span class="sxs-lookup"><span data-stu-id="dd693-1004">The statements</span></span>
```csharp
F(G < A, B > 7);
F(G < A, B >> 7);
```
<span data-ttu-id="dd693-1005">cada um será interpretado como uma chamada para `F` com dois argumentos.</span><span class="sxs-lookup"><span data-stu-id="dd693-1005">will each be interpreted as a call to `F` with two arguments.</span></span> <span data-ttu-id="dd693-1006">A instrução</span><span class="sxs-lookup"><span data-stu-id="dd693-1006">The statement</span></span>
```csharp
x = F < A > +y;
```
<span data-ttu-id="dd693-1007">será interpretado como um operador menor que, maior que operador, e operador de adição unário, como se a instrução tivesse sido gravada `x = (F < A) > (+y)`, em vez de como um *Simple_name* com um *type_argument_list* seguido por um operador Binary Plus.</span><span class="sxs-lookup"><span data-stu-id="dd693-1007">will be interpreted as a less than operator, greater than operator, and unary plus operator, as if the statement had been written `x = (F < A) > (+y)`, instead of as a *simple_name* with a *type_argument_list* followed by a binary plus operator.</span></span> <span data-ttu-id="dd693-1008">Na instrução</span><span class="sxs-lookup"><span data-stu-id="dd693-1008">In the statement</span></span>
```csharp
x = y is C<T> + z;
```
<span data-ttu-id="dd693-1009">os tokens `C<T>` são interpretados como um *namespace_or_type_name* com um *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="dd693-1009">the tokens `C<T>` are interpreted as a *namespace_or_type_name* with a *type_argument_list*.</span></span>

### <a name="invocation-expressions"></a><span data-ttu-id="dd693-1010">Expressões de invocação</span><span class="sxs-lookup"><span data-stu-id="dd693-1010">Invocation expressions</span></span>

<span data-ttu-id="dd693-1011">Um *invocation_expression* é usado para invocar um método.</span><span class="sxs-lookup"><span data-stu-id="dd693-1011">An *invocation_expression* is used to invoke a method.</span></span>

```antlr
invocation_expression
    : primary_expression '(' argument_list? ')'
    ;
```

<span data-ttu-id="dd693-1012">Uma *invocation_expression* é vinculada dinamicamente ([associação dinâmica](expressions.md#dynamic-binding)) se pelo menos uma das seguintes isenções:</span><span class="sxs-lookup"><span data-stu-id="dd693-1012">An *invocation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) if at least one of the following holds:</span></span>

* <span data-ttu-id="dd693-1013">O *primary_expression* tem `dynamic`de tipo de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1013">The *primary_expression* has compile-time type `dynamic`.</span></span>
* <span data-ttu-id="dd693-1014">Pelo menos um argumento da *argument_list* opcional tem o tipo de tempo de compilação `dynamic` e o *primary_expression* não tem um tipo delegado.</span><span class="sxs-lookup"><span data-stu-id="dd693-1014">At least one argument of the optional *argument_list* has compile-time type `dynamic` and the *primary_expression* does not have a delegate type.</span></span>

<span data-ttu-id="dd693-1015">Nesse caso, o compilador classifica o *invocation_expression* como um valor do tipo `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1015">In this case the compiler classifies the *invocation_expression* as a value of type `dynamic`.</span></span> <span data-ttu-id="dd693-1016">As regras abaixo para determinar o significado do *invocation_expression* são aplicadas em tempo de execução, usando o tipo de tempo de execução em vez do tipo de tempo de compilação dos argumentos *primary_expression* e que têm o tipo de tempo de compilação `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1016">The rules below to determine the meaning of the *invocation_expression* are then applied at run-time, using the run-time type instead of the compile-time type of those of the *primary_expression* and arguments which have the compile-time type `dynamic`.</span></span> <span data-ttu-id="dd693-1017">Se o *primary_expression* não tiver o tipo de tempo de compilação `dynamic`, a invocação de método passará por uma verificação de tempo de compilação limitada, conforme descrito em [verificação de tempo de compilação da resolução dinâmica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="dd693-1017">If the *primary_expression* does not have compile-time type `dynamic`, then the method invocation undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="dd693-1018">O *primary_expression* de um *invocation_expression* deve ser um grupo de métodos ou um valor de um *delegate_type*.</span><span class="sxs-lookup"><span data-stu-id="dd693-1018">The *primary_expression* of an *invocation_expression* must be a method group or a value of a *delegate_type*.</span></span> <span data-ttu-id="dd693-1019">Se o *primary_expression* for um grupo de métodos, a *invocation_expression* será uma invocação de método ([invocações de método](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="dd693-1019">If the *primary_expression* is a method group, the *invocation_expression* is a method invocation ([Method invocations](expressions.md#method-invocations)).</span></span> <span data-ttu-id="dd693-1020">Se o *primary_expression* for um valor de um *delegate_type*, o *invocation_expression* será uma invocação de delegado ([invocações de delegado](expressions.md#delegate-invocations)).</span><span class="sxs-lookup"><span data-stu-id="dd693-1020">If the *primary_expression* is a value of a *delegate_type*, the *invocation_expression* is a delegate invocation ([Delegate invocations](expressions.md#delegate-invocations)).</span></span> <span data-ttu-id="dd693-1021">Se o *primary_expression* não for um grupo de métodos nem um valor de um *delegate_type*, ocorrerá um erro de tempo de associação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1021">If the *primary_expression* is neither a method group nor a value of a *delegate_type*, a binding-time error occurs.</span></span>

<span data-ttu-id="dd693-1022">O *argument_list* opcional ([listas de argumentos](expressions.md#argument-lists)) fornece valores ou referências de variáveis para os parâmetros do método.</span><span class="sxs-lookup"><span data-stu-id="dd693-1022">The optional *argument_list* ([Argument lists](expressions.md#argument-lists)) provides values or variable references for the parameters of the method.</span></span>

<span data-ttu-id="dd693-1023">O resultado da avaliação de um *invocation_expression* é classificado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="dd693-1023">The result of evaluating an *invocation_expression* is classified as follows:</span></span>

*  <span data-ttu-id="dd693-1024">Se o *invocation_expression* invocar um método ou delegado que retorna `void`, o resultado será Nothing.</span><span class="sxs-lookup"><span data-stu-id="dd693-1024">If the *invocation_expression* invokes a method or delegate that returns `void`, the result is nothing.</span></span> <span data-ttu-id="dd693-1025">Uma expressão que é classificada como nada é permitida apenas no contexto de uma *statement_expression* ([instruções de expressão](statements.md#expression-statements)) ou como o corpo de uma *lambda_expression* ([expressões de função anônimas](expressions.md#anonymous-function-expressions)).</span><span class="sxs-lookup"><span data-stu-id="dd693-1025">An expression that is classified as nothing is permitted only in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)) or as the body of a *lambda_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)).</span></span> <span data-ttu-id="dd693-1026">Caso contrário, ocorrerá um erro de tempo de vinculação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1026">Otherwise a binding-time error occurs.</span></span>
*  <span data-ttu-id="dd693-1027">Caso contrário, o resultado será um valor do tipo retornado pelo método ou delegado.</span><span class="sxs-lookup"><span data-stu-id="dd693-1027">Otherwise, the result is a value of the type returned by the method or delegate.</span></span>

#### <a name="method-invocations"></a><span data-ttu-id="dd693-1028">Invocações de método</span><span class="sxs-lookup"><span data-stu-id="dd693-1028">Method invocations</span></span>

<span data-ttu-id="dd693-1029">Para uma invocação de método, a *primary_expression* do *invocation_expression* deve ser um grupo de métodos.</span><span class="sxs-lookup"><span data-stu-id="dd693-1029">For a method invocation, the *primary_expression* of the *invocation_expression* must be a method group.</span></span> <span data-ttu-id="dd693-1030">O grupo de métodos identifica o método a ser invocado ou o conjunto de métodos sobrecarregados do qual escolher um método específico a ser invocado.</span><span class="sxs-lookup"><span data-stu-id="dd693-1030">The method group identifies the one method to invoke or the set of overloaded methods from which to choose a specific method to invoke.</span></span> <span data-ttu-id="dd693-1031">No último caso, a determinação do método específico a ser invocado é baseada no contexto fornecido pelos tipos dos argumentos na *argument_list*.</span><span class="sxs-lookup"><span data-stu-id="dd693-1031">In the latter case, determination of the specific method to invoke is based on the context provided by the types of the arguments in the *argument_list*.</span></span>

<span data-ttu-id="dd693-1032">O processamento de tempo de associação de uma invocação de método do formulário `M(A)`, em que `M` é um grupo de métodos (possivelmente incluindo um *type_argument_list*) e `A` é um *argument_list*opcional, consiste nas seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="dd693-1032">The binding-time processing of a method invocation of the form `M(A)`, where `M` is a method group (possibly including a *type_argument_list*), and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="dd693-1033">O conjunto de métodos candidatos para a invocação do método é construído.</span><span class="sxs-lookup"><span data-stu-id="dd693-1033">The set of candidate methods for the method invocation is constructed.</span></span> <span data-ttu-id="dd693-1034">Para cada método `F` associado ao grupo de métodos `M`:</span><span class="sxs-lookup"><span data-stu-id="dd693-1034">For each method `F` associated with the method group `M`:</span></span>
   *  <span data-ttu-id="dd693-1035">Se `F` não for genérica, `F` será um candidato quando:</span><span class="sxs-lookup"><span data-stu-id="dd693-1035">If `F` is non-generic, `F` is a candidate when:</span></span>
      * <span data-ttu-id="dd693-1036">`M` não tem nenhuma lista de argumentos de tipo e</span><span class="sxs-lookup"><span data-stu-id="dd693-1036">`M` has no type argument list, and</span></span>
      * <span data-ttu-id="dd693-1037">`F` é aplicável em relação a `A` ([membro da função aplicável](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="dd693-1037">`F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
   *  <span data-ttu-id="dd693-1038">Se `F` for genérica e `M` não tiver nenhuma lista de argumentos de tipo, `F` será um candidato quando:</span><span class="sxs-lookup"><span data-stu-id="dd693-1038">If `F` is generic and `M` has no type argument list, `F` is a candidate when:</span></span>
      * <span data-ttu-id="dd693-1039">A inferência de tipos ([inferência de tipos](expressions.md#type-inference)) é bem sucedido, inferindo-se a uma lista de argumentos de tipo para a chamada e</span><span class="sxs-lookup"><span data-stu-id="dd693-1039">Type inference ([Type inference](expressions.md#type-inference)) succeeds, inferring a list of type arguments for the call, and</span></span>
      * <span data-ttu-id="dd693-1040">Depois que os argumentos de tipo inferido são substituídos pelos parâmetros de tipo de método correspondentes, todos os tipos construídos na lista de parâmetros de F atendem às suas restrições ([atendendo às restrições](types.md#satisfying-constraints)) e a lista de parâmetros de `F` é aplicável em relação a `A` ([membro de função aplicável](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="dd693-1040">Once the inferred type arguments are substituted for the corresponding method type parameters, all constructed types in the parameter list of F satisfy their constraints ([Satisfying constraints](types.md#satisfying-constraints)), and the parameter list of `F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
   *  <span data-ttu-id="dd693-1041">Se `F` for genérica e `M` incluir uma lista de argumentos de tipo, `F` será um candidato quando:</span><span class="sxs-lookup"><span data-stu-id="dd693-1041">If `F` is generic and `M` includes a type argument list, `F` is a candidate when:</span></span>
      * <span data-ttu-id="dd693-1042">`F` tem o mesmo número de parâmetros de tipo de método que foram fornecidos na lista de argumentos de tipo e</span><span class="sxs-lookup"><span data-stu-id="dd693-1042">`F` has the same number of method type parameters as were supplied in the type argument list, and</span></span>
      * <span data-ttu-id="dd693-1043">Depois que os argumentos de tipo são substituídos pelos parâmetros de tipo de método correspondentes, todos os tipos construídos na lista de parâmetros de F atendem às suas restrições ([atendendo às restrições](types.md#satisfying-constraints)) e a lista de parâmetros de `F` é aplicável em relação a `A` ([membro de função aplicável](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="dd693-1043">Once the type arguments are substituted for the corresponding method type parameters, all constructed types in the parameter list of F satisfy their constraints ([Satisfying constraints](types.md#satisfying-constraints)), and the parameter list of `F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
*  <span data-ttu-id="dd693-1044">O conjunto de métodos candidatos é reduzido para conter apenas métodos dos tipos mais derivados: para cada método `C.F` no conjunto, em que `C` é o tipo no qual o método `F` é declarado, todos os métodos declarados em um tipo base de `C` são removidos do conjunto.</span><span class="sxs-lookup"><span data-stu-id="dd693-1044">The set of candidate methods is reduced to contain only methods from the most derived types: For each method `C.F` in the set, where `C` is the type in which the method `F` is declared, all methods declared in a base type of `C` are removed from the set.</span></span> <span data-ttu-id="dd693-1045">Além disso, se `C` for um tipo de classe diferente de `object`, todos os métodos declarados em um tipo de interface serão removidos do conjunto.</span><span class="sxs-lookup"><span data-stu-id="dd693-1045">Furthermore, if `C` is a class type other than `object`, all methods declared in an interface type are removed from the set.</span></span> <span data-ttu-id="dd693-1046">(Essa última regra só afeta quando o grupo de métodos era o resultado de uma pesquisa de membro em um parâmetro de tipo com uma classe base efetiva diferente de Object e uma interface efetiva não vazia definida.)</span><span class="sxs-lookup"><span data-stu-id="dd693-1046">(This latter rule only has affect when the method group was the result of a member lookup on a type parameter having an effective base class other than object and a non-empty effective interface set.)</span></span>
*  <span data-ttu-id="dd693-1047">Se o conjunto resultante de métodos candidatos estiver vazio, o processamento adicional nas etapas a seguir será abandonado e, em vez disso, será feita uma tentativa de processar a invocação como uma invocação de método de extensão ([invocações de método de extensão](expressions.md#extension-method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="dd693-1047">If the resulting set of candidate methods is empty, then further processing along the following steps are abandoned, and instead an attempt is made to process the invocation as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="dd693-1048">Se isso falhar, não existirá nenhum método aplicável e ocorrerá um erro de tempo de vinculação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1048">If this fails, then no applicable methods exist, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="dd693-1049">O melhor método do conjunto de métodos candidatos é identificado usando as regras de resolução de sobrecarga da [resolução de sobrecarga](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="dd693-1049">The best method of the set of candidate methods is identified using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="dd693-1050">Se um único método melhor não puder ser identificado, a invocação do método será ambígua e ocorrerá um erro de tempo de ligação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1050">If a single best method cannot be identified, the method invocation is ambiguous, and a binding-time error occurs.</span></span> <span data-ttu-id="dd693-1051">Ao executar a resolução de sobrecarga, os parâmetros de um método genérico são considerados após a substituição dos argumentos de tipo (fornecidos ou inferidos) para os parâmetros de tipo de método correspondentes.</span><span class="sxs-lookup"><span data-stu-id="dd693-1051">When performing overload resolution, the parameters of a generic method are considered after substituting the type arguments (supplied or inferred) for the corresponding method type parameters.</span></span>
*  <span data-ttu-id="dd693-1052">A validação final do melhor método escolhido é executada:</span><span class="sxs-lookup"><span data-stu-id="dd693-1052">Final validation of the chosen best method is performed:</span></span>
   * <span data-ttu-id="dd693-1053">O método é validado no contexto do grupo de métodos: se o melhor método é um método estático, o grupo de métodos deve ter resultado de um *Simple_name* ou de um *member_access* por meio de um tipo.</span><span class="sxs-lookup"><span data-stu-id="dd693-1053">The method is validated in the context of the method group: If the best method is a static method, the method group must have resulted from a *simple_name* or a *member_access* through a type.</span></span> <span data-ttu-id="dd693-1054">Se o melhor método for um método de instância, o grupo de métodos deverá ter resultado de uma *Simple_name*, uma *member_access* por meio de uma variável ou valor ou uma *base_access*.</span><span class="sxs-lookup"><span data-stu-id="dd693-1054">If the best method is an instance method, the method group must have resulted from a *simple_name*, a *member_access* through a variable or value, or a *base_access*.</span></span> <span data-ttu-id="dd693-1055">Se nenhum desses requisitos for verdadeiro, ocorrerá um erro de tempo de ligação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1055">If neither of these requirements is true, a binding-time error occurs.</span></span>
   * <span data-ttu-id="dd693-1056">Se o melhor método for um método genérico, os argumentos de tipo (fornecidos ou inferidos) serão verificados em relação às restrições ([atendendo às restrições](types.md#satisfying-constraints)) declaradas no método genérico.</span><span class="sxs-lookup"><span data-stu-id="dd693-1056">If the best method is a generic method, the type arguments (supplied or inferred) are checked against the constraints ([Satisfying constraints](types.md#satisfying-constraints)) declared on the generic method.</span></span> <span data-ttu-id="dd693-1057">Se qualquer argumento de tipo não atender às restrições correspondentes no parâmetro de tipo, ocorrerá um erro de tempo de associação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1057">If any type argument does not satisfy the corresponding constraint(s) on the type parameter, a binding-time error occurs.</span></span>

<span data-ttu-id="dd693-1058">Depois que um método tiver sido selecionado e validado em tempo de vinculação pelas etapas acima, a invocação de tempo de execução real será processada de acordo com as regras de invocação de membro de função descrita na [verificação de tempo de compilação da resolução dinâmica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="dd693-1058">Once a method has been selected and validated at binding-time by the above steps, the actual run-time invocation is processed according to the rules of function member invocation described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="dd693-1059">O efeito intuitivo das regras de resolução descritas acima é o seguinte: para localizar o método específico invocado por uma invocação de método, comece com o tipo indicado pela invocação do método e continue a cadeia de herança até pelo menos uma aplicável, declaração de método acessível e não substituída encontrada.</span><span class="sxs-lookup"><span data-stu-id="dd693-1059">The intuitive effect of the resolution rules described above is as follows: To locate the particular method invoked by a method invocation, start with the type indicated by the method invocation and proceed up the inheritance chain until at least one applicable, accessible, non-override method declaration is found.</span></span> <span data-ttu-id="dd693-1060">Em seguida, execute a inferência de tipos e a resolução de sobrecarga no conjunto de métodos aplicáveis, acessíveis e não substituição declarados nesse tipo e invoque o método, portanto, selecionado.</span><span class="sxs-lookup"><span data-stu-id="dd693-1060">Then perform type inference and overload resolution on the set of applicable, accessible, non-override methods declared in that type and invoke the method thus selected.</span></span> <span data-ttu-id="dd693-1061">Se nenhum método foi encontrado, tente em vez disso processar a invocação como uma invocação de método de extensão.</span><span class="sxs-lookup"><span data-stu-id="dd693-1061">If no method was found, try instead to process the invocation as an extension method invocation.</span></span>

#### <a name="extension-method-invocations"></a><span data-ttu-id="dd693-1062">Invocações de método de extensão</span><span class="sxs-lookup"><span data-stu-id="dd693-1062">Extension method invocations</span></span>

<span data-ttu-id="dd693-1063">Em uma invocação de método ([invocações em instâncias em caixas](expressions.md#invocations-on-boxed-instances)) de um dos formulários</span><span class="sxs-lookup"><span data-stu-id="dd693-1063">In a method invocation ([Invocations on boxed instances](expressions.md#invocations-on-boxed-instances)) of one of the forms</span></span>
```csharp
expr . identifier ( )

expr . identifier ( args )

expr . identifier < typeargs > ( )

expr . identifier < typeargs > ( args )
```
<span data-ttu-id="dd693-1064">Se o processamento normal da invocação não encontrar nenhum método aplicável, será feita uma tentativa de processar a construção como uma invocação de método de extensão.</span><span class="sxs-lookup"><span data-stu-id="dd693-1064">if the normal processing of the invocation finds no applicable methods, an attempt is made to process the construct as an extension method invocation.</span></span> <span data-ttu-id="dd693-1065">Se *expr* ou qualquer um dos *args* tiver o tipo de tempo de compilação `dynamic`, os métodos de extensão não serão aplicados.</span><span class="sxs-lookup"><span data-stu-id="dd693-1065">If *expr* or any of the *args* has compile-time type `dynamic`, extension methods will not apply.</span></span>

<span data-ttu-id="dd693-1066">O objetivo é encontrar a melhor *type_name* `C`, para que a invocação de método estático correspondente possa ocorrer:</span><span class="sxs-lookup"><span data-stu-id="dd693-1066">The objective is to find the best *type_name* `C`, so that the corresponding static method invocation can take place:</span></span>
```csharp
C . identifier ( expr )

C . identifier ( expr , args )

C . identifier < typeargs > ( expr )

C . identifier < typeargs > ( expr , args )
```

<span data-ttu-id="dd693-1067">Um método de extensão `Ci.Mj` será ***elegível*** se:</span><span class="sxs-lookup"><span data-stu-id="dd693-1067">An extension method `Ci.Mj` is ***eligible*** if:</span></span>

*  <span data-ttu-id="dd693-1068">`Ci` é uma classe não genérica e não aninhada</span><span class="sxs-lookup"><span data-stu-id="dd693-1068">`Ci` is a non-generic, non-nested class</span></span>
*  <span data-ttu-id="dd693-1069">O nome do `Mj` é *identificador*</span><span class="sxs-lookup"><span data-stu-id="dd693-1069">The name of `Mj` is *identifier*</span></span>
*  <span data-ttu-id="dd693-1070">`Mj` é acessível e aplicável quando aplicado aos argumentos como um método estático, conforme mostrado acima</span><span class="sxs-lookup"><span data-stu-id="dd693-1070">`Mj` is accessible and applicable when applied to the arguments as a static method as shown above</span></span>
*  <span data-ttu-id="dd693-1071">Existe uma identidade implícita, uma referência ou uma conversão boxing de *expr* para o tipo do primeiro parâmetro de `Mj`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1071">An implicit identity, reference or boxing conversion exists from *expr* to the type of the first parameter of `Mj`.</span></span>

<span data-ttu-id="dd693-1072">A pesquisa por `C` prossegue da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="dd693-1072">The search for `C` proceeds as follows:</span></span>

*  <span data-ttu-id="dd693-1073">Começando com a declaração de namespace delimitadora mais próxima, continuando com cada declaração de namespace delimitadora e terminando com a unidade de compilação que a contém, as tentativas sucessivas são feitas para encontrar um conjunto candidato de métodos de extensão:</span><span class="sxs-lookup"><span data-stu-id="dd693-1073">Starting with the closest enclosing namespace declaration, continuing with each enclosing namespace declaration, and ending with the containing compilation unit, successive attempts are made to find a candidate set of extension methods:</span></span>
   * <span data-ttu-id="dd693-1074">Se o namespace ou a unidade de compilação fornecida diretamente contiver declarações de tipo não genéricas `Ci` com métodos de extensão qualificados `Mj`, o conjunto desses métodos de extensão será o conjunto de candidatos.</span><span class="sxs-lookup"><span data-stu-id="dd693-1074">If the given namespace or compilation unit directly contains non-generic type declarations `Ci` with eligible extension methods `Mj`, then the set of those extension methods is the candidate set.</span></span>
   * <span data-ttu-id="dd693-1075">Se tipos `Ci` importados por *using_static_declarations* e declarados diretamente em namespaces importados por *using_namespace_directive*s no namespace ou na unidade de compilação fornecida diretamente contêm métodos de extensão qualificados `Mj`, o conjunto desses métodos de extensão é o conjunto candidato.</span><span class="sxs-lookup"><span data-stu-id="dd693-1075">If types `Ci` imported by *using_static_declarations* and directly declared in namespaces imported by *using_namespace_directive*s in the given namespace or compilation unit directly contain eligible extension methods `Mj`, then the set of those extension methods is the candidate set.</span></span>
*  <span data-ttu-id="dd693-1076">Se nenhum conjunto candidato for encontrado em qualquer declaração de namespace ou unidade de compilação delimitadora ocorrer um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1076">If no candidate set is found in any enclosing namespace declaration or compilation unit, a compile-time error occurs.</span></span>
*  <span data-ttu-id="dd693-1077">Caso contrário, a resolução de sobrecarga será aplicada ao conjunto de candidatos, conforme descrito em ([resolução de sobrecarga](expressions.md#overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="dd693-1077">Otherwise, overload resolution is applied to the candidate set as described in ([Overload resolution](expressions.md#overload-resolution)).</span></span> <span data-ttu-id="dd693-1078">Se nenhum único método melhor for encontrado, ocorrerá um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1078">If no single best method is found, a compile-time error occurs.</span></span>
*  <span data-ttu-id="dd693-1079">`C` é o tipo no qual o melhor método é declarado como um método de extensão.</span><span class="sxs-lookup"><span data-stu-id="dd693-1079">`C` is the type within which the best method is declared as an extension method.</span></span>

<span data-ttu-id="dd693-1080">Usando `C` como um destino, a chamada de método é processada como uma invocação de método estático ([verificação de tempo de compilação da resolução dinâmica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="dd693-1080">Using `C` as a target, the method call is then processed as a static method invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span>

<span data-ttu-id="dd693-1081">As regras anteriores significam que os métodos de instância têm precedência sobre os métodos de extensão, que os métodos de extensão disponíveis nas declarações de namespace interno têm precedência sobre os métodos de extensão disponíveis nas declarações de namespace externo e essa extensão os métodos declarados diretamente em um namespace têm precedência sobre os métodos de extensão importados para o mesmo namespace com uma diretiva de namespace using.</span><span class="sxs-lookup"><span data-stu-id="dd693-1081">The preceding rules mean that instance methods take precedence over extension methods, that extension methods available in inner namespace declarations take precedence over extension methods available in outer namespace declarations, and that extension methods declared directly in a namespace take precedence over extension methods imported into that same namespace with a using namespace directive.</span></span> <span data-ttu-id="dd693-1082">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="dd693-1082">For example:</span></span>
```csharp
public static class E
{
    public static void F(this object obj, int i) { }

    public static void F(this object obj, string s) { }
}

class A { }

class B
{
    public void F(int i) { }
}

class C
{
    public void F(object obj) { }
}

class X
{
    static void Test(A a, B b, C c) {
        a.F(1);              // E.F(object, int)
        a.F("hello");        // E.F(object, string)

        b.F(1);              // B.F(int)
        b.F("hello");        // E.F(object, string)

        c.F(1);              // C.F(object)
        c.F("hello");        // C.F(object)
    }
}
```

<span data-ttu-id="dd693-1083">No exemplo, o método de `B`tem precedência sobre o primeiro método de extensão e o método de `C`tem precedência sobre os dois métodos de extensão.</span><span class="sxs-lookup"><span data-stu-id="dd693-1083">In the example, `B`'s method takes precedence over the first extension method, and `C`'s method takes precedence over both extension methods.</span></span>

```csharp
public static class C
{
    public static void F(this int i) { Console.WriteLine("C.F({0})", i); }
    public static void G(this int i) { Console.WriteLine("C.G({0})", i); }
    public static void H(this int i) { Console.WriteLine("C.H({0})", i); }
}

namespace N1
{
    public static class D
    {
        public static void F(this int i) { Console.WriteLine("D.F({0})", i); }
        public static void G(this int i) { Console.WriteLine("D.G({0})", i); }
    }
}

namespace N2
{
    using N1;

    public static class E
    {
        public static void F(this int i) { Console.WriteLine("E.F({0})", i); }
    }

    class Test
    {
        static void Main(string[] args)
        {
            1.F();
            2.G();
            3.H();
        }
    }
}
```

<span data-ttu-id="dd693-1084">A saída deste exemplo é:</span><span class="sxs-lookup"><span data-stu-id="dd693-1084">The output of this example is:</span></span>
```console
E.F(1)
D.G(2)
C.H(3)
```
<span data-ttu-id="dd693-1085">`D.G` tem precedência sobre `C.G`e `E.F` tem precedência sobre `D.F` e `C.F`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1085">`D.G` takes precedence over `C.G`, and `E.F` takes precedence over both `D.F` and `C.F`.</span></span>

#### <a name="delegate-invocations"></a><span data-ttu-id="dd693-1086">Delegar invocações</span><span class="sxs-lookup"><span data-stu-id="dd693-1086">Delegate invocations</span></span>

<span data-ttu-id="dd693-1087">Para uma invocação de delegado, a *primary_expression* do *invocation_expression* deve ser um valor de um *delegate_type*.</span><span class="sxs-lookup"><span data-stu-id="dd693-1087">For a delegate invocation, the *primary_expression* of the *invocation_expression* must be a value of a *delegate_type*.</span></span> <span data-ttu-id="dd693-1088">Além disso, Considerando que a *delegate_type* seja um membro de função com a mesma lista de parâmetros que a *delegate_type*, a *delegate_type* deve ser aplicável ([membro de função aplicável](expressions.md#applicable-function-member)) em relação ao *argument_list* do *invocation_expression*.</span><span class="sxs-lookup"><span data-stu-id="dd693-1088">Furthermore, considering the *delegate_type* to be a function member with the same parameter list as the *delegate_type*, the *delegate_type* must be applicable ([Applicable function member](expressions.md#applicable-function-member)) with respect to the *argument_list* of the *invocation_expression*.</span></span>

<span data-ttu-id="dd693-1089">O processamento em tempo de execução de uma invocação de delegado do formulário `D(A)`, em que `D` é uma *primary_expression* de um *delegate_type* e `A` é um *argument_list*opcional, consiste nas seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="dd693-1089">The run-time processing of a delegate invocation of the form `D(A)`, where `D` is a *primary_expression* of a *delegate_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="dd693-1090">`D` é avaliado.</span><span class="sxs-lookup"><span data-stu-id="dd693-1090">`D` is evaluated.</span></span> <span data-ttu-id="dd693-1091">Se essa avaliação causar uma exceção, nenhuma etapa adicional será executada.</span><span class="sxs-lookup"><span data-stu-id="dd693-1091">If this evaluation causes an exception, no further steps are executed.</span></span>
*  <span data-ttu-id="dd693-1092">O valor de `D` é verificado para ser válido.</span><span class="sxs-lookup"><span data-stu-id="dd693-1092">The value of `D` is checked to be valid.</span></span> <span data-ttu-id="dd693-1093">Se o valor de `D` for `null`, um `System.NullReferenceException` será lançado e nenhuma etapa adicional será executada.</span><span class="sxs-lookup"><span data-stu-id="dd693-1093">If the value of `D` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="dd693-1094">Caso contrário, `D` é uma referência a uma instância de delegate.</span><span class="sxs-lookup"><span data-stu-id="dd693-1094">Otherwise, `D` is a reference to a delegate instance.</span></span> <span data-ttu-id="dd693-1095">Invocações de membro de função ([verificação de tempo de compilação de resolução dinâmica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) são executadas em cada uma das entidades que podem ser chamadas na lista de invocação do delegado.</span><span class="sxs-lookup"><span data-stu-id="dd693-1095">Function member invocations ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) are performed on each of the callable entities in the invocation list of the delegate.</span></span> <span data-ttu-id="dd693-1096">Para entidades que podem ser chamadas que consistem em um método de instância e instância, a instância para a invocação é a instância contida na entidade que pôde ser chamada.</span><span class="sxs-lookup"><span data-stu-id="dd693-1096">For callable entities consisting of an instance and instance method, the instance for the invocation is the instance contained in the callable entity.</span></span>

### <a name="element-access"></a><span data-ttu-id="dd693-1097">Acesso ao elemento</span><span class="sxs-lookup"><span data-stu-id="dd693-1097">Element access</span></span>

<span data-ttu-id="dd693-1098">Um *element_access* consiste em um *primary_no_array_creation_expression*, seguido por um token "`[`", seguido por um *argument_list*, seguido por um token "`]`".</span><span class="sxs-lookup"><span data-stu-id="dd693-1098">An *element_access* consists of a *primary_no_array_creation_expression*, followed by a "`[`" token, followed by an *argument_list*, followed by a "`]`" token.</span></span> <span data-ttu-id="dd693-1099">O *argument_list* consiste em um ou mais s de *argumento*, separados por vírgulas.</span><span class="sxs-lookup"><span data-stu-id="dd693-1099">The *argument_list* consists of one or more *argument*s, separated by commas.</span></span>

```antlr
element_access
    : primary_no_array_creation_expression '[' expression_list ']'
    ;
```

<span data-ttu-id="dd693-1100">O *argument_list* de um *element_access* não pode conter argumentos `ref` ou `out`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1100">The *argument_list* of an *element_access* is not allowed to contain `ref` or `out` arguments.</span></span>

<span data-ttu-id="dd693-1101">Uma *element_access* é vinculada dinamicamente ([associação dinâmica](expressions.md#dynamic-binding)) se pelo menos uma das seguintes isenções:</span><span class="sxs-lookup"><span data-stu-id="dd693-1101">An *element_access* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) if at least one of the following holds:</span></span>

* <span data-ttu-id="dd693-1102">O *primary_no_array_creation_expression* tem `dynamic`de tipo de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1102">The *primary_no_array_creation_expression* has compile-time type `dynamic`.</span></span>
* <span data-ttu-id="dd693-1103">Pelo menos uma expressão da *argument_list* tem o tipo de tempo de compilação `dynamic` e a *primary_no_array_creation_expression* não tem um tipo de matriz.</span><span class="sxs-lookup"><span data-stu-id="dd693-1103">At least one expression of the *argument_list* has compile-time type `dynamic` and the *primary_no_array_creation_expression* does not have an array type.</span></span>

<span data-ttu-id="dd693-1104">Nesse caso, o compilador classifica o *element_access* como um valor do tipo `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1104">In this case the compiler classifies the *element_access* as a value of type `dynamic`.</span></span> <span data-ttu-id="dd693-1105">As regras abaixo para determinar o significado da *element_access* são então aplicadas em tempo de execução, usando o tipo de tempo de execução em vez do tipo de tempo de compilação das expressões *primary_no_array_creation_expression* e *argument_list* que têm o tipo de tempo de compilação `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1105">The rules below to determine the meaning of the *element_access* are then applied at run-time, using the run-time type instead of the compile-time type of those of the *primary_no_array_creation_expression* and *argument_list* expressions which have the compile-time type `dynamic`.</span></span> <span data-ttu-id="dd693-1106">Se o *primary_no_array_creation_expression* não tiver o tipo de tempo de compilação `dynamic`, o acesso ao elemento passará por uma verificação de tempo de compilação limitada, conforme descrito em [verificação de tempo de compilação da resolução dinâmica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="dd693-1106">If the *primary_no_array_creation_expression* does not have compile-time type `dynamic`, then the element access undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="dd693-1107">Se a *primary_no_array_creation_expression* de um *element_access* for um valor de um *array_type*, o *element_access* será um acesso à matriz ([acesso à matriz](expressions.md#array-access)).</span><span class="sxs-lookup"><span data-stu-id="dd693-1107">If the *primary_no_array_creation_expression* of an *element_access* is a value of an *array_type*, the *element_access* is an array access ([Array access](expressions.md#array-access)).</span></span> <span data-ttu-id="dd693-1108">Caso contrário, o *primary_no_array_creation_expression* deve ser uma variável ou um valor de uma classe, struct ou tipo de interface que tenha um ou mais membros do indexador; nesse caso, a *element_access* é um acesso do indexador ([acesso ao indexador](expressions.md#indexer-access)).</span><span class="sxs-lookup"><span data-stu-id="dd693-1108">Otherwise, the *primary_no_array_creation_expression* must be a variable or value of a class, struct, or interface type that has one or more indexer members, in which case the *element_access* is an indexer access ([Indexer access](expressions.md#indexer-access)).</span></span>

#### <a name="array-access"></a><span data-ttu-id="dd693-1109">Acesso de matriz</span><span class="sxs-lookup"><span data-stu-id="dd693-1109">Array access</span></span>

<span data-ttu-id="dd693-1110">Para um acesso de matriz, o *primary_no_array_creation_expression* do *element_access* deve ser um valor de um *array_type*.</span><span class="sxs-lookup"><span data-stu-id="dd693-1110">For an array access, the *primary_no_array_creation_expression* of the *element_access* must be a value of an *array_type*.</span></span> <span data-ttu-id="dd693-1111">Além disso, o *argument_list* de um acesso à matriz não tem permissão para conter argumentos nomeados. O número de expressões na *argument_list* deve ser igual à classificação da *array_type*, e cada expressão deve ser do tipo `int`, `uint`, `long`, `ulong`ou deve ser conversível implicitamente em um ou mais desses tipos.</span><span class="sxs-lookup"><span data-stu-id="dd693-1111">Furthermore, the *argument_list* of an array access is not allowed to contain named arguments.The number of expressions in the *argument_list* must be the same as the rank of the *array_type*, and each expression must be of type `int`, `uint`, `long`, `ulong`, or must be implicitly convertible to one or more of these types.</span></span>

<span data-ttu-id="dd693-1112">O resultado da avaliação de um acesso à matriz é uma variável do tipo de elemento da matriz, ou seja, o elemento de matriz selecionado pelo (s) valor (es) das expressões no *argument_list*.</span><span class="sxs-lookup"><span data-stu-id="dd693-1112">The result of evaluating an array access is a variable of the element type of the array, namely the array element selected by the value(s) of the expression(s) in the *argument_list*.</span></span>

<span data-ttu-id="dd693-1113">O processamento em tempo de execução de um acesso de matriz do formulário `P[A]`, em que `P` é uma *primary_no_array_creation_expression* de um *array_type* e `A` é um *argument_list*, consiste nas seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="dd693-1113">The run-time processing of an array access of the form `P[A]`, where `P` is a *primary_no_array_creation_expression* of an *array_type* and `A` is an *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="dd693-1114">`P` é avaliado.</span><span class="sxs-lookup"><span data-stu-id="dd693-1114">`P` is evaluated.</span></span> <span data-ttu-id="dd693-1115">Se essa avaliação causar uma exceção, nenhuma etapa adicional será executada.</span><span class="sxs-lookup"><span data-stu-id="dd693-1115">If this evaluation causes an exception, no further steps are executed.</span></span>
*  <span data-ttu-id="dd693-1116">As expressões de índice da *argument_list* são avaliadas na ordem, da esquerda para a direita.</span><span class="sxs-lookup"><span data-stu-id="dd693-1116">The index expressions of the *argument_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="dd693-1117">Após a avaliação de cada expressão de índice, uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)) para um dos seguintes tipos é executada: `int`, `uint`, `long``ulong`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1117">Following evaluation of each index expression, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to one of the following types is performed: `int`, `uint`, `long`, `ulong`.</span></span> <span data-ttu-id="dd693-1118">O primeiro tipo nesta lista para o qual existe uma conversão implícita é escolhido.</span><span class="sxs-lookup"><span data-stu-id="dd693-1118">The first type in this list for which an implicit conversion exists is chosen.</span></span> <span data-ttu-id="dd693-1119">Por exemplo, se a expressão de índice for do tipo `short`, uma conversão implícita em `int` será executada, já que conversões implícitas de `short` para `int` e de `short` para `long` são possíveis.</span><span class="sxs-lookup"><span data-stu-id="dd693-1119">For instance, if the index expression is of type `short` then an implicit conversion to `int` is performed, since implicit conversions from `short` to `int` and from `short` to `long` are possible.</span></span> <span data-ttu-id="dd693-1120">Se a avaliação de uma expressão de índice ou a conversão implícita subsequente causar uma exceção, não serão avaliadas outras expressões de índice e nenhuma etapa adicional será executada.</span><span class="sxs-lookup"><span data-stu-id="dd693-1120">If evaluation of an index expression or the subsequent implicit conversion causes an exception, then no further index expressions are evaluated and no further steps are executed.</span></span>
*  <span data-ttu-id="dd693-1121">O valor de `P` é verificado para ser válido.</span><span class="sxs-lookup"><span data-stu-id="dd693-1121">The value of `P` is checked to be valid.</span></span> <span data-ttu-id="dd693-1122">Se o valor de `P` for `null`, um `System.NullReferenceException` será lançado e nenhuma etapa adicional será executada.</span><span class="sxs-lookup"><span data-stu-id="dd693-1122">If the value of `P` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="dd693-1123">O valor de cada expressão na *argument_list* é verificado em relação aos limites reais de cada dimensão da instância de matriz referenciada por `P`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1123">The value of each expression in the *argument_list* is checked against the actual bounds of each dimension of the array instance referenced by `P`.</span></span> <span data-ttu-id="dd693-1124">Se um ou mais valores estiverem fora do intervalo, um `System.IndexOutOfRangeException` será lançado e nenhuma etapa adicional será executada.</span><span class="sxs-lookup"><span data-stu-id="dd693-1124">If one or more values are out of range, a `System.IndexOutOfRangeException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="dd693-1125">O local do elemento da matriz fornecido pelas expressões de índice é computado e esse local se torna o resultado do acesso à matriz.</span><span class="sxs-lookup"><span data-stu-id="dd693-1125">The location of the array element given by the index expression(s) is computed, and this location becomes the result of the array access.</span></span>

#### <a name="indexer-access"></a><span data-ttu-id="dd693-1126">Acesso de indexador</span><span class="sxs-lookup"><span data-stu-id="dd693-1126">Indexer access</span></span>

<span data-ttu-id="dd693-1127">Para um acesso de indexador, o *primary_no_array_creation_expression* do *element_access* deve ser uma variável ou um valor de uma classe, struct ou tipo de interface, e esse tipo deve implementar um ou mais indexadores que são aplicáveis em relação ao *argument_list* do *element_access*.</span><span class="sxs-lookup"><span data-stu-id="dd693-1127">For an indexer access, the *primary_no_array_creation_expression* of the *element_access* must be a variable or value of a class, struct, or interface type, and this type must implement one or more indexers that are applicable with respect to the *argument_list* of the *element_access*.</span></span>

<span data-ttu-id="dd693-1128">O processamento de tempo de vinculação de um acesso de indexador do formulário `P[A]`, em que `P` é uma *primary_no_array_creation_expression* de um tipo de classe, estrutura ou interface `T`e `A` é um *argument_list*, consiste nas seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="dd693-1128">The binding-time processing of an indexer access of the form `P[A]`, where `P` is a *primary_no_array_creation_expression* of a class, struct, or interface type `T`, and `A` is an *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="dd693-1129">O conjunto de indexadores fornecido pelo `T` é construído.</span><span class="sxs-lookup"><span data-stu-id="dd693-1129">The set of indexers provided by `T` is constructed.</span></span> <span data-ttu-id="dd693-1130">O conjunto consiste em todos os indexadores declarados em `T` ou em um tipo base de `T` que não são `override` declarações e estão acessíveis no contexto atual ([acesso de membro](basic-concepts.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="dd693-1130">The set consists of all indexers declared in `T` or a base type of `T` that are not `override` declarations and are accessible in the current context ([Member access](basic-concepts.md#member-access)).</span></span>
*  <span data-ttu-id="dd693-1131">O conjunto é reduzido para os indexadores aplicáveis e não ocultos por outros indexadores.</span><span class="sxs-lookup"><span data-stu-id="dd693-1131">The set is reduced to those indexers that are applicable and not hidden by other indexers.</span></span> <span data-ttu-id="dd693-1132">As regras a seguir são aplicadas a cada `S.I` do indexador no conjunto, em que `S` é o tipo no qual o indexador `I` é declarado:</span><span class="sxs-lookup"><span data-stu-id="dd693-1132">The following rules are applied to each indexer `S.I` in the set, where `S` is the type in which the indexer `I` is declared:</span></span>
   * <span data-ttu-id="dd693-1133">Se `I` não for aplicável em relação a `A` ([membro de função aplicável](expressions.md#applicable-function-member)), `I` será removido do conjunto.</span><span class="sxs-lookup"><span data-stu-id="dd693-1133">If `I` is not applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)), then `I` is removed from the set.</span></span>
   * <span data-ttu-id="dd693-1134">Se `I` for aplicável em relação ao `A` ([membro da função aplicável](expressions.md#applicable-function-member)), todos os indexadores declarados em um tipo base de `S` serão removidos do conjunto.</span><span class="sxs-lookup"><span data-stu-id="dd693-1134">If `I` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)), then all indexers declared in a base type of `S` are removed from the set.</span></span>
   * <span data-ttu-id="dd693-1135">Se `I` for aplicável em relação a `A` ([membro de função aplicável](expressions.md#applicable-function-member)) e `S` for um tipo de classe diferente de `object`, todos os indexadores declarados em uma interface serão removidos do conjunto.</span><span class="sxs-lookup"><span data-stu-id="dd693-1135">If `I` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)) and `S` is a class type other than `object`, all indexers declared in an interface are removed from the set.</span></span>
*  <span data-ttu-id="dd693-1136">Se o conjunto resultante de indexadores candidatos estiver vazio, não existirá nenhum indexador aplicável e ocorrerá um erro de tempo de ligação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1136">If the resulting set of candidate indexers is empty, then no applicable indexers exist, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="dd693-1137">O melhor indexador do conjunto de indexadores candidatos é identificado usando as regras de resolução de sobrecarga da [resolução de sobrecarga](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="dd693-1137">The best indexer of the set of candidate indexers is identified using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="dd693-1138">Se um único melhor indexador não puder ser identificado, o acesso ao indexador será ambíguo e ocorrerá um erro de tempo de ligação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1138">If a single best indexer cannot be identified, the indexer access is ambiguous, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="dd693-1139">As expressões de índice da *argument_list* são avaliadas na ordem, da esquerda para a direita.</span><span class="sxs-lookup"><span data-stu-id="dd693-1139">The index expressions of the *argument_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="dd693-1140">O resultado do processamento do acesso ao indexador é uma expressão classificada como um acesso de indexador.</span><span class="sxs-lookup"><span data-stu-id="dd693-1140">The result of processing the indexer access is an expression classified as an indexer access.</span></span> <span data-ttu-id="dd693-1141">A expressão de acesso do indexador faz referência ao indexador determinado na etapa acima e tem uma expressão de instância associada de `P` e uma lista de argumentos associados de `A`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1141">The indexer access expression references the indexer determined in the step above, and has an associated instance expression of `P` and an associated argument list of `A`.</span></span>

<span data-ttu-id="dd693-1142">Dependendo do contexto no qual ele é usado, um acesso ao indexador causa a invocação do *acessador get* ou do *acessador set* do indexador.</span><span class="sxs-lookup"><span data-stu-id="dd693-1142">Depending on the context in which it is used, an indexer access causes invocation of either the *get accessor* or the *set accessor* of the indexer.</span></span> <span data-ttu-id="dd693-1143">Se o acesso do indexador for o destino de uma atribuição, o *acessador set* será invocado para atribuir um novo valor ([atribuição simples](expressions.md#simple-assignment)).</span><span class="sxs-lookup"><span data-stu-id="dd693-1143">If the indexer access is the target of an assignment, the *set accessor* is invoked to assign a new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="dd693-1144">Em todos os outros casos, o *acessador get* é invocado para obter o valor atual ([valores de expressões](expressions.md#values-of-expressions)).</span><span class="sxs-lookup"><span data-stu-id="dd693-1144">In all other cases, the *get accessor* is invoked to obtain the current value ([Values of expressions](expressions.md#values-of-expressions)).</span></span>

### <a name="this-access"></a><span data-ttu-id="dd693-1145">Este acesso</span><span class="sxs-lookup"><span data-stu-id="dd693-1145">This access</span></span>

<span data-ttu-id="dd693-1146">Uma *this_access* consiste na palavra reservada `this`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1146">A *this_access* consists of the reserved word `this`.</span></span>

```antlr
this_access
    : 'this'
    ;
```

<span data-ttu-id="dd693-1147">Um *this_access* é permitido apenas no *bloco* de um construtor de instância, um método de instância ou um acessador de instância.</span><span class="sxs-lookup"><span data-stu-id="dd693-1147">A *this_access* is permitted only in the *block* of an instance constructor, an instance method, or an instance accessor.</span></span> <span data-ttu-id="dd693-1148">Ele tem um dos seguintes significados:</span><span class="sxs-lookup"><span data-stu-id="dd693-1148">It has one of the following meanings:</span></span>

*  <span data-ttu-id="dd693-1149">Quando `this` é usado em um *primary_expression* dentro de um construtor de instância de uma classe, ele é classificado como um valor.</span><span class="sxs-lookup"><span data-stu-id="dd693-1149">When `this` is used in a *primary_expression* within an instance constructor of a class, it is classified as a value.</span></span> <span data-ttu-id="dd693-1150">O tipo do valor é o tipo de instância ([o tipo de instância](classes.md#the-instance-type)) da classe dentro da qual o uso ocorre e o valor é uma referência ao objeto que está sendo construído.</span><span class="sxs-lookup"><span data-stu-id="dd693-1150">The type of the value is the instance type ([The instance type](classes.md#the-instance-type)) of the class within which the usage occurs, and the value is a reference to the object being constructed.</span></span>
*  <span data-ttu-id="dd693-1151">Quando `this` é usado em um *primary_expression* dentro de um método de instância ou acessador de instância de uma classe, ele é classificado como um valor.</span><span class="sxs-lookup"><span data-stu-id="dd693-1151">When `this` is used in a *primary_expression* within an instance method or instance accessor of a class, it is classified as a value.</span></span> <span data-ttu-id="dd693-1152">O tipo do valor é o tipo de instância ([o tipo de instância](classes.md#the-instance-type)) da classe dentro da qual o uso ocorre e o valor é uma referência ao objeto para o qual o método ou acessador foi invocado.</span><span class="sxs-lookup"><span data-stu-id="dd693-1152">The type of the value is the instance type ([The instance type](classes.md#the-instance-type)) of the class within which the usage occurs, and the value is a reference to the object for which the method or accessor was invoked.</span></span>
*  <span data-ttu-id="dd693-1153">Quando `this` é usado em um *primary_expression* dentro de um construtor de instância de uma struct, ele é classificado como uma variável.</span><span class="sxs-lookup"><span data-stu-id="dd693-1153">When `this` is used in a *primary_expression* within an instance constructor of a struct, it is classified as a variable.</span></span> <span data-ttu-id="dd693-1154">O tipo da variável é o tipo de instância ([o tipo de instância](classes.md#the-instance-type)) da estrutura dentro da qual o uso ocorre e a variável representa a estrutura que está sendo construída.</span><span class="sxs-lookup"><span data-stu-id="dd693-1154">The type of the variable is the instance type ([The instance type](classes.md#the-instance-type)) of the struct within which the usage occurs, and the variable represents the struct being constructed.</span></span> <span data-ttu-id="dd693-1155">A variável `this` de um construtor de instância de um struct se comporta exatamente como um parâmetro `out` do tipo struct — em particular, isso significa que a variável deve ser definitivamente atribuída em cada caminho de execução do construtor da instância.</span><span class="sxs-lookup"><span data-stu-id="dd693-1155">The `this` variable of an instance constructor of a struct behaves exactly the same as an `out` parameter of the struct type—in particular, this means that the variable must be definitely assigned in every execution path of the instance constructor.</span></span>
*  <span data-ttu-id="dd693-1156">Quando `this` é usado em um *primary_expression* dentro de um método de instância ou acessador de instância de uma struct, ele é classificado como uma variável.</span><span class="sxs-lookup"><span data-stu-id="dd693-1156">When `this` is used in a *primary_expression* within an instance method or instance accessor of a struct, it is classified as a variable.</span></span> <span data-ttu-id="dd693-1157">O tipo da variável é o tipo de instância ([o tipo de instância](classes.md#the-instance-type)) da estrutura na qual o uso ocorre.</span><span class="sxs-lookup"><span data-stu-id="dd693-1157">The type of the variable is the instance type ([The instance type](classes.md#the-instance-type)) of the struct within which the usage occurs.</span></span>
   * <span data-ttu-id="dd693-1158">Se o método ou acessador não for um iterador ([iteradores](classes.md#iterators)), a variável `this` representará a estrutura para a qual o método ou o acessador foi invocado e se comlatará exatamente como um parâmetro `ref` do tipo struct.</span><span class="sxs-lookup"><span data-stu-id="dd693-1158">If the method or accessor is not an iterator ([Iterators](classes.md#iterators)), the `this` variable represents the struct for which the method or accessor was invoked, and behaves exactly the same as a `ref` parameter of the struct type.</span></span>
   * <span data-ttu-id="dd693-1159">Se o método ou o acessador for um iterador, a variável `this` representa uma cópia da estrutura para a qual o método ou acessador foi invocado e se comporta exatamente como um parâmetro de valor do tipo struct.</span><span class="sxs-lookup"><span data-stu-id="dd693-1159">If the method or accessor is an iterator, the `this` variable represents a copy of the struct for which the method or accessor was invoked, and behaves exactly the same as a value parameter of the struct type.</span></span>

<span data-ttu-id="dd693-1160">O uso de `this` em um *primary_expression* em um contexto diferente daqueles listados acima é um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1160">Use of `this` in a *primary_expression* in a context other than the ones listed above is a compile-time error.</span></span> <span data-ttu-id="dd693-1161">Em particular, não é possível fazer referência a `this` em um método estático, um acessador de propriedade estática ou em uma *variable_initializer* de uma declaração de campo.</span><span class="sxs-lookup"><span data-stu-id="dd693-1161">In particular, it is not possible to refer to `this` in a static method, a static property accessor, or in a *variable_initializer* of a field declaration.</span></span>

### <a name="base-access"></a><span data-ttu-id="dd693-1162">Acesso de base</span><span class="sxs-lookup"><span data-stu-id="dd693-1162">Base access</span></span>

<span data-ttu-id="dd693-1163">Um *base_access* consiste na palavra reservada `base` seguida por um token "`.`" e um identificador ou um *argument_list* entre colchetes:</span><span class="sxs-lookup"><span data-stu-id="dd693-1163">A *base_access* consists of the reserved word `base` followed by either a "`.`" token and an identifier or an *argument_list* enclosed in square brackets:</span></span>

```antlr
base_access
    : 'base' '.' identifier
    | 'base' '[' expression_list ']'
    ;
```

<span data-ttu-id="dd693-1164">Uma *base_access* é usada para acessar membros da classe base que são ocultados por membros nomeados de forma semelhante na classe ou struct atual.</span><span class="sxs-lookup"><span data-stu-id="dd693-1164">A *base_access* is used to access base class members that are hidden by similarly named members in the current class or struct.</span></span> <span data-ttu-id="dd693-1165">Um *base_access* é permitido apenas no *bloco* de um construtor de instância, um método de instância ou um acessador de instância.</span><span class="sxs-lookup"><span data-stu-id="dd693-1165">A *base_access* is permitted only in the *block* of an instance constructor, an instance method, or an instance accessor.</span></span> <span data-ttu-id="dd693-1166">Quando `base.I` ocorre em uma classe ou struct, `I` deve indicar um membro da classe base dessa classe ou struct.</span><span class="sxs-lookup"><span data-stu-id="dd693-1166">When `base.I` occurs in a class or struct, `I` must denote a member of the base class of that class or struct.</span></span> <span data-ttu-id="dd693-1167">Da mesma forma, quando `base[E]` ocorre em uma classe, um indexador aplicável deve existir na classe base.</span><span class="sxs-lookup"><span data-stu-id="dd693-1167">Likewise, when `base[E]` occurs in a class, an applicable indexer must exist in the base class.</span></span>

<span data-ttu-id="dd693-1168">No momento da associação, *base_access* expressões do formulário `base.I` e `base[E]` são avaliadas exatamente como se fossem escritas `((B)this).I` e `((B)this)[E]`, em que `B` é a classe base da classe ou struct na qual a construção ocorre.</span><span class="sxs-lookup"><span data-stu-id="dd693-1168">At binding-time, *base_access* expressions of the form `base.I` and `base[E]` are evaluated exactly as if they were written `((B)this).I` and `((B)this)[E]`, where `B` is the base class of the class or struct in which the construct occurs.</span></span> <span data-ttu-id="dd693-1169">Assim, `base.I` e `base[E]` correspondem a `this.I` e `this[E]`, exceto `this` é exibido como uma instância da classe base.</span><span class="sxs-lookup"><span data-stu-id="dd693-1169">Thus, `base.I` and `base[E]` correspond to `this.I` and `this[E]`, except `this` is viewed as an instance of the base class.</span></span>

<span data-ttu-id="dd693-1170">Quando um *base_access* referencia um membro de função virtual (um método, uma propriedade ou um indexador), a determinação de qual membro de função invocar em tempo de execução ([verificação de tempo de compilação da resolução dinâmica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) é alterada.</span><span class="sxs-lookup"><span data-stu-id="dd693-1170">When a *base_access* references a virtual function member (a method, property, or indexer), the determination of which function member to invoke at run-time ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is changed.</span></span> <span data-ttu-id="dd693-1171">O membro da função que é invocado é determinado pela localização da implementação mais derivada ([métodos virtuais](classes.md#virtual-methods)) do membro da função em relação a `B` (em vez de em relação ao tipo de tempo de execução de `this`, como seria usual em um acesso não-base).</span><span class="sxs-lookup"><span data-stu-id="dd693-1171">The function member that is invoked is determined by finding the most derived implementation ([Virtual methods](classes.md#virtual-methods)) of the function member with respect to `B` (instead of with respect to the run-time type of `this`, as would be usual in a non-base access).</span></span> <span data-ttu-id="dd693-1172">Assim, dentro de um `override` de um membro de função `virtual`, um *base_access* pode ser usado para invocar a implementação herdada do membro da função.</span><span class="sxs-lookup"><span data-stu-id="dd693-1172">Thus, within an `override` of a `virtual` function member, a *base_access* can be used to invoke the inherited implementation of the function member.</span></span> <span data-ttu-id="dd693-1173">Se o membro da função referenciado por uma *base_access* for abstract, ocorrerá um erro de tempo de associação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1173">If the function member referenced by a *base_access* is abstract, a binding-time error occurs.</span></span>

### <a name="postfix-increment-and-decrement-operators"></a><span data-ttu-id="dd693-1174">Operadores de incremento e decremento pós-fixados</span><span class="sxs-lookup"><span data-stu-id="dd693-1174">Postfix increment and decrement operators</span></span>

```antlr
post_increment_expression
    : primary_expression '++'
    ;

post_decrement_expression
    : primary_expression '--'
    ;
```

<span data-ttu-id="dd693-1175">O operando de um incremento de sufixo ou uma operação de decréscimo deve ser uma expressão classificada como uma variável, um acesso de propriedade ou um acesso de indexador.</span><span class="sxs-lookup"><span data-stu-id="dd693-1175">The operand of a postfix increment or decrement operation must be an expression classified as a variable, a property access, or an indexer access.</span></span> <span data-ttu-id="dd693-1176">O resultado da operação é um valor do mesmo tipo que o operando.</span><span class="sxs-lookup"><span data-stu-id="dd693-1176">The result of the operation is a value of the same type as the operand.</span></span>

<span data-ttu-id="dd693-1177">Se o *primary_expression* tiver o tipo de tempo de compilação `dynamic`, o operador será vinculado dinamicamente ([associação dinâmica](expressions.md#dynamic-binding)), o *post_increment_expression* ou *post_decrement_expression* tem o tipo de tempo de compilação `dynamic` e as regras a seguir são aplicadas em tempo de execução usando o tipo de tempo de execução do *primary_expression*.</span><span class="sxs-lookup"><span data-stu-id="dd693-1177">If the *primary_expression* has the compile-time type `dynamic` then the operator is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), the *post_increment_expression* or *post_decrement_expression* has the compile-time type `dynamic` and the following rules are applied at run-time using the run-time type of the *primary_expression*.</span></span>

<span data-ttu-id="dd693-1178">Se o operando de um incremento de sufixo ou uma operação de decréscimo for um acesso de propriedade ou indexador, a propriedade ou o indexador deverá ter um `get` e um acessador `set`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1178">If the operand of a postfix increment or decrement operation is a property or indexer access, the property or indexer must have both a `get` and a `set` accessor.</span></span> <span data-ttu-id="dd693-1179">Se esse não for o caso, ocorrerá um erro de tempo de associação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1179">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="dd693-1180">Resolução de sobrecarga de operador unário ([resolução de sobrecarga de operador unário](expressions.md#unary-operator-overload-resolution)) é aplicada para selecionar uma implementação de operador específica.</span><span class="sxs-lookup"><span data-stu-id="dd693-1180">Unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="dd693-1181">Existem `++` predefinidos e operadores de `--` para os seguintes tipos: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`e qualquer tipo de enumeração.</span><span class="sxs-lookup"><span data-stu-id="dd693-1181">Predefined `++` and `--` operators exist for the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, and any enum type.</span></span> <span data-ttu-id="dd693-1182">Os operadores de `++` predefinidos retornam o valor produzido pela adição de 1 ao operando e os operadores de `--` predefinidos retornam o valor produzido pela subtração de 1 do operando.</span><span class="sxs-lookup"><span data-stu-id="dd693-1182">The predefined `++` operators return the value produced by adding 1 to the operand, and the predefined `--` operators return the value produced by subtracting 1 from the operand.</span></span> <span data-ttu-id="dd693-1183">Em um contexto `checked`, se o resultado dessa adição ou subtração estiver fora do intervalo do tipo de resultado e o tipo de resultado for um tipo integral ou tipo de enumeração, um `System.OverflowException` será gerado.</span><span class="sxs-lookup"><span data-stu-id="dd693-1183">In a `checked` context, if the result of this addition or subtraction is outside the range of the result type and the result type is an integral type or enum type, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="dd693-1184">O processamento em tempo de execução de um incremento de sufixo ou uma operação de diminuição do formulário `x++` ou `x--` consiste nas seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="dd693-1184">The run-time processing of a postfix increment or decrement operation of the form `x++` or `x--` consists of the following steps:</span></span>

*   <span data-ttu-id="dd693-1185">Se `x` for classificado como uma variável:</span><span class="sxs-lookup"><span data-stu-id="dd693-1185">If `x` is classified as a variable:</span></span>
    * <span data-ttu-id="dd693-1186">`x` é avaliado para produzir a variável.</span><span class="sxs-lookup"><span data-stu-id="dd693-1186">`x` is evaluated to produce the variable.</span></span>
    * <span data-ttu-id="dd693-1187">O valor de `x` é salvo.</span><span class="sxs-lookup"><span data-stu-id="dd693-1187">The value of `x` is saved.</span></span>
    * <span data-ttu-id="dd693-1188">O operador selecionado é invocado com o valor salvo de `x` como seu argumento.</span><span class="sxs-lookup"><span data-stu-id="dd693-1188">The selected operator is invoked with the saved value of `x` as its argument.</span></span>
    * <span data-ttu-id="dd693-1189">O valor retornado pelo operador é armazenado no local fornecido pela avaliação de `x`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1189">The value returned by the operator is stored in the location given by the evaluation of `x`.</span></span>
    * <span data-ttu-id="dd693-1190">O valor salvo de `x` se torna o resultado da operação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1190">The saved value of `x` becomes the result of the operation.</span></span>
*   <span data-ttu-id="dd693-1191">Se `x` for classificado como um acesso de propriedade ou indexador:</span><span class="sxs-lookup"><span data-stu-id="dd693-1191">If `x` is classified as a property or indexer access:</span></span>
    * <span data-ttu-id="dd693-1192">A expressão de instância (se `x` não for `static`) e a lista de argumentos (se `x` for um acesso de indexador) associada a `x` são avaliadas e os resultados são usados nas invocações subsequentes de `get` e `set`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1192">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `get` and `set` accessor invocations.</span></span>
    * <span data-ttu-id="dd693-1193">O acessador de `get` de `x` é invocado e o valor retornado é salvo.</span><span class="sxs-lookup"><span data-stu-id="dd693-1193">The `get` accessor of `x` is invoked and the returned value is saved.</span></span>
    * <span data-ttu-id="dd693-1194">O operador selecionado é invocado com o valor salvo de `x` como seu argumento.</span><span class="sxs-lookup"><span data-stu-id="dd693-1194">The selected operator is invoked with the saved value of `x` as its argument.</span></span>
    * <span data-ttu-id="dd693-1195">O acessador de `set` de `x` é invocado com o valor retornado pelo operador como seu argumento `value`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1195">The `set` accessor of `x` is invoked with the value returned by the operator as its `value` argument.</span></span>
    * <span data-ttu-id="dd693-1196">O valor salvo de `x` se torna o resultado da operação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1196">The saved value of `x` becomes the result of the operation.</span></span>

<span data-ttu-id="dd693-1197">Os operadores `++` e `--` também dão suporte à notação de prefixo ([operadores de incremento e diminuição de prefixo](expressions.md#prefix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="dd693-1197">The `++` and `--` operators also support prefix notation ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="dd693-1198">Normalmente, o resultado de `x++` ou `x--` é o valor de `x` antes da operação, enquanto o resultado de `++x` ou `--x` é o valor de `x` após a operação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1198">Typically, the result of `x++` or `x--` is the value of `x` before the operation, whereas the result of `++x` or `--x` is the value of `x` after the operation.</span></span> <span data-ttu-id="dd693-1199">Em ambos os casos, `x` ele mesmo tem o mesmo valor após a operação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1199">In either case, `x` itself has the same value after the operation.</span></span>

<span data-ttu-id="dd693-1200">Uma implementação `operator ++` ou `operator --` pode ser chamada usando o sufixo ou a notação de prefixo.</span><span class="sxs-lookup"><span data-stu-id="dd693-1200">An `operator ++` or `operator --` implementation can be invoked using either postfix or prefix notation.</span></span> <span data-ttu-id="dd693-1201">Não é possível ter implementações de operador separadas para as duas notações.</span><span class="sxs-lookup"><span data-stu-id="dd693-1201">It is not possible to have separate operator implementations for the two notations.</span></span>

### <a name="the-new-operator"></a><span data-ttu-id="dd693-1202">O novo operador</span><span class="sxs-lookup"><span data-stu-id="dd693-1202">The new operator</span></span>

<span data-ttu-id="dd693-1203">O operador `new` é usado para criar novas instâncias de tipos.</span><span class="sxs-lookup"><span data-stu-id="dd693-1203">The `new` operator is used to create new instances of types.</span></span>

<span data-ttu-id="dd693-1204">Há três formas de expressões de `new`:</span><span class="sxs-lookup"><span data-stu-id="dd693-1204">There are three forms of `new` expressions:</span></span>

*  <span data-ttu-id="dd693-1205">As expressões de criação de objeto são usadas para criar novas instâncias de tipos de classe e tipos de valor.</span><span class="sxs-lookup"><span data-stu-id="dd693-1205">Object creation expressions are used to create new instances of class types and value types.</span></span>
*  <span data-ttu-id="dd693-1206">As expressões de criação de matriz são usadas para criar novas instâncias de tipos de matriz.</span><span class="sxs-lookup"><span data-stu-id="dd693-1206">Array creation expressions are used to create new instances of array types.</span></span>
*  <span data-ttu-id="dd693-1207">As expressões de criação de representante são usadas para criar novas instâncias de tipos delegados.</span><span class="sxs-lookup"><span data-stu-id="dd693-1207">Delegate creation expressions are used to create new instances of delegate types.</span></span>

<span data-ttu-id="dd693-1208">O operador `new` implica a criação de uma instância de um tipo, mas não implica necessariamente a alocação dinâmica da memória.</span><span class="sxs-lookup"><span data-stu-id="dd693-1208">The `new` operator implies creation of an instance of a type, but does not necessarily imply dynamic allocation of memory.</span></span> <span data-ttu-id="dd693-1209">Em particular, as instâncias de tipos de valor não exigem memória adicional além das variáveis em que residem, e nenhuma alocação dinâmica ocorre quando `new` é usada para criar instâncias de tipos de valor.</span><span class="sxs-lookup"><span data-stu-id="dd693-1209">In particular, instances of value types require no additional memory beyond the variables in which they reside, and no dynamic allocations occur when `new` is used to create instances of value types.</span></span>

#### <a name="object-creation-expressions"></a><span data-ttu-id="dd693-1210">Expressões de criação de objeto</span><span class="sxs-lookup"><span data-stu-id="dd693-1210">Object creation expressions</span></span>

<span data-ttu-id="dd693-1211">Um *object_creation_expression* é usado para criar uma nova instância de um *class_type* ou um *value_type*.</span><span class="sxs-lookup"><span data-stu-id="dd693-1211">An *object_creation_expression* is used to create a new instance of a *class_type* or a *value_type*.</span></span>

```antlr
object_creation_expression
    : 'new' type '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;

object_or_collection_initializer
    : object_initializer
    | collection_initializer
    ;
```

<span data-ttu-id="dd693-1212">O *tipo* de um *object_creation_expression* deve ser um *class_type*, um *value_type* ou um *type_parameter*.</span><span class="sxs-lookup"><span data-stu-id="dd693-1212">The *type* of an *object_creation_expression* must be a *class_type*, a *value_type* or a *type_parameter*.</span></span> <span data-ttu-id="dd693-1213">O *tipo* não pode ser um `abstract` *class_type*.</span><span class="sxs-lookup"><span data-stu-id="dd693-1213">The *type* cannot be an `abstract` *class_type*.</span></span>

<span data-ttu-id="dd693-1214">O *argument_list* opcional ([listas de argumentos](expressions.md#argument-lists)) só será permitido se o *tipo* for um *class_type* ou um *struct_type*.</span><span class="sxs-lookup"><span data-stu-id="dd693-1214">The optional *argument_list* ([Argument lists](expressions.md#argument-lists)) is permitted only if the *type* is a *class_type* or a *struct_type*.</span></span>

<span data-ttu-id="dd693-1215">Uma expressão de criação de objeto pode omitir a lista de argumentos do construtor e os parênteses delimitadores fornecidos incluem um inicializador de objeto ou inicializador de coleção.</span><span class="sxs-lookup"><span data-stu-id="dd693-1215">An object creation expression can omit the constructor argument list and enclosing parentheses provided it includes an object initializer or collection initializer.</span></span> <span data-ttu-id="dd693-1216">Omitir a lista de argumentos do construtor e parênteses delimitadores é equivalente a especificar uma lista de argumentos vazia.</span><span class="sxs-lookup"><span data-stu-id="dd693-1216">Omitting the constructor argument list and enclosing parentheses is equivalent to specifying an empty argument list.</span></span>

<span data-ttu-id="dd693-1217">O processamento de uma expressão de criação de objeto que inclui um inicializador de objeto ou inicializador de coleção consiste no primeiro processamento do construtor de instância e no processamento das inicializações de membro ou elemento especificadas pelo inicializador de objeto ([inicializadores de objeto](expressions.md#object-initializers)) ou pelo inicializador de coleção ([inicializadores de coleção](expressions.md#collection-initializers)).</span><span class="sxs-lookup"><span data-stu-id="dd693-1217">Processing of an object creation expression that includes an object initializer or collection initializer consists of first processing the instance constructor and then processing the member or element initializations specified by the object initializer ([Object initializers](expressions.md#object-initializers)) or collection initializer ([Collection initializers](expressions.md#collection-initializers)).</span></span>

<span data-ttu-id="dd693-1218">Se qualquer um dos argumentos na *argument_list* opcional tiver o tipo de tempo de compilação `dynamic`, a *object_creation_expression* será vinculada dinamicamente ([associação dinâmica](expressions.md#dynamic-binding)) e as regras a seguir serão aplicadas em tempo de execução usando o tipo de tempo de execução desses argumentos da *argument_list* que têm o tipo de tempo de compilação `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1218">If any of the arguments in the optional *argument_list* has the compile-time type `dynamic` then the *object_creation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) and the following rules are applied at run-time using the run-time type of those arguments of the *argument_list* that have the compile time type `dynamic`.</span></span> <span data-ttu-id="dd693-1219">No entanto, a criação do objeto passa por uma verificação de tempo de compilação limitada, conforme descrito em [verificação de tempo de compilação da resolução dinâmica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="dd693-1219">However, the object creation undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="dd693-1220">O processamento de tempo de associação de um *object_creation_expression* do formulário `new T(A)`, em que `T` é um *class_type* ou um *value_type* e `A` é um *argument_list*opcional, consiste nas seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="dd693-1220">The binding-time processing of an *object_creation_expression* of the form `new T(A)`, where `T` is a *class_type* or a *value_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*   <span data-ttu-id="dd693-1221">Se `T` for um *value_type* e `A` não estiver presente:</span><span class="sxs-lookup"><span data-stu-id="dd693-1221">If `T` is a *value_type* and `A` is not present:</span></span>
    * <span data-ttu-id="dd693-1222">O *object_creation_expression* é uma invocação de construtor padrão.</span><span class="sxs-lookup"><span data-stu-id="dd693-1222">The *object_creation_expression* is a default constructor invocation.</span></span> <span data-ttu-id="dd693-1223">O resultado da *object_creation_expression* é um valor do tipo `T`, ou seja, o valor padrão para `T` conforme definido no [tipo System. ValueType](types.md#the-systemvaluetype-type).</span><span class="sxs-lookup"><span data-stu-id="dd693-1223">The result of the *object_creation_expression* is a value of type `T`, namely the default value for `T` as defined in [The System.ValueType type](types.md#the-systemvaluetype-type).</span></span>
*   <span data-ttu-id="dd693-1224">Caso contrário, se `T` for um *type_parameter* e `A` não estiver presente:</span><span class="sxs-lookup"><span data-stu-id="dd693-1224">Otherwise, if `T` is a *type_parameter* and `A` is not present:</span></span>
    * <span data-ttu-id="dd693-1225">Se nenhuma restrição de tipo de valor ou restrição de Construtor ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)) tiver sido especificada para `T`, ocorrerá um erro de tempo de associação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1225">If no value type constraint or constructor constraint ([Type parameter constraints](classes.md#type-parameter-constraints)) has been specified for `T`, a binding-time error occurs.</span></span>
    * <span data-ttu-id="dd693-1226">O resultado da *object_creation_expression* é um valor do tipo de tempo de execução ao qual o parâmetro de tipo foi associado, ou seja, o resultado da invocação do construtor padrão desse tipo.</span><span class="sxs-lookup"><span data-stu-id="dd693-1226">The result of the *object_creation_expression* is a value of the run-time type that the type parameter has been bound to, namely the result of invoking the default constructor of that type.</span></span> <span data-ttu-id="dd693-1227">O tipo de tempo de execução pode ser um tipo de referência ou um tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="dd693-1227">The run-time type may be a reference type or a value type.</span></span>
*   <span data-ttu-id="dd693-1228">Caso contrário, se `T` for uma *class_type* ou uma *struct_type*:</span><span class="sxs-lookup"><span data-stu-id="dd693-1228">Otherwise, if `T` is a *class_type* or a *struct_type*:</span></span>
    * <span data-ttu-id="dd693-1229">Se `T` for um `abstract` *class_type*, ocorrerá um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1229">If `T` is an `abstract` *class_type*, a compile-time error occurs.</span></span>
    * <span data-ttu-id="dd693-1230">O construtor de instância a ser invocado é determinado usando as regras de resolução de sobrecarga da [resolução de sobrecarga](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="dd693-1230">The instance constructor to invoke is determined using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="dd693-1231">O conjunto de construtores de instância de candidato consiste em todos os construtores de instância acessíveis declarados em `T` que são aplicáveis em relação a `A` ([membro de função aplicável](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="dd693-1231">The set of candidate instance constructors consists of all accessible instance constructors declared in `T` which are applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span> <span data-ttu-id="dd693-1232">Se o conjunto de construtores de instância de candidato estiver vazio, ou se um único Construtor de instância recomendada não puder ser identificado, ocorrerá um erro de tempo de associação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1232">If the set of candidate instance constructors is empty, or if a single best instance constructor cannot be identified, a binding-time error occurs.</span></span>
    * <span data-ttu-id="dd693-1233">O resultado da *object_creation_expression* é um valor do tipo `T`, ou seja, o valor produzido por invocar o construtor da instância determinado na etapa acima.</span><span class="sxs-lookup"><span data-stu-id="dd693-1233">The result of the *object_creation_expression* is a value of type `T`, namely the value produced by invoking the instance constructor determined in the step above.</span></span>
*  <span data-ttu-id="dd693-1234">Caso contrário, o *object_creation_expression* é inválido e ocorre um erro de tempo de ligação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1234">Otherwise, the *object_creation_expression* is invalid, and a binding-time error occurs.</span></span>

<span data-ttu-id="dd693-1235">Mesmo que a *object_creation_expression* seja vinculada dinamicamente, o tipo de tempo de compilação ainda será `T`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1235">Even if the *object_creation_expression* is dynamically bound, the compile-time type is still `T`.</span></span>

<span data-ttu-id="dd693-1236">O processamento em tempo de execução de um *object_creation_expression* do formulário `new T(A)`, em que `T` é *class_type* ou uma *struct_type* e `A` é uma *argument_list*opcional, consiste nas seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="dd693-1236">The run-time processing of an *object_creation_expression* of the form `new T(A)`, where `T` is *class_type* or a *struct_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*   <span data-ttu-id="dd693-1237">Se `T` for uma *class_type*:</span><span class="sxs-lookup"><span data-stu-id="dd693-1237">If `T` is a *class_type*:</span></span>
    * <span data-ttu-id="dd693-1238">Uma nova instância da classe `T` é alocada.</span><span class="sxs-lookup"><span data-stu-id="dd693-1238">A new instance of class `T` is allocated.</span></span> <span data-ttu-id="dd693-1239">Se não houver memória suficiente disponível para alocar a nova instância, um `System.OutOfMemoryException` será lançado e nenhuma etapa adicional será executada.</span><span class="sxs-lookup"><span data-stu-id="dd693-1239">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="dd693-1240">Todos os campos da nova instância são inicializados para seus valores padrão ([valores padrão](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="dd693-1240">All fields of the new instance are initialized to their default values ([Default values](variables.md#default-values)).</span></span>
    * <span data-ttu-id="dd693-1241">O construtor de instância é invocado de acordo com as regras de invocação de membro de função ([verificação de tempo de compilação da resolução dinâmica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="dd693-1241">The instance constructor is invoked according to the rules of function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span> <span data-ttu-id="dd693-1242">Uma referência à instância alocada recentemente é passada automaticamente para o construtor de instância e a instância pode ser acessada de dentro desse construtor como `this`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1242">A reference to the newly allocated instance is automatically passed to the instance constructor and the instance can be accessed from within that constructor as `this`.</span></span>
*   <span data-ttu-id="dd693-1243">Se `T` for uma *struct_type*:</span><span class="sxs-lookup"><span data-stu-id="dd693-1243">If `T` is a *struct_type*:</span></span>
    * <span data-ttu-id="dd693-1244">Uma instância do tipo `T` é criada alocando-se uma variável local temporária.</span><span class="sxs-lookup"><span data-stu-id="dd693-1244">An instance of type `T` is created by allocating a temporary local variable.</span></span> <span data-ttu-id="dd693-1245">Como um construtor de instância de um *struct_type* é necessário para atribuir definitivamente um valor a cada campo da instância que está sendo criada, nenhuma inicialização da variável temporária é necessária.</span><span class="sxs-lookup"><span data-stu-id="dd693-1245">Since an instance constructor of a *struct_type* is required to definitely assign a value to each field of the instance being created, no initialization of the temporary variable is necessary.</span></span>
    * <span data-ttu-id="dd693-1246">O construtor de instância é invocado de acordo com as regras de invocação de membro de função ([verificação de tempo de compilação da resolução dinâmica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="dd693-1246">The instance constructor is invoked according to the rules of function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span> <span data-ttu-id="dd693-1247">Uma referência à instância alocada recentemente é passada automaticamente para o construtor de instância e a instância pode ser acessada de dentro desse construtor como `this`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1247">A reference to the newly allocated instance is automatically passed to the instance constructor and the instance can be accessed from within that constructor as `this`.</span></span>

#### <a name="object-initializers"></a><span data-ttu-id="dd693-1248">Inicializadores de objeto</span><span class="sxs-lookup"><span data-stu-id="dd693-1248">Object initializers</span></span>

<span data-ttu-id="dd693-1249">Um ***inicializador de objeto*** especifica valores para zero ou mais campos, propriedades ou elementos indexados de um objeto.</span><span class="sxs-lookup"><span data-stu-id="dd693-1249">An ***object initializer*** specifies values for zero or more fields, properties or indexed elements of an object.</span></span>

```antlr
object_initializer
    : '{' member_initializer_list? '}'
    | '{' member_initializer_list ',' '}'
    ;

member_initializer_list
    : member_initializer (',' member_initializer)*
    ;

member_initializer
    : initializer_target '=' initializer_value
    ;

initializer_target
    : identifier
    | '[' argument_list ']'
    ;

initializer_value
    : expression
    | object_or_collection_initializer
    ;
```

<span data-ttu-id="dd693-1250">Um inicializador de objeto consiste em uma sequência de inicializadores de membro, delimitados por `{` e `}` tokens e separados por vírgulas.</span><span class="sxs-lookup"><span data-stu-id="dd693-1250">An object initializer consists of a sequence of member initializers, enclosed by `{` and `}` tokens and separated by commas.</span></span> <span data-ttu-id="dd693-1251">Cada *member_initializer* designa um destino para a inicialização.</span><span class="sxs-lookup"><span data-stu-id="dd693-1251">Each *member_initializer* designates a target for the initialization.</span></span> <span data-ttu-id="dd693-1252">Um *identificador* deve nomear um campo ou Propriedade acessível do objeto que está sendo inicializado, enquanto um *argument_list* entre colchetes deve especificar argumentos para um indexador acessível no objeto que está sendo inicializado.</span><span class="sxs-lookup"><span data-stu-id="dd693-1252">An *identifier* must name an accessible field or property of the object being initialized, whereas an *argument_list* enclosed in square brackets must specify arguments for an accessible indexer on the object being initialized.</span></span> <span data-ttu-id="dd693-1253">É um erro para um inicializador de objeto incluir mais de um inicializador de membro para o mesmo campo ou propriedade.</span><span class="sxs-lookup"><span data-stu-id="dd693-1253">It is an error for an object initializer to include more than one member initializer for the same field or property.</span></span>

<span data-ttu-id="dd693-1254">Cada *initializer_target* é seguida por um sinal de igual e uma expressão, um inicializador de objeto ou um inicializador de coleção.</span><span class="sxs-lookup"><span data-stu-id="dd693-1254">Each *initializer_target* is followed by an equals sign and either an expression, an object initializer or a collection initializer.</span></span> <span data-ttu-id="dd693-1255">Não é possível que as expressões no inicializador de objeto façam referência ao objeto recém-criado que está sendo inicializado.</span><span class="sxs-lookup"><span data-stu-id="dd693-1255">It is not possible for expressions within the object initializer to refer to the newly created object it is initializing.</span></span>

<span data-ttu-id="dd693-1256">Um inicializador de membro que especifica uma expressão após o sinal de igual é processado da mesma maneira que uma atribuição ([atribuição simples](expressions.md#simple-assignment)) para o destino.</span><span class="sxs-lookup"><span data-stu-id="dd693-1256">A member initializer that specifies an expression after the equals sign is processed in the same way as an assignment ([Simple assignment](expressions.md#simple-assignment)) to the target.</span></span>

<span data-ttu-id="dd693-1257">Um inicializador de membro que especifica um inicializador de objeto após o sinal de igual é um ***inicializador de objeto aninhado***, ou seja, uma inicialização de um objeto inserido.</span><span class="sxs-lookup"><span data-stu-id="dd693-1257">A member initializer that specifies an object initializer after the equals sign is a ***nested object initializer***, i.e. an initialization of an embedded object.</span></span> <span data-ttu-id="dd693-1258">Em vez de atribuir um novo valor ao campo ou à propriedade, as atribuições no inicializador de objeto aninhado são tratadas como atribuições para membros do campo ou da propriedade.</span><span class="sxs-lookup"><span data-stu-id="dd693-1258">Instead of assigning a new value to the field or property, the assignments in the nested object initializer are treated as assignments to members of the field or property.</span></span> <span data-ttu-id="dd693-1259">Inicializadores de objeto aninhados não podem ser aplicados a propriedades com um tipo de valor, ou a campos somente leitura com um tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="dd693-1259">Nested object initializers cannot be applied to properties with a value type, or to read-only fields with a value type.</span></span>

<span data-ttu-id="dd693-1260">Um inicializador de membro que especifica um inicializador de coleção após o sinal de igual é uma inicialização de uma coleção inserida.</span><span class="sxs-lookup"><span data-stu-id="dd693-1260">A member initializer that specifies a collection initializer after the equals sign is an initialization of an embedded collection.</span></span> <span data-ttu-id="dd693-1261">Em vez de atribuir uma nova coleção ao campo de destino, à propriedade ou ao indexador, os elementos fornecidos no inicializador são adicionados à coleção referenciada pelo destino.</span><span class="sxs-lookup"><span data-stu-id="dd693-1261">Instead of assigning a new collection to the target field, property or indexer, the elements given in the initializer are added to the collection referenced by the target.</span></span> <span data-ttu-id="dd693-1262">O destino deve ser de um tipo de coleção que satisfaça os requisitos especificados em [inicializadores de coleção](expressions.md#collection-initializers).</span><span class="sxs-lookup"><span data-stu-id="dd693-1262">The target must be of a collection type that satisfies the requirements specified in [Collection initializers](expressions.md#collection-initializers).</span></span>

<span data-ttu-id="dd693-1263">Os argumentos para um inicializador de índice sempre serão avaliados exatamente uma vez.</span><span class="sxs-lookup"><span data-stu-id="dd693-1263">The arguments to an index initializer will always be evaluated exactly once.</span></span> <span data-ttu-id="dd693-1264">Portanto, mesmo se os argumentos acabarem nunca sendo usados (por exemplo, devido a um inicializador aninhado vazio), eles serão avaliados para seus efeitos colaterais.</span><span class="sxs-lookup"><span data-stu-id="dd693-1264">Thus, even if the arguments end up never getting used (e.g. because of an empty nested initializer), they will be evaluated for their side effects.</span></span>

<span data-ttu-id="dd693-1265">A classe a seguir representa um ponto com duas coordenadas:</span><span class="sxs-lookup"><span data-stu-id="dd693-1265">The following class represents a point with two coordinates:</span></span>
```csharp
public class Point
{
    int x, y;

    public int X { get { return x; } set { x = value; } }
    public int Y { get { return y; } set { y = value; } }
}
```

<span data-ttu-id="dd693-1266">Uma instância do `Point` pode ser criada e inicializada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="dd693-1266">An instance of `Point` can be created and initialized as follows:</span></span>
```csharp
Point a = new Point { X = 0, Y = 1 };
```
<span data-ttu-id="dd693-1267">que tem o mesmo efeito que</span><span class="sxs-lookup"><span data-stu-id="dd693-1267">which has the same effect as</span></span>
```csharp
Point __a = new Point();
__a.X = 0;
__a.Y = 1; 
Point a = __a;
```
<span data-ttu-id="dd693-1268">em que `__a` é uma variável temporária invisível e inacessível.</span><span class="sxs-lookup"><span data-stu-id="dd693-1268">where `__a` is an otherwise invisible and inaccessible temporary variable.</span></span> <span data-ttu-id="dd693-1269">A classe a seguir representa um retângulo criado a partir de dois pontos:</span><span class="sxs-lookup"><span data-stu-id="dd693-1269">The following class represents a rectangle created from two points:</span></span>
```csharp
public class Rectangle
{
    Point p1, p2;

    public Point P1 { get { return p1; } set { p1 = value; } }
    public Point P2 { get { return p2; } set { p2 = value; } }
}
```

<span data-ttu-id="dd693-1270">Uma instância do `Rectangle` pode ser criada e inicializada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="dd693-1270">An instance of `Rectangle` can be created and initialized as follows:</span></span>
```csharp
Rectangle r = new Rectangle {
    P1 = new Point { X = 0, Y = 1 },
    P2 = new Point { X = 2, Y = 3 }
};
```
<span data-ttu-id="dd693-1271">que tem o mesmo efeito que</span><span class="sxs-lookup"><span data-stu-id="dd693-1271">which has the same effect as</span></span>
```csharp
Rectangle __r = new Rectangle();
Point __p1 = new Point();
__p1.X = 0;
__p1.Y = 1;
__r.P1 = __p1;
Point __p2 = new Point();
__p2.X = 2;
__p2.Y = 3;
__r.P2 = __p2; 
Rectangle r = __r;
```
<span data-ttu-id="dd693-1272">onde `__r`, `__p1` e `__p2` são variáveis temporárias que, de outra forma, são invisíveis e inacessíveis.</span><span class="sxs-lookup"><span data-stu-id="dd693-1272">where `__r`, `__p1` and `__p2` are temporary variables that are otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="dd693-1273">Se o construtor de `Rectangle`alocar as duas instâncias de `Point` inseridas</span><span class="sxs-lookup"><span data-stu-id="dd693-1273">If `Rectangle`'s constructor allocates the two embedded `Point` instances</span></span>
```csharp
public class Rectangle
{
    Point p1 = new Point();
    Point p2 = new Point();

    public Point P1 { get { return p1; } }
    public Point P2 { get { return p2; } }
}
```
<span data-ttu-id="dd693-1274">a construção a seguir pode ser usada para inicializar as instâncias de `Point` inseridas em vez de atribuir novas instâncias:</span><span class="sxs-lookup"><span data-stu-id="dd693-1274">the following construct can be used to initialize the embedded `Point` instances instead of assigning new instances:</span></span>
```csharp
Rectangle r = new Rectangle {
    P1 = { X = 0, Y = 1 },
    P2 = { X = 2, Y = 3 }
};
```
<span data-ttu-id="dd693-1275">que tem o mesmo efeito que</span><span class="sxs-lookup"><span data-stu-id="dd693-1275">which has the same effect as</span></span>
```csharp
Rectangle __r = new Rectangle();
__r.P1.X = 0;
__r.P1.Y = 1;
__r.P2.X = 2;
__r.P2.Y = 3;
Rectangle r = __r;
```

<span data-ttu-id="dd693-1276">Dada uma definição apropriada de C, o exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="dd693-1276">Given an appropriate definition of C, the following example:</span></span>
```csharp
var c = new C {
    x = true,
    y = { a = "Hello" },
    z = { 1, 2, 3 },
    ["x"] = 5,
    [0,0] = { "a", "b" },
    [1,2] = {}
};
```
<span data-ttu-id="dd693-1277">é equivalente a esta série de atribuições:</span><span class="sxs-lookup"><span data-stu-id="dd693-1277">is equivalent to this series of assignments:</span></span>
```csharp
C __c = new C();
__c.x = true;
__c.y.a = "Hello";
__c.z.Add(1); 
__c.z.Add(2);
__c.z.Add(3);
string __i1 = "x";
__c[__i1] = 5;
int __i2 = 0, __i3 = 0;
__c[__i2,__i3].Add("a");
__c[__i2,__i3].Add("b");
int __i4 = 1, __i5 = 2;
var c = __c;
```
<span data-ttu-id="dd693-1278">onde `__c`, etc., são variáveis geradas que são invisíveis e inacessíveis para o código-fonte.</span><span class="sxs-lookup"><span data-stu-id="dd693-1278">where `__c`, etc., are generated variables that are invisible and inaccessible to the source code.</span></span> <span data-ttu-id="dd693-1279">Observe que os argumentos para `[0,0]` são avaliados apenas uma vez e os argumentos para `[1,2]` são avaliados uma vez, mesmo que nunca sejam usados.</span><span class="sxs-lookup"><span data-stu-id="dd693-1279">Note that the arguments for `[0,0]` are evaluated only once, and the arguments for `[1,2]` are evaluated once even though they are never used.</span></span>

#### <a name="collection-initializers"></a><span data-ttu-id="dd693-1280">Inicializadores de coleção</span><span class="sxs-lookup"><span data-stu-id="dd693-1280">Collection initializers</span></span>

<span data-ttu-id="dd693-1281">Um inicializador de coleção especifica os elementos de uma coleção.</span><span class="sxs-lookup"><span data-stu-id="dd693-1281">A collection initializer specifies the elements of a collection.</span></span>

```antlr
collection_initializer
    : '{' element_initializer_list '}'
    | '{' element_initializer_list ',' '}'
    ;

element_initializer_list
    : element_initializer (',' element_initializer)*
    ;

element_initializer
    : non_assignment_expression
    | '{' expression_list '}'
    ;

expression_list
    : expression (',' expression)*
    ;
```

<span data-ttu-id="dd693-1282">Um inicializador de coleção consiste em uma sequência de inicializadores de elemento, delimitada por `{` e `}` tokens e separados por vírgulas.</span><span class="sxs-lookup"><span data-stu-id="dd693-1282">A collection initializer consists of a sequence of element initializers, enclosed by `{` and `}` tokens and separated by commas.</span></span> <span data-ttu-id="dd693-1283">Cada inicializador de elemento especifica um elemento a ser adicionado ao objeto de coleção que está sendo inicializado e consiste em uma lista de expressões delimitadas por `{` e `}` tokens e separados por vírgulas.</span><span class="sxs-lookup"><span data-stu-id="dd693-1283">Each element initializer specifies an element to be added to the collection object being initialized, and consists of a list of expressions enclosed by `{` and `}` tokens and separated by commas.</span></span>  <span data-ttu-id="dd693-1284">Um inicializador de elemento de uma única expressão pode ser gravado sem chaves, mas não pode ser uma expressão de atribuição para evitar ambigüidade com inicializadores de membro.</span><span class="sxs-lookup"><span data-stu-id="dd693-1284">A single-expression element initializer can be written without braces, but cannot then be an assignment expression, to avoid ambiguity with member initializers.</span></span> <span data-ttu-id="dd693-1285">A produção *non_assignment_expression* é definida na [expressão](expressions.md#expression).</span><span class="sxs-lookup"><span data-stu-id="dd693-1285">The *non_assignment_expression* production is defined in [Expression](expressions.md#expression).</span></span>

<span data-ttu-id="dd693-1286">Veja a seguir um exemplo de uma expressão de criação de objeto que inclui um inicializador de coleção:</span><span class="sxs-lookup"><span data-stu-id="dd693-1286">The following is an example of an object creation expression that includes a collection initializer:</span></span>
```csharp
List<int> digits = new List<int> { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
```

<span data-ttu-id="dd693-1287">O objeto de coleção ao qual um inicializador de coleção é aplicado deve ser de um tipo que implementa `System.Collections.IEnumerable` ou um erro em tempo de compilação ocorre.</span><span class="sxs-lookup"><span data-stu-id="dd693-1287">The collection object to which a collection initializer is applied must be of a type that implements `System.Collections.IEnumerable` or a compile-time error occurs.</span></span> <span data-ttu-id="dd693-1288">Para cada elemento especificado na ordem, o inicializador de coleção invoca um método `Add` no objeto de destino com a lista de expressões do inicializador de elemento como lista de argumentos, aplicando a resolução de membro normal e a solução de sobrecarga para cada invocação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1288">For each specified element in order, the collection initializer invokes an `Add` method on the target object with the expression list of the element initializer as argument list, applying normal member lookup and overload resolution for each invocation.</span></span> <span data-ttu-id="dd693-1289">Assim, o objeto de coleção deve ter uma instância aplicável ou um método de extensão com o nome `Add` para cada inicializador de elemento.</span><span class="sxs-lookup"><span data-stu-id="dd693-1289">Thus, the collection object must have an applicable instance or extension method with the name `Add` for each element initializer.</span></span>

<span data-ttu-id="dd693-1290">A classe a seguir representa um contato com um nome e uma lista de números de telefone:</span><span class="sxs-lookup"><span data-stu-id="dd693-1290">The following class represents a contact with a name and a list of phone numbers:</span></span>
```csharp
public class Contact
{
    string name;
    List<string> phoneNumbers = new List<string>();

    public string Name { get { return name; } set { name = value; } }

    public List<string> PhoneNumbers { get { return phoneNumbers; } }
}
```

<span data-ttu-id="dd693-1291">Um `List<Contact>` pode ser criado e inicializado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="dd693-1291">A `List<Contact>` can be created and initialized as follows:</span></span>
```csharp
var contacts = new List<Contact> {
    new Contact {
        Name = "Chris Smith",
        PhoneNumbers = { "206-555-0101", "425-882-8080" }
    },
    new Contact {
        Name = "Bob Harris",
        PhoneNumbers = { "650-555-0199" }
    }
};
```
<span data-ttu-id="dd693-1292">que tem o mesmo efeito que</span><span class="sxs-lookup"><span data-stu-id="dd693-1292">which has the same effect as</span></span>
```csharp
var __clist = new List<Contact>();
Contact __c1 = new Contact();
__c1.Name = "Chris Smith";
__c1.PhoneNumbers.Add("206-555-0101");
__c1.PhoneNumbers.Add("425-882-8080");
__clist.Add(__c1);
Contact __c2 = new Contact();
__c2.Name = "Bob Harris";
__c2.PhoneNumbers.Add("650-555-0199");
__clist.Add(__c2);
var contacts = __clist;
```
<span data-ttu-id="dd693-1293">onde `__clist`, `__c1` e `__c2` são variáveis temporárias que, de outra forma, são invisíveis e inacessíveis.</span><span class="sxs-lookup"><span data-stu-id="dd693-1293">where `__clist`, `__c1` and `__c2` are temporary variables that are otherwise invisible and inaccessible.</span></span>

#### <a name="array-creation-expressions"></a><span data-ttu-id="dd693-1294">Expressões de criação de matriz</span><span class="sxs-lookup"><span data-stu-id="dd693-1294">Array creation expressions</span></span>

<span data-ttu-id="dd693-1295">Um *array_creation_expression* é usado para criar uma nova instância de um *array_type*.</span><span class="sxs-lookup"><span data-stu-id="dd693-1295">An *array_creation_expression* is used to create a new instance of an *array_type*.</span></span>

```antlr
array_creation_expression
    : 'new' non_array_type '[' expression_list ']' rank_specifier* array_initializer?
    | 'new' array_type array_initializer
    | 'new' rank_specifier array_initializer
    ;
```

<span data-ttu-id="dd693-1296">Uma expressão de criação de matriz do primeiro formulário aloca uma instância de matriz do tipo que resulta da exclusão de cada uma das expressões individuais da lista de expressões.</span><span class="sxs-lookup"><span data-stu-id="dd693-1296">An array creation expression of the first form allocates an array instance of the type that results from deleting each of the individual expressions from the expression list.</span></span> <span data-ttu-id="dd693-1297">Por exemplo, a expressão de criação de matriz `new int[10,20]` produz uma instância de matriz do tipo `int[,]`e a expressão de criação de matriz `new int[10][,]` produz uma matriz do tipo `int[][,]`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1297">For example, the array creation expression `new int[10,20]` produces an array instance of type `int[,]`, and the array creation expression `new int[10][,]` produces an array of type `int[][,]`.</span></span> <span data-ttu-id="dd693-1298">Cada expressão na lista de expressões deve ser do tipo `int`, `uint`, `long`ou `ulong`ou conversível implicitamente em um ou mais desses tipos.</span><span class="sxs-lookup"><span data-stu-id="dd693-1298">Each expression in the expression list must be of type `int`, `uint`, `long`, or `ulong`, or implicitly convertible to one or more of these types.</span></span> <span data-ttu-id="dd693-1299">O valor de cada expressão determina o comprimento da dimensão correspondente na instância de matriz alocada recentemente.</span><span class="sxs-lookup"><span data-stu-id="dd693-1299">The value of each expression determines the length of the corresponding dimension in the newly allocated array instance.</span></span> <span data-ttu-id="dd693-1300">Como o comprimento de uma dimensão de matriz deve ser não negativo, é um erro de tempo de compilação ter um *constant_expression* com um valor negativo na lista de expressões.</span><span class="sxs-lookup"><span data-stu-id="dd693-1300">Since the length of an array dimension must be nonnegative, it is a compile-time error to have a *constant_expression* with a negative value in the expression list.</span></span>

<span data-ttu-id="dd693-1301">Exceto em um contexto não seguro ([contextos não seguros](unsafe-code.md#unsafe-contexts)), o layout das matrizes não é especificado.</span><span class="sxs-lookup"><span data-stu-id="dd693-1301">Except in an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)), the layout of arrays is unspecified.</span></span>

<span data-ttu-id="dd693-1302">Se uma expressão de criação de matriz do primeiro formulário incluir um inicializador de matriz, cada expressão na lista de expressões deverá ser uma constante e os comprimentos de classificação e de dimensão especificados pela lista de expressões devem corresponder àqueles do inicializador de matriz.</span><span class="sxs-lookup"><span data-stu-id="dd693-1302">If an array creation expression of the first form includes an array initializer, each expression in the expression list must be a constant and the rank and dimension lengths specified by the expression list must match those of the array initializer.</span></span>

<span data-ttu-id="dd693-1303">Em uma expressão de criação de matriz do segundo ou terceiro formulário, a classificação do tipo de matriz ou especificador de classificação especificado deve corresponder à do inicializador de matriz.</span><span class="sxs-lookup"><span data-stu-id="dd693-1303">In an array creation expression of the second or third form, the rank of the specified array type or rank specifier must match that of the array initializer.</span></span> <span data-ttu-id="dd693-1304">Os comprimentos de dimensão individuais são inferidos do número de elementos em cada um dos níveis de aninhamento correspondentes do inicializador de matriz.</span><span class="sxs-lookup"><span data-stu-id="dd693-1304">The individual dimension lengths are inferred from the number of elements in each of the corresponding nesting levels of the array initializer.</span></span> <span data-ttu-id="dd693-1305">Portanto, a expressão</span><span class="sxs-lookup"><span data-stu-id="dd693-1305">Thus, the expression</span></span>
```csharp
new int[,] {{0, 1}, {2, 3}, {4, 5}}
```
<span data-ttu-id="dd693-1306">corresponde exatamente a</span><span class="sxs-lookup"><span data-stu-id="dd693-1306">exactly corresponds to</span></span>
```csharp
new int[3, 2] {{0, 1}, {2, 3}, {4, 5}}
```

<span data-ttu-id="dd693-1307">Uma expressão de criação de matriz do terceiro formulário é referida como uma ***expressão de criação de matriz digitada implicitamente***.</span><span class="sxs-lookup"><span data-stu-id="dd693-1307">An array creation expression of the third form is referred to as an ***implicitly typed array creation expression***.</span></span> <span data-ttu-id="dd693-1308">Ele é semelhante ao segundo formulário, exceto pelo fato de que o tipo de elemento da matriz não é fornecido explicitamente, mas determinado como o melhor tipo comum ([encontrando o melhor tipo comum de um conjunto de expressões](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) do conjunto de expressões no inicializador de matriz.</span><span class="sxs-lookup"><span data-stu-id="dd693-1308">It is similar to the second form, except that the element type of the array is not explicitly given, but determined as the best common type ([Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) of the set of expressions in the array initializer.</span></span> <span data-ttu-id="dd693-1309">Para uma matriz multidimensional, ou seja, uma em que a *rank_specifier* contém pelo menos uma vírgula, esse conjunto inclui todas as *expressões*s encontradas em *array_initializer*s aninhadas.</span><span class="sxs-lookup"><span data-stu-id="dd693-1309">For a multidimensional array, i.e., one where the *rank_specifier* contains at least one comma, this set comprises all *expression*s found in nested *array_initializer*s.</span></span>

<span data-ttu-id="dd693-1310">Inicializadores de matriz são descritos mais detalhadamente em [inicializadores de matriz](arrays.md#array-initializers).</span><span class="sxs-lookup"><span data-stu-id="dd693-1310">Array initializers are described further in [Array initializers](arrays.md#array-initializers).</span></span>

<span data-ttu-id="dd693-1311">O resultado da avaliação de uma expressão de criação de matriz é classificado como um valor, ou seja, uma referência à instância de matriz alocada recentemente.</span><span class="sxs-lookup"><span data-stu-id="dd693-1311">The result of evaluating an array creation expression is classified as a value, namely a reference to the newly allocated array instance.</span></span> <span data-ttu-id="dd693-1312">O processamento em tempo de execução de uma expressão de criação de matriz consiste nas seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="dd693-1312">The run-time processing of an array creation expression consists of the following steps:</span></span>

*  <span data-ttu-id="dd693-1313">As expressões de comprimento da dimensão da *expression_list* são avaliadas na ordem, da esquerda para a direita.</span><span class="sxs-lookup"><span data-stu-id="dd693-1313">The dimension length expressions of the *expression_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="dd693-1314">Após a avaliação de cada expressão, uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)) para um dos seguintes tipos é executada: `int`, `uint`, `long``ulong`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1314">Following evaluation of each expression, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to one of the following types is performed: `int`, `uint`, `long`, `ulong`.</span></span> <span data-ttu-id="dd693-1315">O primeiro tipo nesta lista para o qual existe uma conversão implícita é escolhido.</span><span class="sxs-lookup"><span data-stu-id="dd693-1315">The first type in this list for which an implicit conversion exists is chosen.</span></span> <span data-ttu-id="dd693-1316">Se a avaliação de uma expressão ou a conversão implícita subsequente causar uma exceção, nenhuma expressão adicional será avaliada e nenhuma outra etapa será executada.</span><span class="sxs-lookup"><span data-stu-id="dd693-1316">If evaluation of an expression or the subsequent implicit conversion causes an exception, then no further expressions are evaluated and no further steps are executed.</span></span>
*  <span data-ttu-id="dd693-1317">Os valores computados para os comprimentos das dimensões são validados da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="dd693-1317">The computed values for the dimension lengths are validated as follows.</span></span> <span data-ttu-id="dd693-1318">Se um ou mais dos valores forem menores que zero, um `System.OverflowException` será lançado e nenhuma etapa adicional será executada.</span><span class="sxs-lookup"><span data-stu-id="dd693-1318">If one or more of the values are less than zero, a `System.OverflowException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="dd693-1319">Uma instância de matriz com os tamanhos de dimensão fornecidos é alocada.</span><span class="sxs-lookup"><span data-stu-id="dd693-1319">An array instance with the given dimension lengths is allocated.</span></span> <span data-ttu-id="dd693-1320">Se não houver memória suficiente disponível para alocar a nova instância, um `System.OutOfMemoryException` será lançado e nenhuma etapa adicional será executada.</span><span class="sxs-lookup"><span data-stu-id="dd693-1320">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="dd693-1321">Todos os elementos da nova instância de matriz são inicializados para seus valores padrão ([valores padrão](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="dd693-1321">All elements of the new array instance are initialized to their default values ([Default values](variables.md#default-values)).</span></span>
*  <span data-ttu-id="dd693-1322">Se a expressão de criação de matriz contiver um inicializador de matriz, cada expressão no inicializador de matriz será avaliada e atribuída ao elemento de matriz correspondente.</span><span class="sxs-lookup"><span data-stu-id="dd693-1322">If the array creation expression contains an array initializer, then each expression in the array initializer is evaluated and assigned to its corresponding array element.</span></span> <span data-ttu-id="dd693-1323">As avaliações e as atribuições são executadas na ordem em que as expressões são gravadas no inicializador de matriz – em outras palavras, os elementos são inicializados em ordem de índice crescente, com a dimensão mais à direita aumentando primeiro.</span><span class="sxs-lookup"><span data-stu-id="dd693-1323">The evaluations and assignments are performed in the order the expressions are written in the array initializer—in other words, elements are initialized in increasing index order, with the rightmost dimension increasing first.</span></span> <span data-ttu-id="dd693-1324">Se a avaliação de uma determinada expressão ou a atribuição subsequente ao elemento de matriz correspondente causar uma exceção, nenhum elemento adicional será inicializado (e os elementos restantes terão, portanto, seus valores padrão).</span><span class="sxs-lookup"><span data-stu-id="dd693-1324">If evaluation of a given expression or the subsequent assignment to the corresponding array element causes an exception, then no further elements are initialized (and the remaining elements will thus have their default values).</span></span>

<span data-ttu-id="dd693-1325">Uma expressão de criação de matriz permite instanciação de uma matriz com elementos de um tipo de matriz, mas os elementos de tal matriz devem ser inicializados manualmente.</span><span class="sxs-lookup"><span data-stu-id="dd693-1325">An array creation expression permits instantiation of an array with elements of an array type, but the elements of such an array must be manually initialized.</span></span> <span data-ttu-id="dd693-1326">Por exemplo, a instrução</span><span class="sxs-lookup"><span data-stu-id="dd693-1326">For example, the statement</span></span>
```csharp
int[][] a = new int[100][];
```
<span data-ttu-id="dd693-1327">Cria uma matriz unidimensional com 100 elementos do tipo `int[]`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1327">creates a single-dimensional array with 100 elements of type `int[]`.</span></span> <span data-ttu-id="dd693-1328">O valor inicial de cada elemento é `null`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1328">The initial value of each element is `null`.</span></span> <span data-ttu-id="dd693-1329">Não é possível que a mesma expressão de criação de matriz também instancie as submatrizes e a instrução</span><span class="sxs-lookup"><span data-stu-id="dd693-1329">It is not possible for the same array creation expression to also instantiate the sub-arrays, and the statement</span></span>
```csharp
int[][] a = new int[100][5];        // Error
```
<span data-ttu-id="dd693-1330">resulta em um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1330">results in a compile-time error.</span></span> <span data-ttu-id="dd693-1331">Em vez disso, a instanciação das submatrizes deve ser executada manualmente, como em</span><span class="sxs-lookup"><span data-stu-id="dd693-1331">Instantiation of the sub-arrays must instead be performed manually, as in</span></span>
```csharp
int[][] a = new int[100][];
for (int i = 0; i < 100; i++) a[i] = new int[5];
```

<span data-ttu-id="dd693-1332">Quando uma matriz de matrizes tem uma forma "retangular", ou seja, quando as submatrizes têm o mesmo comprimento, é mais eficiente usar uma matriz multidimensional.</span><span class="sxs-lookup"><span data-stu-id="dd693-1332">When an array of arrays has a "rectangular" shape, that is when the sub-arrays are all of the same length, it is more efficient to use a multi-dimensional array.</span></span> <span data-ttu-id="dd693-1333">No exemplo acima, a instanciação da matriz de matrizes cria 101 objetos — uma matriz externa e submatrizes 100.</span><span class="sxs-lookup"><span data-stu-id="dd693-1333">In the example above, instantiation of the array of arrays creates 101 objects—one outer array and 100 sub-arrays.</span></span> <span data-ttu-id="dd693-1334">Por outro lado,</span><span class="sxs-lookup"><span data-stu-id="dd693-1334">In contrast,</span></span>
```csharp
int[,] = new int[100, 5];
```
<span data-ttu-id="dd693-1335">cria apenas um único objeto, uma matriz bidimensional e realiza a alocação em uma única instrução.</span><span class="sxs-lookup"><span data-stu-id="dd693-1335">creates only a single object, a two-dimensional array, and accomplishes the allocation in a single statement.</span></span>

<span data-ttu-id="dd693-1336">Veja a seguir exemplos de expressões de criação de matriz digitadas implicitamente:</span><span class="sxs-lookup"><span data-stu-id="dd693-1336">The following are examples of implicitly typed array creation expressions:</span></span>
```csharp
var a = new[] { 1, 10, 100, 1000 };                       // int[]

var b = new[] { 1, 1.5, 2, 2.5 };                         // double[]

var c = new[,] { { "hello", null }, { "world", "!" } };   // string[,]

var d = new[] { 1, "one", 2, "two" };                     // Error
```

<span data-ttu-id="dd693-1337">A última expressão causa um erro de tempo de compilação porque nem `int` nem `string` é implicitamente conversível para a outra e, portanto, não há um melhor tipo comum.</span><span class="sxs-lookup"><span data-stu-id="dd693-1337">The last expression causes a compile-time error because neither `int` nor `string` is implicitly convertible to the other, and so there is no best common type.</span></span> <span data-ttu-id="dd693-1338">Uma expressão de criação de matriz tipada explicitamente deve ser usada neste caso, por exemplo, especificando o tipo a ser `object[]`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1338">An explicitly typed array creation expression must be used in this case, for example specifying the type to be `object[]`.</span></span> <span data-ttu-id="dd693-1339">Como alternativa, um dos elementos pode ser convertido em um tipo base comum, que se tornaria o tipo de elemento inferido.</span><span class="sxs-lookup"><span data-stu-id="dd693-1339">Alternatively, one of the elements can be cast to a common base type, which would then become the inferred element type.</span></span>

<span data-ttu-id="dd693-1340">As expressões de criação de matriz com tipo implícito podem ser combinadas com inicializadores de objeto anônimo ([expressões de criação de objeto anônimo](expressions.md#anonymous-object-creation-expressions)) para criar estruturas de dados com tipo anônimo.</span><span class="sxs-lookup"><span data-stu-id="dd693-1340">Implicitly typed array creation expressions can be combined with anonymous object initializers ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) to create anonymously typed data structures.</span></span> <span data-ttu-id="dd693-1341">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="dd693-1341">For example:</span></span>
```csharp
var contacts = new[] {
    new {
        Name = "Chris Smith",
        PhoneNumbers = new[] { "206-555-0101", "425-882-8080" }
    },
    new {
        Name = "Bob Harris",
        PhoneNumbers = new[] { "650-555-0199" }
    }
};
```

#### <a name="delegate-creation-expressions"></a><span data-ttu-id="dd693-1342">Delegar expressões de criação</span><span class="sxs-lookup"><span data-stu-id="dd693-1342">Delegate creation expressions</span></span>

<span data-ttu-id="dd693-1343">Uma *delegate_creation_expression* é usada para criar uma nova instância de um *delegate_type*.</span><span class="sxs-lookup"><span data-stu-id="dd693-1343">A *delegate_creation_expression* is used to create a new instance of a *delegate_type*.</span></span>

```antlr
delegate_creation_expression
    : 'new' delegate_type '(' expression ')'
    ;
```

<span data-ttu-id="dd693-1344">O argumento de uma expressão de criação de delegado deve ser um grupo de métodos, uma função anônima ou um valor do tipo de tempo de compilação `dynamic` ou um *delegate_type*.</span><span class="sxs-lookup"><span data-stu-id="dd693-1344">The argument of a delegate creation expression must be a method group, an anonymous function or a value of either the compile time type `dynamic` or a *delegate_type*.</span></span> <span data-ttu-id="dd693-1345">Se o argumento for um grupo de métodos, ele identificará o método e, para um método de instância, o objeto para o qual criar um delegado.</span><span class="sxs-lookup"><span data-stu-id="dd693-1345">If the argument is a method group, it identifies the method and, for an instance method, the object for which to create a delegate.</span></span> <span data-ttu-id="dd693-1346">Se o argumento for uma função anônima, ele definirá diretamente os parâmetros e o corpo do método do destino delegado.</span><span class="sxs-lookup"><span data-stu-id="dd693-1346">If the argument is an anonymous function it directly defines the parameters and method body of the delegate target.</span></span> <span data-ttu-id="dd693-1347">Se o argumento for um valor, ele identificará uma instância de delegado da qual criar uma cópia.</span><span class="sxs-lookup"><span data-stu-id="dd693-1347">If the argument is a value it identifies a delegate instance of which to create a copy.</span></span>

<span data-ttu-id="dd693-1348">Se a *expressão* tiver o tipo de tempo de compilação `dynamic`, a *delegate_creation_expression* será vinculada dinamicamente ([associação dinâmica](expressions.md#dynamic-binding)) e as regras abaixo serão aplicadas em tempo de execução usando o tipo de tempo de execução da *expressão*.</span><span class="sxs-lookup"><span data-stu-id="dd693-1348">If the *expression* has the compile-time type `dynamic`, the *delegate_creation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), and the rules below are applied at run-time using the run-time type of the *expression*.</span></span> <span data-ttu-id="dd693-1349">Caso contrário, as regras são aplicadas em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1349">Otherwise the rules are applied at compile-time.</span></span>

<span data-ttu-id="dd693-1350">O processamento de tempo de associação de um *delegate_creation_expression* do formulário `new D(E)`, em que `D` é uma *delegate_type* e `E` é uma *expressão*, consiste nas seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="dd693-1350">The binding-time processing of a *delegate_creation_expression* of the form `new D(E)`, where `D` is a *delegate_type* and `E` is an *expression*, consists of the following steps:</span></span>

*  <span data-ttu-id="dd693-1351">Se `E` for um grupo de métodos, a expressão de criação de representante será processada da mesma maneira que uma conversão de grupo de métodos ([conversões de grupos de métodos](conversions.md#method-group-conversions)) de `E` para `D`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1351">If `E` is a method group, the delegate creation expression is processed in the same way as a method group conversion ([Method group conversions](conversions.md#method-group-conversions)) from `E` to `D`.</span></span>
*  <span data-ttu-id="dd693-1352">Se `E` for uma função anônima, a expressão de criação de representante será processada da mesma maneira que uma conversão de função anônima ([conversões de função anônimas](conversions.md#anonymous-function-conversions)) de `E` para `D`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1352">If `E` is an anonymous function, the delegate creation expression is processed in the same way as an anonymous function conversion ([Anonymous function conversions](conversions.md#anonymous-function-conversions)) from `E` to `D`.</span></span>
*  <span data-ttu-id="dd693-1353">Se `E` for um valor, `E` deverá ser compatível ([delegar declarações](delegates.md#delegate-declarations)) com `D`e o resultado será uma referência a um delegado recém-criado do tipo `D` que se refere à mesma lista de invocação que o `E`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1353">If `E` is a value, `E` must be compatible ([Delegate declarations](delegates.md#delegate-declarations)) with `D`, and the result is a reference to a newly created delegate of type `D` that refers to the same invocation list as `E`.</span></span> <span data-ttu-id="dd693-1354">Se `E` não for compatível com `D`, ocorrerá um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1354">If `E` is not compatible with `D`, a compile-time error occurs.</span></span>

<span data-ttu-id="dd693-1355">O processamento em tempo de execução de um *delegate_creation_expression* do formulário `new D(E)`, em que `D` é uma *delegate_type* e `E` é uma *expressão*, consiste nas seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="dd693-1355">The run-time processing of a *delegate_creation_expression* of the form `new D(E)`, where `D` is a *delegate_type* and `E` is an *expression*, consists of the following steps:</span></span>

*   <span data-ttu-id="dd693-1356">Se `E` for um grupo de métodos, a expressão de criação de representante será avaliada como uma conversão de grupo de métodos ([conversões de grupos de métodos](conversions.md#method-group-conversions)) de `E` para `D`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1356">If `E` is a method group, the delegate creation expression is evaluated as a method group conversion ([Method group conversions](conversions.md#method-group-conversions)) from `E` to `D`.</span></span>
*   <span data-ttu-id="dd693-1357">Se `E` for uma função anônima, a criação de delegado será avaliada como uma conversão de função anônima de `E` para `D` ([conversões de função anônimas](conversions.md#anonymous-function-conversions)).</span><span class="sxs-lookup"><span data-stu-id="dd693-1357">If `E` is an anonymous function, the delegate creation is evaluated as an anonymous function conversion from `E` to `D` ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>
*   <span data-ttu-id="dd693-1358">Se `E` for um valor de uma *delegate_type*:</span><span class="sxs-lookup"><span data-stu-id="dd693-1358">If `E` is a value of a *delegate_type*:</span></span>
    * <span data-ttu-id="dd693-1359">`E` é avaliado.</span><span class="sxs-lookup"><span data-stu-id="dd693-1359">`E` is evaluated.</span></span> <span data-ttu-id="dd693-1360">Se essa avaliação causar uma exceção, nenhuma etapa adicional será executada.</span><span class="sxs-lookup"><span data-stu-id="dd693-1360">If this evaluation causes an exception, no further steps are executed.</span></span>
    * <span data-ttu-id="dd693-1361">Se o valor de `E` for `null`, um `System.NullReferenceException` será lançado e nenhuma etapa adicional será executada.</span><span class="sxs-lookup"><span data-stu-id="dd693-1361">If the value of `E` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="dd693-1362">Uma nova instância do tipo delegado `D` é alocada.</span><span class="sxs-lookup"><span data-stu-id="dd693-1362">A new instance of the delegate type `D` is allocated.</span></span> <span data-ttu-id="dd693-1363">Se não houver memória suficiente disponível para alocar a nova instância, um `System.OutOfMemoryException` será lançado e nenhuma etapa adicional será executada.</span><span class="sxs-lookup"><span data-stu-id="dd693-1363">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="dd693-1364">A nova instância de delegado é inicializada com a mesma lista de invocação que a instância de delegado fornecida por `E`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1364">The new delegate instance is initialized with the same invocation list as the delegate instance given by `E`.</span></span>

<span data-ttu-id="dd693-1365">A lista de invocação de um delegado é determinada quando o delegado é instanciado e, em seguida, permanece constante para todo o tempo de vida do delegado.</span><span class="sxs-lookup"><span data-stu-id="dd693-1365">The invocation list of a delegate is determined when the delegate is instantiated and then remains constant for the entire lifetime of the delegate.</span></span> <span data-ttu-id="dd693-1366">Em outras palavras, não é possível alterar as entidades de destino que podem ser chamadas de um delegado depois que ele tiver sido criado.</span><span class="sxs-lookup"><span data-stu-id="dd693-1366">In other words, it is not possible to change the target callable entities of a delegate once it has been created.</span></span> <span data-ttu-id="dd693-1367">Quando dois delegados são combinados ou um é removido de outro ([delegar declarações](delegates.md#delegate-declarations)), um novo delegado resulta; nenhum delegado existente tem seu conteúdo alterado.</span><span class="sxs-lookup"><span data-stu-id="dd693-1367">When two delegates are combined or one is removed from another ([Delegate declarations](delegates.md#delegate-declarations)), a new delegate results; no existing delegate has its contents changed.</span></span>

<span data-ttu-id="dd693-1368">Não é possível criar um delegado que se refere a uma propriedade, indexador, operador definido pelo usuário, Construtor de instância, destruidor ou construtor estático.</span><span class="sxs-lookup"><span data-stu-id="dd693-1368">It is not possible to create a delegate that refers to a property, indexer, user-defined operator, instance constructor, destructor, or static constructor.</span></span>

<span data-ttu-id="dd693-1369">Conforme descrito acima, quando um delegado é criado a partir de um grupo de métodos, a lista de parâmetros formais e o tipo de retorno do delegado determinam qual dos métodos sobrecarregados selecionar.</span><span class="sxs-lookup"><span data-stu-id="dd693-1369">As described above, when a delegate is created from a method group, the formal parameter list and return type of the delegate determine which of the overloaded methods to select.</span></span> <span data-ttu-id="dd693-1370">No exemplo</span><span class="sxs-lookup"><span data-stu-id="dd693-1370">In the example</span></span>
```csharp
delegate double DoubleFunc(double x);

class A
{
    DoubleFunc f = new DoubleFunc(Square);

    static float Square(float x) {
        return x * x;
    }

    static double Square(double x) {
        return x * x;
    }
}
```
<span data-ttu-id="dd693-1371">o campo `A.f` é inicializado com um delegado que se refere ao segundo método `Square` porque esse método corresponde exatamente à lista de parâmetros formais e ao tipo de retorno de `DoubleFunc`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1371">the `A.f` field is initialized with a delegate that refers to the second `Square` method because that method exactly matches the formal parameter list and return type of `DoubleFunc`.</span></span> <span data-ttu-id="dd693-1372">Se o segundo método de `Square` não estivesse presente, ocorreria um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1372">Had the second `Square` method not been present, a compile-time error would have occurred.</span></span>

#### <a name="anonymous-object-creation-expressions"></a><span data-ttu-id="dd693-1373">Expressões de criação de objeto anônimo</span><span class="sxs-lookup"><span data-stu-id="dd693-1373">Anonymous object creation expressions</span></span>

<span data-ttu-id="dd693-1374">Um *anonymous_object_creation_expression* é usado para criar um objeto de um tipo anônimo.</span><span class="sxs-lookup"><span data-stu-id="dd693-1374">An *anonymous_object_creation_expression* is used to create an object of an anonymous type.</span></span>

```antlr
anonymous_object_creation_expression
    : 'new' anonymous_object_initializer
    ;

anonymous_object_initializer
    : '{' member_declarator_list? '}'
    | '{' member_declarator_list ',' '}'
    ;

member_declarator_list
    : member_declarator (',' member_declarator)*
    ;

member_declarator
    : simple_name
    | member_access
    | base_access
    | null_conditional_member_access
    | identifier '=' expression
    ;
```

<span data-ttu-id="dd693-1375">Um inicializador de objeto anônimo declara um tipo anônimo e retorna uma instância desse tipo.</span><span class="sxs-lookup"><span data-stu-id="dd693-1375">An anonymous object initializer declares an anonymous type and returns an instance of that type.</span></span> <span data-ttu-id="dd693-1376">Um tipo anônimo é um tipo de classe sem nome que herda diretamente de `object`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1376">An anonymous type is a nameless class type that inherits directly from `object`.</span></span> <span data-ttu-id="dd693-1377">Os membros de um tipo anônimo são uma sequência de propriedades somente leitura inferidas do inicializador de objeto anônimo usado para criar uma instância do tipo.</span><span class="sxs-lookup"><span data-stu-id="dd693-1377">The members of an anonymous type are a sequence of read-only properties inferred from the anonymous object initializer used to create an instance of the type.</span></span> <span data-ttu-id="dd693-1378">Especificamente, um inicializador de objeto anônimo do formulário</span><span class="sxs-lookup"><span data-stu-id="dd693-1378">Specifically, an anonymous object initializer of the form</span></span>
```csharp
new { p1 = e1, p2 = e2, ..., pn = en }
```
<span data-ttu-id="dd693-1379">declara um tipo anônimo do formulário</span><span class="sxs-lookup"><span data-stu-id="dd693-1379">declares an anonymous type of the form</span></span>
```csharp
class __Anonymous1
{
    private readonly T1 f1;
    private readonly T2 f2;
    ...
    private readonly Tn fn;

    public __Anonymous1(T1 a1, T2 a2, ..., Tn an) {
        f1 = a1;
        f2 = a2;
        ...
        fn = an;
    }

    public T1 p1 { get { return f1; } }
    public T2 p2 { get { return f2; } }
    ...
    public Tn pn { get { return fn; } }

    public override bool Equals(object __o) { ... }
    public override int GetHashCode() { ... }
}
```
<span data-ttu-id="dd693-1380">onde cada `Tx` é o tipo da expressão correspondente `ex`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1380">where each `Tx` is the type of the corresponding expression `ex`.</span></span> <span data-ttu-id="dd693-1381">A expressão usada em um *member_declarator* deve ter um tipo.</span><span class="sxs-lookup"><span data-stu-id="dd693-1381">The expression used in a *member_declarator* must have a type.</span></span> <span data-ttu-id="dd693-1382">Portanto, é um erro de tempo de compilação para uma expressão em um *member_declarator* ser nulo ou uma função anônima.</span><span class="sxs-lookup"><span data-stu-id="dd693-1382">Thus, it is a compile-time error for an expression in a *member_declarator* to be null or an anonymous function.</span></span> <span data-ttu-id="dd693-1383">Também é um erro de tempo de compilação para que a expressão tenha um tipo não seguro.</span><span class="sxs-lookup"><span data-stu-id="dd693-1383">It is also a compile-time error for the expression to have an unsafe type.</span></span>

<span data-ttu-id="dd693-1384">Os nomes de um tipo anônimo e do parâmetro para seu método `Equals` são gerados automaticamente pelo compilador e não podem ser referenciados no texto do programa.</span><span class="sxs-lookup"><span data-stu-id="dd693-1384">The names of an anonymous type and of the parameter to its `Equals` method are automatically generated by the compiler and cannot be referenced in program text.</span></span>

<span data-ttu-id="dd693-1385">Dentro do mesmo programa, dois inicializadores de objeto anônimos que especificam uma sequência de propriedades dos mesmos nomes e tipos de tempo de compilação na mesma ordem produzirão instâncias do mesmo tipo anônimo.</span><span class="sxs-lookup"><span data-stu-id="dd693-1385">Within the same program, two anonymous object initializers that specify a sequence of properties of the same names and compile-time types in the same order will produce instances of the same anonymous type.</span></span>

<span data-ttu-id="dd693-1386">No exemplo</span><span class="sxs-lookup"><span data-stu-id="dd693-1386">In the example</span></span>
```csharp
var p1 = new { Name = "Lawnmower", Price = 495.00 };
var p2 = new { Name = "Shovel", Price = 26.95 };
p1 = p2;
```
<span data-ttu-id="dd693-1387">a atribuição na última linha é permitida porque `p1` e `p2` são do mesmo tipo anônimo.</span><span class="sxs-lookup"><span data-stu-id="dd693-1387">the assignment on the last line is permitted because `p1` and `p2` are of the same anonymous type.</span></span>

<span data-ttu-id="dd693-1388">Os métodos `Equals` e `GetHashcode` em tipos anônimos substituem os métodos herdados de `object`e são definidos em termos de `Equals` e `GetHashcode` das propriedades, de forma que duas instâncias do mesmo tipo anônimo sejam iguais se e somente se todas as suas propriedades forem iguais.</span><span class="sxs-lookup"><span data-stu-id="dd693-1388">The `Equals` and `GetHashcode` methods on anonymous types override the methods inherited from `object`, and are defined in terms of the `Equals` and `GetHashcode` of the properties, so that two instances of the same anonymous type are equal if and only if all their properties are equal.</span></span>

<span data-ttu-id="dd693-1389">Um Declarador de membro pode ser abreviado como um nome simples ([inferência de tipo](expressions.md#type-inference)), um acesso de membro ([verificação de tempo de compilação de resolução dinâmica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), um acesso base ([acesso de base](expressions.md#base-access)) ou um membro condicional nulo de acesso ([expressões condicionais nulas como inicializadores de projeção](expressions.md#null-conditional-expressions-as-projection-initializers)).</span><span class="sxs-lookup"><span data-stu-id="dd693-1389">A member declarator can be abbreviated to a simple name ([Type inference](expressions.md#type-inference)), a member access ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), a base access ([Base access](expressions.md#base-access)) or a null-conditional member access ([Null-conditional expressions as projection initializers](expressions.md#null-conditional-expressions-as-projection-initializers)).</span></span> <span data-ttu-id="dd693-1390">Isso é chamado de ***inicializador de projeção*** e é abreviado para uma declaração de e atribuição a uma propriedade com o mesmo nome.</span><span class="sxs-lookup"><span data-stu-id="dd693-1390">This is called a ***projection initializer*** and is shorthand for a declaration of and assignment to a property with the same name.</span></span> <span data-ttu-id="dd693-1391">Especificamente, os declaradores de membros dos formulários</span><span class="sxs-lookup"><span data-stu-id="dd693-1391">Specifically, member declarators of the forms</span></span>
```csharp
identifier
expr.identifier
```
<span data-ttu-id="dd693-1392">são precisamente equivalentes aos seguintes, respectivamente:</span><span class="sxs-lookup"><span data-stu-id="dd693-1392">are precisely equivalent to the following, respectively:</span></span>
```csharp
identifier = identifier
identifier = expr.identifier
```

<span data-ttu-id="dd693-1393">Portanto, em um inicializador de projeção, o *identificador* seleciona o valor e o campo ou propriedade ao qual o valor é atribuído.</span><span class="sxs-lookup"><span data-stu-id="dd693-1393">Thus, in a projection initializer the *identifier* selects both the value and the field or property to which the value is assigned.</span></span> <span data-ttu-id="dd693-1394">Intuitivamente, um inicializador de projeção projeta não apenas um valor, mas também o nome do valor.</span><span class="sxs-lookup"><span data-stu-id="dd693-1394">Intuitively, a projection initializer projects not just a value, but also the name of the value.</span></span>

### <a name="the-typeof-operator"></a><span data-ttu-id="dd693-1395">O operador typeof</span><span class="sxs-lookup"><span data-stu-id="dd693-1395">The typeof operator</span></span>

<span data-ttu-id="dd693-1396">O operador de `typeof` é usado para obter o objeto de `System.Type` para um tipo.</span><span class="sxs-lookup"><span data-stu-id="dd693-1396">The `typeof` operator is used to obtain the `System.Type` object for a type.</span></span>

```antlr
typeof_expression
    : 'typeof' '(' type ')'
    | 'typeof' '(' unbound_type_name ')'
    | 'typeof' '(' 'void' ')'
    ;

unbound_type_name
    : identifier generic_dimension_specifier?
    | identifier '::' identifier generic_dimension_specifier?
    | unbound_type_name '.' identifier generic_dimension_specifier?
    ;

generic_dimension_specifier
    : '<' comma* '>'
    ;

comma
    : ','
    ;
```

<span data-ttu-id="dd693-1397">A primeira forma de *typeof_expression* consiste em uma palavra-chave `typeof` seguida por um *tipo*entre parênteses.</span><span class="sxs-lookup"><span data-stu-id="dd693-1397">The first form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized *type*.</span></span> <span data-ttu-id="dd693-1398">O resultado de uma expressão desse formulário é o objeto `System.Type` para o tipo indicado.</span><span class="sxs-lookup"><span data-stu-id="dd693-1398">The result of an expression of this form is the `System.Type` object for the indicated type.</span></span> <span data-ttu-id="dd693-1399">Há apenas um objeto `System.Type` para qualquer tipo específico.</span><span class="sxs-lookup"><span data-stu-id="dd693-1399">There is only one `System.Type` object for any given type.</span></span> <span data-ttu-id="dd693-1400">Isso significa que, para um tipo `T`, `typeof(T) == typeof(T)` é sempre verdadeiro.</span><span class="sxs-lookup"><span data-stu-id="dd693-1400">This means that for a type `T`, `typeof(T) == typeof(T)` is always true.</span></span> <span data-ttu-id="dd693-1401">O *tipo* não pode ser `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1401">The *type* cannot be `dynamic`.</span></span>

<span data-ttu-id="dd693-1402">A segunda forma de *typeof_expression* consiste em uma palavra-chave `typeof` seguida por uma *unbound_type_name*entre parênteses.</span><span class="sxs-lookup"><span data-stu-id="dd693-1402">The second form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized *unbound_type_name*.</span></span> <span data-ttu-id="dd693-1403">Um *unbound_type_name* é muito semelhante a um *type_name* ([namespace e nomes de tipo](basic-concepts.md#namespace-and-type-names)), exceto pelo fato de que um *unbound_type_name* contém *generic_dimension_specifier*s onde um *type_name* contém *type_argument_list*s.</span><span class="sxs-lookup"><span data-stu-id="dd693-1403">An *unbound_type_name* is very similar to a *type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) except that an *unbound_type_name* contains *generic_dimension_specifier*s where a *type_name* contains *type_argument_list*s.</span></span> <span data-ttu-id="dd693-1404">Quando o operando de uma *typeof_expression* é uma sequência de tokens que satisfaz as gramáticas de *unbound_type_name* e *type_name*, ou seja, quando ele não contém uma *generic_dimension_specifier* nem uma *type_argument_list*, a sequência de tokens é considerada uma *type_name*.</span><span class="sxs-lookup"><span data-stu-id="dd693-1404">When the operand of a *typeof_expression* is a sequence of tokens that satisfies the grammars of both *unbound_type_name* and *type_name*, namely when it contains neither a *generic_dimension_specifier* nor a *type_argument_list*, the sequence of tokens is considered to be a *type_name*.</span></span> <span data-ttu-id="dd693-1405">O significado de um *unbound_type_name* é determinado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="dd693-1405">The meaning of an *unbound_type_name* is determined as follows:</span></span>

*  <span data-ttu-id="dd693-1406">Converta a sequência de tokens em uma *type_name* substituindo cada *generic_dimension_specifier* por uma *type_argument_list* com o mesmo número de vírgulas e a palavra-chave `object` como cada *type_argument*.</span><span class="sxs-lookup"><span data-stu-id="dd693-1406">Convert the sequence of tokens to a *type_name* by replacing each *generic_dimension_specifier* with a *type_argument_list* having the same number of commas and the keyword `object` as each *type_argument*.</span></span>
*  <span data-ttu-id="dd693-1407">Avalie as *type_name*resultantes, ignorando todas as restrições de parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="dd693-1407">Evaluate the resulting *type_name*, while ignoring all type parameter constraints.</span></span>
*  <span data-ttu-id="dd693-1408">O *unbound_type_name* é resolvido para o tipo genérico não associado associado ao tipo construído resultante ([tipos vinculados e desvinculados](types.md#bound-and-unbound-types)).</span><span class="sxs-lookup"><span data-stu-id="dd693-1408">The *unbound_type_name* resolves to the unbound generic type associated with the resulting constructed type ([Bound and unbound types](types.md#bound-and-unbound-types)).</span></span>

<span data-ttu-id="dd693-1409">O resultado da *typeof_expression* é o objeto `System.Type` para o tipo genérico não associado resultante.</span><span class="sxs-lookup"><span data-stu-id="dd693-1409">The result of the *typeof_expression* is the `System.Type` object for the resulting unbound generic type.</span></span>

<span data-ttu-id="dd693-1410">A terceira forma de *typeof_expression* consiste em uma palavra-chave `typeof` seguida por uma palavra-chave `void` entre parênteses.</span><span class="sxs-lookup"><span data-stu-id="dd693-1410">The third form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized `void` keyword.</span></span> <span data-ttu-id="dd693-1411">O resultado de uma expressão desse formulário é o `System.Type` objeto que representa a ausência de um tipo.</span><span class="sxs-lookup"><span data-stu-id="dd693-1411">The result of an expression of this form is the `System.Type` object that represents the absence of a type.</span></span> <span data-ttu-id="dd693-1412">O objeto de tipo retornado por `typeof(void)` é diferente do objeto de tipo retornado para qualquer tipo.</span><span class="sxs-lookup"><span data-stu-id="dd693-1412">The type object returned by `typeof(void)` is distinct from the type object returned for any type.</span></span> <span data-ttu-id="dd693-1413">Esse objeto de tipo especial é útil em bibliotecas de classes que permitem a reflexão em métodos no idioma, em que esses métodos desejam ter uma maneira de representar o tipo de retorno de qualquer método, incluindo métodos void, com uma instância de `System.Type`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1413">This special type object is useful in class libraries that allow reflection onto methods in the language, where those methods wish to have a way to represent the return type of any method, including void methods, with an instance of `System.Type`.</span></span>

<span data-ttu-id="dd693-1414">O operador `typeof` pode ser usado em um parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="dd693-1414">The `typeof` operator can be used on a type parameter.</span></span> <span data-ttu-id="dd693-1415">O resultado é o objeto `System.Type` para o tipo de tempo de execução que foi associado ao parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="dd693-1415">The result is the `System.Type` object for the run-time type that was bound to the type parameter.</span></span> <span data-ttu-id="dd693-1416">O operador de `typeof` também pode ser usado em um tipo construído ou um tipo genérico não associado ([tipos vinculados e desvinculados](types.md#bound-and-unbound-types)).</span><span class="sxs-lookup"><span data-stu-id="dd693-1416">The `typeof` operator can also be used on a constructed type or an unbound generic type ([Bound and unbound types](types.md#bound-and-unbound-types)).</span></span> <span data-ttu-id="dd693-1417">O objeto `System.Type` para um tipo genérico não associado não é o mesmo que o objeto `System.Type` do tipo de instância.</span><span class="sxs-lookup"><span data-stu-id="dd693-1417">The `System.Type` object for an unbound generic type is not the same as the `System.Type` object of the instance type.</span></span> <span data-ttu-id="dd693-1418">O tipo de instância é sempre um tipo construído fechado em tempo de execução, de modo que seu objeto de `System.Type` depende dos argumentos de tipo de tempo de execução em uso, enquanto o tipo genérico não associado não tem argumentos de tipo.</span><span class="sxs-lookup"><span data-stu-id="dd693-1418">The instance type is always a closed constructed type at run-time so its `System.Type` object depends on the run-time type arguments in use, while the unbound generic type has no type arguments.</span></span>

<span data-ttu-id="dd693-1419">O exemplo</span><span class="sxs-lookup"><span data-stu-id="dd693-1419">The example</span></span>
```csharp
using System;

class X<T>
{
    public static void PrintTypes() {
        Type[] t = {
            typeof(int),
            typeof(System.Int32),
            typeof(string),
            typeof(double[]),
            typeof(void),
            typeof(T),
            typeof(X<T>),
            typeof(X<X<T>>),
            typeof(X<>)
        };
        for (int i = 0; i < t.Length; i++) {
            Console.WriteLine(t[i]);
        }
    }
}

class Test
{
    static void Main() {
        X<int>.PrintTypes();
    }
}
```
<span data-ttu-id="dd693-1420">produz a seguinte saída:</span><span class="sxs-lookup"><span data-stu-id="dd693-1420">produces the following output:</span></span>
```console
System.Int32
System.Int32
System.String
System.Double[]
System.Void
System.Int32
X`1[System.Int32]
X`1[X`1[System.Int32]]
X`1[T]
```

<span data-ttu-id="dd693-1421">Observe que `int` e `System.Int32` são do mesmo tipo.</span><span class="sxs-lookup"><span data-stu-id="dd693-1421">Note that `int` and `System.Int32` are the same type.</span></span>

<span data-ttu-id="dd693-1422">Observe também que o resultado de `typeof(X<>)` não depende do argumento de tipo, mas o resultado de `typeof(X<T>)`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1422">Also note that the result of `typeof(X<>)` does not depend on the type argument but the result of `typeof(X<T>)` does.</span></span>

### <a name="the-checked-and-unchecked-operators"></a><span data-ttu-id="dd693-1423">Os operadores verificados e não verificados</span><span class="sxs-lookup"><span data-stu-id="dd693-1423">The checked and unchecked operators</span></span>

<span data-ttu-id="dd693-1424">Os operadores de `checked` e `unchecked` são usados para controlar o ***contexto de verificação de estouro*** para operações e conversões aritméticas de tipo integral.</span><span class="sxs-lookup"><span data-stu-id="dd693-1424">The `checked` and `unchecked` operators are used to control the ***overflow checking context*** for integral-type arithmetic operations and conversions.</span></span>

```antlr
checked_expression
    : 'checked' '(' expression ')'
    ;

unchecked_expression
    : 'unchecked' '(' expression ')'
    ;
```

<span data-ttu-id="dd693-1425">O operador `checked` avalia a expressão contida em um contexto selecionado e o operador `unchecked` avalia a expressão contida em um contexto desmarcado.</span><span class="sxs-lookup"><span data-stu-id="dd693-1425">The `checked` operator evaluates the contained expression in a checked context, and the `unchecked` operator evaluates the contained expression in an unchecked context.</span></span> <span data-ttu-id="dd693-1426">Um *checked_expression* ou *unchecked_expression* corresponde exatamente a um *parenthesized_expression* ([expressões entre parênteses](expressions.md#parenthesized-expressions)), exceto que a expressão contida é avaliada no contexto de verificação de estouro especificado.</span><span class="sxs-lookup"><span data-stu-id="dd693-1426">A *checked_expression* or *unchecked_expression* corresponds exactly to a *parenthesized_expression* ([Parenthesized expressions](expressions.md#parenthesized-expressions)), except that the contained expression is evaluated in the given overflow checking context.</span></span>

<span data-ttu-id="dd693-1427">O contexto de verificação de estouro também pode ser controlado por meio das instruções `checked` e `unchecked` ([as instruções marcadas e não verificadas](statements.md#the-checked-and-unchecked-statements)).</span><span class="sxs-lookup"><span data-stu-id="dd693-1427">The overflow checking context can also be controlled through the `checked` and `unchecked` statements ([The checked and unchecked statements](statements.md#the-checked-and-unchecked-statements)).</span></span>

<span data-ttu-id="dd693-1428">As operações a seguir são afetadas pelo contexto de verificação de estouro estabelecido pelo `checked` e `unchecked` de operadores e instruções:</span><span class="sxs-lookup"><span data-stu-id="dd693-1428">The following operations are affected by the overflow checking context established by the `checked` and `unchecked` operators and statements:</span></span>

*  <span data-ttu-id="dd693-1429">Os operadores predefinidos `++` e `--` unários ([incremento de sufixo e diminuição de operadores](expressions.md#postfix-increment-and-decrement-operators) e [incrementos de prefixo e diminuição](expressions.md#prefix-increment-and-decrement-operators)) quando o operando é de um tipo integral.</span><span class="sxs-lookup"><span data-stu-id="dd693-1429">The predefined `++` and `--` unary operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), when the operand is of an integral type.</span></span>
*  <span data-ttu-id="dd693-1430">O operador unário predefinido `-` ([operador menos unário](expressions.md#unary-minus-operator)), quando o operando é de um tipo integral.</span><span class="sxs-lookup"><span data-stu-id="dd693-1430">The predefined `-` unary operator ([Unary minus operator](expressions.md#unary-minus-operator)), when the operand is of an integral type.</span></span>
*  <span data-ttu-id="dd693-1431">O `+`predefinido, `-`, `*`e `/` operadores binários ([operadores aritméticos](expressions.md#arithmetic-operators)), quando ambos os operandos são de tipos integrais.</span><span class="sxs-lookup"><span data-stu-id="dd693-1431">The predefined `+`, `-`, `*`, and `/` binary operators ([Arithmetic operators](expressions.md#arithmetic-operators)), when both operands are of integral types.</span></span>
*  <span data-ttu-id="dd693-1432">Conversões numéricas explícitas ([conversões numéricas explícitas](conversions.md#explicit-numeric-conversions)) de um tipo integral para outro tipo integral, ou de `float` ou `double` a um tipo integral.</span><span class="sxs-lookup"><span data-stu-id="dd693-1432">Explicit numeric conversions ([Explicit numeric conversions](conversions.md#explicit-numeric-conversions)) from one integral type to another integral type, or from `float` or `double` to an integral type.</span></span>

<span data-ttu-id="dd693-1433">Quando uma das operações acima produz um resultado muito grande para representar no tipo de destino, o contexto no qual a operação é executada controla o comportamento resultante:</span><span class="sxs-lookup"><span data-stu-id="dd693-1433">When one of the above operations produce a result that is too large to represent in the destination type, the context in which the operation is performed controls the resulting behavior:</span></span>

*  <span data-ttu-id="dd693-1434">Em um contexto de `checked`, se a operação for uma expressão constante ([expressões constantes](expressions.md#constant-expressions)), ocorrerá um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1434">In a `checked` context, if the operation is a constant expression ([Constant expressions](expressions.md#constant-expressions)), a compile-time error occurs.</span></span> <span data-ttu-id="dd693-1435">Caso contrário, quando a operação for executada em tempo de execução, uma `System.OverflowException` será lançada.</span><span class="sxs-lookup"><span data-stu-id="dd693-1435">Otherwise, when the operation is performed at run-time, a `System.OverflowException` is thrown.</span></span>
*  <span data-ttu-id="dd693-1436">Em um contexto de `unchecked`, o resultado é truncado descartando quaisquer bits de ordem superior que não caibam no tipo de destino.</span><span class="sxs-lookup"><span data-stu-id="dd693-1436">In an `unchecked` context, the result is truncated by discarding any high-order bits that do not fit in the destination type.</span></span>

<span data-ttu-id="dd693-1437">Para expressões não constantes (expressões que são avaliadas em tempo de execução) que não são delimitadas por nenhuma `checked` ou `unchecked` operadores ou instruções, o contexto de verificação de estouro padrão é `unchecked`, a menos que fatores externos (como switches de compilador e configuração do ambiente de execução) chamem `checked` avaliação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1437">For non-constant expressions (expressions that are evaluated at run-time) that are not enclosed by any `checked` or `unchecked` operators or statements, the default overflow checking context is `unchecked` unless external factors (such as compiler switches and execution environment configuration) call for `checked` evaluation.</span></span>

<span data-ttu-id="dd693-1438">Para expressões constantes (expressões que podem ser totalmente avaliadas em tempo de compilação), o contexto de verificação de estouro padrão é sempre `checked`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1438">For constant expressions (expressions that can be fully evaluated at compile-time), the default overflow checking context is always `checked`.</span></span> <span data-ttu-id="dd693-1439">A menos que uma expressão constante seja inserida explicitamente em um contexto de `unchecked`, os estouros que ocorrerem durante a avaliação do tempo de compilação da expressão sempre causarão erros de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1439">Unless a constant expression is explicitly placed in an `unchecked` context, overflows that occur during the compile-time evaluation of the expression always cause compile-time errors.</span></span>

<span data-ttu-id="dd693-1440">O corpo de uma função anônima não é afetado por `checked` ou `unchecked` contextos nos quais a função anônima ocorre.</span><span class="sxs-lookup"><span data-stu-id="dd693-1440">The body of an anonymous function is not affected by `checked` or `unchecked` contexts in which the anonymous function occurs.</span></span>

<span data-ttu-id="dd693-1441">No exemplo</span><span class="sxs-lookup"><span data-stu-id="dd693-1441">In the example</span></span>
```csharp
class Test
{
    static readonly int x = 1000000;
    static readonly int y = 1000000;

    static int F() {
        return checked(x * y);      // Throws OverflowException
    }

    static int G() {
        return unchecked(x * y);    // Returns -727379968
    }

    static int H() {
        return x * y;               // Depends on default
    }
}
```
<span data-ttu-id="dd693-1442">Não são relatados erros de tempo de compilação, pois nenhuma das expressões pode ser avaliada em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1442">no compile-time errors are reported since neither of the expressions can be evaluated at compile-time.</span></span> <span data-ttu-id="dd693-1443">Em tempo de execução, o método `F` gera uma `System.OverflowException`e o método `G` retorna-727379968 (os bits inferiores 32 do resultado fora do intervalo).</span><span class="sxs-lookup"><span data-stu-id="dd693-1443">At run-time, the `F` method throws a `System.OverflowException`, and the `G` method returns -727379968 (the lower 32 bits of the out-of-range result).</span></span> <span data-ttu-id="dd693-1444">O comportamento do método `H` depende do contexto de verificação de estouro padrão para a compilação, mas é o mesmo que `F` ou o mesmo que `G`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1444">The behavior of the `H` method depends on the default overflow checking context for the compilation, but it is either the same as `F` or the same as `G`.</span></span>

<span data-ttu-id="dd693-1445">No exemplo</span><span class="sxs-lookup"><span data-stu-id="dd693-1445">In the example</span></span>
```csharp
class Test
{
    const int x = 1000000;
    const int y = 1000000;

    static int F() {
        return checked(x * y);      // Compile error, overflow
    }

    static int G() {
        return unchecked(x * y);    // Returns -727379968
    }

    static int H() {
        return x * y;               // Compile error, overflow
    }
}
```
<span data-ttu-id="dd693-1446">os estouros que ocorrem ao avaliar as expressões constantes em `F` e `H` causam erros de tempo de compilação a serem relatados porque as expressões são avaliadas em um contexto de `checked`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1446">the overflows that occur when evaluating the constant expressions in `F` and `H` cause compile-time errors to be reported because the expressions are evaluated in a `checked` context.</span></span> <span data-ttu-id="dd693-1447">Um estouro também ocorre ao avaliar a expressão constante em `G`, mas como a avaliação ocorre em um contexto de `unchecked`, o estouro não é relatado.</span><span class="sxs-lookup"><span data-stu-id="dd693-1447">An overflow also occurs when evaluating the constant expression in `G`, but since the evaluation takes place in an `unchecked` context, the overflow is not reported.</span></span>

<span data-ttu-id="dd693-1448">Os operadores `checked` e `unchecked` afetam apenas o contexto de verificação de estouro para as operações que estão contidas nos tokens "`(`" e "`)`".</span><span class="sxs-lookup"><span data-stu-id="dd693-1448">The `checked` and `unchecked` operators only affect the overflow checking context for those operations that are textually contained within the "`(`" and "`)`" tokens.</span></span> <span data-ttu-id="dd693-1449">Os operadores não têm nenhum efeito nos membros da função que são invocados como resultado da avaliação da expressão contida.</span><span class="sxs-lookup"><span data-stu-id="dd693-1449">The operators have no effect on function members that are invoked as a result of evaluating the contained expression.</span></span> <span data-ttu-id="dd693-1450">No exemplo</span><span class="sxs-lookup"><span data-stu-id="dd693-1450">In the example</span></span>
```csharp
class Test
{
    static int Multiply(int x, int y) {
        return x * y;
    }

    static int F() {
        return checked(Multiply(1000000, 1000000));
    }
}
```
<span data-ttu-id="dd693-1451">o uso de `checked` no `F` não afeta a avaliação de `x * y` no `Multiply`, portanto, `x * y` é avaliado no contexto de verificação de estouro padrão.</span><span class="sxs-lookup"><span data-stu-id="dd693-1451">the use of `checked` in `F` does not affect the evaluation of `x * y` in `Multiply`, so `x * y` is evaluated in the default overflow checking context.</span></span>

<span data-ttu-id="dd693-1452">O operador de `unchecked` é conveniente ao escrever constantes dos tipos integrais assinados em notação hexadecimal.</span><span class="sxs-lookup"><span data-stu-id="dd693-1452">The `unchecked` operator is convenient when writing constants of the signed integral types in hexadecimal notation.</span></span> <span data-ttu-id="dd693-1453">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="dd693-1453">For example:</span></span>
```csharp
class Test
{
    public const int AllBits = unchecked((int)0xFFFFFFFF);

    public const int HighBit = unchecked((int)0x80000000);
}
```

<span data-ttu-id="dd693-1454">Ambas as constantes hexadecimais acima são do tipo `uint`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1454">Both of the hexadecimal constants above are of type `uint`.</span></span> <span data-ttu-id="dd693-1455">Como as constantes estão fora do intervalo de `int`, sem o operador `unchecked`, as conversões para `int` produzirão erros em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1455">Because the constants are outside the `int` range, without the `unchecked` operator, the casts to `int` would produce compile-time errors.</span></span>

<span data-ttu-id="dd693-1456">Os operadores e as instruções `checked` e `unchecked` permitem que os programadores controlem determinados aspectos de alguns cálculos numéricos.</span><span class="sxs-lookup"><span data-stu-id="dd693-1456">The `checked` and `unchecked` operators and statements allow programmers to control certain aspects of some numeric calculations.</span></span> <span data-ttu-id="dd693-1457">No entanto, o comportamento de alguns operadores numéricos depende dos tipos de dados dos operandos.</span><span class="sxs-lookup"><span data-stu-id="dd693-1457">However, the behavior of some numeric operators depends on their operands' data types.</span></span> <span data-ttu-id="dd693-1458">Por exemplo, multiplicar dois decimais sempre resulta em uma exceção no estouro mesmo dentro de uma construção de `unchecked` explicitamente.</span><span class="sxs-lookup"><span data-stu-id="dd693-1458">For example, multiplying two decimals always results in an exception on overflow even within an explicitly `unchecked` construct.</span></span> <span data-ttu-id="dd693-1459">Da mesma forma, multiplicar dois floats nunca resulta em uma exceção no estouro mesmo dentro de uma construção de `checked` explicitamente.</span><span class="sxs-lookup"><span data-stu-id="dd693-1459">Similarly, multiplying two floats never results in an exception on overflow even within an explicitly `checked` construct.</span></span> <span data-ttu-id="dd693-1460">Além disso, outros operadores nunca são afetados pelo modo de verificação, sejam eles padrão ou explícitos.</span><span class="sxs-lookup"><span data-stu-id="dd693-1460">In addition, other operators are never affected by the mode of checking, whether default or explicit.</span></span>

### <a name="default-value-expressions"></a><span data-ttu-id="dd693-1461">Expressões de valor padrão</span><span class="sxs-lookup"><span data-stu-id="dd693-1461">Default value expressions</span></span>

<span data-ttu-id="dd693-1462">Uma expressão de valor padrão é usada para obter o valor padrão ([valores padrão](variables.md#default-values)) de um tipo.</span><span class="sxs-lookup"><span data-stu-id="dd693-1462">A default value expression is used to obtain the default value ([Default values](variables.md#default-values)) of a type.</span></span> <span data-ttu-id="dd693-1463">Normalmente, uma expressão de valor padrão é usada para parâmetros de tipo, pois ela poderá não ser conhecida se o parâmetro de tipo for um tipo de valor ou de referência.</span><span class="sxs-lookup"><span data-stu-id="dd693-1463">Typically a default value expression is used for type parameters, since it may not be known if the type parameter is a value type or a reference type.</span></span> <span data-ttu-id="dd693-1464">(Não existe nenhuma conversão do `null` literal para um parâmetro de tipo, a menos que o parâmetro de tipo seja conhecido como um tipo de referência.)</span><span class="sxs-lookup"><span data-stu-id="dd693-1464">(No conversion exists from the `null` literal to a type parameter unless the type parameter is known to be a reference type.)</span></span>

```antlr
default_value_expression
    : 'default' '(' type ')'
    ;
```

<span data-ttu-id="dd693-1465">Se o *tipo* em um *default_value_expression* for avaliado em tempo de execução para um tipo de referência, o resultado será `null` convertido nesse tipo.</span><span class="sxs-lookup"><span data-stu-id="dd693-1465">If the *type* in a *default_value_expression* evaluates at run-time to a reference type, the result is `null` converted to that type.</span></span> <span data-ttu-id="dd693-1466">Se o *tipo* em um *default_value_expression* for avaliado em tempo de execução para um tipo de valor, o resultado será o valor padrão do *value_type*([construtores padrão](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="dd693-1466">If the *type* in a *default_value_expression* evaluates at run-time to a value type, the result is the *value_type*'s default value ([Default constructors](types.md#default-constructors)).</span></span>

<span data-ttu-id="dd693-1467">Uma *default_value_expression* é uma expressão constante ([expressões constantes](expressions.md#constant-expressions)) se o tipo é um tipo de referência ou um parâmetro de tipo que é conhecido como um tipo de referência ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="dd693-1467">A *default_value_expression* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) if the type is a reference type or a type parameter that is known to be a reference type ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="dd693-1468">Além disso, um *default_value_expression* é uma expressão constante se o tipo é um dos seguintes tipos de valor: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`ou qualquer tipo de enumeração.</span><span class="sxs-lookup"><span data-stu-id="dd693-1468">In addition, a *default_value_expression* is a constant expression if the type is one of the following value types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, or any enumeration type.</span></span>


### <a name="nameof-expressions"></a><span data-ttu-id="dd693-1469">Expressões nameof</span><span class="sxs-lookup"><span data-stu-id="dd693-1469">Nameof expressions</span></span>

<span data-ttu-id="dd693-1470">Um *nameof_expression* é usado para obter o nome de uma entidade de programa como uma cadeia de caracteres constante.</span><span class="sxs-lookup"><span data-stu-id="dd693-1470">A *nameof_expression* is used to obtain the name of a program entity as a constant string.</span></span>

```antlr
nameof_expression
    : 'nameof' '(' named_entity ')'
    ;

named_entity
    : simple_name
    | named_entity_target '.' identifier type_argument_list?
    ;

named_entity_target
    : 'this'
    | 'base'
    | named_entity 
    | predefined_type 
    | qualified_alias_member
    ;
```

<span data-ttu-id="dd693-1471">Em termos de gramática, o operando *named_entity* sempre é uma expressão.</span><span class="sxs-lookup"><span data-stu-id="dd693-1471">Grammatically speaking, the *named_entity* operand is always an expression.</span></span> <span data-ttu-id="dd693-1472">Como `nameof` não é uma palavra-chave reservada, uma expressão nameof é sempre sintaticamente ambígua com uma invocação do nome simples `nameof`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1472">Because `nameof` is not a reserved keyword, a nameof expression is always syntactically ambiguous with an invocation of the simple name `nameof`.</span></span> <span data-ttu-id="dd693-1473">Por motivos de compatibilidade, se uma pesquisa de nome ([nomes simples](expressions.md#simple-names)) do nome `nameof` tiver sucesso, a expressão será tratada como uma *invocation_expression* , independentemente de a invocação ser legal ou não.</span><span class="sxs-lookup"><span data-stu-id="dd693-1473">For compatibility reasons, if a name lookup ([Simple names](expressions.md#simple-names)) of the name `nameof` succeeds, the expression is treated as an *invocation_expression* -- regardless of whether the invocation is legal.</span></span> <span data-ttu-id="dd693-1474">Caso contrário, é um *nameof_expression*.</span><span class="sxs-lookup"><span data-stu-id="dd693-1474">Otherwise it is a *nameof_expression*.</span></span>

<span data-ttu-id="dd693-1475">O significado da *named_entity* de uma *nameof_expression* é o significado dela como uma expressão; ou seja, como um *Simple_name*, um *base_access* ou um *member_access*.</span><span class="sxs-lookup"><span data-stu-id="dd693-1475">The meaning of the *named_entity* of a *nameof_expression* is the meaning of it as an expression; that is, either as a *simple_name*, a *base_access* or a *member_access*.</span></span> <span data-ttu-id="dd693-1476">No entanto, onde a pesquisa descrita em [nomes simples](expressions.md#simple-names) e [acesso de membro](expressions.md#member-access) resulta em um erro porque um membro de instância foi encontrado em um contexto estático, um *nameof_expression* não produz esse erro.</span><span class="sxs-lookup"><span data-stu-id="dd693-1476">However, where the lookup described in [Simple names](expressions.md#simple-names) and [Member access](expressions.md#member-access) results in an error because an instance member was found in a static context, a *nameof_expression* produces no such error.</span></span>

<span data-ttu-id="dd693-1477">É um erro de tempo de compilação para um *named_entity* designar um grupo de métodos para ter um *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="dd693-1477">It is a compile-time error for a *named_entity* designating a method group to have a *type_argument_list*.</span></span> <span data-ttu-id="dd693-1478">É um erro de tempo de compilação para um *named_entity_target* ter o tipo `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1478">It is a compile time error for a *named_entity_target* to have the type `dynamic`.</span></span>

<span data-ttu-id="dd693-1479">Uma *nameof_expression* é uma expressão constante do tipo `string`e não tem nenhum efeito no tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="dd693-1479">A *nameof_expression* is a constant expression of type `string`, and has no effect at runtime.</span></span> <span data-ttu-id="dd693-1480">Especificamente, sua *named_entity* não é avaliada e é ignorada para fins de análise de atribuição definitiva ([regras gerais para expressões simples](variables.md#general-rules-for-simple-expressions)).</span><span class="sxs-lookup"><span data-stu-id="dd693-1480">Specifically, its *named_entity* is not evaluated, and is ignored for the purposes of definite assignment analysis ([General rules for simple expressions](variables.md#general-rules-for-simple-expressions)).</span></span> <span data-ttu-id="dd693-1481">Seu valor é o último identificador da *named_entity* antes da *type_argument_list*final opcional, transformada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="dd693-1481">Its value is the last identifier of the *named_entity* before the optional final *type_argument_list*, transformed in the following way:</span></span>

* <span data-ttu-id="dd693-1482">O prefixo "`@`", se usado, será removido.</span><span class="sxs-lookup"><span data-stu-id="dd693-1482">The prefix "`@`", if used, is removed.</span></span>
* <span data-ttu-id="dd693-1483">Cada *unicode_escape_sequence* é transformada em seu caractere Unicode correspondente.</span><span class="sxs-lookup"><span data-stu-id="dd693-1483">Each *unicode_escape_sequence* is transformed into its corresponding Unicode character.</span></span>
* <span data-ttu-id="dd693-1484">Todas as *formatting_characters* são removidas.</span><span class="sxs-lookup"><span data-stu-id="dd693-1484">Any *formatting_characters* are removed.</span></span>

<span data-ttu-id="dd693-1485">Essas são as mesmas transformações aplicadas em [identificadores](lexical-structure.md#identifiers) ao testar a igualdade entre identificadores.</span><span class="sxs-lookup"><span data-stu-id="dd693-1485">These are the same transformations applied in [Identifiers](lexical-structure.md#identifiers) when testing equality between identifiers.</span></span>

<span data-ttu-id="dd693-1486">TODO: exemplos</span><span class="sxs-lookup"><span data-stu-id="dd693-1486">TODO: examples</span></span>

### <a name="anonymous-method-expressions"></a><span data-ttu-id="dd693-1487">Expressões de método anônimo</span><span class="sxs-lookup"><span data-stu-id="dd693-1487">Anonymous method expressions</span></span>

<span data-ttu-id="dd693-1488">Uma *anonymous_method_expression* é uma das duas maneiras de definir uma função anônima.</span><span class="sxs-lookup"><span data-stu-id="dd693-1488">An *anonymous_method_expression* is one of two ways of defining an anonymous function.</span></span> <span data-ttu-id="dd693-1489">Eles são descritos mais detalhadamente em [expressões de função anônimas](expressions.md#anonymous-function-expressions).</span><span class="sxs-lookup"><span data-stu-id="dd693-1489">These are further described in [Anonymous function expressions](expressions.md#anonymous-function-expressions).</span></span>

## <a name="unary-operators"></a><span data-ttu-id="dd693-1490">Operadores unários</span><span class="sxs-lookup"><span data-stu-id="dd693-1490">Unary operators</span></span>

<span data-ttu-id="dd693-1491">Os operadores `?`, `+`, `-`, `!`, `~`, `++`, `--`, CAST e `await` são chamados de operadores unários.</span><span class="sxs-lookup"><span data-stu-id="dd693-1491">The `?`, `+`, `-`, `!`, `~`, `++`, `--`, cast, and `await` operators are called the unary operators.</span></span>

```antlr
unary_expression
    : primary_expression
    | null_conditional_expression
    | '+' unary_expression
    | '-' unary_expression
    | '!' unary_expression
    | '~' unary_expression
    | pre_increment_expression
    | pre_decrement_expression
    | cast_expression
    | await_expression
    | unary_expression_unsafe
    ;
```

<span data-ttu-id="dd693-1492">Se o operando de um *unary_expression* tiver o tipo de tempo de compilação `dynamic`, ele será vinculado dinamicamente ([associação dinâmica](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="dd693-1492">If the operand of a *unary_expression* has the compile-time type `dynamic`, it is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="dd693-1493">Nesse caso, o tipo de tempo de compilação do *unary_expression* é `dynamic`e a resolução descrita abaixo ocorrerá em tempo de execução usando o tipo de tempo de execução do operando.</span><span class="sxs-lookup"><span data-stu-id="dd693-1493">In this case the compile-time type of the *unary_expression* is `dynamic`, and the resolution described below will take place at run-time using the run-time type of the operand.</span></span>

### <a name="null-conditional-operator"></a><span data-ttu-id="dd693-1494">Operador NULL-condicional</span><span class="sxs-lookup"><span data-stu-id="dd693-1494">Null-conditional operator</span></span>

<span data-ttu-id="dd693-1495">O operador NULL-Conditional aplica uma lista de operações ao operando somente se esse operando não for nulo.</span><span class="sxs-lookup"><span data-stu-id="dd693-1495">The null-conditional operator applies a list of operations to its operand only if that operand is non-null.</span></span> <span data-ttu-id="dd693-1496">Caso contrário, o resultado da aplicação do operador será `null`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1496">Otherwise the result of applying the operator is `null`.</span></span>

```antlr
null_conditional_expression
    : primary_expression null_conditional_operations
    ;

null_conditional_operations
    : null_conditional_operations? '?' '.' identifier type_argument_list?
    | null_conditional_operations? '?' '[' argument_list ']'
    | null_conditional_operations '.' identifier type_argument_list?
    | null_conditional_operations '[' argument_list ']'
    | null_conditional_operations '(' argument_list? ')'
    ;
```

<span data-ttu-id="dd693-1497">A lista de operações pode incluir acesso a membros e operações de acesso de elemento (que podem ser nulas-condicionais), bem como invocação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1497">The list of operations can include member access and element access operations (which may themselves be null-conditional), as well as invocation.</span></span>

<span data-ttu-id="dd693-1498">Por exemplo, a expressão `a.b?[0]?.c()` é uma *null_conditional_expression* com uma *primary_expression* `a.b` e *null_conditional_operations* `?[0]` (acesso a elemento condicional nulo), `?.c` (acesso de membro condicional nulo) e `()` (invocação).</span><span class="sxs-lookup"><span data-stu-id="dd693-1498">For example, the expression `a.b?[0]?.c()` is a *null_conditional_expression* with a *primary_expression* `a.b` and *null_conditional_operations* `?[0]` (null-conditional element access), `?.c` (null-conditional member access) and `()` (invocation).</span></span>

<span data-ttu-id="dd693-1499">Para um *null_conditional_expression* `E` com um `P`de *primary_expression* , permita que `E0` seja a expressão obtida com a remoção textual da `?` à esquerda de cada *null_conditional_operations* de `E` que tenha um.</span><span class="sxs-lookup"><span data-stu-id="dd693-1499">For a *null_conditional_expression* `E` with a *primary_expression* `P`, let `E0` be the expression obtained by textually removing the leading `?` from each of the *null_conditional_operations* of `E` that have one.</span></span> <span data-ttu-id="dd693-1500">Conceitualmente, `E0` é a expressão que será avaliada se nenhuma das verificações nulas representadas pelo `?`s encontrar uma `null`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1500">Conceptually, `E0` is the expression that will be evaluated if none of the null checks represented by the `?`s do find a `null`.</span></span>

<span data-ttu-id="dd693-1501">Além disso, permita que `E1` seja a expressão obtida com a remoção textual da `?` à esquerda apenas da primeira das *null_conditional_operations* no `E`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1501">Also, let `E1` be the expression obtained by textually removing the leading `?` from just the first of the *null_conditional_operations* in `E`.</span></span> <span data-ttu-id="dd693-1502">Isso pode levar a uma *expressão primária* (se houver apenas uma `?`) ou a outra *null_conditional_expression*.</span><span class="sxs-lookup"><span data-stu-id="dd693-1502">This may lead to a *primary-expression* (if there was just one `?`) or to another *null_conditional_expression*.</span></span>

<span data-ttu-id="dd693-1503">Por exemplo, se `E` for a expressão `a.b?[0]?.c()`, `E0` será a expressão `a.b[0].c()` e `E1` será a `a.b[0]?.c()`de expressão.</span><span class="sxs-lookup"><span data-stu-id="dd693-1503">For example, if `E` is the expression `a.b?[0]?.c()`, then `E0` is the expression `a.b[0].c()` and `E1` is the expression `a.b[0]?.c()`.</span></span>

<span data-ttu-id="dd693-1504">Se `E0` for classificada como Nothing, `E` será classificada como Nothing.</span><span class="sxs-lookup"><span data-stu-id="dd693-1504">If `E0` is classified as nothing, then `E` is classified as nothing.</span></span> <span data-ttu-id="dd693-1505">Caso contrário, E será classificado como um valor.</span><span class="sxs-lookup"><span data-stu-id="dd693-1505">Otherwise E is classified as a value.</span></span>

<span data-ttu-id="dd693-1506">`E0` e `E1` são usados para determinar o significado de `E`:</span><span class="sxs-lookup"><span data-stu-id="dd693-1506">`E0` and `E1` are used to determine the meaning of `E`:</span></span>

*  <span data-ttu-id="dd693-1507">Se `E` ocorrer como um *statement_expression* o significado de `E` será o mesmo que a instrução</span><span class="sxs-lookup"><span data-stu-id="dd693-1507">If `E` occurs as a *statement_expression* the meaning of `E` is the same as the statement</span></span>

   ```csharp
   if ((object)P != null) E1;
   ```

   <span data-ttu-id="dd693-1508">Exceto que P é avaliada apenas uma vez.</span><span class="sxs-lookup"><span data-stu-id="dd693-1508">except that P is evaluated only once.</span></span>

*  <span data-ttu-id="dd693-1509">Caso contrário, se `E0` for classificado como nada ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1509">Otherwise, if `E0` is classified as nothing a compile-time error occurs.</span></span>

*  <span data-ttu-id="dd693-1510">Caso contrário, permita que `T0` seja o tipo de `E0`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1510">Otherwise, let `T0` be the type of `E0`.</span></span>

   *  <span data-ttu-id="dd693-1511">Se `T0` for um parâmetro de tipo que não é conhecido como um tipo de referência ou um tipo de valor não anulável, ocorrerá um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1511">If `T0` is a type parameter that is not known to be a reference type or a non-nullable value type, a compile-time error occurs.</span></span>

   *  <span data-ttu-id="dd693-1512">Se `T0` for um tipo de valor não anulável, o tipo de `E` será `T0?`e o significado de `E` será o mesmo que</span><span class="sxs-lookup"><span data-stu-id="dd693-1512">If `T0` is a non-nullable value type, then the type of `E` is `T0?`, and the meaning of `E` is the same as</span></span>

      ```csharp
      ((object)P == null) ? (T0?)null : E1
      ```

      <span data-ttu-id="dd693-1513">Exceto que `P` é avaliada apenas uma vez.</span><span class="sxs-lookup"><span data-stu-id="dd693-1513">except that `P` is evaluated only once.</span></span>

   *  <span data-ttu-id="dd693-1514">Caso contrário, o tipo de E é T0, e o significado de E é o mesmo que</span><span class="sxs-lookup"><span data-stu-id="dd693-1514">Otherwise the type of E is T0, and the meaning of E is the same as</span></span>

      ```csharp
      ((object)P == null) ? null : E1
      ```

      <span data-ttu-id="dd693-1515">Exceto que `P` é avaliada apenas uma vez.</span><span class="sxs-lookup"><span data-stu-id="dd693-1515">except that `P` is evaluated only once.</span></span>

<span data-ttu-id="dd693-1516">Se `E1` for um *null_conditional_expression*, essas regras serão aplicadas novamente, aninhando os testes para `null` até que não haja mais `?`, e a expressão foi reduzida até a `E0`de expressão primária...</span><span class="sxs-lookup"><span data-stu-id="dd693-1516">If `E1` is itself a *null_conditional_expression*, then these rules are applied again, nesting the tests for `null` until there are no further `?`'s, and the expression has been reduced all the way down to the primary-expression `E0`.</span></span>

<span data-ttu-id="dd693-1517">Por exemplo, se a expressão `a.b?[0]?.c()` ocorrer como uma expressão de instrução, como na instrução:</span><span class="sxs-lookup"><span data-stu-id="dd693-1517">For example, if the expression `a.b?[0]?.c()` occurs as a statement-expression, as in the statement:</span></span>
```csharp
a.b?[0]?.c();
```
<span data-ttu-id="dd693-1518">seu significado é equivalente a:</span><span class="sxs-lookup"><span data-stu-id="dd693-1518">its meaning is equivalent to:</span></span>
```csharp
if (a.b != null) a.b[0]?.c();
```
<span data-ttu-id="dd693-1519">que novamente é equivalente a:</span><span class="sxs-lookup"><span data-stu-id="dd693-1519">which again is equivalent to:</span></span>
```csharp
if (a.b != null) if (a.b[0] != null) a.b[0].c();
```
<span data-ttu-id="dd693-1520">Exceto que `a.b` e `a.b[0]` são avaliadas apenas uma vez.</span><span class="sxs-lookup"><span data-stu-id="dd693-1520">Except that `a.b` and `a.b[0]` are evaluated only once.</span></span>

<span data-ttu-id="dd693-1521">Se ocorrer em um contexto em que seu valor é usado, como em:</span><span class="sxs-lookup"><span data-stu-id="dd693-1521">If it occurs in a context where its value is used, as in:</span></span>
```csharp
var x = a.b?[0]?.c();
```
<span data-ttu-id="dd693-1522">e supondo que o tipo da invocação final não seja um tipo de valor não anulável, seu significado é equivalente a:</span><span class="sxs-lookup"><span data-stu-id="dd693-1522">and assuming that the type of the final invocation is not a non-nullable value type, its meaning is equivalent to:</span></span>
```csharp
var x = (a.b == null) ? null : (a.b[0] == null) ? null : a.b[0].c();
```
<span data-ttu-id="dd693-1523">Exceto que `a.b` e `a.b[0]` são avaliadas apenas uma vez.</span><span class="sxs-lookup"><span data-stu-id="dd693-1523">except that `a.b` and `a.b[0]` are evaluated only once.</span></span>

#### <a name="null-conditional-expressions-as-projection-initializers"></a><span data-ttu-id="dd693-1524">Expressões condicionais nulas como inicializadores de projeção</span><span class="sxs-lookup"><span data-stu-id="dd693-1524">Null-conditional expressions as projection initializers</span></span>

<span data-ttu-id="dd693-1525">Uma expressão condicional nula só é permitida como uma *member_declarator* em uma *anonymous_object_creation_expression* (expressões de[criação de objeto anônimo](expressions.md#anonymous-object-creation-expressions)) se terminar com um acesso de membro (opcionalmente nulo).</span><span class="sxs-lookup"><span data-stu-id="dd693-1525">A null-conditional expression is only allowed as a *member_declarator* in an *anonymous_object_creation_expression* ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) if it ends with an (optionally null-conditional) member access.</span></span> <span data-ttu-id="dd693-1526">Na gramática, esse requisito pode ser expresso como:</span><span class="sxs-lookup"><span data-stu-id="dd693-1526">Grammatically, this requirement can be expressed as:</span></span>

```antlr
null_conditional_member_access
    : primary_expression null_conditional_operations? '?' '.' identifier type_argument_list?
    | primary_expression null_conditional_operations '.' identifier type_argument_list?
    ;
```

<span data-ttu-id="dd693-1527">Este é um caso especial da gramática para *null_conditional_expression* acima.</span><span class="sxs-lookup"><span data-stu-id="dd693-1527">This is a special case of the grammar for *null_conditional_expression* above.</span></span> <span data-ttu-id="dd693-1528">A produção para *member_declarator* em [expressões de criação de objeto anônimo](expressions.md#anonymous-object-creation-expressions) inclui apenas *null_conditional_member_access*.</span><span class="sxs-lookup"><span data-stu-id="dd693-1528">The production for *member_declarator* in [Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions) then includes only *null_conditional_member_access*.</span></span>

#### <a name="null-conditional-expressions-as-statement-expressions"></a><span data-ttu-id="dd693-1529">Expressões condicionais nulas como expressões de instrução</span><span class="sxs-lookup"><span data-stu-id="dd693-1529">Null-conditional expressions as statement expressions</span></span>

<span data-ttu-id="dd693-1530">Uma expressão condicional nula só é permitida como uma *statement_expression* ([instruções de expressão](statements.md#expression-statements)) se terminar com uma invocação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1530">A null-conditional expression is only allowed as a *statement_expression* ([Expression statements](statements.md#expression-statements)) if it ends with an invocation.</span></span> <span data-ttu-id="dd693-1531">Na gramática, esse requisito pode ser expresso como:</span><span class="sxs-lookup"><span data-stu-id="dd693-1531">Grammatically, this requirement can be expressed as:</span></span>

```antlr
null_conditional_invocation_expression
    : primary_expression null_conditional_operations '(' argument_list? ')'
    ;
```

<span data-ttu-id="dd693-1532">Este é um caso especial da gramática para *null_conditional_expression* acima.</span><span class="sxs-lookup"><span data-stu-id="dd693-1532">This is a special case of the grammar for *null_conditional_expression* above.</span></span> <span data-ttu-id="dd693-1533">A produção para *statement_expression* em [instruções de expressão](statements.md#expression-statements) , em seguida, inclui apenas *null_conditional_invocation_expression*.</span><span class="sxs-lookup"><span data-stu-id="dd693-1533">The production for *statement_expression* in [Expression statements](statements.md#expression-statements) then includes only *null_conditional_invocation_expression*.</span></span>


### <a name="unary-plus-operator"></a><span data-ttu-id="dd693-1534">Operador unário de adição</span><span class="sxs-lookup"><span data-stu-id="dd693-1534">Unary plus operator</span></span>

<span data-ttu-id="dd693-1535">Para uma operação do formulário `+x`, a resolução de sobrecarga de operador unário ([resolução de sobrecarga de operador unário](expressions.md#unary-operator-overload-resolution)) é aplicada para selecionar uma implementação de operador específica.</span><span class="sxs-lookup"><span data-stu-id="dd693-1535">For an operation of the form `+x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="dd693-1536">O operando é convertido para o tipo de parâmetro do operador selecionado e o tipo de resultado é o tipo de retorno do operador.</span><span class="sxs-lookup"><span data-stu-id="dd693-1536">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="dd693-1537">Os operadores de adição de unários predefinidos são:</span><span class="sxs-lookup"><span data-stu-id="dd693-1537">The predefined unary plus operators are:</span></span>

```csharp
int operator +(int x);
uint operator +(uint x);
long operator +(long x);
ulong operator +(ulong x);
float operator +(float x);
double operator +(double x);
decimal operator +(decimal x);
```

<span data-ttu-id="dd693-1538">Para cada um desses operadores, o resultado é simplesmente o valor do operando.</span><span class="sxs-lookup"><span data-stu-id="dd693-1538">For each of these operators, the result is simply the value of the operand.</span></span>

### <a name="unary-minus-operator"></a><span data-ttu-id="dd693-1539">Operador unário de subtração</span><span class="sxs-lookup"><span data-stu-id="dd693-1539">Unary minus operator</span></span>

<span data-ttu-id="dd693-1540">Para uma operação do formulário `-x`, a resolução de sobrecarga de operador unário ([resolução de sobrecarga de operador unário](expressions.md#unary-operator-overload-resolution)) é aplicada para selecionar uma implementação de operador específica.</span><span class="sxs-lookup"><span data-stu-id="dd693-1540">For an operation of the form `-x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="dd693-1541">O operando é convertido para o tipo de parâmetro do operador selecionado e o tipo de resultado é o tipo de retorno do operador.</span><span class="sxs-lookup"><span data-stu-id="dd693-1541">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="dd693-1542">Os operadores de negação predefinidos são:</span><span class="sxs-lookup"><span data-stu-id="dd693-1542">The predefined negation operators are:</span></span>

*  <span data-ttu-id="dd693-1543">Negação de inteiro:</span><span class="sxs-lookup"><span data-stu-id="dd693-1543">Integer negation:</span></span>

   ```csharp
   int operator -(int x);
   long operator -(long x);
   ```

   <span data-ttu-id="dd693-1544">O resultado é calculado com a subtração de `x` de zero.</span><span class="sxs-lookup"><span data-stu-id="dd693-1544">The result is computed by subtracting `x` from zero.</span></span> <span data-ttu-id="dd693-1545">Se o valor de `x` for o menor valor representável do tipo de operando (-2 ^ 31 para `int` ou-2 ^ 63 para `long`), a negação matemática de `x` não será representável dentro do tipo de operando.</span><span class="sxs-lookup"><span data-stu-id="dd693-1545">If the value of `x` is the smallest representable value of the operand type (-2^31 for `int` or -2^63 for `long`), then the mathematical negation of `x` is not representable within the operand type.</span></span> <span data-ttu-id="dd693-1546">Se isso ocorrer em um contexto de `checked`, um `System.OverflowException` será gerado; Se ocorrer dentro de um contexto de `unchecked`, o resultado será o valor do operando e o estouro não será relatado.</span><span class="sxs-lookup"><span data-stu-id="dd693-1546">If this occurs within a `checked` context, a `System.OverflowException` is thrown; if it occurs within an `unchecked` context, the result is the value of the operand and the overflow is not reported.</span></span>

   <span data-ttu-id="dd693-1547">Se o operando do operador de negação for do tipo `uint`, ele será convertido no tipo `long`e o tipo do resultado será `long`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1547">If the operand of the negation operator is of type `uint`, it is converted to type `long`, and the type of the result is `long`.</span></span> <span data-ttu-id="dd693-1548">Uma exceção é a regra que permite que o valor de `int`-2147483648 (-2 ^ 31) seja gravado como um literal inteiro decimal ([literais inteiros](lexical-structure.md#integer-literals)).</span><span class="sxs-lookup"><span data-stu-id="dd693-1548">An exception is the rule that permits the `int` value -2147483648 (-2^31) to be written as a decimal integer literal ([Integer literals](lexical-structure.md#integer-literals)).</span></span>

   <span data-ttu-id="dd693-1549">Se o operando do operador de negação for do tipo `ulong`, ocorrerá um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1549">If the operand of the negation operator is of type `ulong`, a compile-time error occurs.</span></span> <span data-ttu-id="dd693-1550">Uma exceção é a regra que permite que o `long` valor-9.223.372.036.854.775.808 (-2 ^ 63) seja gravado como um literal inteiro decimal ([literais inteiros](lexical-structure.md#integer-literals)).</span><span class="sxs-lookup"><span data-stu-id="dd693-1550">An exception is the rule that permits the `long` value -9223372036854775808 (-2^63) to be written as a decimal integer literal ([Integer literals](lexical-structure.md#integer-literals)).</span></span>

*  <span data-ttu-id="dd693-1551">Negação de ponto flutuante:</span><span class="sxs-lookup"><span data-stu-id="dd693-1551">Floating-point negation:</span></span>

   ```csharp
   float operator -(float x);
   double operator -(double x);
   ```

   <span data-ttu-id="dd693-1552">O resultado é o valor de `x` com seu sinal invertido.</span><span class="sxs-lookup"><span data-stu-id="dd693-1552">The result is the value of `x` with its sign inverted.</span></span> <span data-ttu-id="dd693-1553">Se `x` for NaN, o resultado também será NaN.</span><span class="sxs-lookup"><span data-stu-id="dd693-1553">If `x` is NaN, the result is also NaN.</span></span>

*  <span data-ttu-id="dd693-1554">Negação decimal:</span><span class="sxs-lookup"><span data-stu-id="dd693-1554">Decimal negation:</span></span>

   ```csharp
   decimal operator -(decimal x);
   ```

   <span data-ttu-id="dd693-1555">O resultado é calculado com a subtração de `x` de zero.</span><span class="sxs-lookup"><span data-stu-id="dd693-1555">The result is computed by subtracting `x` from zero.</span></span> <span data-ttu-id="dd693-1556">A negação decimal é equivalente a usar o operador de subtração unário do tipo `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1556">Decimal negation is equivalent to using the unary minus operator of type `System.Decimal`.</span></span>

### <a name="logical-negation-operator"></a><span data-ttu-id="dd693-1557">Operador de negação lógico</span><span class="sxs-lookup"><span data-stu-id="dd693-1557">Logical negation operator</span></span>

<span data-ttu-id="dd693-1558">Para uma operação do formulário `!x`, a resolução de sobrecarga de operador unário ([resolução de sobrecarga de operador unário](expressions.md#unary-operator-overload-resolution)) é aplicada para selecionar uma implementação de operador específica.</span><span class="sxs-lookup"><span data-stu-id="dd693-1558">For an operation of the form `!x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="dd693-1559">O operando é convertido para o tipo de parâmetro do operador selecionado e o tipo de resultado é o tipo de retorno do operador.</span><span class="sxs-lookup"><span data-stu-id="dd693-1559">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="dd693-1560">Existe apenas um operador de negação lógica predefinido:</span><span class="sxs-lookup"><span data-stu-id="dd693-1560">Only one predefined logical negation operator exists:</span></span>
```csharp
bool operator !(bool x);
```

<span data-ttu-id="dd693-1561">Esse operador computa a negação lógica do operando: se o operando for `true`, o resultado será `false`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1561">This operator computes the logical negation of the operand: If the operand is `true`, the result is `false`.</span></span> <span data-ttu-id="dd693-1562">Se o operando for `false`, o resultado será `true`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1562">If the operand is `false`, the result is `true`.</span></span>

### <a name="bitwise-complement-operator"></a><span data-ttu-id="dd693-1563">Operador de complemento de bits</span><span class="sxs-lookup"><span data-stu-id="dd693-1563">Bitwise complement operator</span></span>

<span data-ttu-id="dd693-1564">Para uma operação do formulário `~x`, a resolução de sobrecarga de operador unário ([resolução de sobrecarga de operador unário](expressions.md#unary-operator-overload-resolution)) é aplicada para selecionar uma implementação de operador específica.</span><span class="sxs-lookup"><span data-stu-id="dd693-1564">For an operation of the form `~x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="dd693-1565">O operando é convertido para o tipo de parâmetro do operador selecionado e o tipo de resultado é o tipo de retorno do operador.</span><span class="sxs-lookup"><span data-stu-id="dd693-1565">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="dd693-1566">Os operadores predefinidos de Complementos de bits são:</span><span class="sxs-lookup"><span data-stu-id="dd693-1566">The predefined bitwise complement operators are:</span></span>
```csharp
int operator ~(int x);
uint operator ~(uint x);
long operator ~(long x);
ulong operator ~(ulong x);
```

<span data-ttu-id="dd693-1567">Para cada um desses operadores, o resultado da operação é o complemento bit a bit de `x`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1567">For each of these operators, the result of the operation is the bitwise complement of `x`.</span></span>

<span data-ttu-id="dd693-1568">Todos os tipos de enumeração `E` fornecem implicitamente o seguinte operador de complemento bit a bit:</span><span class="sxs-lookup"><span data-stu-id="dd693-1568">Every enumeration type `E` implicitly provides the following bitwise complement operator:</span></span>

```csharp
E operator ~(E x);
```

<span data-ttu-id="dd693-1569">O resultado da avaliação de `~x`, em que `x` é uma expressão de um tipo de enumeração `E` com um tipo subjacente `U`, é exatamente o mesmo que avaliar `(E)(~(U)x)`, exceto que a conversão em `E` é sempre executada como se fosse em um contexto de `unchecked` ([os operadores marcados e desmarcados](expressions.md#the-checked-and-unchecked-operators)).</span><span class="sxs-lookup"><span data-stu-id="dd693-1569">The result of evaluating `~x`, where `x` is an expression of an enumeration type `E` with an underlying type `U`, is exactly the same as evaluating `(E)(~(U)x)`, except that the conversion to `E` is always performed as if in an `unchecked` context ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)).</span></span>

### <a name="prefix-increment-and-decrement-operators"></a><span data-ttu-id="dd693-1570">Operadores de incremento e decremento pré-fixados</span><span class="sxs-lookup"><span data-stu-id="dd693-1570">Prefix increment and decrement operators</span></span>

```antlr
pre_increment_expression
    : '++' unary_expression
    ;

pre_decrement_expression
    : '--' unary_expression
    ;
```

<span data-ttu-id="dd693-1571">O operando de uma operação de incremento ou diminuição de prefixo deve ser uma expressão classificada como uma variável, um acesso de propriedade ou um acesso de indexador.</span><span class="sxs-lookup"><span data-stu-id="dd693-1571">The operand of a prefix increment or decrement operation must be an expression classified as a variable, a property access, or an indexer access.</span></span> <span data-ttu-id="dd693-1572">O resultado da operação é um valor do mesmo tipo que o operando.</span><span class="sxs-lookup"><span data-stu-id="dd693-1572">The result of the operation is a value of the same type as the operand.</span></span>

<span data-ttu-id="dd693-1573">Se o operando de uma operação de incremento ou diminuição de prefixo for um acesso de propriedade ou indexador, a propriedade ou o indexador deverá ter um `get` e um acessador `set`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1573">If the operand of a prefix increment or decrement operation is a property or indexer access, the property or indexer must have both a `get` and a `set` accessor.</span></span> <span data-ttu-id="dd693-1574">Se esse não for o caso, ocorrerá um erro de tempo de associação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1574">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="dd693-1575">Resolução de sobrecarga de operador unário ([resolução de sobrecarga de operador unário](expressions.md#unary-operator-overload-resolution)) é aplicada para selecionar uma implementação de operador específica.</span><span class="sxs-lookup"><span data-stu-id="dd693-1575">Unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="dd693-1576">Existem `++` predefinidos e operadores de `--` para os seguintes tipos: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`e qualquer tipo de enumeração.</span><span class="sxs-lookup"><span data-stu-id="dd693-1576">Predefined `++` and `--` operators exist for the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, and any enum type.</span></span> <span data-ttu-id="dd693-1577">Os operadores de `++` predefinidos retornam o valor produzido pela adição de 1 ao operando e os operadores de `--` predefinidos retornam o valor produzido pela subtração de 1 do operando.</span><span class="sxs-lookup"><span data-stu-id="dd693-1577">The predefined `++` operators return the value produced by adding 1 to the operand, and the predefined `--` operators return the value produced by subtracting 1 from the operand.</span></span> <span data-ttu-id="dd693-1578">Em um contexto `checked`, se o resultado dessa adição ou subtração estiver fora do intervalo do tipo de resultado e o tipo de resultado for um tipo integral ou tipo de enumeração, um `System.OverflowException` será gerado.</span><span class="sxs-lookup"><span data-stu-id="dd693-1578">In a `checked` context, if the result of this addition or subtraction is outside the range of the result type and the result type is an integral type or enum type, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="dd693-1579">O processamento em tempo de execução de uma operação de incremento de prefixo ou diminuição do formulário `++x` ou `--x` consiste nas seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="dd693-1579">The run-time processing of a prefix increment or decrement operation of the form `++x` or `--x` consists of the following steps:</span></span>

*   <span data-ttu-id="dd693-1580">Se `x` for classificado como uma variável:</span><span class="sxs-lookup"><span data-stu-id="dd693-1580">If `x` is classified as a variable:</span></span>
    * <span data-ttu-id="dd693-1581">`x` é avaliado para produzir a variável.</span><span class="sxs-lookup"><span data-stu-id="dd693-1581">`x` is evaluated to produce the variable.</span></span>
    * <span data-ttu-id="dd693-1582">O operador selecionado é invocado com o valor de `x` como seu argumento.</span><span class="sxs-lookup"><span data-stu-id="dd693-1582">The selected operator is invoked with the value of `x` as its argument.</span></span>
    * <span data-ttu-id="dd693-1583">O valor retornado pelo operador é armazenado no local fornecido pela avaliação de `x`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1583">The value returned by the operator is stored in the location given by the evaluation of `x`.</span></span>
    * <span data-ttu-id="dd693-1584">O valor retornado pelo operador torna-se o resultado da operação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1584">The value returned by the operator becomes the result of the operation.</span></span>
*   <span data-ttu-id="dd693-1585">Se `x` for classificado como um acesso de propriedade ou indexador:</span><span class="sxs-lookup"><span data-stu-id="dd693-1585">If `x` is classified as a property or indexer access:</span></span>
    * <span data-ttu-id="dd693-1586">A expressão de instância (se `x` não for `static`) e a lista de argumentos (se `x` for um acesso de indexador) associada a `x` são avaliadas e os resultados são usados nas invocações subsequentes de `get` e `set`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1586">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `get` and `set` accessor invocations.</span></span>
    * <span data-ttu-id="dd693-1587">O acessador de `get` de `x` é invocado.</span><span class="sxs-lookup"><span data-stu-id="dd693-1587">The `get` accessor of `x` is invoked.</span></span>
    * <span data-ttu-id="dd693-1588">O operador selecionado é invocado com o valor retornado pelo acessador `get` como seu argumento.</span><span class="sxs-lookup"><span data-stu-id="dd693-1588">The selected operator is invoked with the value returned by the `get` accessor as its argument.</span></span>
    * <span data-ttu-id="dd693-1589">O acessador de `set` de `x` é invocado com o valor retornado pelo operador como seu argumento `value`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1589">The `set` accessor of `x` is invoked with the value returned by the operator as its `value` argument.</span></span>
    * <span data-ttu-id="dd693-1590">O valor retornado pelo operador torna-se o resultado da operação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1590">The value returned by the operator becomes the result of the operation.</span></span>

<span data-ttu-id="dd693-1591">Os operadores `++` e `--` também dão suporte à notação de sufixo ([operadores de aumento de sufixo e diminuição](expressions.md#postfix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="dd693-1591">The `++` and `--` operators also support postfix notation ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="dd693-1592">Normalmente, o resultado de `x++` ou `x--` é o valor de `x` antes da operação, enquanto o resultado de `++x` ou `--x` é o valor de `x` após a operação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1592">Typically, the result of `x++` or `x--` is the value of `x` before the operation, whereas the result of `++x` or `--x` is the value of `x` after the operation.</span></span> <span data-ttu-id="dd693-1593">Em ambos os casos, `x` ele mesmo tem o mesmo valor após a operação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1593">In either case, `x` itself has the same value after the operation.</span></span>

<span data-ttu-id="dd693-1594">Uma implementação `operator++` ou `operator--` pode ser chamada usando o sufixo ou a notação de prefixo.</span><span class="sxs-lookup"><span data-stu-id="dd693-1594">An `operator++` or `operator--` implementation can be invoked using either postfix or prefix notation.</span></span> <span data-ttu-id="dd693-1595">Não é possível ter implementações de operador separadas para as duas notações.</span><span class="sxs-lookup"><span data-stu-id="dd693-1595">It is not possible to have separate operator implementations for the two notations.</span></span>

### <a name="cast-expressions"></a><span data-ttu-id="dd693-1596">Expressões de conversão</span><span class="sxs-lookup"><span data-stu-id="dd693-1596">Cast expressions</span></span>

<span data-ttu-id="dd693-1597">Uma *cast_expression* é usada para converter explicitamente uma expressão em um determinado tipo.</span><span class="sxs-lookup"><span data-stu-id="dd693-1597">A *cast_expression* is used to explicitly convert an expression to a given type.</span></span>

```antlr
cast_expression
    : '(' type ')' unary_expression
    ;
```

<span data-ttu-id="dd693-1598">Uma *cast_expression* do formulário `(T)E`, em que `T` é um *tipo* e `E` é um *unary_expression*, executa uma conversão explícita ([conversões explícitas](conversions.md#explicit-conversions)) do valor de `E` para o tipo `T`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1598">A *cast_expression* of the form `(T)E`, where `T` is a *type* and `E` is a *unary_expression*, performs an explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) of the value of `E` to type `T`.</span></span> <span data-ttu-id="dd693-1599">Se não existir nenhuma conversão explícita de `E` para `T`, ocorrerá um erro de tempo de associação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1599">If no explicit conversion exists from `E` to `T`, a binding-time error occurs.</span></span> <span data-ttu-id="dd693-1600">Caso contrário, o resultado será o valor produzido pela conversão explícita.</span><span class="sxs-lookup"><span data-stu-id="dd693-1600">Otherwise, the result is the value produced by the explicit conversion.</span></span> <span data-ttu-id="dd693-1601">O resultado é sempre classificado como um valor, mesmo se `E` denota uma variável.</span><span class="sxs-lookup"><span data-stu-id="dd693-1601">The result is always classified as a value, even if `E` denotes a variable.</span></span>

<span data-ttu-id="dd693-1602">A gramática de um *cast_expression* leva a determinadas ambiguidades sintáticas.</span><span class="sxs-lookup"><span data-stu-id="dd693-1602">The grammar for a *cast_expression* leads to certain syntactic ambiguities.</span></span> <span data-ttu-id="dd693-1603">Por exemplo, a expressão `(x)-y` poderia ser interpretada como uma *cast_expression* (uma conversão de `-y` para o tipo `x`) ou como um *additive_expression* combinado com um *parenthesized_expression* (que computa o valor `x - y)`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1603">For example, the expression `(x)-y` could either be interpreted as a *cast_expression* (a cast of `-y` to type `x`) or as an *additive_expression* combined with a *parenthesized_expression* (which computes the value `x - y)`.</span></span>

<span data-ttu-id="dd693-1604">Para resolver as ambiguidades *cast_expression* , a seguinte regra existe: uma sequência de um ou mais *token*s ([espaço em branco](lexical-structure.md#white-space)) entre parênteses é considerada o início de uma *cast_expression* somente se pelo menos uma das seguintes opções for verdadeira:</span><span class="sxs-lookup"><span data-stu-id="dd693-1604">To resolve *cast_expression* ambiguities, the following rule exists: A sequence of one or more *token*s ([White space](lexical-structure.md#white-space)) enclosed in parentheses is considered the start of a *cast_expression* only if at least one of the following are true:</span></span>

*  <span data-ttu-id="dd693-1605">A sequência de tokens é a gramática correta para um *tipo*, mas não para uma *expressão*.</span><span class="sxs-lookup"><span data-stu-id="dd693-1605">The sequence of tokens is correct grammar for a *type*, but not for an *expression*.</span></span>
*  <span data-ttu-id="dd693-1606">A sequência de tokens é a gramática correta para um *tipo*, e o token imediatamente após os parênteses de fechamento é o token "`~`", o token "`!`", o token "`(`", um *identificador* ([sequências de escape de caractere Unicode](lexical-structure.md#unicode-character-escape-sequences)), um *literal* ([literais](lexical-structure.md#literals)) ou qualquer *palavra-chave* ([palavras-chave](lexical-structure.md#keywords)), exceto `as` e `is`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1606">The sequence of tokens is correct grammar for a *type*, and the token immediately following the closing parentheses is the token "`~`", the token "`!`", the token "`(`", an *identifier* ([Unicode character escape sequences](lexical-structure.md#unicode-character-escape-sequences)), a *literal* ([Literals](lexical-structure.md#literals)), or any *keyword* ([Keywords](lexical-structure.md#keywords)) except `as` and `is`.</span></span>

<span data-ttu-id="dd693-1607">O termo "correção gramatical" acima significa apenas que a sequência de tokens deve estar de acordo com a produção gramatical específica.</span><span class="sxs-lookup"><span data-stu-id="dd693-1607">The term "correct grammar" above means only that the sequence of tokens must conform to the particular grammatical production.</span></span> <span data-ttu-id="dd693-1608">Ele não considera especificamente o significado real de quaisquer identificadores constituintes.</span><span class="sxs-lookup"><span data-stu-id="dd693-1608">It specifically does not consider the actual meaning of any constituent identifiers.</span></span> <span data-ttu-id="dd693-1609">Por exemplo, se `x` e `y` forem identificadores, `x.y` será a gramática correta para um tipo, mesmo se `x.y` não denotar realmente um tipo.</span><span class="sxs-lookup"><span data-stu-id="dd693-1609">For example, if `x` and `y` are identifiers, then `x.y` is correct grammar for a type, even if `x.y` doesn't actually denote a type.</span></span>

<span data-ttu-id="dd693-1610">A partir da regra de desambiguidade, ela é a seguinte, se `x` e `y` são identificadores, `(x)y`, `(x)(y)`e `(x)(-y)` são *cast_expression*s, mas `(x)-y` não é, mesmo que `x` identifique um tipo.</span><span class="sxs-lookup"><span data-stu-id="dd693-1610">From the disambiguation rule it follows that, if `x` and `y` are identifiers, `(x)y`, `(x)(y)`, and `(x)(-y)` are *cast_expression*s, but `(x)-y` is not, even if `x` identifies a type.</span></span> <span data-ttu-id="dd693-1611">No entanto, se `x` for uma palavra-chave que identifica um tipo predefinido (como `int`), todas as quatro formas serão *cast_expression*s (porque essa palavra-chave não poderia ser, por si só, uma expressão).</span><span class="sxs-lookup"><span data-stu-id="dd693-1611">However, if `x` is a keyword that identifies a predefined type (such as `int`), then all four forms are *cast_expression*s (because such a keyword could not possibly be an expression by itself).</span></span>

### <a name="await-expressions"></a><span data-ttu-id="dd693-1612">Expressões Await</span><span class="sxs-lookup"><span data-stu-id="dd693-1612">Await expressions</span></span>

<span data-ttu-id="dd693-1613">O operador Await é usado para suspender a avaliação da função Async delimitadora até que a operação assíncrona representada pelo operando tenha sido concluída.</span><span class="sxs-lookup"><span data-stu-id="dd693-1613">The await operator is used to suspend evaluation of the enclosing async function until the asynchronous operation represented by the operand has completed.</span></span>

```antlr
await_expression
    : 'await' unary_expression
    ;
```

<span data-ttu-id="dd693-1614">Um *await_expression* só é permitido no corpo de uma função Async ([iteradores](classes.md#iterators)).</span><span class="sxs-lookup"><span data-stu-id="dd693-1614">An *await_expression* is only allowed in the body of an async function ([Iterators](classes.md#iterators)).</span></span> <span data-ttu-id="dd693-1615">Na função assíncrona delimitadora mais próxima, um *await_expression* pode não ocorrer nesses locais:</span><span class="sxs-lookup"><span data-stu-id="dd693-1615">Within the nearest enclosing async function, an *await_expression* may not occur in these places:</span></span>

*  <span data-ttu-id="dd693-1616">Dentro de uma função anônima aninhada (não assíncrona)</span><span class="sxs-lookup"><span data-stu-id="dd693-1616">Inside a nested (non-async) anonymous function</span></span>
*  <span data-ttu-id="dd693-1617">Dentro do bloco de um *lock_statement*</span><span class="sxs-lookup"><span data-stu-id="dd693-1617">Inside the block of a *lock_statement*</span></span>
*  <span data-ttu-id="dd693-1618">Em um contexto sem segurança</span><span class="sxs-lookup"><span data-stu-id="dd693-1618">In an unsafe context</span></span>

<span data-ttu-id="dd693-1619">Observe que um *await_expression* não pode ocorrer na maioria dos lugares em um *query_expression*, pois eles são sintaticamente transformados para usar expressões lambda não assíncronas.</span><span class="sxs-lookup"><span data-stu-id="dd693-1619">Note that an *await_expression* cannot occur in most places within a *query_expression*, because those are syntactically transformed to use non-async lambda expressions.</span></span>

<span data-ttu-id="dd693-1620">Dentro de uma função Async, `await` não pode ser usada como um identificador.</span><span class="sxs-lookup"><span data-stu-id="dd693-1620">Inside of an async function, `await` cannot be used as an identifier.</span></span> <span data-ttu-id="dd693-1621">Portanto, não há nenhuma ambiguidade sintática entre Await-Expressions e várias expressões que envolvem identificadores.</span><span class="sxs-lookup"><span data-stu-id="dd693-1621">There is therefore no syntactic ambiguity between await-expressions and various expressions involving identifiers.</span></span> <span data-ttu-id="dd693-1622">Fora das funções assíncronas, `await` atua como um identificador normal.</span><span class="sxs-lookup"><span data-stu-id="dd693-1622">Outside of async functions, `await` acts as a normal identifier.</span></span>

<span data-ttu-id="dd693-1623">O operando de um *await_expression* é chamado de ***tarefa***.</span><span class="sxs-lookup"><span data-stu-id="dd693-1623">The operand of an *await_expression* is called the ***task***.</span></span> <span data-ttu-id="dd693-1624">Ele representa uma operação assíncrona que pode ou não ser concluída no momento em que a *await_expression* for avaliada.</span><span class="sxs-lookup"><span data-stu-id="dd693-1624">It represents an asynchronous operation that may or may not be complete at the time the *await_expression* is evaluated.</span></span> <span data-ttu-id="dd693-1625">A finalidade do operador Await é suspender a execução da função Async delimitadora até que a tarefa esperada seja concluída e, em seguida, obter seu resultado.</span><span class="sxs-lookup"><span data-stu-id="dd693-1625">The purpose of the await operator is to suspend execution of the enclosing async function until the awaited task is complete, and then obtain its outcome.</span></span>

#### <a name="awaitable-expressions"></a><span data-ttu-id="dd693-1626">Expressões awaitable</span><span class="sxs-lookup"><span data-stu-id="dd693-1626">Awaitable expressions</span></span>

<span data-ttu-id="dd693-1627">A tarefa de uma expressão Await deve ser ***awaitable***.</span><span class="sxs-lookup"><span data-stu-id="dd693-1627">The task of an await expression is required to be ***awaitable***.</span></span> <span data-ttu-id="dd693-1628">Uma expressão `t` será awaitable se uma das seguintes isenções:</span><span class="sxs-lookup"><span data-stu-id="dd693-1628">An expression `t` is awaitable if one of the following holds:</span></span>

*  <span data-ttu-id="dd693-1629">`t` é do tipo de tempo de compilação `dynamic`</span><span class="sxs-lookup"><span data-stu-id="dd693-1629">`t` is of compile time type `dynamic`</span></span>
*  <span data-ttu-id="dd693-1630">`t` tem uma instância acessível ou método de extensão chamado `GetAwaiter` sem parâmetros e nenhum parâmetro de tipo, e um tipo de retorno `A` para o qual todos os seguintes itens contêm:</span><span class="sxs-lookup"><span data-stu-id="dd693-1630">`t` has an accessible instance or extension method called `GetAwaiter` with no parameters and no type parameters, and a return type `A` for which all of the following hold:</span></span>
   * <span data-ttu-id="dd693-1631">`A` implementa a interface `System.Runtime.CompilerServices.INotifyCompletion` (daqui em diante, conhecida como `INotifyCompletion` para fins de brevidade)</span><span class="sxs-lookup"><span data-stu-id="dd693-1631">`A` implements the interface `System.Runtime.CompilerServices.INotifyCompletion` (hereafter known as `INotifyCompletion` for brevity)</span></span>
   * <span data-ttu-id="dd693-1632">`A` tem uma propriedade de instância acessível e legível `IsCompleted` do tipo `bool`</span><span class="sxs-lookup"><span data-stu-id="dd693-1632">`A` has an accessible, readable instance property `IsCompleted` of type `bool`</span></span>
   * <span data-ttu-id="dd693-1633">`A` tem um método de instância acessível `GetResult` sem parâmetros e nenhum parâmetro de tipo</span><span class="sxs-lookup"><span data-stu-id="dd693-1633">`A` has an accessible instance method `GetResult` with no parameters and no type parameters</span></span>

<span data-ttu-id="dd693-1634">A finalidade do método de `GetAwaiter` é obter um ***aguardador*** para a tarefa.</span><span class="sxs-lookup"><span data-stu-id="dd693-1634">The purpose of the `GetAwaiter` method is to obtain an ***awaiter*** for the task.</span></span> <span data-ttu-id="dd693-1635">O tipo `A` é chamado de ***tipo de aguardador*** para a expressão Await.</span><span class="sxs-lookup"><span data-stu-id="dd693-1635">The type `A` is called the ***awaiter type*** for the await expression.</span></span>

<span data-ttu-id="dd693-1636">A finalidade da propriedade `IsCompleted` é determinar se a tarefa já foi concluída.</span><span class="sxs-lookup"><span data-stu-id="dd693-1636">The purpose of the `IsCompleted` property is to determine if the task is already complete.</span></span> <span data-ttu-id="dd693-1637">Nesse caso, não há necessidade de suspender a avaliação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1637">If so, there is no need to suspend evaluation.</span></span>

<span data-ttu-id="dd693-1638">A finalidade do método de `INotifyCompletion.OnCompleted` é inscrever uma "continuação" para a tarefa; ou seja, um delegado (do tipo `System.Action`) que será invocado quando a tarefa for concluída.</span><span class="sxs-lookup"><span data-stu-id="dd693-1638">The purpose of the `INotifyCompletion.OnCompleted` method is to sign up a "continuation" to the task; i.e. a delegate (of type `System.Action`) that will be invoked once the task is complete.</span></span>

<span data-ttu-id="dd693-1639">A finalidade do método de `GetResult` é obter o resultado da tarefa quando ela for concluída.</span><span class="sxs-lookup"><span data-stu-id="dd693-1639">The purpose of the `GetResult` method is to obtain the outcome of the task once it is complete.</span></span> <span data-ttu-id="dd693-1640">Esse resultado pode ser uma conclusão bem-sucedida, possivelmente com um valor de resultado, ou pode ser uma exceção que é gerada pelo método de `GetResult`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1640">This outcome may be successful completion, possibly with a result value, or it may be an exception which is thrown by the `GetResult` method.</span></span>

#### <a name="classification-of-await-expressions"></a><span data-ttu-id="dd693-1641">Classificação de expressões Await</span><span class="sxs-lookup"><span data-stu-id="dd693-1641">Classification of await expressions</span></span>

<span data-ttu-id="dd693-1642">A expressão `await t` é classificada da mesma maneira que a expressão `(t).GetAwaiter().GetResult()`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1642">The expression `await t` is classified the same way as the expression `(t).GetAwaiter().GetResult()`.</span></span> <span data-ttu-id="dd693-1643">Portanto, se o tipo de retorno de `GetResult` for `void`, o *await_expression* será classificado como Nothing.</span><span class="sxs-lookup"><span data-stu-id="dd693-1643">Thus, if the return type of `GetResult` is `void`, the *await_expression* is classified as nothing.</span></span> <span data-ttu-id="dd693-1644">Se ele tiver um tipo de retorno não void `T`, o *await_expression* será classificado como um valor do tipo `T`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1644">If it has a non-void return type `T`, the *await_expression* is classified as a value of type `T`.</span></span>

#### <a name="runtime-evaluation-of-await-expressions"></a><span data-ttu-id="dd693-1645">Avaliação de tempo de execução de expressões Await</span><span class="sxs-lookup"><span data-stu-id="dd693-1645">Runtime evaluation of await expressions</span></span>

<span data-ttu-id="dd693-1646">Em tempo de execução, a expressão `await t` é avaliada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="dd693-1646">At runtime, the expression `await t` is evaluated as follows:</span></span>

*  <span data-ttu-id="dd693-1647">Uma `a` de aguardador é obtida avaliando a expressão `(t).GetAwaiter()`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1647">An awaiter `a` is obtained by evaluating the expression `(t).GetAwaiter()`.</span></span>
*  <span data-ttu-id="dd693-1648">Um `b` `bool` é obtido avaliando a expressão `(a).IsCompleted`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1648">A `bool` `b` is obtained by evaluating the expression `(a).IsCompleted`.</span></span>
*  <span data-ttu-id="dd693-1649">Se `b` for `false`, a avaliação dependerá se `a` implementa a interface `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (daqui em diante, conhecida como `ICriticalNotifyCompletion` para fins de brevidade).</span><span class="sxs-lookup"><span data-stu-id="dd693-1649">If `b` is `false` then evaluation depends on whether `a` implements the interface `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (hereafter known as `ICriticalNotifyCompletion` for brevity).</span></span> <span data-ttu-id="dd693-1650">Essa verificação é feita no momento da Associação; ou seja, em tempo de execução, se `a` tiver o tipo de tempo de compilação `dynamic`e, em caso contrário, o tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-1650">This check is done at binding time; i.e. at runtime if `a` has the compile time type `dynamic`, and at compile time otherwise.</span></span> <span data-ttu-id="dd693-1651">Permitir que `r` denotar o delegado de continuação ([iteradores](classes.md#iterators)):</span><span class="sxs-lookup"><span data-stu-id="dd693-1651">Let `r` denote the resumption delegate ([Iterators](classes.md#iterators)):</span></span>
    * <span data-ttu-id="dd693-1652">Se `a` não implementar `ICriticalNotifyCompletion`, a expressão `(a as (INotifyCompletion)).OnCompleted(r)` será avaliada.</span><span class="sxs-lookup"><span data-stu-id="dd693-1652">If `a` does not implement `ICriticalNotifyCompletion`, then the expression `(a as (INotifyCompletion)).OnCompleted(r)` is evaluated.</span></span>
    * <span data-ttu-id="dd693-1653">Se `a` implementa `ICriticalNotifyCompletion`, a expressão `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)` é avaliada.</span><span class="sxs-lookup"><span data-stu-id="dd693-1653">If `a` does implement `ICriticalNotifyCompletion`, then the expression `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)` is evaluated.</span></span>
    * <span data-ttu-id="dd693-1654">A avaliação é suspensa e o controle é retornado para o chamador atual da função Async.</span><span class="sxs-lookup"><span data-stu-id="dd693-1654">Evaluation is then suspended, and control is returned to the current caller of the async function.</span></span>
*  <span data-ttu-id="dd693-1655">Imediatamente após (se `b` foi `true`) ou após a invocação posterior do delegado de continuação (se `b` era `false`), a expressão `(a).GetResult()` é avaliada.</span><span class="sxs-lookup"><span data-stu-id="dd693-1655">Either immediately after (if `b` was `true`), or upon later invocation of the resumption delegate (if `b` was `false`), the expression `(a).GetResult()` is evaluated.</span></span> <span data-ttu-id="dd693-1656">Se ele retornar um valor, esse valor será o resultado da *await_expression*.</span><span class="sxs-lookup"><span data-stu-id="dd693-1656">If it returns a value, that value is the result of the *await_expression*.</span></span> <span data-ttu-id="dd693-1657">Caso contrário, o resultado será Nothing.</span><span class="sxs-lookup"><span data-stu-id="dd693-1657">Otherwise the result is nothing.</span></span>

<span data-ttu-id="dd693-1658">A implementação de um esperador dos métodos de interface `INotifyCompletion.OnCompleted` e `ICriticalNotifyCompletion.UnsafeOnCompleted` deve fazer com que o delegado `r` seja invocado no máximo uma vez.</span><span class="sxs-lookup"><span data-stu-id="dd693-1658">An awaiter's implementation of the interface methods `INotifyCompletion.OnCompleted` and `ICriticalNotifyCompletion.UnsafeOnCompleted` should cause the delegate `r` to be invoked at most once.</span></span> <span data-ttu-id="dd693-1659">Caso contrário, o comportamento da função assíncrona delimitadora será indefinido.</span><span class="sxs-lookup"><span data-stu-id="dd693-1659">Otherwise, the behavior of the enclosing async function is undefined.</span></span>

## <a name="arithmetic-operators"></a><span data-ttu-id="dd693-1660">Operadores aritméticos</span><span class="sxs-lookup"><span data-stu-id="dd693-1660">Arithmetic operators</span></span>

<span data-ttu-id="dd693-1661">Os operadores `*`, `/`, `%`, `+`e `-` são chamados de operadores aritméticos.</span><span class="sxs-lookup"><span data-stu-id="dd693-1661">The `*`, `/`, `%`, `+`, and `-` operators are called the arithmetic operators.</span></span>

```antlr
multiplicative_expression
    : unary_expression
    | multiplicative_expression '*' unary_expression
    | multiplicative_expression '/' unary_expression
    | multiplicative_expression '%' unary_expression
    ;

additive_expression
    : multiplicative_expression
    | additive_expression '+' multiplicative_expression
    | additive_expression '-' multiplicative_expression
    ;
```

<span data-ttu-id="dd693-1662">Se um operando de um operador aritmético tiver o tipo de tempo de compilação `dynamic`, a expressão será vinculada dinamicamente ([associação dinâmica](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="dd693-1662">If an operand of an arithmetic operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="dd693-1663">Nesse caso, o tipo de tempo de compilação da expressão é `dynamic`e a resolução descrita abaixo ocorrerá em tempo de execução usando o tipo de tempo de execução desses operandos que têm o tipo de tempo de compilação `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1663">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

### <a name="multiplication-operator"></a><span data-ttu-id="dd693-1664">Operador de multiplicação</span><span class="sxs-lookup"><span data-stu-id="dd693-1664">Multiplication operator</span></span>

<span data-ttu-id="dd693-1665">Para uma operação do formulário `x * y`, a resolução de sobrecarga de operador binário ([resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)) é aplicada para selecionar uma implementação de operador específica.</span><span class="sxs-lookup"><span data-stu-id="dd693-1665">For an operation of the form `x * y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="dd693-1666">Os operandos são convertidos nos tipos de parâmetro do operador selecionado e o tipo de resultado é o tipo de retorno do operador.</span><span class="sxs-lookup"><span data-stu-id="dd693-1666">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="dd693-1667">Os operadores de multiplicação predefinidos estão listados abaixo.</span><span class="sxs-lookup"><span data-stu-id="dd693-1667">The predefined multiplication operators are listed below.</span></span> <span data-ttu-id="dd693-1668">Todos os operadores computam o produto de `x` e `y`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1668">The operators all compute the product of `x` and `y`.</span></span>

*  <span data-ttu-id="dd693-1669">Multiplicação de inteiro:</span><span class="sxs-lookup"><span data-stu-id="dd693-1669">Integer multiplication:</span></span>

   ```csharp
   int operator *(int x, int y);
   uint operator *(uint x, uint y);
   long operator *(long x, long y);
   ulong operator *(ulong x, ulong y);
   ```

   <span data-ttu-id="dd693-1670">Em um contexto de `checked`, se o produto estiver fora do intervalo do tipo de resultado, um `System.OverflowException` será gerado.</span><span class="sxs-lookup"><span data-stu-id="dd693-1670">In a `checked` context, if the product is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="dd693-1671">Em um contexto de `unchecked`, os estouros não são relatados e quaisquer bits de ordem superior significativos fora do intervalo do tipo de resultado são descartados.</span><span class="sxs-lookup"><span data-stu-id="dd693-1671">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>


*  <span data-ttu-id="dd693-1672">Multiplicação de ponto flutuante:</span><span class="sxs-lookup"><span data-stu-id="dd693-1672">Floating-point multiplication:</span></span>

   ```csharp
   float operator *(float x, float y);
   double operator *(double x, double y);
   ```

   <span data-ttu-id="dd693-1673">O produto é calculado de acordo com as regras de aritmética de IEEE 754.</span><span class="sxs-lookup"><span data-stu-id="dd693-1673">The product is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="dd693-1674">A tabela a seguir lista os resultados de todas as combinações possíveis de valores finitos diferentes de zero, zeros, infinitos e NaN.</span><span class="sxs-lookup"><span data-stu-id="dd693-1674">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="dd693-1675">Na tabela, `x` e `y` são valores finitos positivos.</span><span class="sxs-lookup"><span data-stu-id="dd693-1675">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="dd693-1676">`z` é o resultado de `x * y`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1676">`z` is the result of `x * y`.</span></span> <span data-ttu-id="dd693-1677">Se o resultado for muito grande para o tipo de destino, `z` será infinito.</span><span class="sxs-lookup"><span data-stu-id="dd693-1677">If the result is too large for the destination type, `z` is infinity.</span></span> <span data-ttu-id="dd693-1678">Se o resultado for muito pequeno para o tipo de destino, `z` será zero.</span><span class="sxs-lookup"><span data-stu-id="dd693-1678">If the result is too small for the destination type, `z` is zero.</span></span>

   |      |      |      |     |     |      |      |     |
   |:----:|-----:|:----:|:---:|:---:|:----:|:----:|:----|
   |      | <span data-ttu-id="dd693-1679">\+ y</span><span class="sxs-lookup"><span data-stu-id="dd693-1679">+y</span></span>   | <span data-ttu-id="dd693-1680">-y</span><span class="sxs-lookup"><span data-stu-id="dd693-1680">-y</span></span>   | <span data-ttu-id="dd693-1681">+0</span><span class="sxs-lookup"><span data-stu-id="dd693-1681">+0</span></span>  | <span data-ttu-id="dd693-1682">-0</span><span class="sxs-lookup"><span data-stu-id="dd693-1682">-0</span></span>  | <span data-ttu-id="dd693-1683">+inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1683">+inf</span></span> | <span data-ttu-id="dd693-1684">-inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1684">-inf</span></span> | <span data-ttu-id="dd693-1685">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1685">NaN</span></span> | 
   | <span data-ttu-id="dd693-1686">{1&gt;+&lt;1}x</span><span class="sxs-lookup"><span data-stu-id="dd693-1686">+x</span></span>   | <span data-ttu-id="dd693-1687">+z</span><span class="sxs-lookup"><span data-stu-id="dd693-1687">+z</span></span>   | <span data-ttu-id="dd693-1688">-z</span><span class="sxs-lookup"><span data-stu-id="dd693-1688">-z</span></span>   | <span data-ttu-id="dd693-1689">+0</span><span class="sxs-lookup"><span data-stu-id="dd693-1689">+0</span></span>  | <span data-ttu-id="dd693-1690">-0</span><span class="sxs-lookup"><span data-stu-id="dd693-1690">-0</span></span>  | <span data-ttu-id="dd693-1691">+inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1691">+inf</span></span> | <span data-ttu-id="dd693-1692">-inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1692">-inf</span></span> | <span data-ttu-id="dd693-1693">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1693">NaN</span></span> | 
   | <span data-ttu-id="dd693-1694">{1&gt;-&lt;1}x</span><span class="sxs-lookup"><span data-stu-id="dd693-1694">-x</span></span>   | <span data-ttu-id="dd693-1695">-z</span><span class="sxs-lookup"><span data-stu-id="dd693-1695">-z</span></span>   | <span data-ttu-id="dd693-1696">+z</span><span class="sxs-lookup"><span data-stu-id="dd693-1696">+z</span></span>   | <span data-ttu-id="dd693-1697">-0</span><span class="sxs-lookup"><span data-stu-id="dd693-1697">-0</span></span>  | <span data-ttu-id="dd693-1698">+0</span><span class="sxs-lookup"><span data-stu-id="dd693-1698">+0</span></span>  | <span data-ttu-id="dd693-1699">-inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1699">-inf</span></span> | <span data-ttu-id="dd693-1700">+inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1700">+inf</span></span> | <span data-ttu-id="dd693-1701">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1701">NaN</span></span> | 
   | <span data-ttu-id="dd693-1702">+0</span><span class="sxs-lookup"><span data-stu-id="dd693-1702">+0</span></span>   | <span data-ttu-id="dd693-1703">+0</span><span class="sxs-lookup"><span data-stu-id="dd693-1703">+0</span></span>   | <span data-ttu-id="dd693-1704">-0</span><span class="sxs-lookup"><span data-stu-id="dd693-1704">-0</span></span>   | <span data-ttu-id="dd693-1705">+0</span><span class="sxs-lookup"><span data-stu-id="dd693-1705">+0</span></span>  | <span data-ttu-id="dd693-1706">-0</span><span class="sxs-lookup"><span data-stu-id="dd693-1706">-0</span></span>  | <span data-ttu-id="dd693-1707">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1707">NaN</span></span>  | <span data-ttu-id="dd693-1708">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1708">NaN</span></span>  | <span data-ttu-id="dd693-1709">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1709">NaN</span></span> | 
   | <span data-ttu-id="dd693-1710">-0</span><span class="sxs-lookup"><span data-stu-id="dd693-1710">-0</span></span>   | <span data-ttu-id="dd693-1711">-0</span><span class="sxs-lookup"><span data-stu-id="dd693-1711">-0</span></span>   | <span data-ttu-id="dd693-1712">+0</span><span class="sxs-lookup"><span data-stu-id="dd693-1712">+0</span></span>   | <span data-ttu-id="dd693-1713">-0</span><span class="sxs-lookup"><span data-stu-id="dd693-1713">-0</span></span>  | <span data-ttu-id="dd693-1714">+0</span><span class="sxs-lookup"><span data-stu-id="dd693-1714">+0</span></span>  | <span data-ttu-id="dd693-1715">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1715">NaN</span></span>  | <span data-ttu-id="dd693-1716">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1716">NaN</span></span>  | <span data-ttu-id="dd693-1717">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1717">NaN</span></span> | 
   | <span data-ttu-id="dd693-1718">+inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1718">+inf</span></span> | <span data-ttu-id="dd693-1719">+inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1719">+inf</span></span> | <span data-ttu-id="dd693-1720">-inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1720">-inf</span></span> | <span data-ttu-id="dd693-1721">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1721">NaN</span></span> | <span data-ttu-id="dd693-1722">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1722">NaN</span></span> | <span data-ttu-id="dd693-1723">+inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1723">+inf</span></span> | <span data-ttu-id="dd693-1724">-inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1724">-inf</span></span> | <span data-ttu-id="dd693-1725">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1725">NaN</span></span> | 
   | <span data-ttu-id="dd693-1726">-inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1726">-inf</span></span> | <span data-ttu-id="dd693-1727">-inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1727">-inf</span></span> | <span data-ttu-id="dd693-1728">+inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1728">+inf</span></span> | <span data-ttu-id="dd693-1729">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1729">NaN</span></span> | <span data-ttu-id="dd693-1730">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1730">NaN</span></span> | <span data-ttu-id="dd693-1731">-inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1731">-inf</span></span> | <span data-ttu-id="dd693-1732">+inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1732">+inf</span></span> | <span data-ttu-id="dd693-1733">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1733">NaN</span></span> | 
   | <span data-ttu-id="dd693-1734">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1734">NaN</span></span>  | <span data-ttu-id="dd693-1735">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1735">NaN</span></span>  | <span data-ttu-id="dd693-1736">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1736">NaN</span></span>  | <span data-ttu-id="dd693-1737">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1737">NaN</span></span> | <span data-ttu-id="dd693-1738">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1738">NaN</span></span> | <span data-ttu-id="dd693-1739">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1739">NaN</span></span>  | <span data-ttu-id="dd693-1740">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1740">NaN</span></span>  | <span data-ttu-id="dd693-1741">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1741">NaN</span></span> | 

*  <span data-ttu-id="dd693-1742">Multiplicação decimal:</span><span class="sxs-lookup"><span data-stu-id="dd693-1742">Decimal multiplication:</span></span>

   ```csharp
   decimal operator *(decimal x, decimal y);
   ```

   <span data-ttu-id="dd693-1743">Se o valor resultante for muito grande para representar no formato de `decimal`, um `System.OverflowException` será gerado.</span><span class="sxs-lookup"><span data-stu-id="dd693-1743">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="dd693-1744">Se o valor do resultado for muito pequeno para representar no formato de `decimal`, o resultado será zero.</span><span class="sxs-lookup"><span data-stu-id="dd693-1744">If the result value is too small to represent in the `decimal` format, the result is zero.</span></span> <span data-ttu-id="dd693-1745">A escala do resultado, antes de qualquer arredondamento, é a soma das escalas dos dois operandos.</span><span class="sxs-lookup"><span data-stu-id="dd693-1745">The scale of the result, before any rounding, is the sum of the scales of the two operands.</span></span>

   <span data-ttu-id="dd693-1746">A multiplicação decimal é equivalente a usar o operador de multiplicação do tipo `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1746">Decimal multiplication is equivalent to using the multiplication operator of type `System.Decimal`.</span></span>


### <a name="division-operator"></a><span data-ttu-id="dd693-1747">Operador de divisão</span><span class="sxs-lookup"><span data-stu-id="dd693-1747">Division operator</span></span>

<span data-ttu-id="dd693-1748">Para uma operação do formulário `x / y`, a resolução de sobrecarga de operador binário ([resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)) é aplicada para selecionar uma implementação de operador específica.</span><span class="sxs-lookup"><span data-stu-id="dd693-1748">For an operation of the form `x / y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="dd693-1749">Os operandos são convertidos nos tipos de parâmetro do operador selecionado e o tipo de resultado é o tipo de retorno do operador.</span><span class="sxs-lookup"><span data-stu-id="dd693-1749">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="dd693-1750">Os operadores de divisão predefinidos estão listados abaixo.</span><span class="sxs-lookup"><span data-stu-id="dd693-1750">The predefined division operators are listed below.</span></span> <span data-ttu-id="dd693-1751">Todos os operadores computam o quociente de `x` e `y`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1751">The operators all compute the quotient of `x` and `y`.</span></span>

*  <span data-ttu-id="dd693-1752">Divisão de inteiro:</span><span class="sxs-lookup"><span data-stu-id="dd693-1752">Integer division:</span></span>

   ```csharp
   int operator /(int x, int y);
   uint operator /(uint x, uint y);
   long operator /(long x, long y);
   ulong operator /(ulong x, ulong y);
   ```

   <span data-ttu-id="dd693-1753">Se o valor do operando direito for zero, um `System.DivideByZeroException` será lançado.</span><span class="sxs-lookup"><span data-stu-id="dd693-1753">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span>

   <span data-ttu-id="dd693-1754">A divisão arredonda o resultado em direção a zero.</span><span class="sxs-lookup"><span data-stu-id="dd693-1754">The division rounds the result towards zero.</span></span> <span data-ttu-id="dd693-1755">Portanto, o valor absoluto do resultado é o maior inteiro possível que é menor ou igual ao valor absoluto do quociente dos dois operandos.</span><span class="sxs-lookup"><span data-stu-id="dd693-1755">Thus the absolute value of the result is the largest possible integer that is less than or equal to the absolute value of the quotient of the two operands.</span></span> <span data-ttu-id="dd693-1756">O resultado é zero ou positivo quando os dois operandos têm o mesmo sinal e zero ou negativos quando os dois operandos têm sinais opostos.</span><span class="sxs-lookup"><span data-stu-id="dd693-1756">The result is zero or positive when the two operands have the same sign and zero or negative when the two operands have opposite signs.</span></span>

   <span data-ttu-id="dd693-1757">Se o operando esquerdo for o menor `int` representável ou o valor de `long` e o operando direito for `-1`, ocorrerá um estouro.</span><span class="sxs-lookup"><span data-stu-id="dd693-1757">If the left operand is the smallest representable `int` or `long` value and the right operand is `-1`, an overflow occurs.</span></span> <span data-ttu-id="dd693-1758">Em um contexto de `checked`, isso faz com que um `System.ArithmeticException` (ou uma subclasse dele) seja gerado.</span><span class="sxs-lookup"><span data-stu-id="dd693-1758">In a `checked` context, this causes a `System.ArithmeticException` (or a subclass thereof) to be thrown.</span></span> <span data-ttu-id="dd693-1759">Em um contexto de `unchecked`, ele é definido pela implementação para determinar se um `System.ArithmeticException` (ou uma subclasse dele) é gerado ou se o estouro não é reportado com o valor resultante sendo o do operando esquerdo.</span><span class="sxs-lookup"><span data-stu-id="dd693-1759">In an `unchecked` context, it is implementation-defined as to whether a `System.ArithmeticException` (or a subclass thereof) is thrown or the overflow goes unreported with the resulting value being that of the left operand.</span></span>

*  <span data-ttu-id="dd693-1760">Divisão de ponto flutuante:</span><span class="sxs-lookup"><span data-stu-id="dd693-1760">Floating-point division:</span></span>

   ```csharp
   float operator /(float x, float y);
   double operator /(double x, double y);
   ```

   <span data-ttu-id="dd693-1761">O quociente é calculado de acordo com as regras de aritmética de IEEE 754.</span><span class="sxs-lookup"><span data-stu-id="dd693-1761">The quotient is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="dd693-1762">A tabela a seguir lista os resultados de todas as combinações possíveis de valores finitos diferentes de zero, zeros, infinitos e NaN.</span><span class="sxs-lookup"><span data-stu-id="dd693-1762">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="dd693-1763">Na tabela, `x` e `y` são valores finitos positivos.</span><span class="sxs-lookup"><span data-stu-id="dd693-1763">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="dd693-1764">`z` é o resultado de `x / y`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1764">`z` is the result of `x / y`.</span></span> <span data-ttu-id="dd693-1765">Se o resultado for muito grande para o tipo de destino, `z` será infinito.</span><span class="sxs-lookup"><span data-stu-id="dd693-1765">If the result is too large for the destination type, `z` is infinity.</span></span> <span data-ttu-id="dd693-1766">Se o resultado for muito pequeno para o tipo de destino, `z` será zero.</span><span class="sxs-lookup"><span data-stu-id="dd693-1766">If the result is too small for the destination type, `z` is zero.</span></span>

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="dd693-1767">\+ y</span><span class="sxs-lookup"><span data-stu-id="dd693-1767">+y</span></span>   | <span data-ttu-id="dd693-1768">-y</span><span class="sxs-lookup"><span data-stu-id="dd693-1768">-y</span></span>   | <span data-ttu-id="dd693-1769">+0</span><span class="sxs-lookup"><span data-stu-id="dd693-1769">+0</span></span>   | <span data-ttu-id="dd693-1770">-0</span><span class="sxs-lookup"><span data-stu-id="dd693-1770">-0</span></span>   | <span data-ttu-id="dd693-1771">+inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1771">+inf</span></span> | <span data-ttu-id="dd693-1772">-inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1772">-inf</span></span> | <span data-ttu-id="dd693-1773">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1773">NaN</span></span>  | 
   | <span data-ttu-id="dd693-1774">{1&gt;+&lt;1}x</span><span class="sxs-lookup"><span data-stu-id="dd693-1774">+x</span></span>   | <span data-ttu-id="dd693-1775">+z</span><span class="sxs-lookup"><span data-stu-id="dd693-1775">+z</span></span>   | <span data-ttu-id="dd693-1776">-z</span><span class="sxs-lookup"><span data-stu-id="dd693-1776">-z</span></span>   | <span data-ttu-id="dd693-1777">+inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1777">+inf</span></span> | <span data-ttu-id="dd693-1778">-inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1778">-inf</span></span> | <span data-ttu-id="dd693-1779">+0</span><span class="sxs-lookup"><span data-stu-id="dd693-1779">+0</span></span>   | <span data-ttu-id="dd693-1780">-0</span><span class="sxs-lookup"><span data-stu-id="dd693-1780">-0</span></span>   | <span data-ttu-id="dd693-1781">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1781">NaN</span></span>  | 
   | <span data-ttu-id="dd693-1782">{1&gt;-&lt;1}x</span><span class="sxs-lookup"><span data-stu-id="dd693-1782">-x</span></span>   | <span data-ttu-id="dd693-1783">-z</span><span class="sxs-lookup"><span data-stu-id="dd693-1783">-z</span></span>   | <span data-ttu-id="dd693-1784">+z</span><span class="sxs-lookup"><span data-stu-id="dd693-1784">+z</span></span>   | <span data-ttu-id="dd693-1785">-inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1785">-inf</span></span> | <span data-ttu-id="dd693-1786">+inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1786">+inf</span></span> | <span data-ttu-id="dd693-1787">-0</span><span class="sxs-lookup"><span data-stu-id="dd693-1787">-0</span></span>   | <span data-ttu-id="dd693-1788">+0</span><span class="sxs-lookup"><span data-stu-id="dd693-1788">+0</span></span>   | <span data-ttu-id="dd693-1789">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1789">NaN</span></span>  | 
   | <span data-ttu-id="dd693-1790">+0</span><span class="sxs-lookup"><span data-stu-id="dd693-1790">+0</span></span>   | <span data-ttu-id="dd693-1791">+0</span><span class="sxs-lookup"><span data-stu-id="dd693-1791">+0</span></span>   | <span data-ttu-id="dd693-1792">-0</span><span class="sxs-lookup"><span data-stu-id="dd693-1792">-0</span></span>   | <span data-ttu-id="dd693-1793">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1793">NaN</span></span>  | <span data-ttu-id="dd693-1794">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1794">NaN</span></span>  | <span data-ttu-id="dd693-1795">+0</span><span class="sxs-lookup"><span data-stu-id="dd693-1795">+0</span></span>   | <span data-ttu-id="dd693-1796">-0</span><span class="sxs-lookup"><span data-stu-id="dd693-1796">-0</span></span>   | <span data-ttu-id="dd693-1797">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1797">NaN</span></span>  | 
   | <span data-ttu-id="dd693-1798">-0</span><span class="sxs-lookup"><span data-stu-id="dd693-1798">-0</span></span>   | <span data-ttu-id="dd693-1799">-0</span><span class="sxs-lookup"><span data-stu-id="dd693-1799">-0</span></span>   | <span data-ttu-id="dd693-1800">+0</span><span class="sxs-lookup"><span data-stu-id="dd693-1800">+0</span></span>   | <span data-ttu-id="dd693-1801">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1801">NaN</span></span>  | <span data-ttu-id="dd693-1802">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1802">NaN</span></span>  | <span data-ttu-id="dd693-1803">-0</span><span class="sxs-lookup"><span data-stu-id="dd693-1803">-0</span></span>   | <span data-ttu-id="dd693-1804">+0</span><span class="sxs-lookup"><span data-stu-id="dd693-1804">+0</span></span>   | <span data-ttu-id="dd693-1805">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1805">NaN</span></span>  | 
   | <span data-ttu-id="dd693-1806">+inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1806">+inf</span></span> | <span data-ttu-id="dd693-1807">+inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1807">+inf</span></span> | <span data-ttu-id="dd693-1808">-inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1808">-inf</span></span> | <span data-ttu-id="dd693-1809">+inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1809">+inf</span></span> | <span data-ttu-id="dd693-1810">-inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1810">-inf</span></span> | <span data-ttu-id="dd693-1811">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1811">NaN</span></span>  | <span data-ttu-id="dd693-1812">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1812">NaN</span></span>  | <span data-ttu-id="dd693-1813">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1813">NaN</span></span>  | 
   | <span data-ttu-id="dd693-1814">-inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1814">-inf</span></span> | <span data-ttu-id="dd693-1815">-inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1815">-inf</span></span> | <span data-ttu-id="dd693-1816">+inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1816">+inf</span></span> | <span data-ttu-id="dd693-1817">-inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1817">-inf</span></span> | <span data-ttu-id="dd693-1818">+inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1818">+inf</span></span> | <span data-ttu-id="dd693-1819">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1819">NaN</span></span>  | <span data-ttu-id="dd693-1820">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1820">NaN</span></span>  | <span data-ttu-id="dd693-1821">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1821">NaN</span></span>  | 
   | <span data-ttu-id="dd693-1822">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1822">NaN</span></span>  | <span data-ttu-id="dd693-1823">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1823">NaN</span></span>  | <span data-ttu-id="dd693-1824">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1824">NaN</span></span>  | <span data-ttu-id="dd693-1825">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1825">NaN</span></span>  | <span data-ttu-id="dd693-1826">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1826">NaN</span></span>  | <span data-ttu-id="dd693-1827">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1827">NaN</span></span>  | <span data-ttu-id="dd693-1828">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1828">NaN</span></span>  | <span data-ttu-id="dd693-1829">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1829">NaN</span></span>  | 

*  <span data-ttu-id="dd693-1830">Divisão decimal:</span><span class="sxs-lookup"><span data-stu-id="dd693-1830">Decimal division:</span></span>

   ```csharp
   decimal operator /(decimal x, decimal y);
   ```

   <span data-ttu-id="dd693-1831">Se o valor do operando direito for zero, um `System.DivideByZeroException` será lançado.</span><span class="sxs-lookup"><span data-stu-id="dd693-1831">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span> <span data-ttu-id="dd693-1832">Se o valor resultante for muito grande para representar no formato de `decimal`, um `System.OverflowException` será gerado.</span><span class="sxs-lookup"><span data-stu-id="dd693-1832">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="dd693-1833">Se o valor do resultado for muito pequeno para representar no formato de `decimal`, o resultado será zero.</span><span class="sxs-lookup"><span data-stu-id="dd693-1833">If the result value is too small to represent in the `decimal` format, the result is zero.</span></span> <span data-ttu-id="dd693-1834">A escala do resultado é a menor escala que preservará um resultado igual ao valor decimal representável mais próximo para o resultado matemático verdadeiro.</span><span class="sxs-lookup"><span data-stu-id="dd693-1834">The scale of the result is the smallest scale that will preserve a result equal to the nearest representable decimal value to the true mathematical result.</span></span>

   <span data-ttu-id="dd693-1835">A divisão decimal é equivalente a usar o operador de divisão do tipo `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1835">Decimal division is equivalent to using the division operator of type `System.Decimal`.</span></span>


### <a name="remainder-operator"></a><span data-ttu-id="dd693-1836">Operador de resto</span><span class="sxs-lookup"><span data-stu-id="dd693-1836">Remainder operator</span></span>

<span data-ttu-id="dd693-1837">Para uma operação do formulário `x % y`, a resolução de sobrecarga de operador binário ([resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)) é aplicada para selecionar uma implementação de operador específica.</span><span class="sxs-lookup"><span data-stu-id="dd693-1837">For an operation of the form `x % y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="dd693-1838">Os operandos são convertidos nos tipos de parâmetro do operador selecionado e o tipo de resultado é o tipo de retorno do operador.</span><span class="sxs-lookup"><span data-stu-id="dd693-1838">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="dd693-1839">Os operadores restantes predefinidos estão listados abaixo.</span><span class="sxs-lookup"><span data-stu-id="dd693-1839">The predefined remainder operators are listed below.</span></span> <span data-ttu-id="dd693-1840">Todos os operadores calculam o restante da divisão entre `x` e `y`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1840">The operators all compute the remainder of the division between `x` and `y`.</span></span>

*  <span data-ttu-id="dd693-1841">Restante do inteiro:</span><span class="sxs-lookup"><span data-stu-id="dd693-1841">Integer remainder:</span></span>

   ```csharp
   int operator %(int x, int y);
   uint operator %(uint x, uint y);
   long operator %(long x, long y);
   ulong operator %(ulong x, ulong y);
   ```

   <span data-ttu-id="dd693-1842">O resultado de `x % y` é o valor produzido por `x - (x / y) * y`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1842">The result of `x % y` is the value produced by `x - (x / y) * y`.</span></span> <span data-ttu-id="dd693-1843">Se `y` for zero, um `System.DivideByZeroException` será gerado.</span><span class="sxs-lookup"><span data-stu-id="dd693-1843">If `y` is zero, a `System.DivideByZeroException` is thrown.</span></span>

   <span data-ttu-id="dd693-1844">Se o operando esquerdo for o menor `int` ou valor de `long` e o operando direito for `-1`, um `System.OverflowException` será gerado.</span><span class="sxs-lookup"><span data-stu-id="dd693-1844">If the left operand is the smallest `int` or `long` value and the right operand is `-1`, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="dd693-1845">Em nenhuma hipótese `x % y` gerar uma exceção em que `x / y` não geraria uma exceção.</span><span class="sxs-lookup"><span data-stu-id="dd693-1845">In no case does `x % y` throw an exception where `x / y` would not throw an exception.</span></span>

*  <span data-ttu-id="dd693-1846">Restante do ponto flutuante:</span><span class="sxs-lookup"><span data-stu-id="dd693-1846">Floating-point remainder:</span></span>

   ```csharp
   float operator %(float x, float y);
   double operator %(double x, double y);
   ```

   <span data-ttu-id="dd693-1847">A tabela a seguir lista os resultados de todas as combinações possíveis de valores finitos diferentes de zero, zeros, infinitos e NaN.</span><span class="sxs-lookup"><span data-stu-id="dd693-1847">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="dd693-1848">Na tabela, `x` e `y` são valores finitos positivos.</span><span class="sxs-lookup"><span data-stu-id="dd693-1848">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="dd693-1849">`z` é o resultado de `x % y` e é computado como `x - n * y`, em que `n` é o maior inteiro possível que é menor ou igual a `x / y`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1849">`z` is the result of `x % y` and is computed as `x - n * y`, where `n` is the largest possible integer that is less than or equal to `x / y`.</span></span> <span data-ttu-id="dd693-1850">Esse método de computação do restante é análogo ao usado para operandos inteiros, mas difere da definição do IEEE 754 (em que `n` é o número inteiro mais próximo de `x / y`).</span><span class="sxs-lookup"><span data-stu-id="dd693-1850">This method of computing the remainder is analogous to that used for integer operands, but differs from the IEEE 754 definition (in which `n` is the integer closest to `x / y`).</span></span>

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="dd693-1851">\+ y</span><span class="sxs-lookup"><span data-stu-id="dd693-1851">+y</span></span>   | <span data-ttu-id="dd693-1852">-y</span><span class="sxs-lookup"><span data-stu-id="dd693-1852">-y</span></span>   | <span data-ttu-id="dd693-1853">+0</span><span class="sxs-lookup"><span data-stu-id="dd693-1853">+0</span></span>   | <span data-ttu-id="dd693-1854">-0</span><span class="sxs-lookup"><span data-stu-id="dd693-1854">-0</span></span>   | <span data-ttu-id="dd693-1855">+inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1855">+inf</span></span> | <span data-ttu-id="dd693-1856">-inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1856">-inf</span></span> | <span data-ttu-id="dd693-1857">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1857">NaN</span></span>  | 
   | <span data-ttu-id="dd693-1858">{1&gt;+&lt;1}x</span><span class="sxs-lookup"><span data-stu-id="dd693-1858">+x</span></span>   | <span data-ttu-id="dd693-1859">+z</span><span class="sxs-lookup"><span data-stu-id="dd693-1859">+z</span></span>   | <span data-ttu-id="dd693-1860">+z</span><span class="sxs-lookup"><span data-stu-id="dd693-1860">+z</span></span>   | <span data-ttu-id="dd693-1861">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1861">NaN</span></span>  | <span data-ttu-id="dd693-1862">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1862">NaN</span></span>  | <span data-ttu-id="dd693-1863">x</span><span class="sxs-lookup"><span data-stu-id="dd693-1863">x</span></span>    | <span data-ttu-id="dd693-1864">x</span><span class="sxs-lookup"><span data-stu-id="dd693-1864">x</span></span>    | <span data-ttu-id="dd693-1865">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1865">NaN</span></span>  | 
   | <span data-ttu-id="dd693-1866">{1&gt;-&lt;1}x</span><span class="sxs-lookup"><span data-stu-id="dd693-1866">-x</span></span>   | <span data-ttu-id="dd693-1867">-z</span><span class="sxs-lookup"><span data-stu-id="dd693-1867">-z</span></span>   | <span data-ttu-id="dd693-1868">-z</span><span class="sxs-lookup"><span data-stu-id="dd693-1868">-z</span></span>   | <span data-ttu-id="dd693-1869">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1869">NaN</span></span>  | <span data-ttu-id="dd693-1870">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1870">NaN</span></span>  | <span data-ttu-id="dd693-1871">{1&gt;-&lt;1}x</span><span class="sxs-lookup"><span data-stu-id="dd693-1871">-x</span></span>   | <span data-ttu-id="dd693-1872">{1&gt;-&lt;1}x</span><span class="sxs-lookup"><span data-stu-id="dd693-1872">-x</span></span>   | <span data-ttu-id="dd693-1873">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1873">NaN</span></span>  | 
   | <span data-ttu-id="dd693-1874">+0</span><span class="sxs-lookup"><span data-stu-id="dd693-1874">+0</span></span>   | <span data-ttu-id="dd693-1875">+0</span><span class="sxs-lookup"><span data-stu-id="dd693-1875">+0</span></span>   | <span data-ttu-id="dd693-1876">+0</span><span class="sxs-lookup"><span data-stu-id="dd693-1876">+0</span></span>   | <span data-ttu-id="dd693-1877">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1877">NaN</span></span>  | <span data-ttu-id="dd693-1878">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1878">NaN</span></span>  | <span data-ttu-id="dd693-1879">+0</span><span class="sxs-lookup"><span data-stu-id="dd693-1879">+0</span></span>   | <span data-ttu-id="dd693-1880">+0</span><span class="sxs-lookup"><span data-stu-id="dd693-1880">+0</span></span>   | <span data-ttu-id="dd693-1881">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1881">NaN</span></span>  | 
   | <span data-ttu-id="dd693-1882">-0</span><span class="sxs-lookup"><span data-stu-id="dd693-1882">-0</span></span>   | <span data-ttu-id="dd693-1883">-0</span><span class="sxs-lookup"><span data-stu-id="dd693-1883">-0</span></span>   | <span data-ttu-id="dd693-1884">-0</span><span class="sxs-lookup"><span data-stu-id="dd693-1884">-0</span></span>   | <span data-ttu-id="dd693-1885">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1885">NaN</span></span>  | <span data-ttu-id="dd693-1886">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1886">NaN</span></span>  | <span data-ttu-id="dd693-1887">-0</span><span class="sxs-lookup"><span data-stu-id="dd693-1887">-0</span></span>   | <span data-ttu-id="dd693-1888">-0</span><span class="sxs-lookup"><span data-stu-id="dd693-1888">-0</span></span>   | <span data-ttu-id="dd693-1889">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1889">NaN</span></span>  | 
   | <span data-ttu-id="dd693-1890">+inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1890">+inf</span></span> | <span data-ttu-id="dd693-1891">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1891">NaN</span></span>  | <span data-ttu-id="dd693-1892">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1892">NaN</span></span>  | <span data-ttu-id="dd693-1893">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1893">NaN</span></span>  | <span data-ttu-id="dd693-1894">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1894">NaN</span></span>  | <span data-ttu-id="dd693-1895">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1895">NaN</span></span>  | <span data-ttu-id="dd693-1896">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1896">NaN</span></span>  | <span data-ttu-id="dd693-1897">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1897">NaN</span></span>  | 
   | <span data-ttu-id="dd693-1898">-inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1898">-inf</span></span> | <span data-ttu-id="dd693-1899">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1899">NaN</span></span>  | <span data-ttu-id="dd693-1900">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1900">NaN</span></span>  | <span data-ttu-id="dd693-1901">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1901">NaN</span></span>  | <span data-ttu-id="dd693-1902">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1902">NaN</span></span>  | <span data-ttu-id="dd693-1903">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1903">NaN</span></span>  | <span data-ttu-id="dd693-1904">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1904">NaN</span></span>  | <span data-ttu-id="dd693-1905">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1905">NaN</span></span>  | 
   | <span data-ttu-id="dd693-1906">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1906">NaN</span></span>  | <span data-ttu-id="dd693-1907">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1907">NaN</span></span>  | <span data-ttu-id="dd693-1908">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1908">NaN</span></span>  | <span data-ttu-id="dd693-1909">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1909">NaN</span></span>  | <span data-ttu-id="dd693-1910">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1910">NaN</span></span>  | <span data-ttu-id="dd693-1911">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1911">NaN</span></span>  | <span data-ttu-id="dd693-1912">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1912">NaN</span></span>  | <span data-ttu-id="dd693-1913">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1913">NaN</span></span>  | 

*  <span data-ttu-id="dd693-1914">Restante decimal:</span><span class="sxs-lookup"><span data-stu-id="dd693-1914">Decimal remainder:</span></span>

   ```csharp
   decimal operator %(decimal x, decimal y);
   ```

   <span data-ttu-id="dd693-1915">Se o valor do operando direito for zero, um `System.DivideByZeroException` será lançado.</span><span class="sxs-lookup"><span data-stu-id="dd693-1915">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span> <span data-ttu-id="dd693-1916">A escala do resultado, antes de qualquer arredondamento, é maior das escalas dos dois operandos, e o sinal do resultado, se for diferente de zero, é o mesmo que o de `x`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1916">The scale of the result, before any rounding, is the larger of the scales of the two operands, and the sign of the result, if non-zero, is the same as that of `x`.</span></span>

   <span data-ttu-id="dd693-1917">O restante decimal é equivalente a usar o operador resto do tipo `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1917">Decimal remainder is equivalent to using the remainder operator of type `System.Decimal`.</span></span>


### <a name="addition-operator"></a><span data-ttu-id="dd693-1918">Operador de adição</span><span class="sxs-lookup"><span data-stu-id="dd693-1918">Addition operator</span></span>

<span data-ttu-id="dd693-1919">Para uma operação do formulário `x + y`, a resolução de sobrecarga de operador binário ([resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)) é aplicada para selecionar uma implementação de operador específica.</span><span class="sxs-lookup"><span data-stu-id="dd693-1919">For an operation of the form `x + y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="dd693-1920">Os operandos são convertidos nos tipos de parâmetro do operador selecionado e o tipo de resultado é o tipo de retorno do operador.</span><span class="sxs-lookup"><span data-stu-id="dd693-1920">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="dd693-1921">Os operadores de adição predefinidos são listados abaixo.</span><span class="sxs-lookup"><span data-stu-id="dd693-1921">The predefined addition operators are listed below.</span></span> <span data-ttu-id="dd693-1922">Para tipos numéricos e de enumeração, os operadores de adição predefinidos computam a soma dos dois operandos.</span><span class="sxs-lookup"><span data-stu-id="dd693-1922">For numeric and enumeration types, the predefined addition operators compute the sum of the two operands.</span></span> <span data-ttu-id="dd693-1923">Quando um ou ambos os operandos são do tipo cadeia de caracteres, os operadores de adição predefinidos concatenam a representação de cadeia de caracteres dos operandos.</span><span class="sxs-lookup"><span data-stu-id="dd693-1923">When one or both operands are of type string, the predefined addition operators concatenate the string representation of the operands.</span></span>

*  <span data-ttu-id="dd693-1924">Adição de inteiro:</span><span class="sxs-lookup"><span data-stu-id="dd693-1924">Integer addition:</span></span>

   ```csharp
   int operator +(int x, int y);
   uint operator +(uint x, uint y);
   long operator +(long x, long y);
   ulong operator +(ulong x, ulong y);
   ```

   <span data-ttu-id="dd693-1925">Em um contexto de `checked`, se a soma estiver fora do intervalo do tipo de resultado, uma `System.OverflowException` será lançada.</span><span class="sxs-lookup"><span data-stu-id="dd693-1925">In a `checked` context, if the sum is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="dd693-1926">Em um contexto de `unchecked`, os estouros não são relatados e quaisquer bits de ordem superior significativos fora do intervalo do tipo de resultado são descartados.</span><span class="sxs-lookup"><span data-stu-id="dd693-1926">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>

*  <span data-ttu-id="dd693-1927">Adição de ponto flutuante:</span><span class="sxs-lookup"><span data-stu-id="dd693-1927">Floating-point addition:</span></span>

   ```csharp
   float operator +(float x, float y);
   double operator +(double x, double y);
   ```

   <span data-ttu-id="dd693-1928">A soma é calculada de acordo com as regras de aritmética de IEEE 754.</span><span class="sxs-lookup"><span data-stu-id="dd693-1928">The sum is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="dd693-1929">A tabela a seguir lista os resultados de todas as combinações possíveis de valores finitos diferentes de zero, zeros, infinitos e NaN.</span><span class="sxs-lookup"><span data-stu-id="dd693-1929">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="dd693-1930">Na tabela, `x` e `y` são valores finitos diferentes de zero e `z` é o resultado de `x + y`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1930">In the table, `x` and `y` are nonzero finite values, and `z` is the result of `x + y`.</span></span> <span data-ttu-id="dd693-1931">Se `x` e `y` tiverem a mesma magnitude, mas sinais opostos, `z` será zero positivo.</span><span class="sxs-lookup"><span data-stu-id="dd693-1931">If `x` and `y` have the same magnitude but opposite signs, `z` is positive zero.</span></span> <span data-ttu-id="dd693-1932">Se `x + y` for muito grande para representar no tipo de destino, `z` será um infinito com o mesmo sinal que `x + y`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1932">If `x + y` is too large to represent in the destination type, `z` is an infinity with the same sign as `x + y`.</span></span>

   |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="dd693-1933">{1&gt;y&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1933">y</span></span>    | <span data-ttu-id="dd693-1934">+0</span><span class="sxs-lookup"><span data-stu-id="dd693-1934">+0</span></span>   | <span data-ttu-id="dd693-1935">-0</span><span class="sxs-lookup"><span data-stu-id="dd693-1935">-0</span></span>   | <span data-ttu-id="dd693-1936">+inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1936">+inf</span></span> | <span data-ttu-id="dd693-1937">-inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1937">-inf</span></span> | <span data-ttu-id="dd693-1938">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1938">NaN</span></span>  | 
   | <span data-ttu-id="dd693-1939">x</span><span class="sxs-lookup"><span data-stu-id="dd693-1939">x</span></span>    | <span data-ttu-id="dd693-1940">z</span><span class="sxs-lookup"><span data-stu-id="dd693-1940">z</span></span>    | <span data-ttu-id="dd693-1941">x</span><span class="sxs-lookup"><span data-stu-id="dd693-1941">x</span></span>    | <span data-ttu-id="dd693-1942">x</span><span class="sxs-lookup"><span data-stu-id="dd693-1942">x</span></span>    | <span data-ttu-id="dd693-1943">+inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1943">+inf</span></span> | <span data-ttu-id="dd693-1944">-inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1944">-inf</span></span> | <span data-ttu-id="dd693-1945">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1945">NaN</span></span>  | 
   | <span data-ttu-id="dd693-1946">+0</span><span class="sxs-lookup"><span data-stu-id="dd693-1946">+0</span></span>   | <span data-ttu-id="dd693-1947">{1&gt;y&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1947">y</span></span>    | <span data-ttu-id="dd693-1948">+0</span><span class="sxs-lookup"><span data-stu-id="dd693-1948">+0</span></span>   | <span data-ttu-id="dd693-1949">+0</span><span class="sxs-lookup"><span data-stu-id="dd693-1949">+0</span></span>   | <span data-ttu-id="dd693-1950">+inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1950">+inf</span></span> | <span data-ttu-id="dd693-1951">-inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1951">-inf</span></span> | <span data-ttu-id="dd693-1952">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1952">NaN</span></span>  | 
   | <span data-ttu-id="dd693-1953">-0</span><span class="sxs-lookup"><span data-stu-id="dd693-1953">-0</span></span>   | <span data-ttu-id="dd693-1954">{1&gt;y&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1954">y</span></span>    | <span data-ttu-id="dd693-1955">+0</span><span class="sxs-lookup"><span data-stu-id="dd693-1955">+0</span></span>   | <span data-ttu-id="dd693-1956">-0</span><span class="sxs-lookup"><span data-stu-id="dd693-1956">-0</span></span>   | <span data-ttu-id="dd693-1957">+inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1957">+inf</span></span> | <span data-ttu-id="dd693-1958">-inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1958">-inf</span></span> | <span data-ttu-id="dd693-1959">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1959">NaN</span></span>  | 
   | <span data-ttu-id="dd693-1960">+inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1960">+inf</span></span> | <span data-ttu-id="dd693-1961">+inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1961">+inf</span></span> | <span data-ttu-id="dd693-1962">+inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1962">+inf</span></span> | <span data-ttu-id="dd693-1963">+inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1963">+inf</span></span> | <span data-ttu-id="dd693-1964">+inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1964">+inf</span></span> | <span data-ttu-id="dd693-1965">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1965">NaN</span></span>  | <span data-ttu-id="dd693-1966">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1966">NaN</span></span>  | 
   | <span data-ttu-id="dd693-1967">-inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1967">-inf</span></span> | <span data-ttu-id="dd693-1968">-inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1968">-inf</span></span> | <span data-ttu-id="dd693-1969">-inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1969">-inf</span></span> | <span data-ttu-id="dd693-1970">-inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1970">-inf</span></span> | <span data-ttu-id="dd693-1971">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1971">NaN</span></span>  | <span data-ttu-id="dd693-1972">-inf</span><span class="sxs-lookup"><span data-stu-id="dd693-1972">-inf</span></span> | <span data-ttu-id="dd693-1973">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1973">NaN</span></span>  | 
   | <span data-ttu-id="dd693-1974">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1974">NaN</span></span>  | <span data-ttu-id="dd693-1975">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1975">NaN</span></span>  | <span data-ttu-id="dd693-1976">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1976">NaN</span></span>  | <span data-ttu-id="dd693-1977">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1977">NaN</span></span>  | <span data-ttu-id="dd693-1978">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1978">NaN</span></span>  | <span data-ttu-id="dd693-1979">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1979">NaN</span></span>  | <span data-ttu-id="dd693-1980">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-1980">NaN</span></span>  | 

*  <span data-ttu-id="dd693-1981">Adição de decimal:</span><span class="sxs-lookup"><span data-stu-id="dd693-1981">Decimal addition:</span></span>

   ```csharp
   decimal operator +(decimal x, decimal y);
   ```

   <span data-ttu-id="dd693-1982">Se o valor resultante for muito grande para representar no formato de `decimal`, um `System.OverflowException` será gerado.</span><span class="sxs-lookup"><span data-stu-id="dd693-1982">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="dd693-1983">A escala do resultado, antes de qualquer arredondamento, é maior das escalas dos dois operandos.</span><span class="sxs-lookup"><span data-stu-id="dd693-1983">The scale of the result, before any rounding, is the larger of the scales of the two operands.</span></span>

   <span data-ttu-id="dd693-1984">A adição de decimal é equivalente a usar o operador de adição do tipo `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1984">Decimal addition is equivalent to using the addition operator of type `System.Decimal`.</span></span>

*  <span data-ttu-id="dd693-1985">Adição de enumeração.</span><span class="sxs-lookup"><span data-stu-id="dd693-1985">Enumeration addition.</span></span> <span data-ttu-id="dd693-1986">Cada tipo de enumeração fornece implicitamente os seguintes operadores predefinidos, em que `E` é o tipo de enumeração e `U` é o tipo subjacente de `E`:</span><span class="sxs-lookup"><span data-stu-id="dd693-1986">Every enumeration type implicitly provides the following predefined operators, where `E` is the enum type, and `U` is the underlying type of `E`:</span></span>

   ```csharp
   E operator +(E x, U y);
   E operator +(U x, E y);
   ```

   <span data-ttu-id="dd693-1987">Em tempo de execução, esses operadores são avaliados exatamente como `(E)((U)x + (U)y)`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1987">At run-time these operators are evaluated exactly as `(E)((U)x + (U)y)`.</span></span>

*  <span data-ttu-id="dd693-1988">Concatenação de cadeia de caracteres:</span><span class="sxs-lookup"><span data-stu-id="dd693-1988">String concatenation:</span></span>

   ```csharp
   string operator +(string x, string y);
   string operator +(string x, object y);
   string operator +(object x, string y);
   ```

   <span data-ttu-id="dd693-1989">Essas sobrecargas do operador de `+` binário executam a concatenação de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="dd693-1989">These overloads of the binary `+` operator perform string concatenation.</span></span> <span data-ttu-id="dd693-1990">Se um operando de concatenação de cadeia de caracteres for `null`, uma cadeia de caracteres vazia será substituída.</span><span class="sxs-lookup"><span data-stu-id="dd693-1990">If an operand of string concatenation is `null`, an empty string is substituted.</span></span> <span data-ttu-id="dd693-1991">Caso contrário, qualquer argumento que não seja de cadeia de caracteres é convertido em sua representação de cadeia de caracteres invocando o método de `ToString` virtual herdado do tipo `object`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1991">Otherwise, any non-string argument is converted to its string representation by invoking the virtual `ToString` method inherited from type `object`.</span></span> <span data-ttu-id="dd693-1992">Se `ToString` retornar `null`, uma cadeia de caracteres vazia será substituída.</span><span class="sxs-lookup"><span data-stu-id="dd693-1992">If `ToString` returns `null`, an empty string is substituted.</span></span>

   ```csharp
   using System;
   
   class Test
   {
       static void Main() {
           string s = null;
           Console.WriteLine("s = >" + s + "<");        // displays s = ><
           int i = 1;
           Console.WriteLine("i = " + i);               // displays i = 1
           float f = 1.2300E+15F;
           Console.WriteLine("f = " + f);               // displays f = 1.23E+15
           decimal d = 2.900m;
           Console.WriteLine("d = " + d);               // displays d = 2.900
       }
   }
   ```

   O resultado do operador de concatenação de cadeia de caracteres é uma cadeia de caracteres que consiste nos caracteres do operando esquerdo seguidos dos caracteres do operando à direita. O operador de concatenação de cadeia de caracteres nunca retorna um valor `null`. <span data-ttu-id="dd693-1995">Um `System.OutOfMemoryException` poderá ser gerado se não houver memória suficiente disponível para alocar a cadeia de caracteres resultante.</span><span class="sxs-lookup"><span data-stu-id="dd693-1995">A `System.OutOfMemoryException` may be thrown if there is not enough memory available to allocate the resulting string.</span></span>

*  <span data-ttu-id="dd693-1996">Combinação de delegado.</span><span class="sxs-lookup"><span data-stu-id="dd693-1996">Delegate combination.</span></span> <span data-ttu-id="dd693-1997">Cada tipo delegate fornece implicitamente o seguinte operador predefinido, em que `D` é o tipo delegado:</span><span class="sxs-lookup"><span data-stu-id="dd693-1997">Every delegate type implicitly provides the following predefined operator, where `D` is the delegate type:</span></span>

   ```csharp
   D operator +(D x, D y);
   ```

   <span data-ttu-id="dd693-1998">O operador Binary `+` executa a combinação de delegado quando ambos os operandos são de algum tipo delegado `D`.</span><span class="sxs-lookup"><span data-stu-id="dd693-1998">The binary `+` operator performs delegate combination when both operands are of some delegate type `D`.</span></span> <span data-ttu-id="dd693-1999">(Se os operandos tiverem tipos delegados diferentes, ocorrerá um erro de tempo de associação.) Se o primeiro operando for `null`, o resultado da operação será o valor do segundo operando (mesmo que também seja `null`).</span><span class="sxs-lookup"><span data-stu-id="dd693-1999">(If the operands have different delegate types, a binding-time error occurs.) If the first operand is `null`, the result of the operation is the value of the second operand (even if that is also `null`).</span></span> <span data-ttu-id="dd693-2000">Caso contrário, se o segundo operando for `null`, o resultado da operação será o valor do primeiro operando.</span><span class="sxs-lookup"><span data-stu-id="dd693-2000">Otherwise, if the second operand is `null`, then the result of the operation is the value of the first operand.</span></span> <span data-ttu-id="dd693-2001">Caso contrário, o resultado da operação é uma nova instância de delegado que, quando invocada, invoca o primeiro operando e, em seguida, invoca o segundo operando.</span><span class="sxs-lookup"><span data-stu-id="dd693-2001">Otherwise, the result of the operation is a new delegate instance that, when invoked, invokes the first operand and then invokes the second operand.</span></span> <span data-ttu-id="dd693-2002">Para obter exemplos de combinação de delegado, consulte [operador de subtração](expressions.md#subtraction-operator) e [invocação de delegado](delegates.md#delegate-invocation).</span><span class="sxs-lookup"><span data-stu-id="dd693-2002">For examples of delegate combination, see [Subtraction operator](expressions.md#subtraction-operator) and [Delegate invocation](delegates.md#delegate-invocation).</span></span> <span data-ttu-id="dd693-2003">Como `System.Delegate` não é um tipo delegado, `operator` `+` não está definido para ele.</span><span class="sxs-lookup"><span data-stu-id="dd693-2003">Since `System.Delegate` is not a delegate type, `operator` `+` is not defined for it.</span></span>

### <a name="subtraction-operator"></a><span data-ttu-id="dd693-2004">Operador de subtração</span><span class="sxs-lookup"><span data-stu-id="dd693-2004">Subtraction operator</span></span>

<span data-ttu-id="dd693-2005">Para uma operação do formulário `x - y`, a resolução de sobrecarga de operador binário ([resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)) é aplicada para selecionar uma implementação de operador específica.</span><span class="sxs-lookup"><span data-stu-id="dd693-2005">For an operation of the form `x - y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="dd693-2006">Os operandos são convertidos nos tipos de parâmetro do operador selecionado e o tipo de resultado é o tipo de retorno do operador.</span><span class="sxs-lookup"><span data-stu-id="dd693-2006">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="dd693-2007">Os operadores de subtração predefinidos estão listados abaixo.</span><span class="sxs-lookup"><span data-stu-id="dd693-2007">The predefined subtraction operators are listed below.</span></span> <span data-ttu-id="dd693-2008">Os operadores todos subtraiem `y` de `x`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2008">The operators all subtract `y` from `x`.</span></span>

*  <span data-ttu-id="dd693-2009">Subtração de inteiro:</span><span class="sxs-lookup"><span data-stu-id="dd693-2009">Integer subtraction:</span></span>

   ```csharp
   int operator -(int x, int y);
   uint operator -(uint x, uint y);
   long operator -(long x, long y);
   ulong operator -(ulong x, ulong y);
   ```

   <span data-ttu-id="dd693-2010">Em um contexto de `checked`, se a diferença estiver fora do intervalo do tipo de resultado, um `System.OverflowException` será gerado.</span><span class="sxs-lookup"><span data-stu-id="dd693-2010">In a `checked` context, if the difference is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="dd693-2011">Em um contexto de `unchecked`, os estouros não são relatados e quaisquer bits de ordem superior significativos fora do intervalo do tipo de resultado são descartados.</span><span class="sxs-lookup"><span data-stu-id="dd693-2011">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>

*  <span data-ttu-id="dd693-2012">Subtração de ponto flutuante:</span><span class="sxs-lookup"><span data-stu-id="dd693-2012">Floating-point subtraction:</span></span>

   ```csharp
   float operator -(float x, float y);
   double operator -(double x, double y);
   ```

   <span data-ttu-id="dd693-2013">A diferença é calculada de acordo com as regras de aritmética de IEEE 754.</span><span class="sxs-lookup"><span data-stu-id="dd693-2013">The difference is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="dd693-2014">A tabela a seguir lista os resultados de todas as combinações possíveis de valores finitos diferentes de zero, zeros, infinitos e NaNs.</span><span class="sxs-lookup"><span data-stu-id="dd693-2014">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaNs.</span></span> <span data-ttu-id="dd693-2015">Na tabela, `x` e `y` são valores finitos diferentes de zero e `z` é o resultado de `x - y`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2015">In the table, `x` and `y` are nonzero finite values, and `z` is the result of `x - y`.</span></span> <span data-ttu-id="dd693-2016">Se `x` e `y` forem iguais, `z` será zero positivo.</span><span class="sxs-lookup"><span data-stu-id="dd693-2016">If `x` and `y` are equal, `z` is positive zero.</span></span> <span data-ttu-id="dd693-2017">Se `x - y` for muito grande para representar no tipo de destino, `z` será um infinito com o mesmo sinal que `x - y`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2017">If `x - y` is too large to represent in the destination type, `z` is an infinity with the same sign as `x - y`.</span></span>

   |      |      |      |      |      |      |     |
   |:----:|:----:|:----:|:----:|:----:|:----:|:---:|
   |      | <span data-ttu-id="dd693-2018">{1&gt;y&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-2018">y</span></span>    | <span data-ttu-id="dd693-2019">+0</span><span class="sxs-lookup"><span data-stu-id="dd693-2019">+0</span></span>   | <span data-ttu-id="dd693-2020">-0</span><span class="sxs-lookup"><span data-stu-id="dd693-2020">-0</span></span>   | <span data-ttu-id="dd693-2021">+inf</span><span class="sxs-lookup"><span data-stu-id="dd693-2021">+inf</span></span> | <span data-ttu-id="dd693-2022">-inf</span><span class="sxs-lookup"><span data-stu-id="dd693-2022">-inf</span></span> | <span data-ttu-id="dd693-2023">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-2023">NaN</span></span> | 
   | <span data-ttu-id="dd693-2024">x</span><span class="sxs-lookup"><span data-stu-id="dd693-2024">x</span></span>    | <span data-ttu-id="dd693-2025">z</span><span class="sxs-lookup"><span data-stu-id="dd693-2025">z</span></span>    | <span data-ttu-id="dd693-2026">x</span><span class="sxs-lookup"><span data-stu-id="dd693-2026">x</span></span>    | <span data-ttu-id="dd693-2027">x</span><span class="sxs-lookup"><span data-stu-id="dd693-2027">x</span></span>    | <span data-ttu-id="dd693-2028">-inf</span><span class="sxs-lookup"><span data-stu-id="dd693-2028">-inf</span></span> | <span data-ttu-id="dd693-2029">+inf</span><span class="sxs-lookup"><span data-stu-id="dd693-2029">+inf</span></span> | <span data-ttu-id="dd693-2030">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-2030">NaN</span></span> | 
   | <span data-ttu-id="dd693-2031">+0</span><span class="sxs-lookup"><span data-stu-id="dd693-2031">+0</span></span>   | <span data-ttu-id="dd693-2032">-y</span><span class="sxs-lookup"><span data-stu-id="dd693-2032">-y</span></span>   | <span data-ttu-id="dd693-2033">+0</span><span class="sxs-lookup"><span data-stu-id="dd693-2033">+0</span></span>   | <span data-ttu-id="dd693-2034">+0</span><span class="sxs-lookup"><span data-stu-id="dd693-2034">+0</span></span>   | <span data-ttu-id="dd693-2035">-inf</span><span class="sxs-lookup"><span data-stu-id="dd693-2035">-inf</span></span> | <span data-ttu-id="dd693-2036">+inf</span><span class="sxs-lookup"><span data-stu-id="dd693-2036">+inf</span></span> | <span data-ttu-id="dd693-2037">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-2037">NaN</span></span> | 
   | <span data-ttu-id="dd693-2038">-0</span><span class="sxs-lookup"><span data-stu-id="dd693-2038">-0</span></span>   | <span data-ttu-id="dd693-2039">-y</span><span class="sxs-lookup"><span data-stu-id="dd693-2039">-y</span></span>   | <span data-ttu-id="dd693-2040">-0</span><span class="sxs-lookup"><span data-stu-id="dd693-2040">-0</span></span>   | <span data-ttu-id="dd693-2041">+0</span><span class="sxs-lookup"><span data-stu-id="dd693-2041">+0</span></span>   | <span data-ttu-id="dd693-2042">-inf</span><span class="sxs-lookup"><span data-stu-id="dd693-2042">-inf</span></span> | <span data-ttu-id="dd693-2043">+inf</span><span class="sxs-lookup"><span data-stu-id="dd693-2043">+inf</span></span> | <span data-ttu-id="dd693-2044">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-2044">NaN</span></span> | 
   | <span data-ttu-id="dd693-2045">+inf</span><span class="sxs-lookup"><span data-stu-id="dd693-2045">+inf</span></span> | <span data-ttu-id="dd693-2046">+inf</span><span class="sxs-lookup"><span data-stu-id="dd693-2046">+inf</span></span> | <span data-ttu-id="dd693-2047">+inf</span><span class="sxs-lookup"><span data-stu-id="dd693-2047">+inf</span></span> | <span data-ttu-id="dd693-2048">+inf</span><span class="sxs-lookup"><span data-stu-id="dd693-2048">+inf</span></span> | <span data-ttu-id="dd693-2049">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-2049">NaN</span></span>  | <span data-ttu-id="dd693-2050">+inf</span><span class="sxs-lookup"><span data-stu-id="dd693-2050">+inf</span></span> | <span data-ttu-id="dd693-2051">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-2051">NaN</span></span> | 
   | <span data-ttu-id="dd693-2052">-inf</span><span class="sxs-lookup"><span data-stu-id="dd693-2052">-inf</span></span> | <span data-ttu-id="dd693-2053">-inf</span><span class="sxs-lookup"><span data-stu-id="dd693-2053">-inf</span></span> | <span data-ttu-id="dd693-2054">-inf</span><span class="sxs-lookup"><span data-stu-id="dd693-2054">-inf</span></span> | <span data-ttu-id="dd693-2055">-inf</span><span class="sxs-lookup"><span data-stu-id="dd693-2055">-inf</span></span> | <span data-ttu-id="dd693-2056">-inf</span><span class="sxs-lookup"><span data-stu-id="dd693-2056">-inf</span></span> | <span data-ttu-id="dd693-2057">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-2057">NaN</span></span>  | <span data-ttu-id="dd693-2058">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-2058">NaN</span></span> | 
   | <span data-ttu-id="dd693-2059">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-2059">NaN</span></span>  | <span data-ttu-id="dd693-2060">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-2060">NaN</span></span>  | <span data-ttu-id="dd693-2061">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-2061">NaN</span></span>  | <span data-ttu-id="dd693-2062">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-2062">NaN</span></span>  | <span data-ttu-id="dd693-2063">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-2063">NaN</span></span>  | <span data-ttu-id="dd693-2064">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-2064">NaN</span></span>  | <span data-ttu-id="dd693-2065">{1&gt;NaN&lt;1}</span><span class="sxs-lookup"><span data-stu-id="dd693-2065">NaN</span></span> | 

*  <span data-ttu-id="dd693-2066">Subtração decimal:</span><span class="sxs-lookup"><span data-stu-id="dd693-2066">Decimal subtraction:</span></span>

   ```csharp
   decimal operator -(decimal x, decimal y);
   ```

   <span data-ttu-id="dd693-2067">Se o valor resultante for muito grande para representar no formato de `decimal`, um `System.OverflowException` será gerado.</span><span class="sxs-lookup"><span data-stu-id="dd693-2067">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="dd693-2068">A escala do resultado, antes de qualquer arredondamento, é maior das escalas dos dois operandos.</span><span class="sxs-lookup"><span data-stu-id="dd693-2068">The scale of the result, before any rounding, is the larger of the scales of the two operands.</span></span>

   <span data-ttu-id="dd693-2069">A subtração decimal é equivalente a usar o operador de subtração do tipo `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2069">Decimal subtraction is equivalent to using the subtraction operator of type `System.Decimal`.</span></span>

*  <span data-ttu-id="dd693-2070">Subtração de enumeração.</span><span class="sxs-lookup"><span data-stu-id="dd693-2070">Enumeration subtraction.</span></span> <span data-ttu-id="dd693-2071">Cada tipo de enumeração fornece implicitamente o seguinte operador predefinido, em que `E` é o tipo de enumeração e `U` é o tipo subjacente de `E`:</span><span class="sxs-lookup"><span data-stu-id="dd693-2071">Every enumeration type implicitly provides the following predefined operator, where `E` is the enum type, and `U` is the underlying type of `E`:</span></span>

   ```csharp
   U operator -(E x, E y);
   ```

   <span data-ttu-id="dd693-2072">Esse operador é avaliado exatamente como `(U)((U)x - (U)y)`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2072">This operator is evaluated exactly as `(U)((U)x - (U)y)`.</span></span> <span data-ttu-id="dd693-2073">Em outras palavras, o operador computa a diferença entre os valores ordinais de `x` e `y`, e o tipo de resultado é o tipo subjacente da enumeração.</span><span class="sxs-lookup"><span data-stu-id="dd693-2073">In other words, the operator computes the difference between the ordinal values of `x` and `y`, and the type of the result is the underlying type of the enumeration.</span></span>

   ```csharp
   E operator -(E x, U y);
   ```

   <span data-ttu-id="dd693-2074">Esse operador é avaliado exatamente como `(E)((U)x - y)`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2074">This operator is evaluated exactly as `(E)((U)x - y)`.</span></span> <span data-ttu-id="dd693-2075">Em outras palavras, o operador subtrai um valor do tipo subjacente da enumeração, produzindo um valor da enumeração.</span><span class="sxs-lookup"><span data-stu-id="dd693-2075">In other words, the operator subtracts a value from the underlying type of the enumeration, yielding a value of the enumeration.</span></span>

*  <span data-ttu-id="dd693-2076">Remoção de delegado.</span><span class="sxs-lookup"><span data-stu-id="dd693-2076">Delegate removal.</span></span> <span data-ttu-id="dd693-2077">Cada tipo delegate fornece implicitamente o seguinte operador predefinido, em que `D` é o tipo delegado:</span><span class="sxs-lookup"><span data-stu-id="dd693-2077">Every delegate type implicitly provides the following predefined operator, where `D` is the delegate type:</span></span>

   ```csharp
   D operator -(D x, D y);
   ```

   <span data-ttu-id="dd693-2078">O operador Binary `-` executa a remoção de delegado quando ambos os operandos são de algum tipo delegado `D`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2078">The binary `-` operator performs delegate removal when both operands are of some delegate type `D`.</span></span> <span data-ttu-id="dd693-2079">Se os operandos tiverem tipos delegados diferentes, ocorrerá um erro de tempo de associação.</span><span class="sxs-lookup"><span data-stu-id="dd693-2079">If the operands have different delegate types, a binding-time error occurs.</span></span> <span data-ttu-id="dd693-2080">Se o primeiro operando for `null`, o resultado da operação será `null`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2080">If the first operand is `null`, the result of the operation is `null`.</span></span> <span data-ttu-id="dd693-2081">Caso contrário, se o segundo operando for `null`, o resultado da operação será o valor do primeiro operando.</span><span class="sxs-lookup"><span data-stu-id="dd693-2081">Otherwise, if the second operand is `null`, then the result of the operation is the value of the first operand.</span></span> <span data-ttu-id="dd693-2082">Caso contrário, os dois operandos representam listas de invocação ([declarações de delegado](delegates.md#delegate-declarations)) que têm uma ou mais entradas e o resultado é uma nova lista de invocação que consiste na lista do primeiro operando com as entradas do segundo operando removidas, desde que a lista do segundo operando seja uma sublista contígua adequada do primeiro.</span><span class="sxs-lookup"><span data-stu-id="dd693-2082">Otherwise, both operands represent invocation lists ([Delegate declarations](delegates.md#delegate-declarations)) having one or more entries, and the result is a new invocation list consisting of the first operand's list with the second operand's entries removed from it, provided the second operand's list is a proper contiguous sublist of the first's.</span></span>     <span data-ttu-id="dd693-2083">(Para determinar a igualdade da sublista, as entradas correspondentes são comparadas com o operador de igualdade de delegado ([operadores de igualdade de delegado](expressions.md#delegate-equality-operators)).) Caso contrário, o resultado será o valor do operando esquerdo.</span><span class="sxs-lookup"><span data-stu-id="dd693-2083">(To determine sublist equality, corresponding entries are compared as for the delegate equality operator ([Delegate equality operators](expressions.md#delegate-equality-operators)).) Otherwise, the result is the value of the left operand.</span></span> <span data-ttu-id="dd693-2084">Nenhuma das listas de operandos é alterada no processo.</span><span class="sxs-lookup"><span data-stu-id="dd693-2084">Neither of the operands' lists is changed in the process.</span></span> <span data-ttu-id="dd693-2085">Se a lista do segundo operando corresponder a várias sublistas de entradas contíguas na lista do primeiro operando, a sublista correspondente à direita de entradas contíguas será removida.</span><span class="sxs-lookup"><span data-stu-id="dd693-2085">If the second operand's list matches multiple sublists of contiguous entries in the first operand's list, the right-most matching sublist of contiguous entries is removed.</span></span> <span data-ttu-id="dd693-2086">Se a remoção resultar em uma lista vazia, o resultado será `null`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2086">If removal results in an empty list, the result is `null`.</span></span> <span data-ttu-id="dd693-2087">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="dd693-2087">For example:</span></span>

   ```csharp
   delegate void D(int x);
   
   class C
   {
       public static void M1(int i) { /* ... */ }
       public static void M2(int i) { /* ... */ }
   }

   class Test
   {
       static void Main() { 
           D cd1 = new D(C.M1);
           D cd2 = new D(C.M2);
           D cd3 = cd1 + cd2 + cd2 + cd1;   // M1 + M2 + M2 + M1
           cd3 -= cd1;                      // => M1 + M2 + M2
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd1 + cd2;                // => M2 + M1
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd2 + cd2;                // => M1 + M1
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd2 + cd1;                // => M1 + M2
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd1 + cd1;                // => M1 + M2 + M2 + M1
       }
   }
   ```

## <a name="shift-operators"></a><span data-ttu-id="dd693-2088">Operadores shift</span><span class="sxs-lookup"><span data-stu-id="dd693-2088">Shift operators</span></span>

<span data-ttu-id="dd693-2089">Os operadores de `<<` e `>>` são usados para executar operações de mudança de bits.</span><span class="sxs-lookup"><span data-stu-id="dd693-2089">The `<<` and `>>` operators are used to perform bit shifting operations.</span></span>

```antlr
shift_expression
    : additive_expression
    | shift_expression '<<' additive_expression
    | shift_expression right_shift additive_expression
    ;
```

<span data-ttu-id="dd693-2090">Se um operando de um *shift_expression* tiver o tipo de tempo de compilação `dynamic`, a expressão será vinculada dinamicamente ([associação dinâmica](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="dd693-2090">If an operand of a *shift_expression* has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="dd693-2091">Nesse caso, o tipo de tempo de compilação da expressão é `dynamic`e a resolução descrita abaixo ocorrerá em tempo de execução usando o tipo de tempo de execução desses operandos que têm o tipo de tempo de compilação `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2091">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="dd693-2092">Para uma operação do formulário `x << count` ou `x >> count`, a resolução de sobrecarga de operador binário ([resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)) é aplicada para selecionar uma implementação de operador específica.</span><span class="sxs-lookup"><span data-stu-id="dd693-2092">For an operation of the form `x << count` or `x >> count`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="dd693-2093">Os operandos são convertidos nos tipos de parâmetro do operador selecionado e o tipo de resultado é o tipo de retorno do operador.</span><span class="sxs-lookup"><span data-stu-id="dd693-2093">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="dd693-2094">Ao declarar um operador de deslocamento sobrecarregado, o tipo do primeiro operando sempre deve ser a classe ou struct que contém a declaração do operador e o tipo do segundo operando sempre deve ser `int`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2094">When declaring an overloaded shift operator, the type of the first operand must always be the class or struct containing the operator declaration, and the type of the second operand must always be `int`.</span></span>

<span data-ttu-id="dd693-2095">Os operadores de alternância predefinidos estão listados abaixo.</span><span class="sxs-lookup"><span data-stu-id="dd693-2095">The predefined shift operators are listed below.</span></span>

*  <span data-ttu-id="dd693-2096">Deslocar para a esquerda:</span><span class="sxs-lookup"><span data-stu-id="dd693-2096">Shift left:</span></span>

   ```csharp
   int operator <<(int x, int count);
   uint operator <<(uint x, int count);
   long operator <<(long x, int count);
   ulong operator <<(ulong x, int count);
   ```

   <span data-ttu-id="dd693-2097">O operador `<<` muda `x` à esquerda por um número de bits calculados, conforme descrito abaixo.</span><span class="sxs-lookup"><span data-stu-id="dd693-2097">The `<<` operator shifts `x` left by a number of bits computed as described below.</span></span>

   <span data-ttu-id="dd693-2098">Os bits de ordem superior fora do intervalo do tipo de resultado de `x` são descartados, os bits restantes são deslocados para a esquerda e as posições de bit vazia de ordem inferior são definidas como zero.</span><span class="sxs-lookup"><span data-stu-id="dd693-2098">The high-order bits outside the range of the result type of `x` are discarded, the remaining bits are shifted left, and the low-order empty bit positions are set to zero.</span></span>

*  <span data-ttu-id="dd693-2099">Deslocar para a direita:</span><span class="sxs-lookup"><span data-stu-id="dd693-2099">Shift right:</span></span>

   ```csharp
   int operator >>(int x, int count);
   uint operator >>(uint x, int count);
   long operator >>(long x, int count);
   ulong operator >>(ulong x, int count);
   ```

   <span data-ttu-id="dd693-2100">O operador `>>` muda `x` para a direita por um número de bits calculados, conforme descrito abaixo.</span><span class="sxs-lookup"><span data-stu-id="dd693-2100">The `>>` operator shifts `x` right by a number of bits computed as described below.</span></span>

   <span data-ttu-id="dd693-2101">Quando `x` é do tipo `int` ou `long`, os bits de ordem inferior de `x` são descartados, os bits restantes são deslocados para a direita e as posições de bit vazio de ordem superior são definidas como zero se `x` não for negativo e definido como um se `x` for negativo.</span><span class="sxs-lookup"><span data-stu-id="dd693-2101">When `x` is of type `int` or `long`, the low-order bits of `x` are discarded, the remaining bits are shifted right, and the high-order empty bit positions are set to zero if `x` is non-negative and set to one if `x` is negative.</span></span>

   <span data-ttu-id="dd693-2102">Quando `x` é do tipo `uint` ou `ulong`, os bits de ordem inferior de `x` são descartados, os bits restantes são deslocados para a direita e as posições de bit vazio de ordem superior são definidas como zero.</span><span class="sxs-lookup"><span data-stu-id="dd693-2102">When `x` is of type `uint` or `ulong`, the low-order bits of `x` are discarded, the remaining bits are shifted right, and the high-order empty bit positions are set to zero.</span></span>

<span data-ttu-id="dd693-2103">Para os operadores predefinidos, o número de bits a serem deslocados é calculado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="dd693-2103">For the predefined operators, the number of bits to shift is computed as follows:</span></span>

*  <span data-ttu-id="dd693-2104">Quando o tipo de `x` é `int` ou `uint`, a contagem de turnos é fornecida pelos cinco bits de ordem inferior de `count`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2104">When the type of `x` is `int` or `uint`, the shift count is given by the low-order five bits of `count`.</span></span> <span data-ttu-id="dd693-2105">Em outras palavras, a contagem de deslocamento é calculada a partir de `count & 0x1F`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2105">In other words, the shift count is computed from `count & 0x1F`.</span></span>
*  <span data-ttu-id="dd693-2106">Quando o tipo de `x` é `long` ou `ulong`, a contagem de turnos é fornecida pelos seis bits de ordem inferior de `count`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2106">When the type of `x` is `long` or `ulong`, the shift count is given by the low-order six bits of `count`.</span></span> <span data-ttu-id="dd693-2107">Em outras palavras, a contagem de deslocamento é calculada a partir de `count & 0x3F`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2107">In other words, the shift count is computed from `count & 0x3F`.</span></span>

<span data-ttu-id="dd693-2108">Se a contagem de deslocamento resultante for zero, os operadores de alternância simplesmente retornarão o valor de `x`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2108">If the resulting shift count is zero, the shift operators simply return the value of `x`.</span></span>

<span data-ttu-id="dd693-2109">As operações de Shift nunca causam estouros e produzem os mesmos resultados em contextos `checked` e `unchecked`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2109">Shift operations never cause overflows and produce the same results in `checked` and `unchecked` contexts.</span></span>

<span data-ttu-id="dd693-2110">Quando o operando esquerdo do operador de `>>` for de um tipo integral assinado, o operador executará um deslocamento aritmético à direita, no qual o valor do bit mais significativo (o bit de sinal) do operando será propagado para as posições de bit vazio de ordem superior.</span><span class="sxs-lookup"><span data-stu-id="dd693-2110">When the left operand of the `>>` operator is of a signed integral type, the operator performs an arithmetic shift right wherein the value of the most significant bit (the sign bit) of the operand is propagated to the high-order empty bit positions.</span></span> <span data-ttu-id="dd693-2111">Quando o operando esquerdo do operador de `>>` é de um tipo integral não assinado, o operador executa um direito de deslocamento lógico onde as posições de bits vazias de ordem superior são sempre definidas como zero.</span><span class="sxs-lookup"><span data-stu-id="dd693-2111">When the left operand of the `>>` operator is of an unsigned integral type, the operator performs a logical shift right wherein high-order empty bit positions are always set to zero.</span></span> <span data-ttu-id="dd693-2112">Para executar a operação oposta da inferida do tipo de operando, as conversões explícitas podem ser usadas.</span><span class="sxs-lookup"><span data-stu-id="dd693-2112">To perform the opposite operation of that inferred from the operand type, explicit casts can be used.</span></span> <span data-ttu-id="dd693-2113">Por exemplo, se `x` for uma variável do tipo `int`, a operação `unchecked((int)((uint)x >> y))` executará um deslocamento lógico à direita de `x`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2113">For example, if `x` is a variable of type `int`, the operation `unchecked((int)((uint)x >> y))` performs a logical shift right of `x`.</span></span>

## <a name="relational-and-type-testing-operators"></a><span data-ttu-id="dd693-2114">Operadores de teste de tipo e relacional</span><span class="sxs-lookup"><span data-stu-id="dd693-2114">Relational and type-testing operators</span></span>

<span data-ttu-id="dd693-2115">Os operadores `==`, `!=`, `<`, `>`, `<=`, `>=`, `is` e `as` são chamados de operadores relacionais e de teste de tipo.</span><span class="sxs-lookup"><span data-stu-id="dd693-2115">The `==`, `!=`, `<`, `>`, `<=`, `>=`, `is` and `as` operators are called the relational and type-testing operators.</span></span>

```antlr
relational_expression
    : shift_expression
    | relational_expression '<' shift_expression
    | relational_expression '>' shift_expression
    | relational_expression '<=' shift_expression
    | relational_expression '>=' shift_expression
    | relational_expression 'is' type
    | relational_expression 'as' type
    ;

equality_expression
    : relational_expression
    | equality_expression '==' relational_expression
    | equality_expression '!=' relational_expression
    ;
```

<span data-ttu-id="dd693-2116">O operador de `is` é descrito no [operador is](expressions.md#the-is-operator) e o operador `as` é descrito no [operador as](expressions.md#the-as-operator).</span><span class="sxs-lookup"><span data-stu-id="dd693-2116">The `is` operator is described in [The is operator](expressions.md#the-is-operator) and the `as` operator is described in [The as operator](expressions.md#the-as-operator).</span></span>

<span data-ttu-id="dd693-2117">Os operadores `==`, `!=`, `<`, `>`, `<=` e `>=` são ***operadores de comparação***.</span><span class="sxs-lookup"><span data-stu-id="dd693-2117">The `==`, `!=`, `<`, `>`, `<=` and `>=` operators are ***comparison operators***.</span></span>

<span data-ttu-id="dd693-2118">Se um operando de um operador de comparação tiver o tipo de tempo de compilação `dynamic`, a expressão será vinculada dinamicamente ([associação dinâmica](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="dd693-2118">If an operand of a comparison operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="dd693-2119">Nesse caso, o tipo de tempo de compilação da expressão é `dynamic`e a resolução descrita abaixo ocorrerá em tempo de execução usando o tipo de tempo de execução desses operandos que têm o tipo de tempo de compilação `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2119">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="dd693-2120">Para uma operação do formulário `x` *op* `y`, em que *op* é um operador de comparação, a resolução de sobrecarga ([resolução de sobrecarga do operador binário](expressions.md#binary-operator-overload-resolution)) é aplicada para selecionar uma implementação de operador específica.</span><span class="sxs-lookup"><span data-stu-id="dd693-2120">For an operation of the form `x` *op* `y`, where *op* is a comparison operator, overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="dd693-2121">Os operandos são convertidos nos tipos de parâmetro do operador selecionado e o tipo de resultado é o tipo de retorno do operador.</span><span class="sxs-lookup"><span data-stu-id="dd693-2121">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="dd693-2122">Os operadores de comparação predefinidos são descritos nas seções a seguir.</span><span class="sxs-lookup"><span data-stu-id="dd693-2122">The predefined comparison operators are described in the following sections.</span></span> <span data-ttu-id="dd693-2123">Todos os operadores de comparação predefinidos retornam um resultado do tipo `bool`, conforme descrito na tabela a seguir.</span><span class="sxs-lookup"><span data-stu-id="dd693-2123">All predefined comparison operators return a result of type `bool`, as described in the following table.</span></span>


| <span data-ttu-id="dd693-2124">__Operação__</span><span class="sxs-lookup"><span data-stu-id="dd693-2124">__Operation__</span></span> | <span data-ttu-id="dd693-2125">__Result__</span><span class="sxs-lookup"><span data-stu-id="dd693-2125">__Result__</span></span>                                                       |
|---------------|------------------------------------------------------------------|
| `x == y`      | <span data-ttu-id="dd693-2126">`true` se `x` for igual a `y`, `false` de outra forma</span><span class="sxs-lookup"><span data-stu-id="dd693-2126">`true` if `x` is equal to `y`, `false` otherwise</span></span>                 | 
| `x != y`      | <span data-ttu-id="dd693-2127">`true` se `x` não for igual a `y`, `false` de outra forma</span><span class="sxs-lookup"><span data-stu-id="dd693-2127">`true` if `x` is not equal to `y`, `false` otherwise</span></span>             | 
| `x < y`       | <span data-ttu-id="dd693-2128">`true` se `x` for menor que `y`, `false` de outra forma</span><span class="sxs-lookup"><span data-stu-id="dd693-2128">`true` if `x` is less than `y`, `false` otherwise</span></span>                | 
| `x > y`       | <span data-ttu-id="dd693-2129">`true` se `x` for maior que `y`, `false` de outra forma</span><span class="sxs-lookup"><span data-stu-id="dd693-2129">`true` if `x` is greater than `y`, `false` otherwise</span></span>             | 
| `x <= y`      | <span data-ttu-id="dd693-2130">`true` se `x` for menor ou igual a `y`, `false` caso contrário</span><span class="sxs-lookup"><span data-stu-id="dd693-2130">`true` if `x` is less than or equal to `y`, `false` otherwise</span></span>    | 
| `x >= y`      | <span data-ttu-id="dd693-2131">`true` se `x` for maior ou igual a `y`, `false` de outra forma</span><span class="sxs-lookup"><span data-stu-id="dd693-2131">`true` if `x` is greater than or equal to `y`, `false` otherwise</span></span> | 

### <a name="integer-comparison-operators"></a><span data-ttu-id="dd693-2132">Operadores de comparação de inteiros</span><span class="sxs-lookup"><span data-stu-id="dd693-2132">Integer comparison operators</span></span>

<span data-ttu-id="dd693-2133">Os operadores de comparação de inteiros predefinidos são:</span><span class="sxs-lookup"><span data-stu-id="dd693-2133">The predefined integer comparison operators are:</span></span>
```csharp
bool operator ==(int x, int y);
bool operator ==(uint x, uint y);
bool operator ==(long x, long y);
bool operator ==(ulong x, ulong y);

bool operator !=(int x, int y);
bool operator !=(uint x, uint y);
bool operator !=(long x, long y);
bool operator !=(ulong x, ulong y);

bool operator <(int x, int y);
bool operator <(uint x, uint y);
bool operator <(long x, long y);
bool operator <(ulong x, ulong y);

bool operator >(int x, int y);
bool operator >(uint x, uint y);
bool operator >(long x, long y);
bool operator >(ulong x, ulong y);

bool operator <=(int x, int y);
bool operator <=(uint x, uint y);
bool operator <=(long x, long y);
bool operator <=(ulong x, ulong y);

bool operator >=(int x, int y);
bool operator >=(uint x, uint y);
bool operator >=(long x, long y);
bool operator >=(ulong x, ulong y);
```

<span data-ttu-id="dd693-2134">Cada um desses operadores compara os valores numéricos dos dois operando inteiros e retorna um valor `bool` que indica se a relação específica é `true` ou `false`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2134">Each of these operators compares the numeric values of the two integer operands and returns a `bool` value that indicates whether the particular relation is `true` or `false`.</span></span>

### <a name="floating-point-comparison-operators"></a><span data-ttu-id="dd693-2135">Operadores de comparação de ponto flutuante</span><span class="sxs-lookup"><span data-stu-id="dd693-2135">Floating-point comparison operators</span></span>

<span data-ttu-id="dd693-2136">Os operadores de comparação de ponto flutuante predefinidos são:</span><span class="sxs-lookup"><span data-stu-id="dd693-2136">The predefined floating-point comparison operators are:</span></span>
```csharp
bool operator ==(float x, float y);
bool operator ==(double x, double y);

bool operator !=(float x, float y);
bool operator !=(double x, double y);

bool operator <(float x, float y);
bool operator <(double x, double y);

bool operator >(float x, float y);
bool operator >(double x, double y);

bool operator <=(float x, float y);
bool operator <=(double x, double y);

bool operator >=(float x, float y);
bool operator >=(double x, double y);
```

<span data-ttu-id="dd693-2137">Os operadores comparam os operandos de acordo com as regras do padrão IEEE 754:</span><span class="sxs-lookup"><span data-stu-id="dd693-2137">The operators compare the operands according to the rules of the IEEE 754 standard:</span></span>

*  <span data-ttu-id="dd693-2138">Se qualquer operando for NaN, o resultado será `false` para todos os operadores, exceto `!=`, para o qual o resultado é `true`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2138">If either operand is NaN, the result is `false` for all operators except `!=`, for which the result is `true`.</span></span> <span data-ttu-id="dd693-2139">Para quaisquer dois operandos, `x != y` sempre produz o mesmo resultado que `!(x == y)`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2139">For any two operands, `x != y` always produces the same result as `!(x == y)`.</span></span> <span data-ttu-id="dd693-2140">No entanto, quando um ou ambos os operandos forem NaN, os operadores `<`, `>`, `<=`e `>=` não produzirão os mesmos resultados que a negação lógica do operador oposto.</span><span class="sxs-lookup"><span data-stu-id="dd693-2140">However, when one or both operands are NaN, the `<`, `>`, `<=`, and `>=` operators do not produce the same results as the logical negation of the opposite operator.</span></span> <span data-ttu-id="dd693-2141">Por exemplo, se um dos `x` e `y` for NaN, `x < y` será `false`, mas `!(x >= y)` será `true`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2141">For example, if either of `x` and `y` is NaN, then `x < y` is `false`, but `!(x >= y)` is `true`.</span></span>
*  <span data-ttu-id="dd693-2142">Quando nenhum operando é NaN, os operadores comparam os valores dos dois operandos de ponto flutuante em relação à ordenação</span><span class="sxs-lookup"><span data-stu-id="dd693-2142">When neither operand is NaN, the operators compare the values of the two floating-point operands with respect to the ordering</span></span>

   ```csharp
   -inf < -max < ... < -min < -0.0 == +0.0 < +min < ... < +max < +inf
   ```

   <span data-ttu-id="dd693-2143">onde `min` e `max` são os menores e maiores valores finitos positivos que podem ser representados no formato de ponto flutuante fornecido.</span><span class="sxs-lookup"><span data-stu-id="dd693-2143">where `min` and `max` are the smallest and largest positive finite values that can be represented in the given floating-point format.</span></span> <span data-ttu-id="dd693-2144">Os efeitos notáveis dessa ordem são:</span><span class="sxs-lookup"><span data-stu-id="dd693-2144">Notable effects of this ordering are:</span></span>
   * <span data-ttu-id="dd693-2145">Zeros negativos e positivos são considerados iguais.</span><span class="sxs-lookup"><span data-stu-id="dd693-2145">Negative and positive zeros are considered equal.</span></span>
   * <span data-ttu-id="dd693-2146">Um infinito negativo é considerado menor que todos os outros valores, mas é igual a outro infinito negativo.</span><span class="sxs-lookup"><span data-stu-id="dd693-2146">A negative infinity is considered less than all other values, but equal to another negative infinity.</span></span>
   * <span data-ttu-id="dd693-2147">Um infinito positivo é considerado maior que todos os outros valores, mas é igual a outro infinito positivo.</span><span class="sxs-lookup"><span data-stu-id="dd693-2147">A positive infinity is considered greater than all other values, but equal to another positive infinity.</span></span>

### <a name="decimal-comparison-operators"></a><span data-ttu-id="dd693-2148">Operadores de comparação decimal</span><span class="sxs-lookup"><span data-stu-id="dd693-2148">Decimal comparison operators</span></span>

<span data-ttu-id="dd693-2149">Os operadores de comparação decimal predefinidos são:</span><span class="sxs-lookup"><span data-stu-id="dd693-2149">The predefined decimal comparison operators are:</span></span>
```csharp
bool operator ==(decimal x, decimal y);
bool operator !=(decimal x, decimal y);
bool operator <(decimal x, decimal y);
bool operator >(decimal x, decimal y);
bool operator <=(decimal x, decimal y);
bool operator >=(decimal x, decimal y);
```

<span data-ttu-id="dd693-2150">Cada um desses operadores compara os valores numéricos dos dois operando decimais e retorna um valor `bool` que indica se a relação específica é `true` ou `false`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2150">Each of these operators compares the numeric values of the two decimal operands and returns a `bool` value that indicates whether the particular relation is `true` or `false`.</span></span> <span data-ttu-id="dd693-2151">Cada comparação decimal é equivalente a usar o operador relacional ou de igualdade correspondente do tipo `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2151">Each decimal comparison is equivalent to using the corresponding relational or equality operator of type `System.Decimal`.</span></span>

### <a name="boolean-equality-operators"></a><span data-ttu-id="dd693-2152">Operadores de igualdade boolianos</span><span class="sxs-lookup"><span data-stu-id="dd693-2152">Boolean equality operators</span></span>

<span data-ttu-id="dd693-2153">Os operadores de igualdade boolianos predefinidos são:</span><span class="sxs-lookup"><span data-stu-id="dd693-2153">The predefined boolean equality operators are:</span></span>
```csharp
bool operator ==(bool x, bool y);
bool operator !=(bool x, bool y);
```

<span data-ttu-id="dd693-2154">O resultado de `==` é `true` se `x` e `y` são `true` ou se `x` e `y` são `false`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2154">The result of `==` is `true` if both `x` and `y` are `true` or if both `x` and `y` are `false`.</span></span> <span data-ttu-id="dd693-2155">Caso contrário, o resultado será `false`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2155">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="dd693-2156">O resultado de `!=` é `false` se `x` e `y` são `true` ou se `x` e `y` são `false`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2156">The result of `!=` is `false` if both `x` and `y` are `true` or if both `x` and `y` are `false`.</span></span> <span data-ttu-id="dd693-2157">Caso contrário, o resultado será `true`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2157">Otherwise, the result is `true`.</span></span> <span data-ttu-id="dd693-2158">Quando os operandos são do tipo `bool`, o operador `!=` produz o mesmo resultado que o operador de `^`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2158">When the operands are of type `bool`, the `!=` operator produces the same result as the `^` operator.</span></span>

### <a name="enumeration-comparison-operators"></a><span data-ttu-id="dd693-2159">Operadores de comparação de enumeração</span><span class="sxs-lookup"><span data-stu-id="dd693-2159">Enumeration comparison operators</span></span>

<span data-ttu-id="dd693-2160">Cada tipo de enumeração fornece implicitamente os seguintes operadores de comparação predefinidos:</span><span class="sxs-lookup"><span data-stu-id="dd693-2160">Every enumeration type implicitly provides the following predefined comparison operators:</span></span>
```csharp
bool operator ==(E x, E y);
bool operator !=(E x, E y);
bool operator <(E x, E y);
bool operator >(E x, E y);
bool operator <=(E x, E y);
bool operator >=(E x, E y);
```

<span data-ttu-id="dd693-2161">O resultado da avaliação de `x op y`, em que `x` e `y` são expressões de um tipo de enumeração `E` com um tipo subjacente `U`, e `op` é um dos operadores de comparação, é exatamente o mesmo que avaliar `((U)x) op ((U)y)`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2161">The result of evaluating `x op y`, where `x` and `y` are expressions of an enumeration type `E` with an underlying type `U`, and `op` is one of the comparison operators, is exactly the same as evaluating `((U)x) op ((U)y)`.</span></span> <span data-ttu-id="dd693-2162">Em outras palavras, os operadores de comparação de tipo de enumeração apenas comparam os valores integrais subjacentes dos dois operandos.</span><span class="sxs-lookup"><span data-stu-id="dd693-2162">In other words, the enumeration type comparison operators simply compare the underlying integral values of the two operands.</span></span>

### <a name="reference-type-equality-operators"></a><span data-ttu-id="dd693-2163">Operadores de igualdade de tipo de referência</span><span class="sxs-lookup"><span data-stu-id="dd693-2163">Reference type equality operators</span></span>

<span data-ttu-id="dd693-2164">Os operadores de igualdade do tipo de referência predefinido são:</span><span class="sxs-lookup"><span data-stu-id="dd693-2164">The predefined reference type equality operators are:</span></span>
```csharp
bool operator ==(object x, object y);
bool operator !=(object x, object y);
```

<span data-ttu-id="dd693-2165">Os operadores retornam o resultado da comparação das duas referências para igualdade ou não igualdade.</span><span class="sxs-lookup"><span data-stu-id="dd693-2165">The operators return the result of comparing the two references for equality or non-equality.</span></span>

<span data-ttu-id="dd693-2166">Como os operadores de igualdade do tipo de referência predefinido aceitam operandos do tipo `object`, eles se aplicam a todos os tipos que não declaram os membros `operator ==` e `operator !=` aplicáveis.</span><span class="sxs-lookup"><span data-stu-id="dd693-2166">Since the predefined reference type equality operators accept operands of type `object`, they apply to all types that do not declare applicable `operator ==` and `operator !=` members.</span></span> <span data-ttu-id="dd693-2167">Por outro lado, qualquer operador de igualdade definido pelo usuário aplicável efetivamente oculta os operadores de igualdade do tipo de referência predefinido.</span><span class="sxs-lookup"><span data-stu-id="dd693-2167">Conversely, any applicable user-defined equality operators effectively hide the predefined reference type equality operators.</span></span>

<span data-ttu-id="dd693-2168">Os operadores de igualdade do tipo de referência predefinidos exigem um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="dd693-2168">The predefined reference type equality operators require one of the following:</span></span>

*  <span data-ttu-id="dd693-2169">Ambos os operandos são um valor de um tipo conhecido como um *reference_type* ou o `null`literal.</span><span class="sxs-lookup"><span data-stu-id="dd693-2169">Both operands are a value of a type known to be a *reference_type* or the literal `null`.</span></span> <span data-ttu-id="dd693-2170">Além disso, uma conversão de referência explícita ([conversões de referência explícitas](conversions.md#explicit-reference-conversions)) existe no tipo de um dos operandos para o tipo do outro operando.</span><span class="sxs-lookup"><span data-stu-id="dd693-2170">Furthermore, an explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) exists from the type of either operand to the type of the other operand.</span></span>
*  <span data-ttu-id="dd693-2171">Um operando é um valor do tipo `T` em que `T` é uma *type_parameter* e o outro operando é o `null`literal.</span><span class="sxs-lookup"><span data-stu-id="dd693-2171">One operand is a value of type `T` where `T` is a *type_parameter* and the other operand is the literal `null`.</span></span> <span data-ttu-id="dd693-2172">Além disso `T` não tem a restrição de tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="dd693-2172">Furthermore `T` does not have the value type constraint.</span></span>

<span data-ttu-id="dd693-2173">A menos que uma dessas condições seja verdadeira, ocorre um erro de tempo de associação.</span><span class="sxs-lookup"><span data-stu-id="dd693-2173">Unless one of these conditions are true, a binding-time error occurs.</span></span> <span data-ttu-id="dd693-2174">As principais implicações dessas regras são:</span><span class="sxs-lookup"><span data-stu-id="dd693-2174">Notable implications of these rules are:</span></span>

*  <span data-ttu-id="dd693-2175">É um erro de tempo de ligação para usar os operadores de igualdade do tipo de referência predefinido para comparar duas referências que são conhecidas para serem diferentes no tempo de associação.</span><span class="sxs-lookup"><span data-stu-id="dd693-2175">It is a binding-time error to use the predefined reference type equality operators to compare two references that are known to be different at binding-time.</span></span> <span data-ttu-id="dd693-2176">Por exemplo, se os tipos de tempo de Associação dos operandos forem dois tipos de classe `A` e `B`, e se nem `A` nem `B` derivar de outro, seria impossível para os dois operandos referenciarem o mesmo objeto.</span><span class="sxs-lookup"><span data-stu-id="dd693-2176">For example, if the binding-time types of the operands are two class types `A` and `B`, and if neither `A` nor `B` derives from the other, then it would be impossible for the two operands to reference the same object.</span></span> <span data-ttu-id="dd693-2177">Assim, a operação é considerada um erro de tempo de associação.</span><span class="sxs-lookup"><span data-stu-id="dd693-2177">Thus, the operation is considered a binding-time error.</span></span>
*  <span data-ttu-id="dd693-2178">Os operadores de igualdade do tipo de referência predefinido não permitem que os operandos de tipo de valor sejam comparados.</span><span class="sxs-lookup"><span data-stu-id="dd693-2178">The predefined reference type equality operators do not permit value type operands to be compared.</span></span> <span data-ttu-id="dd693-2179">Portanto, a menos que um tipo de struct declare seus próprios operadores de igualdade, não é possível comparar valores desse tipo struct.</span><span class="sxs-lookup"><span data-stu-id="dd693-2179">Therefore, unless a struct type declares its own equality operators, it is not possible to compare values of that struct type.</span></span>
*  <span data-ttu-id="dd693-2180">Os operadores de igualdade do tipo de referência predefinido nunca causam a ocorrência de operações Boxing para seus operandos.</span><span class="sxs-lookup"><span data-stu-id="dd693-2180">The predefined reference type equality operators never cause boxing operations to occur for their operands.</span></span> <span data-ttu-id="dd693-2181">Seria sem sentido executar essas operações boxing, já que as referências às instâncias emolduradas recentemente alocadas seriam necessariamente diferentes de todas as outras referências.</span><span class="sxs-lookup"><span data-stu-id="dd693-2181">It would be meaningless to perform such boxing operations, since references to the newly allocated boxed instances would necessarily differ from all other references.</span></span>
*  <span data-ttu-id="dd693-2182">Se um operando de um tipo de parâmetro de tipo `T` for comparado com `null`e o tipo de tempo de execução de `T` for um tipo de valor, o resultado da comparação será `false`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2182">If an operand of a type parameter type `T` is compared to `null`, and the run-time type of `T` is a value type, the result of the comparison is `false`.</span></span>

<span data-ttu-id="dd693-2183">O exemplo a seguir verifica se um argumento de um tipo de parâmetro de tipo irrestrito é `null`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2183">The following example checks whether an argument of an unconstrained type parameter type is `null`.</span></span>
```csharp
class C<T>
{
    void F(T x) {
        if (x == null) throw new ArgumentNullException();
        ...
    }
}
```

<span data-ttu-id="dd693-2184">A construção `x == null` é permitida embora `T` possa representar um tipo de valor, e o resultado é simplesmente definido como `false` quando `T` é um tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="dd693-2184">The `x == null` construct is permitted even though `T` could represent a value type, and the result is simply defined to be `false` when `T` is a value type.</span></span>

<span data-ttu-id="dd693-2185">Para uma operação do formulário `x == y` ou `x != y`, se qualquer `operator ==` aplicável ou `operator !=` existir, as regras de resolução de sobrecarga de operador ([resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)) selecionarão esse operador em vez do operador de igualdade do tipo de referência predefinido.</span><span class="sxs-lookup"><span data-stu-id="dd693-2185">For an operation of the form `x == y` or `x != y`, if any applicable `operator ==` or `operator !=` exists, the operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) rules will select that operator instead of the predefined reference type equality operator.</span></span> <span data-ttu-id="dd693-2186">No entanto, sempre é possível selecionar o operador de igualdade do tipo de referência predefinido, convertendo explicitamente um ou ambos os operandos para o tipo `object`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2186">However, it is always possible to select the predefined reference type equality operator by explicitly casting one or both of the operands to type `object`.</span></span> <span data-ttu-id="dd693-2187">O exemplo</span><span class="sxs-lookup"><span data-stu-id="dd693-2187">The example</span></span>
```csharp
using System;

class Test
{
    static void Main() {
        string s = "Test";
        string t = string.Copy(s);
        Console.WriteLine(s == t);
        Console.WriteLine((object)s == t);
        Console.WriteLine(s == (object)t);
        Console.WriteLine((object)s == (object)t);
    }
}
```
<span data-ttu-id="dd693-2188">produz a saída</span><span class="sxs-lookup"><span data-stu-id="dd693-2188">produces the output</span></span>
```console
True
False
False
False
```

<span data-ttu-id="dd693-2189">As variáveis `s` e `t` referem-se a duas instâncias de `string` distintas contendo os mesmos caracteres.</span><span class="sxs-lookup"><span data-stu-id="dd693-2189">The `s` and `t` variables refer to two distinct `string` instances containing the same characters.</span></span> <span data-ttu-id="dd693-2190">A primeira comparação gera `True` porque o operador de igualdade de cadeia de caracteres predefinido ([operadores de igualdade de cadeia de caracteres](expressions.md#string-equality-operators)) é selecionado quando ambos os operandos são do tipo `string`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2190">The first comparison outputs `True` because the predefined string equality operator ([String equality operators](expressions.md#string-equality-operators)) is selected when both operands are of type `string`.</span></span> <span data-ttu-id="dd693-2191">As comparações restantes são todas `False` de saída porque o tipo de referência predefinido operador de igualdade é selecionado quando um ou ambos os operandos são do tipo `object`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2191">The remaining comparisons all output `False` because the predefined reference type equality operator is selected when one or both of the operands are of type `object`.</span></span>

<span data-ttu-id="dd693-2192">Observe que a técnica acima não é significativa para tipos de valor.</span><span class="sxs-lookup"><span data-stu-id="dd693-2192">Note that the above technique is not meaningful for value types.</span></span> <span data-ttu-id="dd693-2193">O exemplo</span><span class="sxs-lookup"><span data-stu-id="dd693-2193">The example</span></span>
```csharp
class Test
{
    static void Main() {
        int i = 123;
        int j = 123;
        System.Console.WriteLine((object)i == (object)j);
    }
}
```
<span data-ttu-id="dd693-2194">gera `False` porque as conversões criam referências a duas instâncias separadas de valores de `int` boxed.</span><span class="sxs-lookup"><span data-stu-id="dd693-2194">outputs `False` because the casts create references to two separate instances of boxed `int` values.</span></span>

### <a name="string-equality-operators"></a><span data-ttu-id="dd693-2195">Operadores de igualdade de cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="dd693-2195">String equality operators</span></span>

<span data-ttu-id="dd693-2196">Os operadores de igualdade de cadeia de caracteres predefinidos são:</span><span class="sxs-lookup"><span data-stu-id="dd693-2196">The predefined string equality operators are:</span></span>
```csharp
bool operator ==(string x, string y);
bool operator !=(string x, string y);
```

<span data-ttu-id="dd693-2197">Dois valores de `string` são considerados iguais quando um dos seguintes é verdadeiro:</span><span class="sxs-lookup"><span data-stu-id="dd693-2197">Two `string` values are considered equal when one of the following is true:</span></span>

*  <span data-ttu-id="dd693-2198">Ambos os valores são `null`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2198">Both values are `null`.</span></span>
*  <span data-ttu-id="dd693-2199">Ambos os valores são referências não nulas a instâncias de cadeia de caracteres que têm comprimentos idênticos e caracteres idênticos em cada posição de caractere.</span><span class="sxs-lookup"><span data-stu-id="dd693-2199">Both values are non-null references to string instances that have identical lengths and identical characters in each character position.</span></span>

<span data-ttu-id="dd693-2200">Os operadores de igualdade de cadeia de caracteres comparam valores de cadeia de caracteres em vez de referências</span><span class="sxs-lookup"><span data-stu-id="dd693-2200">The string equality operators compare string values rather than string references.</span></span> <span data-ttu-id="dd693-2201">Quando duas instâncias de cadeias de caracteres separadas contêm exatamente a mesma sequência de caracteres, os valores das cadeias são iguais, mas as referências são diferentes.</span><span class="sxs-lookup"><span data-stu-id="dd693-2201">When two separate string instances contain the exact same sequence of characters, the values of the strings are equal, but the references are different.</span></span> <span data-ttu-id="dd693-2202">Conforme descrito em [operadores de igualdade de tipo de referência](expressions.md#reference-type-equality-operators), os operadores de igualdade de tipo de referência podem ser usados para comparar referências de cadeia de caracteres em vez de valores de cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="dd693-2202">As described in [Reference type equality operators](expressions.md#reference-type-equality-operators), the reference type equality operators can be used to compare string references instead of string values.</span></span>

### <a name="delegate-equality-operators"></a><span data-ttu-id="dd693-2203">Delegar operadores de igualdade</span><span class="sxs-lookup"><span data-stu-id="dd693-2203">Delegate equality operators</span></span>

<span data-ttu-id="dd693-2204">Cada tipo de delegado fornece implicitamente os seguintes operadores de comparação predefinidos:</span><span class="sxs-lookup"><span data-stu-id="dd693-2204">Every delegate type implicitly provides the following predefined comparison operators:</span></span>

```csharp
bool operator ==(System.Delegate x, System.Delegate y);
bool operator !=(System.Delegate x, System.Delegate y);
```

<span data-ttu-id="dd693-2205">Duas instâncias de delegado são consideradas iguais da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="dd693-2205">Two delegate instances are considered equal as follows:</span></span>

*  <span data-ttu-id="dd693-2206">Se uma das instâncias de delegado for `null`, elas serão iguais se e somente se ambas forem `null`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2206">If either of the delegate instances is `null`, they are equal if and only if both are `null`.</span></span>
*  <span data-ttu-id="dd693-2207">Se os delegados tiverem um tipo de tempo de execução diferente, nunca serão iguais.</span><span class="sxs-lookup"><span data-stu-id="dd693-2207">If the delegates have different run-time type they are never equal.</span></span>
*  <span data-ttu-id="dd693-2208">Se ambas as instâncias de delegado tiverem uma lista de invocação ([declarações de delegado](delegates.md#delegate-declarations)), essas instâncias serão iguais se e somente se suas listas de invocação tiverem o mesmo comprimento, e cada entrada na lista de invocação de uma for igual (conforme definido abaixo) à entrada correspondente, na ordem, na lista de invocação do outro.</span><span class="sxs-lookup"><span data-stu-id="dd693-2208">If both of the delegate instances have an invocation list ([Delegate declarations](delegates.md#delegate-declarations)), those instances are equal if and only if their invocation lists are the same length, and each entry in one's invocation list is equal (as defined below) to the corresponding entry, in order, in the other's invocation list.</span></span>

<span data-ttu-id="dd693-2209">As regras a seguir regem a igualdade de entradas da lista de invocação:</span><span class="sxs-lookup"><span data-stu-id="dd693-2209">The following rules govern the equality of invocation list entries:</span></span>

*  <span data-ttu-id="dd693-2210">Se duas entradas da lista de invocação fizerem referência ao mesmo método estático, as entradas serão iguais.</span><span class="sxs-lookup"><span data-stu-id="dd693-2210">If two invocation list entries both refer to the same static method then the entries are equal.</span></span>
*  <span data-ttu-id="dd693-2211">Se duas entradas da lista de invocação fizerem referência ao mesmo método não-estático no mesmo objeto de destino (conforme definido pelos operadores de igualdade de referência), as entradas serão iguais.</span><span class="sxs-lookup"><span data-stu-id="dd693-2211">If two invocation list entries both refer to the same non-static method on the same target object (as defined by the reference equality operators) then the entries are equal.</span></span>
*  <span data-ttu-id="dd693-2212">As entradas da lista de invocação produzidas da avaliação de *anonymous_method_expression*s ou *lambda_expressions*semanticamente idênticas com o mesmo conjunto (possivelmente vazio) de instâncias de variáveis externas capturadas são permitidas (mas não obrigatórias) para serem iguais.</span><span class="sxs-lookup"><span data-stu-id="dd693-2212">Invocation list entries produced from evaluation of semantically identical *anonymous_method_expression*s or *lambda_expression*s with the same (possibly empty) set of captured outer variable instances are permitted (but not required) to be equal.</span></span>

### <a name="equality-operators-and-null"></a><span data-ttu-id="dd693-2213">Operadores de igualdade e nulo</span><span class="sxs-lookup"><span data-stu-id="dd693-2213">Equality operators and null</span></span>

<span data-ttu-id="dd693-2214">Os operadores `==` e `!=` permitem que um operando seja um valor de um tipo anulável e o outro para ser o `null` literal, mesmo que nenhum operador predefinido ou definido pelo usuário (em forma não elevada ou levantada) exista para a operação.</span><span class="sxs-lookup"><span data-stu-id="dd693-2214">The `==` and `!=` operators permit one operand to be a value of a nullable type and the other to be the `null` literal, even if no predefined or user-defined operator (in unlifted or lifted form) exists for the operation.</span></span>

<span data-ttu-id="dd693-2215">Para uma operação de um dos formulários</span><span class="sxs-lookup"><span data-stu-id="dd693-2215">For an operation of one of the forms</span></span>
```csharp
x == null
null == x
x != null
null != x
```
<span data-ttu-id="dd693-2216">em que `x` é uma expressão de um tipo anulável, se a resolução de sobrecarga do operador ([resolução de sobrecarga do operador binário](expressions.md#binary-operator-overload-resolution)) falhar ao localizar um operador aplicável, o resultado será calculado da propriedade `HasValue` de `x`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2216">where `x` is an expression of a nullable type, if operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) fails to find an applicable operator, the result is instead computed from the `HasValue` property of `x`.</span></span> <span data-ttu-id="dd693-2217">Especificamente, as duas primeiras formas são convertidas em `!x.HasValue`, e os últimos dois formulários são traduzidos em `x.HasValue`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2217">Specifically, the first two forms are translated into `!x.HasValue`, and last two forms are translated into `x.HasValue`.</span></span>

### <a name="the-is-operator"></a><span data-ttu-id="dd693-2218">O operador is</span><span class="sxs-lookup"><span data-stu-id="dd693-2218">The is operator</span></span>

<span data-ttu-id="dd693-2219">O operador `is` é usado para verificar dinamicamente se o tipo de tempo de execução de um objeto é compatível com um determinado tipo.</span><span class="sxs-lookup"><span data-stu-id="dd693-2219">The `is` operator is used to dynamically check if the run-time type of an object is compatible with a given type.</span></span> <span data-ttu-id="dd693-2220">O resultado da operação `E is T`, em que `E` é uma expressão e `T` é um tipo, é um valor booliano que indica se `E` pode ser convertido com êxito no tipo `T` por uma conversão de referência, uma conversão boxing ou uma conversão unboxing.</span><span class="sxs-lookup"><span data-stu-id="dd693-2220">The result of the operation `E is T`, where `E` is an expression and `T` is a type, is a boolean value indicating whether `E` can successfully be converted to type `T` by a reference conversion, a boxing conversion, or an unboxing conversion.</span></span> <span data-ttu-id="dd693-2221">A operação é avaliada da seguinte maneira, após os argumentos de tipo terem sido substituídos por todos os parâmetros de tipo:</span><span class="sxs-lookup"><span data-stu-id="dd693-2221">The operation is evaluated as follows, after type arguments have been substituted for all type parameters:</span></span>

*  <span data-ttu-id="dd693-2222">Se `E` for uma função anônima, ocorrerá um erro em tempo de compilação</span><span class="sxs-lookup"><span data-stu-id="dd693-2222">If `E` is an anonymous function, a compile-time error occurs</span></span>
*  <span data-ttu-id="dd693-2223">Se `E` for um grupo de métodos ou o literal de `null`, de se o tipo de `E` for um tipo de referência ou um tipo anulável e o valor de `E` for NULL, o resultado será false.</span><span class="sxs-lookup"><span data-stu-id="dd693-2223">If `E` is a method group or the `null` literal, of if the type of `E` is a reference type or a nullable type and the value of `E` is null, the result is false.</span></span>
*  <span data-ttu-id="dd693-2224">Caso contrário, permita que `D` represente o tipo dinâmico de `E` da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="dd693-2224">Otherwise, let `D` represent the dynamic type of `E` as follows:</span></span>
   * <span data-ttu-id="dd693-2225">Se o tipo de `E` for um tipo de referência, `D` será o tipo de tempo de execução da referência de instância por `E`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2225">If the type of `E` is a reference type, `D` is the run-time type of the instance reference by `E`.</span></span>
   * <span data-ttu-id="dd693-2226">Se o tipo de `E` for um tipo anulável, `D` será o tipo subjacente desse tipo anulável.</span><span class="sxs-lookup"><span data-stu-id="dd693-2226">If the type of `E` is a nullable type, `D` is the underlying type of that nullable type.</span></span>
   * <span data-ttu-id="dd693-2227">Se o tipo de `E` for um tipo de valor não anulável, `D` será o tipo de `E`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2227">If the type of `E` is a non-nullable value type, `D` is the type of `E`.</span></span>
*  <span data-ttu-id="dd693-2228">O resultado da operação depende `D` e `T` da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="dd693-2228">The result of the operation depends on `D` and `T` as follows:</span></span>
   * <span data-ttu-id="dd693-2229">Se `T` for um tipo de referência, o resultado será true se `D` e `T` forem do mesmo tipo, se `D` for um tipo de referência e uma conversão de referência implícita de `D` para `T` existir ou se `D` for um tipo de valor e uma conversão de Boxing de `D` para `T` existir.</span><span class="sxs-lookup"><span data-stu-id="dd693-2229">If `T` is a reference type, the result is true if `D` and `T` are the same type, if `D` is a reference type and an implicit reference conversion from `D` to `T` exists, or if `D` is a value type and a boxing conversion from `D` to `T` exists.</span></span>
   * <span data-ttu-id="dd693-2230">Se `T` for um tipo anulável, o resultado será true se `D` for o tipo subjacente de `T`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2230">If `T` is a nullable type, the result is true if `D` is the underlying type of `T`.</span></span>
   * <span data-ttu-id="dd693-2231">Se `T` for um tipo de valor não anulável, o resultado será true se `D` e `T` forem do mesmo tipo.</span><span class="sxs-lookup"><span data-stu-id="dd693-2231">If `T` is a non-nullable value type, the result is true if `D` and `T` are the same type.</span></span>
   * <span data-ttu-id="dd693-2232">Caso contrário, o resultado será false.</span><span class="sxs-lookup"><span data-stu-id="dd693-2232">Otherwise, the result is false.</span></span>

<span data-ttu-id="dd693-2233">Observe que as conversões definidas pelo usuário não são consideradas pelo operador de `is`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2233">Note that user defined conversions, are not considered by the `is` operator.</span></span>

### <a name="the-as-operator"></a><span data-ttu-id="dd693-2234">O operador as</span><span class="sxs-lookup"><span data-stu-id="dd693-2234">The as operator</span></span>

<span data-ttu-id="dd693-2235">O operador `as` é usado para converter explicitamente um valor em um determinado tipo de referência ou tipo anulável.</span><span class="sxs-lookup"><span data-stu-id="dd693-2235">The `as` operator is used to explicitly convert a value to a given reference type or nullable type.</span></span> <span data-ttu-id="dd693-2236">Ao contrário de uma expressão de conversão ([expressões de conversão](expressions.md#cast-expressions)), o operador de `as` nunca gera uma exceção.</span><span class="sxs-lookup"><span data-stu-id="dd693-2236">Unlike a cast expression ([Cast expressions](expressions.md#cast-expressions)), the `as` operator never throws an exception.</span></span> <span data-ttu-id="dd693-2237">Em vez disso, se a conversão indicada não for possível, o valor resultante será `null`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2237">Instead, if the indicated conversion is not possible, the resulting value is `null`.</span></span>

<span data-ttu-id="dd693-2238">Em uma operação do formulário `E as T`, `E` deve ser uma expressão e `T` deve ser um tipo de referência, um parâmetro de tipo conhecido como um tipo de referência ou um tipo anulável.</span><span class="sxs-lookup"><span data-stu-id="dd693-2238">In an operation of the form `E as T`, `E` must be an expression and `T` must be a reference type, a type parameter known to be a reference type, or a nullable type.</span></span> <span data-ttu-id="dd693-2239">Além disso, pelo menos um dos seguintes deve ser verdadeiro ou um erro de tempo de compilação ocorre:</span><span class="sxs-lookup"><span data-stu-id="dd693-2239">Furthermore, at least one of the following must be true, or otherwise a compile-time error occurs:</span></span>

*  <span data-ttu-id="dd693-2240">Uma identidade ([conversão de identidade](conversions.md#identity-conversion)), anulável implícita[(conversões anuláveis implícitas](conversions.md#implicit-nullable-conversions)), referência implícita ([conversões de referência implícita](conversions.md#implicit-reference-conversions)), conversão de Boxing ([conversões de conversão](conversions.md#boxing-conversions)de Boxing), nulo explícito ([conversões anuláveis explícitas](conversions.md#explicit-nullable-conversions)), a referência explícita ([conversões de referência explícitas](conversions.md#explicit-reference-conversions)) ou não há conversões unboxing ([unboxing](conversions.md#unboxing-conversions)) de `E` para `T`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2240">An identity ([Identity conversion](conversions.md#identity-conversion)), implicit nullable ([Implicit nullable conversions](conversions.md#implicit-nullable-conversions)), implicit reference ([Implicit reference conversions](conversions.md#implicit-reference-conversions)), boxing ([Boxing conversions](conversions.md#boxing-conversions)), explicit nullable ([Explicit nullable conversions](conversions.md#explicit-nullable-conversions)), explicit reference ([Explicit reference conversions](conversions.md#explicit-reference-conversions)), or unboxing ([Unboxing conversions](conversions.md#unboxing-conversions)) conversion exists from `E` to `T`.</span></span>
*  <span data-ttu-id="dd693-2241">O tipo de `E` ou `T` é um tipo aberto.</span><span class="sxs-lookup"><span data-stu-id="dd693-2241">The type of `E` or `T` is an open type.</span></span>
*  <span data-ttu-id="dd693-2242">`E` é o literal de `null`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2242">`E` is the `null` literal.</span></span>

<span data-ttu-id="dd693-2243">Se o tipo de tempo de compilação de `E` não for `dynamic`, a operação `E as T` produzirá o mesmo resultado que</span><span class="sxs-lookup"><span data-stu-id="dd693-2243">If the compile-time type of `E` is not `dynamic`, the operation `E as T` produces the same result as</span></span>
```csharp
E is T ? (T)(E) : (T)null
```
<span data-ttu-id="dd693-2244">exceto que `E` é avaliado apenas uma vez.</span><span class="sxs-lookup"><span data-stu-id="dd693-2244">except that `E` is only evaluated once.</span></span> <span data-ttu-id="dd693-2245">O compilador pode ser esperado para otimizar `E as T` executar no máximo uma verificação de tipo dinâmico em oposição às duas verificações de tipo dinâmico implícitas pela expansão acima.</span><span class="sxs-lookup"><span data-stu-id="dd693-2245">The compiler can be expected to optimize `E as T` to perform at most one dynamic type check as opposed to the two dynamic type checks implied by the expansion above.</span></span>

<span data-ttu-id="dd693-2246">Se o tipo de tempo de compilação de `E` for `dynamic`, diferentemente do operador de conversão, o operador de `as` não será vinculado dinamicamente ([associação dinâmica](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="dd693-2246">If the compile-time type of `E` is `dynamic`, unlike the cast operator the `as` operator is not dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="dd693-2247">Portanto, a expansão, nesse caso, é:</span><span class="sxs-lookup"><span data-stu-id="dd693-2247">Therefore the expansion in this case is:</span></span>
```csharp
E is T ? (T)(object)(E) : (T)null
```

<span data-ttu-id="dd693-2248">Observe que algumas conversões, como conversões definidas pelo usuário, não são possíveis com o operador de `as` e, em vez disso, devem ser executadas usando expressões de conversão.</span><span class="sxs-lookup"><span data-stu-id="dd693-2248">Note that some conversions, such as user defined conversions, are not possible with the `as` operator and should instead be performed using cast expressions.</span></span>

<span data-ttu-id="dd693-2249">No exemplo</span><span class="sxs-lookup"><span data-stu-id="dd693-2249">In the example</span></span>
```csharp
class X
{

    public string F(object o) {
        return o as string;        // OK, string is a reference type
    }

    public T G<T>(object o) where T: Attribute {
        return o as T;             // Ok, T has a class constraint
    }

    public U H<U>(object o) {
        return o as U;             // Error, U is unconstrained 
    }
}
```
<span data-ttu-id="dd693-2250">o parâmetro de tipo `T` de `G` é conhecido como um tipo de referência, porque ele tem a restrição de classe.</span><span class="sxs-lookup"><span data-stu-id="dd693-2250">the type parameter `T` of `G` is known to be a reference type, because it has the class constraint.</span></span> <span data-ttu-id="dd693-2251">No entanto, o parâmetro de tipo `U` de `H`; Portanto, o uso do operador de `as` no `H` não é permitido.</span><span class="sxs-lookup"><span data-stu-id="dd693-2251">The type parameter `U` of `H` is not however; hence the use of the `as` operator in `H` is disallowed.</span></span>

## <a name="logical-operators"></a><span data-ttu-id="dd693-2252">Operadores lógicos</span><span class="sxs-lookup"><span data-stu-id="dd693-2252">Logical operators</span></span>

<span data-ttu-id="dd693-2253">Os operadores `&`, `^`e `|` são chamados de operadores lógicos.</span><span class="sxs-lookup"><span data-stu-id="dd693-2253">The `&`, `^`, and `|` operators are called the logical operators.</span></span>

```antlr
and_expression
    : equality_expression
    | and_expression '&' equality_expression
    ;

exclusive_or_expression
    : and_expression
    | exclusive_or_expression '^' and_expression
    ;

inclusive_or_expression
    : exclusive_or_expression
    | inclusive_or_expression '|' exclusive_or_expression
    ;
```

<span data-ttu-id="dd693-2254">Se um operando de um operador lógico tiver o tipo de tempo de compilação `dynamic`, a expressão será vinculada dinamicamente ([associação dinâmica](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="dd693-2254">If an operand of a logical operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="dd693-2255">Nesse caso, o tipo de tempo de compilação da expressão é `dynamic`e a resolução descrita abaixo ocorrerá em tempo de execução usando o tipo de tempo de execução desses operandos que têm o tipo de tempo de compilação `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2255">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="dd693-2256">Para uma operação do formulário `x op y`, em que `op` é um dos operadores lógicos, a resolução de sobrecarga ([resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)) é aplicada para selecionar uma implementação de operador específica.</span><span class="sxs-lookup"><span data-stu-id="dd693-2256">For an operation of the form `x op y`, where `op` is one of the logical operators, overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="dd693-2257">Os operandos são convertidos nos tipos de parâmetro do operador selecionado e o tipo de resultado é o tipo de retorno do operador.</span><span class="sxs-lookup"><span data-stu-id="dd693-2257">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="dd693-2258">Os operadores lógicos predefinidos são descritos nas seções a seguir.</span><span class="sxs-lookup"><span data-stu-id="dd693-2258">The predefined logical operators are described in the following sections.</span></span>

### <a name="integer-logical-operators"></a><span data-ttu-id="dd693-2259">Operadores lógicos de inteiros</span><span class="sxs-lookup"><span data-stu-id="dd693-2259">Integer logical operators</span></span>

<span data-ttu-id="dd693-2260">Os operadores lógicos de inteiro predefinidos são:</span><span class="sxs-lookup"><span data-stu-id="dd693-2260">The predefined integer logical operators are:</span></span>
```csharp
int operator &(int x, int y);
uint operator &(uint x, uint y);
long operator &(long x, long y);
ulong operator &(ulong x, ulong y);

int operator |(int x, int y);
uint operator |(uint x, uint y);
long operator |(long x, long y);
ulong operator |(ulong x, ulong y);

int operator ^(int x, int y);
uint operator ^(uint x, uint y);
long operator ^(long x, long y);
ulong operator ^(ulong x, ulong y);
```

<span data-ttu-id="dd693-2261">O operador de `&` computa o `AND` lógico de bits de dois operandos, o operador de `|` computa os `OR` lógicos de bit-de-bit dos dois operandos, e o operador `^` computa a `OR` exclusiva de bits lógicas dos dois operandos.</span><span class="sxs-lookup"><span data-stu-id="dd693-2261">The `&` operator computes the bitwise logical `AND` of the two operands, the `|` operator computes the bitwise logical `OR` of the two operands, and the `^` operator computes the bitwise logical exclusive `OR` of the two operands.</span></span> <span data-ttu-id="dd693-2262">Não são possíveis estouros dessas operações.</span><span class="sxs-lookup"><span data-stu-id="dd693-2262">No overflows are possible from these operations.</span></span>

### <a name="enumeration-logical-operators"></a><span data-ttu-id="dd693-2263">Operadores lógicos de enumeração</span><span class="sxs-lookup"><span data-stu-id="dd693-2263">Enumeration logical operators</span></span>

<span data-ttu-id="dd693-2264">Cada tipo de enumeração `E` fornece implicitamente os seguintes operadores lógicos predefinidos:</span><span class="sxs-lookup"><span data-stu-id="dd693-2264">Every enumeration type `E` implicitly provides the following predefined logical operators:</span></span>

```csharp
E operator &(E x, E y);
E operator |(E x, E y);
E operator ^(E x, E y);
```

<span data-ttu-id="dd693-2265">O resultado da avaliação de `x op y`, em que `x` e `y` são expressões de um tipo de enumeração `E` com um tipo subjacente `U`e `op` é um dos operadores lógicos, é exatamente o mesmo que avaliar `(E)((U)x op (U)y)`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2265">The result of evaluating `x op y`, where `x` and `y` are expressions of an enumeration type `E` with an underlying type `U`, and `op` is one of the logical operators, is exactly the same as evaluating `(E)((U)x op (U)y)`.</span></span> <span data-ttu-id="dd693-2266">Em outras palavras, os operadores lógicos do tipo de enumeração simplesmente executam a operação lógica no tipo subjacente dos dois operandos.</span><span class="sxs-lookup"><span data-stu-id="dd693-2266">In other words, the enumeration type logical operators simply perform the logical operation on the underlying type of the two operands.</span></span>

### <a name="boolean-logical-operators"></a><span data-ttu-id="dd693-2267">Operadores lógicos boolianos</span><span class="sxs-lookup"><span data-stu-id="dd693-2267">Boolean logical operators</span></span>

<span data-ttu-id="dd693-2268">Os operadores lógicos boolianos predefinidos são:</span><span class="sxs-lookup"><span data-stu-id="dd693-2268">The predefined boolean logical operators are:</span></span>
```csharp
bool operator &(bool x, bool y);
bool operator |(bool x, bool y);
bool operator ^(bool x, bool y);
```

<span data-ttu-id="dd693-2269">O resultado de `x & y` será `true` se ambos `x` e `y` forem `true`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2269">The result of `x & y` is `true` if both `x` and `y` are `true`.</span></span> <span data-ttu-id="dd693-2270">Caso contrário, o resultado será `false`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2270">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="dd693-2271">O resultado de `x | y` será `true` se `x` ou `y` for `true`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2271">The result of `x | y` is `true` if either `x` or `y` is `true`.</span></span> <span data-ttu-id="dd693-2272">Caso contrário, o resultado será `false`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2272">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="dd693-2273">O resultado de `x ^ y` será `true` se `x` for `true` e `y` for `false`ou `x` for `false` e `y` for `true`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2273">The result of `x ^ y` is `true` if `x` is `true` and `y` is `false`, or `x` is `false` and `y` is `true`.</span></span> <span data-ttu-id="dd693-2274">Caso contrário, o resultado será `false`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2274">Otherwise, the result is `false`.</span></span> <span data-ttu-id="dd693-2275">Quando os operandos são do tipo `bool`, o operador `^` computa o mesmo resultado que o operador de `!=`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2275">When the operands are of type `bool`, the `^` operator computes the same result as the `!=` operator.</span></span>

### <a name="nullable-boolean-logical-operators"></a><span data-ttu-id="dd693-2276">Operadores lógicos boolianos anuláveis</span><span class="sxs-lookup"><span data-stu-id="dd693-2276">Nullable boolean logical operators</span></span>

<span data-ttu-id="dd693-2277">O tipo booliano anulável `bool?` pode representar três valores, `true`, `false`e `null`e é conceitualmente semelhante ao tipo de três valores usado para expressões booleanas no SQL.</span><span class="sxs-lookup"><span data-stu-id="dd693-2277">The nullable boolean type `bool?` can represent three values, `true`, `false`, and `null`, and is conceptually similar to the three-valued type used for boolean expressions in SQL.</span></span> <span data-ttu-id="dd693-2278">Para garantir que os resultados produzidos pelos operadores de `&` e `|` para os operandos `bool?` sejam consistentes com a lógica de três valores do SQL, os seguintes operadores predefinidos são fornecidos:</span><span class="sxs-lookup"><span data-stu-id="dd693-2278">To ensure that the results produced by the `&` and `|` operators for `bool?` operands are consistent with SQL's three-valued logic, the following predefined operators are provided:</span></span>

```csharp
bool? operator &(bool? x, bool? y);
bool? operator |(bool? x, bool? y);
```

<span data-ttu-id="dd693-2279">A tabela a seguir lista os resultados produzidos por esses operadores para todas as combinações dos valores `true`, `false`e `null`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2279">The following table lists the results produced by these operators for all combinations of the values `true`, `false`, and `null`.</span></span>

| `x`     | `y`     | `x & y` | <code>x &#124; y</code> |
|:-------:|:-------:|:-------:|:-------:|
| `true`  | `true`  | `true`  | `true`  | 
| `true`  | `false` | `false` | `true`  | 
| `true`  | `null`  | `null`  | `true`  | 
| `false` | `true`  | `false` | `true`  | 
| `false` | `false` | `false` | `false` | 
| `false` | `null`  | `false` | `null`  | 
| `null`  | `true`  | `null`  | `true`  | 
| `null`  | `false` | `false` | `null`  | 
| `null`  | `null`  | `null`  | `null`  | 

## <a name="conditional-logical-operators"></a><span data-ttu-id="dd693-2280">Operadores lógicos condicionais</span><span class="sxs-lookup"><span data-stu-id="dd693-2280">Conditional logical operators</span></span>

<span data-ttu-id="dd693-2281">Os operadores de `&&` e `||` são chamados de operadores lógicos condicionais.</span><span class="sxs-lookup"><span data-stu-id="dd693-2281">The `&&` and `||` operators are called the conditional logical operators.</span></span> <span data-ttu-id="dd693-2282">Eles também são chamados de operadores lógicos de "curto-circuito".</span><span class="sxs-lookup"><span data-stu-id="dd693-2282">They are also called the "short-circuiting" logical operators.</span></span>

```antlr
conditional_and_expression
    : inclusive_or_expression
    | conditional_and_expression '&&' inclusive_or_expression
    ;

conditional_or_expression
    : conditional_and_expression
    | conditional_or_expression '||' conditional_and_expression
    ;
```

<span data-ttu-id="dd693-2283">Os operadores `&&` e `||` são versões condicionais dos operadores `&` e `|`:</span><span class="sxs-lookup"><span data-stu-id="dd693-2283">The `&&` and `||` operators are conditional versions of the `&` and `|` operators:</span></span>

*  <span data-ttu-id="dd693-2284">A operação `x && y` corresponde à operação `x & y`, exceto que `y` será avaliada somente se `x` não for `false`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2284">The operation `x && y` corresponds to the operation `x & y`, except that `y` is evaluated only if `x` is not `false`.</span></span>
*  <span data-ttu-id="dd693-2285">A operação `x || y` corresponde à operação `x | y`, exceto que `y` será avaliada somente se `x` não for `true`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2285">The operation `x || y` corresponds to the operation `x | y`, except that `y` is evaluated only if `x` is not `true`.</span></span>

<span data-ttu-id="dd693-2286">Se um operando de um operador lógico condicional tiver o tipo de tempo de compilação `dynamic`, a expressão será vinculada dinamicamente ([associação dinâmica](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="dd693-2286">If an operand of a conditional logical operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="dd693-2287">Nesse caso, o tipo de tempo de compilação da expressão é `dynamic`e a resolução descrita abaixo ocorrerá em tempo de execução usando o tipo de tempo de execução desses operandos que têm o tipo de tempo de compilação `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2287">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="dd693-2288">Uma operação do formulário `x && y` ou `x || y` é processada pela aplicação da resolução de sobrecarga ([resolução de sobrecarga do operador binário](expressions.md#binary-operator-overload-resolution)) como se a operação fosse gravada `x & y` ou `x | y`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2288">An operation of the form `x && y` or `x || y` is processed by applying overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) as if the operation was written `x & y` or `x | y`.</span></span> <span data-ttu-id="dd693-2289">Clique</span><span class="sxs-lookup"><span data-stu-id="dd693-2289">Then,</span></span>

*  <span data-ttu-id="dd693-2290">Se a resolução de sobrecarga não encontrar um único operador melhor, ou se a resolução de sobrecarga selecionar um dos operadores lógicos inteiros predefinidos, ocorrerá um erro de tempo de associação.</span><span class="sxs-lookup"><span data-stu-id="dd693-2290">If overload resolution fails to find a single best operator, or if overload resolution selects one of the predefined integer logical operators, a binding-time error occurs.</span></span>
*  <span data-ttu-id="dd693-2291">Caso contrário, se o operador selecionado for um dos operadores lógicos boolianos predefinidos ([operadores lógicos boolianos](expressions.md#boolean-logical-operators)) ou operadores lógicos boolianos anuláveis ([operadores lógicos boolianos anuláveis](expressions.md#nullable-boolean-logical-operators)), a operação será processada conforme descrito em [operadores lógicos condicionais boolianos](expressions.md#boolean-conditional-logical-operators).</span><span class="sxs-lookup"><span data-stu-id="dd693-2291">Otherwise, if the selected operator is one of the predefined boolean logical operators ([Boolean logical operators](expressions.md#boolean-logical-operators)) or nullable boolean logical operators ([Nullable boolean logical operators](expressions.md#nullable-boolean-logical-operators)), the operation is processed as described in [Boolean conditional logical operators](expressions.md#boolean-conditional-logical-operators).</span></span>
*  <span data-ttu-id="dd693-2292">Caso contrário, o operador selecionado é um operador definido pelo usuário e a operação é processada conforme descrito em [operadores lógicos condicionais definidos pelo usuário](expressions.md#user-defined-conditional-logical-operators).</span><span class="sxs-lookup"><span data-stu-id="dd693-2292">Otherwise, the selected operator is a user-defined operator, and the operation is processed as described in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators).</span></span>

<span data-ttu-id="dd693-2293">Não é possível sobrecarregar diretamente os operadores lógicos condicionais.</span><span class="sxs-lookup"><span data-stu-id="dd693-2293">It is not possible to directly overload the conditional logical operators.</span></span> <span data-ttu-id="dd693-2294">No entanto, como os operadores lógicos condicionais são avaliados em termos de operadores lógicos regulares, as sobrecargas dos operadores lógicos regulares são, com certas restrições, também consideradas sobrecargas dos operadores lógicos condicionais.</span><span class="sxs-lookup"><span data-stu-id="dd693-2294">However, because the conditional logical operators are evaluated in terms of the regular logical operators, overloads of the regular logical operators are, with certain restrictions, also considered overloads of the conditional logical operators.</span></span> <span data-ttu-id="dd693-2295">Isso é descrito mais detalhadamente em [operadores lógicos condicionais definidos pelo usuário](expressions.md#user-defined-conditional-logical-operators).</span><span class="sxs-lookup"><span data-stu-id="dd693-2295">This is described further in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators).</span></span>

### <a name="boolean-conditional-logical-operators"></a><span data-ttu-id="dd693-2296">Operadores lógicos condicionais boolianos</span><span class="sxs-lookup"><span data-stu-id="dd693-2296">Boolean conditional logical operators</span></span>

<span data-ttu-id="dd693-2297">Quando os operandos de `&&` ou `||` são do tipo `bool`, ou quando os operandos são de tipos que não definem um `operator &` ou `operator |`aplicável, mas definem conversões implícitas para `bool`, a operação é processada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="dd693-2297">When the operands of `&&` or `||` are of type `bool`, or when the operands are of types that do not define an applicable `operator &` or `operator |`, but do define implicit conversions to `bool`, the operation is processed as follows:</span></span>

*  <span data-ttu-id="dd693-2298">A operação `x && y` é avaliada como `x ? y : false`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2298">The operation `x && y` is evaluated as `x ? y : false`.</span></span> <span data-ttu-id="dd693-2299">Em outras palavras, `x` é avaliado pela primeira vez e convertido no tipo `bool`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2299">In other words, `x` is first evaluated and converted to type `bool`.</span></span> <span data-ttu-id="dd693-2300">Em seguida, se `x` for `true`, `y` será avaliado e convertido no tipo `bool`, e isso se tornará o resultado da operação.</span><span class="sxs-lookup"><span data-stu-id="dd693-2300">Then, if `x` is `true`, `y` is evaluated and converted to type `bool`, and this becomes the result of the operation.</span></span> <span data-ttu-id="dd693-2301">Caso contrário, o resultado da operação será `false`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2301">Otherwise, the result of the operation is `false`.</span></span>
*  <span data-ttu-id="dd693-2302">A operação `x || y` é avaliada como `x ? true : y`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2302">The operation `x || y` is evaluated as `x ? true : y`.</span></span> <span data-ttu-id="dd693-2303">Em outras palavras, `x` é avaliado pela primeira vez e convertido no tipo `bool`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2303">In other words, `x` is first evaluated and converted to type `bool`.</span></span> <span data-ttu-id="dd693-2304">Em seguida, se `x` for `true`, o resultado da operação será `true`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2304">Then, if `x` is `true`, the result of the operation is `true`.</span></span> <span data-ttu-id="dd693-2305">Caso contrário, `y` será avaliada e convertida no tipo `bool`, e isso se tornará o resultado da operação.</span><span class="sxs-lookup"><span data-stu-id="dd693-2305">Otherwise, `y` is evaluated and converted to type `bool`, and this becomes the result of the operation.</span></span>

### <a name="user-defined-conditional-logical-operators"></a><span data-ttu-id="dd693-2306">Operadores lógicos condicionais definidos pelo usuário</span><span class="sxs-lookup"><span data-stu-id="dd693-2306">User-defined conditional logical operators</span></span>

<span data-ttu-id="dd693-2307">Quando os operandos de `&&` ou `||` são de tipos que declaram um `operator &` ou `operator |`definido pelo usuário, os dois itens a seguir devem ser verdadeiros, em que `T` é o tipo no qual o operador selecionado é declarado:</span><span class="sxs-lookup"><span data-stu-id="dd693-2307">When the operands of `&&` or `||` are of types that declare an applicable user-defined `operator &` or `operator |`, both of the following must be true, where `T` is the type in which the selected operator is declared:</span></span>

*  <span data-ttu-id="dd693-2308">O tipo de retorno e o tipo de cada parâmetro do operador selecionado devem ser `T`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2308">The return type and the type of each parameter of the selected operator must be `T`.</span></span> <span data-ttu-id="dd693-2309">Em outras palavras, o operador deve calcular o `AND` lógico ou o `OR` lógico de dois operandos do tipo `T`e deve retornar um resultado do tipo `T`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2309">In other words, the operator must compute the logical `AND` or the logical `OR` of two operands of type `T`, and must return a result of type `T`.</span></span>
*  <span data-ttu-id="dd693-2310">`T` deve conter declarações de `operator true` e `operator false`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2310">`T` must contain declarations of `operator true` and `operator false`.</span></span>

<span data-ttu-id="dd693-2311">Um erro de tempo de associação ocorre se um desses requisitos não for atendido.</span><span class="sxs-lookup"><span data-stu-id="dd693-2311">A binding-time error occurs if either of these requirements is not satisfied.</span></span> <span data-ttu-id="dd693-2312">Caso contrário, a operação de `&&` ou `||` é avaliada pela combinação do `operator true` definido pelo usuário ou `operator false` com o operador selecionado definido pelo usuário:</span><span class="sxs-lookup"><span data-stu-id="dd693-2312">Otherwise, the `&&` or `||` operation is evaluated by combining the user-defined `operator true` or `operator false` with the selected user-defined operator:</span></span>

*  <span data-ttu-id="dd693-2313">A operação `x && y` é avaliada como `T.false(x) ? x : T.&(x, y)`, em que `T.false(x)` é uma invocação da `operator false` declarada em `T`e `T.&(x, y)` é uma invocação da `operator &`selecionada.</span><span class="sxs-lookup"><span data-stu-id="dd693-2313">The operation `x && y` is evaluated as `T.false(x) ? x : T.&(x, y)`, where `T.false(x)` is an invocation of the `operator false` declared in `T`, and `T.&(x, y)` is an invocation of the selected `operator &`.</span></span> <span data-ttu-id="dd693-2314">Em outras palavras, `x` é avaliado pela primeira vez e `operator false` é invocado no resultado para determinar se `x` é definitivamente falso.</span><span class="sxs-lookup"><span data-stu-id="dd693-2314">In other words, `x` is first evaluated and `operator false` is invoked on the result to determine if `x` is definitely false.</span></span> <span data-ttu-id="dd693-2315">Em seguida, se `x` for definitivamente false, o resultado da operação será o valor calculado anteriormente para `x`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2315">Then, if `x` is definitely false, the result of the operation is the value previously computed for `x`.</span></span> <span data-ttu-id="dd693-2316">Caso contrário, `y` é avaliado e o `operator &` selecionado é invocado no valor calculado anteriormente para `x` e o valor calculado para `y` para produzir o resultado da operação.</span><span class="sxs-lookup"><span data-stu-id="dd693-2316">Otherwise, `y` is evaluated, and the selected `operator &` is invoked on the value previously computed for `x` and the value computed for `y` to produce the result of the operation.</span></span>
*  <span data-ttu-id="dd693-2317">A operação `x || y` é avaliada como `T.true(x) ? x : T.|(x, y)`, em que `T.true(x)` é uma invocação da `operator true` declarada em `T`e `T.|(x,y)` é uma invocação da `operator|`selecionada.</span><span class="sxs-lookup"><span data-stu-id="dd693-2317">The operation `x || y` is evaluated as `T.true(x) ? x : T.|(x, y)`, where `T.true(x)` is an invocation of the `operator true` declared in `T`, and `T.|(x,y)` is an invocation of the selected `operator|`.</span></span> <span data-ttu-id="dd693-2318">Em outras palavras, `x` é avaliado pela primeira vez e `operator true` é invocado no resultado para determinar se `x` é definitivamente verdadeiro.</span><span class="sxs-lookup"><span data-stu-id="dd693-2318">In other words, `x` is first evaluated and `operator true` is invoked on the result to determine if `x` is definitely true.</span></span> <span data-ttu-id="dd693-2319">Em seguida, se `x` for definitivamente true, o resultado da operação será o valor calculado anteriormente para `x`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2319">Then, if `x` is definitely true, the result of the operation is the value previously computed for `x`.</span></span> <span data-ttu-id="dd693-2320">Caso contrário, `y` é avaliado e o `operator |` selecionado é invocado no valor calculado anteriormente para `x` e o valor calculado para `y` para produzir o resultado da operação.</span><span class="sxs-lookup"><span data-stu-id="dd693-2320">Otherwise, `y` is evaluated, and the selected `operator |` is invoked on the value previously computed for `x` and the value computed for `y` to produce the result of the operation.</span></span>

<span data-ttu-id="dd693-2321">Em qualquer uma dessas operações, a expressão fornecida pelo `x` é avaliada apenas uma vez e a expressão fornecida pelo `y` não é avaliada ou avaliada exatamente uma vez.</span><span class="sxs-lookup"><span data-stu-id="dd693-2321">In either of these operations, the expression given by `x` is only evaluated once, and the expression given by `y` is either not evaluated or evaluated exactly once.</span></span>

<span data-ttu-id="dd693-2322">Para obter um exemplo de um tipo que implementa `operator true` e `operator false`, consulte [tipo booliano do banco de dados](structs.md#database-boolean-type).</span><span class="sxs-lookup"><span data-stu-id="dd693-2322">For an example of a type that implements `operator true` and `operator false`, see [Database boolean type](structs.md#database-boolean-type).</span></span>

## <a name="the-null-coalescing-operator"></a><span data-ttu-id="dd693-2323">O operador de União nulo</span><span class="sxs-lookup"><span data-stu-id="dd693-2323">The null coalescing operator</span></span>

<span data-ttu-id="dd693-2324">O operador `??` é chamado de operador de União nulo.</span><span class="sxs-lookup"><span data-stu-id="dd693-2324">The `??` operator is called the null coalescing operator.</span></span>

```antlr
null_coalescing_expression
    : conditional_or_expression
    | conditional_or_expression '??' null_coalescing_expression
    ;
```

<span data-ttu-id="dd693-2325">Uma expressão de União nula do formulário `a ?? b` requer que `a` seja de um tipo anulável ou tipo de referência.</span><span class="sxs-lookup"><span data-stu-id="dd693-2325">A null coalescing expression of the form `a ?? b` requires `a` to be of a nullable type or reference type.</span></span> <span data-ttu-id="dd693-2326">Se `a` não for NULL, o resultado de `a ?? b` será `a`; caso contrário, o resultado será `b`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2326">If `a` is non-null, the result of `a ?? b` is `a`; otherwise, the result is `b`.</span></span> <span data-ttu-id="dd693-2327">A operação só avaliará `b` se `a` for nula.</span><span class="sxs-lookup"><span data-stu-id="dd693-2327">The operation evaluates `b` only if `a` is null.</span></span>

<span data-ttu-id="dd693-2328">O operador de União nulo é associativo à direita, o que significa que as operações são agrupadas da direita para a esquerda.</span><span class="sxs-lookup"><span data-stu-id="dd693-2328">The null coalescing operator is right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="dd693-2329">Por exemplo, uma expressão do formulário `a ?? b ?? c` é avaliada como `a ?? (b ?? c)`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2329">For example, an expression of the form `a ?? b ?? c` is evaluated as `a ?? (b ?? c)`.</span></span> <span data-ttu-id="dd693-2330">Em termos gerais, uma expressão do formulário `E1 ?? E2 ?? ... ?? En` retornará o primeiro dos operandos que não for nulo ou NULL se todos os operandos forem nulos.</span><span class="sxs-lookup"><span data-stu-id="dd693-2330">In general terms, an expression of the form `E1 ?? E2 ?? ... ?? En` returns the first of the operands that is non-null, or null if all operands are null.</span></span>

<span data-ttu-id="dd693-2331">O tipo da expressão `a ?? b` depende de quais conversões implícitas estão disponíveis nos operandos.</span><span class="sxs-lookup"><span data-stu-id="dd693-2331">The type of the expression `a ?? b` depends on which implicit conversions are available on the operands.</span></span> <span data-ttu-id="dd693-2332">Em ordem de preferência, o tipo de `a ?? b` é `A0`, `A`ou `B`, em que `A` é o tipo de `a` (desde que `a` tenha um tipo), `B` é o tipo de `b` (desde que `b` tenha um tipo) e `A0` seja o tipo subjacente de `A` se `A` for um tipo anulável ou `A` contrário.</span><span class="sxs-lookup"><span data-stu-id="dd693-2332">In order of preference, the type of `a ?? b` is `A0`, `A`, or `B`, where `A` is the type of `a` (provided that `a` has a type), `B` is the type of `b` (provided that `b` has a type), and `A0` is the underlying type of `A` if `A` is a nullable type, or `A` otherwise.</span></span> <span data-ttu-id="dd693-2333">Especificamente, `a ?? b` é processada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="dd693-2333">Specifically, `a ?? b` is processed as follows:</span></span>

*  <span data-ttu-id="dd693-2334">Se `A` existir e não for um tipo anulável ou um tipo de referência, ocorrerá um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-2334">If `A` exists and is not a nullable type or a reference type, a compile-time error occurs.</span></span>
*  <span data-ttu-id="dd693-2335">Se `b` for uma expressão dinâmica, o tipo de resultado será `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2335">If `b` is a dynamic expression, the result type is `dynamic`.</span></span> <span data-ttu-id="dd693-2336">Em tempo de execução, `a` é avaliada pela primeira vez.</span><span class="sxs-lookup"><span data-stu-id="dd693-2336">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="dd693-2337">Se `a` não for NULL, `a` será convertido em dinâmico, e isso se tornará o resultado.</span><span class="sxs-lookup"><span data-stu-id="dd693-2337">If `a` is not null, `a` is converted to dynamic, and this becomes the result.</span></span> <span data-ttu-id="dd693-2338">Caso contrário, `b` será avaliado e isso se tornará o resultado.</span><span class="sxs-lookup"><span data-stu-id="dd693-2338">Otherwise, `b` is evaluated, and this becomes the result.</span></span>
*  <span data-ttu-id="dd693-2339">Caso contrário, se `A` existir e for um tipo anulável e existir uma conversão implícita de `b` para `A0`, o tipo de resultado será `A0`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2339">Otherwise, if `A` exists and is a nullable type and an implicit conversion exists from `b` to `A0`, the result type is `A0`.</span></span> <span data-ttu-id="dd693-2340">Em tempo de execução, `a` é avaliada pela primeira vez.</span><span class="sxs-lookup"><span data-stu-id="dd693-2340">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="dd693-2341">Se `a` não for NULL, `a` será desencapsulado para o tipo `A0`, e isso se tornará o resultado.</span><span class="sxs-lookup"><span data-stu-id="dd693-2341">If `a` is not null, `a` is unwrapped to type `A0`, and this becomes the result.</span></span> <span data-ttu-id="dd693-2342">Caso contrário, `b` será avaliada e convertida no tipo `A0`, e isso se tornará o resultado.</span><span class="sxs-lookup"><span data-stu-id="dd693-2342">Otherwise, `b` is evaluated and converted to type `A0`, and this becomes the result.</span></span>
*  <span data-ttu-id="dd693-2343">Caso contrário, se `A` existir e uma conversão implícita existir de `b` para `A`, o tipo de resultado será `A`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2343">Otherwise, if `A` exists and an implicit conversion exists from `b` to `A`, the result type is `A`.</span></span> <span data-ttu-id="dd693-2344">Em tempo de execução, `a` é avaliada pela primeira vez.</span><span class="sxs-lookup"><span data-stu-id="dd693-2344">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="dd693-2345">Se `a` não for NULL, `a` se tornará o resultado.</span><span class="sxs-lookup"><span data-stu-id="dd693-2345">If `a` is not null, `a` becomes the result.</span></span> <span data-ttu-id="dd693-2346">Caso contrário, `b` será avaliada e convertida no tipo `A`, e isso se tornará o resultado.</span><span class="sxs-lookup"><span data-stu-id="dd693-2346">Otherwise, `b` is evaluated and converted to type `A`, and this becomes the result.</span></span>
*  <span data-ttu-id="dd693-2347">Caso contrário, se `b` tiver um tipo `B` e uma conversão implícita existir de `a` para `B`, o tipo de resultado será `B`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2347">Otherwise, if `b` has a type `B` and an implicit conversion exists from `a` to `B`, the result type is `B`.</span></span> <span data-ttu-id="dd693-2348">Em tempo de execução, `a` é avaliada pela primeira vez.</span><span class="sxs-lookup"><span data-stu-id="dd693-2348">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="dd693-2349">Se `a` não for NULL, `a` será desencapsulado para o tipo `A0` (se `A` existir e for anulável) e convertido para o tipo `B`, e isso se tornará o resultado.</span><span class="sxs-lookup"><span data-stu-id="dd693-2349">If `a` is not null, `a` is unwrapped to type `A0` (if `A` exists and is nullable) and converted to type `B`, and this becomes the result.</span></span> <span data-ttu-id="dd693-2350">Caso contrário, `b` será avaliado e se tornará o resultado.</span><span class="sxs-lookup"><span data-stu-id="dd693-2350">Otherwise, `b` is evaluated and becomes the result.</span></span>
*  <span data-ttu-id="dd693-2351">Caso contrário, `a` e `b` são incompatíveis e ocorre um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-2351">Otherwise, `a` and `b` are incompatible, and a compile-time error occurs.</span></span>

## <a name="conditional-operator"></a><span data-ttu-id="dd693-2352">Operador condicional</span><span class="sxs-lookup"><span data-stu-id="dd693-2352">Conditional operator</span></span>

<span data-ttu-id="dd693-2353">O operador `?:` é chamado de operador condicional.</span><span class="sxs-lookup"><span data-stu-id="dd693-2353">The `?:` operator is called the conditional operator.</span></span> <span data-ttu-id="dd693-2354">Ele ocorre às vezes também chamado de operador ternário.</span><span class="sxs-lookup"><span data-stu-id="dd693-2354">It is at times also called the ternary operator.</span></span>

```antlr
conditional_expression
    : null_coalescing_expression
    | null_coalescing_expression '?' expression ':' expression
    ;
```

<span data-ttu-id="dd693-2355">Uma expressão condicional do formulário `b ? x : y` primeiro avalia a condição `b`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2355">A conditional expression of the form `b ? x : y` first evaluates the condition `b`.</span></span> <span data-ttu-id="dd693-2356">Em seguida, se `b` for `true`, `x` será avaliado e se tornará o resultado da operação.</span><span class="sxs-lookup"><span data-stu-id="dd693-2356">Then, if `b` is `true`, `x` is evaluated and becomes the result of the operation.</span></span> <span data-ttu-id="dd693-2357">Caso contrário, `y` será avaliado e se tornará o resultado da operação.</span><span class="sxs-lookup"><span data-stu-id="dd693-2357">Otherwise, `y` is evaluated and becomes the result of the operation.</span></span> <span data-ttu-id="dd693-2358">Uma expressão condicional nunca avalia `x` e `y`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2358">A conditional expression never evaluates both `x` and `y`.</span></span>

<span data-ttu-id="dd693-2359">O operador condicional é associativo à direita, o que significa que as operações são agrupadas da direita para a esquerda.</span><span class="sxs-lookup"><span data-stu-id="dd693-2359">The conditional operator is right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="dd693-2360">Por exemplo, uma expressão do formulário `a ? b : c ? d : e` é avaliada como `a ? b : (c ? d : e)`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2360">For example, an expression of the form `a ? b : c ? d : e` is evaluated as `a ? b : (c ? d : e)`.</span></span>

<span data-ttu-id="dd693-2361">O primeiro operando do operador de `?:` deve ser uma expressão que possa ser convertida implicitamente em `bool`ou uma expressão de um tipo que implemente `operator true`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2361">The first operand of the `?:` operator must be an expression that can be implicitly converted to `bool`, or an expression of a type that implements `operator true`.</span></span> <span data-ttu-id="dd693-2362">Se nenhum desses requisitos for atendido, ocorrerá um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-2362">If neither of these requirements is satisfied, a compile-time error occurs.</span></span>

<span data-ttu-id="dd693-2363">O segundo e o terceiro operandos, `x` e `y`, do operador de `?:` controlam o tipo da expressão condicional.</span><span class="sxs-lookup"><span data-stu-id="dd693-2363">The second and third operands, `x` and `y`, of the `?:` operator control the type of the conditional expression.</span></span>

*  <span data-ttu-id="dd693-2364">Se `x` tiver tipo `X` e `y` tiver tipo `Y`</span><span class="sxs-lookup"><span data-stu-id="dd693-2364">If `x` has type `X` and `y` has type `Y` then</span></span>
   * <span data-ttu-id="dd693-2365">Se houver uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)) de `X` para `Y`, mas não de `Y` para `X`, `Y` será o tipo da expressão condicional.</span><span class="sxs-lookup"><span data-stu-id="dd693-2365">If an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from `X` to `Y`, but not from `Y` to `X`, then `Y` is the type of the conditional expression.</span></span>
   * <span data-ttu-id="dd693-2366">Se houver uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)) de `Y` para `X`, mas não de `X` para `Y`, `X` será o tipo da expressão condicional.</span><span class="sxs-lookup"><span data-stu-id="dd693-2366">If an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from `Y` to `X`, but not from `X` to `Y`, then `X` is the type of the conditional expression.</span></span>
   * <span data-ttu-id="dd693-2367">Caso contrário, nenhum tipo de expressão pode ser determinado e ocorre um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-2367">Otherwise, no expression type can be determined, and a compile-time error occurs.</span></span>
*  <span data-ttu-id="dd693-2368">Se apenas um dos `x` e `y` tiver um tipo, e `x` e `y`forem conversíveis implicitamente para esse tipo, esse será o tipo da expressão condicional.</span><span class="sxs-lookup"><span data-stu-id="dd693-2368">If only one of `x` and `y` has a type, and both `x` and `y`, of are implicitly convertible to that type, then that is the type of the conditional expression.</span></span>
*  <span data-ttu-id="dd693-2369">Caso contrário, nenhum tipo de expressão pode ser determinado e ocorre um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-2369">Otherwise, no expression type can be determined, and a compile-time error occurs.</span></span>

<span data-ttu-id="dd693-2370">O processamento em tempo de execução de uma expressão condicional do formulário `b ? x : y` consiste nas seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="dd693-2370">The run-time processing of a conditional expression of the form `b ? x : y` consists of the following steps:</span></span>

*  <span data-ttu-id="dd693-2371">Primeiro, `b` é avaliado e o valor de `bool` de `b` é determinado:</span><span class="sxs-lookup"><span data-stu-id="dd693-2371">First, `b` is evaluated, and the `bool` value of `b` is determined:</span></span>
   * <span data-ttu-id="dd693-2372">Se uma conversão implícita do tipo de `b` para `bool` existir, essa conversão implícita será executada para produzir um valor de `bool`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2372">If an implicit conversion from the type of `b` to `bool` exists, then this implicit conversion is performed to produce a `bool` value.</span></span>
   * <span data-ttu-id="dd693-2373">Caso contrário, o `operator true` definido pelo tipo de `b` será invocado para produzir um valor de `bool`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2373">Otherwise, the `operator true` defined by the type of `b` is invoked to produce a `bool` value.</span></span>
*  <span data-ttu-id="dd693-2374">Se o valor de `bool` produzido pela etapa acima for `true`, `x` será avaliado e convertido para o tipo da expressão condicional, e isso se tornará o resultado da expressão condicional.</span><span class="sxs-lookup"><span data-stu-id="dd693-2374">If the `bool` value produced by the step above is `true`, then `x` is evaluated and converted to the type of the conditional expression, and this becomes the result of the conditional expression.</span></span>
*  <span data-ttu-id="dd693-2375">Caso contrário, `y` é avaliado e convertido para o tipo da expressão condicional, e isso se torna o resultado da expressão condicional.</span><span class="sxs-lookup"><span data-stu-id="dd693-2375">Otherwise, `y` is evaluated and converted to the type of the conditional expression, and this becomes the result of the conditional expression.</span></span>

## <a name="anonymous-function-expressions"></a><span data-ttu-id="dd693-2376">Expressões de função anônimas</span><span class="sxs-lookup"><span data-stu-id="dd693-2376">Anonymous function expressions</span></span>

<span data-ttu-id="dd693-2377">Uma ***função anônima*** é uma expressão que representa uma definição de método "embutida".</span><span class="sxs-lookup"><span data-stu-id="dd693-2377">An ***anonymous function*** is an expression that represents an "in-line" method definition.</span></span> <span data-ttu-id="dd693-2378">Uma função anônima não tem um valor ou tipo próprio, mas é conversível em um delegate compatível ou tipo de árvore de expressão.</span><span class="sxs-lookup"><span data-stu-id="dd693-2378">An anonymous function does not have a value or type in and of itself, but is convertible to a compatible delegate or expression tree type.</span></span> <span data-ttu-id="dd693-2379">A avaliação de uma conversão de função anônima depende do tipo de destino da conversão: se for um tipo delegado, a conversão será avaliada como um valor delegado que referencia o método que a função anônima define.</span><span class="sxs-lookup"><span data-stu-id="dd693-2379">The evaluation of an anonymous function conversion depends on the target type of the conversion: If it is a delegate type, the conversion evaluates to a delegate value referencing the method which the anonymous function defines.</span></span> <span data-ttu-id="dd693-2380">Se for um tipo de árvore de expressão, a conversão será avaliada como uma árvore de expressão que representa a estrutura do método como uma estrutura de objeto.</span><span class="sxs-lookup"><span data-stu-id="dd693-2380">If it is an expression tree type, the conversion evaluates to an expression tree which represents the structure of the method as an object structure.</span></span>

<span data-ttu-id="dd693-2381">Por motivos históricos, há dois tipos sintáticos de funções anônimas, ou seja, *lambda_expression*s e *anonymous_method_expression*s.</span><span class="sxs-lookup"><span data-stu-id="dd693-2381">For historical reasons there are two syntactic flavors of anonymous functions, namely *lambda_expression*s and *anonymous_method_expression*s.</span></span> <span data-ttu-id="dd693-2382">Para quase todas as finalidades, *lambda_expression*s são mais concisas e expressivas do que *anonymous_method_expression*s, que permanecem na linguagem para compatibilidade com versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="dd693-2382">For almost all purposes, *lambda_expression*s are more concise and expressive than *anonymous_method_expression*s, which remain in the language for backwards compatibility.</span></span>

```antlr
lambda_expression
    : anonymous_function_signature '=>' anonymous_function_body
    ;

anonymous_method_expression
    : 'delegate' explicit_anonymous_function_signature? block
    ;

anonymous_function_signature
    : explicit_anonymous_function_signature
    | implicit_anonymous_function_signature
    ;

explicit_anonymous_function_signature
    : '(' explicit_anonymous_function_parameter_list? ')'
    ;

explicit_anonymous_function_parameter_list
    : explicit_anonymous_function_parameter (',' explicit_anonymous_function_parameter)*
    ;

explicit_anonymous_function_parameter
    : anonymous_function_parameter_modifier? type identifier
    ;

anonymous_function_parameter_modifier
    : 'ref'
    | 'out'
    ;

implicit_anonymous_function_signature
    : '(' implicit_anonymous_function_parameter_list? ')'
    | implicit_anonymous_function_parameter
    ;

implicit_anonymous_function_parameter_list
    : implicit_anonymous_function_parameter (',' implicit_anonymous_function_parameter)*
    ;

implicit_anonymous_function_parameter
    : identifier
    ;

anonymous_function_body
    : expression
    | block
    ;
```

<span data-ttu-id="dd693-2383">O operador de `=>` tem a mesma precedência de atribuição (`=`) e é associativo à direita.</span><span class="sxs-lookup"><span data-stu-id="dd693-2383">The `=>` operator has the same precedence as assignment (`=`) and is right-associative.</span></span>

<span data-ttu-id="dd693-2384">Uma função anônima com o modificador de `async` é uma função Async e segue as regras descritas em [iteradores](classes.md#iterators).</span><span class="sxs-lookup"><span data-stu-id="dd693-2384">An anonymous function with the `async` modifier is an async function and follows the rules described in [Iterators](classes.md#iterators).</span></span>

<span data-ttu-id="dd693-2385">Os parâmetros de uma função anônima na forma de um *lambda_expression* podem ser digitados explícita ou implicitamente.</span><span class="sxs-lookup"><span data-stu-id="dd693-2385">The parameters of an anonymous function in the form of a *lambda_expression* can be explicitly or implicitly typed.</span></span> <span data-ttu-id="dd693-2386">Em uma lista de parâmetros explicitamente tipados, o tipo de cada parâmetro é explicitamente declarado.</span><span class="sxs-lookup"><span data-stu-id="dd693-2386">In an explicitly typed parameter list, the type of each parameter is explicitly stated.</span></span> <span data-ttu-id="dd693-2387">Em uma lista de parâmetros Tipados implicitamente, os tipos dos parâmetros são inferidos do contexto no qual a função anônima ocorre — especificamente, quando a função anônima é convertida em um tipo de delegado compatível ou tipo de árvore de expressão, esse tipo fornece os tipos de parâmetro ([conversões de função anônima](conversions.md#anonymous-function-conversions)).</span><span class="sxs-lookup"><span data-stu-id="dd693-2387">In an implicitly typed parameter list, the types of the parameters are inferred from the context in which the anonymous function occurs—specifically, when the anonymous function is converted to a compatible delegate type or expression tree type, that type provides the parameter types ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>

<span data-ttu-id="dd693-2388">Em uma função anônima com um único parâmetro tipado implicitamente, os parênteses podem ser omitidos da lista de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="dd693-2388">In an anonymous function with a single, implicitly typed parameter, the parentheses may be omitted from the parameter list.</span></span> <span data-ttu-id="dd693-2389">Em outras palavras, uma função anônima do formulário</span><span class="sxs-lookup"><span data-stu-id="dd693-2389">In other words, an anonymous function of the form</span></span>
```csharp
( param ) => expr
```
<span data-ttu-id="dd693-2390">pode ser abreviado como</span><span class="sxs-lookup"><span data-stu-id="dd693-2390">can be abbreviated to</span></span>
```csharp
param => expr
```

<span data-ttu-id="dd693-2391">A lista de parâmetros de uma função anônima na forma de uma *anonymous_method_expression* é opcional.</span><span class="sxs-lookup"><span data-stu-id="dd693-2391">The parameter list of an anonymous function in the form of an *anonymous_method_expression* is optional.</span></span> <span data-ttu-id="dd693-2392">Se for fornecido, os parâmetros deverão ser tipados explicitamente.</span><span class="sxs-lookup"><span data-stu-id="dd693-2392">If given, the parameters must be explicitly typed.</span></span> <span data-ttu-id="dd693-2393">Caso contrário, a função anônima é conversível em um delegado com qualquer lista de parâmetros que não contém `out` parâmetros.</span><span class="sxs-lookup"><span data-stu-id="dd693-2393">If not, the anonymous function is convertible to a delegate with any parameter list not containing `out` parameters.</span></span>

<span data-ttu-id="dd693-2394">Um corpo de *bloco* de uma função anônima pode ser acessado ([pontos de extremidade e acessibilidade](statements.md#end-points-and-reachability)), a menos que a função anônima ocorra dentro de uma instrução inacessível.</span><span class="sxs-lookup"><span data-stu-id="dd693-2394">A *block* body of an anonymous function is reachable ([End points and reachability](statements.md#end-points-and-reachability)) unless the anonymous function occurs inside an unreachable statement.</span></span>

<span data-ttu-id="dd693-2395">Veja a seguir alguns exemplos de funções anônimas:</span><span class="sxs-lookup"><span data-stu-id="dd693-2395">Some examples of anonymous functions follow below:</span></span>

```csharp
x => x + 1                              // Implicitly typed, expression body
x => { return x + 1; }                  // Implicitly typed, statement body
(int x) => x + 1                        // Explicitly typed, expression body
(int x) => { return x + 1; }            // Explicitly typed, statement body
(x, y) => x * y                         // Multiple parameters
() => Console.WriteLine()               // No parameters
async (t1,t2) => await t1 + await t2    // Async
delegate (int x) { return x + 1; }      // Anonymous method expression
delegate { return 1 + 1; }              // Parameter list omitted
```

<span data-ttu-id="dd693-2396">O comportamento de *lambda_expression*s e *anonymous_method_expression*s é o mesmo, exceto os seguintes pontos:</span><span class="sxs-lookup"><span data-stu-id="dd693-2396">The behavior of *lambda_expression*s and *anonymous_method_expression*s is the same except for the following points:</span></span>

*  <span data-ttu-id="dd693-2397">*anonymous_method_expression*s permitem que a lista de parâmetros seja omitida inteiramente, produzindo convertibilidade para delegar tipos de qualquer lista de parâmetros de valor.</span><span class="sxs-lookup"><span data-stu-id="dd693-2397">*anonymous_method_expression*s permit the parameter list to be omitted entirely, yielding convertibility to delegate types of any list of value parameters.</span></span>
*  <span data-ttu-id="dd693-2398">*lambda_expression*s permitem que os tipos de parâmetro sejam omitidos e inferidos, enquanto *anonymous_method_expression*s exigem que os tipos de parâmetros sejam declarados explicitamente.</span><span class="sxs-lookup"><span data-stu-id="dd693-2398">*lambda_expression*s permit parameter types to be omitted and inferred whereas *anonymous_method_expression*s require parameter types to be explicitly stated.</span></span>
*  <span data-ttu-id="dd693-2399">O corpo de uma *lambda_expression* pode ser uma expressão ou um bloco de instrução, enquanto o corpo de uma *anonymous_method_expression* deve ser um bloco de instruções.</span><span class="sxs-lookup"><span data-stu-id="dd693-2399">The body of a *lambda_expression* can be an expression or a statement block whereas the body of an *anonymous_method_expression* must be a statement block.</span></span>
*  <span data-ttu-id="dd693-2400">Somente *lambda_expression*s têm conversões para tipos de árvore de expressão compatíveis ([tipos de árvore de expressão](types.md#expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="dd693-2400">Only *lambda_expression*s have conversions to compatible expression tree types ([Expression tree types](types.md#expression-tree-types)).</span></span>

### <a name="anonymous-function-signatures"></a><span data-ttu-id="dd693-2401">Assinaturas de funções anônimas</span><span class="sxs-lookup"><span data-stu-id="dd693-2401">Anonymous function signatures</span></span>

<span data-ttu-id="dd693-2402">O *anonymous_function_signature* opcional de uma função anônima define os nomes e, opcionalmente, os tipos dos parâmetros formais para a função anônima.</span><span class="sxs-lookup"><span data-stu-id="dd693-2402">The optional *anonymous_function_signature* of an anonymous function defines the names and optionally the types of the formal parameters for the anonymous function.</span></span> <span data-ttu-id="dd693-2403">O escopo dos parâmetros da função anônima é o *anonymous_function_body*.</span><span class="sxs-lookup"><span data-stu-id="dd693-2403">The scope of the parameters of the anonymous function is the *anonymous_function_body*.</span></span> <span data-ttu-id="dd693-2404">([Escopos](basic-concepts.md#scopes)) Junto com a lista de parâmetros (se fornecido), o corpo do método anônimo constitui um espaço de declaração ([declarações](basic-concepts.md#declarations)).</span><span class="sxs-lookup"><span data-stu-id="dd693-2404">([Scopes](basic-concepts.md#scopes)) Together with the parameter list (if given) the anonymous-method-body constitutes a declaration space ([Declarations](basic-concepts.md#declarations)).</span></span> <span data-ttu-id="dd693-2405">Portanto, é um erro de tempo de compilação para o nome de um parâmetro da função anônima corresponder ao nome de uma variável local, constante local ou parâmetro cujo escopo inclui a *anonymous_method_expression* ou *lambda_expression*.</span><span class="sxs-lookup"><span data-stu-id="dd693-2405">It is thus a compile-time error for the name of a parameter of the anonymous function to match the name of a local variable, local constant or parameter whose scope includes the *anonymous_method_expression* or *lambda_expression*.</span></span>

<span data-ttu-id="dd693-2406">Se uma função anônima tiver um *explicit_anonymous_function_signature*, o conjunto de tipos de delegados compatíveis e tipos de árvore de expressão será restrito àqueles que têm os mesmos tipos de parâmetro e modificadores na mesma ordem.</span><span class="sxs-lookup"><span data-stu-id="dd693-2406">If an anonymous function has an *explicit_anonymous_function_signature*, then the set of compatible delegate types and expression tree types is restricted to those that have the same parameter types and modifiers in the same order.</span></span> <span data-ttu-id="dd693-2407">Em contraste com conversões de grupos de métodos ([conversões de grupos de métodos](conversions.md#method-group-conversions)), não há suporte para a variância de contrato de tipos de parâmetro de função anônima.</span><span class="sxs-lookup"><span data-stu-id="dd693-2407">In contrast to method group conversions ([Method group conversions](conversions.md#method-group-conversions)), contra-variance of anonymous function parameter types is not supported.</span></span> <span data-ttu-id="dd693-2408">Se uma função anônima não tiver um *anonymous_function_signature*, o conjunto de tipos de delegados compatíveis e tipos de árvore de expressão será restrito àqueles que não têm parâmetros de `out`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2408">If an anonymous function does not have an *anonymous_function_signature*, then the set of compatible delegate types and expression tree types is restricted to those that have no `out` parameters.</span></span>

<span data-ttu-id="dd693-2409">Observe que um *anonymous_function_signature* não pode incluir atributos ou uma matriz de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="dd693-2409">Note that an *anonymous_function_signature* cannot include attributes or a parameter array.</span></span> <span data-ttu-id="dd693-2410">No entanto, um *anonymous_function_signature* pode ser compatível com um tipo delegado cuja lista de parâmetros contenha uma matriz de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="dd693-2410">Nevertheless, an *anonymous_function_signature* may be compatible with a delegate type whose parameter list contains a parameter array.</span></span>

<span data-ttu-id="dd693-2411">Observe também que a conversão para um tipo de árvore de expressão, mesmo se compatível, ainda pode falhar em tempo de compilação ([tipos de árvore de expressão](types.md#expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="dd693-2411">Note also that conversion to an expression tree type, even if compatible, may still fail at compile-time ([Expression tree types](types.md#expression-tree-types)).</span></span>

### <a name="anonymous-function-bodies"></a><span data-ttu-id="dd693-2412">Corpos de funções anônimas</span><span class="sxs-lookup"><span data-stu-id="dd693-2412">Anonymous function bodies</span></span>

<span data-ttu-id="dd693-2413">O corpo (*expressão* ou *bloco*) de uma função anônima está sujeito às seguintes regras:</span><span class="sxs-lookup"><span data-stu-id="dd693-2413">The body (*expression* or *block*) of an anonymous function is subject to the following rules:</span></span>

*  <span data-ttu-id="dd693-2414">Se a função anônima incluir uma assinatura, os parâmetros especificados na assinatura estarão disponíveis no corpo.</span><span class="sxs-lookup"><span data-stu-id="dd693-2414">If the anonymous function includes a signature, the parameters specified in the signature are available in the body.</span></span> <span data-ttu-id="dd693-2415">Se a função anônima não tiver nenhuma assinatura, ela poderá ser convertida em um tipo de representante ou tipo de expressão com parâmetros ([conversões de função anônimas](conversions.md#anonymous-function-conversions)), mas os parâmetros não poderão ser acessados no corpo.</span><span class="sxs-lookup"><span data-stu-id="dd693-2415">If the anonymous function has no signature it can be converted to a delegate type or expression type having parameters ([Anonymous function conversions](conversions.md#anonymous-function-conversions)), but the parameters cannot be accessed in the body.</span></span>
*  <span data-ttu-id="dd693-2416">Exceto para `ref` ou `out` parâmetros especificados na assinatura (se houver) da função anônima delimitadora mais próxima, trata-se de um erro de tempo de compilação para o corpo acessar um parâmetro `ref` ou `out`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2416">Except for `ref` or `out` parameters specified in the signature (if any) of the nearest enclosing anonymous function, it is a compile-time error for the body to access a `ref` or `out` parameter.</span></span>
*  <span data-ttu-id="dd693-2417">Quando o tipo de `this` é um tipo struct, é um erro de tempo de compilação para o corpo acessar `this`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2417">When the type of `this` is a struct type, it is a compile-time error for the body to access `this`.</span></span> <span data-ttu-id="dd693-2418">Isso é verdadeiro se o acesso for explícito (como no `this.x`) ou implícito (como em `x` em que `x` é um membro de instância da struct).</span><span class="sxs-lookup"><span data-stu-id="dd693-2418">This is true whether the access is explicit (as in `this.x`) or implicit (as in `x` where `x` is an instance member of the struct).</span></span> <span data-ttu-id="dd693-2419">Essa regra simplesmente proíbe esse acesso e não afeta se a pesquisa de membros resulta em um membro da estrutura.</span><span class="sxs-lookup"><span data-stu-id="dd693-2419">This rule simply prohibits such access and does not affect whether member lookup results in a member of the struct.</span></span>
*  <span data-ttu-id="dd693-2420">O corpo tem acesso às variáveis externas ([variáveis externas](expressions.md#outer-variables)) da função anônima.</span><span class="sxs-lookup"><span data-stu-id="dd693-2420">The body has access to the outer variables ([Outer variables](expressions.md#outer-variables)) of the anonymous function.</span></span> <span data-ttu-id="dd693-2421">O acesso de uma variável externa fará referência à instância da variável que está ativa no momento em que o *lambda_expression* ou *anonymous_method_expression* é avaliado ([avaliação de expressões de função anônimas](expressions.md#evaluation-of-anonymous-function-expressions)).</span><span class="sxs-lookup"><span data-stu-id="dd693-2421">Access of an outer variable will reference the instance of the variable that is active at the time the *lambda_expression* or *anonymous_method_expression* is evaluated ([Evaluation of anonymous function expressions](expressions.md#evaluation-of-anonymous-function-expressions)).</span></span>
*  <span data-ttu-id="dd693-2422">É um erro de tempo de compilação para o corpo conter uma instrução `goto`, `break` instrução ou `continue` instrução cujo destino está fora do corpo ou dentro do corpo de uma função anônima contida.</span><span class="sxs-lookup"><span data-stu-id="dd693-2422">It is a compile-time error for the body to contain a `goto` statement, `break` statement, or `continue` statement whose target is outside the body or within the body of a contained anonymous function.</span></span>
*  <span data-ttu-id="dd693-2423">Uma instrução `return` no corpo retorna o controle de uma invocação da função anônima delimitadora mais próxima, não do membro da função delimitadora.</span><span class="sxs-lookup"><span data-stu-id="dd693-2423">A `return` statement in the body returns control from an invocation of the nearest enclosing anonymous function, not from the enclosing function member.</span></span> <span data-ttu-id="dd693-2424">Uma expressão especificada em uma instrução `return` deve ser conversível implicitamente para o tipo de retorno do tipo delegado ou tipo de árvore de expressão para o qual o *lambda_expression* ou *anonymous_method_expression* de fechamento mais próximo é convertido ([conversões de função anônimas](conversions.md#anonymous-function-conversions)).</span><span class="sxs-lookup"><span data-stu-id="dd693-2424">An expression specified in a `return` statement must be implicitly convertible to the return type of the delegate type or expression tree type to which the nearest enclosing *lambda_expression* or *anonymous_method_expression* is converted ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>

<span data-ttu-id="dd693-2425">Ele é explicitamente não especificado se há qualquer maneira de executar o bloco de uma função anônima além da avaliação e invocação do *lambda_expression* ou *anonymous_method_expression*.</span><span class="sxs-lookup"><span data-stu-id="dd693-2425">It is explicitly unspecified whether there is any way to execute the block of an anonymous function other than through evaluation and invocation of the *lambda_expression* or *anonymous_method_expression*.</span></span> <span data-ttu-id="dd693-2426">Em particular, o compilador pode optar por implementar uma função anônima sintetizando um ou mais métodos ou tipos nomeados.</span><span class="sxs-lookup"><span data-stu-id="dd693-2426">In particular, the compiler may choose to implement an anonymous function by synthesizing one or more named methods or types.</span></span> <span data-ttu-id="dd693-2427">Os nomes de quaisquer elementos sintetizados devem ser de um formulário reservado para uso do compilador.</span><span class="sxs-lookup"><span data-stu-id="dd693-2427">The names of any such synthesized elements must be of a form reserved for compiler use.</span></span>

### <a name="overload-resolution-and-anonymous-functions"></a><span data-ttu-id="dd693-2428">Resolução de sobrecarga e funções anônimas</span><span class="sxs-lookup"><span data-stu-id="dd693-2428">Overload resolution and anonymous functions</span></span>

<span data-ttu-id="dd693-2429">As funções anônimas em uma lista de argumentos participam da inferência de tipos e da resolução de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="dd693-2429">Anonymous functions in an argument list participate in type inference and overload resolution.</span></span> <span data-ttu-id="dd693-2430">Veja a [inferência de tipos](expressions.md#type-inference) e a [resolução de sobrecarga](expressions.md#overload-resolution) para as regras exatas.</span><span class="sxs-lookup"><span data-stu-id="dd693-2430">Please refer to [Type inference](expressions.md#type-inference) and [Overload resolution](expressions.md#overload-resolution) for the exact rules.</span></span>

<span data-ttu-id="dd693-2431">O exemplo a seguir ilustra o efeito de funções anônimas na resolução de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="dd693-2431">The following example illustrates the effect of anonymous functions on overload resolution.</span></span>

```csharp
class ItemList<T>: List<T>
{
    public int Sum(Func<T,int> selector) {
        int sum = 0;
        foreach (T item in this) sum += selector(item);
        return sum;
    }

    public double Sum(Func<T,double> selector) {
        double sum = 0;
        foreach (T item in this) sum += selector(item);
        return sum;
    }
}
```

<span data-ttu-id="dd693-2432">A classe `ItemList<T>` tem dois métodos `Sum`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2432">The `ItemList<T>` class has two `Sum` methods.</span></span> <span data-ttu-id="dd693-2433">Cada um recebe um argumento `selector`, que extrai o valor para somar de um item de lista.</span><span class="sxs-lookup"><span data-stu-id="dd693-2433">Each takes a `selector` argument, which extracts the value to sum over from a list item.</span></span> <span data-ttu-id="dd693-2434">O valor extraído pode ser um `int` ou um `double` e a soma resultante é, da mesma forma, uma `int` ou uma `double`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2434">The extracted value can be either an `int` or a `double` and the resulting sum is likewise either an `int` or a `double`.</span></span>

<span data-ttu-id="dd693-2435">Os métodos de `Sum` poderiam, por exemplo, ser usados para computar somas de uma lista de linhas de detalhes em uma ordem.</span><span class="sxs-lookup"><span data-stu-id="dd693-2435">The `Sum` methods could for example be used to compute sums from a list of detail lines in an order.</span></span>

```csharp
class Detail
{
    public int UnitCount;
    public double UnitPrice;
    ...
}

void ComputeSums() {
    ItemList<Detail> orderDetails = GetOrderDetails(...);
    int totalUnits = orderDetails.Sum(d => d.UnitCount);
    double orderTotal = orderDetails.Sum(d => d.UnitPrice * d.UnitCount);
    ...
}
```

<span data-ttu-id="dd693-2436">Na primeira invocação de `orderDetails.Sum`, ambos os métodos de `Sum` são aplicáveis porque a função anônima `d => d. UnitCount` é compatível com `Func<Detail,int>` e `Func<Detail,double>`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2436">In the first invocation of `orderDetails.Sum`, both `Sum` methods are applicable because the anonymous function `d => d. UnitCount` is compatible with both `Func<Detail,int>` and `Func<Detail,double>`.</span></span> <span data-ttu-id="dd693-2437">No entanto, a resolução de sobrecarga escolhe o primeiro método `Sum` porque a conversão em `Func<Detail,int>` é melhor do que a conversão para `Func<Detail,double>`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2437">However, overload resolution picks the first `Sum` method because the conversion to `Func<Detail,int>` is better than the conversion to `Func<Detail,double>`.</span></span>

<span data-ttu-id="dd693-2438">Na segunda invocação de `orderDetails.Sum`, somente o segundo método de `Sum` é aplicável porque a função anônima `d => d.UnitPrice * d.UnitCount` produz um valor do tipo `double`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2438">In the second invocation of `orderDetails.Sum`, only the second `Sum` method is applicable because the anonymous function `d => d.UnitPrice * d.UnitCount` produces a value of type `double`.</span></span> <span data-ttu-id="dd693-2439">Portanto, a resolução de sobrecarga escolhe o segundo método de `Sum` para essa invocação.</span><span class="sxs-lookup"><span data-stu-id="dd693-2439">Thus, overload resolution picks the second `Sum` method for that invocation.</span></span>

### <a name="anonymous-functions-and-dynamic-binding"></a><span data-ttu-id="dd693-2440">Funções anônimas e associação dinâmica</span><span class="sxs-lookup"><span data-stu-id="dd693-2440">Anonymous functions and dynamic binding</span></span>

<span data-ttu-id="dd693-2441">Uma função anônima não pode ser um receptor, argumento ou operando de uma operação vinculada dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="dd693-2441">An anonymous function cannot be a receiver, argument or operand of a dynamically bound operation.</span></span>

### <a name="outer-variables"></a><span data-ttu-id="dd693-2442">Variáveis externas</span><span class="sxs-lookup"><span data-stu-id="dd693-2442">Outer variables</span></span>

<span data-ttu-id="dd693-2443">Qualquer variável local, parâmetro de valor ou matriz de parâmetros cujo escopo inclui o *lambda_expression* ou *anonymous_method_expression* é chamado de uma ***variável externa*** da função anônima.</span><span class="sxs-lookup"><span data-stu-id="dd693-2443">Any local variable, value parameter, or parameter array whose scope includes the *lambda_expression* or *anonymous_method_expression* is called an ***outer variable*** of the anonymous function.</span></span> <span data-ttu-id="dd693-2444">Em um membro de função de instância de uma classe, o valor de `this` é considerado um parâmetro de valor e é uma variável externa de qualquer função anônima contida no membro da função.</span><span class="sxs-lookup"><span data-stu-id="dd693-2444">In an instance function member of a class, the `this` value is considered a value parameter and is an outer variable of any anonymous function contained within the function member.</span></span>

#### <a name="captured-outer-variables"></a><span data-ttu-id="dd693-2445">Variáveis externas capturadas</span><span class="sxs-lookup"><span data-stu-id="dd693-2445">Captured outer variables</span></span>

<span data-ttu-id="dd693-2446">Quando uma variável externa é referenciada por uma função anônima, diz-se que a variável externa foi ***capturada*** pela função anônima.</span><span class="sxs-lookup"><span data-stu-id="dd693-2446">When an outer variable is referenced by an anonymous function, the outer variable is said to have been ***captured*** by the anonymous function.</span></span> <span data-ttu-id="dd693-2447">Normalmente, o tempo de vida de uma variável local é limitado à execução do bloco ou da instrução com a qual está associado ([variáveis locais](variables.md#local-variables)).</span><span class="sxs-lookup"><span data-stu-id="dd693-2447">Ordinarily, the lifetime of a local variable is limited to execution of the block or statement with which it is associated ([Local variables](variables.md#local-variables)).</span></span> <span data-ttu-id="dd693-2448">No entanto, o tempo de vida de uma variável externa capturada é estendido pelo menos até que a árvore de expressão ou delegado criada na função anônima se torne elegível para a coleta de lixo.</span><span class="sxs-lookup"><span data-stu-id="dd693-2448">However, the lifetime of a captured outer variable is extended at least until the delegate or expression tree created from the anonymous function becomes eligible for garbage collection.</span></span>

<span data-ttu-id="dd693-2449">No exemplo</span><span class="sxs-lookup"><span data-stu-id="dd693-2449">In the example</span></span>
```csharp
using System;

delegate int D();

class Test
{
    static D F() {
        int x = 0;
        D result = () => ++x;
        return result;
    }

    static void Main() {
        D d = F();
        Console.WriteLine(d());
        Console.WriteLine(d());
        Console.WriteLine(d());
    }
}
```
<span data-ttu-id="dd693-2450">a variável local `x` é capturada pela função anônima, e o tempo de vida de `x` é estendido pelo menos até que o delegado retornado de `F` se torne qualificado para coleta de lixo (o que não ocorre até o final do programa).</span><span class="sxs-lookup"><span data-stu-id="dd693-2450">the local variable `x` is captured by the anonymous function, and the lifetime of `x` is extended at least until the delegate returned from `F` becomes eligible for garbage collection (which doesn't happen until the very end of the program).</span></span> <span data-ttu-id="dd693-2451">Como cada invocação da função anônima opera na mesma instância do `x`, a saída do exemplo é:</span><span class="sxs-lookup"><span data-stu-id="dd693-2451">Since each invocation of the anonymous function operates on the same instance of `x`, the output of the example is:</span></span>
```console
1
2
3
```

<span data-ttu-id="dd693-2452">Quando uma variável local ou um parâmetro de valor é capturado por uma função anônima, a variável local ou o parâmetro não é mais considerado uma variável fixa ([variáveis fixas e móveis](unsafe-code.md#fixed-and-moveable-variables)), mas, em vez disso, é considerado uma variável móvel.</span><span class="sxs-lookup"><span data-stu-id="dd693-2452">When a local variable or a value parameter is captured by an anonymous function, the local variable or parameter is no longer considered to be a fixed variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)), but is instead considered to be a moveable variable.</span></span> <span data-ttu-id="dd693-2453">Portanto, qualquer código `unsafe` que pega o endereço de uma variável externa capturada deve primeiro usar a instrução `fixed` para corrigir a variável.</span><span class="sxs-lookup"><span data-stu-id="dd693-2453">Thus any `unsafe` code that takes the address of a captured outer variable must first use the `fixed` statement to fix the variable.</span></span>

<span data-ttu-id="dd693-2454">Observe que, diferentemente de uma variável não capturada, uma variável local capturada pode ser exposta simultaneamente a vários threads de execução.</span><span class="sxs-lookup"><span data-stu-id="dd693-2454">Note that unlike an uncaptured variable, a captured local variable can be simultaneously exposed to multiple threads of execution.</span></span>

#### <a name="instantiation-of-local-variables"></a><span data-ttu-id="dd693-2455">Instanciação de variáveis locais</span><span class="sxs-lookup"><span data-stu-id="dd693-2455">Instantiation of local variables</span></span>

<span data-ttu-id="dd693-2456">Uma variável local é considerada como ***instanciada*** quando a execução entra no escopo da variável.</span><span class="sxs-lookup"><span data-stu-id="dd693-2456">A local variable is considered to be ***instantiated*** when execution enters the scope of the variable.</span></span> <span data-ttu-id="dd693-2457">Por exemplo, quando o método a seguir é invocado, a variável local `x` é instanciada e inicializada três vezes — uma vez para cada iteração do loop.</span><span class="sxs-lookup"><span data-stu-id="dd693-2457">For example, when the following method is invoked, the local variable `x` is instantiated and initialized three times—once for each iteration of the loop.</span></span>

```csharp
static void F() {
    for (int i = 0; i < 3; i++) {
        int x = i * 2 + 1;
        ...
    }
}
```

<span data-ttu-id="dd693-2458">No entanto, mover a declaração de `x` fora do loop resulta em uma única instanciação de `x`:</span><span class="sxs-lookup"><span data-stu-id="dd693-2458">However, moving the declaration of `x` outside the loop results in a single instantiation of `x`:</span></span>
```csharp
static void F() {
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        ...
    }
}
```

<span data-ttu-id="dd693-2459">Quando não é capturado, não há como observar exatamente com que frequência uma variável local é instanciada — como os tempos de vida das instâncias são não contíguos, é possível que cada instanciação simplesmente use o mesmo local de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="dd693-2459">When not captured, there is no way to observe exactly how often a local variable is instantiated—because the lifetimes of the instantiations are disjoint, it is possible for each instantiation to simply use the same storage location.</span></span> <span data-ttu-id="dd693-2460">No entanto, quando uma função anônima captura uma variável local, os efeitos da instanciação se tornam aparentes.</span><span class="sxs-lookup"><span data-stu-id="dd693-2460">However, when an anonymous function captures a local variable, the effects of instantiation become apparent.</span></span>

<span data-ttu-id="dd693-2461">O exemplo</span><span class="sxs-lookup"><span data-stu-id="dd693-2461">The example</span></span>
```csharp
using System;

delegate void D();

class Test
{
    static D[] F() {
        D[] result = new D[3];
        for (int i = 0; i < 3; i++) {
            int x = i * 2 + 1;
            result[i] = () => { Console.WriteLine(x); };
        }
        return result;
    }

    static void Main() {
        foreach (D d in F()) d();
    }
}
```
<span data-ttu-id="dd693-2462">produz a saída:</span><span class="sxs-lookup"><span data-stu-id="dd693-2462">produces the output:</span></span>
```console
1
3
5
```

<span data-ttu-id="dd693-2463">No entanto, quando a declaração de `x` é movida para fora do loop:</span><span class="sxs-lookup"><span data-stu-id="dd693-2463">However, when the declaration of `x` is moved outside the loop:</span></span>
```csharp
static D[] F() {
    D[] result = new D[3];
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        result[i] = () => { Console.WriteLine(x); };
    }
    return result;
}
```
<span data-ttu-id="dd693-2464">a saída é:</span><span class="sxs-lookup"><span data-stu-id="dd693-2464">the output is:</span></span>
```console
5
5
5
```

<span data-ttu-id="dd693-2465">Se um loop for declarar uma variável de iteração, essa variável será considerada como sendo declarada fora do loop.</span><span class="sxs-lookup"><span data-stu-id="dd693-2465">If a for-loop declares an iteration variable, that variable itself is considered to be declared outside of the loop.</span></span> <span data-ttu-id="dd693-2466">Portanto, se o exemplo for alterado para capturar a própria variável de iteração:</span><span class="sxs-lookup"><span data-stu-id="dd693-2466">Thus, if the example is changed to capture the iteration variable itself:</span></span>

```csharp
static D[] F() {
    D[] result = new D[3];
    for (int i = 0; i < 3; i++) {
        result[i] = () => { Console.WriteLine(i); };
    }
    return result;
}
```
<span data-ttu-id="dd693-2467">somente uma instância da variável de iteração é capturada, o que produz a saída:</span><span class="sxs-lookup"><span data-stu-id="dd693-2467">only one instance of the iteration variable is captured, which produces the output:</span></span>
```console
3
3
3
```

<span data-ttu-id="dd693-2468">É possível que os delegados de função anônima compartilhem algumas variáveis capturadas, embora tenham instâncias separadas de outros.</span><span class="sxs-lookup"><span data-stu-id="dd693-2468">It is possible for anonymous function delegates to share some captured variables yet have separate instances of others.</span></span> <span data-ttu-id="dd693-2469">Por exemplo, se `F` for alterado para</span><span class="sxs-lookup"><span data-stu-id="dd693-2469">For example, if `F` is changed to</span></span>
```csharp
static D[] F() {
    D[] result = new D[3];
    int x = 0;
    for (int i = 0; i < 3; i++) {
        int y = 0;
        result[i] = () => { Console.WriteLine("{0} {1}", ++x, ++y); };
    }
    return result;
}
```
<span data-ttu-id="dd693-2470">os três delegados capturam a mesma instância de `x`, mas instâncias separadas de `y`, e a saída é:</span><span class="sxs-lookup"><span data-stu-id="dd693-2470">the three delegates capture the same instance of `x` but separate instances of `y`, and the output is:</span></span>
```console
1 1
2 1
3 1
```

<span data-ttu-id="dd693-2471">Funções anônimas separadas podem capturar a mesma instância de uma variável externa.</span><span class="sxs-lookup"><span data-stu-id="dd693-2471">Separate anonymous functions can capture the same instance of an outer variable.</span></span> <span data-ttu-id="dd693-2472">No exemplo:</span><span class="sxs-lookup"><span data-stu-id="dd693-2472">In the example:</span></span>
```csharp
using System;

delegate void Setter(int value);

delegate int Getter();

class Test
{
    static void Main() {
        int x = 0;
        Setter s = (int value) => { x = value; };
        Getter g = () => { return x; };
        s(5);
        Console.WriteLine(g());
        s(10);
        Console.WriteLine(g());
    }
}
```
<span data-ttu-id="dd693-2473">as duas funções anônimas capturam a mesma instância da variável local `x`e, portanto, podem "se comunicar" por meio dessa variável.</span><span class="sxs-lookup"><span data-stu-id="dd693-2473">the two anonymous functions capture the same instance of the local variable `x`, and they can thus "communicate" through that variable.</span></span> <span data-ttu-id="dd693-2474">A saída do exemplo é:</span><span class="sxs-lookup"><span data-stu-id="dd693-2474">The output of the example is:</span></span>
```console
5
10
```

### <a name="evaluation-of-anonymous-function-expressions"></a><span data-ttu-id="dd693-2475">Avaliação de expressões de função anônimas</span><span class="sxs-lookup"><span data-stu-id="dd693-2475">Evaluation of anonymous function expressions</span></span>

<span data-ttu-id="dd693-2476">Uma função anônima `F` sempre deve ser convertida em um tipo delegado `D` ou um tipo de árvore de expressão `E`, diretamente ou por meio da execução de uma expressão de criação de delegado `new D(F)`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2476">An anonymous function `F` must always be converted to a delegate type `D` or an expression tree type `E`, either directly or through the execution of a delegate creation expression `new D(F)`.</span></span> <span data-ttu-id="dd693-2477">Essa conversão determina o resultado da função anônima, conforme descrito em [conversões de função anônimas](conversions.md#anonymous-function-conversions).</span><span class="sxs-lookup"><span data-stu-id="dd693-2477">This conversion determines the result of the anonymous function, as described in [Anonymous function conversions](conversions.md#anonymous-function-conversions).</span></span>

## <a name="query-expressions"></a><span data-ttu-id="dd693-2478">Expressões de consulta</span><span class="sxs-lookup"><span data-stu-id="dd693-2478">Query expressions</span></span>

<span data-ttu-id="dd693-2479">As ***expressões de consulta*** fornecem uma sintaxe de linguagem integrada para consultas que são semelhantes a linguagens de consulta relacionais e hierárquicas, como SQL e XQuery.</span><span class="sxs-lookup"><span data-stu-id="dd693-2479">***Query expressions*** provide a language integrated syntax for queries that is similar to relational and hierarchical query languages such as SQL and XQuery.</span></span>

```antlr
query_expression
    : from_clause query_body
    ;

from_clause
    : 'from' type? identifier 'in' expression
    ;

query_body
    : query_body_clauses? select_or_group_clause query_continuation?
    ;

query_body_clauses
    : query_body_clause
    | query_body_clauses query_body_clause
    ;

query_body_clause
    : from_clause
    | let_clause
    | where_clause
    | join_clause
    | join_into_clause
    | orderby_clause
    ;

let_clause
    : 'let' identifier '=' expression
    ;

where_clause
    : 'where' boolean_expression
    ;

join_clause
    : 'join' type? identifier 'in' expression 'on' expression 'equals' expression
    ;

join_into_clause
    : 'join' type? identifier 'in' expression 'on' expression 'equals' expression 'into' identifier
    ;

orderby_clause
    : 'orderby' orderings
    ;

orderings
    : ordering (',' ordering)*
    ;

ordering
    : expression ordering_direction?
    ;

ordering_direction
    : 'ascending'
    | 'descending'
    ;

select_or_group_clause
    : select_clause
    | group_clause
    ;

select_clause
    : 'select' expression
    ;

group_clause
    : 'group' expression 'by' expression
    ;

query_continuation
    : 'into' identifier query_body
    ;
```

<span data-ttu-id="dd693-2480">Uma expressão de consulta começa com uma cláusula `from` e termina com uma cláusula `select` ou `group`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2480">A query expression begins with a `from` clause and ends with either a `select` or `group` clause.</span></span> <span data-ttu-id="dd693-2481">A cláusula de `from` inicial pode ser seguida por zero ou mais `from`, `let`, `where`, `join` ou `orderby` cláusulas.</span><span class="sxs-lookup"><span data-stu-id="dd693-2481">The initial `from` clause can be followed by zero or more `from`, `let`, `where`, `join` or `orderby` clauses.</span></span> <span data-ttu-id="dd693-2482">Cada cláusula de `from` é um gerador que introduz uma ***variável de intervalo*** que varia sobre os elementos de uma ***sequência***.</span><span class="sxs-lookup"><span data-stu-id="dd693-2482">Each `from` clause is a generator introducing a ***range variable*** which ranges over the elements of a ***sequence***.</span></span> <span data-ttu-id="dd693-2483">Cada cláusula de `let` introduz uma variável de intervalo que representa um valor calculado por meio de variáveis de intervalo anteriores.</span><span class="sxs-lookup"><span data-stu-id="dd693-2483">Each `let` clause introduces a range variable representing a value computed by means of previous range variables.</span></span> <span data-ttu-id="dd693-2484">Cada cláusula de `where` é um filtro que exclui itens do resultado.</span><span class="sxs-lookup"><span data-stu-id="dd693-2484">Each `where` clause is a filter that excludes items from the result.</span></span> <span data-ttu-id="dd693-2485">Cada cláusula `join` compara as chaves especificadas da sequência de origem com chaves de outra sequência, gerando pares correspondentes.</span><span class="sxs-lookup"><span data-stu-id="dd693-2485">Each `join` clause compares specified keys of the source sequence with keys of another sequence, yielding matching pairs.</span></span> <span data-ttu-id="dd693-2486">Cada cláusula de `orderby` reordena os itens de acordo com os critérios especificados. A cláusula final `select` ou `group` especifica a forma do resultado em termos das variáveis de intervalo.</span><span class="sxs-lookup"><span data-stu-id="dd693-2486">Each `orderby` clause reorders items according to specified criteria.The final `select` or `group` clause specifies the shape of the result in terms of the range variables.</span></span> <span data-ttu-id="dd693-2487">Por fim, uma cláusula `into` pode ser usada para "unir" consultas tratando os resultados de uma consulta como um gerador em uma consulta subsequente.</span><span class="sxs-lookup"><span data-stu-id="dd693-2487">Finally, an `into` clause can be used to "splice" queries by treating the results of one query as a generator in a subsequent query.</span></span>

### <a name="ambiguities-in-query-expressions"></a><span data-ttu-id="dd693-2488">Ambiguidades em expressões de consulta</span><span class="sxs-lookup"><span data-stu-id="dd693-2488">Ambiguities in query expressions</span></span>

<span data-ttu-id="dd693-2489">As expressões de consulta contêm um número de "palavras-chave contextuais", ou seja, identificadores que têm significado especial em um determinado contexto.</span><span class="sxs-lookup"><span data-stu-id="dd693-2489">Query expressions contain a number of "contextual keywords", i.e., identifiers that have special meaning in a given context.</span></span> <span data-ttu-id="dd693-2490">Especificamente, esses são `from`, `where`, `join`, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, `select`, `group` e `by`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2490">Specifically these are `from`, `where`, `join`, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, `select`, `group` and `by`.</span></span> <span data-ttu-id="dd693-2491">Para evitar ambigüidades em expressões de consulta causadas pelo uso misto desses identificadores como palavras-chave ou nomes simples, esses identificadores são considerados palavras-chave quando ocorrem em qualquer lugar dentro de uma expressão de consulta.</span><span class="sxs-lookup"><span data-stu-id="dd693-2491">In order to avoid ambiguities in query expressions caused by mixed use of these identifiers as keywords or simple names, these identifiers are considered keywords when occurring anywhere within a query expression.</span></span>

<span data-ttu-id="dd693-2492">Para essa finalidade, uma expressão de consulta é qualquer expressão que comece com "`from identifier`" seguida por qualquer token, exceto "`;`", "`=`" ou "`,`".</span><span class="sxs-lookup"><span data-stu-id="dd693-2492">For this purpose, a query expression is any expression that starts with "`from identifier`" followed by any token except "`;`", "`=`" or "`,`".</span></span>

<span data-ttu-id="dd693-2493">Para usar essas palavras como identificadores dentro de uma expressão de consulta, elas podem ser prefixadas com "`@`" ([identificadores](lexical-structure.md#identifiers)).</span><span class="sxs-lookup"><span data-stu-id="dd693-2493">In order to use these words as identifiers within a query expression, they can be prefixed with "`@`" ([Identifiers](lexical-structure.md#identifiers)).</span></span>

### <a name="query-expression-translation"></a><span data-ttu-id="dd693-2494">Conversão de expressão de consulta</span><span class="sxs-lookup"><span data-stu-id="dd693-2494">Query expression translation</span></span>

<span data-ttu-id="dd693-2495">O C# idioma não especifica a semântica de execução das expressões de consulta.</span><span class="sxs-lookup"><span data-stu-id="dd693-2495">The C# language does not specify the execution semantics of query expressions.</span></span> <span data-ttu-id="dd693-2496">Em vez disso, as expressões de consulta são convertidas em invocações de métodos que aderem ao *padrão de expressão de consulta* ([o padrão de expressão de consulta](expressions.md#the-query-expression-pattern)).</span><span class="sxs-lookup"><span data-stu-id="dd693-2496">Rather, query expressions are translated into invocations of methods that adhere to the *query expression pattern* ([The query expression pattern](expressions.md#the-query-expression-pattern)).</span></span> <span data-ttu-id="dd693-2497">Especificamente, as expressões de consulta são convertidas em invocações de métodos chamados `Where`, `Select`, `SelectMany`, `Join`, `GroupJoin`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `GroupBy`e `Cast`. Esses métodos devem ter assinaturas e tipos de resultados específicos, conforme descrito no [padrão de expressão de consulta](expressions.md#the-query-expression-pattern).</span><span class="sxs-lookup"><span data-stu-id="dd693-2497">Specifically, query expressions are translated into invocations of methods named `Where`, `Select`, `SelectMany`, `Join`, `GroupJoin`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `GroupBy`, and `Cast`.These methods are expected to have particular signatures and result types, as described in [The query expression pattern](expressions.md#the-query-expression-pattern).</span></span> <span data-ttu-id="dd693-2498">Esses métodos podem ser métodos de instância do objeto que está sendo consultado ou métodos de extensão que são externos ao objeto e implementam a execução real da consulta.</span><span class="sxs-lookup"><span data-stu-id="dd693-2498">These methods can be instance methods of the object being queried or extension methods that are external to the object, and they implement the actual execution of the query.</span></span>

<span data-ttu-id="dd693-2499">A conversão de expressões de consulta para invocações de método é um mapeamento sintático que ocorre antes que qualquer associação de tipo ou resolução de sobrecarga tenha sido executada.</span><span class="sxs-lookup"><span data-stu-id="dd693-2499">The translation from query expressions to method invocations is a syntactic mapping that occurs before any type binding or overload resolution has been performed.</span></span> <span data-ttu-id="dd693-2500">A conversão tem a garantia de estar sintaticamente correta, mas não há garantia de que ele produza código C# semanticamente correto.</span><span class="sxs-lookup"><span data-stu-id="dd693-2500">The translation is guaranteed to be syntactically correct, but it is not guaranteed to produce semantically correct C# code.</span></span> <span data-ttu-id="dd693-2501">Após a tradução das expressões de consulta, as invocações de método resultantes são processadas como invocações de método regular, e isso pode, por sua vez, revelar erros, por exemplo, se os métodos não existirem, se os argumentos tiverem tipos incorretos ou se os métodos forem genéricos e falha na inferência de tipos.</span><span class="sxs-lookup"><span data-stu-id="dd693-2501">Following translation of query expressions, the resulting method invocations are processed as regular method invocations, and this may in turn uncover errors, for example if the methods do not exist, if arguments have wrong types, or if the methods are generic and type inference fails.</span></span>

<span data-ttu-id="dd693-2502">Uma expressão de consulta é processada pela aplicação repetida das seguintes traduções até que nenhuma redução adicional seja possível.</span><span class="sxs-lookup"><span data-stu-id="dd693-2502">A query expression is processed by repeatedly applying the following translations until no further reductions are possible.</span></span> <span data-ttu-id="dd693-2503">As traduções são listadas em ordem de aplicativo: cada seção pressupõe que as traduções nas seções anteriores foram executadas exaustivamente e, uma vez esgotadas, uma seção não será revisitada posteriormente no processamento da mesma expressão de consulta.</span><span class="sxs-lookup"><span data-stu-id="dd693-2503">The translations are listed in order of application: each section assumes that the translations in the preceding sections have been performed exhaustively, and once exhausted, a section will not later be revisited in the processing of the same query expression.</span></span>

<span data-ttu-id="dd693-2504">A atribuição de variáveis de intervalo não é permitida em expressões de consulta.</span><span class="sxs-lookup"><span data-stu-id="dd693-2504">Assignment to range variables is not allowed in query expressions.</span></span> <span data-ttu-id="dd693-2505">No entanto, uma C# implementação é permitida nem sempre impor essa restrição, uma vez que isso pode, às vezes, não ser possível com o esquema de tradução sintático apresentado aqui.</span><span class="sxs-lookup"><span data-stu-id="dd693-2505">However a C# implementation is permitted to not always enforce this restriction, since this may sometimes not be possible with the syntactic translation scheme presented here.</span></span>

<span data-ttu-id="dd693-2506">Determinadas traduções injetam variáveis de intervalo com identificadores transparentes denotados por `*`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2506">Certain translations inject range variables with transparent identifiers denoted by `*`.</span></span> <span data-ttu-id="dd693-2507">As propriedades especiais de identificadores transparentes são discutidas mais detalhadamente em [identificadores transparentes](expressions.md#transparent-identifiers).</span><span class="sxs-lookup"><span data-stu-id="dd693-2507">The special properties of transparent identifiers are discussed further in [Transparent identifiers](expressions.md#transparent-identifiers).</span></span>

#### <a name="select-and-groupby-clauses-with-continuations"></a><span data-ttu-id="dd693-2508">Cláusulas Select e GroupBy com continuaçãos</span><span class="sxs-lookup"><span data-stu-id="dd693-2508">Select and groupby clauses with continuations</span></span>

<span data-ttu-id="dd693-2509">Uma expressão de consulta com uma continuação</span><span class="sxs-lookup"><span data-stu-id="dd693-2509">A query expression with a continuation</span></span>
```csharp
from ... into x ...
```
<span data-ttu-id="dd693-2510">é convertido em</span><span class="sxs-lookup"><span data-stu-id="dd693-2510">is translated into</span></span>
```csharp
from x in ( from ... ) ...
```

<span data-ttu-id="dd693-2511">As traduções nas seções a seguir pressupõem que as consultas não têm `into` de continuação.</span><span class="sxs-lookup"><span data-stu-id="dd693-2511">The translations in the following sections assume that queries have no `into` continuations.</span></span>

<span data-ttu-id="dd693-2512">O exemplo</span><span class="sxs-lookup"><span data-stu-id="dd693-2512">The example</span></span>
```csharp
from c in customers
group c by c.Country into g
select new { Country = g.Key, CustCount = g.Count() }
```
<span data-ttu-id="dd693-2513">é convertido em</span><span class="sxs-lookup"><span data-stu-id="dd693-2513">is translated into</span></span>
```csharp
from g in
    from c in customers
    group c by c.Country
select new { Country = g.Key, CustCount = g.Count() }
```
<span data-ttu-id="dd693-2514">a tradução final do que é</span><span class="sxs-lookup"><span data-stu-id="dd693-2514">the final translation of which is</span></span>
```csharp
customers.
GroupBy(c => c.Country).
Select(g => new { Country = g.Key, CustCount = g.Count() })
```

#### <a name="explicit-range-variable-types"></a><span data-ttu-id="dd693-2515">Tipos de variável de intervalo explícito</span><span class="sxs-lookup"><span data-stu-id="dd693-2515">Explicit range variable types</span></span>

<span data-ttu-id="dd693-2516">Uma cláusula `from` que especifica explicitamente um tipo de variável de intervalo</span><span class="sxs-lookup"><span data-stu-id="dd693-2516">A `from` clause that explicitly specifies a range variable type</span></span>
```csharp
from T x in e
```
<span data-ttu-id="dd693-2517">é convertido em</span><span class="sxs-lookup"><span data-stu-id="dd693-2517">is translated into</span></span>
```csharp
from x in ( e ) . Cast < T > ( )
```

<span data-ttu-id="dd693-2518">Uma cláusula `join` que especifica explicitamente um tipo de variável de intervalo</span><span class="sxs-lookup"><span data-stu-id="dd693-2518">A `join` clause that explicitly specifies a range variable type</span></span>
```csharp
join T x in e on k1 equals k2
```
<span data-ttu-id="dd693-2519">é convertido em</span><span class="sxs-lookup"><span data-stu-id="dd693-2519">is translated into</span></span>
```csharp
join x in ( e ) . Cast < T > ( ) on k1 equals k2
```

<span data-ttu-id="dd693-2520">As traduções nas seções a seguir pressupõem que as consultas não têm nenhum tipo de variável de intervalo explícito.</span><span class="sxs-lookup"><span data-stu-id="dd693-2520">The translations in the following sections assume that queries have no explicit range variable types.</span></span>

<span data-ttu-id="dd693-2521">O exemplo</span><span class="sxs-lookup"><span data-stu-id="dd693-2521">The example</span></span>
```csharp
from Customer c in customers
where c.City == "London"
select c
```
<span data-ttu-id="dd693-2522">é convertido em</span><span class="sxs-lookup"><span data-stu-id="dd693-2522">is translated into</span></span>
```csharp
from c in customers.Cast<Customer>()
where c.City == "London"
select c
```
<span data-ttu-id="dd693-2523">a tradução final do que é</span><span class="sxs-lookup"><span data-stu-id="dd693-2523">the final translation of which is</span></span>
```csharp
customers.
Cast<Customer>().
Where(c => c.City == "London")
```

<span data-ttu-id="dd693-2524">Os tipos de variável de intervalo explícitos são úteis para consultar coleções que implementam a interface de `IEnumerable` não genérica, mas não a interface genérica `IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2524">Explicit range variable types are useful for querying collections that implement the non-generic `IEnumerable` interface, but not the generic `IEnumerable<T>` interface.</span></span> <span data-ttu-id="dd693-2525">No exemplo acima, esse seria o caso se `customers` fosse do tipo `ArrayList`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2525">In the example above, this would be the case if `customers` were of type `ArrayList`.</span></span>

#### <a name="degenerate-query-expressions"></a><span data-ttu-id="dd693-2526">Degerar expressões de consulta</span><span class="sxs-lookup"><span data-stu-id="dd693-2526">Degenerate query expressions</span></span>

<span data-ttu-id="dd693-2527">Uma expressão de consulta do formulário</span><span class="sxs-lookup"><span data-stu-id="dd693-2527">A query expression of the form</span></span>
```csharp
from x in e select x
```
<span data-ttu-id="dd693-2528">é convertido em</span><span class="sxs-lookup"><span data-stu-id="dd693-2528">is translated into</span></span>
```csharp
( e ) . Select ( x => x )
```

<span data-ttu-id="dd693-2529">O exemplo</span><span class="sxs-lookup"><span data-stu-id="dd693-2529">The example</span></span>
```csharp
from c in customers
select c
```
<span data-ttu-id="dd693-2530">é convertido em</span><span class="sxs-lookup"><span data-stu-id="dd693-2530">is translated into</span></span>
```csharp
customers.Select(c => c)
```

<span data-ttu-id="dd693-2531">Uma expressão de consulta de degeneração é aquela que seleciona de trivial os elementos da origem.</span><span class="sxs-lookup"><span data-stu-id="dd693-2531">A degenerate query expression is one that trivially selects the elements of the source.</span></span> <span data-ttu-id="dd693-2532">Uma fase posterior da tradução remove as consultas de degeneração introduzidas por outras etapas de conversão, substituindo-as por sua fonte.</span><span class="sxs-lookup"><span data-stu-id="dd693-2532">A later phase of the translation removes degenerate queries introduced by other translation steps by replacing them with their source.</span></span> <span data-ttu-id="dd693-2533">No entanto, é importante garantir que o resultado de uma expressão de consulta nunca seja o próprio objeto de origem, pois isso revelaria o tipo e a identidade da origem para o cliente da consulta.</span><span class="sxs-lookup"><span data-stu-id="dd693-2533">It is important however to ensure that the result of a query expression is never the source object itself, as that would reveal the type and identity of the source to the client of the query.</span></span> <span data-ttu-id="dd693-2534">Portanto, essa etapa protege as consultas de degeneração escritas diretamente no código-fonte chamando explicitamente `Select` na fonte.</span><span class="sxs-lookup"><span data-stu-id="dd693-2534">Therefore this step protects degenerate queries written directly in source code by explicitly calling `Select` on the source.</span></span> <span data-ttu-id="dd693-2535">Em seguida, é até os implementadores de `Select` e outros operadores de consulta para garantir que esses métodos nunca retornem o próprio objeto de origem.</span><span class="sxs-lookup"><span data-stu-id="dd693-2535">It is then up to the implementers of `Select` and other query operators to ensure that these methods never return the source object itself.</span></span>

#### <a name="from-let-where-join-and-orderby-clauses"></a><span data-ttu-id="dd693-2536">De, Let, Where, cláusulas Join e OrderBy</span><span class="sxs-lookup"><span data-stu-id="dd693-2536">From, let, where, join and orderby clauses</span></span>

<span data-ttu-id="dd693-2537">Uma expressão de consulta com uma segunda cláusula `from` seguida por uma cláusula `select`</span><span class="sxs-lookup"><span data-stu-id="dd693-2537">A query expression with a second `from` clause followed by a `select` clause</span></span>
```csharp
from x1 in e1
from x2 in e2
select v
```
<span data-ttu-id="dd693-2538">é convertido em</span><span class="sxs-lookup"><span data-stu-id="dd693-2538">is translated into</span></span>
```csharp
( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => v )
```

<span data-ttu-id="dd693-2539">Uma expressão de consulta com uma segunda cláusula `from` seguida por algo diferente de uma cláusula `select`:</span><span class="sxs-lookup"><span data-stu-id="dd693-2539">A query expression with a second `from` clause followed by something other than a `select` clause:</span></span>

```csharp
from x1 in e1
from x2 in e2
...
```
<span data-ttu-id="dd693-2540">é convertido em</span><span class="sxs-lookup"><span data-stu-id="dd693-2540">is translated into</span></span>
```csharp
from * in ( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => new { x1 , x2 } )
...
```

<span data-ttu-id="dd693-2541">Uma expressão de consulta com uma cláusula `let`</span><span class="sxs-lookup"><span data-stu-id="dd693-2541">A query expression with a `let` clause</span></span>
```csharp
from x in e
let y = f
...
```
<span data-ttu-id="dd693-2542">é convertido em</span><span class="sxs-lookup"><span data-stu-id="dd693-2542">is translated into</span></span>
```csharp
from * in ( e ) . Select ( x => new { x , y = f } )
...
```

<span data-ttu-id="dd693-2543">Uma expressão de consulta com uma cláusula `where`</span><span class="sxs-lookup"><span data-stu-id="dd693-2543">A query expression with a `where` clause</span></span>
```csharp
from x in e
where f
...
```
<span data-ttu-id="dd693-2544">é convertido em</span><span class="sxs-lookup"><span data-stu-id="dd693-2544">is translated into</span></span>
```csharp
from x in ( e ) . Where ( x => f )
...
```

<span data-ttu-id="dd693-2545">Uma expressão de consulta com uma cláusula `join` sem uma `into` seguida por uma cláusula `select`</span><span class="sxs-lookup"><span data-stu-id="dd693-2545">A query expression with a `join` clause without an `into` followed by a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
select v
```
<span data-ttu-id="dd693-2546">é convertido em</span><span class="sxs-lookup"><span data-stu-id="dd693-2546">is translated into</span></span>
```csharp
( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => v )
```

<span data-ttu-id="dd693-2547">Uma expressão de consulta com uma cláusula `join` sem uma `into` seguida por algo diferente de uma cláusula `select`</span><span class="sxs-lookup"><span data-stu-id="dd693-2547">A query expression with a `join` clause without an `into` followed by something other than a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
...
```
<span data-ttu-id="dd693-2548">é convertido em</span><span class="sxs-lookup"><span data-stu-id="dd693-2548">is translated into</span></span>
```csharp
from * in ( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => new { x1 , x2 })
...
```

<span data-ttu-id="dd693-2549">Uma expressão de consulta com uma cláusula `join` com uma `into` seguida por uma cláusula `select`</span><span class="sxs-lookup"><span data-stu-id="dd693-2549">A query expression with a `join` clause with an `into` followed by a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
select v
```
<span data-ttu-id="dd693-2550">é convertido em</span><span class="sxs-lookup"><span data-stu-id="dd693-2550">is translated into</span></span>
```csharp
( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => v )
```

<span data-ttu-id="dd693-2551">Uma expressão de consulta com uma cláusula `join` com uma `into` seguida por algo diferente de uma cláusula `select`</span><span class="sxs-lookup"><span data-stu-id="dd693-2551">A query expression with a `join` clause with an `into` followed by something other than a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
...
```
<span data-ttu-id="dd693-2552">é convertido em</span><span class="sxs-lookup"><span data-stu-id="dd693-2552">is translated into</span></span>
```csharp
from * in ( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => new { x1 , g })
...
```

<span data-ttu-id="dd693-2553">Uma expressão de consulta com uma cláusula `orderby`</span><span class="sxs-lookup"><span data-stu-id="dd693-2553">A query expression with an `orderby` clause</span></span>
```csharp
from x in e
orderby k1 , k2 , ..., kn
...
```
<span data-ttu-id="dd693-2554">é convertido em</span><span class="sxs-lookup"><span data-stu-id="dd693-2554">is translated into</span></span>
```csharp
from x in ( e ) . 
OrderBy ( x => k1 ) . 
ThenBy ( x => k2 ) .
... .
ThenBy ( x => kn )
...
```

<span data-ttu-id="dd693-2555">Se uma cláusula de ordenação especificar um indicador de direção de `descending`, uma invocação de `OrderByDescending` ou `ThenByDescending` será produzida em vez disso.</span><span class="sxs-lookup"><span data-stu-id="dd693-2555">If an ordering clause specifies a `descending` direction indicator, an invocation of `OrderByDescending` or `ThenByDescending` is produced instead.</span></span>

<span data-ttu-id="dd693-2556">As traduções a seguir pressupõem que não há cláusulas `let`, `where`, `join` ou `orderby` e não mais do que a cláusula de `from` inicial em cada expressão de consulta.</span><span class="sxs-lookup"><span data-stu-id="dd693-2556">The following translations assume that there are no `let`, `where`, `join` or `orderby` clauses, and no more than the one initial `from` clause in each query expression.</span></span>

<span data-ttu-id="dd693-2557">O exemplo</span><span class="sxs-lookup"><span data-stu-id="dd693-2557">The example</span></span>
```csharp
from c in customers
from o in c.Orders
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="dd693-2558">é convertido em</span><span class="sxs-lookup"><span data-stu-id="dd693-2558">is translated into</span></span>
```csharp
customers.
SelectMany(c => c.Orders,
     (c,o) => new { c.Name, o.OrderID, o.Total }
)
```

<span data-ttu-id="dd693-2559">O exemplo</span><span class="sxs-lookup"><span data-stu-id="dd693-2559">The example</span></span>
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="dd693-2560">é convertido em</span><span class="sxs-lookup"><span data-stu-id="dd693-2560">is translated into</span></span>
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="dd693-2561">a tradução final do que é</span><span class="sxs-lookup"><span data-stu-id="dd693-2561">the final translation of which is</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.OrderID, x.o.Total })
```
<span data-ttu-id="dd693-2562">onde `x` é um identificador gerado pelo compilador que, de outra forma, é invisível e inacessível.</span><span class="sxs-lookup"><span data-stu-id="dd693-2562">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="dd693-2563">O exemplo</span><span class="sxs-lookup"><span data-stu-id="dd693-2563">The example</span></span>
```csharp
from o in orders
let t = o.Details.Sum(d => d.UnitPrice * d.Quantity)
where t >= 1000
select new { o.OrderID, Total = t }
```
<span data-ttu-id="dd693-2564">é convertido em</span><span class="sxs-lookup"><span data-stu-id="dd693-2564">is translated into</span></span>
```csharp
from * in orders.
    Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) })
where t >= 1000 
select new { o.OrderID, Total = t }
```
<span data-ttu-id="dd693-2565">a tradução final do que é</span><span class="sxs-lookup"><span data-stu-id="dd693-2565">the final translation of which is</span></span>
```csharp
orders.
Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) }).
Where(x => x.t >= 1000).
Select(x => new { x.o.OrderID, Total = x.t })
```
<span data-ttu-id="dd693-2566">onde `x` é um identificador gerado pelo compilador que, de outra forma, é invisível e inacessível.</span><span class="sxs-lookup"><span data-stu-id="dd693-2566">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="dd693-2567">O exemplo</span><span class="sxs-lookup"><span data-stu-id="dd693-2567">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
select new { c.Name, o.OrderDate, o.Total }
```
<span data-ttu-id="dd693-2568">é convertido em</span><span class="sxs-lookup"><span data-stu-id="dd693-2568">is translated into</span></span>
```csharp
customers.Join(orders, c => c.CustomerID, o => o.CustomerID,
    (c, o) => new { c.Name, o.OrderDate, o.Total })
```

<span data-ttu-id="dd693-2569">O exemplo</span><span class="sxs-lookup"><span data-stu-id="dd693-2569">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID into co
let n = co.Count()
where n >= 10
select new { c.Name, OrderCount = n }
```
<span data-ttu-id="dd693-2570">é convertido em</span><span class="sxs-lookup"><span data-stu-id="dd693-2570">is translated into</span></span>
```csharp
from * in customers.
    GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
        (c, co) => new { c, co })
let n = co.Count()
where n >= 10 
select new { c.Name, OrderCount = n }
```
<span data-ttu-id="dd693-2571">a tradução final do que é</span><span class="sxs-lookup"><span data-stu-id="dd693-2571">the final translation of which is</span></span>
```csharp
customers.
GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
    (c, co) => new { c, co }).
Select(x => new { x, n = x.co.Count() }).
Where(y => y.n >= 10).
Select(y => new { y.x.c.Name, OrderCount = y.n)
```
<span data-ttu-id="dd693-2572">onde `x` e `y` são identificadores gerados pelo compilador que, de outra forma, são invisíveis e inacessíveis.</span><span class="sxs-lookup"><span data-stu-id="dd693-2572">where `x` and `y` are compiler generated identifiers that are otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="dd693-2573">O exemplo</span><span class="sxs-lookup"><span data-stu-id="dd693-2573">The example</span></span>
```csharp
from o in orders
orderby o.Customer.Name, o.Total descending
select o
```
<span data-ttu-id="dd693-2574">tem a tradução final</span><span class="sxs-lookup"><span data-stu-id="dd693-2574">has the final translation</span></span>
```csharp
orders.
OrderBy(o => o.Customer.Name).
ThenByDescending(o => o.Total)
```

#### <a name="select-clauses"></a><span data-ttu-id="dd693-2575">Cláusulas de seleção</span><span class="sxs-lookup"><span data-stu-id="dd693-2575">Select clauses</span></span>

<span data-ttu-id="dd693-2576">Uma expressão de consulta do formulário</span><span class="sxs-lookup"><span data-stu-id="dd693-2576">A query expression of the form</span></span>
```csharp
from x in e select v
```
<span data-ttu-id="dd693-2577">é convertido em</span><span class="sxs-lookup"><span data-stu-id="dd693-2577">is translated into</span></span>
```csharp
( e ) . Select ( x => v )
```
<span data-ttu-id="dd693-2578">Exceto quando v é o identificador x, a tradução é simplesmente</span><span class="sxs-lookup"><span data-stu-id="dd693-2578">except when v is the identifier x, the translation is simply</span></span>
```csharp
( e )
```

<span data-ttu-id="dd693-2579">Por exemplo</span><span class="sxs-lookup"><span data-stu-id="dd693-2579">For example</span></span>
```csharp
from c in customers.Where(c => c.City == "London")
select c
```
<span data-ttu-id="dd693-2580">é simplesmente traduzido em</span><span class="sxs-lookup"><span data-stu-id="dd693-2580">is simply translated into</span></span>
```csharp
customers.Where(c => c.City == "London")
```

#### <a name="groupby-clauses"></a><span data-ttu-id="dd693-2581">Cláusulas GroupBy</span><span class="sxs-lookup"><span data-stu-id="dd693-2581">Groupby clauses</span></span>

<span data-ttu-id="dd693-2582">Uma expressão de consulta do formulário</span><span class="sxs-lookup"><span data-stu-id="dd693-2582">A query expression of the form</span></span>
```csharp
from x in e group v by k
```
<span data-ttu-id="dd693-2583">é convertido em</span><span class="sxs-lookup"><span data-stu-id="dd693-2583">is translated into</span></span>
```csharp
( e ) . GroupBy ( x => k , x => v )
```
<span data-ttu-id="dd693-2584">Exceto quando v é o identificador x, a conversão é</span><span class="sxs-lookup"><span data-stu-id="dd693-2584">except when v is the identifier x, the translation is</span></span>
```csharp
( e ) . GroupBy ( x => k )
```

<span data-ttu-id="dd693-2585">O exemplo</span><span class="sxs-lookup"><span data-stu-id="dd693-2585">The example</span></span>
```csharp
from c in customers
group c.Name by c.Country
```
<span data-ttu-id="dd693-2586">é convertido em</span><span class="sxs-lookup"><span data-stu-id="dd693-2586">is translated into</span></span>
```csharp
customers.
GroupBy(c => c.Country, c => c.Name)
```

#### <a name="transparent-identifiers"></a><span data-ttu-id="dd693-2587">Identificadores transparentes</span><span class="sxs-lookup"><span data-stu-id="dd693-2587">Transparent identifiers</span></span>

<span data-ttu-id="dd693-2588">Determinadas traduções injetam variáveis de intervalo com ***identificadores transparentes*** denotados por `*`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2588">Certain translations inject range variables with ***transparent identifiers*** denoted by `*`.</span></span> <span data-ttu-id="dd693-2589">Os identificadores transparentes não são um recurso de idioma adequado; elas existem somente como uma etapa intermediária no processo de conversão de expressão de consulta.</span><span class="sxs-lookup"><span data-stu-id="dd693-2589">Transparent identifiers are not a proper language feature; they exist only as an intermediate step in the query expression translation process.</span></span>

<span data-ttu-id="dd693-2590">Quando uma conversão de consulta injeta um identificador transparente, as etapas de tradução adicionais propagam o identificador transparente para funções anônimas e inicializadores de objeto anônimos.</span><span class="sxs-lookup"><span data-stu-id="dd693-2590">When a query translation injects a transparent identifier, further translation steps propagate the transparent identifier into anonymous functions and anonymous object initializers.</span></span> <span data-ttu-id="dd693-2591">Nesses contextos, os identificadores transparentes têm o seguinte comportamento:</span><span class="sxs-lookup"><span data-stu-id="dd693-2591">In those contexts, transparent identifiers have the following behavior:</span></span>

*  <span data-ttu-id="dd693-2592">Quando um identificador transparente ocorre como um parâmetro em uma função anônima, os membros do tipo anônimo associado são automaticamente no escopo no corpo da função anônima.</span><span class="sxs-lookup"><span data-stu-id="dd693-2592">When a transparent identifier occurs as a parameter in an anonymous function, the members of the associated anonymous type are automatically in scope in the body of the anonymous function.</span></span>
*  <span data-ttu-id="dd693-2593">Quando um membro com um identificador transparente está no escopo, os membros desse membro também estão no escopo.</span><span class="sxs-lookup"><span data-stu-id="dd693-2593">When a member with a transparent identifier is in scope, the members of that member are in scope as well.</span></span>
*  <span data-ttu-id="dd693-2594">Quando um identificador transparente ocorre como um Declarador de membro em um inicializador de objeto anônimo, ele introduz um membro com um identificador transparente.</span><span class="sxs-lookup"><span data-stu-id="dd693-2594">When a transparent identifier occurs as a member declarator in an anonymous object initializer, it introduces a member with a transparent identifier.</span></span>
*  <span data-ttu-id="dd693-2595">Nas etapas de conversão descritas acima, os identificadores transparentes são sempre introduzidos junto com tipos anônimos, com a intenção de capturar várias variáveis de intervalo como membros de um único objeto.</span><span class="sxs-lookup"><span data-stu-id="dd693-2595">In the translation steps described above, transparent identifiers are always introduced together with anonymous types, with the intent of capturing multiple range variables as members of a single object.</span></span> <span data-ttu-id="dd693-2596">Uma implementação do C# tem permissão para usar um mecanismo diferente de tipos anônimos para agrupar várias variáveis de intervalo.</span><span class="sxs-lookup"><span data-stu-id="dd693-2596">An implementation of C# is permitted to use a different mechanism than anonymous types to group together multiple range variables.</span></span> <span data-ttu-id="dd693-2597">Os exemplos de conversão a seguir pressupõem que os tipos anônimos são usados e mostram como os identificadores transparentes podem ser traduzidos fora.</span><span class="sxs-lookup"><span data-stu-id="dd693-2597">The following translation examples assume that anonymous types are used, and show how transparent identifiers can be translated away.</span></span>

<span data-ttu-id="dd693-2598">O exemplo</span><span class="sxs-lookup"><span data-stu-id="dd693-2598">The example</span></span>
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.Total }
```
<span data-ttu-id="dd693-2599">é convertido em</span><span class="sxs-lookup"><span data-stu-id="dd693-2599">is translated into</span></span>
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.Total }
```

<span data-ttu-id="dd693-2600">que é ainda mais convertida em</span><span class="sxs-lookup"><span data-stu-id="dd693-2600">which is further translated into</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(* => o.Total).
Select(* => new { c.Name, o.Total })
```
<span data-ttu-id="dd693-2601">que, quando os identificadores transparentes são apagados, é equivalente a</span><span class="sxs-lookup"><span data-stu-id="dd693-2601">which, when transparent identifiers are erased, is equivalent to</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.Total })
```
<span data-ttu-id="dd693-2602">onde `x` é um identificador gerado pelo compilador que, de outra forma, é invisível e inacessível.</span><span class="sxs-lookup"><span data-stu-id="dd693-2602">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="dd693-2603">O exemplo</span><span class="sxs-lookup"><span data-stu-id="dd693-2603">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
<span data-ttu-id="dd693-2604">é convertido em</span><span class="sxs-lookup"><span data-stu-id="dd693-2604">is translated into</span></span>
```csharp
from * in customers.
    Join(orders, c => c.CustomerID, o => o.CustomerID, 
        (c, o) => new { c, o })
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
<span data-ttu-id="dd693-2605">que é ainda mais reduzido para</span><span class="sxs-lookup"><span data-stu-id="dd693-2605">which is further reduced to</span></span>
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID, (c, o) => new { c, o }).
Join(details, * => o.OrderID, d => d.OrderID, (*, d) => new { *, d }).
Join(products, * => d.ProductID, p => p.ProductID, (*, p) => new { *, p }).
Select(* => new { c.Name, o.OrderDate, p.ProductName })
```
<span data-ttu-id="dd693-2606">a tradução final do que é</span><span class="sxs-lookup"><span data-stu-id="dd693-2606">the final translation of which is</span></span>
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID,
    (c, o) => new { c, o }).
Join(details, x => x.o.OrderID, d => d.OrderID,
    (x, d) => new { x, d }).
Join(products, y => y.d.ProductID, p => p.ProductID,
    (y, p) => new { y, p }).
Select(z => new { z.y.x.c.Name, z.y.x.o.OrderDate, z.p.ProductName })
```
<span data-ttu-id="dd693-2607">onde `x`, `y`e `z` são identificadores gerados pelo compilador que, de outra forma, são invisíveis e inacessíveis.</span><span class="sxs-lookup"><span data-stu-id="dd693-2607">where `x`, `y`, and `z` are compiler generated identifiers that are otherwise invisible and inaccessible.</span></span>

### <a name="the-query-expression-pattern"></a><span data-ttu-id="dd693-2608">O padrão de expressão de consulta</span><span class="sxs-lookup"><span data-stu-id="dd693-2608">The query expression pattern</span></span>

<span data-ttu-id="dd693-2609">O ***padrão de expressão de consulta*** estabelece um padrão de métodos que os tipos podem implementar para dar suporte a expressões de consulta.</span><span class="sxs-lookup"><span data-stu-id="dd693-2609">The ***Query expression pattern*** establishes a pattern of methods that types can implement to support query expressions.</span></span> <span data-ttu-id="dd693-2610">Como as expressões de consulta são convertidas em invocações de método por meio de um mapeamento sintático, os tipos têm flexibilidade considerável em como implementam o padrão de expressão de consulta.</span><span class="sxs-lookup"><span data-stu-id="dd693-2610">Because query expressions are translated to method invocations by means of a syntactic mapping, types have considerable flexibility in how they implement the query expression pattern.</span></span> <span data-ttu-id="dd693-2611">Por exemplo, os métodos do padrão podem ser implementados como métodos de instância ou como métodos de extensão porque os dois têm a mesma sintaxe de invocação, e os métodos podem solicitar delegados ou árvores de expressão porque as funções anônimas são conversíveis para ambos.</span><span class="sxs-lookup"><span data-stu-id="dd693-2611">For example, the methods of the pattern can be implemented as instance methods or as extension methods because the two have the same invocation syntax, and the methods can request delegates or expression trees because anonymous functions are convertible to both.</span></span>

<span data-ttu-id="dd693-2612">A forma recomendada de um `C<T>` de tipo genérico que dá suporte ao padrão de expressão de consulta é mostrada abaixo.</span><span class="sxs-lookup"><span data-stu-id="dd693-2612">The recommended shape of a generic type `C<T>` that supports the query expression pattern is shown below.</span></span> <span data-ttu-id="dd693-2613">Um tipo genérico é usado para ilustrar as relações apropriadas entre os tipos de parâmetro e de resultado, mas é possível implementar o padrão para tipos não genéricos também.</span><span class="sxs-lookup"><span data-stu-id="dd693-2613">A generic type is used in order to illustrate the proper relationships between parameter and result types, but it is possible to implement the pattern for non-generic types as well.</span></span>

```csharp
delegate R Func<T1,R>(T1 arg1);

delegate R Func<T1,T2,R>(T1 arg1, T2 arg2);

class C
{
    public C<T> Cast<T>();
}

class C<T> : C
{
    public C<T> Where(Func<T,bool> predicate);

    public C<U> Select<U>(Func<T,U> selector);

    public C<V> SelectMany<U,V>(Func<T,C<U>> selector,
        Func<T,U,V> resultSelector);

    public C<V> Join<U,K,V>(C<U> inner, Func<T,K> outerKeySelector,
        Func<U,K> innerKeySelector, Func<T,U,V> resultSelector);

    public C<V> GroupJoin<U,K,V>(C<U> inner, Func<T,K> outerKeySelector,
        Func<U,K> innerKeySelector, Func<T,C<U>,V> resultSelector);

    public O<T> OrderBy<K>(Func<T,K> keySelector);

    public O<T> OrderByDescending<K>(Func<T,K> keySelector);

    public C<G<K,T>> GroupBy<K>(Func<T,K> keySelector);

    public C<G<K,E>> GroupBy<K,E>(Func<T,K> keySelector,
        Func<T,E> elementSelector);
}

class O<T> : C<T>
{
    public O<T> ThenBy<K>(Func<T,K> keySelector);

    public O<T> ThenByDescending<K>(Func<T,K> keySelector);
}

class G<K,T> : C<T>
{
    public K Key { get; }
}
```

<span data-ttu-id="dd693-2614">Os métodos acima usam os tipos de delegado genéricos `Func<T1,R>` e `Func<T1,T2,R>`, mas eles poderiam ter usado igualmente outros tipos de árvore de expressão ou de representante com as mesmas relações nos tipos de parâmetro e resultado.</span><span class="sxs-lookup"><span data-stu-id="dd693-2614">The methods above use the generic delegate types `Func<T1,R>` and `Func<T1,T2,R>`, but they could equally well have used other delegate or expression tree types with the same relationships in parameter and result types.</span></span>

<span data-ttu-id="dd693-2615">Observe a relação recomendada entre `C<T>` e `O<T>` que garante que os métodos `ThenBy` e `ThenByDescending` estejam disponíveis somente no resultado de um `OrderBy` ou `OrderByDescending`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2615">Notice the recommended relationship between `C<T>` and `O<T>` which ensures that the `ThenBy` and `ThenByDescending` methods are available only on the result of an `OrderBy` or `OrderByDescending`.</span></span> <span data-ttu-id="dd693-2616">Observe também a forma recomendada do resultado de `GroupBy` – uma sequência de sequências, em que cada sequência interna tem uma propriedade `Key` adicional.</span><span class="sxs-lookup"><span data-stu-id="dd693-2616">Also notice the recommended shape of the result of `GroupBy` -- a sequence of sequences, where each inner sequence has an additional `Key` property.</span></span>

<span data-ttu-id="dd693-2617">O namespace `System.Linq` fornece uma implementação do padrão de operador de consulta para qualquer tipo que implemente a interface `System.Collections.Generic.IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2617">The `System.Linq` namespace provides an implementation of the query operator pattern for any type that implements the `System.Collections.Generic.IEnumerable<T>` interface.</span></span>

## <a name="assignment-operators"></a><span data-ttu-id="dd693-2618">Operadores de atribuição</span><span class="sxs-lookup"><span data-stu-id="dd693-2618">Assignment operators</span></span>

<span data-ttu-id="dd693-2619">Os operadores de atribuição atribuem um novo valor a uma variável, uma propriedade, um evento ou um elemento do indexador.</span><span class="sxs-lookup"><span data-stu-id="dd693-2619">The assignment operators assign a new value to a variable, a property, an event, or an indexer element.</span></span>

```antlr
assignment
    : unary_expression assignment_operator expression
    ;

assignment_operator
    : '='
    | '+='
    | '-='
    | '*='
    | '/='
    | '%='
    | '&='
    | '|='
    | '^='
    | '<<='
    | right_shift_assignment
    ;
```

<span data-ttu-id="dd693-2620">O operando esquerdo de uma atribuição deve ser uma expressão classificada como uma variável, um acesso de propriedade, um acesso de indexador ou um acesso de evento.</span><span class="sxs-lookup"><span data-stu-id="dd693-2620">The left operand of an assignment must be an expression classified as a variable, a property access, an indexer access, or an event access.</span></span>

<span data-ttu-id="dd693-2621">O operador `=` é chamado de ***operador de atribuição simples***.</span><span class="sxs-lookup"><span data-stu-id="dd693-2621">The `=` operator is called the ***simple assignment operator***.</span></span> <span data-ttu-id="dd693-2622">Ele atribui o valor do operando direito à variável, à propriedade ou ao elemento do indexador fornecido pelo operando esquerdo.</span><span class="sxs-lookup"><span data-stu-id="dd693-2622">It assigns the value of the right operand to the variable, property, or indexer element given by the left operand.</span></span> <span data-ttu-id="dd693-2623">O operando esquerdo do operador de atribuição simples não pode ser um acesso de evento (exceto conforme descrito em [eventos de campo](classes.md#field-like-events)).</span><span class="sxs-lookup"><span data-stu-id="dd693-2623">The left operand of the simple assignment operator may not be an event access (except as described in [Field-like events](classes.md#field-like-events)).</span></span> <span data-ttu-id="dd693-2624">O operador de atribuição simples é descrito em [atribuição simples](expressions.md#simple-assignment).</span><span class="sxs-lookup"><span data-stu-id="dd693-2624">The simple assignment operator is described in [Simple assignment](expressions.md#simple-assignment).</span></span>

<span data-ttu-id="dd693-2625">Os operadores de atribuição diferentes do operador de `=` são chamados de ***operadores de atribuição compostos***.</span><span class="sxs-lookup"><span data-stu-id="dd693-2625">The assignment operators other than the `=` operator are called the ***compound assignment operators***.</span></span> <span data-ttu-id="dd693-2626">Esses operadores executam a operação indicada nos dois operandos e, em seguida, atribuim o valor resultante à variável, à propriedade ou ao elemento do indexador fornecido pelo operando esquerdo.</span><span class="sxs-lookup"><span data-stu-id="dd693-2626">These operators perform the indicated operation on the two operands, and then assign the resulting value to the variable, property, or indexer element given by the left operand.</span></span> <span data-ttu-id="dd693-2627">Os operadores de atribuição compostos são descritos em [atribuição composta](expressions.md#compound-assignment).</span><span class="sxs-lookup"><span data-stu-id="dd693-2627">The compound assignment operators are described in [Compound assignment](expressions.md#compound-assignment).</span></span>

<span data-ttu-id="dd693-2628">Os operadores de `+=` e `-=` com uma expressão de acesso de evento como o operando à esquerda são chamados de *operadores de atribuição de eventos*.</span><span class="sxs-lookup"><span data-stu-id="dd693-2628">The `+=` and `-=` operators with an event access expression as the left operand are called the *event assignment operators*.</span></span> <span data-ttu-id="dd693-2629">Nenhum outro operador de atribuição é válido com um acesso de evento como o operando esquerdo.</span><span class="sxs-lookup"><span data-stu-id="dd693-2629">No other assignment operator is valid with an event access as the left operand.</span></span> <span data-ttu-id="dd693-2630">Os operadores de atribuição de eventos são descritos em [atribuição de eventos](expressions.md#event-assignment).</span><span class="sxs-lookup"><span data-stu-id="dd693-2630">The event assignment operators are described in [Event assignment](expressions.md#event-assignment).</span></span>

<span data-ttu-id="dd693-2631">Os operadores de atribuição são associativos à direita, o que significa que as operações são agrupadas da direita para a esquerda.</span><span class="sxs-lookup"><span data-stu-id="dd693-2631">The assignment operators are right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="dd693-2632">Por exemplo, uma expressão do formulário `a = b = c` é avaliada como `a = (b = c)`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2632">For example, an expression of the form `a = b = c` is evaluated as `a = (b = c)`.</span></span>

### <a name="simple-assignment"></a><span data-ttu-id="dd693-2633">Atribuição simples</span><span class="sxs-lookup"><span data-stu-id="dd693-2633">Simple assignment</span></span>

<span data-ttu-id="dd693-2634">O operador `=` é chamado de operador de atribuição simples.</span><span class="sxs-lookup"><span data-stu-id="dd693-2634">The `=` operator is called the simple assignment operator.</span></span>

<span data-ttu-id="dd693-2635">Se o operando esquerdo de uma atribuição simples estiver no formato `E.P` ou `E[Ei]` em que `E` tem o tipo de tempo de compilação `dynamic`, a atribuição será vinculada dinamicamente ([associação dinâmica](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="dd693-2635">If the left operand of a simple assignment is of the form `E.P` or `E[Ei]` where `E` has the compile-time type `dynamic`, then the assignment is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="dd693-2636">Nesse caso, o tipo de tempo de compilação da expressão de atribuição é `dynamic`e a resolução descrita abaixo ocorrerá em tempo de execução com base no tipo de tempo de execução de `E`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2636">In this case the compile-time type of the assignment expression is `dynamic`, and the resolution described below will take place at run-time based on the run-time type of `E`.</span></span>

<span data-ttu-id="dd693-2637">Em uma atribuição simples, o operando à direita deve ser uma expressão que seja conversível implicitamente no tipo do operando esquerdo.</span><span class="sxs-lookup"><span data-stu-id="dd693-2637">In a simple assignment, the right operand must be an expression that is implicitly convertible to the type of the left operand.</span></span> <span data-ttu-id="dd693-2638">A operação atribui o valor do operando direito à variável, à propriedade ou ao elemento do indexador fornecido pelo operando esquerdo.</span><span class="sxs-lookup"><span data-stu-id="dd693-2638">The operation assigns the value of the right operand to the variable, property, or indexer element given by the left operand.</span></span>

<span data-ttu-id="dd693-2639">O resultado de uma expressão de atribuição simples é o valor atribuído ao operando à esquerda.</span><span class="sxs-lookup"><span data-stu-id="dd693-2639">The result of a simple assignment expression is the value assigned to the left operand.</span></span> <span data-ttu-id="dd693-2640">O resultado tem o mesmo tipo que o operando esquerdo e sempre é classificado como um valor.</span><span class="sxs-lookup"><span data-stu-id="dd693-2640">The result has the same type as the left operand and is always classified as a value.</span></span>

<span data-ttu-id="dd693-2641">Se o operando esquerdo for um acesso de propriedade ou indexador, a propriedade ou o indexador deverá ter um acessador `set`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2641">If the left operand is a property or indexer access, the property or indexer must have a `set` accessor.</span></span> <span data-ttu-id="dd693-2642">Se esse não for o caso, ocorrerá um erro de tempo de associação.</span><span class="sxs-lookup"><span data-stu-id="dd693-2642">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="dd693-2643">O processamento em tempo de execução de uma atribuição simples do formulário `x = y` consiste nas seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="dd693-2643">The run-time processing of a simple assignment of the form `x = y` consists of the following steps:</span></span>

*  <span data-ttu-id="dd693-2644">Se `x` for classificado como uma variável:</span><span class="sxs-lookup"><span data-stu-id="dd693-2644">If `x` is classified as a variable:</span></span>
   * <span data-ttu-id="dd693-2645">`x` é avaliado para produzir a variável.</span><span class="sxs-lookup"><span data-stu-id="dd693-2645">`x` is evaluated to produce the variable.</span></span>
   * <span data-ttu-id="dd693-2646">`y` é avaliado e, se necessário, convertido para o tipo de `x` por meio de uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="dd693-2646">`y` is evaluated and, if required, converted to the type of `x` through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
   * <span data-ttu-id="dd693-2647">Se a variável fornecida por `x` for um elemento de matriz de um *reference_type*, uma verificação de tempo de execução será executada para garantir que o valor computado para `y` seja compatível com a instância de matriz da qual `x` é um elemento.</span><span class="sxs-lookup"><span data-stu-id="dd693-2647">If the variable given by `x` is an array element of a *reference_type*, a run-time check is performed to ensure that the value computed for `y` is compatible with the array instance of which `x` is an element.</span></span> <span data-ttu-id="dd693-2648">A verificação terá sucesso se `y` estiver `null`, ou se uma conversão de referência implícita ([conversões de referência implícita](conversions.md#implicit-reference-conversions)) existir do tipo real da instância referenciada por `y` para o tipo de elemento real da instância de matriz que contém `x`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2648">The check succeeds if `y` is `null`, or if an implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from the actual type of the instance referenced by `y` to the actual element type of the array instance containing `x`.</span></span> <span data-ttu-id="dd693-2649">Caso contrário, uma `System.ArrayTypeMismatchException` será gerada.</span><span class="sxs-lookup"><span data-stu-id="dd693-2649">Otherwise, a `System.ArrayTypeMismatchException` is thrown.</span></span>
   * <span data-ttu-id="dd693-2650">O valor resultante da avaliação e da conversão de `y` é armazenado no local fornecido pela avaliação de `x`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2650">The value resulting from the evaluation and conversion of `y` is stored into the location given by the evaluation of `x`.</span></span>
*  <span data-ttu-id="dd693-2651">Se `x` for classificado como um acesso de propriedade ou indexador:</span><span class="sxs-lookup"><span data-stu-id="dd693-2651">If `x` is classified as a property or indexer access:</span></span>
   * <span data-ttu-id="dd693-2652">A expressão de instância (se `x` não for `static`) e a lista de argumentos (se `x` for um acesso de indexador) associada a `x` são avaliadas e os resultados são usados na invocação de acessador de `set` subsequente.</span><span class="sxs-lookup"><span data-stu-id="dd693-2652">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `set` accessor invocation.</span></span>
   * <span data-ttu-id="dd693-2653">`y` é avaliado e, se necessário, convertido para o tipo de `x` por meio de uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="dd693-2653">`y` is evaluated and, if required, converted to the type of `x` through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
   * <span data-ttu-id="dd693-2654">O acessador de `set` de `x` é invocado com o valor calculado para `y` como seu argumento de `value`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2654">The `set` accessor of `x` is invoked with the value computed for `y` as its `value` argument.</span></span>

<span data-ttu-id="dd693-2655">As regras de covariância de matriz ([covariância de matriz](arrays.md#array-covariance)) permitem que um valor de um tipo de matriz `A[]` seja uma referência a uma instância de um tipo de matriz `B[]`, desde que exista uma conversão de referência implícita de `B` para `A`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2655">The array co-variance rules ([Array covariance](arrays.md#array-covariance)) permit a value of an array type `A[]` to be a reference to an instance of an array type `B[]`, provided an implicit reference conversion exists from `B` to `A`.</span></span> <span data-ttu-id="dd693-2656">Devido a essas regras, a atribuição a um elemento de matriz de um *reference_type* requer uma verificação de tempo de execução para garantir que o valor que está sendo atribuído seja compatível com a instância de matriz.</span><span class="sxs-lookup"><span data-stu-id="dd693-2656">Because of these rules, assignment to an array element of a *reference_type* requires a run-time check to ensure that the value being assigned is compatible with the array instance.</span></span> <span data-ttu-id="dd693-2657">No exemplo</span><span class="sxs-lookup"><span data-stu-id="dd693-2657">In the example</span></span>
```csharp
string[] sa = new string[10];
object[] oa = sa;

oa[0] = null;               // Ok
oa[1] = "Hello";            // Ok
oa[2] = new ArrayList();    // ArrayTypeMismatchException
```
<span data-ttu-id="dd693-2658">a última atribuição faz com que uma `System.ArrayTypeMismatchException` seja gerada porque uma instância de `ArrayList` não pode ser armazenada em um elemento de uma `string[]`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2658">the last assignment causes a `System.ArrayTypeMismatchException` to be thrown because an instance of `ArrayList` cannot be stored in an element of a `string[]`.</span></span>

<span data-ttu-id="dd693-2659">Quando uma propriedade ou um indexador declarado em um *struct_type* é o destino de uma atribuição, a expressão de instância associada à propriedade ou ao acesso do indexador deve ser classificada como uma variável.</span><span class="sxs-lookup"><span data-stu-id="dd693-2659">When a property or indexer declared in a *struct_type* is the target of an assignment, the instance expression associated with the property or indexer access must be classified as a variable.</span></span> <span data-ttu-id="dd693-2660">Se a expressão de instância for classificada como um valor, ocorrerá um erro de tempo de associação.</span><span class="sxs-lookup"><span data-stu-id="dd693-2660">If the instance expression is classified as a value, a binding-time error occurs.</span></span> <span data-ttu-id="dd693-2661">Devido ao [acesso de membro](expressions.md#member-access), a mesma regra também se aplica a campos.</span><span class="sxs-lookup"><span data-stu-id="dd693-2661">Because of [Member access](expressions.md#member-access), the same rule also applies to fields.</span></span>

<span data-ttu-id="dd693-2662">Dadas as declarações:</span><span class="sxs-lookup"><span data-stu-id="dd693-2662">Given the declarations:</span></span>
```csharp
struct Point
{
    int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public int X {
        get { return x; }
        set { x = value; }
    }

    public int Y {
        get { return y; }
        set { y = value; }
    }
}

struct Rectangle
{
    Point a, b;

    public Rectangle(Point a, Point b) {
        this.a = a;
        this.b = b;
    }

    public Point A {
        get { return a; }
        set { a = value; }
    }

    public Point B {
        get { return b; }
        set { b = value; }
    }
}
```
<span data-ttu-id="dd693-2663">No exemplo</span><span class="sxs-lookup"><span data-stu-id="dd693-2663">in the example</span></span>
```csharp
Point p = new Point();
p.X = 100;
p.Y = 100;
Rectangle r = new Rectangle();
r.A = new Point(10, 10);
r.B = p;
```
<span data-ttu-id="dd693-2664">as atribuições para `p.X`, `p.Y`, `r.A`e `r.B` são permitidas porque `p` e `r` são variáveis.</span><span class="sxs-lookup"><span data-stu-id="dd693-2664">the assignments to `p.X`, `p.Y`, `r.A`, and `r.B` are permitted because `p` and `r` are variables.</span></span> <span data-ttu-id="dd693-2665">No entanto, no exemplo</span><span class="sxs-lookup"><span data-stu-id="dd693-2665">However, in the example</span></span>
```csharp
Rectangle r = new Rectangle();
r.A.X = 10;
r.A.Y = 10;
r.B.X = 100;
r.B.Y = 100;
```
<span data-ttu-id="dd693-2666">as atribuições são todas inválidas, pois `r.A` e `r.B` não são variáveis.</span><span class="sxs-lookup"><span data-stu-id="dd693-2666">the assignments are all invalid, since `r.A` and `r.B` are not variables.</span></span>

### <a name="compound-assignment"></a><span data-ttu-id="dd693-2667">Atribuição composta</span><span class="sxs-lookup"><span data-stu-id="dd693-2667">Compound assignment</span></span>

<span data-ttu-id="dd693-2668">Se o operando esquerdo de uma atribuição composta estiver no formato `E.P` ou `E[Ei]` em que `E` tem o tipo de tempo de compilação `dynamic`, a atribuição será vinculada dinamicamente ([associação dinâmica](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="dd693-2668">If the left operand of a compound assignment is of the form `E.P` or `E[Ei]` where `E` has the compile-time type `dynamic`, then the assignment is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="dd693-2669">Nesse caso, o tipo de tempo de compilação da expressão de atribuição é `dynamic`e a resolução descrita abaixo ocorrerá em tempo de execução com base no tipo de tempo de execução de `E`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2669">In this case the compile-time type of the assignment expression is `dynamic`, and the resolution described below will take place at run-time based on the run-time type of `E`.</span></span>

<span data-ttu-id="dd693-2670">Uma operação do formulário `x op= y` é processada pela aplicação da resolução de sobrecarga do operador binário ([resolução de sobrecarga do operador binário](expressions.md#binary-operator-overload-resolution)) como se a operação fosse gravada `x op y`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2670">An operation of the form `x op= y` is processed by applying binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) as if the operation was written `x op y`.</span></span> <span data-ttu-id="dd693-2671">Clique</span><span class="sxs-lookup"><span data-stu-id="dd693-2671">Then,</span></span>

*  <span data-ttu-id="dd693-2672">Se o tipo de retorno do operador selecionado for implicitamente conversível para o tipo de `x`, a operação será avaliada como `x = x op y`, exceto que `x` é avaliada apenas uma vez.</span><span class="sxs-lookup"><span data-stu-id="dd693-2672">If the return type of the selected operator is implicitly convertible to the type of `x`, the operation is evaluated as `x = x op y`, except that `x` is evaluated only once.</span></span>
*  <span data-ttu-id="dd693-2673">Caso contrário, se o operador selecionado for um operador predefinido, se o tipo de retorno do operador selecionado for explicitamente conversível para o tipo de `x`, e se `y` for implicitamente conversível no tipo de `x` ou se o operador for um operador de deslocamento, a operação será avaliada como `x = (T)(x op y)`, em que `T` é o tipo de `x`, exceto que `x` é avaliado apenas uma vez.</span><span class="sxs-lookup"><span data-stu-id="dd693-2673">Otherwise, if the selected operator is a predefined operator, if the return type of the selected operator is explicitly convertible to the type of `x`, and if `y` is implicitly convertible to the type of `x` or the operator is a shift operator, then the operation is evaluated as `x = (T)(x op y)`, where `T` is the type of `x`, except that `x` is evaluated only once.</span></span>
*  <span data-ttu-id="dd693-2674">Caso contrário, a atribuição composta é inválida e ocorre um erro de tempo de ligação.</span><span class="sxs-lookup"><span data-stu-id="dd693-2674">Otherwise, the compound assignment is invalid, and a binding-time error occurs.</span></span>

<span data-ttu-id="dd693-2675">O termo "avaliado apenas uma vez" significa que, na avaliação de `x op y`, os resultados de quaisquer expressões constituintes de `x` são salvos temporariamente e, em seguida, reutilizados ao executar a atribuição para `x`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2675">The term "evaluated only once" means that in the evaluation of `x op y`, the results of any constituent expressions of `x` are temporarily saved and then reused when performing the assignment to `x`.</span></span> <span data-ttu-id="dd693-2676">Por exemplo, no `A()[B()] += C()`de atribuição, em que `A` é um método que retorna `int[]`e `B` e `C` são métodos que retornam `int`, os métodos são invocados apenas uma vez, na ordem `A`, `B`, `C`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2676">For example, in the assignment `A()[B()] += C()`, where `A` is a method returning `int[]`, and `B` and `C` are methods returning `int`, the methods are invoked only once, in the order `A`, `B`, `C`.</span></span>

<span data-ttu-id="dd693-2677">Quando o operando esquerdo de uma atribuição composta é um acesso de propriedade ou indexador, a propriedade ou o indexador deve ter um acessador `get` e um acessador `set`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2677">When the left operand of a compound assignment is a property access or indexer access, the property or indexer must have both a `get` accessor and a `set` accessor.</span></span> <span data-ttu-id="dd693-2678">Se esse não for o caso, ocorrerá um erro de tempo de associação.</span><span class="sxs-lookup"><span data-stu-id="dd693-2678">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="dd693-2679">A segunda regra acima permite que `x op= y` seja avaliada como `x = (T)(x op y)` em determinados contextos.</span><span class="sxs-lookup"><span data-stu-id="dd693-2679">The second rule above permits `x op= y` to be evaluated as `x = (T)(x op y)` in certain contexts.</span></span> <span data-ttu-id="dd693-2680">A regra existe de modo que os operadores predefinidos possam ser usados como operadores compostos quando o operando esquerdo for do tipo `sbyte`, `byte`, `short`, `ushort`ou `char`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2680">The rule exists such that the predefined operators can be used as compound operators when the left operand is of type `sbyte`, `byte`, `short`, `ushort`, or `char`.</span></span> <span data-ttu-id="dd693-2681">Mesmo quando ambos os argumentos são de um desses tipos, os operadores predefinidos produzem um resultado do tipo `int`, conforme descrito em [promoções numéricas binárias](expressions.md#binary-numeric-promotions).</span><span class="sxs-lookup"><span data-stu-id="dd693-2681">Even when both arguments are of one of those types, the predefined operators produce a result of type `int`, as described in [Binary numeric promotions](expressions.md#binary-numeric-promotions).</span></span> <span data-ttu-id="dd693-2682">Portanto, sem uma conversão, não seria possível atribuir o resultado ao operando esquerdo.</span><span class="sxs-lookup"><span data-stu-id="dd693-2682">Thus, without a cast it would not be possible to assign the result to the left operand.</span></span>

<span data-ttu-id="dd693-2683">O efeito intuitivo da regra para operadores predefinidos é simplesmente que `x op= y` é permitido se ambos os `x op y` e `x = y` são permitidos.</span><span class="sxs-lookup"><span data-stu-id="dd693-2683">The intuitive effect of the rule for predefined operators is simply that `x op= y` is permitted if both of `x op y` and `x = y` are permitted.</span></span> <span data-ttu-id="dd693-2684">No exemplo</span><span class="sxs-lookup"><span data-stu-id="dd693-2684">In the example</span></span>
```csharp
byte b = 0;
char ch = '\0';
int i = 0;

b += 1;             // Ok
b += 1000;          // Error, b = 1000 not permitted
b += i;             // Error, b = i not permitted
b += (byte)i;       // Ok

ch += 1;            // Error, ch = 1 not permitted
ch += (char)1;      // Ok
```
<span data-ttu-id="dd693-2685">o motivo intuitivo de cada erro é que uma atribuição simples correspondente também teria sido um erro.</span><span class="sxs-lookup"><span data-stu-id="dd693-2685">the intuitive reason for each error is that a corresponding simple assignment would also have been an error.</span></span>

<span data-ttu-id="dd693-2686">Isso também significa que as operações de atribuição compostas dão suporte a operações levantadas.</span><span class="sxs-lookup"><span data-stu-id="dd693-2686">This also means that compound assignment operations support lifted operations.</span></span> <span data-ttu-id="dd693-2687">No exemplo</span><span class="sxs-lookup"><span data-stu-id="dd693-2687">In the example</span></span>
```csharp
int? i = 0;
i += 1;             // Ok
```
<span data-ttu-id="dd693-2688">o operador levantado `+(int?,int?)` é usado.</span><span class="sxs-lookup"><span data-stu-id="dd693-2688">the lifted operator `+(int?,int?)` is used.</span></span>

### <a name="event-assignment"></a><span data-ttu-id="dd693-2689">Atribuição de evento</span><span class="sxs-lookup"><span data-stu-id="dd693-2689">Event assignment</span></span>

<span data-ttu-id="dd693-2690">Se o operando esquerdo de um operador de `+=` ou `-=` for classificado como um acesso de evento, a expressão será avaliada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="dd693-2690">If the left operand of a `+=` or `-=` operator is classified as an event access, then the expression is evaluated as follows:</span></span>

*  <span data-ttu-id="dd693-2691">A expressão de instância, se houver, do acesso ao evento é avaliada.</span><span class="sxs-lookup"><span data-stu-id="dd693-2691">The instance expression, if any, of the event access is evaluated.</span></span>
*  <span data-ttu-id="dd693-2692">O operando à direita do operador `+=` ou `-=` é avaliado e, se necessário, é convertido para o tipo do operando esquerdo por meio de uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="dd693-2692">The right operand of the `+=` or `-=` operator is evaluated, and, if required, converted to the type of the left operand through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
*  <span data-ttu-id="dd693-2693">Um acessador de eventos do evento é invocado, com a lista de argumentos que consiste no operando direito, após a avaliação e, se necessário, a conversão.</span><span class="sxs-lookup"><span data-stu-id="dd693-2693">An event accessor of the event is invoked, with argument list consisting of the right operand, after evaluation and, if necessary, conversion.</span></span> <span data-ttu-id="dd693-2694">Se o operador foi `+=`, o acessador de `add` é invocado; Se o operador tiver sido `-=`, o acessador de `remove` será invocado.</span><span class="sxs-lookup"><span data-stu-id="dd693-2694">If the operator was `+=`, the `add` accessor is invoked; if the operator was `-=`, the `remove` accessor is invoked.</span></span>

<span data-ttu-id="dd693-2695">Uma expressão de atribuição de evento não produz um valor.</span><span class="sxs-lookup"><span data-stu-id="dd693-2695">An event assignment expression does not yield a value.</span></span> <span data-ttu-id="dd693-2696">Assim, uma expressão de atribuição de evento é válida somente no contexto de uma *statement_expression* ([instruções de expressão](statements.md#expression-statements)).</span><span class="sxs-lookup"><span data-stu-id="dd693-2696">Thus, an event assignment expression is valid only in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>

## <a name="expression"></a><span data-ttu-id="dd693-2697">Expressão</span><span class="sxs-lookup"><span data-stu-id="dd693-2697">Expression</span></span>

<span data-ttu-id="dd693-2698">Uma *expressão* é uma *non_assignment_expression* ou uma *atribuição*.</span><span class="sxs-lookup"><span data-stu-id="dd693-2698">An *expression* is either a *non_assignment_expression* or an *assignment*.</span></span>

```antlr
expression
    : non_assignment_expression
    | assignment
    ;

non_assignment_expression
    : conditional_expression
    | lambda_expression
    | query_expression
    ;
```

## <a name="constant-expressions"></a><span data-ttu-id="dd693-2699">Expressões constantes</span><span class="sxs-lookup"><span data-stu-id="dd693-2699">Constant expressions</span></span>

<span data-ttu-id="dd693-2700">Uma *constant_expression* é uma expressão que pode ser totalmente avaliada em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-2700">A *constant_expression* is an expression that can be fully evaluated at compile-time.</span></span>

```antlr
constant_expression
    : expression
    ;
```

<span data-ttu-id="dd693-2701">Uma expressão constante deve ser o `null` literal ou um valor com um dos seguintes tipos: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `object`, `string`ou qualquer tipo de enumeração.</span><span class="sxs-lookup"><span data-stu-id="dd693-2701">A constant expression must be the `null` literal or a value with one of  the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `object`, `string`, or any enumeration type.</span></span> <span data-ttu-id="dd693-2702">Somente as seguintes construções são permitidas em expressões constantes:</span><span class="sxs-lookup"><span data-stu-id="dd693-2702">Only the following constructs are permitted in constant expressions:</span></span>

*  <span data-ttu-id="dd693-2703">Literais (incluindo o `null` literal).</span><span class="sxs-lookup"><span data-stu-id="dd693-2703">Literals (including the `null` literal).</span></span>
*  <span data-ttu-id="dd693-2704">Referências a `const` membros de tipos de classe e struct.</span><span class="sxs-lookup"><span data-stu-id="dd693-2704">References to `const` members of class and struct types.</span></span>
*  <span data-ttu-id="dd693-2705">Referências a membros de tipos de enumeração.</span><span class="sxs-lookup"><span data-stu-id="dd693-2705">References to members of enumeration types.</span></span>
*  <span data-ttu-id="dd693-2706">Referências a parâmetros de `const` ou variáveis locais</span><span class="sxs-lookup"><span data-stu-id="dd693-2706">References to `const` parameters or local variables</span></span>
*  <span data-ttu-id="dd693-2707">Subexpressão entre parênteses, que são expressões constantes.</span><span class="sxs-lookup"><span data-stu-id="dd693-2707">Parenthesized sub-expressions, which are themselves constant expressions.</span></span>
*  <span data-ttu-id="dd693-2708">Expressões de conversão, desde que o tipo de destino seja um dos tipos listados acima.</span><span class="sxs-lookup"><span data-stu-id="dd693-2708">Cast expressions, provided the target type is one of the types listed above.</span></span>
*  <span data-ttu-id="dd693-2709">expressões de `checked` e `unchecked`</span><span class="sxs-lookup"><span data-stu-id="dd693-2709">`checked` and `unchecked` expressions</span></span>
*  <span data-ttu-id="dd693-2710">Expressões de valor padrão</span><span class="sxs-lookup"><span data-stu-id="dd693-2710">Default value expressions</span></span>
*  <span data-ttu-id="dd693-2711">Expressões nameof</span><span class="sxs-lookup"><span data-stu-id="dd693-2711">Nameof expressions</span></span>
*  <span data-ttu-id="dd693-2712">Os operadores `+`, `-`, `!`e `~` unários predefinidos.</span><span class="sxs-lookup"><span data-stu-id="dd693-2712">The predefined `+`, `-`, `!`, and `~` unary operators.</span></span>
*  <span data-ttu-id="dd693-2713">Os operadores predefinidos `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `|`, `^`, `&&`, `||`, `==`, `!=`, `<`, `>`, `<=`e `>=` binários, desde que cada operando seja de um tipo listado acima.</span><span class="sxs-lookup"><span data-stu-id="dd693-2713">The predefined `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `|`, `^`, `&&`, `||`, `==`, `!=`, `<`, `>`, `<=`, and `>=` binary operators, provided each operand is of a type listed above.</span></span>
*  <span data-ttu-id="dd693-2714">O operador condicional `?:`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2714">The `?:` conditional operator.</span></span>

<span data-ttu-id="dd693-2715">As conversões a seguir são permitidas em expressões constantes:</span><span class="sxs-lookup"><span data-stu-id="dd693-2715">The following conversions are permitted in constant expressions:</span></span>

*  <span data-ttu-id="dd693-2716">Conversões de identidade</span><span class="sxs-lookup"><span data-stu-id="dd693-2716">Identity conversions</span></span>
*  <span data-ttu-id="dd693-2717">Conversões numéricas</span><span class="sxs-lookup"><span data-stu-id="dd693-2717">Numeric conversions</span></span>
*  <span data-ttu-id="dd693-2718">Conversões de enumeração</span><span class="sxs-lookup"><span data-stu-id="dd693-2718">Enumeration conversions</span></span>
*  <span data-ttu-id="dd693-2719">Conversões de expressão de constante</span><span class="sxs-lookup"><span data-stu-id="dd693-2719">Constant expression conversions</span></span>
*  <span data-ttu-id="dd693-2720">Conversões de referência implícitas e explícitas, desde que a origem das conversões seja uma expressão constante que é avaliada como o valor NULL.</span><span class="sxs-lookup"><span data-stu-id="dd693-2720">Implicit and explicit reference conversions, provided that the source of the conversions is a constant expression that evaluates to the null value.</span></span>

<span data-ttu-id="dd693-2721">Outras conversões incluindo boxing, unboxing e conversões de referência implícita de valores não nulos não são permitidas em expressões constantes.</span><span class="sxs-lookup"><span data-stu-id="dd693-2721">Other conversions including boxing, unboxing and implicit reference conversions of non-null values are not permitted in constant expressions.</span></span> <span data-ttu-id="dd693-2722">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="dd693-2722">For example:</span></span>
```csharp
class C {
    const object i = 5;         // error: boxing conversion not permitted
    const object str = "hello"; // error: implicit reference conversion
}
```
<span data-ttu-id="dd693-2723">a inicialização de i é um erro porque uma conversão boxing é necessária.</span><span class="sxs-lookup"><span data-stu-id="dd693-2723">the initialization of i is an error because a boxing conversion is required.</span></span> <span data-ttu-id="dd693-2724">A inicialização de str é um erro porque uma conversão de referência implícita de um valor não nulo é necessária.</span><span class="sxs-lookup"><span data-stu-id="dd693-2724">The initialization of str is an error because an implicit reference conversion from a non-null value is required.</span></span>

<span data-ttu-id="dd693-2725">Sempre que uma expressão atende aos requisitos listados acima, a expressão é avaliada em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-2725">Whenever an expression fulfills the requirements listed above, the expression is evaluated at compile-time.</span></span> <span data-ttu-id="dd693-2726">Isso é verdadeiro mesmo que a expressão seja uma subexpressão de uma expressão maior que contenha construções não constantes.</span><span class="sxs-lookup"><span data-stu-id="dd693-2726">This is true even if the expression is a sub-expression of a larger expression that contains non-constant constructs.</span></span>

<span data-ttu-id="dd693-2727">A avaliação de tempo de compilação de expressões constantes usa as mesmas regras que a avaliação de tempo de execução de expressões não constantes, exceto que, quando a avaliação de tempo de execução tiver gerado uma exceção, a avaliação de tempo de compilação causará a ocorrência de um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-2727">The compile-time evaluation of constant expressions uses the same rules as run-time evaluation of non-constant expressions, except that where run-time evaluation would have thrown an exception, compile-time evaluation causes a compile-time error to occur.</span></span>

<span data-ttu-id="dd693-2728">A menos que uma expressão constante seja colocada explicitamente em um contexto de `unchecked`, os estouros que ocorrem em operações aritméticas de tipo integral e conversões durante a avaliação de tempo de compilação da expressão sempre causam erros de tempo de compilação ([expressões constantes](expressions.md#constant-expressions)).</span><span class="sxs-lookup"><span data-stu-id="dd693-2728">Unless a constant expression is explicitly placed in an `unchecked` context, overflows that occur in integral-type arithmetic operations and conversions during the compile-time evaluation of the expression always cause compile-time errors ([Constant expressions](expressions.md#constant-expressions)).</span></span>

<span data-ttu-id="dd693-2729">Expressões constantes ocorrem nos contextos listados abaixo.</span><span class="sxs-lookup"><span data-stu-id="dd693-2729">Constant expressions occur in the contexts listed below.</span></span> <span data-ttu-id="dd693-2730">Nesses contextos, ocorrerá um erro de tempo de compilação se uma expressão não puder ser totalmente avaliada em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="dd693-2730">In these contexts, a compile-time error occurs if an expression cannot be fully evaluated at compile-time.</span></span>

*  <span data-ttu-id="dd693-2731">Declarações de constantes ([constantes](classes.md#constants)).</span><span class="sxs-lookup"><span data-stu-id="dd693-2731">Constant declarations ([Constants](classes.md#constants)).</span></span>
*  <span data-ttu-id="dd693-2732">Declarações de membro de enumeração ([membros enum](enums.md#enum-members)).</span><span class="sxs-lookup"><span data-stu-id="dd693-2732">Enumeration member declarations ([Enum members](enums.md#enum-members)).</span></span>
*  <span data-ttu-id="dd693-2733">Argumentos padrão de listas de parâmetros formais ([parâmetros de método](classes.md#method-parameters))</span><span class="sxs-lookup"><span data-stu-id="dd693-2733">Default arguments of formal parameter lists ([Method parameters](classes.md#method-parameters))</span></span>
*  <span data-ttu-id="dd693-2734">`case` rótulos de uma instrução `switch` ([a instrução switch](statements.md#the-switch-statement)).</span><span class="sxs-lookup"><span data-stu-id="dd693-2734">`case` labels of a `switch` statement ([The switch statement](statements.md#the-switch-statement)).</span></span>
*  <span data-ttu-id="dd693-2735">instruções de `goto case` ([a instrução goto](statements.md#the-goto-statement)).</span><span class="sxs-lookup"><span data-stu-id="dd693-2735">`goto case` statements ([The goto statement](statements.md#the-goto-statement)).</span></span>
*  <span data-ttu-id="dd693-2736">Comprimentos de dimensão em uma expressão de criação de matriz ([expressões de criação de matriz](expressions.md#array-creation-expressions)) que inclui um inicializador.</span><span class="sxs-lookup"><span data-stu-id="dd693-2736">Dimension lengths in an array creation expression ([Array creation expressions](expressions.md#array-creation-expressions)) that includes an initializer.</span></span>
*  <span data-ttu-id="dd693-2737">Atributos ([atributos](attributes.md)).</span><span class="sxs-lookup"><span data-stu-id="dd693-2737">Attributes ([Attributes](attributes.md)).</span></span>

<span data-ttu-id="dd693-2738">Uma conversão de expressão constante implícita ([conversões de expressão de constante implícita](conversions.md#implicit-constant-expression-conversions)) permite que uma expressão constante do tipo `int` seja convertida em `sbyte`, `byte`, `short`, `ushort`, `uint`ou `ulong`, desde que o valor da expressão constante esteja dentro do intervalo do tipo de destino.</span><span class="sxs-lookup"><span data-stu-id="dd693-2738">An implicit constant expression conversion ([Implicit constant expression conversions](conversions.md#implicit-constant-expression-conversions)) permits a constant expression of type `int` to be converted to `sbyte`, `byte`, `short`, `ushort`, `uint`, or `ulong`, provided the value of the constant expression is within the range of the destination type.</span></span>

## <a name="boolean-expressions"></a><span data-ttu-id="dd693-2739">expressões Boolianas</span><span class="sxs-lookup"><span data-stu-id="dd693-2739">Boolean expressions</span></span>

<span data-ttu-id="dd693-2740">Uma *Boolean_expression* é uma expressão que produz um resultado do tipo `bool`; diretamente ou por meio da aplicação de `operator true` em determinados contextos, conforme especificado no seguinte.</span><span class="sxs-lookup"><span data-stu-id="dd693-2740">A *boolean_expression* is an expression that yields a result of type `bool`; either directly or through application of `operator true` in certain contexts as specified in the following.</span></span>

```antlr
boolean_expression
    : expression
    ;
```

<span data-ttu-id="dd693-2741">A expressão condicional de controle de *um if_statement* ([a instrução if](statements.md#the-if-statement)), *while_statement* ([a instrução while](statements.md#the-while-statement)) *, do_statement* ([a instrução](statements.md#the-do-statement)do) ou *for_statement* ([a instrução for](statements.md#the-for-statement)) é uma *Boolean_expression*.</span><span class="sxs-lookup"><span data-stu-id="dd693-2741">The controlling conditional expression of an *if_statement* ([The if statement](statements.md#the-if-statement)), *while_statement* ([The while statement](statements.md#the-while-statement)), *do_statement* ([The do statement](statements.md#the-do-statement)), or *for_statement* ([The for statement](statements.md#the-for-statement)) is a *boolean_expression*.</span></span> <span data-ttu-id="dd693-2742">A expressão condicional de controle do operador de `?:` ([operador condicional](expressions.md#conditional-operator)) segue as mesmas regras que uma *Boolean_expression*, mas, por motivos de precedência de operador, é classificada como uma *conditional_or_expression*.</span><span class="sxs-lookup"><span data-stu-id="dd693-2742">The controlling conditional expression of the `?:` operator ([Conditional operator](expressions.md#conditional-operator)) follows the same rules as a *boolean_expression*, but for reasons of operator precedence is classified as a *conditional_or_expression*.</span></span>

<span data-ttu-id="dd693-2743">Um `E` de *Boolean_expression* é necessário para poder produzir um valor do tipo `bool`, da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="dd693-2743">A *boolean_expression* `E` is required to be able to produce a value of type `bool`, as follows:</span></span>

*  <span data-ttu-id="dd693-2744">Se `E` for implicitamente conversível para `bool`, em tempo de execução, a conversão implícita será aplicada.</span><span class="sxs-lookup"><span data-stu-id="dd693-2744">If `E` is implicitly convertible to `bool` then at runtime that implicit conversion is applied.</span></span>
*  <span data-ttu-id="dd693-2745">Caso contrário, a resolução de sobrecarga de operador unário ([resolução de sobrecarga de operador unário](expressions.md#unary-operator-overload-resolution)) é usada para encontrar uma melhor implementação exclusiva do operador `true` em `E`, e essa implementação é aplicada em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="dd693-2745">Otherwise, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is used to find a unique best implementation of operator `true` on `E`, and that implementation is applied at runtime.</span></span>
*  <span data-ttu-id="dd693-2746">Se esse operador não for encontrado, ocorrerá um erro de tempo de associação.</span><span class="sxs-lookup"><span data-stu-id="dd693-2746">If no such operator is found, a binding-time error occurs.</span></span>

<span data-ttu-id="dd693-2747">O tipo de struct `DBBool` no [tipo de banco de dados booliano](structs.md#database-boolean-type) fornece um exemplo de um tipo que implementa `operator true` e `operator false`.</span><span class="sxs-lookup"><span data-stu-id="dd693-2747">The `DBBool` struct type in [Database boolean type](structs.md#database-boolean-type) provides an example of a type that implements `operator true` and `operator false`.</span></span>
