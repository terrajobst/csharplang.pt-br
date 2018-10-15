# <a name="expressions"></a><span data-ttu-id="70e32-101">Expressões</span><span class="sxs-lookup"><span data-stu-id="70e32-101">Expressions</span></span>

<span data-ttu-id="70e32-102">Uma expressão é uma sequência de operadores e operandos.</span><span class="sxs-lookup"><span data-stu-id="70e32-102">An expression is a sequence of operators and operands.</span></span> <span data-ttu-id="70e32-103">Este capítulo define a sintaxe, a ordem de avaliação dos operandos e operadores e o significado das expressões.</span><span class="sxs-lookup"><span data-stu-id="70e32-103">This chapter defines the syntax, order of evaluation of operands and operators, and meaning of expressions.</span></span>

## <a name="expression-classifications"></a><span data-ttu-id="70e32-104">Classificações de expressão</span><span class="sxs-lookup"><span data-stu-id="70e32-104">Expression classifications</span></span>

<span data-ttu-id="70e32-105">Uma expressão é classificada como um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="70e32-105">An expression is classified as one of the following:</span></span>

*  <span data-ttu-id="70e32-106">Um valor.</span><span class="sxs-lookup"><span data-stu-id="70e32-106">A value.</span></span> <span data-ttu-id="70e32-107">Cada valor tem um tipo associado.</span><span class="sxs-lookup"><span data-stu-id="70e32-107">Every value has an associated type.</span></span>
*  <span data-ttu-id="70e32-108">Uma variável.</span><span class="sxs-lookup"><span data-stu-id="70e32-108">A variable.</span></span> <span data-ttu-id="70e32-109">Cada variável tem um tipo associado, ou seja, o tipo declarado da variável.</span><span class="sxs-lookup"><span data-stu-id="70e32-109">Every variable has an associated type, namely the declared type of the variable.</span></span>
*  <span data-ttu-id="70e32-110">Um namespace.</span><span class="sxs-lookup"><span data-stu-id="70e32-110">A namespace.</span></span> <span data-ttu-id="70e32-111">Uma expressão com essa classificação só pode aparecer como o lado esquerdo de uma *member_access* ([acesso de membro](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="70e32-111">An expression with this classification can only appear as the left hand side of a *member_access* ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="70e32-112">Em qualquer outro contexto, uma expressão classificada como um namespace causa um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-112">In any other context, an expression classified as a namespace causes a compile-time error.</span></span>
*  <span data-ttu-id="70e32-113">Um tipo.</span><span class="sxs-lookup"><span data-stu-id="70e32-113">A type.</span></span> <span data-ttu-id="70e32-114">Uma expressão com essa classificação só pode aparecer como o lado esquerdo de uma *member_access* ([acesso de membro](expressions.md#member-access)), ou como um operando para o `as` operador ([o operador ](expressions.md#the-as-operator)), o `is` operador ([o operador is](expressions.md#the-is-operator)), ou o `typeof` operador ([o operador typeof](expressions.md#the-typeof-operator)).</span><span class="sxs-lookup"><span data-stu-id="70e32-114">An expression with this classification can only appear as the left hand side of a *member_access* ([Member access](expressions.md#member-access)), or as an operand for the `as` operator ([The as operator](expressions.md#the-as-operator)), the `is` operator ([The is operator](expressions.md#the-is-operator)), or the `typeof` operator ([The typeof operator](expressions.md#the-typeof-operator)).</span></span> <span data-ttu-id="70e32-115">Em qualquer outro contexto, uma classificado como um tipo de expressão causa um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-115">In any other context, an expression classified as a type causes a compile-time error.</span></span>
*  <span data-ttu-id="70e32-116">Um grupo de método, que é um conjunto de métodos sobrecarregados resultante de uma pesquisa de membro ([pesquisa de membro](expressions.md#member-lookup)).</span><span class="sxs-lookup"><span data-stu-id="70e32-116">A method group, which is a set of overloaded methods resulting from a member lookup ([Member lookup](expressions.md#member-lookup)).</span></span> <span data-ttu-id="70e32-117">Um grupo de método pode ter uma expressão de instância associada e uma lista de argumentos de tipo associado.</span><span class="sxs-lookup"><span data-stu-id="70e32-117">A method group may have an associated instance expression and an associated type argument list.</span></span> <span data-ttu-id="70e32-118">Quando um método de instância é invocado, o resultado da avaliação da expressão de instância torna-se a instância representada pelo `this` ([esse acesso](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="70e32-118">When an instance method is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)).</span></span> <span data-ttu-id="70e32-119">Um grupo de métodos é permitido em uma *invocation_expression* ([expressões de invocação](expressions.md#invocation-expressions)), uma *delegate_creation_expression* ([delegar a criação expressões](expressions.md#delegate-creation-expressions)) e como o lado esquerdo de um é o operador e pode ser convertido implicitamente em um tipo de delegado compatível ([conversões de grupo de método](conversions.md#method-group-conversions)).</span><span class="sxs-lookup"><span data-stu-id="70e32-119">A method group is permitted in an *invocation_expression* ([Invocation expressions](expressions.md#invocation-expressions)) , a *delegate_creation_expression* ([Delegate creation expressions](expressions.md#delegate-creation-expressions)) and as the left hand side of an is operator, and can be implicitly converted to a compatible delegate type ([Method group conversions](conversions.md#method-group-conversions)).</span></span> <span data-ttu-id="70e32-120">Em qualquer outro contexto, uma expressão classificada como um grupo de métodos causa um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-120">In any other context, an expression classified as a method group causes a compile-time error.</span></span>
*  <span data-ttu-id="70e32-121">Um literal nulo.</span><span class="sxs-lookup"><span data-stu-id="70e32-121">A null literal.</span></span> <span data-ttu-id="70e32-122">Uma expressão com essa classificação pode ser convertida implicitamente para um tipo de referência ou tipo anulável.</span><span class="sxs-lookup"><span data-stu-id="70e32-122">An expression with this classification can be implicitly converted to a reference type or nullable type.</span></span>
*  <span data-ttu-id="70e32-123">Uma função anônima.</span><span class="sxs-lookup"><span data-stu-id="70e32-123">An anonymous function.</span></span> <span data-ttu-id="70e32-124">Uma expressão com essa classificação pode ser implicitamente convertida para um tipo de delegado compatível ou tipo de árvore de expressão.</span><span class="sxs-lookup"><span data-stu-id="70e32-124">An expression with this classification can be implicitly converted to a compatible delegate type or expression tree type.</span></span>
*  <span data-ttu-id="70e32-125">Um acesso de propriedade.</span><span class="sxs-lookup"><span data-stu-id="70e32-125">A property access.</span></span> <span data-ttu-id="70e32-126">Todo acesso de propriedade tem um tipo associado, ou seja, o tipo da propriedade.</span><span class="sxs-lookup"><span data-stu-id="70e32-126">Every property access has an associated type, namely the type of the property.</span></span> <span data-ttu-id="70e32-127">Além disso, um acesso de propriedade pode ter uma expressão de instância associada.</span><span class="sxs-lookup"><span data-stu-id="70e32-127">Furthermore, a property access may have an associated instance expression.</span></span> <span data-ttu-id="70e32-128">Quando um acessador (o `get` ou `set` bloco) de uma instância de acesso de propriedade é invocado, o resultado da avaliação da expressão de instância torna-se a instância representada pelo `this` ([esse acesso](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="70e32-128">When an accessor (the `get` or `set` block) of an instance property access is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)).</span></span>
*  <span data-ttu-id="70e32-129">Acesso de um evento.</span><span class="sxs-lookup"><span data-stu-id="70e32-129">An event access.</span></span> <span data-ttu-id="70e32-130">Acesso de cada evento tem um tipo associado, ou seja, o tipo do evento.</span><span class="sxs-lookup"><span data-stu-id="70e32-130">Every event access has an associated type, namely the type of the event.</span></span> <span data-ttu-id="70e32-131">Além disso, o acesso de um evento pode ter uma expressão de instância associada.</span><span class="sxs-lookup"><span data-stu-id="70e32-131">Furthermore, an event access may have an associated instance expression.</span></span> <span data-ttu-id="70e32-132">Um acesso de evento pode aparecer como o operando à esquerda do `+=` e `-=` operadores ([atribuição de evento](expressions.md#event-assignment)).</span><span class="sxs-lookup"><span data-stu-id="70e32-132">An event access may appear as the left hand operand of the `+=` and `-=` operators ([Event assignment](expressions.md#event-assignment)).</span></span> <span data-ttu-id="70e32-133">Em qualquer outro contexto, uma expressão classificada como um acesso de evento causa um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-133">In any other context, an expression classified as an event access causes a compile-time error.</span></span>
*  <span data-ttu-id="70e32-134">Acesso de um indexador.</span><span class="sxs-lookup"><span data-stu-id="70e32-134">An indexer access.</span></span> <span data-ttu-id="70e32-135">Cada acesso do indexador tem um tipo associado, ou seja, o tipo de elemento do indexador.</span><span class="sxs-lookup"><span data-stu-id="70e32-135">Every indexer access has an associated type, namely the element type of the indexer.</span></span> <span data-ttu-id="70e32-136">Além disso, o acesso de um indexador tem uma expressão de instância associada e uma lista de argumentos associados.</span><span class="sxs-lookup"><span data-stu-id="70e32-136">Furthermore, an indexer access has an associated instance expression and an associated argument list.</span></span> <span data-ttu-id="70e32-137">Quando um acessador (o `get` ou `set` bloco) de um indexador acesso é invocado, o resultado da avaliação da expressão de instância torna-se a instância representada pelo `this` ([esse acesso](expressions.md#this-access)) e o resultado de Avaliando a lista de argumentos torna-se a lista de parâmetros de invocação.</span><span class="sxs-lookup"><span data-stu-id="70e32-137">When an accessor (the `get` or `set` block) of an indexer access is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)), and the result of evaluating the argument list becomes the parameter list of the invocation.</span></span>
*  <span data-ttu-id="70e32-138">nada.</span><span class="sxs-lookup"><span data-stu-id="70e32-138">Nothing.</span></span> <span data-ttu-id="70e32-139">Isso ocorre quando a expressão é uma invocação de um método com um tipo de retorno `void`.</span><span class="sxs-lookup"><span data-stu-id="70e32-139">This occurs when the expression is an invocation of a method with a return type of `void`.</span></span> <span data-ttu-id="70e32-140">Uma expressão classificada como nada só é válido no contexto de um *statement_expression* ([instruções de expressão](statements.md#expression-statements)).</span><span class="sxs-lookup"><span data-stu-id="70e32-140">An expression classified as nothing is only valid in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>

<span data-ttu-id="70e32-141">O resultado final de uma expressão nunca é um namespace, tipo, o grupo de método ou acesso ao evento.</span><span class="sxs-lookup"><span data-stu-id="70e32-141">The final result of an expression is never a namespace, type, method group, or event access.</span></span> <span data-ttu-id="70e32-142">Em vez disso, conforme observado acima, essas categorias de expressões são construções intermediárias que são permitidas somente em determinados contextos.</span><span class="sxs-lookup"><span data-stu-id="70e32-142">Rather, as noted above, these categories of expressions are intermediate constructs that are only permitted in certain contexts.</span></span>

<span data-ttu-id="70e32-143">Um acesso de propriedade ou o acesso do indexador é sempre reclassificado como um valor pela execução de uma invocação do *acessador get* ou o *acessador set*.</span><span class="sxs-lookup"><span data-stu-id="70e32-143">A property access or indexer access is always reclassified as a value by performing an invocation of the *get accessor* or the *set accessor*.</span></span> <span data-ttu-id="70e32-144">O acessador particular é determinado pelo contexto de acesso de propriedade ou indexador: se o acesso é o destino de uma atribuição, o *acessador set* é chamado para atribuir um novo valor ([atribuição simples](expressions.md#simple-assignment)) .</span><span class="sxs-lookup"><span data-stu-id="70e32-144">The particular accessor is determined by the context of the property or indexer access: If the access is the target of an assignment, the *set accessor* is invoked to assign a new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="70e32-145">Caso contrário, o *acessador get* é chamado para obter o valor atual ([valores das expressões](expressions.md#values-of-expressions)).</span><span class="sxs-lookup"><span data-stu-id="70e32-145">Otherwise, the *get accessor* is invoked to obtain the current value ([Values of expressions](expressions.md#values-of-expressions)).</span></span>

### <a name="values-of-expressions"></a><span data-ttu-id="70e32-146">Valores de expressões</span><span class="sxs-lookup"><span data-stu-id="70e32-146">Values of expressions</span></span>

<span data-ttu-id="70e32-147">A maioria das construções que envolvem uma expressão, por fim, exige que a expressão para denotar uma ***valor***.</span><span class="sxs-lookup"><span data-stu-id="70e32-147">Most of the constructs that involve an expression ultimately require the expression to denote a ***value***.</span></span> <span data-ttu-id="70e32-148">Nesses casos, se a expressão real denota um namespace, um tipo, um grupo de métodos ou nada, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-148">In such cases, if the actual expression denotes a namespace, a type, a method group, or nothing, a compile-time error occurs.</span></span> <span data-ttu-id="70e32-149">No entanto, se a expressão denota um acesso de propriedade, um acesso de indexador ou uma variável, o valor da propriedade, indexador ou variável implicitamente é substituído:</span><span class="sxs-lookup"><span data-stu-id="70e32-149">However, if the expression denotes a property access, an indexer access, or a variable, the value of the property, indexer, or variable is implicitly substituted:</span></span>

*  <span data-ttu-id="70e32-150">O valor de uma variável é simplesmente o valor armazenado atualmente no local de armazenamento identificado pela variável.</span><span class="sxs-lookup"><span data-stu-id="70e32-150">The value of a variable is simply the value currently stored in the storage location identified by the variable.</span></span> <span data-ttu-id="70e32-151">Uma variável deve ser considerada atribuída definitivamente ([atribuição definitiva](variables.md#definite-assignment)) antes de seu valor pode ser obtido ou, caso contrário, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-151">A variable must be considered definitely assigned ([Definite assignment](variables.md#definite-assignment)) before its value can be obtained, or otherwise a compile-time error occurs.</span></span>
*  <span data-ttu-id="70e32-152">O valor de uma expressão de acesso de propriedade é obtido invocando o *acessador get* da propriedade.</span><span class="sxs-lookup"><span data-stu-id="70e32-152">The value of a property access expression is obtained by invoking the *get accessor* of the property.</span></span> <span data-ttu-id="70e32-153">Se a propriedade não tiver nenhuma *acessador get*, ocorre um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-153">If the property has no *get accessor*, a compile-time error occurs.</span></span> <span data-ttu-id="70e32-154">Caso contrário, uma invocação de membro de função ([verificação de resolução de sobrecarga de dinâmica de tempo de compilação](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) é executada, e o resultado da invocação se tornará o valor da expressão de acesso de propriedade.</span><span class="sxs-lookup"><span data-stu-id="70e32-154">Otherwise, a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is performed, and the result of the invocation becomes the value of the property access expression.</span></span>
*  <span data-ttu-id="70e32-155">O valor de uma expressão de acesso do indexador é obtido invocando o *acessador get* do indexador.</span><span class="sxs-lookup"><span data-stu-id="70e32-155">The value of an indexer access expression is obtained by invoking the *get accessor* of the indexer.</span></span> <span data-ttu-id="70e32-156">Se o indexador não tiver nenhuma *acessador get*, ocorre um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-156">If the indexer has no *get accessor*, a compile-time error occurs.</span></span> <span data-ttu-id="70e32-157">Caso contrário, uma invocação de membro de função ([verificação de resolução de sobrecarga de dinâmica de tempo de compilação](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) é executada com o argumento de lista associado com a expressão de acesso do indexador e o resultado da invocação se tornará o valor da expressão de acesso do indexador.</span><span class="sxs-lookup"><span data-stu-id="70e32-157">Otherwise, a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is performed with the argument list associated with the indexer access expression, and the result of the invocation becomes the value of the indexer access expression.</span></span>

## <a name="static-and-dynamic-binding"></a><span data-ttu-id="70e32-158">Associação estática e dinâmica</span><span class="sxs-lookup"><span data-stu-id="70e32-158">Static and Dynamic Binding</span></span>

<span data-ttu-id="70e32-159">O processo de determinar o significado de uma operação com base no tipo ou valor de expressões constituintes (argumentos, operandos, receptores) é geralmente denominado ***associação***.</span><span class="sxs-lookup"><span data-stu-id="70e32-159">The process of determining the meaning of an operation based on the type or value of constituent expressions (arguments, operands, receivers) is often referred to as ***binding***.</span></span> <span data-ttu-id="70e32-160">Por exemplo o significado de uma chamada de método é determinado com base no tipo dos argumentos e o receptor.</span><span class="sxs-lookup"><span data-stu-id="70e32-160">For instance the meaning of a method call is determined based on the type of the receiver and arguments.</span></span> <span data-ttu-id="70e32-161">O significado de um operador é determinado com base no tipo de seus operandos.</span><span class="sxs-lookup"><span data-stu-id="70e32-161">The meaning of an operator is determined based on the type of its operands.</span></span>

<span data-ttu-id="70e32-162">Em c# o significado de uma operação normalmente é determinada em tempo de compilação, com base no tipo de tempo de compilação das suas expressões constituintes.</span><span class="sxs-lookup"><span data-stu-id="70e32-162">In C# the meaning of an operation is usually determined at compile-time, based on the compile-time type of its constituent expressions.</span></span> <span data-ttu-id="70e32-163">Da mesma forma, se uma expressão contiver um erro, o erro é detectado e reportado pelo compilador.</span><span class="sxs-lookup"><span data-stu-id="70e32-163">Likewise, if an expression contains an error, the error is detected and reported by the compiler.</span></span> <span data-ttu-id="70e32-164">Essa abordagem é conhecida como ***vinculação estática***.</span><span class="sxs-lookup"><span data-stu-id="70e32-164">This approach is known as ***static binding***.</span></span>

<span data-ttu-id="70e32-165">No entanto, se uma expressão é uma expressão dinâmica (ou seja, tem o tipo `dynamic`) indica que qualquer ligação que participa do deve ser baseada em seu tipo de tempo de execução (ou seja, o tipo real do objeto, ele indicará em tempo de execução) em vez do tipo tem no tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-165">However, if an expression is a dynamic expression (i.e. has the type `dynamic`) this indicates that any binding that it participates in should be based on its run-time type (i.e. the actual type of the object it denotes at run-time) rather than the type it has at compile-time.</span></span> <span data-ttu-id="70e32-166">A associação dessa operação, portanto, é adiada até o momento em que a operação deve ser executado durante a execução do programa.</span><span class="sxs-lookup"><span data-stu-id="70e32-166">The binding of such an operation is therefore deferred until the time where the operation is to be executed during the running of the program.</span></span> <span data-ttu-id="70e32-167">Isso é conhecido como ***vinculação dinâmica***.</span><span class="sxs-lookup"><span data-stu-id="70e32-167">This is referred to as ***dynamic binding***.</span></span>

<span data-ttu-id="70e32-168">Quando uma operação é vinculada dinamicamente, pouca ou nenhuma verificação é executada pelo compilador.</span><span class="sxs-lookup"><span data-stu-id="70e32-168">When an operation is dynamically bound, little or no checking is performed by the compiler.</span></span> <span data-ttu-id="70e32-169">Em vez disso, se a associação de tempo de execução falhar, os erros são relatados como exceções em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="70e32-169">Instead if the run-time binding fails, errors are reported as exceptions at run-time.</span></span>

<span data-ttu-id="70e32-170">As seguintes operações em c# estão sujeitos a associação:</span><span class="sxs-lookup"><span data-stu-id="70e32-170">The following operations in C# are subject to binding:</span></span>

*  <span data-ttu-id="70e32-171">Acesso de membro: `e.M`</span><span class="sxs-lookup"><span data-stu-id="70e32-171">Member access: `e.M`</span></span>
*  <span data-ttu-id="70e32-172">Invocação de método: `e.M(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="70e32-172">Method invocation: `e.M(e1, ..., eN)`</span></span>
*  <span data-ttu-id="70e32-173">Invocação de delegado:`e(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="70e32-173">Delegate invocation:`e(e1, ..., eN)`</span></span>
*  <span data-ttu-id="70e32-174">Acesso de elemento: `e[e1, ..., eN]`</span><span class="sxs-lookup"><span data-stu-id="70e32-174">Element access: `e[e1, ..., eN]`</span></span>
*  <span data-ttu-id="70e32-175">Criação do objeto: `new C(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="70e32-175">Object creation: `new C(e1, ..., eN)`</span></span>
*  <span data-ttu-id="70e32-176">Operadores unários sobrecarregados: `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`</span><span class="sxs-lookup"><span data-stu-id="70e32-176">Overloaded unary operators: `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`</span></span>
*  <span data-ttu-id="70e32-177">Sobrecarga dos operadores binários: `+`, `-`, `*`, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, `^`, `<<` , `>>`, `==`,`!=`, `>`, `<`, `>=`, `<=`</span><span class="sxs-lookup"><span data-stu-id="70e32-177">Overloaded binary operators: `+`, `-`, `*`, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, `^`, `<<`, `>>`, `==`,`!=`, `>`, `<`, `>=`, `<=`</span></span>
*  <span data-ttu-id="70e32-178">Operadores de atribuição: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, `>>=`</span><span class="sxs-lookup"><span data-stu-id="70e32-178">Assignment operators: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, `>>=`</span></span>
*  <span data-ttu-id="70e32-179">Conversões implícitas e explícitas</span><span class="sxs-lookup"><span data-stu-id="70e32-179">Implicit and explicit conversions</span></span>

<span data-ttu-id="70e32-180">Quando não há expressões dinâmicas estão envolvidas, c# assume como padrão a vinculação estática, o que significa que os tipos de tempo de compilação de expressões constituintes são usados no processo de seleção.</span><span class="sxs-lookup"><span data-stu-id="70e32-180">When no dynamic expressions are involved, C# defaults to static binding, which means that the compile-time types of constituent expressions are used in the selection process.</span></span> <span data-ttu-id="70e32-181">No entanto, quando uma das expressões constituintes nas operações listadas acima é uma expressão dinâmica, a operação em vez disso, vinculada dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="70e32-181">However, when one of the constituent expressions in the operations listed above is a dynamic expression, the operation is instead dynamically bound.</span></span>

### <a name="binding-time"></a><span data-ttu-id="70e32-182">Tempo de associação</span><span class="sxs-lookup"><span data-stu-id="70e32-182">Binding-time</span></span>

<span data-ttu-id="70e32-183">Vinculação estática ocorre em tempo de compilação, ao passo que a vinculação dinâmica ocorre em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="70e32-183">Static binding takes place at compile-time, whereas dynamic binding takes place at run-time.</span></span> <span data-ttu-id="70e32-184">Nas seções a seguir, o termo ***tempo de associação*** refere-se ao tempo de compilação ou tempo de execução, dependendo de quando ocorre a associação.</span><span class="sxs-lookup"><span data-stu-id="70e32-184">In the following sections, the term ***binding-time*** refers to either compile-time or run-time, depending on when the binding takes place.</span></span>

<span data-ttu-id="70e32-185">O exemplo a seguir ilustra as noções de associação estática e dinâmica e de tempo de associação:</span><span class="sxs-lookup"><span data-stu-id="70e32-185">The following example illustrates the notions of static and dynamic binding and of binding-time:</span></span>
```csharp
object  o = 5;
dynamic d = 5;

Console.WriteLine(5);  // static  binding to Console.WriteLine(int)
Console.WriteLine(o);  // static  binding to Console.WriteLine(object)
Console.WriteLine(d);  // dynamic binding to Console.WriteLine(int)
```

<span data-ttu-id="70e32-186">As duas primeiras chamadas são vinculadas estaticamente: a sobrecarga de `Console.WriteLine` é escolhida com base no tipo de tempo de compilação do seu argumento.</span><span class="sxs-lookup"><span data-stu-id="70e32-186">The first two calls are statically bound: the overload of `Console.WriteLine` is picked based on the compile-time type of their argument.</span></span> <span data-ttu-id="70e32-187">Assim, o tempo de associação é o tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-187">Thus, the binding-time is compile-time.</span></span>

<span data-ttu-id="70e32-188">A terceira chamada associada dinamicamente: a sobrecarga de `Console.WriteLine` é escolhida com base no tipo de tempo de execução do seu argumento.</span><span class="sxs-lookup"><span data-stu-id="70e32-188">The third call is dynamically bound: the overload of `Console.WriteLine` is picked based on the run-time type of its argument.</span></span> <span data-ttu-id="70e32-189">Isso acontece porque o argumento é uma expressão dinâmica – seu tipo de tempo de compilação é `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="70e32-189">This happens because the argument is a dynamic expression -- its compile-time type is `dynamic`.</span></span> <span data-ttu-id="70e32-190">Portanto, o tempo de associação para a terceira chamada é o tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="70e32-190">Thus, the binding-time for the third call is run-time.</span></span>

### <a name="dynamic-binding"></a><span data-ttu-id="70e32-191">Associação dinâmica</span><span class="sxs-lookup"><span data-stu-id="70e32-191">Dynamic binding</span></span>

<span data-ttu-id="70e32-192">A finalidade de associação dinâmica é permitir que programas em c# interagir com ***objetos dinâmicos***, ou seja, sistema de tipos de objetos que não seguem as regras normais do c#.</span><span class="sxs-lookup"><span data-stu-id="70e32-192">The purpose of dynamic binding is to allow C# programs to interact with ***dynamic objects***, i.e. objects that do not follow the normal rules of the C# type system.</span></span> <span data-ttu-id="70e32-193">Objetos dinâmicos podem ser objetos de outras linguagens de programação com sistemas de tipos diferentes, ou podem ser objetos por meio de programação são configuradas para implementar suas próprias semânticas de associação para operações diferentes.</span><span class="sxs-lookup"><span data-stu-id="70e32-193">Dynamic objects may be objects from other programming languages with different types systems, or they may be objects that are programmatically setup to implement their own binding semantics for different operations.</span></span>

<span data-ttu-id="70e32-194">O mecanismo pelo qual um objeto dinâmico implementa seu próprio semântica é definido pela implementação.</span><span class="sxs-lookup"><span data-stu-id="70e32-194">The mechanism by which a dynamic object implements its own semantics is implementation defined.</span></span> <span data-ttu-id="70e32-195">Uma determinada interface – novamente definido pela implementação – é implementada por objetos dinâmicos para sinalizar para o c# tempo de execução que eles têm semântica especial.</span><span class="sxs-lookup"><span data-stu-id="70e32-195">A given interface -- again implementation defined -- is implemented by dynamic objects to signal to the C# run-time that they have special semantics.</span></span> <span data-ttu-id="70e32-196">Assim, sempre que as operações em um objeto dinâmico são vinculadas dinamicamente, suas próprias semânticas de associação, em vez da linguagem c#, conforme especificado neste documento, assumir.</span><span class="sxs-lookup"><span data-stu-id="70e32-196">Thus, whenever operations on a dynamic object are dynamically bound, their own binding semantics, rather than those of C# as specified in this document, take over.</span></span>

<span data-ttu-id="70e32-197">Enquanto a finalidade de associação dinâmica é permitir a interoperação com objetos dinâmicos, c# permite a associação dinâmica em todos os objetos, independentemente de estarem dinâmicos ou não.</span><span class="sxs-lookup"><span data-stu-id="70e32-197">While the purpose of dynamic binding is to allow interoperation with dynamic objects, C# allows dynamic binding on all objects, whether they are dynamic or not.</span></span> <span data-ttu-id="70e32-198">Isso permite uma integração mais suave de objetos dinâmicos, como os resultados das operações neles talvez por si próprios ser objetos dinâmicos, mas são ainda de um tipo desconhecido para o programador em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-198">This allows for a smoother integration of dynamic objects, as the results of operations on them may not themselves be dynamic objects, but are still of a type unknown to the programmer at compile-time.</span></span> <span data-ttu-id="70e32-199">Também a vinculação dinâmica pode ajudar a eliminar o código de baseados em reflexão propenso a erro mesmo quando não há objetos envolvidos são objetos dinâmicos.</span><span class="sxs-lookup"><span data-stu-id="70e32-199">Also dynamic binding can help eliminate error-prone reflection-based code even when no objects involved are dynamic objects.</span></span>

<span data-ttu-id="70e32-200">As seções a seguir descrevem cada constructo no idioma for exatamente quando vinculação dinâmica é aplicada, o que compilar a verificação de tempo – se qualquer um-- é aplicada e qual classificação de resultados e a expressão de tempo de compilação é.</span><span class="sxs-lookup"><span data-stu-id="70e32-200">The following sections describe for each construct in the language exactly when dynamic binding is applied, what compile time checking -- if any -- is applied, and what the compile-time result and expression classification is.</span></span>

### <a name="types-of-constituent-expressions"></a><span data-ttu-id="70e32-201">Tipos de expressões constituintes</span><span class="sxs-lookup"><span data-stu-id="70e32-201">Types of constituent expressions</span></span>

<span data-ttu-id="70e32-202">Quando uma operação é vinculada estaticamente, o tipo de uma expressão constituinte (por exemplo, um receptor e argumento, um índice ou um operando) é sempre considerado como o tipo de tempo de compilação dessa expressão.</span><span class="sxs-lookup"><span data-stu-id="70e32-202">When an operation is statically bound, the type of a constituent expression (e.g. a receiver, and argument, an index or an operand) is always considered to be the compile-time type of that expression.</span></span>

<span data-ttu-id="70e32-203">Quando uma operação dinamicamente é vinculada, o tipo de uma expressão constituinte é determinado de maneiras diferentes dependendo do tipo de tempo de compilação da expressão constituinte:</span><span class="sxs-lookup"><span data-stu-id="70e32-203">When an operation is dynamically bound, the type of a constituent expression is determined in different ways depending on the compile-time type of the constituent expression:</span></span>

*  <span data-ttu-id="70e32-204">Uma expressão constituinte do tipo de tempo de compilação `dynamic` é considerado como tendo o tipo do valor real que a expressão é avaliada em tempo de execução</span><span class="sxs-lookup"><span data-stu-id="70e32-204">A constituent expression of compile-time type `dynamic` is considered to have the type of the actual value that the expression evaluates to at runtime</span></span>
*  <span data-ttu-id="70e32-205">Uma expressão constituinte cujo tipo de tempo de compilação é um parâmetro de tipo é considerada como tendo o tipo que o parâmetro de tipo é associado em tempo de execução</span><span class="sxs-lookup"><span data-stu-id="70e32-205">A constituent expression whose compile-time type is a type parameter is considered to have the type which the type parameter is bound to at runtime</span></span>
*  <span data-ttu-id="70e32-206">Caso contrário, a expressão constituinte é considerada como tendo seu tipo de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-206">Otherwise the constituent expression is considered to have its compile-time type.</span></span>

## <a name="operators"></a><span data-ttu-id="70e32-207">Operadores</span><span class="sxs-lookup"><span data-stu-id="70e32-207">Operators</span></span>

<span data-ttu-id="70e32-208">Expressões são construídas a partir ***operandos*** e ***operadores***.</span><span class="sxs-lookup"><span data-stu-id="70e32-208">Expressions are constructed from ***operands*** and ***operators***.</span></span> <span data-ttu-id="70e32-209">Os operadores de uma expressão indicam quais operações devem ser aplicadas aos operandos.</span><span class="sxs-lookup"><span data-stu-id="70e32-209">The operators of an expression indicate which operations to apply to the operands.</span></span> <span data-ttu-id="70e32-210">Exemplos de operadores incluem `+`, `-`, `*`, `/` e `new`.</span><span class="sxs-lookup"><span data-stu-id="70e32-210">Examples of operators include `+`, `-`, `*`, `/`, and `new`.</span></span> <span data-ttu-id="70e32-211">Exemplos de operandos incluem literais, campos, variáveis locais e expressões.</span><span class="sxs-lookup"><span data-stu-id="70e32-211">Examples of operands include literals, fields, local variables, and expressions.</span></span>

<span data-ttu-id="70e32-212">Há três tipos de operadores:</span><span class="sxs-lookup"><span data-stu-id="70e32-212">There are three kinds of operators:</span></span>

*  <span data-ttu-id="70e32-213">Operadores unários.</span><span class="sxs-lookup"><span data-stu-id="70e32-213">Unary operators.</span></span> <span data-ttu-id="70e32-214">Os operadores unários usam um operando e usar a notação de prefixo (como `--x`) ou notação de sufixo (como `x++`).</span><span class="sxs-lookup"><span data-stu-id="70e32-214">The unary operators take one operand and use either prefix notation (such as `--x`) or postfix notation (such as `x++`).</span></span>
*  <span data-ttu-id="70e32-215">Operadores binários.</span><span class="sxs-lookup"><span data-stu-id="70e32-215">Binary operators.</span></span> <span data-ttu-id="70e32-216">Os operadores binários usam dois operandos e todas usam a notação de infixo (como `x + y`).</span><span class="sxs-lookup"><span data-stu-id="70e32-216">The binary operators take two operands and all use infix notation (such as `x + y`).</span></span>
*  <span data-ttu-id="70e32-217">operador ternário.</span><span class="sxs-lookup"><span data-stu-id="70e32-217">Ternary operator.</span></span> <span data-ttu-id="70e32-218">Apenas um operador ternário `?:`, existe; ele usa três operandos e usa a notação de infixo (`c ? x : y`).</span><span class="sxs-lookup"><span data-stu-id="70e32-218">Only one ternary operator, `?:`, exists; it takes three operands and uses infix notation (`c ? x : y`).</span></span>

<span data-ttu-id="70e32-219">A ordem de avaliação de operadores em uma expressão é determinada pelo ***precedência*** e ***associatividade*** dos operadores ([precedência e associatividade](expressions.md#operator-precedence-and-associativity)) .</span><span class="sxs-lookup"><span data-stu-id="70e32-219">The order of evaluation of operators in an expression is determined by the ***precedence*** and ***associativity*** of the operators ([Operator precedence and associativity](expressions.md#operator-precedence-and-associativity)).</span></span>

<span data-ttu-id="70e32-220">Os operandos em uma expressão são avaliados da esquerda para a direita.</span><span class="sxs-lookup"><span data-stu-id="70e32-220">Operands in an expression are evaluated from left to right.</span></span> <span data-ttu-id="70e32-221">Por exemplo, na `F(i) + G(i++) * H(i)`, método `F` for chamado usando o valor antigo da `i`, em seguida, o método `G` é chamado com o valor antigo da `i`e, finalmente, o método `H` é chamado com o novo valor de `i`.</span><span class="sxs-lookup"><span data-stu-id="70e32-221">For example, in `F(i) + G(i++) * H(i)`, method `F` is called using the old value of `i`, then method `G` is called with the old value of `i`, and, finally, method `H` is called with the new value of `i`.</span></span> <span data-ttu-id="70e32-222">Isso é separada e não relacionadas à precedência de operador.</span><span class="sxs-lookup"><span data-stu-id="70e32-222">This is separate from and unrelated to operator precedence.</span></span>

<span data-ttu-id="70e32-223">Determinados operadores podem ser ***sobrecarregados***.</span><span class="sxs-lookup"><span data-stu-id="70e32-223">Certain operators can be ***overloaded***.</span></span> <span data-ttu-id="70e32-224">Sobrecarga de operador permite que implementações de operador definido pelo usuário deve ser especificado para operações em que um ou ambos os operandos são de um tipo de classe ou estrutura definida pelo usuário ([sobrecarga de operador](expressions.md#operator-overloading)).</span><span class="sxs-lookup"><span data-stu-id="70e32-224">Operator overloading permits user-defined operator implementations to be specified for operations where one or both of the operands are of a user-defined class or struct type ([Operator overloading](expressions.md#operator-overloading)).</span></span>

### <a name="operator-precedence-and-associativity"></a><span data-ttu-id="70e32-225">Precedência e associatividade do operador</span><span class="sxs-lookup"><span data-stu-id="70e32-225">Operator precedence and associativity</span></span>

<span data-ttu-id="70e32-226">Quando uma expressão contiver vários operadores, a ***precedência*** dos operadores controla a ordem na qual os operadores individuais são avaliados.</span><span class="sxs-lookup"><span data-stu-id="70e32-226">When an expression contains multiple operators, the ***precedence*** of the operators controls the order in which the individual operators are evaluated.</span></span> <span data-ttu-id="70e32-227">Por exemplo, a expressão `x + y * z` é avaliado como `x + (y * z)` porque o `*` operador tem precedência maior do que o binário `+` operador.</span><span class="sxs-lookup"><span data-stu-id="70e32-227">For example, the expression `x + y * z` is evaluated as `x + (y * z)` because the `*` operator has higher precedence than the binary `+` operator.</span></span> <span data-ttu-id="70e32-228">A precedência de um operador é estabelecida pela definição de sua produção de gramática associado.</span><span class="sxs-lookup"><span data-stu-id="70e32-228">The precedence of an operator is established by the definition of its associated grammar production.</span></span> <span data-ttu-id="70e32-229">Por exemplo, um *additive_expression* consiste em uma sequência de *multiplicative_expression*s separados por `+` ou `-` operadores, proporcionando assim a `+` e `-` operadores menor precedência que o `*`, `/`, e `%` operadores.</span><span class="sxs-lookup"><span data-stu-id="70e32-229">For example, an *additive_expression* consists of a sequence of *multiplicative_expression*s separated by `+` or `-` operators, thus giving the `+` and `-` operators lower precedence than the `*`, `/`, and `%` operators.</span></span>

<span data-ttu-id="70e32-230">A tabela a seguir resume todos os operadores na ordem de precedência da mais alta para a mais baixa:</span><span class="sxs-lookup"><span data-stu-id="70e32-230">The following table summarizes all operators in order of precedence from highest to lowest:</span></span>

| <span data-ttu-id="70e32-231">__Section__</span><span class="sxs-lookup"><span data-stu-id="70e32-231">__Section__</span></span>                                                                                   | <span data-ttu-id="70e32-232">__Categoria__</span><span class="sxs-lookup"><span data-stu-id="70e32-232">__Category__</span></span>                | <span data-ttu-id="70e32-233">__Operadores__</span><span class="sxs-lookup"><span data-stu-id="70e32-233">__Operators__</span></span> | 
|-----------------------------------------------------------------------------------------------|-----------------------------|---------------|
| [<span data-ttu-id="70e32-234">Expressões primárias</span><span class="sxs-lookup"><span data-stu-id="70e32-234">Primary expressions</span></span>](expressions.md#primary-expressions)                                     | <span data-ttu-id="70e32-235">Primária</span><span class="sxs-lookup"><span data-stu-id="70e32-235">Primary</span></span>                     | <span data-ttu-id="70e32-236">`x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate`</span><span class="sxs-lookup"><span data-stu-id="70e32-236">`x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate`</span></span> | 
| [<span data-ttu-id="70e32-237">Operadores unários</span><span class="sxs-lookup"><span data-stu-id="70e32-237">Unary operators</span></span>](expressions.md#unary-operators)                                             | <span data-ttu-id="70e32-238">Unário</span><span class="sxs-lookup"><span data-stu-id="70e32-238">Unary</span></span>                       | <span data-ttu-id="70e32-239">`+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x`</span><span class="sxs-lookup"><span data-stu-id="70e32-239">`+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x`</span></span> | 
| [<span data-ttu-id="70e32-240">Operadores aritméticos</span><span class="sxs-lookup"><span data-stu-id="70e32-240">Arithmetic operators</span></span>](expressions.md#arithmetic-operators)                                   | <span data-ttu-id="70e32-241">Multiplicativo</span><span class="sxs-lookup"><span data-stu-id="70e32-241">Multiplicative</span></span>              | <span data-ttu-id="70e32-242">`*`  `/`  `%`</span><span class="sxs-lookup"><span data-stu-id="70e32-242">`*`  `/`  `%`</span></span> | 
| [<span data-ttu-id="70e32-243">Operadores aritméticos</span><span class="sxs-lookup"><span data-stu-id="70e32-243">Arithmetic operators</span></span>](expressions.md#arithmetic-operators)                                   | <span data-ttu-id="70e32-244">Aditivo</span><span class="sxs-lookup"><span data-stu-id="70e32-244">Additive</span></span>                    | <span data-ttu-id="70e32-245">`+`  `-`</span><span class="sxs-lookup"><span data-stu-id="70e32-245">`+`  `-`</span></span>      | 
| [<span data-ttu-id="70e32-246">Operadores shift</span><span class="sxs-lookup"><span data-stu-id="70e32-246">Shift operators</span></span>](expressions.md#shift-operators)                                             | <span data-ttu-id="70e32-247">Shift</span><span class="sxs-lookup"><span data-stu-id="70e32-247">Shift</span></span>                       | <span data-ttu-id="70e32-248">`<<`  `>>`</span><span class="sxs-lookup"><span data-stu-id="70e32-248">`<<`  `>>`</span></span>    | 
| [<span data-ttu-id="70e32-249">Operadores de teste de tipo e relacional</span><span class="sxs-lookup"><span data-stu-id="70e32-249">Relational and type-testing operators</span></span>](expressions.md#relational-and-type-testing-operators) | <span data-ttu-id="70e32-250">Teste de tipo e relacional</span><span class="sxs-lookup"><span data-stu-id="70e32-250">Relational and type testing</span></span> | <span data-ttu-id="70e32-251">`<`  `>`  `<=`  `>=`  `is`  `as`</span><span class="sxs-lookup"><span data-stu-id="70e32-251">`<`  `>`  `<=`  `>=`  `is`  `as`</span></span> | 
| [<span data-ttu-id="70e32-252">Operadores de teste de tipo e relacional</span><span class="sxs-lookup"><span data-stu-id="70e32-252">Relational and type-testing operators</span></span>](expressions.md#relational-and-type-testing-operators) | <span data-ttu-id="70e32-253">Igualdade</span><span class="sxs-lookup"><span data-stu-id="70e32-253">Equality</span></span>                    | <span data-ttu-id="70e32-254">`==`  `!=`</span><span class="sxs-lookup"><span data-stu-id="70e32-254">`==`  `!=`</span></span>    | 
| [<span data-ttu-id="70e32-255">Operadores lógicos</span><span class="sxs-lookup"><span data-stu-id="70e32-255">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="70e32-256">AND lógico</span><span class="sxs-lookup"><span data-stu-id="70e32-256">Logical AND</span></span>                 | `&`           | 
| [<span data-ttu-id="70e32-257">Operadores lógicos</span><span class="sxs-lookup"><span data-stu-id="70e32-257">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="70e32-258">XOR lógico</span><span class="sxs-lookup"><span data-stu-id="70e32-258">Logical XOR</span></span>                 | `^`           | 
| [<span data-ttu-id="70e32-259">Operadores lógicos</span><span class="sxs-lookup"><span data-stu-id="70e32-259">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="70e32-260">OR lógico</span><span class="sxs-lookup"><span data-stu-id="70e32-260">Logical OR</span></span>                  | `|`           |
| [<span data-ttu-id="70e32-261">Operadores lógicos condicionais</span><span class="sxs-lookup"><span data-stu-id="70e32-261">Conditional logical operators</span></span>](expressions.md#conditional-logical-operators)                 | <span data-ttu-id="70e32-262">AND condicional</span><span class="sxs-lookup"><span data-stu-id="70e32-262">Conditional AND</span></span>             | `&&`          | 
| [<span data-ttu-id="70e32-263">Operadores lógicos condicionais</span><span class="sxs-lookup"><span data-stu-id="70e32-263">Conditional logical operators</span></span>](expressions.md#conditional-logical-operators)                 | <span data-ttu-id="70e32-264">OR condicional</span><span class="sxs-lookup"><span data-stu-id="70e32-264">Conditional OR</span></span>              | `||`          | 
| [<span data-ttu-id="70e32-265">O operador de coalescência nula</span><span class="sxs-lookup"><span data-stu-id="70e32-265">The null coalescing operator</span></span>](expressions.md#the-null-coalescing-operator)                   | <span data-ttu-id="70e32-266">Coalescência nula</span><span class="sxs-lookup"><span data-stu-id="70e32-266">Null coalescing</span></span>             | `??`          | 
| [<span data-ttu-id="70e32-267">Operador condicional</span><span class="sxs-lookup"><span data-stu-id="70e32-267">Conditional operator</span></span>](expressions.md#conditional-operator)                                   | <span data-ttu-id="70e32-268">Condicional</span><span class="sxs-lookup"><span data-stu-id="70e32-268">Conditional</span></span>                 | `?:`          | 
| <span data-ttu-id="70e32-269">[Operadores de atribuição](expressions.md#assignment-operators), [expressões de função anônima](expressions.md#anonymous-function-expressions)</span><span class="sxs-lookup"><span data-stu-id="70e32-269">[Assignment operators](expressions.md#assignment-operators), [Anonymous function expressions](expressions.md#anonymous-function-expressions)</span></span>  | <span data-ttu-id="70e32-270">Atribuição e expressão lambda</span><span class="sxs-lookup"><span data-stu-id="70e32-270">Assignment and lambda expression</span></span> | <span data-ttu-id="70e32-271">`=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  \`</span><span class="sxs-lookup"><span data-stu-id="70e32-271">`=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  \`</span></span>|=`  `=>` | 

<span data-ttu-id="70e32-272">Quando ocorre um operando entre dois operadores com a mesma precedência, a associatividade dos operadores controla a ordem na qual as operações são executadas:</span><span class="sxs-lookup"><span data-stu-id="70e32-272">When an operand occurs between two operators with the same precedence, the associativity of the operators controls the order in which the operations are performed:</span></span>

*  <span data-ttu-id="70e32-273">Exceto para os operadores de atribuição e o operador de união nulo, todos os operadores binários são ***associativos à esquerda***, que significa que operações são executadas da esquerda para a direita.</span><span class="sxs-lookup"><span data-stu-id="70e32-273">Except for the assignment operators and the null coalescing operator, all binary operators are ***left-associative***, meaning that operations are performed from left to right.</span></span> <span data-ttu-id="70e32-274">Por exemplo, `x + y + z` é avaliado como `(x + y) + z`.</span><span class="sxs-lookup"><span data-stu-id="70e32-274">For example, `x + y + z` is evaluated as `(x + y) + z`.</span></span>
*  <span data-ttu-id="70e32-275">Os operadores de atribuição, o operador de união nulo e o operador condicional (`?:`) estão ***associativo***, que significa que as operações são executadas da direita para esquerda.</span><span class="sxs-lookup"><span data-stu-id="70e32-275">The assignment operators, the null coalescing operator and the conditional operator (`?:`) are ***right-associative***, meaning that operations are performed from right to left.</span></span> <span data-ttu-id="70e32-276">Por exemplo, `x = y = z` é avaliado como `x = (y = z)`.</span><span class="sxs-lookup"><span data-stu-id="70e32-276">For example, `x = y = z` is evaluated as `x = (y = z)`.</span></span>

<span data-ttu-id="70e32-277">Precedência e associatividade podem ser controladas usando parênteses.</span><span class="sxs-lookup"><span data-stu-id="70e32-277">Precedence and associativity can be controlled using parentheses.</span></span> <span data-ttu-id="70e32-278">Por exemplo, `x + y * z` primeiro multiplica `y` por `z` e, em seguida, adiciona o resultado a `x`, mas `(x + y) * z` primeiro adiciona `x` e `y` e, em seguida, multiplica o resultado por `z`.</span><span class="sxs-lookup"><span data-stu-id="70e32-278">For example, `x + y * z` first multiplies `y` by `z` and then adds the result to `x`, but `(x + y) * z` first adds `x` and `y` and then multiplies the result by `z`.</span></span>

### <a name="operator-overloading"></a><span data-ttu-id="70e32-279">Sobrecarga de operador</span><span class="sxs-lookup"><span data-stu-id="70e32-279">Operator overloading</span></span>

<span data-ttu-id="70e32-280">Todos os operadores unários e binários têm predefinidos implementações que ficam automaticamente disponíveis em qualquer expressão.</span><span class="sxs-lookup"><span data-stu-id="70e32-280">All unary and binary operators have predefined implementations that are automatically available in any expression.</span></span> <span data-ttu-id="70e32-281">As implementações predefinidas, além de implementações definidas pelo usuário podem ser introduzidas, incluindo `operator` declarações em classes e structs ([operadores](classes.md#operators)).</span><span class="sxs-lookup"><span data-stu-id="70e32-281">In addition to the predefined implementations, user-defined implementations can be introduced by including `operator` declarations in classes and structs ([Operators](classes.md#operators)).</span></span> <span data-ttu-id="70e32-282">Implementações de operador definido pelo usuário sempre têm precedência sobre implementações de operadores predefinidos: somente quando não há implementações aplicável operador definido pelo usuário existem serão as implementações de operador pré-definido ser considerado, conforme descrito em [ Resolução de sobrecarga de operador unário](expressions.md#unary-operator-overload-resolution) e [resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="70e32-282">User-defined operator implementations always take precedence over predefined operator implementations: Only when no applicable user-defined operator implementations exist will the predefined operator implementations be considered, as described in [Unary operator overload resolution](expressions.md#unary-operator-overload-resolution) and [Binary operator overload resolution](expressions.md#binary-operator-overload-resolution).</span></span>

<span data-ttu-id="70e32-283">O ***operadores sobrecarregáveis unários*** são:</span><span class="sxs-lookup"><span data-stu-id="70e32-283">The ***overloadable unary operators*** are:</span></span>
```csharp
+   -   !   ~   ++   --   true   false
```

<span data-ttu-id="70e32-284">Embora `true` e `false` não são explicitamente usados em expressões (e, portanto, não são incluídos na tabela precedência [precedência e associatividade](expressions.md#operator-precedence-and-associativity)), eles são considerados operadores porque eles são invocado em vários contextos de expressão: expressões Boolianas ([expressões Boolianas](expressions.md#boolean-expressions)) e expressões que envolvem a condicional ([operador condicional](expressions.md#conditional-operator)) e condicional lógico operadores ([operadores lógicos condicionais](expressions.md#conditional-logical-operators)).</span><span class="sxs-lookup"><span data-stu-id="70e32-284">Although `true` and `false` are not used explicitly in expressions (and therefore are not included in the precedence table in [Operator precedence and associativity](expressions.md#operator-precedence-and-associativity)), they are considered operators because they are invoked in several expression contexts: boolean expressions ([Boolean expressions](expressions.md#boolean-expressions)) and expressions involving the conditional ([Conditional operator](expressions.md#conditional-operator)), and conditional logical operators ([Conditional logical operators](expressions.md#conditional-logical-operators)).</span></span>

<span data-ttu-id="70e32-285">O ***sobrecarregáveis operadores binários*** são:</span><span class="sxs-lookup"><span data-stu-id="70e32-285">The ***overloadable binary operators*** are:</span></span>
```csharp
+   -   *   /   %   &   |   ^   <<   >>   ==   !=   >   <   >=   <=
```

<span data-ttu-id="70e32-286">Somente os operadores listados acima podem ser sobrecarregados.</span><span class="sxs-lookup"><span data-stu-id="70e32-286">Only the operators listed above can be overloaded.</span></span> <span data-ttu-id="70e32-287">Em particular, não é possível sobrecarregar o acesso de membro de invocação de método, ou o `=`, `&&`, `||`, `??`, `?:`, `=>`, `checked`, `unchecked`, `new`, `typeof`, `default`, `as`, e `is` operadores.</span><span class="sxs-lookup"><span data-stu-id="70e32-287">In particular, it is not possible to overload member access, method invocation, or the `=`, `&&`, `||`, `??`, `?:`, `=>`, `checked`, `unchecked`, `new`, `typeof`, `default`, `as`, and `is` operators.</span></span>

<span data-ttu-id="70e32-288">Quando um operador binário está sobrecarregado, o operador de atribuição correspondente, se houver, também estará implicitamente sobrecarregado.</span><span class="sxs-lookup"><span data-stu-id="70e32-288">When a binary operator is overloaded, the corresponding assignment operator, if any, is also implicitly overloaded.</span></span> <span data-ttu-id="70e32-289">Por exemplo, uma sobrecarga de operador `*` também é uma sobrecarga de operador `*=`.</span><span class="sxs-lookup"><span data-stu-id="70e32-289">For example, an overload of operator `*` is also an overload of operator `*=`.</span></span> <span data-ttu-id="70e32-290">Isso é descrito posteriormente em [atribuição composta](expressions.md#compound-assignment).</span><span class="sxs-lookup"><span data-stu-id="70e32-290">This is described further in [Compound assignment](expressions.md#compound-assignment).</span></span> <span data-ttu-id="70e32-291">Observe que o operador de atribuição (`=`) não pode ser sobrecarregado.</span><span class="sxs-lookup"><span data-stu-id="70e32-291">Note that the assignment operator itself (`=`) cannot be overloaded.</span></span> <span data-ttu-id="70e32-292">Uma atribuição sempre executa uma simple cópia bit a bit de um valor em uma variável.</span><span class="sxs-lookup"><span data-stu-id="70e32-292">An assignment always performs a simple bit-wise copy of a value into a variable.</span></span>

<span data-ttu-id="70e32-293">Converter operações, como `(T)x`, estão sobrecarregados, fornecendo conversões definidas pelo usuário ([conversões definidas pelo usuário](conversions.md#user-defined-conversions)).</span><span class="sxs-lookup"><span data-stu-id="70e32-293">Cast operations, such as `(T)x`, are overloaded by providing user-defined conversions ([User-defined conversions](conversions.md#user-defined-conversions)).</span></span>

<span data-ttu-id="70e32-294">Acesso de elemento, como `a[x]`, não é considerado um operador que pode ser sobrecarregado.</span><span class="sxs-lookup"><span data-stu-id="70e32-294">Element access, such as `a[x]`, is not considered an overloadable operator.</span></span> <span data-ttu-id="70e32-295">Em vez disso, a indexação de definida pelo usuário tem suporte por meio de indexadores ([indexadores](classes.md#indexers)).</span><span class="sxs-lookup"><span data-stu-id="70e32-295">Instead, user-defined indexing is supported through indexers ([Indexers](classes.md#indexers)).</span></span>

<span data-ttu-id="70e32-296">Em expressões, operadores são referenciados usando a notação do operador e em declarações, operadores são referenciados usando a notação funcional.</span><span class="sxs-lookup"><span data-stu-id="70e32-296">In expressions, operators are referenced using operator notation, and in declarations, operators are referenced using functional notation.</span></span> <span data-ttu-id="70e32-297">A tabela a seguir mostra a relação entre o operador e notações funcionais para o operador unário ou binário.</span><span class="sxs-lookup"><span data-stu-id="70e32-297">The following table shows the relationship between operator and functional notations for unary and binary operators.</span></span> <span data-ttu-id="70e32-298">Na primeira entrada, *op* denota qualquer operador de prefixo unário que pode ser sobrecarregado.</span><span class="sxs-lookup"><span data-stu-id="70e32-298">In the first entry, *op* denotes any overloadable unary prefix operator.</span></span> <span data-ttu-id="70e32-299">Na segunda entrada, *op* denota o sufixo unário `++` e `--` operadores.</span><span class="sxs-lookup"><span data-stu-id="70e32-299">In the second entry, *op* denotes the unary postfix `++` and `--` operators.</span></span> <span data-ttu-id="70e32-300">Na entrada do terceiro *op* denota qualquer operador binário que pode ser sobrecarregado.</span><span class="sxs-lookup"><span data-stu-id="70e32-300">In the third entry, *op* denotes any overloadable binary operator.</span></span>


| <span data-ttu-id="70e32-301">__Notação do operador__</span><span class="sxs-lookup"><span data-stu-id="70e32-301">__Operator notation__</span></span> | <span data-ttu-id="70e32-302">__Notação funcional__</span><span class="sxs-lookup"><span data-stu-id="70e32-302">__Functional notation__</span></span> |
|-----------------------|-------------------------|
| `op x`                | `operator op(x)`        | 
| `x op`                | `operator op(x)`        | 
| `x op y`              | `operator op(x,y)`      | 

<span data-ttu-id="70e32-303">Declarações de operador definido pelo usuário sempre exigem pelo menos um dos parâmetros para ser do tipo de classe ou struct que contém a declaração do operador.</span><span class="sxs-lookup"><span data-stu-id="70e32-303">User-defined operator declarations always require at least one of the parameters to be of the class or struct type that contains the operator declaration.</span></span> <span data-ttu-id="70e32-304">Portanto, não é possível para um operador definido pelo usuário ter a mesma assinatura como um operador predefinido.</span><span class="sxs-lookup"><span data-stu-id="70e32-304">Thus, it is not possible for a user-defined operator to have the same signature as a predefined operator.</span></span>

<span data-ttu-id="70e32-305">Declarações de operador definido pelo usuário não é possível modificar a sintaxe, a precedência ou a associatividade do operador.</span><span class="sxs-lookup"><span data-stu-id="70e32-305">User-defined operator declarations cannot modify the syntax, precedence, or associativity of an operator.</span></span> <span data-ttu-id="70e32-306">Por exemplo, o `/` operador é sempre um operador binário, sempre tem o nível de precedência especificada no [precedência e associatividade](expressions.md#operator-precedence-and-associativity)e é sempre associativos à esquerda.</span><span class="sxs-lookup"><span data-stu-id="70e32-306">For example, the `/` operator is always a binary operator, always has the precedence level specified in [Operator precedence and associativity](expressions.md#operator-precedence-and-associativity), and is always left-associative.</span></span>

<span data-ttu-id="70e32-307">Embora seja possível para um operador definido pelo usuário executar qualquer cálculo, o que desejar, implementações que produzem resultados diferentes daqueles que intuitivamente são esperados são altamente desaconselhável.</span><span class="sxs-lookup"><span data-stu-id="70e32-307">While it is possible for a user-defined operator to perform any computation it pleases, implementations that produce results other than those that are intuitively expected are strongly discouraged.</span></span> <span data-ttu-id="70e32-308">Por exemplo, uma implementação de `operator ==` deve comparar os dois operandos quanto à igualdade e retornar um apropriado `bool` resultado.</span><span class="sxs-lookup"><span data-stu-id="70e32-308">For example, an implementation of `operator ==` should compare the two operands for equality and return an appropriate `bool` result.</span></span>

<span data-ttu-id="70e32-309">As descrições dos operadores individuais na [expressões primárias](expressions.md#primary-expressions) por meio [operadores lógicos condicionais](expressions.md#conditional-logical-operators) especificar as implementações predefinidas de operadores e todas as regras adicionais que se aplicam para cada operador.</span><span class="sxs-lookup"><span data-stu-id="70e32-309">The descriptions of individual operators in [Primary expressions](expressions.md#primary-expressions) through [Conditional logical operators](expressions.md#conditional-logical-operators) specify the predefined implementations of the operators and any additional rules that apply to each operator.</span></span> <span data-ttu-id="70e32-310">Verifique as descrições de usar os termos ***resolução de sobrecarga de operador unário***, ***resolução de sobrecarga de operador binário***, e ***promoção numérica***, definições de quais são encontrado nas seções a seguir.</span><span class="sxs-lookup"><span data-stu-id="70e32-310">The descriptions make use of the terms ***unary operator overload resolution***, ***binary operator overload resolution***, and ***numeric promotion***, definitions of which are found in the following sections.</span></span>

### <a name="unary-operator-overload-resolution"></a><span data-ttu-id="70e32-311">Resolução de sobrecarga de operador unário</span><span class="sxs-lookup"><span data-stu-id="70e32-311">Unary operator overload resolution</span></span>

<span data-ttu-id="70e32-312">Uma operação do formulário `op x` ou `x op`, onde `op` é um operador unário que pode ser sobrecarregado, e `x` é uma expressão do tipo `X`, é processado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="70e32-312">An operation of the form `op x` or `x op`, where `op` is an overloadable unary operator, and `x` is an expression of type `X`, is processed as follows:</span></span>

*  <span data-ttu-id="70e32-313">O conjunto de operadores definidos pelo usuário de candidato fornecidos pelo `X` para a operação `operator op(x)` é determinado usando as regras da [operadores definidos pelo usuário do candidato](expressions.md#candidate-user-defined-operators).</span><span class="sxs-lookup"><span data-stu-id="70e32-313">The set of candidate user-defined operators provided by `X` for the operation `operator op(x)` is determined using the rules of [Candidate user-defined operators](expressions.md#candidate-user-defined-operators).</span></span>
*  <span data-ttu-id="70e32-314">Se o conjunto de operadores definidos pelo usuário do candidato não estiver vazio, em seguida, isso se torna o conjunto de operadores de candidato para a operação.</span><span class="sxs-lookup"><span data-stu-id="70e32-314">If the set of candidate user-defined operators is not empty, then this becomes the set of candidate operators for the operation.</span></span> <span data-ttu-id="70e32-315">Caso contrário, o operador unário de predefinidos `operator op` implementações, incluindo seus formulários elevados, tornam-se o conjunto de operadores de candidato para a operação.</span><span class="sxs-lookup"><span data-stu-id="70e32-315">Otherwise, the predefined unary `operator op` implementations, including their lifted forms, become the set of candidate operators for the operation.</span></span> <span data-ttu-id="70e32-316">As implementações predefinidas de um determinado operador são especificadas na descrição do operador ([expressões primárias](expressions.md#primary-expressions) e [operadores unários](expressions.md#unary-operators)).</span><span class="sxs-lookup"><span data-stu-id="70e32-316">The predefined implementations of a given operator are specified in the description of the operator ([Primary expressions](expressions.md#primary-expressions) and [Unary operators](expressions.md#unary-operators)).</span></span>
*  <span data-ttu-id="70e32-317">As regras de resolução de sobrecarga [resolução de sobrecarga](expressions.md#overload-resolution) são aplicadas ao conjunto de operadores de candidato para selecionar o operador de melhor em relação à lista de argumentos `(x)`, e este operador torna-se o resultado da sobrecarga processo de resolução.</span><span class="sxs-lookup"><span data-stu-id="70e32-317">The overload resolution rules of [Overload resolution](expressions.md#overload-resolution) are applied to the set of candidate operators to select the best operator with respect to the argument list `(x)`, and this operator becomes the result of the overload resolution process.</span></span> <span data-ttu-id="70e32-318">Se a resolução de sobrecarga não selecionar um único operador melhor, ocorrerá um erro de tempo de associação.</span><span class="sxs-lookup"><span data-stu-id="70e32-318">If overload resolution fails to select a single best operator, a binding-time error occurs.</span></span>

### <a name="binary-operator-overload-resolution"></a><span data-ttu-id="70e32-319">Resolução de sobrecarga de operador binário</span><span class="sxs-lookup"><span data-stu-id="70e32-319">Binary operator overload resolution</span></span>

<span data-ttu-id="70e32-320">Uma operação do formulário `x op y`, onde `op` é um operador binário que pode ser sobrecarregado, `x` é uma expressão do tipo `X`, e `y` é uma expressão do tipo `Y`, é processado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="70e32-320">An operation of the form `x op y`, where `op` is an overloadable binary operator, `x` is an expression of type `X`, and `y` is an expression of type `Y`, is processed as follows:</span></span>

*  <span data-ttu-id="70e32-321">O conjunto de operadores definidos pelo usuário de candidato fornecidos pelo `X` e `Y` para a operação `operator op(x,y)` é determinado.</span><span class="sxs-lookup"><span data-stu-id="70e32-321">The set of candidate user-defined operators provided by `X` and `Y` for the operation `operator op(x,y)` is determined.</span></span> <span data-ttu-id="70e32-322">O conjunto consiste na união dos operadores candidato fornecidos pelo `X` e os operadores de candidato fornecidos pelo `Y`, cada determinado usando as regras da [operadores definidos pelo usuário do candidato](expressions.md#candidate-user-defined-operators).</span><span class="sxs-lookup"><span data-stu-id="70e32-322">The set consists of the union of the candidate operators provided by `X` and the candidate operators provided by `Y`, each determined using the rules of [Candidate user-defined operators](expressions.md#candidate-user-defined-operators).</span></span> <span data-ttu-id="70e32-323">Se `X` e `Y` são do mesmo tipo, ou se `X` e `Y` são derivados de um tipo de base comum, em seguida, operadores de candidato compartilhado ocorrerem somente em conjunto combinado uma vez.</span><span class="sxs-lookup"><span data-stu-id="70e32-323">If `X` and `Y` are the same type, or if `X` and `Y` are derived from a common base type, then shared candidate operators only occur in the combined set once.</span></span>
*  <span data-ttu-id="70e32-324">Se o conjunto de operadores definidos pelo usuário do candidato não estiver vazio, em seguida, isso se torna o conjunto de operadores de candidato para a operação.</span><span class="sxs-lookup"><span data-stu-id="70e32-324">If the set of candidate user-defined operators is not empty, then this becomes the set of candidate operators for the operation.</span></span> <span data-ttu-id="70e32-325">Caso contrário, o binário predefinido `operator op` implementações, incluindo seus formulários elevados, tornam-se o conjunto de operadores de candidato para a operação.</span><span class="sxs-lookup"><span data-stu-id="70e32-325">Otherwise, the predefined binary `operator op` implementations, including their lifted forms,  become the set of candidate operators for the operation.</span></span> <span data-ttu-id="70e32-326">As implementações predefinidas de um determinado operador são especificadas na descrição do operador ([operadores aritméticos](expressions.md#arithmetic-operators) por meio [operadores lógicos condicionais](expressions.md#conditional-logical-operators)).</span><span class="sxs-lookup"><span data-stu-id="70e32-326">The predefined implementations of a given operator are specified in the description of the operator ([Arithmetic operators](expressions.md#arithmetic-operators) through [Conditional logical operators](expressions.md#conditional-logical-operators)).</span></span> <span data-ttu-id="70e32-327">Para os operadores predefinidos para enum e delegado, os operadores apenas considerados são aquelas definidas por um tipo de enum ou delegado que é o tipo de tempo de associação de um dos operandos.</span><span class="sxs-lookup"><span data-stu-id="70e32-327">For predefined enum and delegate operators, the only operators considered are those defined by an enum or delegate type that is the binding-time type of one of the operands.</span></span>
*  <span data-ttu-id="70e32-328">As regras de resolução de sobrecarga [resolução de sobrecarga](expressions.md#overload-resolution) são aplicadas ao conjunto de operadores de candidato para selecionar o operador de melhor em relação à lista de argumentos `(x,y)`, e este operador torna-se o resultado da sobrecarga processo de resolução.</span><span class="sxs-lookup"><span data-stu-id="70e32-328">The overload resolution rules of [Overload resolution](expressions.md#overload-resolution) are applied to the set of candidate operators to select the best operator with respect to the argument list `(x,y)`, and this operator becomes the result of the overload resolution process.</span></span> <span data-ttu-id="70e32-329">Se a resolução de sobrecarga não selecionar um único operador melhor, ocorrerá um erro de tempo de associação.</span><span class="sxs-lookup"><span data-stu-id="70e32-329">If overload resolution fails to select a single best operator, a binding-time error occurs.</span></span>

### <a name="candidate-user-defined-operators"></a><span data-ttu-id="70e32-330">Operadores definidos pelo usuário do candidato</span><span class="sxs-lookup"><span data-stu-id="70e32-330">Candidate user-defined operators</span></span>

<span data-ttu-id="70e32-331">Dado um tipo `T` e uma operação `operator op(A)`, onde `op` é um operador que pode ser sobrecarregado e `A` é uma lista de argumentos, o conjunto de candidatos operadores definidos pelo usuário fornecidos pelo `T` para `operator op(A)` é determinada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="70e32-331">Given a type `T` and an operation `operator op(A)`, where `op` is an overloadable operator and `A` is an argument list, the set of candidate user-defined operators provided by `T` for `operator op(A)` is determined as follows:</span></span>

*  <span data-ttu-id="70e32-332">Determinar o tipo `T0`.</span><span class="sxs-lookup"><span data-stu-id="70e32-332">Determine the type `T0`.</span></span> <span data-ttu-id="70e32-333">Se `T` é um tipo anulável, `T0` é o seu tipo subjacente, caso contrário `T0` é igual a `T`.</span><span class="sxs-lookup"><span data-stu-id="70e32-333">If `T` is a nullable type, `T0` is its underlying type, otherwise `T0` is equal to `T`.</span></span>
*  <span data-ttu-id="70e32-334">Para todos os `operator op` declarações na `T0` e todas as retiradas formas desses operadores, se pelo menos um operador é aplicável ([membro da função aplicável](expressions.md#applicable-function-member)) em relação à lista de argumentos `A`, em seguida, o conjunto de operadores de candidato consiste em todos os tais operadores aplicáveis na `T0`.</span><span class="sxs-lookup"><span data-stu-id="70e32-334">For all `operator op` declarations in `T0` and all lifted forms of such operators, if at least one operator is applicable ([Applicable function member](expressions.md#applicable-function-member)) with respect to the argument list `A`, then the set of candidate operators consists of all such applicable operators in `T0`.</span></span>
*  <span data-ttu-id="70e32-335">Caso contrário, se `T0` é `object`, o conjunto de operadores de release candidate está vazio.</span><span class="sxs-lookup"><span data-stu-id="70e32-335">Otherwise, if `T0` is `object`, the set of candidate operators is empty.</span></span>
*  <span data-ttu-id="70e32-336">Caso contrário, o conjunto de operadores de candidato fornecidos pelo `T0` é o conjunto de operadores de candidato fornecidos pela classe base direta de `T0`, ou a classe base efetiva de `T0` se `T0` é um parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="70e32-336">Otherwise, the set of candidate operators provided by `T0` is the set of candidate operators provided by the direct base class of `T0`, or the effective base class of `T0` if `T0` is a type parameter.</span></span>

### <a name="numeric-promotions"></a><span data-ttu-id="70e32-337">Promoções numéricas</span><span class="sxs-lookup"><span data-stu-id="70e32-337">Numeric promotions</span></span>

<span data-ttu-id="70e32-338">Promoção numérica consiste em realizar automaticamente determinadas conversões implícitas de operandos dos operadores numéricos binários e unária predefinida.</span><span class="sxs-lookup"><span data-stu-id="70e32-338">Numeric promotion consists of automatically performing certain implicit conversions of the operands of the predefined unary and binary numeric operators.</span></span> <span data-ttu-id="70e32-339">Promoção numérica não é um mecanismo distinto, mas em vez disso, um efeito de aplicar a resolução de sobrecarga para os operadores predefinidos.</span><span class="sxs-lookup"><span data-stu-id="70e32-339">Numeric promotion is not a distinct mechanism, but rather an effect of applying overload resolution to the predefined operators.</span></span> <span data-ttu-id="70e32-340">Promoção numérica especificamente não afeta a avaliação dos operadores definidos pelo usuário, embora os operadores definidos pelo usuário podem ser implementados para apresentar efeitos semelhantes.</span><span class="sxs-lookup"><span data-stu-id="70e32-340">Numeric promotion specifically does not affect evaluation of user-defined operators, although user-defined operators can be implemented to exhibit similar effects.</span></span>

<span data-ttu-id="70e32-341">Como um exemplo de promoção numérico, considere as implementações predefinidas do binário `*` operador:</span><span class="sxs-lookup"><span data-stu-id="70e32-341">As an example of numeric promotion, consider the predefined implementations of the binary `*` operator:</span></span>

```csharp
int operator *(int x, int y);
uint operator *(uint x, uint y);
long operator *(long x, long y);
ulong operator *(ulong x, ulong y);
float operator *(float x, float y);
double operator *(double x, double y);
decimal operator *(decimal x, decimal y);
```

<span data-ttu-id="70e32-342">Quando as regras de resolução de sobrecarga ([resolução de sobrecarga](expressions.md#overload-resolution)) são aplicadas a esse conjunto de operadores, o efeito é selecionar o primeiro dos operadores para o qual existem conversões implícitas entre os tipos de operando.</span><span class="sxs-lookup"><span data-stu-id="70e32-342">When overload resolution rules ([Overload resolution](expressions.md#overload-resolution)) are applied to this set of operators, the effect is to select the first of the operators for which implicit conversions exist from the operand types.</span></span> <span data-ttu-id="70e32-343">Por exemplo, para a operação `b * s`, onde `b` é um `byte` e `s` é um `short`, seleciona de resolução de sobrecarga `operator *(int,int)` como o operador melhor.</span><span class="sxs-lookup"><span data-stu-id="70e32-343">For example, for the operation `b * s`, where `b` is a `byte` and `s` is a `short`, overload resolution selects `operator *(int,int)` as the best operator.</span></span> <span data-ttu-id="70e32-344">Portanto, o efeito é que `b` e `s` são convertidos em `int`, e o tipo do resultado é `int`.</span><span class="sxs-lookup"><span data-stu-id="70e32-344">Thus, the effect is that `b` and `s` are converted to `int`, and the type of the result is `int`.</span></span> <span data-ttu-id="70e32-345">Da mesma forma, para a operação `i * d`, onde `i` é um `int` e `d` é um `double`, seleciona de resolução de sobrecarga `operator *(double,double)` como o operador melhor.</span><span class="sxs-lookup"><span data-stu-id="70e32-345">Likewise, for the operation `i * d`, where `i` is an `int` and `d` is a `double`, overload resolution selects `operator *(double,double)` as the best operator.</span></span>

#### <a name="unary-numeric-promotions"></a><span data-ttu-id="70e32-346">Promoções de numérico unário</span><span class="sxs-lookup"><span data-stu-id="70e32-346">Unary numeric promotions</span></span>

<span data-ttu-id="70e32-347">Promoção de unário numérica ocorre para os operandos de predefinida `+`, `-`, e `~` operadores unários.</span><span class="sxs-lookup"><span data-stu-id="70e32-347">Unary numeric promotion occurs for the operands of the predefined `+`, `-`, and `~` unary operators.</span></span> <span data-ttu-id="70e32-348">Promoção de numérico unária consiste apenas em operandos do tipo de conversão `sbyte`, `byte`, `short`, `ushort`, ou `char` digitar `int`.</span><span class="sxs-lookup"><span data-stu-id="70e32-348">Unary numeric promotion simply consists of converting operands of type `sbyte`, `byte`, `short`, `ushort`, or `char` to type `int`.</span></span> <span data-ttu-id="70e32-349">Além disso, para o operador unário `-` operador unário de promoção numérico converte operandos do tipo `uint` digitar `long`.</span><span class="sxs-lookup"><span data-stu-id="70e32-349">Additionally, for the unary `-` operator, unary numeric promotion converts operands of type `uint` to type `long`.</span></span>

#### <a name="binary-numeric-promotions"></a><span data-ttu-id="70e32-350">Promoções numéricas binárias</span><span class="sxs-lookup"><span data-stu-id="70e32-350">Binary numeric promotions</span></span>

<span data-ttu-id="70e32-351">Promoção de numérica binária ocorre para os operandos de predefinida `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `==`, `!=`, `>`, `<`, `>=`, e `<=` operadores binários.</span><span class="sxs-lookup"><span data-stu-id="70e32-351">Binary numeric promotion occurs for the operands of the predefined `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `==`, `!=`, `>`, `<`, `>=`, and `<=` binary operators.</span></span> <span data-ttu-id="70e32-352">Promoção numérica binária implicitamente converte ambos os operandos em um tipo comum que, no caso dos operadores relacionais, também se tornará o tipo de resultado da operação.</span><span class="sxs-lookup"><span data-stu-id="70e32-352">Binary numeric promotion implicitly converts both operands to a common type which, in case of the non-relational operators, also becomes the result type of the operation.</span></span> <span data-ttu-id="70e32-353">Promoção de numérica binária consiste em Aplicar regras a seguir, na ordem em que eles aparecem aqui:</span><span class="sxs-lookup"><span data-stu-id="70e32-353">Binary numeric promotion consists of applying the following rules, in the order they appear here:</span></span>

*  <span data-ttu-id="70e32-354">Se qualquer operando for do tipo `decimal`, o outro operando será convertido ao tipo `decimal`, ou um erro em tempo de vinculação ocorrerá se o outro operando for do tipo `float` ou `double`.</span><span class="sxs-lookup"><span data-stu-id="70e32-354">If either operand is of type `decimal`, the other operand is converted to type `decimal`, or a binding-time error occurs if the other operand is of type `float` or `double`.</span></span>
*  <span data-ttu-id="70e32-355">Caso contrário, se qualquer operando for do tipo `double`, o outro operando será convertido ao tipo `double`.</span><span class="sxs-lookup"><span data-stu-id="70e32-355">Otherwise, if either operand is of type `double`, the other operand is converted to type `double`.</span></span>
*  <span data-ttu-id="70e32-356">Caso contrário, se qualquer operando for do tipo `float`, o outro operando será convertido ao tipo `float`.</span><span class="sxs-lookup"><span data-stu-id="70e32-356">Otherwise, if either operand is of type `float`, the other operand is converted to type `float`.</span></span>
*  <span data-ttu-id="70e32-357">Caso contrário, se qualquer operando for do tipo `ulong`, o outro operando será convertido ao tipo `ulong`, ou um erro em tempo de vinculação ocorrerá se o outro operando for do tipo `sbyte`, `short`, `int`, ou `long`.</span><span class="sxs-lookup"><span data-stu-id="70e32-357">Otherwise, if either operand is of type `ulong`, the other operand is converted to type `ulong`, or a binding-time error occurs if the other operand is of type `sbyte`, `short`, `int`, or `long`.</span></span>
*  <span data-ttu-id="70e32-358">Caso contrário, se qualquer operando for do tipo `long`, o outro operando será convertido ao tipo `long`.</span><span class="sxs-lookup"><span data-stu-id="70e32-358">Otherwise, if either operand is of type `long`, the other operand is converted to type `long`.</span></span>
*  <span data-ttu-id="70e32-359">Caso contrário, se qualquer operando for do tipo `uint` e o outro operando for do tipo `sbyte`, `short`, ou `int`, ambos os operandos serão convertidos ao tipo `long`.</span><span class="sxs-lookup"><span data-stu-id="70e32-359">Otherwise, if either operand is of type `uint` and the other operand is of type `sbyte`, `short`, or `int`, both operands are converted to type `long`.</span></span>
*  <span data-ttu-id="70e32-360">Caso contrário, se qualquer operando for do tipo `uint`, o outro operando será convertido ao tipo `uint`.</span><span class="sxs-lookup"><span data-stu-id="70e32-360">Otherwise, if either operand is of type `uint`, the other operand is converted to type `uint`.</span></span>
*  <span data-ttu-id="70e32-361">Caso contrário, os dois operandos serão convertidos ao tipo `int`.</span><span class="sxs-lookup"><span data-stu-id="70e32-361">Otherwise, both operands are converted to type `int`.</span></span>

<span data-ttu-id="70e32-362">Observe que a primeira regra não permite todas as operações que combinam os `decimal` tipo com o `double` e `float` tipos.</span><span class="sxs-lookup"><span data-stu-id="70e32-362">Note that the first rule disallows any operations that mix the `decimal` type with the `double` and `float` types.</span></span> <span data-ttu-id="70e32-363">A regra segue do fato de que não há nenhuma conversão implícita entre o `decimal` tipo e o `double` e `float` tipos.</span><span class="sxs-lookup"><span data-stu-id="70e32-363">The rule follows from the fact that there are no implicit conversions between the `decimal` type and the `double` and `float` types.</span></span>

<span data-ttu-id="70e32-364">Observe também que não é possível que um operando para ser do tipo `ulong` quando o outro operando for de um tipo integral com sinal.</span><span class="sxs-lookup"><span data-stu-id="70e32-364">Also note that it is not possible for an operand to be of type `ulong` when the other operand is of a signed integral type.</span></span> <span data-ttu-id="70e32-365">O motivo é que existe nenhum tipo integral que pode representar a gama completa de `ulong` , bem como os tipos integrais com sinal.</span><span class="sxs-lookup"><span data-stu-id="70e32-365">The reason is that no integral type exists that can represent the full range of `ulong` as well as the signed integral types.</span></span>

<span data-ttu-id="70e32-366">Em ambos os casos acima, uma expressão de conversão pode ser usada para converter explicitamente um operando em um tipo que é compatível com o outro operando.</span><span class="sxs-lookup"><span data-stu-id="70e32-366">In both of the above cases, a cast expression can be used to explicitly convert one operand to a type that is compatible with the other operand.</span></span>

<span data-ttu-id="70e32-367">No exemplo</span><span class="sxs-lookup"><span data-stu-id="70e32-367">In the example</span></span>
```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (1.0 + percent / 100.0);
}
```
<span data-ttu-id="70e32-368">um erro de tempo de associação ocorre porque uma `decimal` não pode ser multiplicada por uma `double`.</span><span class="sxs-lookup"><span data-stu-id="70e32-368">a binding-time error occurs because a `decimal` cannot be multiplied by a `double`.</span></span> <span data-ttu-id="70e32-369">O erro seja resolvido com a conversão explícita para o segundo operando `decimal`, da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="70e32-369">The error is resolved by explicitly converting the second operand to `decimal`, as follows:</span></span>

```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (decimal)(1.0 + percent / 100.0);
}
```

### <a name="lifted-operators"></a><span data-ttu-id="70e32-370">Operadores canceladas</span><span class="sxs-lookup"><span data-stu-id="70e32-370">Lifted operators</span></span>

<span data-ttu-id="70e32-371">***Eliminada operadores*** permitir que operadores predefinidos e definidos pelo usuário que operam em tipos de valor não anulável para também ser usados com formulários que permitem valor nulos desses tipos.</span><span class="sxs-lookup"><span data-stu-id="70e32-371">***Lifted operators*** permit predefined and user-defined operators that operate on non-nullable value types to also be used with nullable forms of those types.</span></span> <span data-ttu-id="70e32-372">Operadores elevadas são construídos a partir de operadores predefinidos e definidos pelo usuário que atendem a certos requisitos, conforme descrito a seguir:</span><span class="sxs-lookup"><span data-stu-id="70e32-372">Lifted operators are constructed from predefined and user-defined operators that meet certain requirements, as described in the following:</span></span>

*   <span data-ttu-id="70e32-373">Para os operadores unários</span><span class="sxs-lookup"><span data-stu-id="70e32-373">For the unary operators</span></span>

    ```csharp
    +  ++  -  --  !  ~
    ```

    <span data-ttu-id="70e32-374">existe em um formato elevado de um operador se os tipos de operando e o resultado são os dois tipos de valor não anulável.</span><span class="sxs-lookup"><span data-stu-id="70e32-374">a lifted form of an operator exists if the operand and result types are both non-nullable value types.</span></span> <span data-ttu-id="70e32-375">O formulário elevado é construído com a adição de um único `?` modificador para os tipos de operando e resultado.</span><span class="sxs-lookup"><span data-stu-id="70e32-375">The lifted form is constructed by adding a single `?` modifier to the operand and result types.</span></span> <span data-ttu-id="70e32-376">O operador elevado produz um valor nulo se o operando for nulo.</span><span class="sxs-lookup"><span data-stu-id="70e32-376">The lifted operator produces a null value if the operand is null.</span></span> <span data-ttu-id="70e32-377">Caso contrário, o operador cancelado ou não, o operando, aplica o operador subjacente e encapsula o resultado.</span><span class="sxs-lookup"><span data-stu-id="70e32-377">Otherwise, the lifted operator unwraps the operand, applies the underlying operator, and wraps the result.</span></span>

*   <span data-ttu-id="70e32-378">Para os operadores binários</span><span class="sxs-lookup"><span data-stu-id="70e32-378">For the binary operators</span></span>

    ```csharp
    +  -  *  /  %  &  |  ^  <<  >>
    ```

    <span data-ttu-id="70e32-379">existe em um formato elevado de um operador se os tipos de operando e o resultado são todos os tipos de valor não anulável.</span><span class="sxs-lookup"><span data-stu-id="70e32-379">a lifted form of an operator exists if the operand and result types are all non-nullable value types.</span></span> <span data-ttu-id="70e32-380">O formulário elevado é construído com a adição de um único `?` modificador para cada tipo de operando e o resultado.</span><span class="sxs-lookup"><span data-stu-id="70e32-380">The lifted form is constructed by adding a single `?` modifier to each operand and result type.</span></span> <span data-ttu-id="70e32-381">O operador elevado produz um valor nulo se um ou ambos os operandos forem nulos (uma exceção que está sendo a `&` e `|` operadores da `bool?` de tipo, conforme descrito em [boolianos operadores lógicos](expressions.md#boolean-logical-operators)).</span><span class="sxs-lookup"><span data-stu-id="70e32-381">The lifted operator produces a null value if one or both operands are null (an exception being the `&` and `|` operators of the `bool?` type, as described in [Boolean logical operators](expressions.md#boolean-logical-operators)).</span></span> <span data-ttu-id="70e32-382">Caso contrário, o operador cancelado ou não, os operandos, aplica o operador subjacente e encapsula o resultado.</span><span class="sxs-lookup"><span data-stu-id="70e32-382">Otherwise, the lifted operator unwraps the operands, applies the underlying operator, and wraps the result.</span></span>

*   <span data-ttu-id="70e32-383">Para os operadores de igualdade</span><span class="sxs-lookup"><span data-stu-id="70e32-383">For the equality operators</span></span>

    ```csharp
    ==  !=
    ```

    <span data-ttu-id="70e32-384">um formulário elevado de um operador existe se os tipos de operando forem tipos de valor não anulável e se o tipo de resultado é `bool`.</span><span class="sxs-lookup"><span data-stu-id="70e32-384">a lifted form of an operator exists if the operand types are both non-nullable value types and if the result type is `bool`.</span></span> <span data-ttu-id="70e32-385">O formulário elevado é construído com a adição de um único `?` modificador para cada tipo de operando.</span><span class="sxs-lookup"><span data-stu-id="70e32-385">The lifted form is constructed by adding a single `?` modifier to each operand type.</span></span> <span data-ttu-id="70e32-386">O operador elevado considera iguais de dois valores nulos e um valor nulo desiguais para qualquer valor não nulo.</span><span class="sxs-lookup"><span data-stu-id="70e32-386">The lifted operator considers two null values equal, and a null value unequal to any non-null value.</span></span> <span data-ttu-id="70e32-387">Se ambos os operandos forem não nulos, o operador de cancelada ou não, os operandos e aplica o operador de base para produzir o `bool` resultado.</span><span class="sxs-lookup"><span data-stu-id="70e32-387">If both operands are non-null, the lifted operator unwraps the operands and applies the underlying operator to produce the `bool` result.</span></span>

*   <span data-ttu-id="70e32-388">Para os operadores relacionais</span><span class="sxs-lookup"><span data-stu-id="70e32-388">For the relational operators</span></span>

    ```csharp
    <  >  <=  >=
    ```

    <span data-ttu-id="70e32-389">um formulário elevado de um operador existe se os tipos de operando forem tipos de valor não anulável e se o tipo de resultado é `bool`.</span><span class="sxs-lookup"><span data-stu-id="70e32-389">a lifted form of an operator exists if the operand types are both non-nullable value types and if the result type is `bool`.</span></span> <span data-ttu-id="70e32-390">O formulário elevado é construído com a adição de um único `?` modificador para cada tipo de operando.</span><span class="sxs-lookup"><span data-stu-id="70e32-390">The lifted form is constructed by adding a single `?` modifier to each operand type.</span></span> <span data-ttu-id="70e32-391">O operador elevado produz o valor `false` se um ou ambos os operandos são nulos.</span><span class="sxs-lookup"><span data-stu-id="70e32-391">The lifted operator produces the value `false` if one or both operands are null.</span></span> <span data-ttu-id="70e32-392">Caso contrário, o operador cancelado ou não, os operandos e aplica o operador de base para produzir o `bool` resultado.</span><span class="sxs-lookup"><span data-stu-id="70e32-392">Otherwise, the lifted operator unwraps the operands and applies the underlying operator to produce the `bool` result.</span></span>

## <a name="member-lookup"></a><span data-ttu-id="70e32-393">Pesquisa de membro</span><span class="sxs-lookup"><span data-stu-id="70e32-393">Member lookup</span></span>

<span data-ttu-id="70e32-394">Uma pesquisa de membro é o processo pelo qual o significado de um nome no contexto de um tipo é determinado.</span><span class="sxs-lookup"><span data-stu-id="70e32-394">A member lookup is the process whereby the meaning of a name in the context of a type is determined.</span></span> <span data-ttu-id="70e32-395">Uma pesquisa de membro pode ocorrer como parte da avaliação de uma *simple_name* ([nomes simples](expressions.md#simple-names)) ou uma *member_access* ([acesso de membro](expressions.md#member-access)) em um expressão.</span><span class="sxs-lookup"><span data-stu-id="70e32-395">A member lookup can occur as part of evaluating a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)) in an expression.</span></span> <span data-ttu-id="70e32-396">Se o *simple_name* ou *member_access* ocorre como o *primary_expression* de uma *invocation_expression* ([ As invocações de método](expressions.md#method-invocations)), o membro deve ser invocado.</span><span class="sxs-lookup"><span data-stu-id="70e32-396">If the *simple_name* or *member_access* occurs as the *primary_expression* of an *invocation_expression* ([Method invocations](expressions.md#method-invocations)), the member is said to be invoked.</span></span>

<span data-ttu-id="70e32-397">Se um membro é um método ou evento, ou se for uma constante, campo ou propriedade de um tipo de delegado ([delegados](delegates.md)) ou o tipo `dynamic` ([tipo dinâmico](types.md#the-dynamic-type)), em seguida, o membro deve ser *invocable*.</span><span class="sxs-lookup"><span data-stu-id="70e32-397">If a member is a method or event, or if it is a constant, field or property of either a delegate type ([Delegates](delegates.md)) or the type `dynamic` ([The dynamic type](types.md#the-dynamic-type)), then the member is said to be *invocable*.</span></span>

<span data-ttu-id="70e32-398">Pesquisa de membro considera não apenas o nome de um membro, mas também o número de parâmetros de tipo que do membro e se o membro for acessível.</span><span class="sxs-lookup"><span data-stu-id="70e32-398">Member lookup considers not only the name of a member but also the number of type parameters the member has and whether the member is accessible.</span></span> <span data-ttu-id="70e32-399">Para fins de pesquisa de membro, métodos genéricos e tipos genéricos aninhados têm o número de parâmetros de tipo indicado em suas respectivas declarações e todos os outros membros têm zero parâmetros de tipo.</span><span class="sxs-lookup"><span data-stu-id="70e32-399">For the purposes of member lookup, generic methods and nested generic types have the number of type parameters indicated in their respective declarations and all other members have zero type parameters.</span></span>

<span data-ttu-id="70e32-400">Uma pesquisa de um nome de membro `N` com `K` parâmetros de tipo em um tipo `T` é processado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="70e32-400">A member lookup of a name `N` with `K` type parameters in a type `T` is processed as follows:</span></span>

*  <span data-ttu-id="70e32-401">Primeiro, um conjunto de membros acessíveis chamado `N` é determinado:</span><span class="sxs-lookup"><span data-stu-id="70e32-401">First, a set of accessible members named `N` is determined:</span></span>
    * <span data-ttu-id="70e32-402">Se `T` é um parâmetro de tipo, em seguida, o conjunto é a união dos conjuntos de membros acessíveis nomeados `N` em cada um dos tipos especificados como uma restrição primária ou restrição secundária ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)) para `T`, juntamente com o conjunto de membros acessíveis nomeados `N` em `object`.</span><span class="sxs-lookup"><span data-stu-id="70e32-402">If `T` is a type parameter, then the set is the union of the sets of accessible members named `N` in each of the types specified as a primary constraint or secondary constraint ([Type parameter constraints](classes.md#type-parameter-constraints)) for `T`, along with the set of accessible members named `N` in `object`.</span></span>
    * <span data-ttu-id="70e32-403">Caso contrário, o conjunto consiste em todos os acessível ([acesso de membro](basic-concepts.md#member-access)) membros nomeados `N` na `T`, incluindo membros herdados e os membros acessíveis nomeados `N` em `object`.</span><span class="sxs-lookup"><span data-stu-id="70e32-403">Otherwise, the set consists of all accessible ([Member access](basic-concepts.md#member-access)) members named `N` in `T`, including inherited members and the accessible members named `N` in `object`.</span></span> <span data-ttu-id="70e32-404">Se `T` é um tipo construído, o conjunto de membros é obtido, substituindo os argumentos de tipo, conforme descrito em [membros de tipos construídos](classes.md#members-of-constructed-types).</span><span class="sxs-lookup"><span data-stu-id="70e32-404">If `T` is a constructed type, the set of members is obtained by substituting type arguments as described in [Members of constructed types](classes.md#members-of-constructed-types).</span></span> <span data-ttu-id="70e32-405">Os membros que incluem um `override` modificador são excluídos do conjunto.</span><span class="sxs-lookup"><span data-stu-id="70e32-405">Members that include an `override` modifier are excluded from the set.</span></span>
*  <span data-ttu-id="70e32-406">Em seguida, se `K` for zero, todos os aninhados de tipos cujas declarações incluem parâmetros de tipo são removidos.</span><span class="sxs-lookup"><span data-stu-id="70e32-406">Next, if `K` is zero, all nested types whose declarations include type parameters are removed.</span></span> <span data-ttu-id="70e32-407">Se `K` for diferente de zero, todos os membros com um número diferente de tipo de parâmetros são removidos.</span><span class="sxs-lookup"><span data-stu-id="70e32-407">If `K` is not zero, all members with a different number of type parameters are removed.</span></span> <span data-ttu-id="70e32-408">Observe que, quando `K` é zero, métodos com parâmetros não forem removidos, desde que o processo de inferência de tipo do tipo ([inferência de tipo](expressions.md#type-inference)) pode ser capaz de inferir os argumentos de tipo.</span><span class="sxs-lookup"><span data-stu-id="70e32-408">Note that when `K` is zero, methods having type parameters are not removed, since the type inference process ([Type inference](expressions.md#type-inference)) might be able to infer the type arguments.</span></span>
*  <span data-ttu-id="70e32-409">Em seguida, se o membro é *invocado*, todos os não-*invocable* os membros são removidos do conjunto.</span><span class="sxs-lookup"><span data-stu-id="70e32-409">Next, if the member is *invoked*, all non-*invocable* members are removed from the set.</span></span>
*  <span data-ttu-id="70e32-410">Em seguida, os membros que estão ocultos por outros membros são removidos do conjunto.</span><span class="sxs-lookup"><span data-stu-id="70e32-410">Next, members that are hidden by other members are removed from the set.</span></span> <span data-ttu-id="70e32-411">Para cada membro `S.M` no conjunto, onde `S` é o tipo no qual o membro `M` for declarado, as seguintes regras são aplicadas:</span><span class="sxs-lookup"><span data-stu-id="70e32-411">For every member `S.M` in the set, where `S` is the type in which the member `M` is declared, the following rules are applied:</span></span>
    * <span data-ttu-id="70e32-412">Se `M` é uma constante, campo, propriedade, evento ou membro de enumeração, em seguida, todos os membros declarados em um tipo base do `S` são removidas do conjunto.</span><span class="sxs-lookup"><span data-stu-id="70e32-412">If `M` is a constant, field, property, event, or enumeration member, then all members declared in a base type of `S` are removed from the set.</span></span>
    * <span data-ttu-id="70e32-413">Se `M` é uma declaração de tipo e, em seguida, todos os tipos que não são declarados em um tipo base de `S` são removidas do conjunto, e todas as declarações com o mesmo número de parâmetros de tipo como de tipo `M` declarado em um tipo base do `S` são removidos do conjunto.</span><span class="sxs-lookup"><span data-stu-id="70e32-413">If `M` is a type declaration, then all non-types declared in a base type of `S` are removed from the set, and all type declarations with the same number of type parameters as `M` declared in a base type of `S` are removed from the set.</span></span>
    * <span data-ttu-id="70e32-414">Se `M` é um método, em seguida, todos os membros não-método declarado em um tipo base do `S` são removidas do conjunto.</span><span class="sxs-lookup"><span data-stu-id="70e32-414">If `M` is a method, then all non-method members declared in a base type of `S` are removed from the set.</span></span>
*  <span data-ttu-id="70e32-415">Em seguida, os membros de interface que ficam ocultos por membros de classe são removidos do conjunto.</span><span class="sxs-lookup"><span data-stu-id="70e32-415">Next, interface members that are hidden by class members are removed from the set.</span></span> <span data-ttu-id="70e32-416">Esta etapa só terá efeito se `T` é um parâmetro de tipo e `T` tem os dois uma classe base efetivada diferente de `object` e uma interface de efetivada vazio definido ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="70e32-416">This step only has an effect if `T` is a type parameter and `T` has both an effective base class other than `object` and a non-empty effective interface set ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="70e32-417">Para cada membro `S.M` no conjunto, onde `S` é o tipo no qual o membro `M` for declarado, as seguintes regras são aplicadas se `S` é uma declaração de classe diferente de `object`:</span><span class="sxs-lookup"><span data-stu-id="70e32-417">For every member `S.M` in the set, where `S` is the type in which the member `M` is declared, the following rules are applied if `S` is a class declaration other than `object`:</span></span>
    * <span data-ttu-id="70e32-418">Se `M` é uma constante, campo, propriedade, evento, membro de enumeração ou declaração de tipo, em seguida, todos os membros declarados na declaração de interface são removidos do conjunto.</span><span class="sxs-lookup"><span data-stu-id="70e32-418">If `M` is a constant, field, property, event, enumeration member, or type declaration, then all members declared in an interface declaration are removed from the set.</span></span>
    * <span data-ttu-id="70e32-419">Se `M` é um método, em seguida, todos os membros não-método declarados em uma declaração de interface são removidos do conjunto e todos os métodos com a mesma assinatura que `M` declarado em uma interface de declaração são removidos do conjunto.</span><span class="sxs-lookup"><span data-stu-id="70e32-419">If `M` is a method, then all non-method members declared in an interface declaration are removed from the set, and all methods with the same signature as `M` declared in an interface declaration are removed from the set.</span></span>
*  <span data-ttu-id="70e32-420">Por fim, o resultado da pesquisa de ter removido a membros ocultos, é determinado:</span><span class="sxs-lookup"><span data-stu-id="70e32-420">Finally, having removed hidden members, the result of the lookup is determined:</span></span>
    * <span data-ttu-id="70e32-421">Se o conjunto consiste em um único membro que não é um método, esse membro é o resultado da pesquisa.</span><span class="sxs-lookup"><span data-stu-id="70e32-421">If the set consists of a single member that is not a method, then this member is the result of the lookup.</span></span>
    * <span data-ttu-id="70e32-422">Caso contrário, se o conjunto contém apenas os métodos, esse grupo de métodos é o resultado da pesquisa.</span><span class="sxs-lookup"><span data-stu-id="70e32-422">Otherwise, if the set contains only methods, then this group of methods is the result of the lookup.</span></span>
    * <span data-ttu-id="70e32-423">Caso contrário, a pesquisa é ambígua e ocorre um erro em tempo de vinculação.</span><span class="sxs-lookup"><span data-stu-id="70e32-423">Otherwise, the lookup is ambiguous, and a binding-time error occurs.</span></span>

<span data-ttu-id="70e32-424">Para pesquisas de membro em tipos diferentes de parâmetros de tipo e interfaces e pesquisas de membro em interfaces que são estritamente herança única (tem exatamente zero ou uma interface direta da base de cada interface na cadeia de herança), é o efeito das regras de pesquisa simplesmente que derivado membros ocultar membros base com o mesmo nome ou assinatura.</span><span class="sxs-lookup"><span data-stu-id="70e32-424">For member lookups in types other than type parameters and interfaces, and member lookups in interfaces that are strictly single-inheritance (each interface in the inheritance chain has exactly zero or one direct base interface), the effect of the lookup rules is simply that derived members hide base members with the same name or signature.</span></span> <span data-ttu-id="70e32-425">Essas pesquisas de herança única nunca são ambíguas.</span><span class="sxs-lookup"><span data-stu-id="70e32-425">Such single-inheritance lookups are never ambiguous.</span></span> <span data-ttu-id="70e32-426">As ambiguidades que possivelmente podem surgir de pesquisas de membro em interfaces de herança múltipla são descritas em [acesso de membro de Interface](interfaces.md#interface-member-access).</span><span class="sxs-lookup"><span data-stu-id="70e32-426">The ambiguities that can possibly arise from member lookups in multiple-inheritance interfaces are described in [Interface member access](interfaces.md#interface-member-access).</span></span>

### <a name="base-types"></a><span data-ttu-id="70e32-427">Tipos base</span><span class="sxs-lookup"><span data-stu-id="70e32-427">Base types</span></span>

<span data-ttu-id="70e32-428">Para fins de pesquisa de membro, um tipo `T` é considerado como tendo os seguintes tipos de base:</span><span class="sxs-lookup"><span data-stu-id="70e32-428">For purposes of member lookup, a type `T` is considered to have the following base types:</span></span>

*  <span data-ttu-id="70e32-429">Se `T` está `object`, em seguida, `T` não tem nenhum tipo de base.</span><span class="sxs-lookup"><span data-stu-id="70e32-429">If `T` is `object`, then `T` has no base type.</span></span>
*  <span data-ttu-id="70e32-430">Se `T` é um *enum_type*, os tipos base do `T` são os tipos de classe `System.Enum`, `System.ValueType`, e `object`.</span><span class="sxs-lookup"><span data-stu-id="70e32-430">If `T` is an *enum_type*, the base types of `T` are the class types `System.Enum`, `System.ValueType`, and `object`.</span></span>
*  <span data-ttu-id="70e32-431">Se `T` é um *struct_type*, os tipos base do `T` são os tipos de classe `System.ValueType` e `object`.</span><span class="sxs-lookup"><span data-stu-id="70e32-431">If `T` is a *struct_type*, the base types of `T` are the class types `System.ValueType` and `object`.</span></span>
*  <span data-ttu-id="70e32-432">Se `T` é um *class_type*, os tipos base do `T` são as classes base `T`, incluindo o tipo de classe `object`.</span><span class="sxs-lookup"><span data-stu-id="70e32-432">If `T` is a *class_type*, the base types of `T` are the base classes of `T`, including the class type `object`.</span></span>
*  <span data-ttu-id="70e32-433">Se `T` é um *interface_type*, os tipos base do `T` são as interfaces base dos `T` e o tipo de classe `object`.</span><span class="sxs-lookup"><span data-stu-id="70e32-433">If `T` is an *interface_type*, the base types of `T` are the base interfaces of `T` and the class type `object`.</span></span>
*  <span data-ttu-id="70e32-434">Se `T` é um *array_type*, os tipos base do `T` são os tipos de classe `System.Array` e `object`.</span><span class="sxs-lookup"><span data-stu-id="70e32-434">If `T` is an *array_type*, the base types of `T` are the class types `System.Array` and `object`.</span></span>
*  <span data-ttu-id="70e32-435">Se `T` é um *delegate_type*, os tipos base do `T` são os tipos de classe `System.Delegate` e `object`.</span><span class="sxs-lookup"><span data-stu-id="70e32-435">If `T` is a *delegate_type*, the base types of `T` are the class types `System.Delegate` and `object`.</span></span>

## <a name="function-members"></a><span data-ttu-id="70e32-436">Membros da função</span><span class="sxs-lookup"><span data-stu-id="70e32-436">Function members</span></span>

<span data-ttu-id="70e32-437">Membros da função são membros que contêm instruções executáveis.</span><span class="sxs-lookup"><span data-stu-id="70e32-437">Function members are members that contain executable statements.</span></span> <span data-ttu-id="70e32-438">Membros da função sempre são membros de tipos e não podem ser membros de namespaces.</span><span class="sxs-lookup"><span data-stu-id="70e32-438">Function members are always members of types and cannot be members of namespaces.</span></span> <span data-ttu-id="70e32-439">C# define as seguintes categorias de membros da função:</span><span class="sxs-lookup"><span data-stu-id="70e32-439">C# defines the following categories of function members:</span></span>

*  <span data-ttu-id="70e32-440">Métodos</span><span class="sxs-lookup"><span data-stu-id="70e32-440">Methods</span></span>
*  <span data-ttu-id="70e32-441">Propriedades</span><span class="sxs-lookup"><span data-stu-id="70e32-441">Properties</span></span>
*  <span data-ttu-id="70e32-442">Eventos</span><span class="sxs-lookup"><span data-stu-id="70e32-442">Events</span></span>
*  <span data-ttu-id="70e32-443">Indexadores</span><span class="sxs-lookup"><span data-stu-id="70e32-443">Indexers</span></span>
*  <span data-ttu-id="70e32-444">Operadores definidos pelo usuário</span><span class="sxs-lookup"><span data-stu-id="70e32-444">User-defined operators</span></span>
*  <span data-ttu-id="70e32-445">Construtores de instância</span><span class="sxs-lookup"><span data-stu-id="70e32-445">Instance constructors</span></span>
*  <span data-ttu-id="70e32-446">Construtores estáticos</span><span class="sxs-lookup"><span data-stu-id="70e32-446">Static constructors</span></span>
*  <span data-ttu-id="70e32-447">Destruidores</span><span class="sxs-lookup"><span data-stu-id="70e32-447">Destructors</span></span>

<span data-ttu-id="70e32-448">Exceto para os destruidores e construtores estáticos (que não podem ser invocados explicitamente), as instruções contidas em membros de função são executadas por meio de invocações de função de membro.</span><span class="sxs-lookup"><span data-stu-id="70e32-448">Except for destructors and static constructors (which cannot be invoked explicitly), the statements contained in function members are executed through function member invocations.</span></span> <span data-ttu-id="70e32-449">A sintaxe real para a gravação de uma invocação de membro de função depende a categoria de membro de função específica.</span><span class="sxs-lookup"><span data-stu-id="70e32-449">The actual syntax for writing a function member invocation depends on the particular function member category.</span></span>

<span data-ttu-id="70e32-450">A lista de argumentos ([listas de argumentos](expressions.md#argument-lists)) de um membro da função chamada fornece os valores reais ou referências de variável para os parâmetros do membro da função.</span><span class="sxs-lookup"><span data-stu-id="70e32-450">The argument list ([Argument lists](expressions.md#argument-lists)) of a function member invocation provides actual values or variable references for the parameters of the function member.</span></span>

<span data-ttu-id="70e32-451">As invocações de métodos genéricos podem empregar a inferência de tipo para determinar o conjunto de argumentos de tipo para passar para o método.</span><span class="sxs-lookup"><span data-stu-id="70e32-451">Invocations of generic methods may employ type inference to determine the set of type arguments to pass to the method.</span></span> <span data-ttu-id="70e32-452">Esse processo é descrito em [inferência de tipo](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="70e32-452">This process is described in [Type inference](expressions.md#type-inference).</span></span>

<span data-ttu-id="70e32-453">As invocações de métodos, indexadores, operadores e construtores de instância empregam a resolução de sobrecarga para determinar quais de um conjunto de candidatos de membros da função para invocar.</span><span class="sxs-lookup"><span data-stu-id="70e32-453">Invocations of methods, indexers, operators and instance constructors employ overload resolution to determine which of a candidate set of function members to invoke.</span></span> <span data-ttu-id="70e32-454">Esse processo é descrito em [resolução de sobrecarga](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="70e32-454">This process is described in [Overload resolution](expressions.md#overload-resolution).</span></span>

<span data-ttu-id="70e32-455">Depois que um membro da função específica tiver sido identificado em tempo de associação, possivelmente por meio da resolução de sobrecarga, o processo de tempo de execução real de invocar o membro da função é descrito no [verificação de resolução de sobrecarga dinâmicostempodecompilação](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="70e32-455">Once a particular function member has been identified at binding-time, possibly through overload resolution, the actual run-time process of invoking the function member is described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="70e32-456">A tabela a seguir resume o processamento que ocorre em construções que envolvem as seis categorias de membros da função que podem ser chamados explicitamente.</span><span class="sxs-lookup"><span data-stu-id="70e32-456">The following table summarizes the processing that takes place in constructs involving the six categories of function members that can be explicitly invoked.</span></span> <span data-ttu-id="70e32-457">Na tabela, `e`, `x`, `y`, e `value` indicar classificadas como variáveis ou valores, as expressões `T` indica uma expressão classificada como um tipo, `F` é o nome simple de um método e `P` é o nome simple de uma propriedade.</span><span class="sxs-lookup"><span data-stu-id="70e32-457">In the table, `e`, `x`, `y`, and `value` indicate expressions classified as variables or values, `T` indicates an expression classified as a type, `F` is the simple name of a method, and `P` is the simple name of a property.</span></span>


| <span data-ttu-id="70e32-458">__Construir__</span><span class="sxs-lookup"><span data-stu-id="70e32-458">__Construct__</span></span>     | <span data-ttu-id="70e32-459">__Exemplo__</span><span class="sxs-lookup"><span data-stu-id="70e32-459">__Example__</span></span>    | <span data-ttu-id="70e32-460">__Descrição__</span><span class="sxs-lookup"><span data-stu-id="70e32-460">__Description__</span></span> |
|-------------------|----------------|-----------------|
| <span data-ttu-id="70e32-461">Invocação de método</span><span class="sxs-lookup"><span data-stu-id="70e32-461">Method invocation</span></span> | `F(x,y)`       | <span data-ttu-id="70e32-462">Resolução de sobrecarga é aplicada para selecionar o melhor método `F` na classe ou struct recipiente.</span><span class="sxs-lookup"><span data-stu-id="70e32-462">Overload resolution is applied to select the best method `F` in the containing class or struct.</span></span> <span data-ttu-id="70e32-463">O método é invocado com a lista de argumentos `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="70e32-463">The method is invoked with the argument list `(x,y)`.</span></span> <span data-ttu-id="70e32-464">Se o método não for `static`, a expressão de instância é `this`.</span><span class="sxs-lookup"><span data-stu-id="70e32-464">If the method is not `static`, the instance expression is `this`.</span></span> | 
|                   | `T.F(x,y)`     | <span data-ttu-id="70e32-465">Resolução de sobrecarga é aplicada para selecionar o melhor método de `F` na classe ou struct `T`.</span><span class="sxs-lookup"><span data-stu-id="70e32-465">Overload resolution is applied to select the best method `F` in the class or struct `T`.</span></span> <span data-ttu-id="70e32-466">Um erro em tempo de vinculação ocorrerá se o método não é `static`.</span><span class="sxs-lookup"><span data-stu-id="70e32-466">A binding-time error occurs if the method is not `static`.</span></span> <span data-ttu-id="70e32-467">O método é invocado com a lista de argumentos `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="70e32-467">The method is invoked with the argument list `(x,y)`.</span></span> | 
|                   | `e.F(x,y)`     | <span data-ttu-id="70e32-468">Resolução de sobrecarga é aplicada para selecionar o melhor método F na classe, struct ou interface fornecido pelo tipo de `e`.</span><span class="sxs-lookup"><span data-stu-id="70e32-468">Overload resolution is applied to select the best method F in the class, struct, or interface given by the type of `e`.</span></span> <span data-ttu-id="70e32-469">Um erro em tempo de vinculação ocorrerá se o método é `static`.</span><span class="sxs-lookup"><span data-stu-id="70e32-469">A binding-time error occurs if the method is `static`.</span></span> <span data-ttu-id="70e32-470">O método é invocado com a expressão de instância `e` e a lista de argumentos `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="70e32-470">The method is invoked with the instance expression `e` and the argument list `(x,y)`.</span></span> | 
| <span data-ttu-id="70e32-471">Acesso de propriedade</span><span class="sxs-lookup"><span data-stu-id="70e32-471">Property access</span></span>   | `P`            | <span data-ttu-id="70e32-472">O `get` acessador da propriedade `P` na classe ou struct recipiente é invocado.</span><span class="sxs-lookup"><span data-stu-id="70e32-472">The `get` accessor of the property `P` in the containing class or struct is invoked.</span></span> <span data-ttu-id="70e32-473">Ocorrerá um erro de tempo de compilação se `P` é somente gravação.</span><span class="sxs-lookup"><span data-stu-id="70e32-473">A compile-time error occurs if `P` is write-only.</span></span> <span data-ttu-id="70e32-474">Se `P` não é `static`, a expressão de instância é `this`.</span><span class="sxs-lookup"><span data-stu-id="70e32-474">If `P` is not `static`, the instance expression is `this`.</span></span> | 
|                   | `P = value`    | <span data-ttu-id="70e32-475">O `set` acessador da propriedade `P` na classe ou struct recipiente é invocado com a lista de argumentos `(value)`.</span><span class="sxs-lookup"><span data-stu-id="70e32-475">The `set` accessor of the property `P` in the containing class or struct is invoked with the argument list `(value)`.</span></span> <span data-ttu-id="70e32-476">Ocorrerá um erro de tempo de compilação se `P` é somente leitura.</span><span class="sxs-lookup"><span data-stu-id="70e32-476">A compile-time error occurs if `P` is read-only.</span></span> <span data-ttu-id="70e32-477">Se `P` não é `static`, a expressão de instância é `this`.</span><span class="sxs-lookup"><span data-stu-id="70e32-477">If `P` is not `static`, the instance expression is `this`.</span></span> | 
|                   | `T.P`          | <span data-ttu-id="70e32-478">O `get` acessador da propriedade `P` na classe ou struct `T` é invocado.</span><span class="sxs-lookup"><span data-stu-id="70e32-478">The `get` accessor of the property `P` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="70e32-479">Ocorrerá um erro de tempo de compilação se `P` não é `static` ou se `P` é somente gravação.</span><span class="sxs-lookup"><span data-stu-id="70e32-479">A compile-time error occurs if `P` is not `static` or if `P` is write-only.</span></span> | 
|                   | `T.P = value`  | <span data-ttu-id="70e32-480">O `set` acessador da propriedade `P` na classe ou struct `T` é invocado com a lista de argumentos `(value)`.</span><span class="sxs-lookup"><span data-stu-id="70e32-480">The `set` accessor of the property `P` in the class or struct `T` is invoked with the argument list `(value)`.</span></span> <span data-ttu-id="70e32-481">Ocorrerá um erro de tempo de compilação se `P` não é `static` ou se `P` é somente leitura.</span><span class="sxs-lookup"><span data-stu-id="70e32-481">A compile-time error occurs if `P` is not `static` or if `P` is read-only.</span></span> | 
|                   | `e.P`          | <span data-ttu-id="70e32-482">O `get` acessador da propriedade `P` na classe, struct ou interface fornecido pelo tipo de `e` é invocado com a expressão de instância `e`.</span><span class="sxs-lookup"><span data-stu-id="70e32-482">The `get` accessor of the property `P` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="70e32-483">Um erro em tempo de vinculação ocorrerá se `P` está `static` ou se `P` é somente gravação.</span><span class="sxs-lookup"><span data-stu-id="70e32-483">A binding-time error occurs if `P` is `static` or if `P` is write-only.</span></span> | 
|                   | `e.P = value`  | <span data-ttu-id="70e32-484">O `set` acessador da propriedade `P` na classe, struct ou interface fornecido pelo tipo de `e` é invocado com a expressão de instância `e` e a lista de argumentos `(value)`.</span><span class="sxs-lookup"><span data-stu-id="70e32-484">The `set` accessor of the property `P` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e` and the argument list `(value)`.</span></span> <span data-ttu-id="70e32-485">Um erro em tempo de vinculação ocorrerá se `P` está `static` ou se `P` é somente leitura.</span><span class="sxs-lookup"><span data-stu-id="70e32-485">A binding-time error occurs if `P` is `static` or if `P` is read-only.</span></span> | 
| <span data-ttu-id="70e32-486">Acesso ao evento</span><span class="sxs-lookup"><span data-stu-id="70e32-486">Event access</span></span>      | `E += value`   | <span data-ttu-id="70e32-487">O `add` acessador do evento `E` na classe ou struct recipiente é invocado.</span><span class="sxs-lookup"><span data-stu-id="70e32-487">The `add` accessor of the event `E` in the containing class or struct is invoked.</span></span> <span data-ttu-id="70e32-488">Se `E` é não estático, a expressão de instância é `this`.</span><span class="sxs-lookup"><span data-stu-id="70e32-488">If `E` is not static, the instance expression is `this`.</span></span> | 
|                   | `E -= value`   | <span data-ttu-id="70e32-489">O `remove` acessador do evento `E` na classe ou struct recipiente é invocado.</span><span class="sxs-lookup"><span data-stu-id="70e32-489">The `remove` accessor of the event `E` in the containing class or struct is invoked.</span></span> <span data-ttu-id="70e32-490">Se `E` é não estático, a expressão de instância é `this`.</span><span class="sxs-lookup"><span data-stu-id="70e32-490">If `E` is not static, the instance expression is `this`.</span></span> | 
|                   | `T.E += value` | <span data-ttu-id="70e32-491">O `add` acessador do evento `E` na classe ou struct `T` é invocado.</span><span class="sxs-lookup"><span data-stu-id="70e32-491">The `add` accessor of the event `E` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="70e32-492">Ocorrerá um erro de tempo de associação se `E` não é estático.</span><span class="sxs-lookup"><span data-stu-id="70e32-492">A binding-time error occurs if `E` is not static.</span></span> | 
|                   | `T.E -= value` | <span data-ttu-id="70e32-493">O `remove` acessador do evento `E` na classe ou struct `T` é invocado.</span><span class="sxs-lookup"><span data-stu-id="70e32-493">The `remove` accessor of the event `E` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="70e32-494">Ocorrerá um erro de tempo de associação se `E` não é estático.</span><span class="sxs-lookup"><span data-stu-id="70e32-494">A binding-time error occurs if `E` is not static.</span></span> | 
|                   | `e.E += value` | <span data-ttu-id="70e32-495">O `add` acessador do evento `E` na classe, struct ou interface fornecido pelo tipo de `e` é invocado com a expressão de instância `e`.</span><span class="sxs-lookup"><span data-stu-id="70e32-495">The `add` accessor of the event `E` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="70e32-496">Ocorrerá um erro de tempo de associação se `E` é estático.</span><span class="sxs-lookup"><span data-stu-id="70e32-496">A binding-time error occurs if `E` is static.</span></span> | 
|                   | `e.E -= value` | <span data-ttu-id="70e32-497">O `remove` acessador do evento `E` na classe, struct ou interface fornecido pelo tipo de `e` é invocado com a expressão de instância `e`.</span><span class="sxs-lookup"><span data-stu-id="70e32-497">The `remove` accessor of the event `E` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="70e32-498">Ocorrerá um erro de tempo de associação se `E` é estático.</span><span class="sxs-lookup"><span data-stu-id="70e32-498">A binding-time error occurs if `E` is static.</span></span> | 
| <span data-ttu-id="70e32-499">Acesso do indexador</span><span class="sxs-lookup"><span data-stu-id="70e32-499">Indexer access</span></span>    | `e[x,y]`       | <span data-ttu-id="70e32-500">Resolução de sobrecarga é aplicada para selecionar o indexador melhor na classe, struct ou interface fornecido pelo tipo de e.</span><span class="sxs-lookup"><span data-stu-id="70e32-500">Overload resolution is applied to select the best indexer in the class, struct, or interface given by the type of e.</span></span> <span data-ttu-id="70e32-501">O `get` acessador do indexador é invocado com a expressão de instância `e` e a lista de argumentos `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="70e32-501">The `get` accessor of the indexer is invoked with the instance expression `e` and the argument list `(x,y)`.</span></span> <span data-ttu-id="70e32-502">Um erro em tempo de vinculação ocorrerá se o indexador é somente gravação.</span><span class="sxs-lookup"><span data-stu-id="70e32-502">A binding-time error occurs if the indexer is write-only.</span></span> | 
|                   | `e[x,y] = value` | <span data-ttu-id="70e32-503">Resolução de sobrecarga é aplicada para selecionar o indexador melhor na classe, struct ou interface fornecido pelo tipo de `e`.</span><span class="sxs-lookup"><span data-stu-id="70e32-503">Overload resolution is applied to select the best indexer in the class, struct, or interface given by the type of `e`.</span></span> <span data-ttu-id="70e32-504">O `set` acessador do indexador é invocado com a expressão de instância `e` e a lista de argumentos `(x,y,value)`.</span><span class="sxs-lookup"><span data-stu-id="70e32-504">The `set` accessor of the indexer is invoked with the instance expression `e` and the argument list `(x,y,value)`.</span></span> <span data-ttu-id="70e32-505">Um erro em tempo de vinculação ocorrerá se o indexador é somente leitura.</span><span class="sxs-lookup"><span data-stu-id="70e32-505">A binding-time error occurs if the indexer is read-only.</span></span> | 
| <span data-ttu-id="70e32-506">Invocação de operador</span><span class="sxs-lookup"><span data-stu-id="70e32-506">Operator invocation</span></span> | `-x`         | <span data-ttu-id="70e32-507">Resolução de sobrecarga é aplicada para selecionar o operador unário melhor na classe ou struct fornecido pelo tipo de `x`.</span><span class="sxs-lookup"><span data-stu-id="70e32-507">Overload resolution is applied to select the best unary operator in the class or struct given by the type of `x`.</span></span> <span data-ttu-id="70e32-508">O operador selecionado é invocado com a lista de argumentos `(x)`.</span><span class="sxs-lookup"><span data-stu-id="70e32-508">The selected operator is invoked with the argument list `(x)`.</span></span> | 
|                     | `x + y`      | <span data-ttu-id="70e32-509">Resolução de sobrecarga é aplicada para selecionar o operador binário melhor nas classes ou structs fornecido pelos tipos de `x` e `y`.</span><span class="sxs-lookup"><span data-stu-id="70e32-509">Overload resolution is applied to select the best binary operator in the classes or structs given by the types of `x` and `y`.</span></span> <span data-ttu-id="70e32-510">O operador selecionado é invocado com a lista de argumentos `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="70e32-510">The selected operator is invoked with the argument list `(x,y)`.</span></span> | 
| <span data-ttu-id="70e32-511">Invocação do construtor de instância</span><span class="sxs-lookup"><span data-stu-id="70e32-511">Instance constructor invocation</span></span> | `new T(x,y)` | <span data-ttu-id="70e32-512">Resolução de sobrecarga é aplicada para selecionar o melhor construtor de instância na classe ou struct `T`.</span><span class="sxs-lookup"><span data-stu-id="70e32-512">Overload resolution is applied to select the best instance constructor in the class or struct `T`.</span></span> <span data-ttu-id="70e32-513">O construtor de instância é invocado com a lista de argumentos `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="70e32-513">The instance constructor is invoked with the argument list `(x,y)`.</span></span> | 

### <a name="argument-lists"></a><span data-ttu-id="70e32-514">Listas de argumentos</span><span class="sxs-lookup"><span data-stu-id="70e32-514">Argument lists</span></span>

<span data-ttu-id="70e32-515">Cada invocação de membro e o delegado de função inclui uma lista de argumentos que fornece os valores reais ou referências de variável para os parâmetros do membro da função.</span><span class="sxs-lookup"><span data-stu-id="70e32-515">Every function member and delegate invocation includes an argument list which provides actual values or variable references for the parameters of the function member.</span></span> <span data-ttu-id="70e32-516">A sintaxe para especificar a lista de argumentos de uma invocação de membro de função depende a categoria de membro da função:</span><span class="sxs-lookup"><span data-stu-id="70e32-516">The syntax for specifying the argument list of a function member invocation depends on the function member category:</span></span>

*  <span data-ttu-id="70e32-517">Por exemplo construtores, métodos, indexadores e delegados, os argumentos são especificados como um *argument_list*, conforme descrito abaixo.</span><span class="sxs-lookup"><span data-stu-id="70e32-517">For instance constructors, methods, indexers and delegates, the arguments are specified as an *argument_list*, as described below.</span></span> <span data-ttu-id="70e32-518">Para indexadores, ao invocar o `set` acessador, a lista de argumentos Além disso, inclui a expressão especificada como o operando à direita do operador de atribuição.</span><span class="sxs-lookup"><span data-stu-id="70e32-518">For indexers, when invoking the `set` accessor, the argument list additionally includes the expression specified as the right operand of the assignment operator.</span></span>
*  <span data-ttu-id="70e32-519">Para propriedades, a lista de argumentos está vazia ao invocar o `get` acessador e consiste na expressão especificada como o operando à direita do operador de atribuição ao invocar o `set` acessador.</span><span class="sxs-lookup"><span data-stu-id="70e32-519">For properties, the argument list is empty when invoking the `get` accessor, and consists of the expression specified as the right operand of the assignment operator when invoking the `set` accessor.</span></span>
*  <span data-ttu-id="70e32-520">Para eventos, a lista de argumentos é formada pela expressão especificada como o operando direito dos `+=` ou `-=` operador.</span><span class="sxs-lookup"><span data-stu-id="70e32-520">For events, the argument list consists of the expression specified as the right operand of the `+=` or `-=` operator.</span></span>
*  <span data-ttu-id="70e32-521">Operadores definidos pelo usuário, a lista de argumentos consiste em único operando do operador unário ou os dois operandos do operador binário.</span><span class="sxs-lookup"><span data-stu-id="70e32-521">For user-defined operators, the argument list consists of the single operand of the unary operator or the two operands of the binary operator.</span></span>

<span data-ttu-id="70e32-522">Os argumentos de propriedades ([propriedades](classes.md#properties)), eventos ([eventos](classes.md#events)) e operadores definidos pelo usuário ([operadores](classes.md#operators)) sempre são passados como parâmetros de valor ([ Parâmetros de valores](classes.md#value-parameters)).</span><span class="sxs-lookup"><span data-stu-id="70e32-522">The arguments of properties ([Properties](classes.md#properties)), events ([Events](classes.md#events)), and user-defined operators ([Operators](classes.md#operators)) are always passed as value parameters ([Value parameters](classes.md#value-parameters)).</span></span> <span data-ttu-id="70e32-523">Os argumentos de indexadores ([indexadores](classes.md#indexers)) sempre são passados como parâmetros de valor ([parâmetros de valores](classes.md#value-parameters)) ou matrizes de parâmetros ([matrizes de parâmetro](classes.md#parameter-arrays)).</span><span class="sxs-lookup"><span data-stu-id="70e32-523">The arguments of indexers ([Indexers](classes.md#indexers)) are always passed as value parameters ([Value parameters](classes.md#value-parameters)) or parameter arrays ([Parameter arrays](classes.md#parameter-arrays)).</span></span> <span data-ttu-id="70e32-524">Não há suporte para parâmetros de referência e de saída para essas categorias de membros da função.</span><span class="sxs-lookup"><span data-stu-id="70e32-524">Reference and output parameters are not supported for these categories of function members.</span></span>

<span data-ttu-id="70e32-525">Os argumentos de uma invocação de construtor, método, indexador ou delegado de instância são especificados como um *argument_list*:</span><span class="sxs-lookup"><span data-stu-id="70e32-525">The arguments of an instance constructor, method, indexer or delegate invocation are specified as an *argument_list*:</span></span>

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

<span data-ttu-id="70e32-526">Uma *argument_list* consiste em um ou mais *argumento*s, separados por vírgulas.</span><span class="sxs-lookup"><span data-stu-id="70e32-526">An *argument_list* consists of one or more *argument*s, separated by commas.</span></span> <span data-ttu-id="70e32-527">Cada argumento consiste em um recurso opcional *argument_name* seguido por um *argument_value*.</span><span class="sxs-lookup"><span data-stu-id="70e32-527">Each argument consists of an optional  *argument_name* followed by an *argument_value*.</span></span> <span data-ttu-id="70e32-528">Uma *argumento* com um *argument_name* é conhecido como um ***argumento nomeado***, enquanto um *argumento* sem um  *argument_name* é um ***argumento posicional***.</span><span class="sxs-lookup"><span data-stu-id="70e32-528">An *argument* with an *argument_name* is referred to as a ***named argument***, whereas an *argument* without an *argument_name* is a ***positional argument***.</span></span> <span data-ttu-id="70e32-529">É um erro para um argumento posicional aparecer após um argumento nomeado em uma *argument_list*.</span><span class="sxs-lookup"><span data-stu-id="70e32-529">It is an error for a positional argument to appear after a named argument in an *argument_list*.</span></span>

<span data-ttu-id="70e32-530">O *argument_value* pode assumir um dos seguintes formatos:</span><span class="sxs-lookup"><span data-stu-id="70e32-530">The *argument_value* can take one of the following forms:</span></span>

*  <span data-ttu-id="70e32-531">Uma *expressão*, indicando que o argumento é passado como um parâmetro de valor ([parâmetros de valores](classes.md#value-parameters)).</span><span class="sxs-lookup"><span data-stu-id="70e32-531">An *expression*, indicating that the argument is passed as a value parameter ([Value parameters](classes.md#value-parameters)).</span></span>
*  <span data-ttu-id="70e32-532">A palavra-chave `ref` seguido por um *variable_reference* ([referências variáveis](variables.md#variable-references)), indicando que o argumento é passado como um parâmetro de referência ([fazer referência a parâmetros ](classes.md#reference-parameters)).</span><span class="sxs-lookup"><span data-stu-id="70e32-532">The keyword `ref` followed by a *variable_reference* ([Variable references](variables.md#variable-references)), indicating that the argument is passed as a reference parameter ([Reference parameters](classes.md#reference-parameters)).</span></span> <span data-ttu-id="70e32-533">Uma variável deve ser definitivamente atribuída ([atribuição definitiva](variables.md#definite-assignment)) antes que ele pode ser passado como um parâmetro de referência.</span><span class="sxs-lookup"><span data-stu-id="70e32-533">A variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) before it can be passed as a reference parameter.</span></span> <span data-ttu-id="70e32-534">A palavra-chave `out` seguido por um *variable_reference* ([referências variáveis](variables.md#variable-references)), indicando que o argumento é passado como um parâmetro de saída ([deparâmetrosdesaída](classes.md#output-parameters)).</span><span class="sxs-lookup"><span data-stu-id="70e32-534">The keyword `out` followed by a *variable_reference* ([Variable references](variables.md#variable-references)), indicating that the argument is passed as an output parameter ([Output parameters](classes.md#output-parameters)).</span></span> <span data-ttu-id="70e32-535">Uma variável é considerada atribuída definitivamente ([atribuição definitiva](variables.md#definite-assignment)) após uma invocação de membro de função em que a variável é passada como um parâmetro de saída.</span><span class="sxs-lookup"><span data-stu-id="70e32-535">A variable is considered definitely assigned ([Definite assignment](variables.md#definite-assignment)) following a function member invocation in which the variable is passed as an output parameter.</span></span>

#### <a name="corresponding-parameters"></a><span data-ttu-id="70e32-536">Parâmetros correspondentes</span><span class="sxs-lookup"><span data-stu-id="70e32-536">Corresponding parameters</span></span>

<span data-ttu-id="70e32-537">Para cada argumento na lista de argumentos deve ser um parâmetro correspondente no membro da função ou delegado que está sendo invocado.</span><span class="sxs-lookup"><span data-stu-id="70e32-537">For each argument in an argument list there has to be a corresponding parameter in the function member or delegate being invoked.</span></span>

<span data-ttu-id="70e32-538">A lista de parâmetros usada no seguinte é determinada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="70e32-538">The parameter list used in the following is determined as follows:</span></span>

*  <span data-ttu-id="70e32-539">Para indexadores definidos nas classes e métodos virtuais, a lista de parâmetros separado da declaração mais específica ou substituição do membro da função, começando com o tipo estático do receptor e a pesquisa por meio de suas classes base.</span><span class="sxs-lookup"><span data-stu-id="70e32-539">For virtual methods and indexers defined in classes, the parameter list is picked from the most specific declaration or override of the function member, starting with the static type of the receiver, and searching through its base classes.</span></span>
*  <span data-ttu-id="70e32-540">Para métodos de interface e indexadores, a lista de parâmetros é escolhida formam a definição mais específica do membro, começando com o tipo de interface e a pesquisa por meio de interfaces base.</span><span class="sxs-lookup"><span data-stu-id="70e32-540">For interface methods and indexers, the parameter list is picked form the most specific definition of the member, starting with the interface type and searching through the base interfaces.</span></span> <span data-ttu-id="70e32-541">Se nenhuma lista de parâmetro exclusivo é encontrado, uma lista de parâmetros com nomes inacessíveis e sem parâmetros opcionais é construída, para que as invocações não é possível usar parâmetros nomeados ou omitir argumentos opcionais.</span><span class="sxs-lookup"><span data-stu-id="70e32-541">If no unique parameter list is found, a parameter list with inaccessible names and no optional parameters is constructed, so that invocations cannot use named parameters or omit optional arguments.</span></span>
*  <span data-ttu-id="70e32-542">Para métodos parciais, lista de parâmetros de declaração de definição de método parcial é usada.</span><span class="sxs-lookup"><span data-stu-id="70e32-542">For partial methods, the parameter list of the defining partial method declaration is used.</span></span>
*  <span data-ttu-id="70e32-543">Para todos os outros membros da função e representantes, há apenas uma lista de parâmetro único, que é usada.</span><span class="sxs-lookup"><span data-stu-id="70e32-543">For all other function members and delegates there is only a single parameter list, which is the one used.</span></span>

<span data-ttu-id="70e32-544">A posição de um argumento ou um parâmetro é definida como o número de argumentos ou parâmetros que precedem-lo na lista de argumentos ou lista de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="70e32-544">The position of an argument or parameter is defined as the number of arguments or parameters preceding it in the argument list or parameter list.</span></span>

<span data-ttu-id="70e32-545">Os parâmetros para argumentos da função membro correspondentes são estabelecidos da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="70e32-545">The corresponding parameters for function member arguments are established as follows:</span></span>

*  <span data-ttu-id="70e32-546">Argumentos de *argument_list* de construtores de instância, métodos, indexadores e delegados:</span><span class="sxs-lookup"><span data-stu-id="70e32-546">Arguments in the *argument_list* of instance constructors, methods, indexers and delegates:</span></span>
    * <span data-ttu-id="70e32-547">Um argumento posicional em que um parâmetro fixado ocorre na mesma posição na lista de parâmetros corresponde a esse parâmetro.</span><span class="sxs-lookup"><span data-stu-id="70e32-547">A positional argument where a fixed parameter occurs at the same position in the parameter list corresponds to that parameter.</span></span>
    * <span data-ttu-id="70e32-548">Um argumento posicional de um membro da função com uma matriz de parâmetros invocado em sua forma normal corresponde à matriz de parâmetros, que deve ocorrer na mesma posição na lista de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="70e32-548">A positional argument of a function member with a parameter array invoked in its normal form corresponds to the parameter  array, which must occur at the same position in the parameter list.</span></span>
    * <span data-ttu-id="70e32-549">Um argumento posicional de um membro da função com uma matriz de parâmetros invocado em sua forma expandida, onde não ocorre a nenhum parâmetro fixo na mesma posição na lista de parâmetros, corresponde a um elemento na matriz de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="70e32-549">A positional argument of a function member with a parameter array invoked in its expanded form, where no fixed parameter occurs at the same position in the parameter list, corresponds to an element in the parameter array.</span></span>
    * <span data-ttu-id="70e32-550">Um argumento nomeado corresponde ao parâmetro de mesmo nome na lista de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="70e32-550">A named argument corresponds to the parameter of the same name in the parameter list.</span></span>
    * <span data-ttu-id="70e32-551">Para indexadores, ao invocar o `set` acessador, a expressão especificada como o operando à direita do operador de atribuição corresponde ao implícito `value` parâmetro do `set` declaração do acessador.</span><span class="sxs-lookup"><span data-stu-id="70e32-551">For indexers, when invoking the `set` accessor, the expression specified as the right operand of the assignment operator corresponds to the implicit `value` parameter of the `set` accessor declaration.</span></span>
*  <span data-ttu-id="70e32-552">Para propriedades, ao invocar o `get` acessador lá estão sem argumentos.</span><span class="sxs-lookup"><span data-stu-id="70e32-552">For properties, when invoking the `get` accessor there are no arguments.</span></span> <span data-ttu-id="70e32-553">Ao invocar o `set` acessador, a expressão especificada como o operando à direita do operador de atribuição corresponde ao implícito `value` parâmetro do `set` declaração do acessador.</span><span class="sxs-lookup"><span data-stu-id="70e32-553">When invoking the `set` accessor, the expression specified as the right operand of the assignment operator corresponds to the implicit `value` parameter of the `set` accessor declaration.</span></span>
*  <span data-ttu-id="70e32-554">Operadores de unário definido pelo usuário (incluindo conversões), o único operando corresponde ao parâmetro único da declaração do operador.</span><span class="sxs-lookup"><span data-stu-id="70e32-554">For user-defined unary operators (including conversions), the single operand corresponds to the single parameter of the operator declaration.</span></span>
*  <span data-ttu-id="70e32-555">Operadores binários definidos pelo usuário, o operando esquerdo corresponde ao primeiro parâmetro e o operando direito corresponde ao segundo parâmetro da declaração do operador.</span><span class="sxs-lookup"><span data-stu-id="70e32-555">For user-defined binary operators, the left operand corresponds to the first parameter, and the right operand corresponds to the second parameter of the operator declaration.</span></span>

#### <a name="run-time-evaluation-of-argument-lists"></a><span data-ttu-id="70e32-556">Avaliação do tempo de execução das listas de argumentos</span><span class="sxs-lookup"><span data-stu-id="70e32-556">Run-time evaluation of argument lists</span></span>

<span data-ttu-id="70e32-557">Durante o processamento de tempo de execução de uma invocação de membro de função ([verificação de resolução de sobrecarga de dinâmica de tempo de compilação](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), as expressões ou referências de variável de uma lista de argumentos são avaliadas em ordem, da esquerda para a direita, como a seguir:</span><span class="sxs-lookup"><span data-stu-id="70e32-557">During the run-time processing of a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), the expressions or variable references of an argument list are evaluated in order, from left to right, as follows:</span></span>

*  <span data-ttu-id="70e32-558">Para um parâmetro de valor, a expressão de argumento é avaliada e uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)) para o parâmetro correspondente tipo é executado.</span><span class="sxs-lookup"><span data-stu-id="70e32-558">For a value parameter, the argument expression is evaluated and an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to the corresponding parameter type is performed.</span></span> <span data-ttu-id="70e32-559">O valor resultante se torna o valor inicial do parâmetro value na invocação de membro da função.</span><span class="sxs-lookup"><span data-stu-id="70e32-559">The resulting value becomes the initial value of the value parameter in the function member invocation.</span></span>
*  <span data-ttu-id="70e32-560">Para um parâmetro de referência ou de saída, a referência de variável é avaliada e o local de armazenamento resultante se torna o local de armazenamento, representado pelo parâmetro na invocação de membro da função.</span><span class="sxs-lookup"><span data-stu-id="70e32-560">For a reference or output parameter, the variable reference is evaluated and the resulting storage location becomes the storage location represented by the parameter in the function member invocation.</span></span> <span data-ttu-id="70e32-561">Se a referência de variável fornecida como um referência ou parâmetro de saída é um elemento de matriz de um *reference_type*, é realizada uma verificação de tempo de execução para garantir que o tipo de elemento da matriz é idêntico ao tipo do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="70e32-561">If the variable reference given as a reference or output parameter is an array element of a *reference_type*, a run-time check is performed to ensure that the element type of the array is identical to the type of the parameter.</span></span> <span data-ttu-id="70e32-562">Se essa verificação falha, um `System.ArrayTypeMismatchException` é gerada.</span><span class="sxs-lookup"><span data-stu-id="70e32-562">If this check fails, a `System.ArrayTypeMismatchException` is thrown.</span></span>

<span data-ttu-id="70e32-563">Métodos, indexadores e construtores de instância podem declarar o parâmetro mais à direita como uma matriz de parâmetro ([matrizes de parâmetro](classes.md#parameter-arrays)).</span><span class="sxs-lookup"><span data-stu-id="70e32-563">Methods, indexers, and instance constructors may declare their right-most parameter to be a parameter array ([Parameter arrays](classes.md#parameter-arrays)).</span></span> <span data-ttu-id="70e32-564">Esses membros de função são invocados em sua forma normal ou em seus formulários expandidos, dependendo do que é aplicável ([membro da função aplicável](expressions.md#applicable-function-member)):</span><span class="sxs-lookup"><span data-stu-id="70e32-564">Such function members are invoked either in their normal form or in their expanded form depending on which is applicable ([Applicable function member](expressions.md#applicable-function-member)):</span></span>

*  <span data-ttu-id="70e32-565">Quando um membro de função com uma matriz de parâmetros é invocado em sua forma normal, o argumento fornecido para a matriz de parâmetros deve ser uma única expressão que é implicitamente conversível ([conversões implícitas](conversions.md#implicit-conversions)) para o tipo de matriz de parâmetro.</span><span class="sxs-lookup"><span data-stu-id="70e32-565">When a function member with a parameter array is invoked in its normal form, the argument given for the parameter array must be a single expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the parameter array type.</span></span> <span data-ttu-id="70e32-566">Nesse caso, a matriz de parâmetro Age exatamente como um parâmetro de valor.</span><span class="sxs-lookup"><span data-stu-id="70e32-566">In this case, the parameter array acts precisely like a value parameter.</span></span>
*  <span data-ttu-id="70e32-567">Quando um membro de função com uma matriz de parâmetros é invocado em sua forma expandida, a invocação deve especificar zero ou mais argumentos posicionais para a matriz de parâmetros, onde cada argumento é uma expressão que é implicitamente conversível ([implícitas conversões](conversions.md#implicit-conversions)) para o tipo de elemento da matriz de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="70e32-567">When a function member with a parameter array is invoked in its expanded form, the invocation must specify zero or more positional arguments for the parameter array, where each argument is an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the element type of the parameter array.</span></span> <span data-ttu-id="70e32-568">Nesse caso, a invocação cria uma instância do tipo de parâmetro de matriz com um comprimento correspondente ao número de argumentos, inicializa os elementos da instância de matriz com os valores de argumento fornecido e usa a instância de matriz recém-criado como o real argumento.</span><span class="sxs-lookup"><span data-stu-id="70e32-568">In this case, the invocation creates an instance of the parameter array type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="70e32-569">As expressões de uma lista de argumentos sempre são avaliadas na ordem em que elas são gravadas.</span><span class="sxs-lookup"><span data-stu-id="70e32-569">The expressions of an argument list are always evaluated in the order they are written.</span></span> <span data-ttu-id="70e32-570">Portanto, o exemplo</span><span class="sxs-lookup"><span data-stu-id="70e32-570">Thus, the example</span></span>
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
<span data-ttu-id="70e32-571">produz a saída</span><span class="sxs-lookup"><span data-stu-id="70e32-571">produces the output</span></span>
```
x = 0, y = 1, z = 2
x = 4, y = -1, z = 3
```

<span data-ttu-id="70e32-572">As regras de variação conjunta de matriz ([covariância de matriz](arrays.md#array-covariance)) permite um valor de um tipo de matriz `A[]` seja uma referência a uma instância de um tipo de matriz `B[]`, desde que existe uma conversão de referência implícita de `B` para `A`.</span><span class="sxs-lookup"><span data-stu-id="70e32-572">The array co-variance rules ([Array covariance](arrays.md#array-covariance)) permit a value of an array type `A[]` to be a reference to an instance of an array type `B[]`, provided an implicit reference conversion exists from `B` to `A`.</span></span> <span data-ttu-id="70e32-573">Devido a essas regras, quando um elemento de matriz de um *reference_type* é passado como um parâmetro de saída ou de referência, uma verificação de tempo de execução é necessária para garantir que o tipo de elemento real da matriz é idêntico do parâmetro.</span><span class="sxs-lookup"><span data-stu-id="70e32-573">Because of these rules, when an array element of a *reference_type* is passed as a reference or output parameter, a run-time check is required to ensure that the actual element type of the array is identical to that of the parameter.</span></span> <span data-ttu-id="70e32-574">No exemplo</span><span class="sxs-lookup"><span data-stu-id="70e32-574">In the example</span></span>
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
<span data-ttu-id="70e32-575">a segunda chamada de `F` faz com que um `System.ArrayTypeMismatchException` seja lançada porque o tipo de elemento real do `b` é `string` e não `object`.</span><span class="sxs-lookup"><span data-stu-id="70e32-575">the second invocation of `F` causes a `System.ArrayTypeMismatchException` to be thrown because the actual element type of `b` is `string` and not `object`.</span></span>

<span data-ttu-id="70e32-576">Quando um membro de função com uma matriz de parâmetros é invocado em sua forma expandida, a invocação é processada exatamente como se uma expressão de criação de matriz com um inicializador de matriz ([expressões de criação de matriz](expressions.md#array-creation-expressions)) foi inserido em torno de parâmetros expandidos.</span><span class="sxs-lookup"><span data-stu-id="70e32-576">When a function member with a parameter array is invoked in its expanded form, the invocation is processed exactly as if an array creation expression with an array initializer ([Array creation expressions](expressions.md#array-creation-expressions)) was inserted around the expanded parameters.</span></span> <span data-ttu-id="70e32-577">Por exemplo, dada a declaração</span><span class="sxs-lookup"><span data-stu-id="70e32-577">For example, given the declaration</span></span>
```csharp
void F(int x, int y, params object[] args);
```
<span data-ttu-id="70e32-578">as seguintes invocações da forma expandida do método</span><span class="sxs-lookup"><span data-stu-id="70e32-578">the following invocations of the expanded form of the method</span></span>
```csharp
F(10, 20);
F(10, 20, 30, 40);
F(10, 20, 1, "hello", 3.0);
```
<span data-ttu-id="70e32-579">corresponder exatamente à</span><span class="sxs-lookup"><span data-stu-id="70e32-579">correspond exactly to</span></span>
```csharp
F(10, 20, new object[] {});
F(10, 20, new object[] {30, 40});
F(10, 20, new object[] {1, "hello", 3.0});
```

<span data-ttu-id="70e32-580">Em particular, observe que uma matriz vazia é criada quando há zero argumentos fornecidos para a matriz de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="70e32-580">In particular, note that an empty array is created when there are zero arguments given for the parameter array.</span></span>

<span data-ttu-id="70e32-581">Quando os argumentos forem omitidos de um membro da função com parâmetros opcionais correspondentes, os argumentos padrão da declaração de membro da função são passados implicitamente.</span><span class="sxs-lookup"><span data-stu-id="70e32-581">When arguments are omitted from a function member with corresponding optional parameters, the default arguments of the function member declaration are implicitly passed.</span></span> <span data-ttu-id="70e32-582">Porque eles sempre são constantes, sua avaliação não afetará a ordem de avaliação dos argumentos restantes.</span><span class="sxs-lookup"><span data-stu-id="70e32-582">Because these are always constant, their evaluation will not impact the evaluation order of the remaining arguments.</span></span>

### <a name="type-inference"></a><span data-ttu-id="70e32-583">Inferência de tipo</span><span class="sxs-lookup"><span data-stu-id="70e32-583">Type inference</span></span>

<span data-ttu-id="70e32-584">Quando um método genérico é chamado sem especificar argumentos de tipo, uma ***inferência de tipo*** processo tenta inferir argumentos de tipo para a chamada.</span><span class="sxs-lookup"><span data-stu-id="70e32-584">When a generic method is called without specifying type arguments, a ***type inference*** process attempts to infer type arguments for the call.</span></span> <span data-ttu-id="70e32-585">A presença de inferência de tipo permite uma sintaxe mais conveniente a ser usado para chamar um método genérico e permite ao programador Evite especificar informações de tipo redundantes.</span><span class="sxs-lookup"><span data-stu-id="70e32-585">The presence of type inference allows a more convenient syntax to be used for calling a generic method, and allows the programmer to avoid specifying redundant type information.</span></span> <span data-ttu-id="70e32-586">Por exemplo, dada a declaração de método:</span><span class="sxs-lookup"><span data-stu-id="70e32-586">For example, given the method declaration:</span></span>
```csharp
class Chooser
{
    static Random rand = new Random();

    public static T Choose<T>(T first, T second) {
        return (rand.Next(2) == 0)? first: second;
    }
}
```
<span data-ttu-id="70e32-587">é possível invocar o `Choose` método sem especificar explicitamente um argumento de tipo:</span><span class="sxs-lookup"><span data-stu-id="70e32-587">it is possible to invoke the `Choose` method without explicitly specifying a type argument:</span></span>
```csharp
int i = Chooser.Choose(5, 213);                 // Calls Choose<int>

string s = Chooser.Choose("foo", "bar");        // Calls Choose<string>
```

<span data-ttu-id="70e32-588">Por meio de inferência de tipo, os argumentos de tipo `int` e `string` são determinados dos argumentos para o método.</span><span class="sxs-lookup"><span data-stu-id="70e32-588">Through type inference, the type arguments `int` and `string` are determined from the arguments to the method.</span></span>

<span data-ttu-id="70e32-589">Inferência de tipo ocorre como parte do processamento de tempo de associação de uma invocação de método ([invocações de método](expressions.md#method-invocations)) e ocorre antes da etapa de resolução de sobrecarga da invocação.</span><span class="sxs-lookup"><span data-stu-id="70e32-589">Type inference occurs as part of the binding-time processing of a method invocation ([Method invocations](expressions.md#method-invocations)) and takes place before the overload resolution step of the invocation.</span></span> <span data-ttu-id="70e32-590">Quando um grupo de método em particular for especificado em uma invocação de método e nenhum argumento de tipo é especificado como parte da invocação do método, a inferência de tipos é aplicada a cada método genérico no grupo de método.</span><span class="sxs-lookup"><span data-stu-id="70e32-590">When a particular method group is specified in a method invocation, and no type arguments are specified as part of the method invocation, type inference is applied to each generic method in the method group.</span></span> <span data-ttu-id="70e32-591">Se a inferência de tipo for bem-sucedida, os argumentos de tipo inferidos são usados para determinar os tipos de argumentos para a resolução de sobrecarga subsequentes.</span><span class="sxs-lookup"><span data-stu-id="70e32-591">If type inference succeeds, then the inferred type arguments are used to determine the types of arguments for subsequent overload resolution.</span></span> <span data-ttu-id="70e32-592">Se a resolução de sobrecarga escolhe um método genérico que o usado para invocar, os argumentos de tipo inferidos são usados como os argumentos de tipo real para a invocação.</span><span class="sxs-lookup"><span data-stu-id="70e32-592">If overload resolution chooses a generic method as the one to invoke, then the inferred type arguments are used as the actual type arguments for the invocation.</span></span> <span data-ttu-id="70e32-593">Se a falha de inferência de tipo para um método específico, esse método não participará na resolução de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="70e32-593">If type inference for a particular method fails, that method does not participate in overload resolution.</span></span> <span data-ttu-id="70e32-594">A falha de inferência de tipo, por si só, não causa um erro em tempo de vinculação.</span><span class="sxs-lookup"><span data-stu-id="70e32-594">The failure of type inference, in and of itself, does not cause a binding-time error.</span></span> <span data-ttu-id="70e32-595">No entanto, isso geralmente leva a um erro em tempo de vinculação quando a resolução de sobrecarga, em seguida, consegue encontrar nenhum método aplicável.</span><span class="sxs-lookup"><span data-stu-id="70e32-595">However, it often leads to a binding-time error when overload resolution then fails to find any applicable methods.</span></span>

<span data-ttu-id="70e32-596">Se o número fornecido de argumentos é diferente do número de parâmetros no método, em seguida, inferência imediatamente falhará.</span><span class="sxs-lookup"><span data-stu-id="70e32-596">If the supplied number of arguments is different than the number of parameters in the method, then inference immediately fails.</span></span> <span data-ttu-id="70e32-597">Caso contrário, suponha que o método genérico tem a seguinte assinatura:</span><span class="sxs-lookup"><span data-stu-id="70e32-597">Otherwise, assume that the generic method has the following signature:</span></span>
```csharp
Tr M<X1,...,Xn>(T1 x1, ..., Tm xm)
```

<span data-ttu-id="70e32-598">Com uma chamada de método do formulário `M(E1...Em)` a tarefa de inferência de tipo é encontrar os argumentos de tipo exclusivo `S1...Sn` para cada um dos parâmetros de tipo `X1...Xn` para que a chamada `M<S1...Sn>(E1...Em)` se torna válido.</span><span class="sxs-lookup"><span data-stu-id="70e32-598">With a method call of the form `M(E1...Em)` the task of type inference is to find unique type arguments `S1...Sn` for each of the type parameters `X1...Xn` so that the call `M<S1...Sn>(E1...Em)` becomes valid.</span></span>

<span data-ttu-id="70e32-599">Durante o processo de inferência de cada parâmetro de tipo `Xi` seja *fixo* a um determinado tipo `Si` ou *unfixed* com um conjunto associado de *limites*.</span><span class="sxs-lookup"><span data-stu-id="70e32-599">During the process of inference each type parameter `Xi` is either *fixed* to a particular type `Si` or *unfixed* with an associated set of *bounds*.</span></span> <span data-ttu-id="70e32-600">Cada um dos limites é algum tipo `T`.</span><span class="sxs-lookup"><span data-stu-id="70e32-600">Each of the bounds is some type `T`.</span></span> <span data-ttu-id="70e32-601">Inicialmente, cada variável de tipo `Xi` é unfixed com um conjunto vazio de limites.</span><span class="sxs-lookup"><span data-stu-id="70e32-601">Initially each type variable `Xi` is unfixed with an empty set of bounds.</span></span>

<span data-ttu-id="70e32-602">Inferência de tipos ocorre em fases.</span><span class="sxs-lookup"><span data-stu-id="70e32-602">Type inference takes place in phases.</span></span> <span data-ttu-id="70e32-603">Cada fase tentará inferir argumentos de tipo para obter mais variáveis de tipo com base nas descobertas da fase anterior.</span><span class="sxs-lookup"><span data-stu-id="70e32-603">Each phase will try to infer type arguments for more type variables based on the findings of the previous phase.</span></span> <span data-ttu-id="70e32-604">A primeira fase faz algumas inferências inicias dos limites, enquanto a segunda fase variáveis de tipo para tipos específicos de correções e infere o dos limites.</span><span class="sxs-lookup"><span data-stu-id="70e32-604">The first phase makes some initial inferences of bounds, whereas the second phase fixes type variables to specific types and infers further bounds.</span></span> <span data-ttu-id="70e32-605">A segunda fase pode ter que ser repetido um número de vezes.</span><span class="sxs-lookup"><span data-stu-id="70e32-605">The second phase may have to be repeated a number of times.</span></span>

<span data-ttu-id="70e32-606">*Observação:* tipo inferência ocorre não apenas quando um método genérico é chamado.</span><span class="sxs-lookup"><span data-stu-id="70e32-606">*Note:* Type inference takes place not only when a generic method is called.</span></span> <span data-ttu-id="70e32-607">Inferência de tipo para conversão de grupos de método é descrita em [inferência de tipos para a conversão de grupos de métodos](expressions.md#type-inference-for-conversion-of-method-groups) e encontrar o melhor tipo comum de um conjunto de expressões é descrita em [localizando o melhor tipo comum de um conjunto expressões](expressions.md#finding-the-best-common-type-of-a-set-of-expressions).</span><span class="sxs-lookup"><span data-stu-id="70e32-607">Type inference for conversion of method groups is described in [Type inference for conversion of method groups](expressions.md#type-inference-for-conversion-of-method-groups) and finding the best common type of a set of expressions is described in [Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions).</span></span>

#### <a name="the-first-phase"></a><span data-ttu-id="70e32-608">A primeira fase</span><span class="sxs-lookup"><span data-stu-id="70e32-608">The first phase</span></span>

<span data-ttu-id="70e32-609">Para cada um dos argumentos de método `Ei`:</span><span class="sxs-lookup"><span data-stu-id="70e32-609">For each of the method arguments `Ei`:</span></span>

*   <span data-ttu-id="70e32-610">Se `Ei` é uma função anônima, uma *inferência de tipo de parâmetro explícito* ([inferências de tipo de parâmetro explícito](expressions.md#explicit-parameter-type-inferences)) é feita do `Ei` para `Ti`</span><span class="sxs-lookup"><span data-stu-id="70e32-610">If `Ei` is an anonymous function, an *explicit parameter type inference* ([Explicit parameter type inferences](expressions.md#explicit-parameter-type-inferences)) is made from `Ei` to `Ti`</span></span>
*   <span data-ttu-id="70e32-611">Caso contrário, se `Ei` tem um tipo `U` e `xi` é um parâmetro de valor, uma *inferência de tipos de limite inferior* é feita *do* `U` *para* `Ti`.</span><span class="sxs-lookup"><span data-stu-id="70e32-611">Otherwise, if `Ei` has a type `U` and `xi` is a value parameter then a *lower-bound inference* is made *from* `U` *to* `Ti`.</span></span>
*   <span data-ttu-id="70e32-612">Caso contrário, se `Ei` tem um tipo `U` e `xi` é uma `ref` ou `out` parâmetro, uma *exato inferência* é feita *de* `U` *à* `Ti`.</span><span class="sxs-lookup"><span data-stu-id="70e32-612">Otherwise, if `Ei` has a type `U` and `xi` is a `ref` or `out` parameter then an *exact inference* is made *from* `U` *to* `Ti`.</span></span>
*   <span data-ttu-id="70e32-613">Caso contrário, nenhuma inferência é feita para esse argumento.</span><span class="sxs-lookup"><span data-stu-id="70e32-613">Otherwise, no inference is made for this argument.</span></span>


#### <a name="the-second-phase"></a><span data-ttu-id="70e32-614">A segunda fase</span><span class="sxs-lookup"><span data-stu-id="70e32-614">The second phase</span></span>

<span data-ttu-id="70e32-615">A segunda fase procede da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="70e32-615">The second phase proceeds as follows:</span></span>

*   <span data-ttu-id="70e32-616">Todos os *unfixed* variáveis do tipo `Xi` quais não *dependem* ([dependência](expressions.md#dependence)) qualquer `Xj` são fixos ([corrigindo](expressions.md#fixing)).</span><span class="sxs-lookup"><span data-stu-id="70e32-616">All *unfixed* type variables `Xi` which do not *depend on* ([Dependence](expressions.md#dependence)) any `Xj` are fixed ([Fixing](expressions.md#fixing)).</span></span>
*   <span data-ttu-id="70e32-617">Se essas variáveis de tipo não existir, todos os *unfixed* variáveis do tipo `Xi` são *fixa* para o qual todos os itens a seguir manter:</span><span class="sxs-lookup"><span data-stu-id="70e32-617">If no such type variables exist, all *unfixed* type variables `Xi` are *fixed* for which all of the following hold:</span></span>
    *   <span data-ttu-id="70e32-618">Há pelo menos uma variável de tipo `Xj` depende do que `Xi`</span><span class="sxs-lookup"><span data-stu-id="70e32-618">There is at least one type variable `Xj` that depends on `Xi`</span></span>
    *   <span data-ttu-id="70e32-619">`Xi` tem um conjunto não vazio de limites</span><span class="sxs-lookup"><span data-stu-id="70e32-619">`Xi` has a non-empty set of bounds</span></span>
*   <span data-ttu-id="70e32-620">Se essas variáveis de tipo não existem e ainda houver *unfixed* variáveis do tipo, falha de inferência de tipo.</span><span class="sxs-lookup"><span data-stu-id="70e32-620">If no such type variables exist and there are still *unfixed* type variables, type inference fails.</span></span>
*   <span data-ttu-id="70e32-621">Caso contrário, se nenhuma outra *unfixed* as variáveis de tipo existem, inferência de tipo é bem-sucedida.</span><span class="sxs-lookup"><span data-stu-id="70e32-621">Otherwise, if no further *unfixed* type variables exist, type inference succeeds.</span></span>
*   <span data-ttu-id="70e32-622">Caso contrário, para todos os argumentos `Ei` com o tipo de parâmetro correspondente `Ti` em que o *tipos de saída* ([tipos de saída](expressions.md#output-types)) contêm *unfixed* variáveis do tipo `Xj` , mas o *tipos de entrada* ([tipos de entrada](expressions.md#input-types)) não fizer isso, uma *inferência de tipo de saída* ([inferências de tipo de saída ](expressions.md#output-type-inferences)) é feita *partir* `Ei` *para* `Ti`.</span><span class="sxs-lookup"><span data-stu-id="70e32-622">Otherwise, for all arguments `Ei` with corresponding parameter type `Ti` where the *output types* ([Output types](expressions.md#output-types)) contain *unfixed* type variables `Xj` but the *input types* ([Input types](expressions.md#input-types)) do not, an *output type inference* ([Output type inferences](expressions.md#output-type-inferences)) is made *from* `Ei` *to* `Ti`.</span></span> <span data-ttu-id="70e32-623">Em seguida, a segunda fase é repetida.</span><span class="sxs-lookup"><span data-stu-id="70e32-623">Then the second phase is repeated.</span></span>

#### <a name="input-types"></a><span data-ttu-id="70e32-624">Tipos de entrada</span><span class="sxs-lookup"><span data-stu-id="70e32-624">Input types</span></span>

<span data-ttu-id="70e32-625">Se `E` é um grupo de método ou função anônima de tipo implícito e `T` é um delegado, tipo ou tipo de árvore de expressão e todos os tipos de parâmetro `T` são *tipos de entrada* de `E` *com tipo* `T`.</span><span class="sxs-lookup"><span data-stu-id="70e32-625">If `E` is a method group or implicitly typed anonymous function and `T` is a delegate type or expression tree type then all the parameter types of `T` are *input types* of `E` *with type* `T`.</span></span>

####  <a name="output-types"></a><span data-ttu-id="70e32-626">Tipos de saída</span><span class="sxs-lookup"><span data-stu-id="70e32-626">Output types</span></span>

<span data-ttu-id="70e32-627">Se `E` é um grupo de método ou uma função anônima e `T` é um delegado, tipo ou tipo de árvore de expressão e o tipo de retorno `T` é uma *tipo de saída* `E` *com tipo*  `T`.</span><span class="sxs-lookup"><span data-stu-id="70e32-627">If `E` is a method group or an anonymous function and `T` is a delegate type or expression tree type then the return type of `T` is an *output type of* `E` *with type* `T`.</span></span>

#### <a name="dependence"></a><span data-ttu-id="70e32-628">Dependência</span><span class="sxs-lookup"><span data-stu-id="70e32-628">Dependence</span></span>

<span data-ttu-id="70e32-629">Uma *unfixed* variável de tipo `Xi` *depende diretamente* uma variável do tipo unfixed `Xj` se algum argumento `Ek` com tipo `Tk` `Xj` ocorre em um *tipo de entrada* de `Ek` com tipo `Tk` e `Xi` ocorre em um *tipo de saída* de `Ek` com tipo `Tk`.</span><span class="sxs-lookup"><span data-stu-id="70e32-629">An *unfixed* type variable `Xi` *depends directly on* an unfixed type variable `Xj` if for some argument `Ek` with type `Tk` `Xj` occurs in an *input type* of `Ek` with type `Tk` and `Xi` occurs in an *output type* of `Ek` with type `Tk`.</span></span>

<span data-ttu-id="70e32-630">`Xj` *depende* `Xi` se `Xj` *depende diretamente* `Xi` ou se `Xi` *depende diretamente* `Xk` e `Xk` *depende* `Xj`.</span><span class="sxs-lookup"><span data-stu-id="70e32-630">`Xj` *depends on* `Xi` if `Xj` *depends directly on* `Xi` or if `Xi` *depends directly on* `Xk` and `Xk` *depends on* `Xj`.</span></span> <span data-ttu-id="70e32-631">Portanto, "depende" é o fechamento transitivo, mas não reflexivo de "depende diretamente".</span><span class="sxs-lookup"><span data-stu-id="70e32-631">Thus "depends on" is the transitive but not reflexive closure of "depends directly on".</span></span>

#### <a name="output-type-inferences"></a><span data-ttu-id="70e32-632">Inferências de tipo de saída</span><span class="sxs-lookup"><span data-stu-id="70e32-632">Output type inferences</span></span>

<span data-ttu-id="70e32-633">Uma *inferência de tipo de saída* é feita *partir* uma expressão `E` *para* um tipo `T` da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="70e32-633">An *output type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>

*  <span data-ttu-id="70e32-634">Se `E` é uma função anônima com o tipo de retorno deduzido `U` ([tipo de retorno inferido](expressions.md#inferred-return-type)) e `T` é um tipo de delegado ou tipo de árvore de expressão com o tipo de retorno `Tb`, em seguida, um *inferência de tipos de limite inferior* ([inferências de limite inferior](expressions.md#lower-bound-inferences)) é feita *do* `U` *para* `Tb`.</span><span class="sxs-lookup"><span data-stu-id="70e32-634">If `E` is an anonymous function with inferred return type  `U` ([Inferred return type](expressions.md#inferred-return-type)) and `T` is a delegate type or expression tree type with return type `Tb`, then a *lower-bound inference* ([Lower-bound inferences](expressions.md#lower-bound-inferences)) is made *from* `U` *to* `Tb`.</span></span>
*  <span data-ttu-id="70e32-635">Caso contrário, se `E` é um grupo de método e `T` é um tipo de delegado ou tipo de árvore de expressão com tipos de parâmetro `T1...Tk` e o tipo de retorno `Tb`e a resolução de sobrecarga de `E` com os tipos de `T1...Tk` produz um um único método com o tipo de retorno `U`, em seguida, um *inferência de tipos de limite inferior* é feita *da* `U` *para* `Tb`.</span><span class="sxs-lookup"><span data-stu-id="70e32-635">Otherwise, if `E` is a method group and `T` is a delegate type or expression tree type with parameter types `T1...Tk` and return type `Tb`, and overload resolution of `E` with the types `T1...Tk` yields a single method with return type `U`, then a *lower-bound inference* is made *from* `U` *to* `Tb`.</span></span>
*  <span data-ttu-id="70e32-636">Caso contrário, se `E` é uma expressão com o tipo `U`, em seguida, um *inferência de tipos de limite inferior* é feita *do* `U` *para* `T`.</span><span class="sxs-lookup"><span data-stu-id="70e32-636">Otherwise, if `E` is an expression with type `U`, then a *lower-bound inference* is made *from* `U` *to* `T`.</span></span>
*  <span data-ttu-id="70e32-637">Caso contrário, nenhum inferências são feitas.</span><span class="sxs-lookup"><span data-stu-id="70e32-637">Otherwise, no inferences are made.</span></span>

#### <a name="explicit-parameter-type-inferences"></a><span data-ttu-id="70e32-638">Inferências de tipo de parâmetro explícito</span><span class="sxs-lookup"><span data-stu-id="70e32-638">Explicit parameter type inferences</span></span>

<span data-ttu-id="70e32-639">Uma *inferência de tipo de parâmetro explícito* é feita *partir* uma expressão `E` *para* um tipo `T` da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="70e32-639">An *explicit parameter type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>

*  <span data-ttu-id="70e32-640">Se `E` é uma função anônima digitada explicitamente com tipos de parâmetro `U1...Uk` e `T` é um tipo de delegado ou tipo de árvore de expressão com tipos de parâmetro `V1...Vk` , em seguida, para cada `Ui` um *exata inferência* ([exato inferências](expressions.md#exact-inferences)) é feita *da* `Ui` *para* correspondente `Vi`.</span><span class="sxs-lookup"><span data-stu-id="70e32-640">If `E` is an explicitly typed anonymous function with parameter types `U1...Uk` and `T` is a delegate type or expression tree type with parameter types `V1...Vk` then for each `Ui` an *exact inference* ([Exact inferences](expressions.md#exact-inferences)) is made *from* `Ui` *to* the corresponding `Vi`.</span></span>

#### <a name="exact-inferences"></a><span data-ttu-id="70e32-641">Inferências exatas</span><span class="sxs-lookup"><span data-stu-id="70e32-641">Exact inferences</span></span>

<span data-ttu-id="70e32-642">Uma *exato inferência* *de* um tipo `U` *para* um tipo `V` é feita da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="70e32-642">An *exact inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="70e32-643">Se `V` é um dos *unfixed* `Xi` , em seguida, `U` é adicionado ao conjunto de limites exatos para `Xi`.</span><span class="sxs-lookup"><span data-stu-id="70e32-643">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of exact bounds for `Xi`.</span></span>

*  <span data-ttu-id="70e32-644">Caso contrário, define `V1...Vk` e `U1...Uk` são determinadas pela verificação se qualquer um dos casos a seguir se aplicam:</span><span class="sxs-lookup"><span data-stu-id="70e32-644">Otherwise, sets `V1...Vk` and `U1...Uk` are determined by checking if any of the following cases apply:</span></span>

   *  <span data-ttu-id="70e32-645">`V` é um tipo de matriz `V1[...]` e `U` é um tipo de matriz `U1[...]` da mesma classificação</span><span class="sxs-lookup"><span data-stu-id="70e32-645">`V` is an array type `V1[...]` and `U` is an array type `U1[...]`  of the same rank</span></span>
   *  <span data-ttu-id="70e32-646">`V` é o tipo `V1?` e `U` é do tipo `U1?`</span><span class="sxs-lookup"><span data-stu-id="70e32-646">`V` is the type `V1?` and `U` is the type `U1?`</span></span>
   *  <span data-ttu-id="70e32-647">`V` é um tipo construído `C<V1...Vk>`e `U` é um tipo construído `C<U1...Uk>`</span><span class="sxs-lookup"><span data-stu-id="70e32-647">`V` is a constructed type `C<V1...Vk>`and `U` is a constructed type `C<U1...Uk>`</span></span>

   <span data-ttu-id="70e32-648">Se qualquer um desses casos, em seguida, aplicar um *exato inferência* é feita *de* cada `Ui` *para* correspondente `Vi`.</span><span class="sxs-lookup"><span data-stu-id="70e32-648">If any of these cases apply then an *exact inference* is made *from* each `Ui` *to* the corresponding `Vi`.</span></span>

*  <span data-ttu-id="70e32-649">Caso contrário, nenhum inferências são feitas.</span><span class="sxs-lookup"><span data-stu-id="70e32-649">Otherwise no inferences are made.</span></span>

#### <a name="lower-bound-inferences"></a><span data-ttu-id="70e32-650">Limite inferior inferências</span><span class="sxs-lookup"><span data-stu-id="70e32-650">Lower-bound inferences</span></span>

<span data-ttu-id="70e32-651">Um *inferência de tipos de limite inferior* *partir* um tipo `U` *para* um tipo `V` é feita da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="70e32-651">A *lower-bound inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="70e32-652">Se `V` é um dos *unfixed* `Xi` , em seguida, `U` é adicionado ao conjunto de limites inferiores do `Xi`.</span><span class="sxs-lookup"><span data-stu-id="70e32-652">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of lower bounds for `Xi`.</span></span>
*  <span data-ttu-id="70e32-653">Caso contrário, se `V` é o tipo `V1?`e `U` é o tipo `U1?` uma inferência de limite inferior é feita da `U1` para `V1`.</span><span class="sxs-lookup"><span data-stu-id="70e32-653">Otherwise, if `V` is the type `V1?`and `U` is the type `U1?` then a lower bound inference is made from `U1` to `V1`.</span></span>
*  <span data-ttu-id="70e32-654">Caso contrário, define `U1...Uk` e `V1...Vk` são determinadas pela verificação se qualquer um dos casos a seguir se aplicam:</span><span class="sxs-lookup"><span data-stu-id="70e32-654">Otherwise, sets `U1...Uk` and `V1...Vk` are determined by checking if any of the following cases apply:</span></span>
   *  <span data-ttu-id="70e32-655">`V` é um tipo de matriz `V1[...]` e `U` é um tipo de matriz `U1[...]` (ou um parâmetro de tipo cujo tipo base eficaz é `U1[...]`) da mesma classificação</span><span class="sxs-lookup"><span data-stu-id="70e32-655">`V` is an array type `V1[...]` and `U` is an array type `U1[...]` (or a type parameter whose effective base type is `U1[...]`) of the same rank</span></span>
   *  <span data-ttu-id="70e32-656">`V` é um dos `IEnumerable<V1>`, `ICollection<V1>` ou `IList<V1>` e `U` é um tipo de matriz unidimensional `U1[]`(ou um parâmetro de tipo cujo tipo base eficaz é `U1[]`)</span><span class="sxs-lookup"><span data-stu-id="70e32-656">`V` is one of `IEnumerable<V1>`, `ICollection<V1>` or `IList<V1>` and `U` is a one-dimensional array type `U1[]`(or a type parameter whose effective base type is `U1[]`)</span></span>
   *  <span data-ttu-id="70e32-657">`V` é um tipo de classe, struct, interface ou delegado construído `C<V1...Vk>` e há um tipo exclusivo `C<U1...Uk>` , de modo que `U` (ou, se `U` é um parâmetro de tipo, sua classe base efetivo ou qualquer membro do seu conjunto efetivo de interface) é idêntico ao, herda (direta ou indiretamente) ou implementa (direta ou indiretamente) `C<U1...Uk>`.</span><span class="sxs-lookup"><span data-stu-id="70e32-657">`V` is a constructed class, struct, interface or delegate type `C<V1...Vk>` and there is a unique type `C<U1...Uk>` such that `U` (or, if `U` is a type parameter, its effective base class or any member of its effective interface set) is identical to, inherits from (directly or indirectly), or implements (directly or indirectly) `C<U1...Uk>`.</span></span>

      <span data-ttu-id="70e32-658">(A restrição "exclusividade" significa que, na interface do case `C<T> {} class U: C<X>, C<Y> {}`, nenhuma inferência é feita quando a inferência de `U` à `C<T>` porque `U1` poderia ser `X` ou `Y`.)</span><span class="sxs-lookup"><span data-stu-id="70e32-658">(The "uniqueness" restriction means that in the case interface `C<T> {} class U: C<X>, C<Y> {}`, then no inference is made when inferring from `U` to `C<T>` because `U1` could be `X` or `Y`.)</span></span>

   <span data-ttu-id="70e32-659">Se qualquer um desses casos aplicar uma inferência é feita *partir* cada `Ui` *para* correspondente `Vi` da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="70e32-659">If any of these cases apply then an inference is made *from* each `Ui` *to* the corresponding `Vi` as follows:</span></span>

   *  <span data-ttu-id="70e32-660">Se `Ui` não é conhecido como um tipo de referência, uma *exato inferência* é feita</span><span class="sxs-lookup"><span data-stu-id="70e32-660">If `Ui` is not known to be a reference type then an *exact inference* is made</span></span>
   *  <span data-ttu-id="70e32-661">Caso contrário, se `U` é um tipo de matriz, um *inferência de tipos de limite inferior* é feita</span><span class="sxs-lookup"><span data-stu-id="70e32-661">Otherwise, if `U` is an array type then a *lower-bound inference* is made</span></span>
   *  <span data-ttu-id="70e32-662">Caso contrário, se `V` está `C<V1...Vk>` e em seguida, depende de inferência de tipos no parâmetro de tipo i-th `C`:</span><span class="sxs-lookup"><span data-stu-id="70e32-662">Otherwise, if `V` is `C<V1...Vk>` then inference depends on the i-th type parameter of `C`:</span></span>
      *  <span data-ttu-id="70e32-663">Se ele é covariante, uma *inferência de tipos de limite inferior* é feita.</span><span class="sxs-lookup"><span data-stu-id="70e32-663">If it is covariant then a *lower-bound inference* is made.</span></span>
      *  <span data-ttu-id="70e32-664">Se ele é contravariante, então um *inferência de tipos de limite superior* é feita.</span><span class="sxs-lookup"><span data-stu-id="70e32-664">If it is contravariant then an *upper-bound inference* is made.</span></span>
      *  <span data-ttu-id="70e32-665">Se ele for invariável, uma *exato inferência* é feita.</span><span class="sxs-lookup"><span data-stu-id="70e32-665">If it is invariant then an *exact inference* is made.</span></span>
*  <span data-ttu-id="70e32-666">Caso contrário, nenhum inferências são feitas.</span><span class="sxs-lookup"><span data-stu-id="70e32-666">Otherwise, no inferences are made.</span></span>

#### <a name="upper-bound-inferences"></a><span data-ttu-id="70e32-667">Limite superior inferências</span><span class="sxs-lookup"><span data-stu-id="70e32-667">Upper-bound inferences</span></span>

<span data-ttu-id="70e32-668">Uma *inferência de tipos de limite superior* *de* um tipo `U` *para* um tipo `V` é feita da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="70e32-668">An *upper-bound inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="70e32-669">Se `V` é um dos *unfixed* `Xi` , em seguida, `U` é adicionado ao conjunto de limites superiores do `Xi`.</span><span class="sxs-lookup"><span data-stu-id="70e32-669">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of upper bounds for `Xi`.</span></span>
*  <span data-ttu-id="70e32-670">Caso contrário, define `V1...Vk` e `U1...Uk` são determinadas pela verificação se qualquer um dos casos a seguir se aplicam:</span><span class="sxs-lookup"><span data-stu-id="70e32-670">Otherwise, sets `V1...Vk` and `U1...Uk` are determined by checking if any of the following cases apply:</span></span>
   *  <span data-ttu-id="70e32-671">`U` é um tipo de matriz `U1[...]` e `V` é um tipo de matriz `V1[...]` da mesma classificação</span><span class="sxs-lookup"><span data-stu-id="70e32-671">`U` is an array type `U1[...]` and `V` is an array type `V1[...]` of the same rank</span></span>
   *  <span data-ttu-id="70e32-672">`U` é um dos `IEnumerable<Ue>`, `ICollection<Ue>` ou `IList<Ue>` e `V` é um tipo de matriz unidimensional `Ve[]`</span><span class="sxs-lookup"><span data-stu-id="70e32-672">`U` is one of `IEnumerable<Ue>`, `ICollection<Ue>` or `IList<Ue>` and `V` is a one-dimensional array type `Ve[]`</span></span>
   *  <span data-ttu-id="70e32-673">`U` é o tipo `U1?` e `V` é do tipo `V1?`</span><span class="sxs-lookup"><span data-stu-id="70e32-673">`U` is the type `U1?` and `V` is the type `V1?`</span></span>
   *  <span data-ttu-id="70e32-674">`U` é construído de classe, struct, tipo de interface ou delegado `C<U1...Uk>` e `V` é uma classe, struct, tipo de delegado ou interface que é idêntico a herda (direta ou indiretamente) ou implementa (direta ou indiretamente) um tipo exclusivo `C<V1...Vk>`</span><span class="sxs-lookup"><span data-stu-id="70e32-674">`U` is constructed class, struct, interface or delegate type `C<U1...Uk>` and `V` is a class, struct, interface or delegate type which is identical to, inherits from (directly or indirectly), or implements (directly or indirectly) a unique type `C<V1...Vk>`</span></span>

      <span data-ttu-id="70e32-675">(A restrição "exclusividade" significa que, se tivermos `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`, nenhuma inferência é feita quando a inferência de `C<U1>` para `V<Q>`.</span><span class="sxs-lookup"><span data-stu-id="70e32-675">(The "uniqueness" restriction means that if we have `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`, then no inference is made when inferring from `C<U1>` to `V<Q>`.</span></span> <span data-ttu-id="70e32-676">Inferências não são feitas de `U1` para um `X<Q>` ou `Y<Q>`.)</span><span class="sxs-lookup"><span data-stu-id="70e32-676">Inferences are not made from `U1` to either `X<Q>` or `Y<Q>`.)</span></span>

   <span data-ttu-id="70e32-677">Se qualquer um desses casos aplicar uma inferência é feita *partir* cada `Ui` *para* correspondente `Vi` da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="70e32-677">If any of these cases apply then an inference is made *from* each `Ui` *to* the corresponding `Vi` as follows:</span></span>
   *  <span data-ttu-id="70e32-678">Se `Ui` não é conhecido como um tipo de referência, uma *exato inferência* é feita</span><span class="sxs-lookup"><span data-stu-id="70e32-678">If  `Ui` is not known to be a reference type then an *exact inference* is made</span></span>
   *  <span data-ttu-id="70e32-679">Caso contrário, se `V` é um tipo de matriz, uma *inferência de tipos de limite superior* é feita</span><span class="sxs-lookup"><span data-stu-id="70e32-679">Otherwise, if `V` is an array type then an *upper-bound inference* is made</span></span>
   *  <span data-ttu-id="70e32-680">Caso contrário, se `U` está `C<U1...Uk>` e em seguida, depende de inferência de tipos no parâmetro de tipo i-th `C`:</span><span class="sxs-lookup"><span data-stu-id="70e32-680">Otherwise, if `U` is `C<U1...Uk>` then inference depends on the i-th type parameter of `C`:</span></span>
      *  <span data-ttu-id="70e32-681">Se ele é covariante, uma *inferência de tipos de limite superior* é feita.</span><span class="sxs-lookup"><span data-stu-id="70e32-681">If it is covariant then an *upper-bound inference* is made.</span></span>
      *  <span data-ttu-id="70e32-682">Se ele é contravariante, em seguida, um *inferência de tipos de limite inferior* é feita.</span><span class="sxs-lookup"><span data-stu-id="70e32-682">If it is contravariant then a *lower-bound inference* is made.</span></span>
      *  <span data-ttu-id="70e32-683">Se ele for invariável, uma *exato inferência* é feita.</span><span class="sxs-lookup"><span data-stu-id="70e32-683">If it is invariant then an *exact inference* is made.</span></span>
*  <span data-ttu-id="70e32-684">Caso contrário, nenhum inferências são feitas.</span><span class="sxs-lookup"><span data-stu-id="70e32-684">Otherwise, no inferences are made.</span></span>   

#### <a name="fixing"></a><span data-ttu-id="70e32-685">Corrigir</span><span class="sxs-lookup"><span data-stu-id="70e32-685">Fixing</span></span>

<span data-ttu-id="70e32-686">Uma *unfixed* variável de tipo `Xi` com um conjunto de limites é *fixa* da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="70e32-686">An *unfixed* type variable `Xi` with a set of bounds is *fixed* as follows:</span></span>

*  <span data-ttu-id="70e32-687">O conjunto de *tipos de candidatos* `Uj` começa como o conjunto de todos os tipos no conjunto de limites para `Xi`.</span><span class="sxs-lookup"><span data-stu-id="70e32-687">The set of *candidate types* `Uj` starts out as the set of all types in the set of bounds for `Xi`.</span></span>
*  <span data-ttu-id="70e32-688">Em seguida, examinaremos cada limite para `Xi` por sua vez: cada limite exato `U` de `Xi` todos os tipos `Uj` que não são idêntico ao `U` são removidas do conjunto de candidatas.</span><span class="sxs-lookup"><span data-stu-id="70e32-688">We then examine each bound for `Xi` in turn: For each exact bound `U` of `Xi` all types `Uj` which are not identical to `U` are removed from the candidate set.</span></span> <span data-ttu-id="70e32-689">Cada limite inferior `U` dos `Xi` todos os tipos `Uj` para o qual existe é *não* uma conversão implícita de `U` são removidas do conjunto de candidatas.</span><span class="sxs-lookup"><span data-stu-id="70e32-689">For each lower bound `U` of `Xi` all types `Uj` to which there is *not* an implicit conversion from `U` are removed from the candidate set.</span></span> <span data-ttu-id="70e32-690">Para cada limite superior `U` de `Xi` todos os tipos `Uj` daí que está *não* uma conversão implícita para `U` são removidas do conjunto de candidatos.</span><span class="sxs-lookup"><span data-stu-id="70e32-690">For each upper bound `U` of `Xi` all types `Uj` from which there is *not* an implicit conversion to `U` are removed from the candidate set.</span></span>
*  <span data-ttu-id="70e32-691">Se entre os demais tipos de candidato `Uj` há um tipo exclusivo `V` de que há uma conversão implícita para todos os outros candidatos tipos, em seguida, `Xi` está fixado em `V`.</span><span class="sxs-lookup"><span data-stu-id="70e32-691">If among the remaining candidate types `Uj` there is a unique type `V` from which there is an implicit conversion to all the other candidate types, then `Xi` is fixed to `V`.</span></span>
*  <span data-ttu-id="70e32-692">Caso contrário, a inferência de tipo falhará.</span><span class="sxs-lookup"><span data-stu-id="70e32-692">Otherwise, type inference fails.</span></span>

#### <a name="inferred-return-type"></a><span data-ttu-id="70e32-693">Tipo de retorno deduzido</span><span class="sxs-lookup"><span data-stu-id="70e32-693">Inferred return type</span></span>

<span data-ttu-id="70e32-694">O inferido do tipo de retorno de uma função anônima `F` é usado durante a resolução de sobrecarga e de inferência de tipo.</span><span class="sxs-lookup"><span data-stu-id="70e32-694">The inferred return type of an anonymous function `F` is used during type inference and overload resolution.</span></span> <span data-ttu-id="70e32-695">O tipo de retorno deduzido só pode ser determinado para uma função anônima onde o parâmetro todos os tipos são conhecidos, ou porque eles são atribuídos explicitamente, fornecido por meio de uma conversão de função anônima ou inferido durante a inferência de tipo em um delimitador genérica invocação de método.</span><span class="sxs-lookup"><span data-stu-id="70e32-695">The inferred return type can only be determined for an anonymous function where all parameter types are known, either because they are explicitly given, provided through an anonymous function conversion or inferred during type inference on an enclosing generic method invocation.</span></span>

<span data-ttu-id="70e32-696">O ***inferido do tipo de resultado*** é determinado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="70e32-696">The ***inferred result type*** is determined as follows:</span></span>

*  <span data-ttu-id="70e32-697">Se o corpo da `F` é um *expressão* que tem um tipo e, em seguida, o tipo inferido do resultado de `F` é o tipo dessa expressão.</span><span class="sxs-lookup"><span data-stu-id="70e32-697">If the body of `F` is an *expression* that has a type, then the inferred result type of `F` is the type of that expression.</span></span>
*  <span data-ttu-id="70e32-698">Se o corpo da `F` é um *bloco* e o conjunto de expressões no bloco de `return` instruções tem um tipo comum melhor `T` ([localizando o melhor tipo comum de um conjunto de expressões](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), em seguida, o tipo de resultado inferido de `F` é `T`.</span><span class="sxs-lookup"><span data-stu-id="70e32-698">If the body of `F` is a *block* and the set of expressions in the block's `return` statements has a best common type `T` ([Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), then the inferred result type of `F` is `T`.</span></span>
*  <span data-ttu-id="70e32-699">Caso contrário, um tipo de resultado não pode ser inferido para `F`.</span><span class="sxs-lookup"><span data-stu-id="70e32-699">Otherwise, a result type cannot be inferred for `F`.</span></span>

<span data-ttu-id="70e32-700">O ***inferido do tipo de retorno*** é determinado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="70e32-700">The ***inferred return type*** is determined as follows:</span></span>

*  <span data-ttu-id="70e32-701">Se `F` é assíncrono e o corpo da `F` é uma expressão classificada como nada ([classificações de expressão](expressions.md#expression-classifications)), ou um bloco de instruções onde não há instruções return têm expressões, é do tipo de retorno inferido `System.Threading.Tasks.Task`</span><span class="sxs-lookup"><span data-stu-id="70e32-701">If `F` is async and the body of `F` is either an expression classified as nothing ([Expression classifications](expressions.md#expression-classifications)), or a statement block where no return statements have expressions, the inferred return type is `System.Threading.Tasks.Task`</span></span>
*  <span data-ttu-id="70e32-702">Se `F` é assíncrono e tem um tipo de resultado inferido `T`, é do tipo de retorno inferido `System.Threading.Tasks.Task<T>`.</span><span class="sxs-lookup"><span data-stu-id="70e32-702">If `F` is async and has an inferred result type `T`, the inferred return type is `System.Threading.Tasks.Task<T>`.</span></span>
*  <span data-ttu-id="70e32-703">Se `F` é não assíncronas e tem um tipo de resultado inferido `T`, é do tipo de retorno inferido `T`.</span><span class="sxs-lookup"><span data-stu-id="70e32-703">If `F` is non-async and has an inferred result type `T`, the inferred return type is `T`.</span></span>
*  <span data-ttu-id="70e32-704">Caso contrário, um tipo de retorno não pode ser inferido para `F`.</span><span class="sxs-lookup"><span data-stu-id="70e32-704">Otherwise a return type cannot be inferred for `F`.</span></span>

<span data-ttu-id="70e32-705">Como um exemplo de inferência de tipo que envolvem funções anônimas, considere a `Select` método de extensão declarados no `System.Linq.Enumerable` classe:</span><span class="sxs-lookup"><span data-stu-id="70e32-705">As an example of type inference involving anonymous functions, consider the `Select` extension method declared in the `System.Linq.Enumerable` class:</span></span>
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

<span data-ttu-id="70e32-706">Supondo que o `System.Linq` namespace foi importado com um `using` cláusula e dada a classe `Customer` com um `Name` propriedade do tipo `string`, o `Select` método pode ser usado para selecionar os nomes de uma lista de clientes:</span><span class="sxs-lookup"><span data-stu-id="70e32-706">Assuming the `System.Linq` namespace was imported with a `using` clause, and given a class `Customer` with a `Name` property of type `string`, the `Select` method can be used to select the names of a list of customers:</span></span>
```csharp
List<Customer> customers = GetCustomerList();
IEnumerable<string> names = customers.Select(c => c.Name);
```

<span data-ttu-id="70e32-707">A invocação de método de extensão ([invocações de método de extensão](expressions.md#extension-method-invocations)) de `Select` é processada por reescrever a chamada para uma invocação de método estático:</span><span class="sxs-lookup"><span data-stu-id="70e32-707">The extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)) of `Select` is processed by rewriting the invocation to a static method invocation:</span></span>
```csharp
IEnumerable<string> names = Enumerable.Select(customers, c => c.Name);
```

<span data-ttu-id="70e32-708">Como argumentos de tipo não foram especificados explicitamente, a inferência de tipo é usada para inferir os argumentos de tipo.</span><span class="sxs-lookup"><span data-stu-id="70e32-708">Since type arguments were not explicitly specified, type inference is used to infer the type arguments.</span></span> <span data-ttu-id="70e32-709">Primeiro, o `customers` argumento está relacionado à `source` parâmetro, inferir `T` ser `Customer`.</span><span class="sxs-lookup"><span data-stu-id="70e32-709">First, the `customers` argument is related to the `source` parameter, inferring `T` to be `Customer`.</span></span> <span data-ttu-id="70e32-710">Em seguida, usando a função anônima digite descrito acima, do processo de inferência `c` recebe o tipo `Customer`e a expressão `c.Name` está relacionado ao tipo de retorno dos `selector` parâmetro, inferindo `S` ser `string`.</span><span class="sxs-lookup"><span data-stu-id="70e32-710">Then, using the anonymous function type inference process described above, `c` is given type `Customer`, and the expression `c.Name` is related to the return type of the `selector` parameter, inferring `S` to be `string`.</span></span> <span data-ttu-id="70e32-711">Portanto, a invocação é equivalente a</span><span class="sxs-lookup"><span data-stu-id="70e32-711">Thus, the invocation is equivalent to</span></span>
```csharp
Sequence.Select<Customer,string>(customers, (Customer c) => c.Name)
```
<span data-ttu-id="70e32-712">e o resultado é do tipo `IEnumerable<string>`.</span><span class="sxs-lookup"><span data-stu-id="70e32-712">and the result is of type `IEnumerable<string>`.</span></span>

<span data-ttu-id="70e32-713">O exemplo a seguir demonstra o tipo de função anônima como inferência de tipos permite que as informações de tipo "fluir" entre os argumentos em uma invocação de método genérico.</span><span class="sxs-lookup"><span data-stu-id="70e32-713">The following example demonstrates how anonymous function type inference allows type information to "flow" between arguments in a generic method invocation.</span></span> <span data-ttu-id="70e32-714">Com o método:</span><span class="sxs-lookup"><span data-stu-id="70e32-714">Given the method:</span></span>
```csharp
static Z F<X,Y,Z>(X value, Func<X,Y> f1, Func<Y,Z> f2) {
    return f2(f1(value));
}
```

<span data-ttu-id="70e32-715">Inferência de tipo para a invocação:</span><span class="sxs-lookup"><span data-stu-id="70e32-715">Type inference for the invocation:</span></span>
```csharp
double seconds = F("1:15:30", s => TimeSpan.Parse(s), t => t.TotalSeconds);
```
<span data-ttu-id="70e32-716">procede da seguinte maneira: primeiro, o argumento `"1:15:30"` está relacionado à `value` parâmetro, inferir `X` ser `string`.</span><span class="sxs-lookup"><span data-stu-id="70e32-716">proceeds as follows: First, the argument `"1:15:30"` is related to the `value` parameter, inferring `X` to be `string`.</span></span> <span data-ttu-id="70e32-717">Então, o parâmetro da primeira função anônima `s`, recebe o tipo inferido `string`e a expressão `TimeSpan.Parse(s)` está relacionado ao tipo de retorno de `f1`, inferindo `Y` ser `System.TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="70e32-717">Then, the parameter of the first anonymous function, `s`, is given the inferred type `string`, and the expression `TimeSpan.Parse(s)` is related to the return type of `f1`, inferring `Y` to be `System.TimeSpan`.</span></span> <span data-ttu-id="70e32-718">Por fim, o parâmetro da segunda função anônima, `t`, recebe o tipo inferido `System.TimeSpan`e a expressão `t.TotalSeconds` está relacionado ao tipo de retorno de `f2`, inferindo `Z` ser `double`.</span><span class="sxs-lookup"><span data-stu-id="70e32-718">Finally, the parameter of the second anonymous function, `t`, is given the inferred type `System.TimeSpan`, and the expression `t.TotalSeconds` is related to the return type of `f2`, inferring `Z` to be `double`.</span></span> <span data-ttu-id="70e32-719">Portanto, o resultado da invocação é do tipo `double`.</span><span class="sxs-lookup"><span data-stu-id="70e32-719">Thus, the result of the invocation is of type `double`.</span></span>

#### <a name="type-inference-for-conversion-of-method-groups"></a><span data-ttu-id="70e32-720">Inferência de tipo para conversão de grupos de método</span><span class="sxs-lookup"><span data-stu-id="70e32-720">Type inference for conversion of method groups</span></span>

<span data-ttu-id="70e32-721">Semelhante às chamadas de métodos genéricos, inferência de tipo também deve ser aplicada quando um grupo de métodos `M` que contém um método genérico é convertido em um tipo de delegado fornecidos `D` ([conversões de grupo de método](conversions.md#method-group-conversions)).</span><span class="sxs-lookup"><span data-stu-id="70e32-721">Similar to calls of generic methods, type inference must also be applied when a method group `M` containing a generic method is converted to a given delegate type `D` ([Method group conversions](conversions.md#method-group-conversions)).</span></span> <span data-ttu-id="70e32-722">Um método</span><span class="sxs-lookup"><span data-stu-id="70e32-722">Given a method</span></span>
```csharp
Tr M<X1...Xn>(T1 x1 ... Tm xm)
```
<span data-ttu-id="70e32-723">e o grupo de métodos `M` que está sendo atribuído ao tipo de delegado `D` a tarefa de inferência de tipo é encontrar os argumentos de tipo `S1...Sn` , de modo que a expressão:</span><span class="sxs-lookup"><span data-stu-id="70e32-723">and the method group `M` being assigned to the delegate type `D` the task of type inference is to find type arguments `S1...Sn` so that the expression:</span></span>
```csharp
M<S1...Sn>
```
<span data-ttu-id="70e32-724">se torna compatível ([declarações de delegado](delegates.md#delegate-declarations)) com `D`.</span><span class="sxs-lookup"><span data-stu-id="70e32-724">becomes compatible ([Delegate declarations](delegates.md#delegate-declarations)) with `D`.</span></span>

<span data-ttu-id="70e32-725">Diferente do algoritmo de inferência de tipo para chamadas de método genérico, nesse caso, há apenas o argumento *tipos*, nenhum argumento *expressões*.</span><span class="sxs-lookup"><span data-stu-id="70e32-725">Unlike the type inference algorithm for generic method calls, in this case there are only argument *types*, no argument *expressions*.</span></span> <span data-ttu-id="70e32-726">Em particular, não há nenhuma função anônima e, portanto, sem a necessidade de várias fases de inferência de tipos.</span><span class="sxs-lookup"><span data-stu-id="70e32-726">In particular, there are no anonymous functions and hence no need for multiple phases of inference.</span></span>

<span data-ttu-id="70e32-727">Em vez disso, todos os `Xi` são considerados *unfixed*e uma *inferência de tipos de limite inferior* é feita *do* cada tipo de argumento `Uj` de `D` *à* o tipo de parâmetro correspondente `Tj` de `M`.</span><span class="sxs-lookup"><span data-stu-id="70e32-727">Instead, all `Xi` are considered *unfixed*, and a *lower-bound inference* is made *from* each argument type `Uj` of `D` *to* the corresponding parameter type `Tj` of `M`.</span></span> <span data-ttu-id="70e32-728">Se qualquer uma da `Xi` não há limites foram encontrados, falha de inferência de tipo.</span><span class="sxs-lookup"><span data-stu-id="70e32-728">If for any of the `Xi` no bounds were found, type inference fails.</span></span> <span data-ttu-id="70e32-729">Caso contrário, todos os `Xi` estão *fixo* correspondente `Si`, que são o resultado de inferência de tipo.</span><span class="sxs-lookup"><span data-stu-id="70e32-729">Otherwise, all `Xi` are *fixed* to corresponding `Si`, which are the result of type inference.</span></span>

#### <a name="finding-the-best-common-type-of-a-set-of-expressions"></a><span data-ttu-id="70e32-730">Localizando o melhor tipo comum de um conjunto de expressões</span><span class="sxs-lookup"><span data-stu-id="70e32-730">Finding the best common type of a set of expressions</span></span>

<span data-ttu-id="70e32-731">Em alguns casos, um tipo comum deve ser deduzida para um conjunto de expressões.</span><span class="sxs-lookup"><span data-stu-id="70e32-731">In some cases, a common type needs to be inferred for a set of expressions.</span></span> <span data-ttu-id="70e32-732">Em particular, os tipos de elementos de matrizes de tipo implícito e os tipos de retorno de funções anônimas com *bloco* corpos encontram-se dessa maneira.</span><span class="sxs-lookup"><span data-stu-id="70e32-732">In particular, the element types of implicitly typed arrays and the return types of anonymous functions with *block* bodies are found in this way.</span></span>

<span data-ttu-id="70e32-733">Intuitivamente, dado um conjunto de expressões `E1...Em` essa inferência de tipos deve ser equivalente a chamar um método</span><span class="sxs-lookup"><span data-stu-id="70e32-733">Intuitively, given a set of expressions `E1...Em` this inference should be equivalent to calling a method</span></span>
```csharp
Tr M<X>(X x1 ... X xm)
```
<span data-ttu-id="70e32-734">com o `Ei` como argumentos.</span><span class="sxs-lookup"><span data-stu-id="70e32-734">with the `Ei` as arguments.</span></span>

<span data-ttu-id="70e32-735">Mais precisamente, a inferência começa com um *unfixed* variável de tipo `X`.</span><span class="sxs-lookup"><span data-stu-id="70e32-735">More precisely, the inference starts out with an *unfixed* type variable `X`.</span></span> <span data-ttu-id="70e32-736">*Inferências de tipo de saída* , em seguida, são feitas *partir* cada `Ei` *para* `X`.</span><span class="sxs-lookup"><span data-stu-id="70e32-736">*Output type inferences* are then made *from* each `Ei` *to* `X`.</span></span> <span data-ttu-id="70e32-737">Por fim, `X` está *fixo* e, se for bem-sucedido, resultante de tipo `S` é o melhor tipo comum resultante para as expressões.</span><span class="sxs-lookup"><span data-stu-id="70e32-737">Finally, `X` is *fixed* and, if successful, the resulting type `S` is the resulting best common type for the expressions.</span></span> <span data-ttu-id="70e32-738">Se nenhum `S` existir, as expressões não têm nenhum tipo comum melhor.</span><span class="sxs-lookup"><span data-stu-id="70e32-738">If no such `S` exists, the expressions have no best common type.</span></span>

### <a name="overload-resolution"></a><span data-ttu-id="70e32-739">Resolução de sobrecarga</span><span class="sxs-lookup"><span data-stu-id="70e32-739">Overload resolution</span></span>

<span data-ttu-id="70e32-740">Resolução de sobrecarga é um mecanismo de tempo de associação para selecionar o membro da função melhor invocar dada uma lista de argumentos e um conjunto de membros da função de candidato.</span><span class="sxs-lookup"><span data-stu-id="70e32-740">Overload resolution is a binding-time mechanism for selecting the best function member to invoke given an argument list and a set of candidate function members.</span></span> <span data-ttu-id="70e32-741">Resolução de sobrecarga seleciona o membro da função para invocar nos seguintes contextos distintos em c#:</span><span class="sxs-lookup"><span data-stu-id="70e32-741">Overload resolution selects the function member to invoke in the following distinct contexts within C#:</span></span>

*  <span data-ttu-id="70e32-742">Invocação de um método chamado em um *invocation_expression* ([invocações de método](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="70e32-742">Invocation of a method named in an *invocation_expression* ([Method invocations](expressions.md#method-invocations)).</span></span>
*  <span data-ttu-id="70e32-743">Invocação de um construtor de instância nomeada em um *object_creation_expression* ([expressões de criação do objeto](expressions.md#object-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="70e32-743">Invocation of an instance constructor named in an *object_creation_expression* ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span>
*  <span data-ttu-id="70e32-744">Invocação de um acessador de indexador por meio de um *element_access* ([acesso de elemento](expressions.md#element-access)).</span><span class="sxs-lookup"><span data-stu-id="70e32-744">Invocation of an indexer accessor through an *element_access* ([Element access](expressions.md#element-access)).</span></span>
*  <span data-ttu-id="70e32-745">Invocação de um operador predefinido ou definidos pelo usuário referenciada em uma expressão ([resolução de sobrecarga de operador unário](expressions.md#unary-operator-overload-resolution) e [resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="70e32-745">Invocation of a predefined or user-defined operator referenced in an expression ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution) and [Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)).</span></span>

<span data-ttu-id="70e32-746">Cada um desses contextos define o conjunto de membros da função release candidate e a lista de argumentos em sua própria maneira exclusiva, conforme descrito em detalhes nas seções listados acima.</span><span class="sxs-lookup"><span data-stu-id="70e32-746">Each of these contexts defines the set of candidate function members and the list of arguments in its own unique way, as described in detail in the sections listed above.</span></span> <span data-ttu-id="70e32-747">Por exemplo, o conjunto de candidatos para uma invocação de método não inclui métodos marcados `override` ([pesquisa de membro](expressions.md#member-lookup)), e métodos em uma classe base não são candidatos se qualquer método em uma classe derivada é aplicável ([ As invocações de método](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="70e32-747">For example, the set of candidates for a method invocation does not include methods marked `override` ([Member lookup](expressions.md#member-lookup)), and methods in a base class are not candidates if any method in a derived class is applicable ([Method invocations](expressions.md#method-invocations)).</span></span>

<span data-ttu-id="70e32-748">Depois que foram identificados os membros da função release candidate e a lista de argumentos, a seleção do membro da função recomendada é a mesma em todos os casos:</span><span class="sxs-lookup"><span data-stu-id="70e32-748">Once the candidate function members and the argument list have been identified, the selection of the best function member is the same in all cases:</span></span>

*  <span data-ttu-id="70e32-749">Dado o conjunto de membros da função candidato aplicável, a melhor função membro nesse conjunto é localizado.</span><span class="sxs-lookup"><span data-stu-id="70e32-749">Given the set of applicable candidate function members, the best function member in that set is located.</span></span> <span data-ttu-id="70e32-750">Se o conjunto contém apenas um membro da função, esse membro da função é o membro da função melhor.</span><span class="sxs-lookup"><span data-stu-id="70e32-750">If the set contains only one function member, then that function member is the best function member.</span></span> <span data-ttu-id="70e32-751">Caso contrário, o membro da função melhor será o membro de uma função que é melhor do que todos os outros membros da função em relação à lista de argumento fornecido, desde que cada membro da função é comparado com todos os outros membros da função usando as regras em [ Membro de função melhor](expressions.md#better-function-member).</span><span class="sxs-lookup"><span data-stu-id="70e32-751">Otherwise, the best function member is the one function member that is better than all other function members with respect to the given argument list, provided that each function member is compared to all other function members using the rules in [Better function member](expressions.md#better-function-member).</span></span> <span data-ttu-id="70e32-752">Se não houver exatamente um membro da função que é melhor do que todos os outros membros da função, em seguida, a invocação de membro da função é ambígua e ocorre um erro em tempo de vinculação.</span><span class="sxs-lookup"><span data-stu-id="70e32-752">If there is not exactly one function member that is better than all other function members, then the function member invocation is ambiguous and a binding-time error occurs.</span></span>

<span data-ttu-id="70e32-753">As seções a seguir definem o significado exato dos termos ***membro da função aplicáveis*** e ***melhor membro da função***.</span><span class="sxs-lookup"><span data-stu-id="70e32-753">The following sections define the exact meanings of the terms ***applicable function member*** and ***better function member***.</span></span>

#### <a name="applicable-function-member"></a><span data-ttu-id="70e32-754">Membro de função aplicáveis</span><span class="sxs-lookup"><span data-stu-id="70e32-754">Applicable function member</span></span>

<span data-ttu-id="70e32-755">Um membro da função deve ser um ***membro da função aplicáveis*** em relação a uma lista de argumentos `A` quando todos os itens a seguir forem verdadeiras:</span><span class="sxs-lookup"><span data-stu-id="70e32-755">A function member is said to be an ***applicable function member*** with respect to an argument list `A` when all of the following are true:</span></span>

*  <span data-ttu-id="70e32-756">Cada argumento na `A` corresponde a um parâmetro na declaração de membro da função, conforme descrito em [correspondente parâmetros](expressions.md#corresponding-parameters), e qualquer parâmetro não correspondente ao nenhum argumento é um parâmetro opcional.</span><span class="sxs-lookup"><span data-stu-id="70e32-756">Each argument in `A` corresponds to a parameter in the function member declaration as described in [Corresponding parameters](expressions.md#corresponding-parameters), and any parameter to which no argument corresponds is an optional parameter.</span></span>
*  <span data-ttu-id="70e32-757">Para cada argumento na `A`, o modo do argumento de passagem de parâmetro (ou seja, valor, `ref`, ou `out`) é idêntico para o modo de passagem de parâmetro do parâmetro correspondente, e</span><span class="sxs-lookup"><span data-stu-id="70e32-757">For each argument in `A`, the parameter passing mode of the argument (i.e., value, `ref`, or `out`) is identical to the parameter passing mode of the corresponding parameter, and</span></span>
   *  <span data-ttu-id="70e32-758">para um parâmetro de valor ou uma matriz de parâmetros, uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)) existe do argumento para o tipo do parâmetro correspondente, ou</span><span class="sxs-lookup"><span data-stu-id="70e32-758">for a value parameter or a parameter array, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from the argument to the type of the corresponding parameter, or</span></span>
   *  <span data-ttu-id="70e32-759">para um `ref` ou `out` parâmetro, o tipo do argumento é idêntico ao tipo do parâmetro correspondente.</span><span class="sxs-lookup"><span data-stu-id="70e32-759">for a `ref` or `out` parameter, the type of the argument is identical to the type of the corresponding parameter.</span></span> <span data-ttu-id="70e32-760">Afinal de contas, uma `ref` ou `out` parâmetro é um alias para o argumento passado.</span><span class="sxs-lookup"><span data-stu-id="70e32-760">After all, a `ref` or `out` parameter is an alias for the argument passed.</span></span>

<span data-ttu-id="70e32-761">Para um membro da função que inclui uma matriz de parâmetros, se o membro da função for aplicável, as regras acima, ele deve ser aplicável em sua ***forma normal***.</span><span class="sxs-lookup"><span data-stu-id="70e32-761">For a function member that includes a parameter array, if the function member is applicable by the above rules, it is said to be applicable in its ***normal form***.</span></span> <span data-ttu-id="70e32-762">Se um membro da função que inclui uma matriz de parâmetro não for aplicável em sua forma normal, o membro da função em vez disso, pode ser aplicável em sua ***expandido formulário***:</span><span class="sxs-lookup"><span data-stu-id="70e32-762">If a function member that includes a parameter array is not applicable in its normal form, the function member may instead be applicable in its ***expanded form***:</span></span>

*  <span data-ttu-id="70e32-763">A forma expandida é construída, substituindo a matriz de parâmetros na declaração de membro da função com zero ou mais parâmetros de valor do tipo de elemento do parâmetro de matriz, que o número de argumentos na lista de argumentos `A` corresponde ao total número de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="70e32-763">The expanded form is constructed by replacing the parameter array in the function member declaration with zero or more value parameters of the element type of the parameter array such that the number of arguments in the argument list `A` matches the total number of parameters.</span></span> <span data-ttu-id="70e32-764">Se `A` tem menos argumentos que o número de parâmetros fixos na declaração de membro da função, a forma expandida do membro da função não pode ser construída e, portanto, não é aplicável.</span><span class="sxs-lookup"><span data-stu-id="70e32-764">If `A` has fewer arguments than the number of fixed parameters in the function member declaration, the expanded form of the function member cannot be constructed and is thus not applicable.</span></span>
*  <span data-ttu-id="70e32-765">Caso contrário, a forma expandida é aplicável se cada argumento na `A` o modo de passagem de parâmetro do argumento é idêntico para o modo de passagem de parâmetro do parâmetro correspondente, e</span><span class="sxs-lookup"><span data-stu-id="70e32-765">Otherwise, the expanded form is applicable if for each argument in `A` the parameter passing mode of the argument is identical to the parameter passing mode of the corresponding parameter, and</span></span>
   *  <span data-ttu-id="70e32-766">para um parâmetro de valor fixo ou criados pela expansão, uma conversão implícita de um parâmetro de valor ([conversões implícitas](conversions.md#implicit-conversions)) existe do tipo do argumento para o tipo do parâmetro correspondente, ou</span><span class="sxs-lookup"><span data-stu-id="70e32-766">for a fixed value parameter or a value parameter created by the expansion, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from the type of the argument to the type of the corresponding parameter, or</span></span>
   *  <span data-ttu-id="70e32-767">para um `ref` ou `out` parâmetro, o tipo do argumento é idêntico ao tipo do parâmetro correspondente.</span><span class="sxs-lookup"><span data-stu-id="70e32-767">for a `ref` or `out` parameter, the type of the argument is identical to the type of the corresponding parameter.</span></span>

#### <a name="better-function-member"></a><span data-ttu-id="70e32-768">Membro de função melhor</span><span class="sxs-lookup"><span data-stu-id="70e32-768">Better function member</span></span>

<span data-ttu-id="70e32-769">A fim de determinar o membro da função melhor, uma lista de argumentos simplificada um é construída que contém apenas as expressões do argumento em si na ordem em que eles aparecem na lista de argumentos original.</span><span class="sxs-lookup"><span data-stu-id="70e32-769">For the purposes of determining the better function member, a stripped-down argument list A is constructed containing just the argument expressions themselves in the order they appear in the original argument list.</span></span>

<span data-ttu-id="70e32-770">Listas de parâmetros para cada função candidato membros são construídos da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="70e32-770">Parameter lists for each of the candidate function members are constructed in the following way:</span></span>

*  <span data-ttu-id="70e32-771">A forma expandida será usada se o membro da função foi aplicável apenas a forma expandida.</span><span class="sxs-lookup"><span data-stu-id="70e32-771">The expanded form is used if the function member was applicable only in the expanded form.</span></span>
*  <span data-ttu-id="70e32-772">Parâmetros opcionais sem argumentos correspondentes são removidos da lista de parâmetros</span><span class="sxs-lookup"><span data-stu-id="70e32-772">Optional parameters with no corresponding arguments are removed from the parameter list</span></span>
*  <span data-ttu-id="70e32-773">Os parâmetros são reordenados para que eles ocorram na mesma posição como o argumento correspondente na lista de argumentos.</span><span class="sxs-lookup"><span data-stu-id="70e32-773">The parameters are reordered so that they occur at the same position as the corresponding argument in the argument list.</span></span>

<span data-ttu-id="70e32-774">Dada uma lista de argumentos `A` com um conjunto de expressões de argumento `{E1, E2, ..., En}` e dois membros da função aplicável `Mp` e `Mq` com tipos de parâmetro `{P1, P2, ..., Pn}` e `{Q1, Q2, ..., Qn}`, `Mp` é definido como um ***melhor membro da função*** que `Mq` se</span><span class="sxs-lookup"><span data-stu-id="70e32-774">Given an argument list `A` with a set of argument expressions `{E1, E2, ..., En}` and two applicable function members `Mp` and `Mq` with parameter types `{P1, P2, ..., Pn}` and `{Q1, Q2, ..., Qn}`, `Mp` is defined to be a ***better function member*** than `Mq` if</span></span>

*  <span data-ttu-id="70e32-775">para cada argumento, a conversão implícita da `Ex` à `Qx` não é melhor do que a conversão implícita de `Ex` para `Px`, e</span><span class="sxs-lookup"><span data-stu-id="70e32-775">for each argument, the implicit conversion from `Ex` to `Qx` is not better than the implicit conversion from `Ex` to `Px`, and</span></span>
*  <span data-ttu-id="70e32-776">para pelo menos um argumento, a conversão de `Ex` à `Px` é melhor do que a conversão de `Ex` para `Qx`.</span><span class="sxs-lookup"><span data-stu-id="70e32-776">for at least one argument, the conversion from `Ex` to `Px` is better than the conversion from `Ex` to `Qx`.</span></span>

<span data-ttu-id="70e32-777">Ao fazer isso, se `Mp` ou `Mq` é aplicável em sua forma expandida, em seguida, `Px` ou `Qx` refere-se a um parâmetro no formato expandido da lista de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="70e32-777">When performing this evaluation, if `Mp` or `Mq` is applicable in its expanded form, then `Px` or `Qx` refers to a parameter in the expanded form of the parameter list.</span></span>

<span data-ttu-id="70e32-778">Caso o parâmetro de tipo sequências `{P1, P2, ..., Pn}` e `{Q1, Q2, ..., Qn}` são equivalentes (ou seja, cada `Pi` tem uma conversão de identidade para os respectivos `Qi`), as seguintes regras de desempate são aplicadas em ordem, para determinar o melhor membro de função.</span><span class="sxs-lookup"><span data-stu-id="70e32-778">In case the parameter type sequences `{P1, P2, ..., Pn}` and `{Q1, Q2, ..., Qn}` are equivalent (i.e. each `Pi` has an identity conversion to the corresponding `Qi`), the following tie-breaking rules are applied, in order, to determine the better function member.</span></span>

*  <span data-ttu-id="70e32-779">Se `Mp` é um método não genérico e `Mq` é um método genérico, em seguida, `Mp` é melhor do que `Mq`.</span><span class="sxs-lookup"><span data-stu-id="70e32-779">If `Mp` is a non-generic method and `Mq` is a generic method, then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="70e32-780">Caso contrário, se `Mp` é aplicável em sua forma normal e `Mq` tem um `params` de matriz e é aplicável apenas em sua forma expandida, em seguida, `Mp` é melhor do que `Mq`.</span><span class="sxs-lookup"><span data-stu-id="70e32-780">Otherwise, if `Mp` is applicable in its normal form and `Mq` has a `params` array and is applicable only in its expanded form, then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="70e32-781">Caso contrário, se `Mp` mais declarou parâmetros que `Mq`, em seguida, `Mp` é melhor do que `Mq`.</span><span class="sxs-lookup"><span data-stu-id="70e32-781">Otherwise, if `Mp` has more declared parameters than `Mq`, then `Mp` is better than `Mq`.</span></span> <span data-ttu-id="70e32-782">Isso pode ocorrer se os dois métodos têm `params` matrizes e são aplicável apenas em seus formulários expandidos.</span><span class="sxs-lookup"><span data-stu-id="70e32-782">This can occur if both methods have `params` arrays and are applicable only in their expanded forms.</span></span>
*  <span data-ttu-id="70e32-783">Caso contrário se todos os parâmetros de `Mp` tem um argumento correspondente, enquanto os argumentos padrão precisam ser substituídos pelo menos um parâmetro opcional nas `Mq` , em seguida, `Mp` é melhor do que `Mq`.</span><span class="sxs-lookup"><span data-stu-id="70e32-783">Otherwise if all parameters of `Mp` have a corresponding argument whereas default arguments need to be substituted for at least one optional parameter in `Mq` then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="70e32-784">Caso contrário, se `Mp` tem tipos de parâmetro mais específicos que `Mq`, em seguida, `Mp` é melhor do que `Mq`.</span><span class="sxs-lookup"><span data-stu-id="70e32-784">Otherwise, if `Mp` has more specific parameter types than `Mq`, then `Mp` is better than `Mq`.</span></span> <span data-ttu-id="70e32-785">Deixe `{R1, R2, ..., Rn}` e `{S1, S2, ..., Sn}` representam os tipos de parâmetro não instanciado e não expandidas de `Mp` e `Mq`.</span><span class="sxs-lookup"><span data-stu-id="70e32-785">Let `{R1, R2, ..., Rn}` and `{S1, S2, ..., Sn}` represent the uninstantiated and unexpanded parameter types of `Mp` and `Mq`.</span></span> <span data-ttu-id="70e32-786">`Mp`do tipos de parâmetro são mais específicos que `Mq`do se, para cada parâmetro, `Rx` não é menos específicas `Sx`e, pelo menos um parâmetro, `Rx` é mais específico que `Sx`:</span><span class="sxs-lookup"><span data-stu-id="70e32-786">`Mp`'s parameter types are more specific than `Mq`'s if, for each parameter, `Rx` is not less specific than `Sx`, and, for at least one parameter, `Rx` is more specific than `Sx`:</span></span>
   *  <span data-ttu-id="70e32-787">Um parâmetro de tipo é menos específico um parâmetro sem tipo.</span><span class="sxs-lookup"><span data-stu-id="70e32-787">A type parameter is less specific than a non-type parameter.</span></span>
   *  <span data-ttu-id="70e32-788">Recursivamente, um tipo construído é mais específico do que outro tipo construído (com o mesmo número de argumentos de tipo) se pelo menos um argumento de tipo é mais específico e nenhum argumento de tipo é menos específico do argumento de tipo correspondente no outro.</span><span class="sxs-lookup"><span data-stu-id="70e32-788">Recursively, a constructed type is more specific than another constructed type (with the same number of type arguments) if at least one type argument is more specific and no type argument is less specific than the corresponding type argument in the other.</span></span>
   *  <span data-ttu-id="70e32-789">Um tipo de matriz é mais específico do que outro tipo de matriz (com o mesmo número de dimensões), se o tipo de elemento da primeira é mais específico do que o tipo de elemento da segunda.</span><span class="sxs-lookup"><span data-stu-id="70e32-789">An array type is more specific than another array type (with the same number of dimensions) if the element type of the first is more specific than the element type of the second.</span></span>
*  <span data-ttu-id="70e32-790">Caso contrário, se um membro é um operador sem comparação de precisão e a outra é um operador elevado, a sem comparação de precisão é melhor.</span><span class="sxs-lookup"><span data-stu-id="70e32-790">Otherwise if one member is a non-lifted operator and  the other is a lifted operator, the non-lifted one is better.</span></span>
*  <span data-ttu-id="70e32-791">Caso contrário, nenhum membro da função é melhor.</span><span class="sxs-lookup"><span data-stu-id="70e32-791">Otherwise, neither function member is better.</span></span>

#### <a name="better-conversion-from-expression"></a><span data-ttu-id="70e32-792">Melhor conversão de expressão</span><span class="sxs-lookup"><span data-stu-id="70e32-792">Better conversion from expression</span></span>

<span data-ttu-id="70e32-793">Dada uma conversão implícita `C1` que converte de uma expressão `E` a um tipo `T1`e uma conversão implícita `C2` que converte de uma expressão `E` a um tipo `T2`, `C1` é um ***melhor conversão*** que `C2` se `E` corresponder exatamente a `T2` e mantém pelo menos um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="70e32-793">Given an implicit conversion `C1` that converts from an expression `E` to a type `T1`, and an implicit conversion `C2` that converts from an expression `E` to a type `T2`, `C1` is a ***better conversion*** than `C2` if `E` does not exactly match `T2` and at least one of the following holds:</span></span>

* <span data-ttu-id="70e32-794">`E` corresponde exatamente `T1` ([correspondem exatamente a expressão](expressions.md#exactly-matching-expression))</span><span class="sxs-lookup"><span data-stu-id="70e32-794">`E` exactly matches `T1` ([Exactly matching Expression](expressions.md#exactly-matching-expression))</span></span>
* <span data-ttu-id="70e32-795">`T1` é um destino de conversão melhor que `T2` ([melhor destino da conversão](expressions.md#better-conversion-target))</span><span class="sxs-lookup"><span data-stu-id="70e32-795">`T1` is a better conversion target than `T2` ([Better conversion target](expressions.md#better-conversion-target))</span></span>

#### <a name="exactly-matching-expression"></a><span data-ttu-id="70e32-796">Expressão exatamente correspondente</span><span class="sxs-lookup"><span data-stu-id="70e32-796">Exactly matching Expression</span></span>

<span data-ttu-id="70e32-797">Considerando uma expressão `E` e um tipo `T`, `E` corresponda exatamente ao `T` se tiver mantido por um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="70e32-797">Given an expression `E` and a type `T`, `E` exactly matches `T` if one of the following holds:</span></span>

*  <span data-ttu-id="70e32-798">`E` tem um tipo `S`, e existe uma conversão de identidade de `S` para `T`</span><span class="sxs-lookup"><span data-stu-id="70e32-798">`E` has a type `S`, and an identity conversion exists from `S` to `T`</span></span>
*  <span data-ttu-id="70e32-799">`E` é uma função anônima, `T` é um tipo de delegado `D` ou um tipo de árvore de expressão `Expression<D>` e mantém por um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="70e32-799">`E` is an anonymous function, `T` is either a delegate type `D` or an expression tree type `Expression<D>` and one of the following holds:</span></span>
   *  <span data-ttu-id="70e32-800">Um tipo de retorno inferido `X` existe para o `E` no contexto da lista de parâmetros de `D` ([tipo de retorno inferido](expressions.md#inferred-return-type)), e existe uma conversão de identidade de `X` para o tipo de retorno `D`</span><span class="sxs-lookup"><span data-stu-id="70e32-800">An inferred return type `X` exists for `E` in the context of the parameter list of `D` ([Inferred return type](expressions.md#inferred-return-type)), and an identity conversion exists from `X` to the return type of `D`</span></span>
   *  <span data-ttu-id="70e32-801">Qualquer um dos `E` é não assíncronas e `D` tem um tipo de retorno `Y` ou `E` é assíncrono e `D` tem um tipo de retorno `Task<Y>`, e uma das opções a seguir contém:</span><span class="sxs-lookup"><span data-stu-id="70e32-801">Either `E` is non-async and `D` has a return type `Y` or `E` is async and `D` has a return type `Task<Y>`, and one of the following holds:</span></span>
      * <span data-ttu-id="70e32-802">O corpo da `E` é uma expressão que exatamente correspondências `Y`</span><span class="sxs-lookup"><span data-stu-id="70e32-802">The body of `E` is an expression that exactly matches `Y`</span></span>
      * <span data-ttu-id="70e32-803">O corpo da `E` é um bloco de instrução onde cada instrução return retorna uma expressão que exatamente correspondências `Y`</span><span class="sxs-lookup"><span data-stu-id="70e32-803">The body of `E` is a statement block where every return statement returns an expression that exactly matches `Y`</span></span>

#### <a name="better-conversion-target"></a><span data-ttu-id="70e32-804">Melhor destino da conversão</span><span class="sxs-lookup"><span data-stu-id="70e32-804">Better conversion target</span></span>

<span data-ttu-id="70e32-805">Dois tipos diferentes de dado `T1` e `T2`, `T1` é um destino de conversão melhor que `T2` se nenhuma conversão implícita de `T2` para `T1` existir, e pelo menos um dos seguintes contém:</span><span class="sxs-lookup"><span data-stu-id="70e32-805">Given two different types `T1` and `T2`, `T1` is a better conversion target than `T2` if no implicit conversion from `T2` to `T1` exists, and at least one of the following holds:</span></span>

*  <span data-ttu-id="70e32-806">Uma conversão implícita da `T1` para `T2` existe</span><span class="sxs-lookup"><span data-stu-id="70e32-806">An implicit conversion from `T1` to `T2` exists</span></span>
*  <span data-ttu-id="70e32-807">`T1` é um tipo de delegado `D1` ou um tipo de árvore de expressão `Expression<D1>`, `T2` é um tipo de delegado `D2` ou um tipo de árvore de expressão `Expression<D2>`, `D1` tem um tipo de retorno `S1` e uma da contém a seguinte:</span><span class="sxs-lookup"><span data-stu-id="70e32-807">`T1` is either a delegate type `D1` or an expression tree type `Expression<D1>`, `T2` is either a delegate type `D2` or an expression tree type `Expression<D2>`, `D1` has a return type `S1` and one of the following holds:</span></span>
   * <span data-ttu-id="70e32-808">`D2` void está retornando</span><span class="sxs-lookup"><span data-stu-id="70e32-808">`D2` is void returning</span></span>
   * <span data-ttu-id="70e32-809">`D2` tem um tipo de retorno `S2`, e `S1` é um destino de conversão melhor que `S2`</span><span class="sxs-lookup"><span data-stu-id="70e32-809">`D2` has a return type `S2`, and `S1` is a better conversion target than `S2`</span></span>
*  <span data-ttu-id="70e32-810">`T1` está `Task<S1>`, `T2` é `Task<S2>`, e `S1` é um destino de conversão melhor que `S2`</span><span class="sxs-lookup"><span data-stu-id="70e32-810">`T1` is `Task<S1>`, `T2` is `Task<S2>`, and `S1` is a better conversion target than `S2`</span></span>
*  <span data-ttu-id="70e32-811">`T1` está `S1` ou `S1?` onde `S1` é um tipo integral com sinal, e `T2` está `S2` ou `S2?` onde `S2` é um tipo integral sem sinal.</span><span class="sxs-lookup"><span data-stu-id="70e32-811">`T1` is `S1` or `S1?` where `S1` is a signed integral type, and `T2` is `S2` or `S2?` where `S2` is an unsigned integral type.</span></span> <span data-ttu-id="70e32-812">Especificamente:</span><span class="sxs-lookup"><span data-stu-id="70e32-812">Specifically:</span></span>
   * <span data-ttu-id="70e32-813">`S1` está `sbyte` e `S2` é `byte`, `ushort`, `uint`, ou `ulong`</span><span class="sxs-lookup"><span data-stu-id="70e32-813">`S1` is `sbyte` and `S2` is `byte`, `ushort`, `uint`, or `ulong`</span></span>
   * <span data-ttu-id="70e32-814">`S1` está `short` e `S2` é `ushort`, `uint`, ou `ulong`</span><span class="sxs-lookup"><span data-stu-id="70e32-814">`S1` is `short` and `S2` is `ushort`, `uint`, or `ulong`</span></span>
   * <span data-ttu-id="70e32-815">`S1` está `int` e `S2` é `uint`, ou `ulong`</span><span class="sxs-lookup"><span data-stu-id="70e32-815">`S1` is `int` and `S2` is `uint`, or `ulong`</span></span>
   * <span data-ttu-id="70e32-816">`S1` está `long` e `S2` é `ulong`</span><span class="sxs-lookup"><span data-stu-id="70e32-816">`S1` is `long` and `S2` is `ulong`</span></span>

#### <a name="overloading-in-generic-classes"></a><span data-ttu-id="70e32-817">Sobrecarga em classes genéricas</span><span class="sxs-lookup"><span data-stu-id="70e32-817">Overloading in generic classes</span></span>

<span data-ttu-id="70e32-818">Embora assinaturas conforme declarado devem ser exclusivas, é possível que a substituição de argumentos de tipo resulta em assinaturas idênticas.</span><span class="sxs-lookup"><span data-stu-id="70e32-818">While signatures as declared must be unique, it is possible that substitution of type arguments results in identical signatures.</span></span> <span data-ttu-id="70e32-819">Nesses casos, as regras de resolução de sobrecarga acima de desempate escolherá o membro mais específico.</span><span class="sxs-lookup"><span data-stu-id="70e32-819">In such cases, the tie-breaking rules of overload resolution above will pick the most specific member.</span></span>

<span data-ttu-id="70e32-820">Os exemplos a seguir mostram as sobrecargas que são válidas e inválidas de acordo com essa regra:</span><span class="sxs-lookup"><span data-stu-id="70e32-820">The following examples show overloads that are valid and invalid according to this rule:</span></span>

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

### <a name="compile-time-checking-of-dynamic-overload-resolution"></a><span data-ttu-id="70e32-821">Verificação de resolução de sobrecarga de dinâmica de tempo de compilação</span><span class="sxs-lookup"><span data-stu-id="70e32-821">Compile-time checking of dynamic overload resolution</span></span>

<span data-ttu-id="70e32-822">Para operações de ligação mais dinamicamente o conjunto de possíveis candidatos para a resolução é desconhecido no tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-822">For most dynamically bound operations the set of possible candidates for resolution is unknown at compile-time.</span></span> <span data-ttu-id="70e32-823">Em alguns casos, no entanto o conjunto de candidato é conhecido no tempo de compilação:</span><span class="sxs-lookup"><span data-stu-id="70e32-823">In certain cases, however the candidate set is known at compile-time:</span></span>

*  <span data-ttu-id="70e32-824">Chamadas de método estático com argumentos dinâmicos</span><span class="sxs-lookup"><span data-stu-id="70e32-824">Static method calls with dynamic arguments</span></span>
*  <span data-ttu-id="70e32-825">Chamadas de método de instância em que o receptor não for uma expressão dinâmica</span><span class="sxs-lookup"><span data-stu-id="70e32-825">Instance method calls where the receiver is not a dynamic expression</span></span>
*  <span data-ttu-id="70e32-826">Chamadas de indexador em que o receptor não for uma expressão dinâmica</span><span class="sxs-lookup"><span data-stu-id="70e32-826">Indexer calls where the receiver is not a dynamic expression</span></span>
*  <span data-ttu-id="70e32-827">Chamadas de construtor com argumentos dinâmicos</span><span class="sxs-lookup"><span data-stu-id="70e32-827">Constructor calls with dynamic arguments</span></span>

<span data-ttu-id="70e32-828">Nesses casos, uma verificação limitada de tempo de compilação é executada para cada candidato ver se qualquer um deles, possivelmente, pode aplicar em tempo de execução. Essa verificação consiste as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="70e32-828">In these cases a limited compile-time check is performed for each candidate to see if any of them could possibly apply at run-time.This check consists of the following steps:</span></span>

*  <span data-ttu-id="70e32-829">Inferência de tipo parcial: qualquer tipo de argumento que não depende diretamente ou indiretamente um argumento do tipo `dynamic` é inferido usando as regras da [inferência de tipo](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="70e32-829">Partial type inference: Any type argument that does not depend directly or indirectly on an argument of type `dynamic` is inferred using the rules of [Type inference](expressions.md#type-inference).</span></span> <span data-ttu-id="70e32-830">Os argumentos de tipo restantes são desconhecidos.</span><span class="sxs-lookup"><span data-stu-id="70e32-830">The remaining type arguments are unknown.</span></span>
*  <span data-ttu-id="70e32-831">Verificação de aplicabilidade parcial: aplicabilidade é verificada de acordo com a [membro da função aplicável](expressions.md#applicable-function-member), mas ignorando os parâmetros cujos tipos são desconhecidos.</span><span class="sxs-lookup"><span data-stu-id="70e32-831">Partial applicability check: Applicability is checked according to [Applicable function member](expressions.md#applicable-function-member), but ignoring parameters whose types are unknown.</span></span>
*  <span data-ttu-id="70e32-832">Se nenhum candidato passar nesse teste, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-832">If no candidate passes this test, a compile-time error occurs.</span></span>

### <a name="function-member-invocation"></a><span data-ttu-id="70e32-833">Invocação de membro de função</span><span class="sxs-lookup"><span data-stu-id="70e32-833">Function member invocation</span></span>

<span data-ttu-id="70e32-834">Esta seção descreve o processo que ocorre em tempo de execução para invocar um membro de função específica.</span><span class="sxs-lookup"><span data-stu-id="70e32-834">This section describes the process that takes place at run-time to invoke a particular function member.</span></span> <span data-ttu-id="70e32-835">Supõe-se que um processo de tempo de associação já determinou que o membro específico para invocar, possivelmente, aplicando resolução de sobrecarga para um conjunto de membros da função de candidato.</span><span class="sxs-lookup"><span data-stu-id="70e32-835">It is assumed that a binding-time process has already determined the particular member to invoke, possibly by applying overload resolution to a set of candidate function members.</span></span>

<span data-ttu-id="70e32-836">Para fins de descrever o processo de invocação, os membros da função são divididos em duas categorias:</span><span class="sxs-lookup"><span data-stu-id="70e32-836">For purposes of describing the invocation process, function members are divided into two categories:</span></span>

*  <span data-ttu-id="70e32-837">Membros de função estática.</span><span class="sxs-lookup"><span data-stu-id="70e32-837">Static function members.</span></span> <span data-ttu-id="70e32-838">Esses são métodos estáticos, construtores de instância, os acessadores de propriedade estática e operadores definidos pelo usuário.</span><span class="sxs-lookup"><span data-stu-id="70e32-838">These are instance constructors, static methods, static property accessors, and user-defined operators.</span></span> <span data-ttu-id="70e32-839">Membros da função estáticos são sempre não virtual.</span><span class="sxs-lookup"><span data-stu-id="70e32-839">Static function members are always non-virtual.</span></span>
*  <span data-ttu-id="70e32-840">Membros da função de instância.</span><span class="sxs-lookup"><span data-stu-id="70e32-840">Instance function members.</span></span> <span data-ttu-id="70e32-841">Esses são métodos de instância, os acessadores de propriedade de instância e acessadores de indexador.</span><span class="sxs-lookup"><span data-stu-id="70e32-841">These are instance methods, instance property accessors, and indexer accessors.</span></span> <span data-ttu-id="70e32-842">Membros da função de instância são não virtual ou virtual e sempre são invocados em uma instância específica.</span><span class="sxs-lookup"><span data-stu-id="70e32-842">Instance function members are either non-virtual or virtual, and are always invoked on a particular instance.</span></span> <span data-ttu-id="70e32-843">A instância é calculada por uma expressão de instância, e se torna acessível dentro do membro da função como `this` ([esse acesso](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="70e32-843">The instance is computed by an instance expression, and it becomes accessible within the function member as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="70e32-844">O processamento de tempo de execução de uma invocação de membro de função consiste em etapas a seguir, onde `M` é o membro da função e, se `M` é um membro de instância, `E` é a expressão da instância:</span><span class="sxs-lookup"><span data-stu-id="70e32-844">The run-time processing of a function member invocation consists of the following steps, where `M` is the function member and, if `M` is an instance member, `E` is the instance expression:</span></span>

*  <span data-ttu-id="70e32-845">Se `M` é um membro da função estática:</span><span class="sxs-lookup"><span data-stu-id="70e32-845">If `M` is a static function member:</span></span>
   * <span data-ttu-id="70e32-846">A lista de argumentos é avaliada, conforme descrito em [listas de argumentos](expressions.md#argument-lists).</span><span class="sxs-lookup"><span data-stu-id="70e32-846">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="70e32-847">`M` é invocado.</span><span class="sxs-lookup"><span data-stu-id="70e32-847">`M` is invoked.</span></span>

*  <span data-ttu-id="70e32-848">Se `M` é um membro da função de instância declarado em um *value_type*:</span><span class="sxs-lookup"><span data-stu-id="70e32-848">If `M` is an instance function member declared in a *value_type*:</span></span>
   * <span data-ttu-id="70e32-849">`E` é avaliado.</span><span class="sxs-lookup"><span data-stu-id="70e32-849">`E` is evaluated.</span></span> <span data-ttu-id="70e32-850">Se essa avaliação causa uma exceção, nenhuma outra etapa é executada.</span><span class="sxs-lookup"><span data-stu-id="70e32-850">If this evaluation causes an exception, then no further steps are executed.</span></span>
   * <span data-ttu-id="70e32-851">Se `E` não é classificado como uma variável e, em seguida, uma variável local temporária do `E`do tipo é criado e o valor de `E` é atribuído a essa variável.</span><span class="sxs-lookup"><span data-stu-id="70e32-851">If `E` is not classified as a variable, then a temporary local variable of `E`'s type is created and the value of `E` is assigned to that variable.</span></span> <span data-ttu-id="70e32-852">`E` em seguida, ser reclassificado como uma referência para a variável local temporário.</span><span class="sxs-lookup"><span data-stu-id="70e32-852">`E` is then reclassified as a reference to that temporary local variable.</span></span> <span data-ttu-id="70e32-853">Variável temporária é acessível como `this` dentro de `M`, mas não em qualquer outra forma.</span><span class="sxs-lookup"><span data-stu-id="70e32-853">The temporary variable is accessible as `this` within `M`, but not in any other way.</span></span> <span data-ttu-id="70e32-854">Assim, somente quando `E` é uma variável true é possível para o chamador observar as alterações que `M` faz com `this`.</span><span class="sxs-lookup"><span data-stu-id="70e32-854">Thus, only when `E` is a true variable is it possible for the caller to observe the changes that `M` makes to `this`.</span></span>
   * <span data-ttu-id="70e32-855">A lista de argumentos é avaliada, conforme descrito em [listas de argumentos](expressions.md#argument-lists).</span><span class="sxs-lookup"><span data-stu-id="70e32-855">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="70e32-856">`M` é invocado.</span><span class="sxs-lookup"><span data-stu-id="70e32-856">`M` is invoked.</span></span> <span data-ttu-id="70e32-857">A variável referenciada por `E` torna-se a variável referenciada por `this`.</span><span class="sxs-lookup"><span data-stu-id="70e32-857">The variable referenced by `E` becomes the variable referenced by `this`.</span></span>

*  <span data-ttu-id="70e32-858">Se `M` é um membro da função de instância declarado em um *reference_type*:</span><span class="sxs-lookup"><span data-stu-id="70e32-858">If `M` is an instance function member declared in a *reference_type*:</span></span>
   * <span data-ttu-id="70e32-859">`E` é avaliado.</span><span class="sxs-lookup"><span data-stu-id="70e32-859">`E` is evaluated.</span></span> <span data-ttu-id="70e32-860">Se essa avaliação causa uma exceção, nenhuma outra etapa é executada.</span><span class="sxs-lookup"><span data-stu-id="70e32-860">If this evaluation causes an exception, then no further steps are executed.</span></span>
   * <span data-ttu-id="70e32-861">A lista de argumentos é avaliada, conforme descrito em [listas de argumentos](expressions.md#argument-lists).</span><span class="sxs-lookup"><span data-stu-id="70e32-861">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="70e32-862">Se o tipo de `E` é um *value_type*, uma conversão boxing ([conversões Boxing](types.md#boxing-conversions)) é executada para converter `E` para o tipo `object`, e `E` é considerado para ser do tipo `object` nas etapas a seguir.</span><span class="sxs-lookup"><span data-stu-id="70e32-862">If the type of `E` is a *value_type*, a boxing conversion ([Boxing conversions](types.md#boxing-conversions)) is performed to convert `E` to type `object`, and `E` is considered to be of type `object` in the following steps.</span></span> <span data-ttu-id="70e32-863">Nesse caso, `M` só pode ser um membro de `System.Object`.</span><span class="sxs-lookup"><span data-stu-id="70e32-863">In this case, `M` could only be a member of `System.Object`.</span></span>
   * <span data-ttu-id="70e32-864">O valor de `E` é verificado para ser válido.</span><span class="sxs-lookup"><span data-stu-id="70e32-864">The value of `E` is checked to be valid.</span></span> <span data-ttu-id="70e32-865">Se o valor de `E` está `null`, um `System.NullReferenceException` é lançada e nenhuma outra etapa é executada.</span><span class="sxs-lookup"><span data-stu-id="70e32-865">If the value of `E` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
   * <span data-ttu-id="70e32-866">A implementação de membro da função para invocar é determinada:</span><span class="sxs-lookup"><span data-stu-id="70e32-866">The function member implementation to invoke is determined:</span></span>
     * <span data-ttu-id="70e32-867">Se o tempo de associação de tipo de `E` é uma interface, o membro da função para invocar é a implementação de `M` fornecida pelo tipo de tempo de execução da instância referenciada por `E`.</span><span class="sxs-lookup"><span data-stu-id="70e32-867">If the binding-time type of `E` is an interface, the function member to invoke is the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span> <span data-ttu-id="70e32-868">Este membro da função é determinado pela aplicação das regras de mapeamento de interface ([mapeamento de Interface](interfaces.md#interface-mapping)) para determinar a implementação de `M` fornecida pelo tipo de tempo de execução da instância referenciada por `E`.</span><span class="sxs-lookup"><span data-stu-id="70e32-868">This function member is determined by applying the interface mapping rules ([Interface mapping](interfaces.md#interface-mapping)) to determine the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span>
     * <span data-ttu-id="70e32-869">Caso contrário, se `M` é um membro da função virtual, o membro da função para invocar é a implementação de `M` fornecida pelo tipo de tempo de execução da instância referenciada por `E`.</span><span class="sxs-lookup"><span data-stu-id="70e32-869">Otherwise, if `M` is a virtual function member, the function member to invoke is the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span> <span data-ttu-id="70e32-870">Este membro da função é determinado pela aplicação das regras para determinar a implementação mais derivada ([métodos virtuais](classes.md#virtual-methods)) de `M` em relação ao tipo de tempo de execução da instância referenciada por `E`.</span><span class="sxs-lookup"><span data-stu-id="70e32-870">This function member is determined by applying the rules for determining the most derived implementation ([Virtual methods](classes.md#virtual-methods)) of `M` with respect to the run-time type of the instance referenced by `E`.</span></span>
     * <span data-ttu-id="70e32-871">Caso contrário, `M` é um membro da função não virtual e é o membro da função para invocar `M` em si.</span><span class="sxs-lookup"><span data-stu-id="70e32-871">Otherwise, `M` is a non-virtual function member, and the function member to invoke is `M` itself.</span></span>
   * <span data-ttu-id="70e32-872">A implementação de membro da função determinada na etapa anterior é invocada.</span><span class="sxs-lookup"><span data-stu-id="70e32-872">The function member implementation determined in the step above is invoked.</span></span> <span data-ttu-id="70e32-873">O objeto referenciado por `E` torna-se o objeto referenciado por `this`.</span><span class="sxs-lookup"><span data-stu-id="70e32-873">The object referenced by `E` becomes the object referenced by `this`.</span></span>

#### <a name="invocations-on-boxed-instances"></a><span data-ttu-id="70e32-874">Invocações de instâncias do box</span><span class="sxs-lookup"><span data-stu-id="70e32-874">Invocations on boxed instances</span></span>

<span data-ttu-id="70e32-875">Um membro da função implementada em uma *value_type* pode ser invocado por meio de uma instância demarcada desse *value_type* nas seguintes situações:</span><span class="sxs-lookup"><span data-stu-id="70e32-875">A function member implemented in a *value_type* can be invoked through a boxed instance of that *value_type* in the following situations:</span></span>

*  <span data-ttu-id="70e32-876">Quando o membro da função é um `override` de um método herdado do tipo `object` e é invocado por meio de uma expressão de instância do tipo `object`.</span><span class="sxs-lookup"><span data-stu-id="70e32-876">When the function member is an `override` of a method inherited from type `object` and is invoked through an instance expression of type `object`.</span></span>
*  <span data-ttu-id="70e32-877">Quando o membro da função é uma implementação de um membro da função de interface e é invocado por meio de uma expressão de instância de um *interface_type*.</span><span class="sxs-lookup"><span data-stu-id="70e32-877">When the function member is an implementation of an interface function member and is invoked through an instance expression of an *interface_type*.</span></span>
*  <span data-ttu-id="70e32-878">Quando o membro da função é invocado por meio de um delegado.</span><span class="sxs-lookup"><span data-stu-id="70e32-878">When the function member is invoked through a delegate.</span></span>

<span data-ttu-id="70e32-879">Nessas situações, a instância do box é considerada como tendo uma variável do *value_type*, e essa variável se torna a variável referenciada por `this` dentro a invocação de membro da função.</span><span class="sxs-lookup"><span data-stu-id="70e32-879">In these situations, the boxed instance is considered to contain a variable of the *value_type*, and this variable becomes the variable referenced by `this` within the function member invocation.</span></span> <span data-ttu-id="70e32-880">Em particular, isso significa que, quando um membro da função é invocado em uma instância do box, é possível que o membro da função modificar o valor contido na instância do box.</span><span class="sxs-lookup"><span data-stu-id="70e32-880">In particular, this means that when a function member is invoked on a boxed instance, it is possible for the function member to modify the value contained in the boxed instance.</span></span>

## <a name="primary-expressions"></a><span data-ttu-id="70e32-881">Expressões primárias</span><span class="sxs-lookup"><span data-stu-id="70e32-881">Primary expressions</span></span>

<span data-ttu-id="70e32-882">Expressões primárias incluem as formas mais simples de expressões.</span><span class="sxs-lookup"><span data-stu-id="70e32-882">Primary expressions include the simplest forms of expressions.</span></span>

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

<span data-ttu-id="70e32-883">Expressões primárias são divididas entre *array_creation_expression*s e *primary_no_array_creation_expression*s.</span><span class="sxs-lookup"><span data-stu-id="70e32-883">Primary expressions are divided between *array_creation_expression*s and *primary_no_array_creation_expression*s.</span></span> <span data-ttu-id="70e32-884">Tratar a expressão de criação de matriz dessa forma, em vez de listá-la junto com as outras formas de expressão simples, permite que a gramática para não permitir código potencialmente confuso, como</span><span class="sxs-lookup"><span data-stu-id="70e32-884">Treating array-creation-expression in this way, rather than listing it along with the other simple expression forms, enables the grammar to disallow potentially confusing code such as</span></span>
```csharp
object o = new int[3][1];
```
<span data-ttu-id="70e32-885">Caso contrário, que poderiam ser interpretados como</span><span class="sxs-lookup"><span data-stu-id="70e32-885">which would otherwise be interpreted as</span></span>
```csharp
object o = (new int[3])[1];
```

### <a name="literals"></a><span data-ttu-id="70e32-886">Literais</span><span class="sxs-lookup"><span data-stu-id="70e32-886">Literals</span></span>

<span data-ttu-id="70e32-887">Um *primary_expression* que consiste em um *literal* ([literais](lexical-structure.md#literals)) é classificado como um valor.</span><span class="sxs-lookup"><span data-stu-id="70e32-887">A *primary_expression* that consists of a *literal* ([Literals](lexical-structure.md#literals)) is classified as a value.</span></span>


### <a name="interpolated-strings"></a><span data-ttu-id="70e32-888">Cadeias de caracteres interpoladas</span><span class="sxs-lookup"><span data-stu-id="70e32-888">Interpolated strings</span></span>

<span data-ttu-id="70e32-889">Uma *interpolated_string_expression* consiste em um `$` sinal seguido por uma regular ou textual cadeia de caracteres literal, no qual os buracos, delimitadas por `{` e `}`, inclua expressões e formatação especificações.</span><span class="sxs-lookup"><span data-stu-id="70e32-889">An *interpolated_string_expression* consists of a `$` sign followed by a regular or verbatim string literal, wherein holes, delimited by `{` and `}`, enclose expressions and formatting specifications.</span></span> <span data-ttu-id="70e32-890">Uma expressão de cadeia de caracteres interpolada é o resultado de uma *interpolated_string_literal* que foi dividido em tokens individuais, conforme descrito em [interpoladas literais de cadeia de caracteres](lexical-structure.md#interpolated-string-literals).</span><span class="sxs-lookup"><span data-stu-id="70e32-890">An interpolated string expression is the result of an *interpolated_string_literal* that has been broken up into individual tokens, as described in [Interpolated string literals](lexical-structure.md#interpolated-string-literals).</span></span>

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

<span data-ttu-id="70e32-891">O *constant_expression* uma interpolação deve ter uma conversão implícita em `int`.</span><span class="sxs-lookup"><span data-stu-id="70e32-891">The *constant_expression* in an interpolation must have an implicit conversion to `int`.</span></span>

<span data-ttu-id="70e32-892">Uma *interpolated_string_expression* é classificado como um valor.</span><span class="sxs-lookup"><span data-stu-id="70e32-892">An *interpolated_string_expression* is classified as a value.</span></span> <span data-ttu-id="70e32-893">Se ele imediatamente é convertido em `System.IFormattable` ou `System.FormattableString` com uma conversão implícita de cadeia de caracteres interpolada ([implícitas interpoladas conversões de cadeia de caracteres](conversions.md#implicit-interpolated-string-conversions)), a expressão de cadeia de caracteres interpolada tem esse tipo.</span><span class="sxs-lookup"><span data-stu-id="70e32-893">If it is immediately converted to `System.IFormattable` or `System.FormattableString` with an implicit interpolated string conversion ([Implicit interpolated string conversions](conversions.md#implicit-interpolated-string-conversions)), the interpolated string expression has that type.</span></span> <span data-ttu-id="70e32-894">Caso contrário, ele tem o tipo `string`.</span><span class="sxs-lookup"><span data-stu-id="70e32-894">Otherwise, it has the type `string`.</span></span>

<span data-ttu-id="70e32-895">Se for o tipo de uma cadeia de caracteres interpolada `System.IFormattable` ou `System.FormattableString`, o significado é uma chamada para `System.Runtime.CompilerServices.FormattableStringFactory.Create`.</span><span class="sxs-lookup"><span data-stu-id="70e32-895">If the type of an interpolated string is `System.IFormattable` or `System.FormattableString`, the meaning is a call to `System.Runtime.CompilerServices.FormattableStringFactory.Create`.</span></span> <span data-ttu-id="70e32-896">Se o tipo for `string`, o significado da expressão é uma chamada para `string.Format`.</span><span class="sxs-lookup"><span data-stu-id="70e32-896">If the type is `string`, the meaning of the expression is a call to `string.Format`.</span></span> <span data-ttu-id="70e32-897">Em ambos os casos, a lista de argumentos da chamada consiste em uma cadeia de caracteres de formato literal com espaços reservados para cada interpolação e um argumento para cada expressão correspondente para os espaços reservados.</span><span class="sxs-lookup"><span data-stu-id="70e32-897">In both cases, the argument list of the call consists of a format string literal with placeholders for each interpolation, and an argument for each expression corresponding to the place holders.</span></span>

<span data-ttu-id="70e32-898">A cadeia de caracteres de formato literal é construída da seguinte maneira, onde `N` é o número de interpolações em de *interpolated_string_expression*:</span><span class="sxs-lookup"><span data-stu-id="70e32-898">The format string literal is constructed as follows, where `N` is the number of interpolations in the *interpolated_string_expression*:</span></span>

*  <span data-ttu-id="70e32-899">Se um *interpolated_regular_string_whole* ou um *interpolated_verbatim_string_whole* segue o `$` entrar, em seguida, o literal de cadeia de caracteres de formato é esse token.</span><span class="sxs-lookup"><span data-stu-id="70e32-899">If an *interpolated_regular_string_whole* or an *interpolated_verbatim_string_whole* follows the `$` sign, then the format string literal is that token.</span></span>
*  <span data-ttu-id="70e32-900">Caso contrário, a cadeia de caracteres de formato literal consiste em:</span><span class="sxs-lookup"><span data-stu-id="70e32-900">Otherwise, the format string literal consists of:</span></span> 
   *  <span data-ttu-id="70e32-901">Primeiro o *interpolated_regular_string_start* ou *interpolated_verbatim_string_start*</span><span class="sxs-lookup"><span data-stu-id="70e32-901">First the *interpolated_regular_string_start* or *interpolated_verbatim_string_start*</span></span>
   *  <span data-ttu-id="70e32-902">Em seguida, para cada número `I` partir `0` para `N-1`:</span><span class="sxs-lookup"><span data-stu-id="70e32-902">Then for each number `I` from `0` to `N-1`:</span></span> 
      * <span data-ttu-id="70e32-903">A representação decimal de `I`</span><span class="sxs-lookup"><span data-stu-id="70e32-903">The decimal representation of `I`</span></span>
      * <span data-ttu-id="70e32-904">Então, se correspondente *interpolação* tem um *constant_expression*, um `,` (vírgula) seguida pela representação decimal do valor da *constant_expression*</span><span class="sxs-lookup"><span data-stu-id="70e32-904">Then, if the corresponding *interpolation* has a *constant_expression*, a `,` (comma) followed by the decimal representation of the value of the *constant_expression*</span></span>
      * <span data-ttu-id="70e32-905">Em seguida, a *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_mid* ou *interpolated_ verbatim_string_end* imediatamente após a interpolação correspondente.</span><span class="sxs-lookup"><span data-stu-id="70e32-905">Then the *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_mid* or *interpolated_verbatim_string_end* immediately following the corresponding interpolation.</span></span>

<span data-ttu-id="70e32-906">Os argumentos subsequentes são simplesmente o *expressões* da *interpolações* (se houver), na ordem.</span><span class="sxs-lookup"><span data-stu-id="70e32-906">The subsequent arguments are simply the *expressions* from the *interpolations* (if any), in order.</span></span>

<span data-ttu-id="70e32-907">TODO: exemplos.</span><span class="sxs-lookup"><span data-stu-id="70e32-907">TODO: examples.</span></span>


### <a name="simple-names"></a><span data-ttu-id="70e32-908">Nomes simples</span><span class="sxs-lookup"><span data-stu-id="70e32-908">Simple names</span></span>

<span data-ttu-id="70e32-909">Um *simple_name* consiste em um identificador, opcionalmente seguido por uma lista de argumentos de tipo:</span><span class="sxs-lookup"><span data-stu-id="70e32-909">A *simple_name* consists of an identifier, optionally followed by a type argument list:</span></span>

```antlr
simple_name
    : identifier type_argument_list?
    ;
```

<span data-ttu-id="70e32-910">Um *simple_name* é do formulário `I` ou do formulário `I<A1,...,Ak>`, onde `I` é um identificador único e `<A1,...,Ak>` é um recurso opcional *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="70e32-910">A *simple_name* is either of the form `I` or of the form `I<A1,...,Ak>`, where `I` is a single identifier and `<A1,...,Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="70e32-911">Quando nenhum *type_argument_list* é especificado, considere `K` seja zero.</span><span class="sxs-lookup"><span data-stu-id="70e32-911">When no *type_argument_list* is specified, consider `K` to be zero.</span></span> <span data-ttu-id="70e32-912">O *simple_name* é avaliado e classificados da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="70e32-912">The *simple_name* is evaluated and classified as follows:</span></span>

*  <span data-ttu-id="70e32-913">Se `K` é zero e o *simple_name* aparece dentro de uma *bloco* e, se a *bloco*do (ou um delimitador *bloco*do) local espaço de declaração de variável ([declarações](basic-concepts.md#declarations)) contém uma variável local, parâmetro ou constante com nome `I`, em seguida, o *simple_name* refere-se para a variável local, parâmetro ou constante e é classificado como uma variável ou um valor.</span><span class="sxs-lookup"><span data-stu-id="70e32-913">If `K` is zero and the *simple_name* appears within a *block* and if the *block*'s (or an enclosing *block*'s) local variable declaration space ([Declarations](basic-concepts.md#declarations)) contains a local variable, parameter or constant with name `I`, then the *simple_name* refers to that local variable, parameter or constant and is classified as a variable or value.</span></span>
*  <span data-ttu-id="70e32-914">Se `K` é zero e o *simple_name* é exibida dentro do corpo de uma declaração de método genérico e se essa declaração inclui um parâmetro de tipo com nome `I`, em seguida, a *simple_name*se refere a esse parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="70e32-914">If `K` is zero and the *simple_name* appears within the body of a generic method declaration and if that declaration includes a type parameter with name `I`, then the *simple_name* refers to that type parameter.</span></span>
*  <span data-ttu-id="70e32-915">Caso contrário, para cada tipo de instância `T` ([o tipo de instância](classes.md#the-instance-type)), começando com o tipo de instância de declaração de tipo de delimitador imediatamente e continuando com o tipo de instância de cada classe delimitadora ou struct declaração (se houver):</span><span class="sxs-lookup"><span data-stu-id="70e32-915">Otherwise, for each instance type `T` ([The instance type](classes.md#the-instance-type)), starting with the instance type of the immediately enclosing type declaration and continuing with the instance type of each enclosing class or struct declaration (if any):</span></span>
   *  <span data-ttu-id="70e32-916">Se `K` é zero e a declaração de `T` inclui um parâmetro de tipo com nome `I`, em seguida, o *simple_name* refere-se a esse parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="70e32-916">If `K` is zero and the declaration of `T` includes a type parameter with name `I`, then the *simple_name* refers to that type parameter.</span></span>
   *  <span data-ttu-id="70e32-917">Caso contrário, se uma pesquisa de membro ([pesquisa de membro](expressions.md#member-lookup)) de `I` na `T` com `K` argumentos de tipo produz uma correspondência:</span><span class="sxs-lookup"><span data-stu-id="70e32-917">Otherwise, if a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `T` with `K` type arguments produces a match:</span></span>
      * <span data-ttu-id="70e32-918">Se `T` é o tipo de instância do tipo de classe ou struct imediatamente delimitador e a pesquisa identifica um ou mais métodos, o resultado é um grupo de método com uma expressão de instância associada do `this`.</span><span class="sxs-lookup"><span data-stu-id="70e32-918">If `T` is the instance type of the immediately enclosing class or struct type and the lookup identifies one or more methods, the result is a method group with an associated instance expression of `this`.</span></span> <span data-ttu-id="70e32-919">Se uma lista de argumentos de tipo foi especificada, ele será usado na chamada de um método genérico ([invocações de método](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="70e32-919">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
      * <span data-ttu-id="70e32-920">Caso contrário, se `T` é o tipo de instância do tipo imediatamente delimitador de classe ou struct, se a pesquisa identifica um membro de instância, e se a referência ocorre dentro do corpo de um construtor de instância, um método de instância ou um acessador de instância, o resultado é o mesmo que um acesso de membro ([acesso de membro](expressions.md#member-access)) do formulário `this.I`.</span><span class="sxs-lookup"><span data-stu-id="70e32-920">Otherwise, if `T` is the instance type of the immediately enclosing class or struct type, if the lookup identifies an instance member, and if the reference occurs within the body of an instance constructor, an instance method, or an instance accessor, the result is the same as a member access ([Member access](expressions.md#member-access)) of the form `this.I`.</span></span> <span data-ttu-id="70e32-921">Isso só pode acontecer quando `K` é zero.</span><span class="sxs-lookup"><span data-stu-id="70e32-921">This can only happen when `K` is zero.</span></span>
      * <span data-ttu-id="70e32-922">Caso contrário, o resultado é o mesmo que um acesso de membro ([acesso de membro](expressions.md#member-access)) do formulário `T.I` ou `T.I<A1,...,Ak>`.</span><span class="sxs-lookup"><span data-stu-id="70e32-922">Otherwise, the result is the same as a member access ([Member access](expressions.md#member-access)) of the form `T.I` or `T.I<A1,...,Ak>`.</span></span> <span data-ttu-id="70e32-923">Nesse caso, ele é um erro de tempo de associação para o *simple_name* para se referir a um membro de instância.</span><span class="sxs-lookup"><span data-stu-id="70e32-923">In this case, it is a binding-time error for the *simple_name* to refer to an instance member.</span></span>

*  <span data-ttu-id="70e32-924">Caso contrário, para cada namespace `N`, começando com o namespace no qual o *simple_name* ocorre, continuando com cada delimitador namespace (se houver) e terminando com o namespace global, são as seguintes etapas avaliada até que uma entidade está localizada:</span><span class="sxs-lookup"><span data-stu-id="70e32-924">Otherwise, for each namespace `N`, starting with the namespace in which the *simple_name* occurs, continuing with each enclosing namespace (if any), and ending with the global namespace, the following steps are evaluated until an entity is located:</span></span>
   *  <span data-ttu-id="70e32-925">Se `K` é zero e `I` é o nome de um namespace em `N`, então:</span><span class="sxs-lookup"><span data-stu-id="70e32-925">If `K` is zero and `I` is the name of a namespace in `N`, then:</span></span>
      * <span data-ttu-id="70e32-926">Se o local onde o *simple_name* ocorre estiver envolvido por uma declaração de namespace `N` e a declaração de namespace contém uma *extern_alias_directive* ou  *using_alias_directive* que associa o nome `I` com um namespace ou tipo, em seguida, a *simple_name* é ambíguo e ocorrer um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-926">If the location where the *simple_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *simple_name* is ambiguous and a compile-time error occurs.</span></span>
      * <span data-ttu-id="70e32-927">Caso contrário, o *simple_name* refere-se ao namespace chamado `I` em `N`.</span><span class="sxs-lookup"><span data-stu-id="70e32-927">Otherwise, the *simple_name* refers to the namespace named `I` in `N`.</span></span>
   *  <span data-ttu-id="70e32-928">Caso contrário, se `N` contém um tipo acessível com o nome `I` e `K` , em seguida, os parâmetros de tipo:</span><span class="sxs-lookup"><span data-stu-id="70e32-928">Otherwise, if `N` contains an accessible type having name `I` and `K` type parameters, then:</span></span>
      * <span data-ttu-id="70e32-929">Se `K` é zero e o local onde o *simple_name* ocorre é incluído por uma declaração de namespace `N` e a declaração de namespace contém um *extern_alias_directive*ou *using_alias_directive* que associa o nome `I` com um namespace ou tipo, em seguida, a *simple_name* é ambíguo e ocorrer um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-929">If `K` is zero and the location where the *simple_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *simple_name* is ambiguous and a compile-time error occurs.</span></span>
      * <span data-ttu-id="70e32-930">Caso contrário, o *namespace_or_type_name* refere-se para o tipo construído com os argumentos de tipo em questão.</span><span class="sxs-lookup"><span data-stu-id="70e32-930">Otherwise, the *namespace_or_type_name* refers to the type constructed with the given type arguments.</span></span>
   *  <span data-ttu-id="70e32-931">Caso contrário, se o local em que o *simple_name* ocorre é incluído por uma declaração de namespace para `N`:</span><span class="sxs-lookup"><span data-stu-id="70e32-931">Otherwise, if the location where the *simple_name* occurs is enclosed by a namespace declaration for `N`:</span></span>
      * <span data-ttu-id="70e32-932">Se `K` é zero e a declaração de namespace contém uma *extern_alias_directive* ou *using_alias_directive* que associa o nome `I` com um namespace importado ou tipo, em seguida, a *simple_name* refere-se a esse namespace ou tipo.</span><span class="sxs-lookup"><span data-stu-id="70e32-932">If `K` is zero and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with an imported namespace or type, then the *simple_name* refers to that namespace or type.</span></span>
      * <span data-ttu-id="70e32-933">Caso contrário, se as declarações de namespaces e o tipo importado pela *using_namespace_directive*s e *using_static_directive*s da declaração do namespace conter exatamente um tipo acessível ou tendo o nome de membro estático sem extensão `I` e `K` parâmetros de tipo, o *simple_name* refere-se a esse tipo ou membro construído com os argumentos de tipo em questão.</span><span class="sxs-lookup"><span data-stu-id="70e32-933">Otherwise, if the namespaces and type declarations imported by the *using_namespace_directive*s and *using_static_directive*s of the namespace declaration contain exactly one accessible type or non-extension static member having name `I` and `K` type parameters, then the *simple_name* refers to that type or member constructed with the given type arguments.</span></span>
      * <span data-ttu-id="70e32-934">Caso contrário, se os tipos e namespaces importados pelo *using_namespace_directive*s da declaração do namespace contêm mais de um tipo acessível ou ter um nome de membro estático do método de extensão não `I` e `K` parâmetros de tipo, o *simple_name* é ambíguo e ocorre um erro.</span><span class="sxs-lookup"><span data-stu-id="70e32-934">Otherwise, if the namespaces and types imported by the *using_namespace_directive*s of the namespace declaration contain more than one accessible type or non-extension-method static member having name `I` and `K` type parameters, then the *simple_name* is ambiguous and an error occurs.</span></span>

   <span data-ttu-id="70e32-935">Observe que toda essa etapa é exatamente paralela para a etapa correspondente no processamento de uma *namespace_or_type_name* ([nomes de Namespace e tipo](basic-concepts.md#namespace-and-type-names)).</span><span class="sxs-lookup"><span data-stu-id="70e32-935">Note that this entire step is exactly parallel to the corresponding step in the processing of a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)).</span></span>

*  <span data-ttu-id="70e32-936">Caso contrário, o *simple_name* é indefinido e ocorrer um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-936">Otherwise, the *simple_name* is undefined and a compile-time error occurs.</span></span>


### <a name="parenthesized-expressions"></a><span data-ttu-id="70e32-937">Expressões entre parênteses</span><span class="sxs-lookup"><span data-stu-id="70e32-937">Parenthesized expressions</span></span>

<span data-ttu-id="70e32-938">Um *parenthesized_expression* consiste em um *expressão* entre parênteses.</span><span class="sxs-lookup"><span data-stu-id="70e32-938">A *parenthesized_expression* consists of an *expression* enclosed in parentheses.</span></span>

```antlr
parenthesized_expression
    : '(' expression ')'
    ;
```

<span data-ttu-id="70e32-939">Um *parenthesized_expression* é avaliada por avaliar o *expressão* dentro dos parênteses.</span><span class="sxs-lookup"><span data-stu-id="70e32-939">A *parenthesized_expression* is evaluated by evaluating the *expression* within the parentheses.</span></span> <span data-ttu-id="70e32-940">Se o *expressão* dentro dos parênteses denota um namespace ou tipo, ocorre um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-940">If the *expression* within the parentheses denotes a namespace or type, a compile-time error occurs.</span></span> <span data-ttu-id="70e32-941">Caso contrário, o resultado do *parenthesized_expression* é o resultado da avaliação de independente *expressão*.</span><span class="sxs-lookup"><span data-stu-id="70e32-941">Otherwise, the result of the *parenthesized_expression* is the result of the evaluation of the contained *expression*.</span></span>

### <a name="member-access"></a><span data-ttu-id="70e32-942">Acesso de membros</span><span class="sxs-lookup"><span data-stu-id="70e32-942">Member access</span></span>

<span data-ttu-id="70e32-943">Um *member_access* consiste em um *primary_expression*, um *predefined_type*, ou um *qualified_alias_member*, seguido por um"`.`"token, seguido por um *identificador*, opcionalmente seguido por um *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="70e32-943">A *member_access* consists of a *primary_expression*, a *predefined_type*, or a *qualified_alias_member*, followed by a "`.`" token, followed by an *identifier*, optionally followed by a *type_argument_list*.</span></span>

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

<span data-ttu-id="70e32-944">O *qualified_alias_member* produção é definida no [qualificadores alias de Namespace](namespaces.md#namespace-alias-qualifiers).</span><span class="sxs-lookup"><span data-stu-id="70e32-944">The *qualified_alias_member* production is defined in [Namespace alias qualifiers](namespaces.md#namespace-alias-qualifiers).</span></span>

<span data-ttu-id="70e32-945">Um *member_access* é do formulário `E.I` ou do formulário `E.I<A1, ..., Ak>`, onde `E` é uma expressão primária `I` é um identificador único e `<A1, ..., Ak>` é um recurso opcional  *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="70e32-945">A *member_access* is either of the form `E.I` or of the form `E.I<A1, ..., Ak>`, where `E` is a primary-expression, `I` is a single identifier and `<A1, ..., Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="70e32-946">Quando nenhum *type_argument_list* é especificado, considere `K` seja zero.</span><span class="sxs-lookup"><span data-stu-id="70e32-946">When no *type_argument_list* is specified, consider `K` to be zero.</span></span>

<span data-ttu-id="70e32-947">Um *member_access* com um *primary_expression* do tipo `dynamic` associado dinamicamente ([vinculação dinâmica](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="70e32-947">A *member_access* with a *primary_expression* of type `dynamic` is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="70e32-948">Nesse caso, o compilador classifica o acesso de membro como um acesso de propriedade do tipo `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="70e32-948">In this case the compiler classifies the member access as a property access of type `dynamic`.</span></span> <span data-ttu-id="70e32-949">As regras abaixo para determinar o significado do *member_access* , em seguida, são aplicadas em tempo de execução, usando o tipo de tempo de execução em vez do tipo de tempo de compilação da *primary_expression*.</span><span class="sxs-lookup"><span data-stu-id="70e32-949">The rules below to determine the meaning of the *member_access* are then applied at run-time, using the run-time type instead of the compile-time type of the *primary_expression*.</span></span> <span data-ttu-id="70e32-950">Se essa classificação de tempo de execução leva a um grupo de métodos, o acesso de membro deve ser o *primary_expression* de uma *invocation_expression*.</span><span class="sxs-lookup"><span data-stu-id="70e32-950">If this run-time classification leads to a method group, then the member access must be the *primary_expression* of an *invocation_expression*.</span></span>

<span data-ttu-id="70e32-951">O *member_access* é avaliado e classificados da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="70e32-951">The *member_access* is evaluated and classified as follows:</span></span>

*  <span data-ttu-id="70e32-952">Se `K` é zero e `E` é um namespace e `E` contém um namespace aninhado com nome `I`, o resultado será desse namespace.</span><span class="sxs-lookup"><span data-stu-id="70e32-952">If `K` is zero and `E` is a namespace and `E` contains a nested namespace with name `I`, then the result is that namespace.</span></span>
*  <span data-ttu-id="70e32-953">Caso contrário, se `E` é um namespace e `E` contém um tipo acessível com nome `I` e `K` parâmetros de tipo, em seguida, o resultado será aquele tipo construído com os argumentos de tipo em questão.</span><span class="sxs-lookup"><span data-stu-id="70e32-953">Otherwise, if `E` is a namespace and `E` contains an accessible type having name `I` and `K` type parameters, then the result is that type constructed with the given type arguments.</span></span>
*  <span data-ttu-id="70e32-954">Se `E` é um *predefined_type* ou uma *primary_expression* classificado como um tipo, se `E` não é um parâmetro de tipo e se uma pesquisa de membro ([depesquisademembro](expressions.md#member-lookup)) dos `I` na `E` com `K` parâmetros de tipo produz uma correspondência, em seguida, `E.I` é avaliado e classificados da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="70e32-954">If `E` is a *predefined_type* or a *primary_expression* classified as a type, if `E` is not a type parameter, and if a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `E` with `K` type parameters produces a match, then `E.I` is evaluated and classified as follows:</span></span>
   *  <span data-ttu-id="70e32-955">Se `I` identifica um tipo, em seguida, o resultado será aquele tipo construído com os argumentos de tipo em questão.</span><span class="sxs-lookup"><span data-stu-id="70e32-955">If `I` identifies a type, then the result is that type constructed with the given type arguments.</span></span>
   *  <span data-ttu-id="70e32-956">Se `I` identifica um ou mais métodos, o resultado será um grupo de métodos sem expressão de instância associada.</span><span class="sxs-lookup"><span data-stu-id="70e32-956">If `I` identifies one or more methods, then the result is a method group with no associated instance expression.</span></span> <span data-ttu-id="70e32-957">Se uma lista de argumentos de tipo foi especificada, ele será usado na chamada de um método genérico ([invocações de método](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="70e32-957">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
   *  <span data-ttu-id="70e32-958">Se `I` identifica um `static` propriedade e, em seguida, o resultado é um acesso de propriedade sem expressão de instância associada.</span><span class="sxs-lookup"><span data-stu-id="70e32-958">If `I` identifies a `static` property, then the result is a property access with no associated instance expression.</span></span>
   *  <span data-ttu-id="70e32-959">Se `I` identifica um `static` campo:</span><span class="sxs-lookup"><span data-stu-id="70e32-959">If `I` identifies a `static` field:</span></span>
      * <span data-ttu-id="70e32-960">Se o campo estiver `readonly` e a referência ocorre fora do construtor estático da classe ou struct em que o campo é declarado e, em seguida, o resultado é um valor, ou seja, o valor do campo estático `I` em `E`.</span><span class="sxs-lookup"><span data-stu-id="70e32-960">If the field is `readonly` and the reference occurs outside the static constructor of the class or struct in which the field is declared, then the result is a value, namely the value of the static field `I` in `E`.</span></span>
      * <span data-ttu-id="70e32-961">Caso contrário, o resultado é uma variável, ou seja, o campo estático `I` em `E`.</span><span class="sxs-lookup"><span data-stu-id="70e32-961">Otherwise, the result is a variable, namely the static field `I` in `E`.</span></span>
   *  <span data-ttu-id="70e32-962">Se `I` identifica um `static` eventos:</span><span class="sxs-lookup"><span data-stu-id="70e32-962">If `I` identifies a `static` event:</span></span>
      * <span data-ttu-id="70e32-963">Se a referência ocorre dentro da classe ou struct em que o evento é declarado e o evento foi declarado sem *event_accessor_declarations* ([eventos](classes.md#events)), em seguida, `E.I` é processado exatamente como se `I` foram um campo estático.</span><span class="sxs-lookup"><span data-stu-id="70e32-963">If the reference occurs within the class or struct in which the event is declared, and the event was declared without *event_accessor_declarations* ([Events](classes.md#events)), then `E.I` is processed exactly as if `I` were a static field.</span></span>
      * <span data-ttu-id="70e32-964">Caso contrário, o resultado é um acesso de evento sem expressão de instância associada.</span><span class="sxs-lookup"><span data-stu-id="70e32-964">Otherwise, the result is an event access with no associated instance expression.</span></span>
   *  <span data-ttu-id="70e32-965">Se `I` identifica uma constante, o resultado será um valor, ou seja, o valor dessa constante.</span><span class="sxs-lookup"><span data-stu-id="70e32-965">If `I` identifies a constant, then the result is a value, namely the value of that constant.</span></span>
    * <span data-ttu-id="70e32-966">Se `I` identifica um membro de enumeração, o resultado será um valor, ou seja, o valor desse membro de enumeração.</span><span class="sxs-lookup"><span data-stu-id="70e32-966">If `I` identifies an enumeration member, then the result is a value, namely the value of that enumeration member.</span></span>
    * <span data-ttu-id="70e32-967">Caso contrário, `E.I` é uma referência de membro inválido e ocorre um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-967">Otherwise, `E.I` is an invalid member reference, and a compile-time error occurs.</span></span>
*  <span data-ttu-id="70e32-968">Se `E` é um acesso de propriedade, acesso de indexador, variável ou valor, o tipo do qual é `T`e uma pesquisa de membro ([pesquisa de membro](expressions.md#member-lookup)) do `I` na `T` com `K` argumentos de tipo produz uma correspondência, em seguida, `E.I` é avaliado e classificados da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="70e32-968">If `E` is a property access, indexer access, variable, or value, the type of which is `T`, and a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `T` with `K` type arguments produces a match, then `E.I` is evaluated and classified as follows:</span></span>
   *  <span data-ttu-id="70e32-969">Primeiro, se `E` é uma propriedade ou acesso do indexador, então o valor da propriedade ou indexador acesso é obtido ([valores de expressões](expressions.md#values-of-expressions)) e `E` é reclassificado como um valor.</span><span class="sxs-lookup"><span data-stu-id="70e32-969">First, if `E` is a property or indexer access, then the value of the property or indexer access is obtained ([Values of expressions](expressions.md#values-of-expressions)) and `E` is reclassified as a value.</span></span>
   *  <span data-ttu-id="70e32-970">Se `I` identifica um ou mais métodos, o resultado será um grupo de métodos com uma expressão de instância associada do `E`.</span><span class="sxs-lookup"><span data-stu-id="70e32-970">If `I` identifies one or more methods, then the result is a method group with an associated instance expression of `E`.</span></span> <span data-ttu-id="70e32-971">Se uma lista de argumentos de tipo foi especificada, ele será usado na chamada de um método genérico ([invocações de método](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="70e32-971">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
   *  <span data-ttu-id="70e32-972">Se `I` identifica uma propriedade de instância</span><span class="sxs-lookup"><span data-stu-id="70e32-972">If `I` identifies an instance property,</span></span>
      * <span data-ttu-id="70e32-973">Se `E` está `this`, `I` identifica uma propriedade implementada automaticamente ([implementada automaticamente propriedades](classes.md#automatically-implemented-properties)) sem um setter e a referência ocorre dentro de um construtor de instância para um tipo de classe ou struct `T`, em seguida, o resultado é uma variável, ou seja, o campo de suporte oculto para a propriedade automática fornecida pelo `I` na instância do `T` fornecido pela `this`.</span><span class="sxs-lookup"><span data-stu-id="70e32-973">If `E` is `this`, `I` identifies an automatically implemented property ([Automatically implemented properties](classes.md#automatically-implemented-properties)) without a setter, and the reference occurs within an instance constructor for a class or struct type `T`, then the result is a variable, namely the hidden backing field for the auto-property given by `I` in the instance of `T` given by `this`.</span></span>
      * <span data-ttu-id="70e32-974">Caso contrário, o resultado é um acesso de propriedade com uma expressão de instância associada do `E`.</span><span class="sxs-lookup"><span data-stu-id="70e32-974">Otherwise, the result is a property access with an associated instance expression of `E`.</span></span>
   *  <span data-ttu-id="70e32-975">Se `T` é um *class_type* e `I` identifica um campo de instância disso *class_type*:</span><span class="sxs-lookup"><span data-stu-id="70e32-975">If `T` is a *class_type* and `I` identifies an instance field of that *class_type*:</span></span>
      * <span data-ttu-id="70e32-976">Se o valor de `E` está `null`, em seguida, um `System.NullReferenceException` é gerada.</span><span class="sxs-lookup"><span data-stu-id="70e32-976">If the value of `E` is `null`, then a `System.NullReferenceException` is thrown.</span></span>
      * <span data-ttu-id="70e32-977">Caso contrário, se o campo estiver `readonly` e a referência ocorre fora de um construtor de instância da classe na qual o campo é declarado e, em seguida, o resultado é um valor, ou seja, o valor do campo `I` no objeto referenciado pelo `E`.</span><span class="sxs-lookup"><span data-stu-id="70e32-977">Otherwise, if the field is `readonly` and the reference occurs outside an instance constructor of the class in which the field is declared, then the result is a value, namely the value of the field `I` in the object referenced by `E`.</span></span>
      * <span data-ttu-id="70e32-978">Caso contrário, o resultado é uma variável, ou seja, o campo `I` no objeto referenciado pelo `E`.</span><span class="sxs-lookup"><span data-stu-id="70e32-978">Otherwise, the result is a variable, namely the field `I` in the object referenced by `E`.</span></span>
   *  <span data-ttu-id="70e32-979">Se `T` é um *struct_type* e `I` identifica um campo de instância disso *struct_type*:</span><span class="sxs-lookup"><span data-stu-id="70e32-979">If `T` is a *struct_type* and `I` identifies an instance field of that *struct_type*:</span></span>
      * <span data-ttu-id="70e32-980">Se `E` é um valor, ou se o campo estiver `readonly` e a referência ocorre fora de um construtor de instância do struct em que o campo é declarado e, em seguida, o resultado é um valor, ou seja, o valor do campo `I` na instância do struct fornecida por `E`.</span><span class="sxs-lookup"><span data-stu-id="70e32-980">If `E` is a value, or if the field is `readonly` and the reference occurs outside an instance constructor of the struct in which the field is declared, then the result is a value, namely the value of the field `I` in the struct instance given by `E`.</span></span>
      * <span data-ttu-id="70e32-981">Caso contrário, o resultado é uma variável, ou seja, o campo `I` na instância do struct fornecida pelo `E`.</span><span class="sxs-lookup"><span data-stu-id="70e32-981">Otherwise, the result is a variable, namely the field `I` in the struct instance given by `E`.</span></span>
   *  <span data-ttu-id="70e32-982">Se `I` identifica um evento da instância:</span><span class="sxs-lookup"><span data-stu-id="70e32-982">If `I` identifies an instance event:</span></span>
      * <span data-ttu-id="70e32-983">Se a referência ocorre dentro da classe ou struct em que o evento é declarado e o evento foi declarado sem *event_accessor_declarations* ([eventos](classes.md#events)), e a referência não ocorre como o lado esquerdo de uma `+=` ou `-=` operador, em seguida, `E.I` é processado exatamente como se `I` foi um campo de instância.</span><span class="sxs-lookup"><span data-stu-id="70e32-983">If the reference occurs within the class or struct in which the event is declared, and the event was declared without *event_accessor_declarations* ([Events](classes.md#events)), and the reference does not occur as the left-hand side of a `+=` or `-=` operator, then `E.I` is processed exactly as if `I` was an instance field.</span></span>
      * <span data-ttu-id="70e32-984">Caso contrário, o resultado é um evento de acesso com uma expressão de instância associada do `E`.</span><span class="sxs-lookup"><span data-stu-id="70e32-984">Otherwise, the result is an event access with an associated instance expression of `E`.</span></span>
*  <span data-ttu-id="70e32-985">Caso contrário, é feita uma tentativa para processar `E.I` como uma invocação de método de extensão ([invocações de método de extensão](expressions.md#extension-method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="70e32-985">Otherwise, an attempt is made to process `E.I` as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="70e32-986">Se isso falhar, `E.I` é uma referência de membro inválido e ocorre um erro em tempo de vinculação.</span><span class="sxs-lookup"><span data-stu-id="70e32-986">If this fails, `E.I` is an invalid member reference, and a binding-time error occurs.</span></span>

#### <a name="identical-simple-names-and-type-names"></a><span data-ttu-id="70e32-987">Nomes idênticos de simples e nomes de tipo</span><span class="sxs-lookup"><span data-stu-id="70e32-987">Identical simple names and type names</span></span>

<span data-ttu-id="70e32-988">Em um acesso de membro do formulário `E.I`, se `E` é um identificador único e se o significado dos `E` como um *simple_name* ([nomes simples](expressions.md#simple-names)) é uma constante, campo, propriedade, variável local ou parâmetro com o mesmo tipo que o significado dos `E` como um *type_name* ([nomes de Namespace e tipo](basic-concepts.md#namespace-and-type-names)), em seguida, os dois significados possíveis de `E` são permitido.</span><span class="sxs-lookup"><span data-stu-id="70e32-988">In a member access of the form `E.I`, if `E` is a single identifier, and if the meaning of `E` as a *simple_name* ([Simple names](expressions.md#simple-names)) is a constant, field, property, local variable, or parameter with the same type as the meaning of `E` as a *type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)), then both possible meanings of `E` are permitted.</span></span> <span data-ttu-id="70e32-989">Os dois significados possíveis das `E.I` nunca são ambíguas, desde `I` necessariamente deve ser um membro do tipo `E` em ambos os casos.</span><span class="sxs-lookup"><span data-stu-id="70e32-989">The two possible meanings of `E.I` are never ambiguous, since `I` must necessarily be a member of the type `E` in both cases.</span></span> <span data-ttu-id="70e32-990">Em outras palavras, a regra simplesmente permite acesso a membros estáticos e tipos aninhados de `E` onde caso contrário, seria ter ocorrido um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-990">In other words, the rule simply permits access to the static members and nested types of `E` where a compile-time error would otherwise have occurred.</span></span> <span data-ttu-id="70e32-991">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="70e32-991">For example:</span></span>
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

#### <a name="grammar-ambiguities"></a><span data-ttu-id="70e32-992">Ambiguidades de gramática</span><span class="sxs-lookup"><span data-stu-id="70e32-992">Grammar ambiguities</span></span>

<span data-ttu-id="70e32-993">As produções para *simple_name* ([nomes simples](expressions.md#simple-names)) e *member_access* ([acesso de membro](expressions.md#member-access)) pode dar origem a ambiguidades no gramática de expressões.</span><span class="sxs-lookup"><span data-stu-id="70e32-993">The productions for *simple_name* ([Simple names](expressions.md#simple-names)) and *member_access* ([Member access](expressions.md#member-access)) can give rise to ambiguities in the grammar for expressions.</span></span> <span data-ttu-id="70e32-994">Por exemplo, a instrução:</span><span class="sxs-lookup"><span data-stu-id="70e32-994">For example, the statement:</span></span>
```
F(G<A,B>(7));
```
<span data-ttu-id="70e32-995">pode ser interpretado como uma chamada para `F` com dois argumentos, `G < A` e `B > (7)`.</span><span class="sxs-lookup"><span data-stu-id="70e32-995">could be interpreted as a call to `F` with two arguments, `G < A` and `B > (7)`.</span></span> <span data-ttu-id="70e32-996">Como alternativa, ele poderia ser interpretado como uma chamada para `F` com um argumento, que é uma chamada para um método genérico `G` com dois argumentos de tipo e um argumento regular.</span><span class="sxs-lookup"><span data-stu-id="70e32-996">Alternatively, it could be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span>

<span data-ttu-id="70e32-997">Se uma sequência de tokens pode ser analisada (no contexto) como um *simple_name* ([nomes simples](expressions.md#simple-names)), *member_access* ([acesso de membro](expressions.md#member-access)), ou *pointer_member_access* ([acesso de membro do ponteiro](unsafe-code.md#pointer-member-access)) terminando com um *type_argument_list* ([argumentos de tipo](types.md#type-arguments)), o imediatamente após o fechamento do token `>` token é examinado.</span><span class="sxs-lookup"><span data-stu-id="70e32-997">If a sequence of tokens can be parsed (in context) as a *simple_name* ([Simple names](expressions.md#simple-names)), *member_access* ([Member access](expressions.md#member-access)), or *pointer_member_access* ([Pointer member access](unsafe-code.md#pointer-member-access)) ending with a *type_argument_list* ([Type arguments](types.md#type-arguments)), the token immediately following the closing `>` token is examined.</span></span> <span data-ttu-id="70e32-998">Se ele for um dos</span><span class="sxs-lookup"><span data-stu-id="70e32-998">If it is one of</span></span>
```csharp
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```
<span data-ttu-id="70e32-999">em seguida, a *type_argument_list* é mantido como parte do *simple_name*, *member_access* ou *pointer_member_access* e qualquer a análise da sequência de tokens de outra possíveis será descartada.</span><span class="sxs-lookup"><span data-stu-id="70e32-999">then the *type_argument_list* is retained as part of the *simple_name*, *member_access* or *pointer_member_access* and any other possible parse of the sequence of tokens is discarded.</span></span> <span data-ttu-id="70e32-1000">Caso contrário, o *type_argument_list* não é considerado parte do *simple_name*, *member_access* ou *pointer_member_access*, mesmo se não houver nenhuma análise da sequência de tokens de outro possíveis.</span><span class="sxs-lookup"><span data-stu-id="70e32-1000">Otherwise, the *type_argument_list* is not considered to be part of the *simple_name*, *member_access* or *pointer_member_access*, even if there is no other possible parse of the sequence of tokens.</span></span> <span data-ttu-id="70e32-1001">Observe que essas regras não são aplicadas durante a análise de um *type_argument_list* em um *namespace_or_type_name* ([nomes de Namespace e tipo](basic-concepts.md#namespace-and-type-names)).</span><span class="sxs-lookup"><span data-stu-id="70e32-1001">Note that these rules are not applied when parsing a *type_argument_list* in a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)).</span></span> <span data-ttu-id="70e32-1002">A instrução</span><span class="sxs-lookup"><span data-stu-id="70e32-1002">The statement</span></span>
```csharp
F(G<A,B>(7));
```
<span data-ttu-id="70e32-1003">será, de acordo com a esta regra, interpretada como uma chamada para `F` com um argumento, que é uma chamada para um método genérico `G` com dois argumentos de tipo e um argumento regular.</span><span class="sxs-lookup"><span data-stu-id="70e32-1003">will, according to this rule, be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span> <span data-ttu-id="70e32-1004">As instruções</span><span class="sxs-lookup"><span data-stu-id="70e32-1004">The statements</span></span>
```csharp
F(G < A, B > 7);
F(G < A, B >> 7);
```
<span data-ttu-id="70e32-1005">cada uma ser interpretada como uma chamada para `F` com dois argumentos.</span><span class="sxs-lookup"><span data-stu-id="70e32-1005">will each be interpreted as a call to `F` with two arguments.</span></span> <span data-ttu-id="70e32-1006">A instrução</span><span class="sxs-lookup"><span data-stu-id="70e32-1006">The statement</span></span>
```csharp
x = F < A > +y;
```
<span data-ttu-id="70e32-1007">será interpretado como um operador menor que, maior que o operador e operador de adição unário, como se tivesse sido escrita a instrução `x = (F < A) > (+y)`, em vez de como uma *simple_name* com um *type_argument_list* seguido por um operador de adição binária.</span><span class="sxs-lookup"><span data-stu-id="70e32-1007">will be interpreted as a less than operator, greater than operator, and unary plus operator, as if the statement had been written `x = (F < A) > (+y)`, instead of as a *simple_name* with a *type_argument_list* followed by a binary plus operator.</span></span> <span data-ttu-id="70e32-1008">Na instrução</span><span class="sxs-lookup"><span data-stu-id="70e32-1008">In the statement</span></span>
```csharp
x = y is C<T> + z;
```
<span data-ttu-id="70e32-1009">os tokens `C<T>` são interpretados como uma *namespace_or_type_name* com um *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="70e32-1009">the tokens `C<T>` are interpreted as a *namespace_or_type_name* with a *type_argument_list*.</span></span>

### <a name="invocation-expressions"></a><span data-ttu-id="70e32-1010">Expressões de invocação</span><span class="sxs-lookup"><span data-stu-id="70e32-1010">Invocation expressions</span></span>

<span data-ttu-id="70e32-1011">Uma *invocation_expression* é usado para invocar um método.</span><span class="sxs-lookup"><span data-stu-id="70e32-1011">An *invocation_expression* is used to invoke a method.</span></span>

```antlr
invocation_expression
    : primary_expression '(' argument_list? ')'
    ;
```

<span data-ttu-id="70e32-1012">Uma *invocation_expression* associado dinamicamente ([vinculação dinâmica](expressions.md#dynamic-binding)) se tiver mantido pelo menos um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="70e32-1012">An *invocation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) if at least one of the following holds:</span></span>

* <span data-ttu-id="70e32-1013">O *primary_expression* tem o tipo de tempo de compilação `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1013">The *primary_expression* has compile-time type `dynamic`.</span></span>
* <span data-ttu-id="70e32-1014">Pelo menos um argumento de opcional *argument_list* tem o tipo de tempo de compilação `dynamic` e o *primary_expression* não tem um tipo de delegado.</span><span class="sxs-lookup"><span data-stu-id="70e32-1014">At least one argument of the optional *argument_list* has compile-time type `dynamic` and the *primary_expression* does not have a delegate type.</span></span>

<span data-ttu-id="70e32-1015">Nesse caso, o compilador classifica os *invocation_expression* como um valor do tipo `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1015">In this case the compiler classifies the *invocation_expression* as a value of type `dynamic`.</span></span> <span data-ttu-id="70e32-1016">As regras abaixo para determinar o significado do *invocation_expression* , em seguida, são aplicadas em tempo de execução, usando o tipo de tempo de execução em vez do tipo de tempo de compilação do *primary_expression* e argumentos que têm o tipo de tempo de compilação `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1016">The rules below to determine the meaning of the *invocation_expression* are then applied at run-time, using the run-time type instead of the compile-time type of those of the *primary_expression* and arguments which have the compile-time type `dynamic`.</span></span> <span data-ttu-id="70e32-1017">Se o *primary_expression* não tem o tipo de tempo de compilação `dynamic`, em seguida, a invocação do método passa por uma verificação de tempo de compilação limitada, conforme descrito em [verificação de resolução de sobrecarga de dinâmica de tempo de compilação ](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="70e32-1017">If the *primary_expression* does not have compile-time type `dynamic`, then the method invocation undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="70e32-1018">O *primary_expression* de uma *invocation_expression* deve ser um grupo de método ou um valor de um *delegate_type*.</span><span class="sxs-lookup"><span data-stu-id="70e32-1018">The *primary_expression* of an *invocation_expression* must be a method group or a value of a *delegate_type*.</span></span> <span data-ttu-id="70e32-1019">Se o *primary_expression* é um grupo de método, o *invocation_expression* é uma invocação de método ([invocações de método](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="70e32-1019">If the *primary_expression* is a method group, the *invocation_expression* is a method invocation ([Method invocations](expressions.md#method-invocations)).</span></span> <span data-ttu-id="70e32-1020">Se o *primary_expression* é um valor de uma *delegate_type*, o *invocation_expression* é uma invocação de delegado ([delegar invocações](expressions.md#delegate-invocations)).</span><span class="sxs-lookup"><span data-stu-id="70e32-1020">If the *primary_expression* is a value of a *delegate_type*, the *invocation_expression* is a delegate invocation ([Delegate invocations](expressions.md#delegate-invocations)).</span></span> <span data-ttu-id="70e32-1021">Se o *primary_expression* não é um grupo de métodos nem um valor de uma *delegate_type*, ocorrerá um erro em tempo de associação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1021">If the *primary_expression* is neither a method group nor a value of a *delegate_type*, a binding-time error occurs.</span></span>

<span data-ttu-id="70e32-1022">Opcional *argument_list* ([listas de argumentos](expressions.md#argument-lists)) fornece os valores ou referências de variáveis para os parâmetros do método.</span><span class="sxs-lookup"><span data-stu-id="70e32-1022">The optional *argument_list* ([Argument lists](expressions.md#argument-lists)) provides values or variable references for the parameters of the method.</span></span>

<span data-ttu-id="70e32-1023">O resultado da avaliação de uma *invocation_expression* são classificados da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="70e32-1023">The result of evaluating an *invocation_expression* is classified as follows:</span></span>

*  <span data-ttu-id="70e32-1024">Se o *invocation_expression* invoca um método ou um delegado que retorna `void`, o resultado é que nada.</span><span class="sxs-lookup"><span data-stu-id="70e32-1024">If the *invocation_expression* invokes a method or delegate that returns `void`, the result is nothing.</span></span> <span data-ttu-id="70e32-1025">Uma expressão que é classificada como nada é permitido apenas no contexto de um *statement_expression* ([instruções de expressão](statements.md#expression-statements)) ou como o corpo de um *lambda_expression*([Expressões de função anônima](expressions.md#anonymous-function-expressions)).</span><span class="sxs-lookup"><span data-stu-id="70e32-1025">An expression that is classified as nothing is permitted only in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)) or as the body of a *lambda_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)).</span></span> <span data-ttu-id="70e32-1026">Caso contrário, ocorrerá um erro em tempo de vinculação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1026">Otherwise a binding-time error occurs.</span></span>
*  <span data-ttu-id="70e32-1027">Caso contrário, o resultado é um valor do tipo retornado pelo método ou delegate.</span><span class="sxs-lookup"><span data-stu-id="70e32-1027">Otherwise, the result is a value of the type returned by the method or delegate.</span></span>

#### <a name="method-invocations"></a><span data-ttu-id="70e32-1028">Invocações de método</span><span class="sxs-lookup"><span data-stu-id="70e32-1028">Method invocations</span></span>

<span data-ttu-id="70e32-1029">Para uma invocação de método, o *primary_expression* da *invocation_expression* deve ser um grupo de método.</span><span class="sxs-lookup"><span data-stu-id="70e32-1029">For a method invocation, the *primary_expression* of the *invocation_expression* must be a method group.</span></span> <span data-ttu-id="70e32-1030">O grupo de método identifica um método para invocar ou o conjunto de métodos sobrecarregados para escolher um método específico para invocar.</span><span class="sxs-lookup"><span data-stu-id="70e32-1030">The method group identifies the one method to invoke or the set of overloaded methods from which to choose a specific method to invoke.</span></span> <span data-ttu-id="70e32-1031">No último caso, a determinação do método específico para invocar se baseia o contexto fornecido pelos tipos dos argumentos de *argument_list*.</span><span class="sxs-lookup"><span data-stu-id="70e32-1031">In the latter case, determination of the specific method to invoke is based on the context provided by the types of the arguments in the *argument_list*.</span></span>

<span data-ttu-id="70e32-1032">O processamento de tempo de associação de uma invocação de método do formulário `M(A)`, onde `M` é um grupo de métodos (incluindo, possivelmente, um *type_argument_list*), e `A` é um recurso opcional *argument_ lista*, consiste as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="70e32-1032">The binding-time processing of a method invocation of the form `M(A)`, where `M` is a method group (possibly including a *type_argument_list*), and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="70e32-1033">O conjunto de métodos de candidato para a invocação do método é construído.</span><span class="sxs-lookup"><span data-stu-id="70e32-1033">The set of candidate methods for the method invocation is constructed.</span></span> <span data-ttu-id="70e32-1034">Para cada método de `F` associada ao grupo de método `M`:</span><span class="sxs-lookup"><span data-stu-id="70e32-1034">For each method `F` associated with the method group `M`:</span></span>
   *  <span data-ttu-id="70e32-1035">Se `F` é não genérica, `F` é um candidato quando:</span><span class="sxs-lookup"><span data-stu-id="70e32-1035">If `F` is non-generic, `F` is a candidate when:</span></span>
      * <span data-ttu-id="70e32-1036">`M` não tem nenhuma lista de argumentos de tipo, e</span><span class="sxs-lookup"><span data-stu-id="70e32-1036">`M` has no type argument list, and</span></span>
      * <span data-ttu-id="70e32-1037">`F` é aplicável em relação às `A` ([membro da função aplicável](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="70e32-1037">`F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
   *  <span data-ttu-id="70e32-1038">Se `F` é genérico e `M` não tem nenhuma lista de argumentos de tipo `F` é um candidato quando:</span><span class="sxs-lookup"><span data-stu-id="70e32-1038">If `F` is generic and `M` has no type argument list, `F` is a candidate when:</span></span>
      * <span data-ttu-id="70e32-1039">Inferência de tipo ([inferência de tipo](expressions.md#type-inference)) for bem-sucedida, inferir uma lista de argumentos de tipo para a chamada, e</span><span class="sxs-lookup"><span data-stu-id="70e32-1039">Type inference ([Type inference](expressions.md#type-inference)) succeeds, inferring a list of type arguments for the call, and</span></span>
      * <span data-ttu-id="70e32-1040">Depois que os argumentos de tipo inferidos são substituídos por parâmetros de tipo correspondente do método, todos os tipos construídos na lista de parâmetros de F atender às suas restrições ([atendendo às restrições de](types.md#satisfying-constraints)) e a lista de parâmetro de `F` é aplicável em relação às `A` ([membro da função aplicável](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="70e32-1040">Once the inferred type arguments are substituted for the corresponding method type parameters, all constructed types in the parameter list of F satisfy their constraints ([Satisfying constraints](types.md#satisfying-constraints)), and the parameter list of `F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
   *  <span data-ttu-id="70e32-1041">Se `F` é genérico e `M` inclui uma lista de argumentos de tipo `F` é um candidato quando:</span><span class="sxs-lookup"><span data-stu-id="70e32-1041">If `F` is generic and `M` includes a type argument list, `F` is a candidate when:</span></span>
      * <span data-ttu-id="70e32-1042">`F` tem o mesmo número de parâmetros de tipo de método como foram fornecidos na lista de argumentos de tipo, e</span><span class="sxs-lookup"><span data-stu-id="70e32-1042">`F` has the same number of method type parameters as were supplied in the type argument list, and</span></span>
      * <span data-ttu-id="70e32-1043">Depois que os argumentos de tipo são substituídos por parâmetros de tipo correspondente do método, todos os tipos construídos na lista de parâmetros de F atender às suas restrições ([atendendo às restrições](types.md#satisfying-constraints)) e a lista de parâmetros de `F` é aplicável em relação às `A` ([membro da função aplicável](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="70e32-1043">Once the type arguments are substituted for the corresponding method type parameters, all constructed types in the parameter list of F satisfy their constraints ([Satisfying constraints](types.md#satisfying-constraints)), and the parameter list of `F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
*  <span data-ttu-id="70e32-1044">O conjunto de métodos de candidato é reduzido para conter apenas métodos de tipos mais derivados: para cada método de `C.F` no conjunto, onde `C` é o tipo no qual o método `F` for declarado, todos os métodos declarados em um tipo base do `C`são removidos do conjunto.</span><span class="sxs-lookup"><span data-stu-id="70e32-1044">The set of candidate methods is reduced to contain only methods from the most derived types: For each method `C.F` in the set, where `C` is the type in which the method `F` is declared, all methods declared in a base type of `C` are removed from the set.</span></span> <span data-ttu-id="70e32-1045">Além disso, se `C` é um tipo de classe diferente de `object`, todos os métodos declarados em um tipo de interface são removidos do conjunto.</span><span class="sxs-lookup"><span data-stu-id="70e32-1045">Furthermore, if `C` is a class type other than `object`, all methods declared in an interface type are removed from the set.</span></span> <span data-ttu-id="70e32-1046">(Essa última regra só tem efeito quando o grupo de método foi o resultado de uma pesquisa de membro em um parâmetro de tipo que tem uma classe base efetivada que não seja o objeto e uma interface de efetivada vazio definido.)</span><span class="sxs-lookup"><span data-stu-id="70e32-1046">(This latter rule only has affect when the method group was the result of a member lookup on a type parameter having an effective base class other than object and a non-empty effective interface set.)</span></span>
*  <span data-ttu-id="70e32-1047">Se o conjunto de métodos de candidato resultante está vazio, processamento adicional ao longo das etapas a seguir são abandonados e, em vez disso, é feita uma tentativa para processar a invocação de uma invocação de método de extensão ([invocações de método de extensão](expressions.md#extension-method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="70e32-1047">If the resulting set of candidate methods is empty, then further processing along the following steps are abandoned, and instead an attempt is made to process the invocation as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="70e32-1048">Se isso falhar, então nenhum método aplicável existe e ocorre um erro em tempo de vinculação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1048">If this fails, then no applicable methods exist, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="70e32-1049">O melhor método do conjunto de métodos de candidato é identificado usando as regras de resolução de sobrecarga de [resolução de sobrecarga](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="70e32-1049">The best method of the set of candidate methods is identified using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="70e32-1050">Se um único método melhor não pode ser identificado, a invocação do método é ambígua e ocorre um erro em tempo de vinculação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1050">If a single best method cannot be identified, the method invocation is ambiguous, and a binding-time error occurs.</span></span> <span data-ttu-id="70e32-1051">Ao executar a resolução de sobrecarga, os parâmetros de um método genérico são considerados depois de substituir os argumentos de tipo (fornecida ou inferido) para os parâmetros de tipo correspondente do método.</span><span class="sxs-lookup"><span data-stu-id="70e32-1051">When performing overload resolution, the parameters of a generic method are considered after substituting the type arguments (supplied or inferred) for the corresponding method type parameters.</span></span>
*  <span data-ttu-id="70e32-1052">Validação final do método melhor escolhido é executada:</span><span class="sxs-lookup"><span data-stu-id="70e32-1052">Final validation of the chosen best method is performed:</span></span>
   * <span data-ttu-id="70e32-1053">O método é validado no contexto do grupo de método: se o melhor método é um método estático, o grupo de método deve ter resultado de uma *simple_name* ou um *member_access* por meio de um tipo.</span><span class="sxs-lookup"><span data-stu-id="70e32-1053">The method is validated in the context of the method group: If the best method is a static method, the method group must have resulted from a *simple_name* or a *member_access* through a type.</span></span> <span data-ttu-id="70e32-1054">Se o melhor método é um método de instância, o grupo de método deve ter resultado de uma *simple_name*, um *member_access* por meio de uma variável ou um valor, ou uma *base_access*.</span><span class="sxs-lookup"><span data-stu-id="70e32-1054">If the best method is an instance method, the method group must have resulted from a *simple_name*, a *member_access* through a variable or value, or a *base_access*.</span></span> <span data-ttu-id="70e32-1055">Se nenhum desses requisitos for true, ocorrerá um erro de tempo de associação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1055">If neither of these requirements is true, a binding-time error occurs.</span></span>
   * <span data-ttu-id="70e32-1056">Se o melhor método é um método genérico, os argumentos de tipo (fornecida ou inferido) são verificados em relação às restrições ([atendendo às restrições de](types.md#satisfying-constraints)) declarado no método genérico.</span><span class="sxs-lookup"><span data-stu-id="70e32-1056">If the best method is a generic method, the type arguments (supplied or inferred) are checked against the constraints ([Satisfying constraints](types.md#satisfying-constraints)) declared on the generic method.</span></span> <span data-ttu-id="70e32-1057">Se qualquer argumento de tipo não satisfaz a ões correspondente no parâmetro de tipo, ocorrerá um erro de tempo de associação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1057">If any type argument does not satisfy the corresponding constraint(s) on the type parameter, a binding-time error occurs.</span></span>

<span data-ttu-id="70e32-1058">Depois que um método foi selecionado e validado no tempo de associação, as etapas acima, a invocação de tempo de execução real é processada de acordo com as regras da invocação de membro de função descrito na [verificação de resolução de sobrecarga de dinâmica de tempo de compilação ](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="70e32-1058">Once a method has been selected and validated at binding-time by the above steps, the actual run-time invocation is processed according to the rules of function member invocation described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="70e32-1059">O efeito intuitivo das regras de resolução descrito acima é da seguinte maneira: para localizar o método específico, invocado por uma invocação de método, comece com o tipo indicado pela invocação do método e continuar a cadeia de herança até que pelo menos um aplicável, declaração de método acessíveis, a substituição não for encontrada.</span><span class="sxs-lookup"><span data-stu-id="70e32-1059">The intuitive effect of the resolution rules described above is as follows: To locate the particular method invoked by a method invocation, start with the type indicated by the method invocation and proceed up the inheritance chain until at least one applicable, accessible, non-override method declaration is found.</span></span> <span data-ttu-id="70e32-1060">Em seguida, realizar a inferência de tipo e resolução no conjunto de métodos aplicáveis, acessíveis e não-override declaradas nesse tipo de sobrecarga e invocar o método selecionado, portanto.</span><span class="sxs-lookup"><span data-stu-id="70e32-1060">Then perform type inference and overload resolution on the set of applicable, accessible, non-override methods declared in that type and invoke the method thus selected.</span></span> <span data-ttu-id="70e32-1061">Se nenhum método foi encontrado, tente em vez disso processar a invocação de uma invocação de método de extensão.</span><span class="sxs-lookup"><span data-stu-id="70e32-1061">If no method was found, try instead to process the invocation as an extension method invocation.</span></span>

#### <a name="extension-method-invocations"></a><span data-ttu-id="70e32-1062">Invocações de método de extensão</span><span class="sxs-lookup"><span data-stu-id="70e32-1062">Extension method invocations</span></span>

<span data-ttu-id="70e32-1063">Em uma invocação de método ([invocações em instâncias demarcadas](expressions.md#invocations-on-boxed-instances)) de uma das formas</span><span class="sxs-lookup"><span data-stu-id="70e32-1063">In a method invocation ([Invocations on boxed instances](expressions.md#invocations-on-boxed-instances)) of one of the forms</span></span>
```csharp
expr . identifier ( )

expr . identifier ( args )

expr . identifier < typeargs > ( )

expr . identifier < typeargs > ( args )
```
<span data-ttu-id="70e32-1064">Se o processamento normal de invocação não encontrar nenhum método aplicável, é feita uma tentativa para processar a construção de uma invocação de método de extensão.</span><span class="sxs-lookup"><span data-stu-id="70e32-1064">if the normal processing of the invocation finds no applicable methods, an attempt is made to process the construct as an extension method invocation.</span></span> <span data-ttu-id="70e32-1065">Se *expr* ou qualquer um dos *args* tem o tipo de tempo de compilação `dynamic`, métodos de extensão não serão aplicada.</span><span class="sxs-lookup"><span data-stu-id="70e32-1065">If *expr* or any of the *args* has compile-time type `dynamic`, extension methods will not apply.</span></span>

<span data-ttu-id="70e32-1066">O objetivo é encontrar a melhor *type_name* `C`, de modo que a invocação de método estático correspondentes podem ocorrer:</span><span class="sxs-lookup"><span data-stu-id="70e32-1066">The objective is to find the best *type_name* `C`, so that the corresponding static method invocation can take place:</span></span>
```csharp
C . identifier ( expr )

C . identifier ( expr , args )

C . identifier < typeargs > ( expr )

C . identifier < typeargs > ( expr , args )
```

<span data-ttu-id="70e32-1067">Um método de extensão `Ci.Mj` está ***qualificados*** se:</span><span class="sxs-lookup"><span data-stu-id="70e32-1067">An extension method `Ci.Mj` is ***eligible*** if:</span></span>

*  <span data-ttu-id="70e32-1068">`Ci` é uma classe não genérica, não aninhadas</span><span class="sxs-lookup"><span data-stu-id="70e32-1068">`Ci` is a non-generic, non-nested class</span></span>
*  <span data-ttu-id="70e32-1069">O nome da `Mj` é *identificador*</span><span class="sxs-lookup"><span data-stu-id="70e32-1069">The name of `Mj` is *identifier*</span></span>
*  <span data-ttu-id="70e32-1070">`Mj` é aplicável quando aplicado aos argumentos como um método estático, como mostrado acima e acessível</span><span class="sxs-lookup"><span data-stu-id="70e32-1070">`Mj` is accessible and applicable when applied to the arguments as a static method as shown above</span></span>
*  <span data-ttu-id="70e32-1071">Existe uma identidade implícitas, referência ou conversão boxing a partir *expr* para o tipo do primeiro parâmetro de `Mj`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1071">An implicit identity, reference or boxing conversion exists from *expr* to the type of the first parameter of `Mj`.</span></span>

<span data-ttu-id="70e32-1072">A pesquisa de `C` procede da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="70e32-1072">The search for `C` proceeds as follows:</span></span>

*  <span data-ttu-id="70e32-1073">Começando com a mais próxima declaração namespace delimitador, continuando com cada declaração de namespace delimitador e terminando com a unidade de compilação que contém, tentativas sucessivas são feitas para localizar um conjunto de candidatos de métodos de extensão:</span><span class="sxs-lookup"><span data-stu-id="70e32-1073">Starting with the closest enclosing namespace declaration, continuing with each enclosing namespace declaration, and ending with the containing compilation unit, successive attempts are made to find a candidate set of extension methods:</span></span>
   * <span data-ttu-id="70e32-1074">Se a unidade de compilação ou de namespace determinada diretamente contém declarações de tipo não genérico `Ci` com métodos de extensão qualificados `Mj`, em seguida, o conjunto desses métodos de extensão é o conjunto de candidatos.</span><span class="sxs-lookup"><span data-stu-id="70e32-1074">If the given namespace or compilation unit directly contains non-generic type declarations `Ci` with eligible extension methods `Mj`, then the set of those extension methods is the candidate set.</span></span>
   * <span data-ttu-id="70e32-1075">Se tipos `Ci` importados pelo *using_static_declarations* e diretamente declarados em namespaces importados pelo *using_namespace_directive*s na unidade de especificada namespace ou de compilação diretamente contém métodos de extensão qualificados `Mj`, em seguida, o conjunto desses métodos de extensão é o conjunto de candidatos.</span><span class="sxs-lookup"><span data-stu-id="70e32-1075">If types `Ci` imported by *using_static_declarations* and directly declared in namespaces imported by *using_namespace_directive*s in the given namespace or compilation unit directly contain eligible extension methods `Mj`, then the set of those extension methods is the candidate set.</span></span>
*  <span data-ttu-id="70e32-1076">Se nenhum conjunto de candidato for encontrado em qualquer unidade de compilação ou de declaração de namespace delimitador, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1076">If no candidate set is found in any enclosing namespace declaration or compilation unit, a compile-time error occurs.</span></span>
*  <span data-ttu-id="70e32-1077">Caso contrário, a resolução de sobrecarga é aplicada ao candidato definido conforme descrito em ([resolução de sobrecarga](expressions.md#overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="70e32-1077">Otherwise, overload resolution is applied to the candidate set as described in ([Overload resolution](expressions.md#overload-resolution)).</span></span> <span data-ttu-id="70e32-1078">Se nenhum método único de melhor for encontrado, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1078">If no single best method is found, a compile-time error occurs.</span></span>
*  <span data-ttu-id="70e32-1079">`C` é o tipo no qual o melhor método é declarado como um método de extensão.</span><span class="sxs-lookup"><span data-stu-id="70e32-1079">`C` is the type within which the best method is declared as an extension method.</span></span>

<span data-ttu-id="70e32-1080">Usando o `C` como um destino, a chamada de método é processada como uma invocação de método estático ([verificação de resolução de sobrecarga de dinâmica de tempo de compilação](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="70e32-1080">Using `C` as a target, the method call is then processed as a static method invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span>

<span data-ttu-id="70e32-1081">As regras acima significam que os métodos de instância têm precedência sobre métodos de extensão, que os métodos de extensão disponíveis em declarações de namespace interno têm precedência sobre métodos de extensão disponíveis em declarações de namespace externo e essa extensão métodos declarados diretamente em um namespace têm precedência sobre métodos de extensão, importados para o mesmo namespace com o uso de uma diretiva de namespace.</span><span class="sxs-lookup"><span data-stu-id="70e32-1081">The preceding rules mean that instance methods take precedence over extension methods, that extension methods available in inner namespace declarations take precedence over extension methods available in outer namespace declarations, and that extension methods declared directly in a namespace take precedence over extension methods imported into that same namespace with a using namespace directive.</span></span> <span data-ttu-id="70e32-1082">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="70e32-1082">For example:</span></span>
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

<span data-ttu-id="70e32-1083">No exemplo, `B`do método tem precedência sobre o primeiro método de extensão, e `C`do método tem precedência sobre os dois métodos de extensão.</span><span class="sxs-lookup"><span data-stu-id="70e32-1083">In the example, `B`'s method takes precedence over the first extension method, and `C`'s method takes precedence over both extension methods.</span></span>

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

<span data-ttu-id="70e32-1084">A saída deste exemplo é:</span><span class="sxs-lookup"><span data-stu-id="70e32-1084">The output of this example is:</span></span>
```
E.F(1)
D.G(2)
C.H(3)
```
<span data-ttu-id="70e32-1085">`D.G` tem precedência sobre `C.G`, e `E.F` prevalece sobre ambos `D.F` e `C.F`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1085">`D.G` takes precedence over `C.G`, and `E.F` takes precedence over both `D.F` and `C.F`.</span></span>

#### <a name="delegate-invocations"></a><span data-ttu-id="70e32-1086">Invocações delegadas</span><span class="sxs-lookup"><span data-stu-id="70e32-1086">Delegate invocations</span></span>

<span data-ttu-id="70e32-1087">Para uma invocação de delegado, o *primary_expression* da *invocation_expression* deve ser um valor de um *delegate_type*.</span><span class="sxs-lookup"><span data-stu-id="70e32-1087">For a delegate invocation, the *primary_expression* of the *invocation_expression* must be a value of a *delegate_type*.</span></span> <span data-ttu-id="70e32-1088">Além disso, considerando o *delegate_type* ser um membro da função com a mesma lista de parâmetros como o *delegate_type*, o *delegate_type* deve ser aplicável ( [Membro da função aplicáveis](expressions.md#applicable-function-member)) com relação ao *argument_list* dos *invocation_expression*.</span><span class="sxs-lookup"><span data-stu-id="70e32-1088">Furthermore, considering the *delegate_type* to be a function member with the same parameter list as the *delegate_type*, the *delegate_type* must be applicable ([Applicable function member](expressions.md#applicable-function-member)) with respect to the *argument_list* of the *invocation_expression*.</span></span>

<span data-ttu-id="70e32-1089">O processamento de tempo de execução de uma invocação de delegado do formulário `D(A)`, onde `D` é um *primary_expression* de um *delegate_type* e `A` é um opcional*argument_list*, consiste as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="70e32-1089">The run-time processing of a delegate invocation of the form `D(A)`, where `D` is a *primary_expression* of a *delegate_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="70e32-1090">`D` é avaliado.</span><span class="sxs-lookup"><span data-stu-id="70e32-1090">`D` is evaluated.</span></span> <span data-ttu-id="70e32-1091">Se essa avaliação causa uma exceção, nenhuma outra etapa é executada.</span><span class="sxs-lookup"><span data-stu-id="70e32-1091">If this evaluation causes an exception, no further steps are executed.</span></span>
*  <span data-ttu-id="70e32-1092">O valor de `D` é verificado para ser válido.</span><span class="sxs-lookup"><span data-stu-id="70e32-1092">The value of `D` is checked to be valid.</span></span> <span data-ttu-id="70e32-1093">Se o valor de `D` está `null`, um `System.NullReferenceException` é lançada e nenhuma outra etapa é executada.</span><span class="sxs-lookup"><span data-stu-id="70e32-1093">If the value of `D` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="70e32-1094">Caso contrário, `D` é uma referência a uma instância de delegado.</span><span class="sxs-lookup"><span data-stu-id="70e32-1094">Otherwise, `D` is a reference to a delegate instance.</span></span> <span data-ttu-id="70e32-1095">Invocações de membro de função ([verificação de resolução de sobrecarga de dinâmica de tempo de compilação](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) são executadas em cada uma das entidades que pode ser chamadas na lista de invocação do delegado.</span><span class="sxs-lookup"><span data-stu-id="70e32-1095">Function member invocations ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) are performed on each of the callable entities in the invocation list of the delegate.</span></span> <span data-ttu-id="70e32-1096">Para entidades que pode ser chamadas consiste em uma instância e o método de instância, a instância para a invocação é a instância contida na entidade que pode ser chamada.</span><span class="sxs-lookup"><span data-stu-id="70e32-1096">For callable entities consisting of an instance and instance method, the instance for the invocation is the instance contained in the callable entity.</span></span>

### <a name="element-access"></a><span data-ttu-id="70e32-1097">Acesso de elemento</span><span class="sxs-lookup"><span data-stu-id="70e32-1097">Element access</span></span>

<span data-ttu-id="70e32-1098">Uma *element_access* consiste em um *primary_no_array_creation_expression*, seguido por um "`[`" token, seguido por um *argument_list*, seguido por um " `]`"token.</span><span class="sxs-lookup"><span data-stu-id="70e32-1098">An *element_access* consists of a *primary_no_array_creation_expression*, followed by a "`[`" token, followed by an *argument_list*, followed by a "`]`" token.</span></span> <span data-ttu-id="70e32-1099">O *argument_list* consiste em um ou mais *argumento*s, separados por vírgulas.</span><span class="sxs-lookup"><span data-stu-id="70e32-1099">The *argument_list* consists of one or more *argument*s, separated by commas.</span></span>

```antlr
element_access
    : primary_no_array_creation_expression '[' expression_list ']'
    ;
```

<span data-ttu-id="70e32-1100">O *argument_list* de uma *element_access* não pode conter `ref` ou `out` argumentos.</span><span class="sxs-lookup"><span data-stu-id="70e32-1100">The *argument_list* of an *element_access* is not allowed to contain `ref` or `out` arguments.</span></span>

<span data-ttu-id="70e32-1101">Uma *element_access* associado dinamicamente ([vinculação dinâmica](expressions.md#dynamic-binding)) se tiver mantido pelo menos um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="70e32-1101">An *element_access* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) if at least one of the following holds:</span></span>

* <span data-ttu-id="70e32-1102">O *primary_no_array_creation_expression* tem o tipo de tempo de compilação `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1102">The *primary_no_array_creation_expression* has compile-time type `dynamic`.</span></span>
* <span data-ttu-id="70e32-1103">Pelo menos uma expressão do *argument_list* tem o tipo de tempo de compilação `dynamic` e o *primary_no_array_creation_expression* não tem um tipo de matriz.</span><span class="sxs-lookup"><span data-stu-id="70e32-1103">At least one expression of the *argument_list* has compile-time type `dynamic` and the *primary_no_array_creation_expression* does not have an array type.</span></span>

<span data-ttu-id="70e32-1104">Nesse caso, o compilador classifica os *element_access* como um valor do tipo `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1104">In this case the compiler classifies the *element_access* as a value of type `dynamic`.</span></span> <span data-ttu-id="70e32-1105">As regras abaixo para determinar o significado do *element_access* , em seguida, são aplicadas em tempo de execução, usando o tipo de tempo de execução em vez do tipo de tempo de compilação do *primary_no_array_creation_expression*e *argument_list* expressões que têm o tipo de tempo de compilação `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1105">The rules below to determine the meaning of the *element_access* are then applied at run-time, using the run-time type instead of the compile-time type of those of the *primary_no_array_creation_expression* and *argument_list* expressions which have the compile-time type `dynamic`.</span></span> <span data-ttu-id="70e32-1106">Se o *primary_no_array_creation_expression* não tem o tipo de tempo de compilação `dynamic`, em seguida, o acesso de elemento passa por uma verificação de tempo de compilação limitada, conforme descrito em [verificando dinâmico de tempo de compilação resolução de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="70e32-1106">If the *primary_no_array_creation_expression* does not have compile-time type `dynamic`, then the element access undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="70e32-1107">Se o *primary_no_array_creation_expression* de uma *element_access* é um valor de um *array_type*, o *element_access* é um acesso de matriz ([acesso de matriz](expressions.md#array-access)).</span><span class="sxs-lookup"><span data-stu-id="70e32-1107">If the *primary_no_array_creation_expression* of an *element_access* is a value of an *array_type*, the *element_access* is an array access ([Array access](expressions.md#array-access)).</span></span> <span data-ttu-id="70e32-1108">Caso contrário, o *primary_no_array_creation_expression* deve ser uma variável ou um valor de uma classe, struct ou tipo de interface que tem um ou mais membros de indexador, nesse caso, o *element_access* é um acesso do indexador ([acesso do indexador](expressions.md#indexer-access)).</span><span class="sxs-lookup"><span data-stu-id="70e32-1108">Otherwise, the *primary_no_array_creation_expression* must be a variable or value of a class, struct, or interface type that has one or more indexer members, in which case the *element_access* is an indexer access ([Indexer access](expressions.md#indexer-access)).</span></span>

#### <a name="array-access"></a><span data-ttu-id="70e32-1109">Acesso à matriz</span><span class="sxs-lookup"><span data-stu-id="70e32-1109">Array access</span></span>

<span data-ttu-id="70e32-1110">Para obter um acesso de matriz, o *primary_no_array_creation_expression* da *element_access* deve ser um valor de um *array_type*.</span><span class="sxs-lookup"><span data-stu-id="70e32-1110">For an array access, the *primary_no_array_creation_expression* of the *element_access* must be a value of an *array_type*.</span></span> <span data-ttu-id="70e32-1111">Além disso, o *argument_list* de uma matriz acesso não é permitido para conter argumentos nomeados. O número de expressões na *argument_list* deve ser o mesmo que a classificação do *array_type*, e cada expressão deve ser do tipo `int`, `uint`, `long`, `ulong`, ou deve ser implicitamente conversível para um ou mais desses tipos.</span><span class="sxs-lookup"><span data-stu-id="70e32-1111">Furthermore, the *argument_list* of an array access is not allowed to contain named arguments.The number of expressions in the *argument_list* must be the same as the rank of the *array_type*, and each expression must be of type `int`, `uint`, `long`, `ulong`, or must be implicitly convertible to one or more of these types.</span></span>

<span data-ttu-id="70e32-1112">O resultado da avaliação de um acesso à matriz é uma variável do tipo de elemento da matriz, ou seja, o elemento de matriz selecionado pelos valores das expressões na *argument_list*.</span><span class="sxs-lookup"><span data-stu-id="70e32-1112">The result of evaluating an array access is a variable of the element type of the array, namely the array element selected by the value(s) of the expression(s) in the *argument_list*.</span></span>

<span data-ttu-id="70e32-1113">O processamento de tempo de execução de um acesso de matriz no formato `P[A]`, onde `P` é um *primary_no_array_creation_expression* de um *array_type* e `A` é um *argument_list*, consiste as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="70e32-1113">The run-time processing of an array access of the form `P[A]`, where `P` is a *primary_no_array_creation_expression* of an *array_type* and `A` is an *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="70e32-1114">`P` é avaliado.</span><span class="sxs-lookup"><span data-stu-id="70e32-1114">`P` is evaluated.</span></span> <span data-ttu-id="70e32-1115">Se essa avaliação causa uma exceção, nenhuma outra etapa é executada.</span><span class="sxs-lookup"><span data-stu-id="70e32-1115">If this evaluation causes an exception, no further steps are executed.</span></span>
*  <span data-ttu-id="70e32-1116">As expressões de índice do *argument_list* são avaliados na ordem da esquerda para a direita.</span><span class="sxs-lookup"><span data-stu-id="70e32-1116">The index expressions of the *argument_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="70e32-1117">Após a avaliação de cada expressão de índice, uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)) é executada para um dos seguintes tipos: `int`, `uint`, `long`, `ulong`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1117">Following evaluation of each index expression, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to one of the following types is performed: `int`, `uint`, `long`, `ulong`.</span></span> <span data-ttu-id="70e32-1118">O primeiro tipo nessa lista para o qual existe uma conversão implícita é escolhido.</span><span class="sxs-lookup"><span data-stu-id="70e32-1118">The first type in this list for which an implicit conversion exists is chosen.</span></span> <span data-ttu-id="70e32-1119">Por exemplo, se a expressão de índice é do tipo `short` , em seguida, uma conversão implícita para `int` é executada, desde a conversões implícitas de `short` para `int` bidirecionalmente `short` para `long` são possíveis.</span><span class="sxs-lookup"><span data-stu-id="70e32-1119">For instance, if the index expression is of type `short` then an implicit conversion to `int` is performed, since implicit conversions from `short` to `int` and from `short` to `long` are possible.</span></span> <span data-ttu-id="70e32-1120">Se a avaliação de uma expressão de índice ou a conversão implícita subsequente faz com que uma exceção, nenhuma expressão de índice adicional é avaliado, e nenhuma outra etapas são executadas.</span><span class="sxs-lookup"><span data-stu-id="70e32-1120">If evaluation of an index expression or the subsequent implicit conversion causes an exception, then no further index expressions are evaluated and no further steps are executed.</span></span>
*  <span data-ttu-id="70e32-1121">O valor de `P` é verificado para ser válido.</span><span class="sxs-lookup"><span data-stu-id="70e32-1121">The value of `P` is checked to be valid.</span></span> <span data-ttu-id="70e32-1122">Se o valor de `P` está `null`, um `System.NullReferenceException` é lançada e nenhuma outra etapa é executada.</span><span class="sxs-lookup"><span data-stu-id="70e32-1122">If the value of `P` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="70e32-1123">O valor de cada expressão na *argument_list* é verificado em relação os limites reais de cada dimensão da instância de matriz referenciada por `P`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1123">The value of each expression in the *argument_list* is checked against the actual bounds of each dimension of the array instance referenced by `P`.</span></span> <span data-ttu-id="70e32-1124">Se um ou mais valores estão fora do intervalo, um `System.IndexOutOfRangeException` é lançada e nenhuma outra etapa é executada.</span><span class="sxs-lookup"><span data-stu-id="70e32-1124">If one or more values are out of range, a `System.IndexOutOfRangeException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="70e32-1125">O local do elemento da matriz fornecido pelas expressões de índice é computado, e esse local se tornará o resultado do acesso de matriz.</span><span class="sxs-lookup"><span data-stu-id="70e32-1125">The location of the array element given by the index expression(s) is computed, and this location becomes the result of the array access.</span></span>

#### <a name="indexer-access"></a><span data-ttu-id="70e32-1126">Acesso do indexador</span><span class="sxs-lookup"><span data-stu-id="70e32-1126">Indexer access</span></span>

<span data-ttu-id="70e32-1127">Para acessar um indexador, o *primary_no_array_creation_expression* da *element_access* deve ser uma variável ou um valor de uma classe, struct ou tipo de interface, e esse tipo deve implementar uma ou mais indexadores são aplicáveis em relação à *argument_list* da *element_access*.</span><span class="sxs-lookup"><span data-stu-id="70e32-1127">For an indexer access, the *primary_no_array_creation_expression* of the *element_access* must be a variable or value of a class, struct, or interface type, and this type must implement one or more indexers that are applicable with respect to the *argument_list* of the *element_access*.</span></span>

<span data-ttu-id="70e32-1128">O processamento de tempo de associação de um acesso do indexador do formulário `P[A]`, onde `P` é um *primary_no_array_creation_expression* de uma classe, struct ou tipo de interface `T`, e `A` é um *argument_list*, consiste as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="70e32-1128">The binding-time processing of an indexer access of the form `P[A]`, where `P` is a *primary_no_array_creation_expression* of a class, struct, or interface type `T`, and `A` is an *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="70e32-1129">O conjunto de indexadores fornecidos pelo `T` é construído.</span><span class="sxs-lookup"><span data-stu-id="70e32-1129">The set of indexers provided by `T` is constructed.</span></span> <span data-ttu-id="70e32-1130">O conjunto consiste em todos os indexadores declarados na `T` ou um tipo base `T` que não estão `override` declarações e pode ser acessada no contexto atual ([acesso de membro](basic-concepts.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="70e32-1130">The set consists of all indexers declared in `T` or a base type of `T` that are not `override` declarations and are accessible in the current context ([Member access](basic-concepts.md#member-access)).</span></span>
*  <span data-ttu-id="70e32-1131">O conjunto é reduzido para esses indexadores são aplicáveis e não ocultos por outros indexadores.</span><span class="sxs-lookup"><span data-stu-id="70e32-1131">The set is reduced to those indexers that are applicable and not hidden by other indexers.</span></span> <span data-ttu-id="70e32-1132">As seguintes regras são aplicadas a cada indexador `S.I` no conjunto, onde `S` é o tipo no qual o indexador `I` é declarado:</span><span class="sxs-lookup"><span data-stu-id="70e32-1132">The following rules are applied to each indexer `S.I` in the set, where `S` is the type in which the indexer `I` is declared:</span></span>
   * <span data-ttu-id="70e32-1133">Se `I` não é aplicável em relação às `A` ([membro da função aplicável](expressions.md#applicable-function-member)), em seguida, `I` é removido do conjunto.</span><span class="sxs-lookup"><span data-stu-id="70e32-1133">If `I` is not applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)), then `I` is removed from the set.</span></span>
   * <span data-ttu-id="70e32-1134">Se `I` é aplicável em relação às `A` ([membro da função aplicável](expressions.md#applicable-function-member)), em seguida, todos os indexadores declarados em um tipo base de `S` são removidas do conjunto de.</span><span class="sxs-lookup"><span data-stu-id="70e32-1134">If `I` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)), then all indexers declared in a base type of `S` are removed from the set.</span></span>
   * <span data-ttu-id="70e32-1135">Se `I` é aplicável em relação às `A` ([membro da função aplicável](expressions.md#applicable-function-member)) e `S` é um tipo de classe diferente de `object`, declarados em uma interface de todos os indexadores são removidos do conjunto.</span><span class="sxs-lookup"><span data-stu-id="70e32-1135">If `I` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)) and `S` is a class type other than `object`, all indexers declared in an interface are removed from the set.</span></span>
*  <span data-ttu-id="70e32-1136">Se o conjunto resultante de indexadores de candidato estiver vazio, em seguida, sem indexadores aplicáveis existirem e ocorre um erro em tempo de vinculação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1136">If the resulting set of candidate indexers is empty, then no applicable indexers exist, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="70e32-1137">O indexador melhor do conjunto de indexadores do candidato é identificado usando as regras de resolução de sobrecarga de [resolução de sobrecarga](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="70e32-1137">The best indexer of the set of candidate indexers is identified using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="70e32-1138">Se um único indexador melhor não pode ser identificado, o acesso do indexador é ambíguo e ocorre um erro em tempo de vinculação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1138">If a single best indexer cannot be identified, the indexer access is ambiguous, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="70e32-1139">As expressões de índice do *argument_list* são avaliados na ordem da esquerda para a direita.</span><span class="sxs-lookup"><span data-stu-id="70e32-1139">The index expressions of the *argument_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="70e32-1140">O resultado do processamento de acesso do indexador é uma expressão classificada como um acesso do indexador.</span><span class="sxs-lookup"><span data-stu-id="70e32-1140">The result of processing the indexer access is an expression classified as an indexer access.</span></span> <span data-ttu-id="70e32-1141">A expressão de acesso do indexador referencia o indexador determinado na etapa anterior e tem uma expressão de instância associada do `P` e uma lista de argumentos associados de `A`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1141">The indexer access expression references the indexer determined in the step above, and has an associated instance expression of `P` and an associated argument list of `A`.</span></span>

<span data-ttu-id="70e32-1142">Dependendo do contexto no qual ele é usado, um acesso de indexador faz com que a invocação de um a *acessador get* ou o *acessador set* do indexador.</span><span class="sxs-lookup"><span data-stu-id="70e32-1142">Depending on the context in which it is used, an indexer access causes invocation of either the *get accessor* or the *set accessor* of the indexer.</span></span> <span data-ttu-id="70e32-1143">Se o acesso do indexador é o destino de uma atribuição, o *acessador set* é chamado para atribuir um novo valor ([atribuição simples](expressions.md#simple-assignment)).</span><span class="sxs-lookup"><span data-stu-id="70e32-1143">If the indexer access is the target of an assignment, the *set accessor* is invoked to assign a new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="70e32-1144">Em todos os outros casos, o *acessador get* é chamado para obter o valor atual ([valores das expressões](expressions.md#values-of-expressions)).</span><span class="sxs-lookup"><span data-stu-id="70e32-1144">In all other cases, the *get accessor* is invoked to obtain the current value ([Values of expressions](expressions.md#values-of-expressions)).</span></span>

### <a name="this-access"></a><span data-ttu-id="70e32-1145">Esse acesso</span><span class="sxs-lookup"><span data-stu-id="70e32-1145">This access</span></span>

<span data-ttu-id="70e32-1146">Um *this_access* consiste em uma palavra reservada `this`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1146">A *this_access* consists of the reserved word `this`.</span></span>

```antlr
this_access
    : 'this'
    ;
```

<span data-ttu-id="70e32-1147">Um *this_access* é permitido somente na *bloco* de um construtor de instância, um método de instância ou um acessador de instância.</span><span class="sxs-lookup"><span data-stu-id="70e32-1147">A *this_access* is permitted only in the *block* of an instance constructor, an instance method, or an instance accessor.</span></span> <span data-ttu-id="70e32-1148">Ele tem um dos seguintes significados:</span><span class="sxs-lookup"><span data-stu-id="70e32-1148">It has one of the following meanings:</span></span>

*  <span data-ttu-id="70e32-1149">Quando `this` é usado em uma *primary_expression* dentro de um construtor de instância de uma classe, ele é classificado como um valor.</span><span class="sxs-lookup"><span data-stu-id="70e32-1149">When `this` is used in a *primary_expression* within an instance constructor of a class, it is classified as a value.</span></span> <span data-ttu-id="70e32-1150">O tipo do valor é o tipo de instância ([o tipo de instância](classes.md#the-instance-type)) da classe na qual ocorre o uso e o valor é uma referência ao objeto que está sendo construído.</span><span class="sxs-lookup"><span data-stu-id="70e32-1150">The type of the value is the instance type ([The instance type](classes.md#the-instance-type)) of the class within which the usage occurs, and the value is a reference to the object being constructed.</span></span>
*  <span data-ttu-id="70e32-1151">Quando `this` é usado em uma *primary_expression* dentro de um método de instância ou o acessador de instância de uma classe, ele é classificado como um valor.</span><span class="sxs-lookup"><span data-stu-id="70e32-1151">When `this` is used in a *primary_expression* within an instance method or instance accessor of a class, it is classified as a value.</span></span> <span data-ttu-id="70e32-1152">O tipo do valor é o tipo de instância ([o tipo de instância](classes.md#the-instance-type)) da classe na qual ocorre o uso e o valor é uma referência ao objeto para o qual o método ou o acessador foi invocado.</span><span class="sxs-lookup"><span data-stu-id="70e32-1152">The type of the value is the instance type ([The instance type](classes.md#the-instance-type)) of the class within which the usage occurs, and the value is a reference to the object for which the method or accessor was invoked.</span></span>
*  <span data-ttu-id="70e32-1153">Quando `this` é usado em uma *primary_expression* dentro de um construtor de instância de um struct, ele é classificado como uma variável.</span><span class="sxs-lookup"><span data-stu-id="70e32-1153">When `this` is used in a *primary_expression* within an instance constructor of a struct, it is classified as a variable.</span></span> <span data-ttu-id="70e32-1154">O tipo da variável é o tipo de instância ([o tipo de instância](classes.md#the-instance-type)) do struct no qual ocorre o uso e a variável representa a estrutura que está sendo construída.</span><span class="sxs-lookup"><span data-stu-id="70e32-1154">The type of the variable is the instance type ([The instance type](classes.md#the-instance-type)) of the struct within which the usage occurs, and the variable represents the struct being constructed.</span></span> <span data-ttu-id="70e32-1155">O `this` variável de um construtor de instância de um struct se comporta exatamente como um `out` parâmetro do tipo struct — em particular, isso significa que a variável deve ser definitivamente atribuída em cada caminho de execução da instância construtor.</span><span class="sxs-lookup"><span data-stu-id="70e32-1155">The `this` variable of an instance constructor of a struct behaves exactly the same as an `out` parameter of the struct type—in particular, this means that the variable must be definitely assigned in every execution path of the instance constructor.</span></span>
*  <span data-ttu-id="70e32-1156">Quando `this` é usado em uma *primary_expression* dentro de um método de instância ou o acessador de instância de um struct, ele é classificado como uma variável.</span><span class="sxs-lookup"><span data-stu-id="70e32-1156">When `this` is used in a *primary_expression* within an instance method or instance accessor of a struct, it is classified as a variable.</span></span> <span data-ttu-id="70e32-1157">O tipo da variável é o tipo de instância ([o tipo de instância](classes.md#the-instance-type)) do struct no qual ocorre o uso.</span><span class="sxs-lookup"><span data-stu-id="70e32-1157">The type of the variable is the instance type ([The instance type](classes.md#the-instance-type)) of the struct within which the usage occurs.</span></span>
   * <span data-ttu-id="70e32-1158">Se o método ou o acessador não é um iterador ([iteradores](classes.md#iterators)), o `this` variável representa a estrutura para o qual o acessador ou método foi chamado e se comporta exatamente como um `ref` parâmetro do tipo struct.</span><span class="sxs-lookup"><span data-stu-id="70e32-1158">If the method or accessor is not an iterator ([Iterators](classes.md#iterators)), the `this` variable represents the struct for which the method or accessor was invoked, and behaves exactly the same as a `ref` parameter of the struct type.</span></span>
   * <span data-ttu-id="70e32-1159">Se o método ou o acessador é um iterador, o `this` variável representa uma cópia da estrutura para o qual o acessador ou método foi chamado e se comporta exatamente como um parâmetro de valor do tipo struct.</span><span class="sxs-lookup"><span data-stu-id="70e32-1159">If the method or accessor is an iterator, the `this` variable represents a copy of the struct for which the method or accessor was invoked, and behaves exactly the same as a value parameter of the struct type.</span></span>

<span data-ttu-id="70e32-1160">Uso de `this` em um *primary_expression* em um contexto diferente daqueles listados acima é um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1160">Use of `this` in a *primary_expression* in a context other than the ones listed above is a compile-time error.</span></span> <span data-ttu-id="70e32-1161">Em particular, não é possível se referir `this` em um método estático, um acessador de propriedade estática, ou em um *variable_initializer* de uma declaração de campo.</span><span class="sxs-lookup"><span data-stu-id="70e32-1161">In particular, it is not possible to refer to `this` in a static method, a static property accessor, or in a *variable_initializer* of a field declaration.</span></span>

### <a name="base-access"></a><span data-ttu-id="70e32-1162">Acesso de base</span><span class="sxs-lookup"><span data-stu-id="70e32-1162">Base access</span></span>

<span data-ttu-id="70e32-1163">Um *base_access* consiste em uma palavra reservada `base` seguida por um "`.`" token e um identificador ou um *argument_list* entre colchetes:</span><span class="sxs-lookup"><span data-stu-id="70e32-1163">A *base_access* consists of the reserved word `base` followed by either a "`.`" token and an identifier or an *argument_list* enclosed in square brackets:</span></span>

```antlr
base_access
    : 'base' '.' identifier
    | 'base' '[' expression_list ']'
    ;
```

<span data-ttu-id="70e32-1164">Um *base_access* é usado para acessar membros de classe base que estão ocultos, da mesma forma nomeados membros na classe atual ou struct.</span><span class="sxs-lookup"><span data-stu-id="70e32-1164">A *base_access* is used to access base class members that are hidden by similarly named members in the current class or struct.</span></span> <span data-ttu-id="70e32-1165">Um *base_access* é permitido somente na *bloco* de um construtor de instância, um método de instância ou um acessador de instância.</span><span class="sxs-lookup"><span data-stu-id="70e32-1165">A *base_access* is permitted only in the *block* of an instance constructor, an instance method, or an instance accessor.</span></span> <span data-ttu-id="70e32-1166">Quando `base.I` ocorre em uma classe ou struct, `I` deve indicar um membro da classe base dessa classe ou struct.</span><span class="sxs-lookup"><span data-stu-id="70e32-1166">When `base.I` occurs in a class or struct, `I` must denote a member of the base class of that class or struct.</span></span> <span data-ttu-id="70e32-1167">Da mesma forma, quando `base[E]` ocorre em uma classe, um indexador aplicável deve existir na classe base.</span><span class="sxs-lookup"><span data-stu-id="70e32-1167">Likewise, when `base[E]` occurs in a class, an applicable indexer must exist in the base class.</span></span>

<span data-ttu-id="70e32-1168">Em tempo de vinculação *base_access* expressões do formulário `base.I` e `base[E]` são avaliadas exatamente como se eles fossem escritos `((B)this).I` e `((B)this)[E]`, onde `B` é a classe base da classe ou struct no qual ocorre a construção.</span><span class="sxs-lookup"><span data-stu-id="70e32-1168">At binding-time, *base_access* expressions of the form `base.I` and `base[E]` are evaluated exactly as if they were written `((B)this).I` and `((B)this)[E]`, where `B` is the base class of the class or struct in which the construct occurs.</span></span> <span data-ttu-id="70e32-1169">Portanto, `base.I` e `base[E]` correspondem aos `this.I` e `this[E]`, exceto `this` é visto como uma instância da classe base.</span><span class="sxs-lookup"><span data-stu-id="70e32-1169">Thus, `base.I` and `base[E]` correspond to `this.I` and `this[E]`, except `this` is viewed as an instance of the base class.</span></span>

<span data-ttu-id="70e32-1170">Quando um *base_access* faz referência a um membro de função virtual (um método, propriedade ou indexador), a determinação de qual função de membro a ser invocado em tempo de execução ([verificação de resolução de sobrecarga de dinâmica de tempo de compilação ](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) é alterado.</span><span class="sxs-lookup"><span data-stu-id="70e32-1170">When a *base_access* references a virtual function member (a method, property, or indexer), the determination of which function member to invoke at run-time ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is changed.</span></span> <span data-ttu-id="70e32-1171">O membro da função que é invocado é determinado pelo Localizando a implementação mais derivada ([métodos virtuais](classes.md#virtual-methods)) do membro da função em relação às `B` (em vez de em relação ao tipo de tempo de execução de `this`, como seria normal em um acesso de base não).</span><span class="sxs-lookup"><span data-stu-id="70e32-1171">The function member that is invoked is determined by finding the most derived implementation ([Virtual methods](classes.md#virtual-methods)) of the function member with respect to `B` (instead of with respect to the run-time type of `this`, as would be usual in a non-base access).</span></span> <span data-ttu-id="70e32-1172">Portanto, dentro de um `override` de um `virtual` membro da função, um *base_access* pode ser usado para chamar a implementação herdada do membro da função.</span><span class="sxs-lookup"><span data-stu-id="70e32-1172">Thus, within an `override` of a `virtual` function member, a *base_access* can be used to invoke the inherited implementation of the function member.</span></span> <span data-ttu-id="70e32-1173">Se o membro da função referenciado por uma *base_access* é abstrato, ocorrerá um erro em tempo de associação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1173">If the function member referenced by a *base_access* is abstract, a binding-time error occurs.</span></span>

### <a name="postfix-increment-and-decrement-operators"></a><span data-ttu-id="70e32-1174">Incremento de sufixo e operadores de decremento</span><span class="sxs-lookup"><span data-stu-id="70e32-1174">Postfix increment and decrement operators</span></span>

```antlr
post_increment_expression
    : primary_expression '++'
    ;

post_decrement_expression
    : primary_expression '--'
    ;
```

<span data-ttu-id="70e32-1175">O operando de um incremento ou decremento pós-fixada operação deve ser uma expressão classificada como uma variável, um acesso de propriedade ou um acesso do indexador.</span><span class="sxs-lookup"><span data-stu-id="70e32-1175">The operand of a postfix increment or decrement operation must be an expression classified as a variable, a property access, or an indexer access.</span></span> <span data-ttu-id="70e32-1176">O resultado da operação é um valor do mesmo tipo como o operando.</span><span class="sxs-lookup"><span data-stu-id="70e32-1176">The result of the operation is a value of the same type as the operand.</span></span>

<span data-ttu-id="70e32-1177">Se o *primary_expression* tem o tipo de tempo de compilação `dynamic` e em seguida, o operador está dinamicamente vinculado ([vinculação dinâmica](expressions.md#dynamic-binding)), o *post_increment_expression*ou *post_decrement_expression* tem o tipo de tempo de compilação `dynamic` e as seguintes regras são aplicadas em tempo de execução usando o tipo de tempo de execução do *primary_expression*.</span><span class="sxs-lookup"><span data-stu-id="70e32-1177">If the *primary_expression* has the compile-time type `dynamic` then the operator is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), the *post_increment_expression* or *post_decrement_expression* has the compile-time type `dynamic` and the following rules are applied at run-time using the run-time type of the *primary_expression*.</span></span>

<span data-ttu-id="70e32-1178">Se o operando de um sufixo incrementar ou operação de decremento é uma propriedade ou o acesso do indexador, propriedade ou indexador deve ter uma `get` e um `set` acessador.</span><span class="sxs-lookup"><span data-stu-id="70e32-1178">If the operand of a postfix increment or decrement operation is a property or indexer access, the property or indexer must have both a `get` and a `set` accessor.</span></span> <span data-ttu-id="70e32-1179">Se isso não for o caso, ocorre um erro em tempo de vinculação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1179">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="70e32-1180">Resolução de sobrecarga de operador unário ([resolução de sobrecarga de operador unário](expressions.md#unary-operator-overload-resolution)) é aplicado para selecionar uma implementação do operador específico.</span><span class="sxs-lookup"><span data-stu-id="70e32-1180">Unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="70e32-1181">Predefinidas `++` e `--` operadores existem para os seguintes tipos: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char` , `float`, `double`, `decimal`e qualquer tipo de enumeração.</span><span class="sxs-lookup"><span data-stu-id="70e32-1181">Predefined `++` and `--` operators exist for the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, and any enum type.</span></span> <span data-ttu-id="70e32-1182">Predefinido `++` operadores retornam o valor produzido pela adição de 1 para o operando e predefinido `--` operadores retornam o valor produzido pela subtração de 1 de operando.</span><span class="sxs-lookup"><span data-stu-id="70e32-1182">The predefined `++` operators return the value produced by adding 1 to the operand, and the predefined `--` operators return the value produced by subtracting 1 from the operand.</span></span> <span data-ttu-id="70e32-1183">Em um `checked` contexto, se o resultado dessa adição ou subtração está fora do intervalo do tipo de resultado e o tipo de resultado é um tipo integral ou um tipo de enumeração, um `System.OverflowException` é gerada.</span><span class="sxs-lookup"><span data-stu-id="70e32-1183">In a `checked` context, if the result of this addition or subtraction is outside the range of the result type and the result type is an integral type or enum type, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="70e32-1184">O processamento de tempo de execução de um incremento ou decremento a operação do formulário `x++` ou `x--` consiste as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="70e32-1184">The run-time processing of a postfix increment or decrement operation of the form `x++` or `x--` consists of the following steps:</span></span>

*   <span data-ttu-id="70e32-1185">Se `x` é classificado como uma variável:</span><span class="sxs-lookup"><span data-stu-id="70e32-1185">If `x` is classified as a variable:</span></span>
    * <span data-ttu-id="70e32-1186">`x` é avaliado para produzir a variável.</span><span class="sxs-lookup"><span data-stu-id="70e32-1186">`x` is evaluated to produce the variable.</span></span>
    * <span data-ttu-id="70e32-1187">O valor de `x` é salvo.</span><span class="sxs-lookup"><span data-stu-id="70e32-1187">The value of `x` is saved.</span></span>
    * <span data-ttu-id="70e32-1188">O operador selecionado é invocado com o valor salvo do `x` como seu argumento.</span><span class="sxs-lookup"><span data-stu-id="70e32-1188">The selected operator is invoked with the saved value of `x` as its argument.</span></span>
    * <span data-ttu-id="70e32-1189">O valor retornado pelo operador é armazenado no local fornecido pela avaliação de `x`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1189">The value returned by the operator is stored in the location given by the evaluation of `x`.</span></span>
    * <span data-ttu-id="70e32-1190">O valor salvo do `x` se tornará o resultado da operação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1190">The saved value of `x` becomes the result of the operation.</span></span>
*   <span data-ttu-id="70e32-1191">Se `x` é classificado como um acesso de propriedade ou indexador:</span><span class="sxs-lookup"><span data-stu-id="70e32-1191">If `x` is classified as a property or indexer access:</span></span>
    * <span data-ttu-id="70e32-1192">A expressão de instância (se `x` não é `static`) e a lista de argumentos (se `x` é um indexador de acesso) associadas com `x` são avaliados, e os resultados são usados em subsequente `get` e `set` invocações do acessador.</span><span class="sxs-lookup"><span data-stu-id="70e32-1192">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `get` and `set` accessor invocations.</span></span>
    * <span data-ttu-id="70e32-1193">O `get` acessador de `x` é invocado e o valor retornado é salvo.</span><span class="sxs-lookup"><span data-stu-id="70e32-1193">The `get` accessor of `x` is invoked and the returned value is saved.</span></span>
    * <span data-ttu-id="70e32-1194">O operador selecionado é invocado com o valor salvo do `x` como seu argumento.</span><span class="sxs-lookup"><span data-stu-id="70e32-1194">The selected operator is invoked with the saved value of `x` as its argument.</span></span>
    * <span data-ttu-id="70e32-1195">O `set` acessador da `x` é invocado com o valor retornado pelo operador como seu `value` argumento.</span><span class="sxs-lookup"><span data-stu-id="70e32-1195">The `set` accessor of `x` is invoked with the value returned by the operator as its `value` argument.</span></span>
    * <span data-ttu-id="70e32-1196">O valor salvo do `x` se tornará o resultado da operação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1196">The saved value of `x` becomes the result of the operation.</span></span>

<span data-ttu-id="70e32-1197">O `++` e `--` operadores também dão suporte a notação de prefixo ([incremento de prefixo e operadores de decremento](expressions.md#prefix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="70e32-1197">The `++` and `--` operators also support prefix notation ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="70e32-1198">Normalmente, o resultado de `x++` ou `x--` é o valor da `x` antes da operação, enquanto que o resultado da `++x` ou `--x` é o valor do `x` após a operação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1198">Typically, the result of `x++` or `x--` is the value of `x` before the operation, whereas the result of `++x` or `--x` is the value of `x` after the operation.</span></span> <span data-ttu-id="70e32-1199">Em ambos os casos, `x` em si tem o mesmo valor após a operação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1199">In either case, `x` itself has the same value after the operation.</span></span>

<span data-ttu-id="70e32-1200">Uma `operator ++` ou `operator --` implementação pode ser invocada usando a notação de prefixo ou sufixo.</span><span class="sxs-lookup"><span data-stu-id="70e32-1200">An `operator ++` or `operator --` implementation can be invoked using either postfix or prefix notation.</span></span> <span data-ttu-id="70e32-1201">Não é possível ter implementações de operadores separados para as duas notações.</span><span class="sxs-lookup"><span data-stu-id="70e32-1201">It is not possible to have separate operator implementations for the two notations.</span></span>

### <a name="the-new-operator"></a><span data-ttu-id="70e32-1202">O novo operador</span><span class="sxs-lookup"><span data-stu-id="70e32-1202">The new operator</span></span>

<span data-ttu-id="70e32-1203">O `new` operador é usado para criar novas instâncias de tipos.</span><span class="sxs-lookup"><span data-stu-id="70e32-1203">The `new` operator is used to create new instances of types.</span></span>

<span data-ttu-id="70e32-1204">Há três formas de `new` expressões:</span><span class="sxs-lookup"><span data-stu-id="70e32-1204">There are three forms of `new` expressions:</span></span>

*  <span data-ttu-id="70e32-1205">Expressões de criação de objeto são usadas para criar novas instâncias de tipos de classes e tipos de valor.</span><span class="sxs-lookup"><span data-stu-id="70e32-1205">Object creation expressions are used to create new instances of class types and value types.</span></span>
*  <span data-ttu-id="70e32-1206">Expressões de criação de matriz são usadas para criar novas instâncias dos tipos de matriz.</span><span class="sxs-lookup"><span data-stu-id="70e32-1206">Array creation expressions are used to create new instances of array types.</span></span>
*  <span data-ttu-id="70e32-1207">Expressões de criação de delegado são usadas para criar novas instâncias do representante de tipos.</span><span class="sxs-lookup"><span data-stu-id="70e32-1207">Delegate creation expressions are used to create new instances of delegate types.</span></span>

<span data-ttu-id="70e32-1208">O `new` operador implica a criação de uma instância de um tipo, mas não implica necessariamente alocação dinâmica de memória.</span><span class="sxs-lookup"><span data-stu-id="70e32-1208">The `new` operator implies creation of an instance of a type, but does not necessarily imply dynamic allocation of memory.</span></span> <span data-ttu-id="70e32-1209">Em particular, instâncias de tipos de valor não exigem nenhuma memória adicional, além de variáveis em que eles residem, e nenhuma alocação de dinâmica ocorre quando `new` é usado para criar instâncias de tipos de valor.</span><span class="sxs-lookup"><span data-stu-id="70e32-1209">In particular, instances of value types require no additional memory beyond the variables in which they reside, and no dynamic allocations occur when `new` is used to create instances of value types.</span></span>

#### <a name="object-creation-expressions"></a><span data-ttu-id="70e32-1210">Expressões de criação de objeto</span><span class="sxs-lookup"><span data-stu-id="70e32-1210">Object creation expressions</span></span>

<span data-ttu-id="70e32-1211">Uma *object_creation_expression* é usado para criar uma nova instância de um *class_type* ou uma *value_type*.</span><span class="sxs-lookup"><span data-stu-id="70e32-1211">An *object_creation_expression* is used to create a new instance of a *class_type* or a *value_type*.</span></span>

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

<span data-ttu-id="70e32-1212">O *tipo* de uma *object_creation_expression* deve ser um *class_type*, um *value_type* ou uma *type_parameter* .</span><span class="sxs-lookup"><span data-stu-id="70e32-1212">The *type* of an *object_creation_expression* must be a *class_type*, a *value_type* or a *type_parameter*.</span></span> <span data-ttu-id="70e32-1213">O *tipo* não pode ser um `abstract` *class_type*.</span><span class="sxs-lookup"><span data-stu-id="70e32-1213">The *type* cannot be an `abstract` *class_type*.</span></span>

<span data-ttu-id="70e32-1214">Opcional *argument_list* ([listas de argumentos](expressions.md#argument-lists)) é permitido somente se o *tipo* é um *class_type* ou uma *struct_ tipo*.</span><span class="sxs-lookup"><span data-stu-id="70e32-1214">The optional *argument_list* ([Argument lists](expressions.md#argument-lists)) is permitted only if the *type* is a *class_type* or a *struct_type*.</span></span>

<span data-ttu-id="70e32-1215">Uma expressão de criação de objeto pode omitir a lista de argumentos de construtor e colocar parênteses fornecido inclui um inicializador de objeto ou um inicializador de coleção.</span><span class="sxs-lookup"><span data-stu-id="70e32-1215">An object creation expression can omit the constructor argument list and enclosing parentheses provided it includes an object initializer or collection initializer.</span></span> <span data-ttu-id="70e32-1216">Omitir a lista de argumentos de construtor e colocando parênteses são equivalente a especificar uma lista de argumentos vazia.</span><span class="sxs-lookup"><span data-stu-id="70e32-1216">Omitting the constructor argument list and enclosing parentheses is equivalent to specifying an empty argument list.</span></span>

<span data-ttu-id="70e32-1217">Processamento de uma expressão de criação de objeto que inclui um inicializador de objeto ou um inicializador de coleção consiste em processamento pela primeira vez o construtor de instância e, em seguida, processar as inicializações de membro ou o elemento especificadas pelo inicializador de objeto ([ Inicializadores de objeto](expressions.md#object-initializers)) ou o inicializador de coleção ([inicializadores de coleção](expressions.md#collection-initializers)).</span><span class="sxs-lookup"><span data-stu-id="70e32-1217">Processing of an object creation expression that includes an object initializer or collection initializer consists of first processing the instance constructor and then processing the member or element initializations specified by the object initializer ([Object initializers](expressions.md#object-initializers)) or collection initializer ([Collection initializers](expressions.md#collection-initializers)).</span></span>

<span data-ttu-id="70e32-1218">Se qualquer um dos argumentos opcionais *argument_list* tem o tipo de tempo de compilação `dynamic` o *object_creation_expression* associado dinamicamente ([deassociaçãodinâmica](expressions.md#dynamic-binding)) e as seguintes regras são aplicadas em tempo de execução usando o tipo de tempo de execução desses argumentos do *argument_list* que têm o tipo de tempo de compilação `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1218">If any of the arguments in the optional *argument_list* has the compile-time type `dynamic` then the *object_creation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) and the following rules are applied at run-time using the run-time type of those arguments of the *argument_list* that have the compile time type `dynamic`.</span></span> <span data-ttu-id="70e32-1219">No entanto, a criação do objeto sofre uma verificação de tempo de compilação limitada, conforme descrito em [verificação de resolução de sobrecarga de dinâmica de tempo de compilação](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="70e32-1219">However, the object creation undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="70e32-1220">O processamento de tempo de associação de um *object_creation_expression* do formulário `new T(A)`, onde `T` é um *class_type* ou uma *value_type* e `A` é um recurso opcional *argument_list*, consiste as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="70e32-1220">The binding-time processing of an *object_creation_expression* of the form `new T(A)`, where `T` is a *class_type* or a *value_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*   <span data-ttu-id="70e32-1221">Se `T` é um *value_type* e `A` não está presente:</span><span class="sxs-lookup"><span data-stu-id="70e32-1221">If `T` is a *value_type* and `A` is not present:</span></span>
    * <span data-ttu-id="70e32-1222">O *object_creation_expression* é uma invocação do construtor padrão.</span><span class="sxs-lookup"><span data-stu-id="70e32-1222">The *object_creation_expression* is a default constructor invocation.</span></span> <span data-ttu-id="70e32-1223">O resultado do *object_creation_expression* é um valor do tipo `T`, ou seja, o valor padrão para `T` conforme definido na [ValueType o tipo](types.md#the-systemvaluetype-type).</span><span class="sxs-lookup"><span data-stu-id="70e32-1223">The result of the *object_creation_expression* is a value of type `T`, namely the default value for `T` as defined in [The System.ValueType type](types.md#the-systemvaluetype-type).</span></span>
*   <span data-ttu-id="70e32-1224">Caso contrário, se `T` é um *type_parameter* e `A` não está presente:</span><span class="sxs-lookup"><span data-stu-id="70e32-1224">Otherwise, if `T` is a *type_parameter* and `A` is not present:</span></span>
    * <span data-ttu-id="70e32-1225">Se nenhuma restrição de tipo de valor ou uma restrição de construtor ([restrições de parâmetro de tipo](classes.md#type-parameter-constraints)) foi especificado para `T`, ocorrerá um erro em tempo de associação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1225">If no value type constraint or constructor constraint ([Type parameter constraints](classes.md#type-parameter-constraints)) has been specified for `T`, a binding-time error occurs.</span></span>
    * <span data-ttu-id="70e32-1226">O resultado do *object_creation_expression* é um valor do tipo de tempo de execução que o parâmetro de tipo foi associado, ou seja, o resultado de chamar o construtor padrão desse tipo.</span><span class="sxs-lookup"><span data-stu-id="70e32-1226">The result of the *object_creation_expression* is a value of the run-time type that the type parameter has been bound to, namely the result of invoking the default constructor of that type.</span></span> <span data-ttu-id="70e32-1227">O tipo de tempo de execução pode ser um tipo de referência ou um tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="70e32-1227">The run-time type may be a reference type or a value type.</span></span>
*   <span data-ttu-id="70e32-1228">Caso contrário, se `T` é um *class_type* ou uma *struct_type*:</span><span class="sxs-lookup"><span data-stu-id="70e32-1228">Otherwise, if `T` is a *class_type* or a *struct_type*:</span></span>
    * <span data-ttu-id="70e32-1229">Se `T` é um `abstract` *class_type*, ocorre um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1229">If `T` is an `abstract` *class_type*, a compile-time error occurs.</span></span>
    * <span data-ttu-id="70e32-1230">Para invocar o construtor de instância é determinado usando as regras de resolução de sobrecarga de [resolução de sobrecarga](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="70e32-1230">The instance constructor to invoke is determined using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="70e32-1231">O conjunto de construtores de instância de candidato consiste em todos os construtores de instância acessível declarados em `T` que são aplicável em relação às `A` ([membro da função aplicável](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="70e32-1231">The set of candidate instance constructors consists of all accessible instance constructors declared in `T` which are applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span> <span data-ttu-id="70e32-1232">Se o conjunto de construtores de instância de release candidate está vazio ou se um construtor de instância única melhor não pode ser identificado, ocorrerá um erro de tempo de associação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1232">If the set of candidate instance constructors is empty, or if a single best instance constructor cannot be identified, a binding-time error occurs.</span></span>
    * <span data-ttu-id="70e32-1233">O resultado do *object_creation_expression* é um valor do tipo `T`, ou seja, o valor produzido por invocar o construtor de instância determinado na etapa anterior.</span><span class="sxs-lookup"><span data-stu-id="70e32-1233">The result of the *object_creation_expression* is a value of type `T`, namely the value produced by invoking the instance constructor determined in the step above.</span></span>
*  <span data-ttu-id="70e32-1234">Caso contrário, o *object_creation_expression* for inválido, um erro de tempo de associação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1234">Otherwise, the *object_creation_expression* is invalid, and a binding-time error occurs.</span></span>

<span data-ttu-id="70e32-1235">Mesmo se o *object_creation_expression* é dinamicamente vinculado, o tipo de tempo de compilação é ainda `T`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1235">Even if the *object_creation_expression* is dynamically bound, the compile-time type is still `T`.</span></span>

<span data-ttu-id="70e32-1236">O processamento de tempo de execução de um *object_creation_expression* do formulário `new T(A)`, onde `T` está *class_type* ou uma *struct_type* e `A` é um recurso opcional *argument_list*, consiste as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="70e32-1236">The run-time processing of an *object_creation_expression* of the form `new T(A)`, where `T` is *class_type* or a *struct_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*   <span data-ttu-id="70e32-1237">Se `T` é um *class_type*:</span><span class="sxs-lookup"><span data-stu-id="70e32-1237">If `T` is a *class_type*:</span></span>
    * <span data-ttu-id="70e32-1238">Uma nova instância da classe `T` é alocado.</span><span class="sxs-lookup"><span data-stu-id="70e32-1238">A new instance of class `T` is allocated.</span></span> <span data-ttu-id="70e32-1239">Se não houver memória suficiente disponível para alocar a nova instância, um `System.OutOfMemoryException` é lançada e nenhuma outra etapa é executada.</span><span class="sxs-lookup"><span data-stu-id="70e32-1239">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="70e32-1240">Todos os campos da nova instância são inicializados com seus valores padrão ([valores padrão](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="70e32-1240">All fields of the new instance are initialized to their default values ([Default values](variables.md#default-values)).</span></span>
    * <span data-ttu-id="70e32-1241">O construtor de instância é invocado de acordo com as regras da invocação de membro de função ([verificação de resolução de sobrecarga de dinâmica de tempo de compilação](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="70e32-1241">The instance constructor is invoked according to the rules of function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span> <span data-ttu-id="70e32-1242">Uma referência à instância recentemente alocada automaticamente é passada para o construtor de instância e a instância pode ser acessada de dentro desse construtor como `this`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1242">A reference to the newly allocated instance is automatically passed to the instance constructor and the instance can be accessed from within that constructor as `this`.</span></span>
*   <span data-ttu-id="70e32-1243">Se `T` é um *struct_type*:</span><span class="sxs-lookup"><span data-stu-id="70e32-1243">If `T` is a *struct_type*:</span></span>
    * <span data-ttu-id="70e32-1244">Uma instância do tipo `T` é criado ao alocar uma variável local temporária.</span><span class="sxs-lookup"><span data-stu-id="70e32-1244">An instance of type `T` is created by allocating a temporary local variable.</span></span> <span data-ttu-id="70e32-1245">Desde um construtor de instância de um *struct_type* é necessária, definitivamente, atribuir um valor para cada campo da instância que está sendo criado, nenhuma inicialização da variável temporária é necessária.</span><span class="sxs-lookup"><span data-stu-id="70e32-1245">Since an instance constructor of a *struct_type* is required to definitely assign a value to each field of the instance being created, no initialization of the temporary variable is necessary.</span></span>
    * <span data-ttu-id="70e32-1246">O construtor de instância é invocado de acordo com as regras da invocação de membro de função ([verificação de resolução de sobrecarga de dinâmica de tempo de compilação](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="70e32-1246">The instance constructor is invoked according to the rules of function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span> <span data-ttu-id="70e32-1247">Uma referência à instância recentemente alocada automaticamente é passada para o construtor de instância e a instância pode ser acessada de dentro desse construtor como `this`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1247">A reference to the newly allocated instance is automatically passed to the instance constructor and the instance can be accessed from within that constructor as `this`.</span></span>

#### <a name="object-initializers"></a><span data-ttu-id="70e32-1248">Inicializadores de objeto</span><span class="sxs-lookup"><span data-stu-id="70e32-1248">Object initializers</span></span>

<span data-ttu-id="70e32-1249">Uma ***inicializador de objeto*** especifica valores para zero ou mais campos, propriedades ou elementos indexados de um objeto.</span><span class="sxs-lookup"><span data-stu-id="70e32-1249">An ***object initializer*** specifies values for zero or more fields, properties or indexed elements of an object.</span></span>

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

<span data-ttu-id="70e32-1250">Um inicializador de objeto consiste em uma sequência de inicializadores de membro, delimitados por `{` e `}` tokens e separados por vírgulas.</span><span class="sxs-lookup"><span data-stu-id="70e32-1250">An object initializer consists of a sequence of member initializers, enclosed by `{` and `}` tokens and separated by commas.</span></span> <span data-ttu-id="70e32-1251">Cada *member_initializer* designa um destino para a inicialização.</span><span class="sxs-lookup"><span data-stu-id="70e32-1251">Each *member_initializer* designates a target for the initialization.</span></span> <span data-ttu-id="70e32-1252">Uma *identificador* deve nomear um campo acessível ou uma propriedade do objeto que está sendo inicializado, enquanto um *argument_list* colocados entre colchetes deve especificar argumentos para um indexador acessível no objeto que está sendo inicializado.</span><span class="sxs-lookup"><span data-stu-id="70e32-1252">An *identifier* must name an accessible field or property of the object being initialized, whereas an *argument_list* enclosed in square brackets must specify arguments for an accessible indexer on the object being initialized.</span></span> <span data-ttu-id="70e32-1253">É um erro para um inicializador de objeto incluir mais de um inicializador de membro para o mesmo campo ou propriedade.</span><span class="sxs-lookup"><span data-stu-id="70e32-1253">It is an error for an object initializer to include more than one member initializer for the same field or property.</span></span>

<span data-ttu-id="70e32-1254">Cada *initializer_target* é seguido por um sinal de igual e uma expressão, um inicializador de objeto ou um inicializador de coleção.</span><span class="sxs-lookup"><span data-stu-id="70e32-1254">Each *initializer_target* is followed by an equals sign and either an expression, an object initializer or a collection initializer.</span></span> <span data-ttu-id="70e32-1255">Não é possível para expressões no inicializador de objeto para fazer referência ao objeto recém-criado que está sendo inicializado.</span><span class="sxs-lookup"><span data-stu-id="70e32-1255">It is not possible for expressions within the object initializer to refer to the newly created object it is initializing.</span></span>

<span data-ttu-id="70e32-1256">Um inicializador de membro que especifica uma expressão depois que o sinal de igual é processado da mesma maneira que uma atribuição ([atribuição simples](expressions.md#simple-assignment)) para o destino.</span><span class="sxs-lookup"><span data-stu-id="70e32-1256">A member initializer that specifies an expression after the equals sign is processed in the same way as an assignment ([Simple assignment](expressions.md#simple-assignment)) to the target.</span></span>

<span data-ttu-id="70e32-1257">Um inicializador de membro que especifica um inicializador de objeto após o sinal de igual é um ***inicializador de objeto aninhado***, ou seja, uma inicialização de um objeto inserido.</span><span class="sxs-lookup"><span data-stu-id="70e32-1257">A member initializer that specifies an object initializer after the equals sign is a ***nested object initializer***, i.e. an initialization of an embedded object.</span></span> <span data-ttu-id="70e32-1258">Em vez de atribuir um novo valor para o campo ou propriedade, as atribuições no inicializador de objeto aninhado são tratadas como atribuições aos membros do campo ou propriedade.</span><span class="sxs-lookup"><span data-stu-id="70e32-1258">Instead of assigning a new value to the field or property, the assignments in the nested object initializer are treated as assignments to members of the field or property.</span></span> <span data-ttu-id="70e32-1259">Inicializadores de objeto aninhado não podem ser aplicados a propriedades com um tipo de valor, ou a campos somente leitura com um tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="70e32-1259">Nested object initializers cannot be applied to properties with a value type, or to read-only fields with a value type.</span></span>

<span data-ttu-id="70e32-1260">Um inicializador de membro que especifica um inicializador de coleção após o sinal de igual é uma inicialização de uma coleção inserida.</span><span class="sxs-lookup"><span data-stu-id="70e32-1260">A member initializer that specifies a collection initializer after the equals sign is an initialization of an embedded collection.</span></span> <span data-ttu-id="70e32-1261">Em vez de atribuir uma nova coleção para o campo de destino, propriedade ou indexador, os elementos fornecidos no inicializador são adicionados à coleção referenciada pelo destino.</span><span class="sxs-lookup"><span data-stu-id="70e32-1261">Instead of assigning a new collection to the target field, property or indexer, the elements given in the initializer are added to the collection referenced by the target.</span></span> <span data-ttu-id="70e32-1262">O destino deve ser de um tipo de coleção que atende aos requisitos especificados em [inicializadores de coleção](expressions.md#collection-initializers).</span><span class="sxs-lookup"><span data-stu-id="70e32-1262">The target must be of a collection type that satisfies the requirements specified in [Collection initializers](expressions.md#collection-initializers).</span></span>

<span data-ttu-id="70e32-1263">Os argumentos para um inicializador de índice sempre serão avaliados exatamente uma vez.</span><span class="sxs-lookup"><span data-stu-id="70e32-1263">The arguments to an index initializer will always be evaluated exactly once.</span></span> <span data-ttu-id="70e32-1264">Portanto, mesmo se os argumentos acabam nunca sendo usado (por exemplo, devido a um inicializador vazio aninhado), eles serão avaliados por seus efeitos colaterais.</span><span class="sxs-lookup"><span data-stu-id="70e32-1264">Thus, even if the arguments end up never getting used (e.g. because of an empty nested initializer), they will be evaluated for their side effects.</span></span>

<span data-ttu-id="70e32-1265">A classe a seguir representa um ponto com duas coordenadas:</span><span class="sxs-lookup"><span data-stu-id="70e32-1265">The following class represents a point with two coordinates:</span></span>
```csharp
public class Point
{
    int x, y;

    public int X { get { return x; } set { x = value; } }
    public int Y { get { return y; } set { y = value; } }
}
```

<span data-ttu-id="70e32-1266">Uma instância de `Point` pode ser criado e inicializado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="70e32-1266">An instance of `Point` can be created and initialized as follows:</span></span>
```csharp
Point a = new Point { X = 0, Y = 1 };
```
<span data-ttu-id="70e32-1267">que tem o mesmo efeito que</span><span class="sxs-lookup"><span data-stu-id="70e32-1267">which has the same effect as</span></span>
```csharp
Point __a = new Point();
__a.X = 0;
__a.Y = 1; 
Point a = __a;
```
<span data-ttu-id="70e32-1268">onde `__a` é uma variável temporária caso contrário, invisível e inacessível.</span><span class="sxs-lookup"><span data-stu-id="70e32-1268">where `__a` is an otherwise invisible and inaccessible temporary variable.</span></span> <span data-ttu-id="70e32-1269">A classe a seguir representa um retângulo criado a partir de dois pontos:</span><span class="sxs-lookup"><span data-stu-id="70e32-1269">The following class represents a rectangle created from two points:</span></span>
```csharp
public class Rectangle
{
    Point p1, p2;

    public Point P1 { get { return p1; } set { p1 = value; } }
    public Point P2 { get { return p2; } set { p2 = value; } }
}
```

<span data-ttu-id="70e32-1270">Uma instância de `Rectangle` pode ser criado e inicializado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="70e32-1270">An instance of `Rectangle` can be created and initialized as follows:</span></span>
```csharp
Rectangle r = new Rectangle {
    P1 = new Point { X = 0, Y = 1 },
    P2 = new Point { X = 2, Y = 3 }
};
```
<span data-ttu-id="70e32-1271">que tem o mesmo efeito que</span><span class="sxs-lookup"><span data-stu-id="70e32-1271">which has the same effect as</span></span>
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
<span data-ttu-id="70e32-1272">em que `__r`, `__p1` e `__p2` são variáveis temporárias que estariam invisíveis e inacessíveis.</span><span class="sxs-lookup"><span data-stu-id="70e32-1272">where `__r`, `__p1` and `__p2` are temporary variables that are otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="70e32-1273">Se `Rectangle`do construtor aloca os dois inseridos `Point` instâncias</span><span class="sxs-lookup"><span data-stu-id="70e32-1273">If `Rectangle`'s constructor allocates the two embedded `Point` instances</span></span>
```csharp
public class Rectangle
{
    Point p1 = new Point();
    Point p2 = new Point();

    public Point P1 { get { return p1; } }
    public Point P2 { get { return p2; } }
}
```
<span data-ttu-id="70e32-1274">a construção a seguir pode ser usada para inicializar o embedded `Point` instâncias em vez de atribuir novas instâncias:</span><span class="sxs-lookup"><span data-stu-id="70e32-1274">the following construct can be used to initialize the embedded `Point` instances instead of assigning new instances:</span></span>
```csharp
Rectangle r = new Rectangle {
    P1 = { X = 0, Y = 1 },
    P2 = { X = 2, Y = 3 }
};
```
<span data-ttu-id="70e32-1275">que tem o mesmo efeito que</span><span class="sxs-lookup"><span data-stu-id="70e32-1275">which has the same effect as</span></span>
```csharp
Rectangle __r = new Rectangle();
__r.P1.X = 0;
__r.P1.Y = 1;
__r.P2.X = 2;
__r.P2.Y = 3;
Rectangle r = __r;
```

<span data-ttu-id="70e32-1276">Dada uma definição de apropriada de C, o exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="70e32-1276">Given an appropriate definition of C, the following example:</span></span>
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
<span data-ttu-id="70e32-1277">é equivalente a esta série de atribuições de:</span><span class="sxs-lookup"><span data-stu-id="70e32-1277">is equivalent to this series of assignments:</span></span>
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
<span data-ttu-id="70e32-1278">onde `__c`, etc., são geradas variáveis que são invisíveis e inacessíveis para o código-fonte.</span><span class="sxs-lookup"><span data-stu-id="70e32-1278">where `__c`, etc., are generated variables that are invisible and inaccessible to the source code.</span></span> <span data-ttu-id="70e32-1279">Observe que os argumentos para `[0,0]` são avaliada apenas uma vez e os argumentos para `[1,2]` são avaliadas uma vez, mesmo que eles nunca são usados.</span><span class="sxs-lookup"><span data-stu-id="70e32-1279">Note that the arguments for `[0,0]` are evaluated only once, and the arguments for `[1,2]` are evaluated once even though they are never used.</span></span>

#### <a name="collection-initializers"></a><span data-ttu-id="70e32-1280">Inicializadores de coleção</span><span class="sxs-lookup"><span data-stu-id="70e32-1280">Collection initializers</span></span>

<span data-ttu-id="70e32-1281">Um inicializador de coleção Especifica os elementos de uma coleção.</span><span class="sxs-lookup"><span data-stu-id="70e32-1281">A collection initializer specifies the elements of a collection.</span></span>

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

<span data-ttu-id="70e32-1282">Um inicializador de coleção consiste em uma sequência de inicializadores de elemento, entre `{` e `}` tokens e separados por vírgulas.</span><span class="sxs-lookup"><span data-stu-id="70e32-1282">A collection initializer consists of a sequence of element initializers, enclosed by `{` and `}` tokens and separated by commas.</span></span> <span data-ttu-id="70e32-1283">Cada inicializador de elemento Especifica um elemento a ser adicionado ao objeto de coleção que está sendo inicializado e consiste em uma lista de expressões entre `{` e `}` tokens e separados por vírgulas.</span><span class="sxs-lookup"><span data-stu-id="70e32-1283">Each element initializer specifies an element to be added to the collection object being initialized, and consists of a list of expressions enclosed by `{` and `}` tokens and separated by commas.</span></span>  <span data-ttu-id="70e32-1284">Um inicializador de elemento de expressão única pode ser escrito sem as chaves, mas, em seguida, não pode ser uma expressão de atribuição, para evitar ambiguidade com inicializadores de membro.</span><span class="sxs-lookup"><span data-stu-id="70e32-1284">A single-expression element initializer can be written without braces, but cannot then be an assignment expression, to avoid ambiguity with member initializers.</span></span> <span data-ttu-id="70e32-1285">O *non_assignment_expression* produção é definida no [expressão](expressions.md#expression).</span><span class="sxs-lookup"><span data-stu-id="70e32-1285">The *non_assignment_expression* production is defined in [Expression](expressions.md#expression).</span></span>

<span data-ttu-id="70e32-1286">Este é um exemplo de uma expressão de criação de objeto que inclui um inicializador de coleção:</span><span class="sxs-lookup"><span data-stu-id="70e32-1286">The following is an example of an object creation expression that includes a collection initializer:</span></span>
```csharp
List<int> digits = new List<int> { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
```

<span data-ttu-id="70e32-1287">O objeto de coleção ao qual um inicializador de coleção é aplicado deve ser de um tipo que implementa `System.Collections.IEnumerable` ou ocorre um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1287">The collection object to which a collection initializer is applied must be of a type that implements `System.Collections.IEnumerable` or a compile-time error occurs.</span></span> <span data-ttu-id="70e32-1288">Para cada elemento na ordem, o inicializador de coleção invoca um `Add` método no destino do objeto com a lista de expressões do inicializador de elemento como a lista de argumentos, aplicação de pesquisa de membro normal e resolução para cada invocação de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="70e32-1288">For each specified element in order, the collection initializer invokes an `Add` method on the target object with the expression list of the element initializer as argument list, applying normal member lookup and overload resolution for each invocation.</span></span> <span data-ttu-id="70e32-1289">Portanto, o objeto de coleção deve ter um método de instância ou extensão aplicável, com o nome `Add` para cada inicializador de elemento.</span><span class="sxs-lookup"><span data-stu-id="70e32-1289">Thus, the collection object must have an applicable instance or extension method with the name `Add` for each element initializer.</span></span>

<span data-ttu-id="70e32-1290">A classe a seguir representa um contato com um nome e uma lista de números de telefone:</span><span class="sxs-lookup"><span data-stu-id="70e32-1290">The following class represents a contact with a name and a list of phone numbers:</span></span>
```csharp
public class Contact
{
    string name;
    List<string> phoneNumbers = new List<string>();

    public string Name { get { return name; } set { name = value; } }

    public List<string> PhoneNumbers { get { return phoneNumbers; } }
}
```

<span data-ttu-id="70e32-1291">Um `List<Contact>` pode ser criado e inicializado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="70e32-1291">A `List<Contact>` can be created and initialized as follows:</span></span>
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
<span data-ttu-id="70e32-1292">que tem o mesmo efeito que</span><span class="sxs-lookup"><span data-stu-id="70e32-1292">which has the same effect as</span></span>
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
<span data-ttu-id="70e32-1293">em que `__clist`, `__c1` e `__c2` são variáveis temporárias que estariam invisíveis e inacessíveis.</span><span class="sxs-lookup"><span data-stu-id="70e32-1293">where `__clist`, `__c1` and `__c2` are temporary variables that are otherwise invisible and inaccessible.</span></span>

#### <a name="array-creation-expressions"></a><span data-ttu-id="70e32-1294">Expressões de criação de matriz</span><span class="sxs-lookup"><span data-stu-id="70e32-1294">Array creation expressions</span></span>

<span data-ttu-id="70e32-1295">Uma *array_creation_expression* é usado para criar uma nova instância de um *array_type*.</span><span class="sxs-lookup"><span data-stu-id="70e32-1295">An *array_creation_expression* is used to create a new instance of an *array_type*.</span></span>

```antlr
array_creation_expression
    : 'new' non_array_type '[' expression_list ']' rank_specifier* array_initializer?
    | 'new' array_type array_initializer
    | 'new' rank_specifier array_initializer
    ;
```

<span data-ttu-id="70e32-1296">Uma expressão de criação de matriz do primeiro formulário aloca uma instância de matriz do tipo que é o resultado da exclusão de cada uma das expressões individuais da lista de expressões.</span><span class="sxs-lookup"><span data-stu-id="70e32-1296">An array creation expression of the first form allocates an array instance of the type that results from deleting each of the individual expressions from the expression list.</span></span> <span data-ttu-id="70e32-1297">Por exemplo, a expressão de criação de matriz `new int[10,20]` produz uma instância de matriz do tipo `int[,]`e a expressão de criação de matriz `new int[10][,]` produz uma matriz do tipo `int[][,]`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1297">For example, the array creation expression `new int[10,20]` produces an array instance of type `int[,]`, and the array creation expression `new int[10][,]` produces an array of type `int[][,]`.</span></span> <span data-ttu-id="70e32-1298">Cada expressão na lista da expressão deve ser do tipo `int`, `uint`, `long`, ou `ulong`, ou implicitamente conversível em um ou mais desses tipos.</span><span class="sxs-lookup"><span data-stu-id="70e32-1298">Each expression in the expression list must be of type `int`, `uint`, `long`, or `ulong`, or implicitly convertible to one or more of these types.</span></span> <span data-ttu-id="70e32-1299">O valor de cada expressão determina o comprimento da dimensão correspondente na instância de matriz recém-alocada.</span><span class="sxs-lookup"><span data-stu-id="70e32-1299">The value of each expression determines the length of the corresponding dimension in the newly allocated array instance.</span></span> <span data-ttu-id="70e32-1300">Como o comprimento de uma dimensão de matriz deve ser não negativo, ele é um erro de tempo de compilação para ter uma *constant_expression* com um valor negativo na lista de expressões.</span><span class="sxs-lookup"><span data-stu-id="70e32-1300">Since the length of an array dimension must be nonnegative, it is a compile-time error to have a *constant_expression* with a negative value in the expression list.</span></span>

<span data-ttu-id="70e32-1301">Exceto em um contexto sem segurança ([contextos não seguros](unsafe-code.md#unsafe-contexts)), o layout de matrizes não é especificado.</span><span class="sxs-lookup"><span data-stu-id="70e32-1301">Except in an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)), the layout of arrays is unspecified.</span></span>

<span data-ttu-id="70e32-1302">Se uma expressão de criação de matriz do primeiro formulário inclui um inicializador de matriz, cada expressão na lista da expressão deve ser uma constante e os comprimentos de dimensão e de classificação especificados pela lista de expressão devem corresponder do inicializador de matriz.</span><span class="sxs-lookup"><span data-stu-id="70e32-1302">If an array creation expression of the first form includes an array initializer, each expression in the expression list must be a constant and the rank and dimension lengths specified by the expression list must match those of the array initializer.</span></span>

<span data-ttu-id="70e32-1303">Em uma expressão de criação de matriz no formato de segundo ou terceiro, a classificação do especificador de tipo ou a classificação de matriz especificado deve corresponder do inicializador de matriz.</span><span class="sxs-lookup"><span data-stu-id="70e32-1303">In an array creation expression of the second or third form, the rank of the specified array type or rank specifier must match that of the array initializer.</span></span> <span data-ttu-id="70e32-1304">Os comprimentos de dimensão individuais são inferidos do número de elementos em cada um dos níveis de aninhamento correspondentes do inicializador de matriz.</span><span class="sxs-lookup"><span data-stu-id="70e32-1304">The individual dimension lengths are inferred from the number of elements in each of the corresponding nesting levels of the array initializer.</span></span> <span data-ttu-id="70e32-1305">Assim, a expressão</span><span class="sxs-lookup"><span data-stu-id="70e32-1305">Thus, the expression</span></span>
```csharp
new int[,] {{0, 1}, {2, 3}, {4, 5}}
```
<span data-ttu-id="70e32-1306">corresponde exatamente ao</span><span class="sxs-lookup"><span data-stu-id="70e32-1306">exactly corresponds to</span></span>
```csharp
new int[3, 2] {{0, 1}, {2, 3}, {4, 5}}
```

<span data-ttu-id="70e32-1307">Uma expressão de criação de matriz do terceiro formulário é conhecida como um ***expressão de criação de matriz de tipo implícito***.</span><span class="sxs-lookup"><span data-stu-id="70e32-1307">An array creation expression of the third form is referred to as an ***implicitly typed array creation expression***.</span></span> <span data-ttu-id="70e32-1308">Ele é semelhante ao segundo formato, exceto que o tipo de elemento da matriz não for explicitamente fornecido, mas é determinado como o tipo mais comum ([localizando o melhor tipo comum de um conjunto de expressões](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) do conjunto de expressões na matriz inicializador.</span><span class="sxs-lookup"><span data-stu-id="70e32-1308">It is similar to the second form, except that the element type of the array is not explicitly given, but determined as the best common type ([Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) of the set of expressions in the array initializer.</span></span> <span data-ttu-id="70e32-1309">Para uma matriz multidimensional, ou seja, aquele em que o *rank_specifier* contém pelo menos uma vírgula, este conjunto consiste em todos *expressão*s encontrado no aninhados *array_initializer*s.</span><span class="sxs-lookup"><span data-stu-id="70e32-1309">For a multidimensional array, i.e., one where the *rank_specifier* contains at least one comma, this set comprises all *expression*s found in nested *array_initializer*s.</span></span>

<span data-ttu-id="70e32-1310">Inicializadores de matriz são descritos mais detalhadamente em [inicializadores de matriz](arrays.md#array-initializers).</span><span class="sxs-lookup"><span data-stu-id="70e32-1310">Array initializers are described further in [Array initializers](arrays.md#array-initializers).</span></span>

<span data-ttu-id="70e32-1311">O resultado da avaliação de uma expressão de criação de matriz é classificado como um valor, ou seja, uma referência para a instância de matriz recém-alocada.</span><span class="sxs-lookup"><span data-stu-id="70e32-1311">The result of evaluating an array creation expression is classified as a value, namely a reference to the newly allocated array instance.</span></span> <span data-ttu-id="70e32-1312">O processamento de tempo de execução de uma expressão de criação de matriz consiste as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="70e32-1312">The run-time processing of an array creation expression consists of the following steps:</span></span>

*  <span data-ttu-id="70e32-1313">As expressões de comprimento da dimensão do *lista_de_expressão* são avaliados na ordem da esquerda para a direita.</span><span class="sxs-lookup"><span data-stu-id="70e32-1313">The dimension length expressions of the *expression_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="70e32-1314">Após a avaliação de cada expressão, uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)) é executada para um dos seguintes tipos: `int`, `uint`, `long`, `ulong`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1314">Following evaluation of each expression, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to one of the following types is performed: `int`, `uint`, `long`, `ulong`.</span></span> <span data-ttu-id="70e32-1315">O primeiro tipo nessa lista para o qual existe uma conversão implícita é escolhido.</span><span class="sxs-lookup"><span data-stu-id="70e32-1315">The first type in this list for which an implicit conversion exists is chosen.</span></span> <span data-ttu-id="70e32-1316">Se a avaliação de uma expressão ou a conversão implícita subsequente faz com que uma exceção, não há mais expressões são avaliadas, e nenhuma outra etapa é executada.</span><span class="sxs-lookup"><span data-stu-id="70e32-1316">If evaluation of an expression or the subsequent implicit conversion causes an exception, then no further expressions are evaluated and no further steps are executed.</span></span>
*  <span data-ttu-id="70e32-1317">Os valores computados para os tamanhos da dimensão são validados da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="70e32-1317">The computed values for the dimension lengths are validated as follows.</span></span> <span data-ttu-id="70e32-1318">Se um ou mais dos valores são menor que zero, um `System.OverflowException` é lançada e nenhuma outra etapa é executada.</span><span class="sxs-lookup"><span data-stu-id="70e32-1318">If one or more of the values are less than zero, a `System.OverflowException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="70e32-1319">Uma instância de matriz com os comprimentos de determinada dimensão é alocada.</span><span class="sxs-lookup"><span data-stu-id="70e32-1319">An array instance with the given dimension lengths is allocated.</span></span> <span data-ttu-id="70e32-1320">Se não houver memória suficiente disponível para alocar a nova instância, um `System.OutOfMemoryException` é lançada e nenhuma outra etapa é executada.</span><span class="sxs-lookup"><span data-stu-id="70e32-1320">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="70e32-1321">Todos os elementos da nova instância de matriz são inicializados com seus valores padrão ([valores padrão](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="70e32-1321">All elements of the new array instance are initialized to their default values ([Default values](variables.md#default-values)).</span></span>
*  <span data-ttu-id="70e32-1322">Se a expressão de criação de matriz contiver um inicializador de matriz, cada expressão de inicializador de matriz é avaliada e atribuída ao seu elemento correspondente da matriz.</span><span class="sxs-lookup"><span data-stu-id="70e32-1322">If the array creation expression contains an array initializer, then each expression in the array initializer is evaluated and assigned to its corresponding array element.</span></span> <span data-ttu-id="70e32-1323">As atribuições e avaliações são executadas na ordem em que as expressões são gravadas no inicializador de matriz — em outras palavras, os elementos são inicializados em índice ordem crescente, com a dimensão mais à direita, aumentando pela primeira vez.</span><span class="sxs-lookup"><span data-stu-id="70e32-1323">The evaluations and assignments are performed in the order the expressions are written in the array initializer—in other words, elements are initialized in increasing index order, with the rightmost dimension increasing first.</span></span> <span data-ttu-id="70e32-1324">Se a avaliação de uma determinada expressão ou atribuição subsequente para o elemento de matriz correspondente faz com que uma exceção, em seguida, sem elementos adicionais são inicializados (e os elementos restantes, portanto, terão seus valores padrão).</span><span class="sxs-lookup"><span data-stu-id="70e32-1324">If evaluation of a given expression or the subsequent assignment to the corresponding array element causes an exception, then no further elements are initialized (and the remaining elements will thus have their default values).</span></span>

<span data-ttu-id="70e32-1325">Uma expressão de criação de matriz permite a instanciação de uma matriz com elementos de um tipo de matriz, mas os elementos de como uma matriz devem ser inicializados manualmente.</span><span class="sxs-lookup"><span data-stu-id="70e32-1325">An array creation expression permits instantiation of an array with elements of an array type, but the elements of such an array must be manually initialized.</span></span> <span data-ttu-id="70e32-1326">Por exemplo, a instrução</span><span class="sxs-lookup"><span data-stu-id="70e32-1326">For example, the statement</span></span>
```csharp
int[][] a = new int[100][];
```
<span data-ttu-id="70e32-1327">cria uma matriz unidimensional com 100 elementos do tipo `int[]`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1327">creates a single-dimensional array with 100 elements of type `int[]`.</span></span> <span data-ttu-id="70e32-1328">O valor inicial de cada elemento é `null`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1328">The initial value of each element is `null`.</span></span> <span data-ttu-id="70e32-1329">Não é possível para a mesma expressão de criação de matriz para também criar uma instância de submatrizes e a instrução</span><span class="sxs-lookup"><span data-stu-id="70e32-1329">It is not possible for the same array creation expression to also instantiate the sub-arrays, and the statement</span></span>
```csharp
int[][] a = new int[100][5];        // Error
```
<span data-ttu-id="70e32-1330">resultados em um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1330">results in a compile-time error.</span></span> <span data-ttu-id="70e32-1331">Instanciação de subpropriedades matrizes em vez disso, deve ser executada manualmente, como em</span><span class="sxs-lookup"><span data-stu-id="70e32-1331">Instantiation of the sub-arrays must instead be performed manually, as in</span></span>
```csharp
int[][] a = new int[100][];
for (int i = 0; i < 100; i++) a[i] = new int[5];
```

<span data-ttu-id="70e32-1332">Quando uma matriz de matrizes tem um formato "retangular", ou seja, quando as matrizes de subpropriedades são todos do mesmo comprimento, é mais eficiente usar uma matriz multidimensional.</span><span class="sxs-lookup"><span data-stu-id="70e32-1332">When an array of arrays has a "rectangular" shape, that is when the sub-arrays are all of the same length, it is more efficient to use a multi-dimensional array.</span></span> <span data-ttu-id="70e32-1333">No exemplo acima, a instanciação da matriz de matrizes cria 101 objetos — uma matriz externa e submatrizes de 100.</span><span class="sxs-lookup"><span data-stu-id="70e32-1333">In the example above, instantiation of the array of arrays creates 101 objects—one outer array and 100 sub-arrays.</span></span> <span data-ttu-id="70e32-1334">Por outro lado,</span><span class="sxs-lookup"><span data-stu-id="70e32-1334">In contrast,</span></span>
```csharp
int[,] = new int[100, 5];
```
<span data-ttu-id="70e32-1335">cria apenas um único objeto, uma matriz bidimensional e realiza a alocação em uma única instrução.</span><span class="sxs-lookup"><span data-stu-id="70e32-1335">creates only a single object, a two-dimensional array, and accomplishes the allocation in a single statement.</span></span>

<span data-ttu-id="70e32-1336">A seguir estão exemplos de expressões de criação de matriz de tipo implícito:</span><span class="sxs-lookup"><span data-stu-id="70e32-1336">The following are examples of implicitly typed array creation expressions:</span></span>
```csharp
var a = new[] { 1, 10, 100, 1000 };                       // int[]

var b = new[] { 1, 1.5, 2, 2.5 };                         // double[]

var c = new[,] { { "hello", null }, { "world", "!" } };   // string[,]

var d = new[] { 1, "one", 2, "two" };                     // Error
```

<span data-ttu-id="70e32-1337">A última expressão causa um erro de tempo de compilação porque nem `int` nem `string` é implicitamente conversível para o outro e, em seguida, digite aqui é não é mais comum.</span><span class="sxs-lookup"><span data-stu-id="70e32-1337">The last expression causes a compile-time error because neither `int` nor `string` is implicitly convertible to the other, and so there is no best common type.</span></span> <span data-ttu-id="70e32-1338">Uma expressão de criação de matriz tipada explicitamente deve ser usada nesse caso, por exemplo, especificando o tipo a ser `object[]`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1338">An explicitly typed array creation expression must be used in this case, for example specifying the type to be `object[]`.</span></span> <span data-ttu-id="70e32-1339">Como alternativa, um dos elementos pode ser convertido em um tipo de base comum, que, em seguida, iria se tornar o tipo do elemento deduzido.</span><span class="sxs-lookup"><span data-stu-id="70e32-1339">Alternatively, one of the elements can be cast to a common base type, which would then become the inferred element type.</span></span>

<span data-ttu-id="70e32-1340">Expressões de criação de matriz implícita podem ser combinadas com inicializadores de objeto anônimos ([expressões de criação de objeto anônimo](expressions.md#anonymous-object-creation-expressions)) criar anonimamente digitado estruturas de dados.</span><span class="sxs-lookup"><span data-stu-id="70e32-1340">Implicitly typed array creation expressions can be combined with anonymous object initializers ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) to create anonymously typed data structures.</span></span> <span data-ttu-id="70e32-1341">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="70e32-1341">For example:</span></span>
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

#### <a name="delegate-creation-expressions"></a><span data-ttu-id="70e32-1342">Expressões de criação de delegado</span><span class="sxs-lookup"><span data-stu-id="70e32-1342">Delegate creation expressions</span></span>

<span data-ttu-id="70e32-1343">Um *delegate_creation_expression* é usado para criar uma nova instância de um *delegate_type*.</span><span class="sxs-lookup"><span data-stu-id="70e32-1343">A *delegate_creation_expression* is used to create a new instance of a *delegate_type*.</span></span>

```antlr
delegate_creation_expression
    : 'new' delegate_type '(' expression ')'
    ;
```

<span data-ttu-id="70e32-1344">O argumento de uma expressão de criação de delegado deve ser um grupo de método, uma função anônima ou um valor de tipo de tempo de compilação `dynamic` ou um *delegate_type*.</span><span class="sxs-lookup"><span data-stu-id="70e32-1344">The argument of a delegate creation expression must be a method group, an anonymous function or a value of either the compile time type `dynamic` or a *delegate_type*.</span></span> <span data-ttu-id="70e32-1345">Se o argumento for um grupo de métodos, ele identifica o método e, para um método de instância, o objeto para o qual criar um delegado.</span><span class="sxs-lookup"><span data-stu-id="70e32-1345">If the argument is a method group, it identifies the method and, for an instance method, the object for which to create a delegate.</span></span> <span data-ttu-id="70e32-1346">Se o argumento for uma função anônima diretamente define os parâmetros e o corpo do método de destino de delegado.</span><span class="sxs-lookup"><span data-stu-id="70e32-1346">If the argument is an anonymous function it directly defines the parameters and method body of the delegate target.</span></span> <span data-ttu-id="70e32-1347">Se o argumento for um valor que identifica uma instância de delegado do qual criar uma cópia.</span><span class="sxs-lookup"><span data-stu-id="70e32-1347">If the argument is a value it identifies a delegate instance of which to create a copy.</span></span>

<span data-ttu-id="70e32-1348">Se o *expressão* tem o tipo de tempo de compilação `dynamic`, o *delegate_creation_expression* associado dinamicamente ([vinculação dinâmica](expressions.md#dynamic-binding)) e as regras abaixo são aplicadas em tempo de execução usando o tipo de tempo de execução do *expressão*.</span><span class="sxs-lookup"><span data-stu-id="70e32-1348">If the *expression* has the compile-time type `dynamic`, the *delegate_creation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), and the rules below are applied at run-time using the run-time type of the *expression*.</span></span> <span data-ttu-id="70e32-1349">Caso contrário, as regras são aplicadas em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1349">Otherwise the rules are applied at compile-time.</span></span>

<span data-ttu-id="70e32-1350">O processamento de tempo de associação de um *delegate_creation_expression* do formulário `new D(E)`, onde `D` é um *delegate_type* e `E` é uma *expressão* , consiste as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="70e32-1350">The binding-time processing of a *delegate_creation_expression* of the form `new D(E)`, where `D` is a *delegate_type* and `E` is an *expression*, consists of the following steps:</span></span>

*  <span data-ttu-id="70e32-1351">Se `E` é um grupo de método, a expressão de criação de delegado é processada da mesma forma como a conversão de grupos de método ([conversões de grupo de método](conversions.md#method-group-conversions)) de `E` para `D`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1351">If `E` is a method group, the delegate creation expression is processed in the same way as a method group conversion ([Method group conversions](conversions.md#method-group-conversions)) from `E` to `D`.</span></span>
*  <span data-ttu-id="70e32-1352">Se `E` é uma função anônima, a expressão de criação de delegado é processada da mesma forma como uma conversão de função anônima ([conversões de função anônima](conversions.md#anonymous-function-conversions)) de `E` para `D`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1352">If `E` is an anonymous function, the delegate creation expression is processed in the same way as an anonymous function conversion ([Anonymous function conversions](conversions.md#anonymous-function-conversions)) from `E` to `D`.</span></span>
*  <span data-ttu-id="70e32-1353">Se `E` é um valor `E` devem ser compatíveis ([declarações de delegado](delegates.md#delegate-declarations)) com `D`, e o resultado é uma referência a um delegado recém-criado do tipo `D` que se refere à invocação do mesma Listar como `E`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1353">If `E` is a value, `E` must be compatible ([Delegate declarations](delegates.md#delegate-declarations)) with `D`, and the result is a reference to a newly created delegate of type `D` that refers to the same invocation list as `E`.</span></span> <span data-ttu-id="70e32-1354">Se `E` não é compatível com `D`, ocorre um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1354">If `E` is not compatible with `D`, a compile-time error occurs.</span></span>

<span data-ttu-id="70e32-1355">O processamento de tempo de execução de um *delegate_creation_expression* do formulário `new D(E)`, onde `D` é um *delegate_type* e `E` é uma *expressão* , consiste as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="70e32-1355">The run-time processing of a *delegate_creation_expression* of the form `new D(E)`, where `D` is a *delegate_type* and `E` is an *expression*, consists of the following steps:</span></span>

*   <span data-ttu-id="70e32-1356">Se `E` é um grupo de método, a expressão de criação de delegado é avaliada como uma conversão de grupo de método ([conversões de grupo de método](conversions.md#method-group-conversions)) de `E` para `D`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1356">If `E` is a method group, the delegate creation expression is evaluated as a method group conversion ([Method group conversions](conversions.md#method-group-conversions)) from `E` to `D`.</span></span>
*   <span data-ttu-id="70e32-1357">Se `E` é uma função anônima, a criação de delegado é avaliada como uma conversão de função anônima da `E` à `D` ([conversões de função anônima](conversions.md#anonymous-function-conversions)).</span><span class="sxs-lookup"><span data-stu-id="70e32-1357">If `E` is an anonymous function, the delegate creation is evaluated as an anonymous function conversion from `E` to `D` ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>
*   <span data-ttu-id="70e32-1358">Se `E` é um valor de uma *delegate_type*:</span><span class="sxs-lookup"><span data-stu-id="70e32-1358">If `E` is a value of a *delegate_type*:</span></span>
    * <span data-ttu-id="70e32-1359">`E` é avaliado.</span><span class="sxs-lookup"><span data-stu-id="70e32-1359">`E` is evaluated.</span></span> <span data-ttu-id="70e32-1360">Se essa avaliação causa uma exceção, nenhuma outra etapa é executada.</span><span class="sxs-lookup"><span data-stu-id="70e32-1360">If this evaluation causes an exception, no further steps are executed.</span></span>
    * <span data-ttu-id="70e32-1361">Se o valor de `E` está `null`, um `System.NullReferenceException` é lançada e nenhuma outra etapa é executada.</span><span class="sxs-lookup"><span data-stu-id="70e32-1361">If the value of `E` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="70e32-1362">Uma nova instância do tipo de delegado `D` é alocado.</span><span class="sxs-lookup"><span data-stu-id="70e32-1362">A new instance of the delegate type `D` is allocated.</span></span> <span data-ttu-id="70e32-1363">Se não houver memória suficiente disponível para alocar a nova instância, um `System.OutOfMemoryException` é lançada e nenhuma outra etapa é executada.</span><span class="sxs-lookup"><span data-stu-id="70e32-1363">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="70e32-1364">A nova instância de delegado é inicializada com a mesma lista de invocação que a instância do representante fornecida pelo `E`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1364">The new delegate instance is initialized with the same invocation list as the delegate instance given by `E`.</span></span>

<span data-ttu-id="70e32-1365">A lista de invocação de um delegado é determinada quando o delegado é instanciado e, em seguida, é uma constante para o tempo de vida inteiro do delegado.</span><span class="sxs-lookup"><span data-stu-id="70e32-1365">The invocation list of a delegate is determined when the delegate is instantiated and then remains constant for the entire lifetime of the delegate.</span></span> <span data-ttu-id="70e32-1366">Em outras palavras, não é possível alterar as entidades de destino que pode ser chamado de um delegado após ele ter sido criado.</span><span class="sxs-lookup"><span data-stu-id="70e32-1366">In other words, it is not possible to change the target callable entities of a delegate once it has been created.</span></span> <span data-ttu-id="70e32-1367">Quando dois delegados são combinados ou um for removido de outra ([declarações de delegado](delegates.md#delegate-declarations)), faz com que um novo delegado; nenhum delegado existente tem seu conteúdo alterado.</span><span class="sxs-lookup"><span data-stu-id="70e32-1367">When two delegates are combined or one is removed from another ([Delegate declarations](delegates.md#delegate-declarations)), a new delegate results; no existing delegate has its contents changed.</span></span>

<span data-ttu-id="70e32-1368">Não é possível criar um delegado que faz referência a uma propriedade, indexador, operador definido pelo usuário, construtor de instância, destruidor ou construtor estático.</span><span class="sxs-lookup"><span data-stu-id="70e32-1368">It is not possible to create a delegate that refers to a property, indexer, user-defined operator, instance constructor, destructor, or static constructor.</span></span>

<span data-ttu-id="70e32-1369">Conforme descrito acima, quando um delegado é criado de um grupo de método, a lista de parâmetros formais e tipo de retorno do delegado de determinar quais métodos sobrecarregados para selecionar.</span><span class="sxs-lookup"><span data-stu-id="70e32-1369">As described above, when a delegate is created from a method group, the formal parameter list and return type of the delegate determine which of the overloaded methods to select.</span></span> <span data-ttu-id="70e32-1370">No exemplo</span><span class="sxs-lookup"><span data-stu-id="70e32-1370">In the example</span></span>
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
<span data-ttu-id="70e32-1371">o `A.f` campo é inicializado com um delegado que se refere à segunda `Square` método porque esse método corresponde exatamente a lista de parâmetros formais e o tipo de retorno de `DoubleFunc`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1371">the `A.f` field is initialized with a delegate that refers to the second `Square` method because that method exactly matches the formal parameter list and return type of `DoubleFunc`.</span></span> <span data-ttu-id="70e32-1372">Tinha o segundo `Square` método não estado presente, um erro de tempo de compilação teria ocorrido.</span><span class="sxs-lookup"><span data-stu-id="70e32-1372">Had the second `Square` method not been present, a compile-time error would have occurred.</span></span>

#### <a name="anonymous-object-creation-expressions"></a><span data-ttu-id="70e32-1373">Expressões de criação de objeto anônimo</span><span class="sxs-lookup"><span data-stu-id="70e32-1373">Anonymous object creation expressions</span></span>

<span data-ttu-id="70e32-1374">Uma *anonymous_object_creation_expression* é usado para criar um objeto de um tipo anônimo.</span><span class="sxs-lookup"><span data-stu-id="70e32-1374">An *anonymous_object_creation_expression* is used to create an object of an anonymous type.</span></span>

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

<span data-ttu-id="70e32-1375">Um inicializador de objeto anônimos declara um tipo anônimo e retorna uma instância desse tipo.</span><span class="sxs-lookup"><span data-stu-id="70e32-1375">An anonymous object initializer declares an anonymous type and returns an instance of that type.</span></span> <span data-ttu-id="70e32-1376">Um tipo anônimo é um tipo sem nome de classe que herda diretamente de `object`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1376">An anonymous type is a nameless class type that inherits directly from `object`.</span></span> <span data-ttu-id="70e32-1377">Os membros de um tipo anônimo são uma sequência de propriedades somente leitura inferidos do inicializador de objeto anônimo usado para criar uma instância do tipo.</span><span class="sxs-lookup"><span data-stu-id="70e32-1377">The members of an anonymous type are a sequence of read-only properties inferred from the anonymous object initializer used to create an instance of the type.</span></span> <span data-ttu-id="70e32-1378">Especificamente, um inicializador de objeto anônimo do formulário</span><span class="sxs-lookup"><span data-stu-id="70e32-1378">Specifically, an anonymous object initializer of the form</span></span>
```csharp
new { p1 = e1, p2 = e2, ..., pn = en }
```
<span data-ttu-id="70e32-1379">declara um tipo anônimo do formulário</span><span class="sxs-lookup"><span data-stu-id="70e32-1379">declares an anonymous type of the form</span></span>
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
<span data-ttu-id="70e32-1380">em que cada `Tx` é o tipo da expressão correspondente `ex`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1380">where each `Tx` is the type of the corresponding expression `ex`.</span></span> <span data-ttu-id="70e32-1381">A expressão usada em uma *member_declarator* deve ter um tipo.</span><span class="sxs-lookup"><span data-stu-id="70e32-1381">The expression used in a *member_declarator* must have a type.</span></span> <span data-ttu-id="70e32-1382">Portanto, é um erro de tempo de compilação para uma expressão em uma *member_declarator* ser null ou uma função anônima.</span><span class="sxs-lookup"><span data-stu-id="70e32-1382">Thus, it is a compile-time error for an expression in a *member_declarator* to be null or an anonymous function.</span></span> <span data-ttu-id="70e32-1383">Também é um erro de tempo de compilação para a expressão ter um tipo que não é seguro.</span><span class="sxs-lookup"><span data-stu-id="70e32-1383">It is also a compile-time error for the expression to have an unsafe type.</span></span>

<span data-ttu-id="70e32-1384">Os nomes de um tipo anônimo e do parâmetro a seu `Equals` método são gerados automaticamente pelo compilador e não pode ser referenciado no texto do programa.</span><span class="sxs-lookup"><span data-stu-id="70e32-1384">The names of an anonymous type and of the parameter to its `Equals` method are automatically generated by the compiler and cannot be referenced in program text.</span></span>

<span data-ttu-id="70e32-1385">Dentro do mesmo programa, dois inicializadores de objeto anônimos que especificam uma sequência de propriedades dos mesmos nomes e tipos de tempo de compilação na mesma ordem produzirá instâncias do mesmo tipo anônimo.</span><span class="sxs-lookup"><span data-stu-id="70e32-1385">Within the same program, two anonymous object initializers that specify a sequence of properties of the same names and compile-time types in the same order will produce instances of the same anonymous type.</span></span>

<span data-ttu-id="70e32-1386">No exemplo</span><span class="sxs-lookup"><span data-stu-id="70e32-1386">In the example</span></span>
```csharp
var p1 = new { Name = "Lawnmower", Price = 495.00 };
var p2 = new { Name = "Shovel", Price = 26.95 };
p1 = p2;
```
<span data-ttu-id="70e32-1387">a atribuição na última linha é permitida porque `p1` e `p2` são do mesmo tipo anônimo.</span><span class="sxs-lookup"><span data-stu-id="70e32-1387">the assignment on the last line is permitted because `p1` and `p2` are of the same anonymous type.</span></span>

<span data-ttu-id="70e32-1388">O `Equals` e `GetHashcode` métodos em tipos anônimos substituir os métodos herdados da `object`e são definidos em termos do `Equals` e `GetHashcode` das propriedades, para que duas instâncias do mesmo tipo anônimo são iguais se e apenas se todas as suas propriedades são iguais.</span><span class="sxs-lookup"><span data-stu-id="70e32-1388">The `Equals` and `GetHashcode` methods on anonymous types override the methods inherited from `object`, and are defined in terms of the `Equals` and `GetHashcode` of the properties, so that two instances of the same anonymous type are equal if and only if all their properties are equal.</span></span>

<span data-ttu-id="70e32-1389">Um Declarador de membro pode ser abreviado como um nome simples ([inferência](expressions.md#type-inference)), um acesso de membro ([verificação de resolução de sobrecarga de dinâmica de tempo de compilação](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), um acesso de base ([deacessodeBase](expressions.md#base-access)) ou um acesso de membro condicional nulo ([expressões condicionais nulos como inicializadores de projeção](expressions.md#null-conditional-expressions-as-projection-initializers)).</span><span class="sxs-lookup"><span data-stu-id="70e32-1389">A member declarator can be abbreviated to a simple name ([Type inference](expressions.md#type-inference)), a member access ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), a base access ([Base access](expressions.md#base-access)) or a null-conditional member access ([Null-conditional expressions as projection initializers](expressions.md#null-conditional-expressions-as-projection-initializers)).</span></span> <span data-ttu-id="70e32-1390">Isso é chamado de um ***inicializadores de projeção*** e é uma abreviação para uma declaração de e a atribuição a uma propriedade com o mesmo nome.</span><span class="sxs-lookup"><span data-stu-id="70e32-1390">This is called a ***projection initializer*** and is shorthand for a declaration of and assignment to a property with the same name.</span></span> <span data-ttu-id="70e32-1391">Especificamente, os declaradores de membro dos formulários</span><span class="sxs-lookup"><span data-stu-id="70e32-1391">Specifically, member declarators of the forms</span></span>
```csharp
identifier
expr.identifier
```
<span data-ttu-id="70e32-1392">são exatamente equivalentes às seguintes, respectivamente:</span><span class="sxs-lookup"><span data-stu-id="70e32-1392">are precisely equivalent to the following, respectively:</span></span>
```csharp
identifier = identifier
identifier = expr.identifier
```

<span data-ttu-id="70e32-1393">Assim, em inicializadores de projeção a *identificador* seleciona tanto o valor e o campo ou propriedade à qual o valor é atribuído.</span><span class="sxs-lookup"><span data-stu-id="70e32-1393">Thus, in a projection initializer the *identifier* selects both the value and the field or property to which the value is assigned.</span></span> <span data-ttu-id="70e32-1394">Intuitivamente, um inicializador de projeção projetos não apenas um valor, mas também o nome do valor.</span><span class="sxs-lookup"><span data-stu-id="70e32-1394">Intuitively, a projection initializer projects not just a value, but also the name of the value.</span></span>

### <a name="the-typeof-operator"></a><span data-ttu-id="70e32-1395">O operador typeof</span><span class="sxs-lookup"><span data-stu-id="70e32-1395">The typeof operator</span></span>

<span data-ttu-id="70e32-1396">O `typeof` operador é usado para obter o `System.Type` objeto para um tipo.</span><span class="sxs-lookup"><span data-stu-id="70e32-1396">The `typeof` operator is used to obtain the `System.Type` object for a type.</span></span>

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

<span data-ttu-id="70e32-1397">A primeira forma de *typeof_expression* consiste em um `typeof` seguido por um parênteses de palavra-chave *tipo*.</span><span class="sxs-lookup"><span data-stu-id="70e32-1397">The first form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized *type*.</span></span> <span data-ttu-id="70e32-1398">O resultado de uma expressão desse formulário é o `System.Type` objeto para o tipo indicado.</span><span class="sxs-lookup"><span data-stu-id="70e32-1398">The result of an expression of this form is the `System.Type` object for the indicated type.</span></span> <span data-ttu-id="70e32-1399">Há apenas um `System.Type` objeto para qualquer tipo determinado.</span><span class="sxs-lookup"><span data-stu-id="70e32-1399">There is only one `System.Type` object for any given type.</span></span> <span data-ttu-id="70e32-1400">Isso significa que, para um tipo `T`, `typeof(T) == typeof(T)` é sempre verdadeiro.</span><span class="sxs-lookup"><span data-stu-id="70e32-1400">This means that for a type `T`, `typeof(T) == typeof(T)` is always true.</span></span> <span data-ttu-id="70e32-1401">O *tipo* não pode ser `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1401">The *type* cannot be `dynamic`.</span></span>

<span data-ttu-id="70e32-1402">A segunda forma de *typeof_expression* consiste em um `typeof` seguido por um parênteses de palavra-chave *unbound_type_name*.</span><span class="sxs-lookup"><span data-stu-id="70e32-1402">The second form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized *unbound_type_name*.</span></span> <span data-ttu-id="70e32-1403">Uma *unbound_type_name* é muito semelhante a um *type_name* ([nomes de Namespace e tipo](basic-concepts.md#namespace-and-type-names)), exceto que um *unbound_type_name* contém *generic_dimension_specifier*s em que um *type_name* contém *type_argument_list*s.</span><span class="sxs-lookup"><span data-stu-id="70e32-1403">An *unbound_type_name* is very similar to a *type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) except that an *unbound_type_name* contains *generic_dimension_specifier*s where a *type_name* contains *type_argument_list*s.</span></span> <span data-ttu-id="70e32-1404">Quando o operando de um *typeof_expression* é uma sequência de tokens que satisfaça as gramáticas de ambos *unbound_type_name* e *type_name*, ou seja, quando ele não contém nem uma *generic_dimension_specifier* nem um *type_argument_list*, a sequência de tokens é considerada um *type_name*.</span><span class="sxs-lookup"><span data-stu-id="70e32-1404">When the operand of a *typeof_expression* is a sequence of tokens that satisfies the grammars of both *unbound_type_name* and *type_name*, namely when it contains neither a *generic_dimension_specifier* nor a *type_argument_list*, the sequence of tokens is considered to be a *type_name*.</span></span> <span data-ttu-id="70e32-1405">O significado de um *unbound_type_name* é determinado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="70e32-1405">The meaning of an *unbound_type_name* is determined as follows:</span></span>

*  <span data-ttu-id="70e32-1406">Converter a sequência de tokens para um *type_name* , substituindo cada *generic_dimension_specifier* com um *type_argument_list* tendo o mesmo número de vírgulas e o palavra-chave `object` à medida que cada *type_argument*.</span><span class="sxs-lookup"><span data-stu-id="70e32-1406">Convert the sequence of tokens to a *type_name* by replacing each *generic_dimension_specifier* with a *type_argument_list* having the same number of commas and the keyword `object` as each *type_argument*.</span></span>
*  <span data-ttu-id="70e32-1407">Avaliar resultante *type_name*, ignorando todas as restrições de parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="70e32-1407">Evaluate the resulting *type_name*, while ignoring all type parameter constraints.</span></span>
*  <span data-ttu-id="70e32-1408">O *unbound_type_name* resolve para o tipo genérico não associado, associado com o tipo construído resultante ([associado e não associados a tipos](types.md#bound-and-unbound-types)).</span><span class="sxs-lookup"><span data-stu-id="70e32-1408">The *unbound_type_name* resolves to the unbound generic type associated with the resulting constructed type ([Bound and unbound types](types.md#bound-and-unbound-types)).</span></span>

<span data-ttu-id="70e32-1409">O resultado do *typeof_expression* é o `System.Type` objeto resultante não associado de tipo genérico.</span><span class="sxs-lookup"><span data-stu-id="70e32-1409">The result of the *typeof_expression* is the `System.Type` object for the resulting unbound generic type.</span></span>

<span data-ttu-id="70e32-1410">O terceiro formulário da *typeof_expression* consiste em um `typeof` palavra-chave, seguido por um entre parênteses `void` palavra-chave.</span><span class="sxs-lookup"><span data-stu-id="70e32-1410">The third form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized `void` keyword.</span></span> <span data-ttu-id="70e32-1411">O resultado de uma expressão desse formulário é o `System.Type` objeto que representa a ausência de um tipo.</span><span class="sxs-lookup"><span data-stu-id="70e32-1411">The result of an expression of this form is the `System.Type` object that represents the absence of a type.</span></span> <span data-ttu-id="70e32-1412">O objeto do tipo retornado por `typeof(void)` é diferente do tipo objeto retornado para qualquer tipo.</span><span class="sxs-lookup"><span data-stu-id="70e32-1412">The type object returned by `typeof(void)` is distinct from the type object returned for any type.</span></span> <span data-ttu-id="70e32-1413">Esse objeto de tipo especial é útil em bibliotecas de classes que permitem que a reflexão para métodos no idioma, em que esses métodos deverão ter uma forma de representar o tipo de retorno de qualquer método, incluindo métodos void, com uma instância do `System.Type`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1413">This special type object is useful in class libraries that allow reflection onto methods in the language, where those methods wish to have a way to represent the return type of any method, including void methods, with an instance of `System.Type`.</span></span>

<span data-ttu-id="70e32-1414">O `typeof` operador pode ser usado em um parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="70e32-1414">The `typeof` operator can be used on a type parameter.</span></span> <span data-ttu-id="70e32-1415">O resultado é o `System.Type` objeto para o tipo de tempo de execução que estava associado ao parâmetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="70e32-1415">The result is the `System.Type` object for the run-time type that was bound to the type parameter.</span></span> <span data-ttu-id="70e32-1416">O `typeof` operador também pode ser usado em um tipo construído ou um tipo genérico não associado ([ligado e não associados a tipos de](types.md#bound-and-unbound-types)).</span><span class="sxs-lookup"><span data-stu-id="70e32-1416">The `typeof` operator can also be used on a constructed type or an unbound generic type ([Bound and unbound types](types.md#bound-and-unbound-types)).</span></span> <span data-ttu-id="70e32-1417">O `System.Type` do objeto para um tipo genérico não associado não é igual a `System.Type` objeto do tipo de instância.</span><span class="sxs-lookup"><span data-stu-id="70e32-1417">The `System.Type` object for an unbound generic type is not the same as the `System.Type` object of the instance type.</span></span> <span data-ttu-id="70e32-1418">O tipo de instância é sempre um tipo construído fechado em tempo de execução para sua `System.Type` objeto depende dos argumentos de tipo de tempo de execução em uso, enquanto o tipo genérico não associado não tiver nenhum argumento de tipo.</span><span class="sxs-lookup"><span data-stu-id="70e32-1418">The instance type is always a closed constructed type at run-time so its `System.Type` object depends on the run-time type arguments in use, while the unbound generic type has no type arguments.</span></span>

<span data-ttu-id="70e32-1419">O exemplo</span><span class="sxs-lookup"><span data-stu-id="70e32-1419">The example</span></span>
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
<span data-ttu-id="70e32-1420">produz a seguinte saída:</span><span class="sxs-lookup"><span data-stu-id="70e32-1420">produces the following output:</span></span>
```
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

<span data-ttu-id="70e32-1421">Observe que `int` e `System.Int32` são do mesmo tipo.</span><span class="sxs-lookup"><span data-stu-id="70e32-1421">Note that `int` and `System.Int32` are the same type.</span></span>

<span data-ttu-id="70e32-1422">Observe também que o resultado de `typeof(X<>)` não requer que o argumento de tipo, mas o resultado de `typeof(X<T>)` faz.</span><span class="sxs-lookup"><span data-stu-id="70e32-1422">Also note that the result of `typeof(X<>)` does not depend on the type argument but the result of `typeof(X<T>)` does.</span></span>

### <a name="the-checked-and-unchecked-operators"></a><span data-ttu-id="70e32-1423">Os operadores checked e unchecked</span><span class="sxs-lookup"><span data-stu-id="70e32-1423">The checked and unchecked operators</span></span>

<span data-ttu-id="70e32-1424">O `checked` e `unchecked` operadores são usados para controlar a ***contexto de verificação de estouro*** para conversões e operações aritméticas de tipo integral.</span><span class="sxs-lookup"><span data-stu-id="70e32-1424">The `checked` and `unchecked` operators are used to control the ***overflow checking context*** for integral-type arithmetic operations and conversions.</span></span>

```antlr
checked_expression
    : 'checked' '(' expression ')'
    ;

unchecked_expression
    : 'unchecked' '(' expression ')'
    ;
```

<span data-ttu-id="70e32-1425">O `checked` operador avalia a expressão contida em um contexto verificado e o `unchecked` operador avalia a expressão contida em um contexto desmarcado.</span><span class="sxs-lookup"><span data-stu-id="70e32-1425">The `checked` operator evaluates the contained expression in a checked context, and the `unchecked` operator evaluates the contained expression in an unchecked context.</span></span> <span data-ttu-id="70e32-1426">Um *checked_expression* ou *unchecked_expression* corresponde exatamente um *parenthesized_expression* ([asexpressõesentreparênteses](expressions.md#parenthesized-expressions)), exceto que a expressão contida é avaliada no contexto de verificação de estouro determinado.</span><span class="sxs-lookup"><span data-stu-id="70e32-1426">A *checked_expression* or *unchecked_expression* corresponds exactly to a *parenthesized_expression* ([Parenthesized expressions](expressions.md#parenthesized-expressions)), except that the contained expression is evaluated in the given overflow checking context.</span></span>

<span data-ttu-id="70e32-1427">O contexto de verificação de estouro também pode ser controlado por meio de `checked` e `unchecked` instruções ([as instruções checked e unchecked](statements.md#the-checked-and-unchecked-statements)).</span><span class="sxs-lookup"><span data-stu-id="70e32-1427">The overflow checking context can also be controlled through the `checked` and `unchecked` statements ([The checked and unchecked statements](statements.md#the-checked-and-unchecked-statements)).</span></span>

<span data-ttu-id="70e32-1428">As seguintes operações são afetadas pelo contexto estabelecido de verificação de estouro de `checked` e `unchecked` operadores e as instruções:</span><span class="sxs-lookup"><span data-stu-id="70e32-1428">The following operations are affected by the overflow checking context established by the `checked` and `unchecked` operators and statements:</span></span>

*  <span data-ttu-id="70e32-1429">Predefinido `++` e `--` operadores unários ([incremento de sufixo e operadores de decremento](expressions.md#postfix-increment-and-decrement-operators) e [incremento de prefixo e operadores de decremento](expressions.md#prefix-increment-and-decrement-operators)), quando o operando for do integral tipo.</span><span class="sxs-lookup"><span data-stu-id="70e32-1429">The predefined `++` and `--` unary operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), when the operand is of an integral type.</span></span>
*  <span data-ttu-id="70e32-1430">Predefinido `-` operador unário ([operador de subtração unário](expressions.md#unary-minus-operator)), quando o operando for do tipo integral.</span><span class="sxs-lookup"><span data-stu-id="70e32-1430">The predefined `-` unary operator ([Unary minus operator](expressions.md#unary-minus-operator)), when the operand is of an integral type.</span></span>
*  <span data-ttu-id="70e32-1431">Predefinido `+`, `-`, `*`, e `/` operadores binários ([operadores aritméticos](expressions.md#arithmetic-operators)), quando ambos os operandos forem de tipos integrais.</span><span class="sxs-lookup"><span data-stu-id="70e32-1431">The predefined `+`, `-`, `*`, and `/` binary operators ([Arithmetic operators](expressions.md#arithmetic-operators)), when both operands are of integral types.</span></span>
*  <span data-ttu-id="70e32-1432">Conversões numéricas explícitas ([conversões numéricas explícitas](conversions.md#explicit-numeric-conversions)) de um tipo integral para outro tipo integral ou de `float` ou `double` para um tipo integral.</span><span class="sxs-lookup"><span data-stu-id="70e32-1432">Explicit numeric conversions ([Explicit numeric conversions](conversions.md#explicit-numeric-conversions)) from one integral type to another integral type, or from `float` or `double` to an integral type.</span></span>

<span data-ttu-id="70e32-1433">Quando uma das operações acima produzir um resultado que é muito grande para ser representado no tipo de destino, o contexto no qual a operação é executados controles o comportamento resultante:</span><span class="sxs-lookup"><span data-stu-id="70e32-1433">When one of the above operations produce a result that is too large to represent in the destination type, the context in which the operation is performed controls the resulting behavior:</span></span>

*  <span data-ttu-id="70e32-1434">Em um `checked` contexto, se a operação for uma expressão de constante ([expressões constantes](expressions.md#constant-expressions)), ocorre um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1434">In a `checked` context, if the operation is a constant expression ([Constant expressions](expressions.md#constant-expressions)), a compile-time error occurs.</span></span> <span data-ttu-id="70e32-1435">Caso contrário, quando a operação é executada em tempo de execução, um `System.OverflowException` é gerada.</span><span class="sxs-lookup"><span data-stu-id="70e32-1435">Otherwise, when the operation is performed at run-time, a `System.OverflowException` is thrown.</span></span>
*  <span data-ttu-id="70e32-1436">Em um `unchecked` contexto, o resultado é truncado descartando quaisquer bits de ordem superior que não cabem no tipo de destino.</span><span class="sxs-lookup"><span data-stu-id="70e32-1436">In an `unchecked` context, the result is truncated by discarding any high-order bits that do not fit in the destination type.</span></span>

<span data-ttu-id="70e32-1437">Para expressões não constantes (expressões são avaliadas em tempo de execução) que não são incluídos por qualquer `checked` ou `unchecked` operadores ou mais instruções, o contexto de verificação de estouro de padrão é `unchecked` , a menos que os fatores de externo (por exemplo, o compilador comutadores e configuração do ambiente de execução) chamam para `checked` avaliação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1437">For non-constant expressions (expressions that are evaluated at run-time) that are not enclosed by any `checked` or `unchecked` operators or statements, the default overflow checking context is `unchecked` unless external factors (such as compiler switches and execution environment configuration) call for `checked` evaluation.</span></span>

<span data-ttu-id="70e32-1438">Para expressões de constante (expressões que podem ser completamente avaliadas em tempo de compilação), o contexto de verificação de estouro de padrão sempre é `checked`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1438">For constant expressions (expressions that can be fully evaluated at compile-time), the default overflow checking context is always `checked`.</span></span> <span data-ttu-id="70e32-1439">A menos que uma expressão de constante seja explicitamente colocada em um `unchecked` contexto, estouros que ocorrem durante a avaliação do tempo de compilação da expressão sempre causam erros de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1439">Unless a constant expression is explicitly placed in an `unchecked` context, overflows that occur during the compile-time evaluation of the expression always cause compile-time errors.</span></span>

<span data-ttu-id="70e32-1440">O corpo de uma função anônima não é afetado pelas `checked` ou `unchecked` contextos em que a função anônima ocorre.</span><span class="sxs-lookup"><span data-stu-id="70e32-1440">The body of an anonymous function is not affected by `checked` or `unchecked` contexts in which the anonymous function occurs.</span></span>

<span data-ttu-id="70e32-1441">No exemplo</span><span class="sxs-lookup"><span data-stu-id="70e32-1441">In the example</span></span>
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
<span data-ttu-id="70e32-1442">Nenhum erro de tempo de compilação é relatado, pois nenhuma das expressões pode ser avaliada em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1442">no compile-time errors are reported since neither of the expressions can be evaluated at compile-time.</span></span> <span data-ttu-id="70e32-1443">Em tempo de execução, o `F` método lança um `System.OverflowException`e o `G` método retorna-727379968 (os 32 bits inferiores do resultado fora do intervalo).</span><span class="sxs-lookup"><span data-stu-id="70e32-1443">At run-time, the `F` method throws a `System.OverflowException`, and the `G` method returns -727379968 (the lower 32 bits of the out-of-range result).</span></span> <span data-ttu-id="70e32-1444">O comportamento do `H` método depende do contexto para a compilação de verificação de estouro de padrão, mas é o mesmo que `F` ou o mesmo que `G`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1444">The behavior of the `H` method depends on the default overflow checking context for the compilation, but it is either the same as `F` or the same as `G`.</span></span>

<span data-ttu-id="70e32-1445">No exemplo</span><span class="sxs-lookup"><span data-stu-id="70e32-1445">In the example</span></span>
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
<span data-ttu-id="70e32-1446">os estouros ocorridos durante a avaliação das expressões constantes na `F` e `H` causar erros de tempo de compilação a ser relatado porque as expressões são avaliadas em um `checked` contexto.</span><span class="sxs-lookup"><span data-stu-id="70e32-1446">the overflows that occur when evaluating the constant expressions in `F` and `H` cause compile-time errors to be reported because the expressions are evaluated in a `checked` context.</span></span> <span data-ttu-id="70e32-1447">Um estouro também ocorre ao avaliar a expressão de constante `G`, mas já que a avaliação é feita em um `unchecked` contexto, o estouro não será relatado.</span><span class="sxs-lookup"><span data-stu-id="70e32-1447">An overflow also occurs when evaluating the constant expression in `G`, but since the evaluation takes place in an `unchecked` context, the overflow is not reported.</span></span>

<span data-ttu-id="70e32-1448">O `checked` e `unchecked` operadores afetam apenas o contexto para as operações que estão contidos textualmente de verificação de estouro de "`(`"e"`)`" tokens.</span><span class="sxs-lookup"><span data-stu-id="70e32-1448">The `checked` and `unchecked` operators only affect the overflow checking context for those operations that are textually contained within the "`(`" and "`)`" tokens.</span></span> <span data-ttu-id="70e32-1449">Os operadores não têm efeito nos membros da função que são invocados como resultado da avaliação da expressão independente.</span><span class="sxs-lookup"><span data-stu-id="70e32-1449">The operators have no effect on function members that are invoked as a result of evaluating the contained expression.</span></span> <span data-ttu-id="70e32-1450">No exemplo</span><span class="sxs-lookup"><span data-stu-id="70e32-1450">In the example</span></span>
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
<span data-ttu-id="70e32-1451">o uso de `checked` na `F` não afeta a avaliação de `x * y` na `Multiply`, então `x * y` é avaliada no contexto de verificação de estouro de padrão.</span><span class="sxs-lookup"><span data-stu-id="70e32-1451">the use of `checked` in `F` does not affect the evaluation of `x * y` in `Multiply`, so `x * y` is evaluated in the default overflow checking context.</span></span>

<span data-ttu-id="70e32-1452">O `unchecked` operador é conveniente ao escrever constantes dos tipos integrais com sinal em notação hexadecimal.</span><span class="sxs-lookup"><span data-stu-id="70e32-1452">The `unchecked` operator is convenient when writing constants of the signed integral types in hexadecimal notation.</span></span> <span data-ttu-id="70e32-1453">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="70e32-1453">For example:</span></span>
```csharp
class Test
{
    public const int AllBits = unchecked((int)0xFFFFFFFF);

    public const int HighBit = unchecked((int)0x80000000);
}
```

<span data-ttu-id="70e32-1454">Ambas as constantes hexadecimais acima são do tipo `uint`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1454">Both of the hexadecimal constants above are of type `uint`.</span></span> <span data-ttu-id="70e32-1455">Como as constantes são fora o `int` de intervalo, sem a `unchecked` operador, as conversões para `int` geraria erros de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1455">Because the constants are outside the `int` range, without the `unchecked` operator, the casts to `int` would produce compile-time errors.</span></span>

<span data-ttu-id="70e32-1456">O `checked` e `unchecked` operadores e as instruções permitem que os programadores controlar determinados aspectos de alguns cálculos numéricos.</span><span class="sxs-lookup"><span data-stu-id="70e32-1456">The `checked` and `unchecked` operators and statements allow programmers to control certain aspects of some numeric calculations.</span></span> <span data-ttu-id="70e32-1457">No entanto, o comportamento de alguns operadores numéricos depende de tipos de dados de seus operandos.</span><span class="sxs-lookup"><span data-stu-id="70e32-1457">However, the behavior of some numeric operators depends on their operands' data types.</span></span> <span data-ttu-id="70e32-1458">Por exemplo, a multiplicação de duas casas decimais sempre resulta em uma exceção no estouro, mesmo dentro um explicitamente `unchecked` construir.</span><span class="sxs-lookup"><span data-stu-id="70e32-1458">For example, multiplying two decimals always results in an exception on overflow even within an explicitly `unchecked` construct.</span></span> <span data-ttu-id="70e32-1459">Da mesma forma, a multiplicação de dois floats nunca resultados em uma exceção no estouro até mesmo dentro um explicitamente `checked` construir.</span><span class="sxs-lookup"><span data-stu-id="70e32-1459">Similarly, multiplying two floats never results in an exception on overflow even within an explicitly `checked` construct.</span></span> <span data-ttu-id="70e32-1460">Além disso, outros operadores nunca são afetados pelo modo de verificação, se padrão ou explícita.</span><span class="sxs-lookup"><span data-stu-id="70e32-1460">In addition, other operators are never affected by the mode of checking, whether default or explicit.</span></span>

### <a name="default-value-expressions"></a><span data-ttu-id="70e32-1461">Expressões de valor padrão</span><span class="sxs-lookup"><span data-stu-id="70e32-1461">Default value expressions</span></span>

<span data-ttu-id="70e32-1462">Uma expressão de valor padrão é usada para obter o valor padrão ([valores padrão](variables.md#default-values)) de um tipo.</span><span class="sxs-lookup"><span data-stu-id="70e32-1462">A default value expression is used to obtain the default value ([Default values](variables.md#default-values)) of a type.</span></span> <span data-ttu-id="70e32-1463">Normalmente, uma expressão de valor padrão é usada para parâmetros de tipo, desde que ele pode não ser conhecido se o parâmetro de tipo é um tipo de valor ou um tipo de referência.</span><span class="sxs-lookup"><span data-stu-id="70e32-1463">Typically a default value expression is used for type parameters, since it may not be known if the type parameter is a value type or a reference type.</span></span> <span data-ttu-id="70e32-1464">(Não existe conversão do `null` literal a um parâmetro de tipo, a menos que o parâmetro de tipo é conhecido por ser um tipo de referência.)</span><span class="sxs-lookup"><span data-stu-id="70e32-1464">(No conversion exists from the `null` literal to a type parameter unless the type parameter is known to be a reference type.)</span></span>

```antlr
default_value_expression
    : 'default' '(' type ')'
    ;
```

<span data-ttu-id="70e32-1465">Se o *tipo* em um *default_value_expression* é avaliada em tempo de execução para um tipo de referência, o resultado é `null` convertido nesse tipo.</span><span class="sxs-lookup"><span data-stu-id="70e32-1465">If the *type* in a *default_value_expression* evaluates at run-time to a reference type, the result is `null` converted to that type.</span></span> <span data-ttu-id="70e32-1466">Se o *tipo* em um *default_value_expression* é avaliada em tempo de execução para um tipo de valor, o resultado é o *value_type*do valor padrão ([padrão construtores](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="70e32-1466">If the *type* in a *default_value_expression* evaluates at run-time to a value type, the result is the *value_type*'s default value ([Default constructors](types.md#default-constructors)).</span></span>

<span data-ttu-id="70e32-1467">Um *default_value_expression* é uma expressão constante ([expressões constantes](expressions.md#constant-expressions)) se o tipo é um tipo de referência ou um parâmetro de tipo é conhecido por ser um tipo de referência ([parâmetro de tipo restrições de](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="70e32-1467">A *default_value_expression* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) if the type is a reference type or a type parameter that is known to be a reference type ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="70e32-1468">Além disso, uma *default_value_expression* é uma expressão constante, se o tipo é um dos seguintes tipos de valor: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, ou qualquer tipo de enumeração.</span><span class="sxs-lookup"><span data-stu-id="70e32-1468">In addition, a *default_value_expression* is a constant expression if the type is one of the following value types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, or any enumeration type.</span></span>


### <a name="nameof-expressions"></a><span data-ttu-id="70e32-1469">Expressões Nameof</span><span class="sxs-lookup"><span data-stu-id="70e32-1469">Nameof expressions</span></span>

<span data-ttu-id="70e32-1470">Um *nameof_expression* é usado para obter o nome de uma entidade programa como uma cadeia de caracteres constante.</span><span class="sxs-lookup"><span data-stu-id="70e32-1470">A *nameof_expression* is used to obtain the name of a program entity as a constant string.</span></span>

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

<span data-ttu-id="70e32-1471">Gramaticalmente falando, o *named_entity* operando sempre é uma expressão.</span><span class="sxs-lookup"><span data-stu-id="70e32-1471">Grammatically speaking, the *named_entity* operand is always an expression.</span></span> <span data-ttu-id="70e32-1472">Porque `nameof` não é uma palavra-chave reservada, a expressão nameof sempre é sintaticamente ambígua com uma invocação do nome simple `nameof`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1472">Because `nameof` is not a reserved keyword, a nameof expression is always syntactically ambiguous with an invocation of the simple name `nameof`.</span></span> <span data-ttu-id="70e32-1473">Por razões de compatibilidade, se uma pesquisa de nome ([nomes simples](expressions.md#simple-names)) de nome `nameof` for bem-sucedida, a expressão é tratada como um *invocation_expression* – independentemente de se a invocação é legal.</span><span class="sxs-lookup"><span data-stu-id="70e32-1473">For compatibility reasons, if a name lookup ([Simple names](expressions.md#simple-names)) of the name `nameof` succeeds, the expression is treated as an *invocation_expression* -- regardless of whether the invocation is legal.</span></span> <span data-ttu-id="70e32-1474">Caso contrário, ele é um *nameof_expression*.</span><span class="sxs-lookup"><span data-stu-id="70e32-1474">Otherwise it is a *nameof_expression*.</span></span>

<span data-ttu-id="70e32-1475">O significado do *named_entity* de uma *nameof_expression* é o significado de-lo como uma expressão; ou seja, tanto como um *simple_name*, um *base_access*  ou um *member_access*.</span><span class="sxs-lookup"><span data-stu-id="70e32-1475">The meaning of the *named_entity* of a *nameof_expression* is the meaning of it as an expression; that is, either as a *simple_name*, a *base_access* or a *member_access*.</span></span> <span data-ttu-id="70e32-1476">No entanto, em que a pesquisa descrito em [nomes simples](expressions.md#simple-names) e [acesso de membro](expressions.md#member-access) resulta em um erro porque um membro de instância foi encontrado em um contexto estático, uma *nameof_expression*não produz nenhum esse erro.</span><span class="sxs-lookup"><span data-stu-id="70e32-1476">However, where the lookup described in [Simple names](expressions.md#simple-names) and [Member access](expressions.md#member-access) results in an error because an instance member was found in a static context, a *nameof_expression* produces no such error.</span></span>

<span data-ttu-id="70e32-1477">É um erro de tempo de compilação para um *named_entity* designar um grupo de métodos para ter um *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="70e32-1477">It is a compile-time error for a *named_entity* designating a method group to have a *type_argument_list*.</span></span> <span data-ttu-id="70e32-1478">É um erro de tempo de compilação para um *named_entity_target* para ter o tipo `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1478">It is a compile time error for a *named_entity_target* to have the type `dynamic`.</span></span>

<span data-ttu-id="70e32-1479">Um *nameof_expression* é uma expressão constante do tipo `string`, e não tem nenhum efeito em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="70e32-1479">A *nameof_expression* is a constant expression of type `string`, and has no effect at runtime.</span></span> <span data-ttu-id="70e32-1480">Especificamente, sua *named_entity* não será avaliada e é ignorada para fins de análise de atribuição definitiva ([regras gerais para expressões simples](variables.md#general-rules-for-simple-expressions)).</span><span class="sxs-lookup"><span data-stu-id="70e32-1480">Specifically, its *named_entity* is not evaluated, and is ignored for the purposes of definite assignment analysis ([General rules for simple expressions](variables.md#general-rules-for-simple-expressions)).</span></span> <span data-ttu-id="70e32-1481">Seu valor é o último identificador do *named_entity* antes do final opcional *type_argument_list*, transformada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="70e32-1481">Its value is the last identifier of the *named_entity* before the optional final *type_argument_list*, transformed in the following way:</span></span>

* <span data-ttu-id="70e32-1482">O prefixo "`@`", se usado, é removido.</span><span class="sxs-lookup"><span data-stu-id="70e32-1482">The prefix "`@`", if used, is removed.</span></span>
* <span data-ttu-id="70e32-1483">Cada *unicode_escape_sequence* é transformado em seu caractere Unicode correspondente.</span><span class="sxs-lookup"><span data-stu-id="70e32-1483">Each *unicode_escape_sequence* is transformed into its corresponding Unicode character.</span></span>
* <span data-ttu-id="70e32-1484">Qualquer *formatting_characters* são removidos.</span><span class="sxs-lookup"><span data-stu-id="70e32-1484">Any *formatting_characters* are removed.</span></span>

<span data-ttu-id="70e32-1485">Essas são as mesmas transformações aplicadas na [identificadores](lexical-structure.md#identifiers) ao testar a igualdade entre identificadores.</span><span class="sxs-lookup"><span data-stu-id="70e32-1485">These are the same transformations applied in [Identifiers](lexical-structure.md#identifiers) when testing equality between identifiers.</span></span>

<span data-ttu-id="70e32-1486">TODO: exemplos</span><span class="sxs-lookup"><span data-stu-id="70e32-1486">TODO: examples</span></span>

### <a name="anonymous-method-expressions"></a><span data-ttu-id="70e32-1487">Expressões de método anônimo</span><span class="sxs-lookup"><span data-stu-id="70e32-1487">Anonymous method expressions</span></span>

<span data-ttu-id="70e32-1488">Uma *anonymous_method_expression* é uma das duas maneiras de definir uma função anônima.</span><span class="sxs-lookup"><span data-stu-id="70e32-1488">An *anonymous_method_expression* is one of two ways of defining an anonymous function.</span></span> <span data-ttu-id="70e32-1489">Além disso, elas são descritas em [expressões de função anônima](expressions.md#anonymous-function-expressions).</span><span class="sxs-lookup"><span data-stu-id="70e32-1489">These are further described in [Anonymous function expressions](expressions.md#anonymous-function-expressions).</span></span>

## <a name="unary-operators"></a><span data-ttu-id="70e32-1490">Operadores unários</span><span class="sxs-lookup"><span data-stu-id="70e32-1490">Unary operators</span></span>

<span data-ttu-id="70e32-1491">O `?`, `+`, `-`, `!`, `~`, `++`, `--`, converta, e `await` os operadores unários são chamados de operadores.</span><span class="sxs-lookup"><span data-stu-id="70e32-1491">The `?`, `+`, `-`, `!`, `~`, `++`, `--`, cast, and `await` operators are called the unary operators.</span></span>

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

<span data-ttu-id="70e32-1492">Se o operando de um *unary_expression* tem o tipo de tempo de compilação `dynamic`, ele está vinculado dinamicamente ([vinculação dinâmica](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="70e32-1492">If the operand of a *unary_expression* has the compile-time type `dynamic`, it is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="70e32-1493">Nesse caso, o tempo de compilação de tipo dos *unary_expression* é `dynamic`, e a resolução descrita a seguir ocorrerá em tempo de execução usando o tipo de tempo de execução do operando.</span><span class="sxs-lookup"><span data-stu-id="70e32-1493">In this case the compile-time type of the *unary_expression* is `dynamic`, and the resolution described below will take place at run-time using the run-time type of the operand.</span></span>

### <a name="null-conditional-operator"></a><span data-ttu-id="70e32-1494">Operador nulo condicional</span><span class="sxs-lookup"><span data-stu-id="70e32-1494">Null-conditional operator</span></span>

<span data-ttu-id="70e32-1495">O operador nulo condicional se aplica uma lista das operações ao operando somente se o operando não for nulo.</span><span class="sxs-lookup"><span data-stu-id="70e32-1495">The null-conditional operator applies a list of operations to its operand only if that operand is non-null.</span></span> <span data-ttu-id="70e32-1496">Caso contrário, o resultado da aplicação do operador é `null`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1496">Otherwise the result of applying the operator is `null`.</span></span>

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

<span data-ttu-id="70e32-1497">A lista de operações pode incluir acesso de membro e operações de acesso de elemento (que podem estar nulo-condicional), bem como invocação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1497">The list of operations can include member access and element access operations (which may themselves be null-conditional), as well as invocation.</span></span>

<span data-ttu-id="70e32-1498">Por exemplo, a expressão `a.b?[0]?.c()` é um *null_conditional_expression* com um *primary_expression* `a.b` e *null_conditional_operations* `?[0]` (acesso de elemento nulo-condicional), `?.c` (acesso de membro nulo condicional) e `()` (invocação).</span><span class="sxs-lookup"><span data-stu-id="70e32-1498">For example, the expression `a.b?[0]?.c()` is a *null_conditional_expression* with a *primary_expression* `a.b` and *null_conditional_operations* `?[0]` (null-conditional element access), `?.c` (null-conditional member access) and `()` (invocation).</span></span>

<span data-ttu-id="70e32-1499">Para um *null_conditional_expression* `E` com um *primary_expression* `P`, permitem que `E0` ser a expressão obtida removendo textualmente o entrelinhamento `?`de cada um dos *null_conditional_operations* de `E` que tiver uma.</span><span class="sxs-lookup"><span data-stu-id="70e32-1499">For a *null_conditional_expression* `E` with a *primary_expression* `P`, let `E0` be the expression obtained by textually removing the leading `?` from each of the *null_conditional_operations* of `E` that have one.</span></span> <span data-ttu-id="70e32-1500">Conceitualmente, `E0` é a expressão que será avaliada se nenhum das verificações nulas representado pela `?`s encontrar um `null`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1500">Conceptually, `E0` is the expression that will be evaluated if none of the null checks represented by the `?`s do find a `null`.</span></span>

<span data-ttu-id="70e32-1501">Além disso, permitir que `E1` ser a expressão obtida removendo textualmente o entrelinhamento `?` de apenas o primeiro a *null_conditional_operations* em `E`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1501">Also, let `E1` be the expression obtained by textually removing the leading `?` from just the first of the *null_conditional_operations* in `E`.</span></span> <span data-ttu-id="70e32-1502">Isso pode levar a um *primary-expression* (se houver apenas um `?`) ou para outro *null_conditional_expression*.</span><span class="sxs-lookup"><span data-stu-id="70e32-1502">This may lead to a *primary-expression* (if there was just one `?`) or to another *null_conditional_expression*.</span></span>

<span data-ttu-id="70e32-1503">Por exemplo, se `E` é a expressão `a.b?[0]?.c()`, em seguida, `E0` é a expressão `a.b[0].c()` e `E1` é a expressão `a.b[0]?.c()`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1503">For example, if `E` is the expression `a.b?[0]?.c()`, then `E0` is the expression `a.b[0].c()` and `E1` is the expression `a.b[0]?.c()`.</span></span>

<span data-ttu-id="70e32-1504">Se `E0` é classificado como nothing, em seguida, `E` é classificado como nada.</span><span class="sxs-lookup"><span data-stu-id="70e32-1504">If `E0` is classified as nothing, then `E` is classified as nothing.</span></span> <span data-ttu-id="70e32-1505">Caso contrário, E é classificado como um valor.</span><span class="sxs-lookup"><span data-stu-id="70e32-1505">Otherwise E is classified as a value.</span></span>

<span data-ttu-id="70e32-1506">`E0` e `E1` são usados para determinar o significado de `E`:</span><span class="sxs-lookup"><span data-stu-id="70e32-1506">`E0` and `E1` are used to determine the meaning of `E`:</span></span>

*  <span data-ttu-id="70e32-1507">Se `E` ocorre como uma *statement_expression* o significado de `E` é o mesmo que a instrução</span><span class="sxs-lookup"><span data-stu-id="70e32-1507">If `E` occurs as a *statement_expression* the meaning of `E` is the same as the statement</span></span>

   ```csharp
   if ((object)P != null) E1;
   ```

   <span data-ttu-id="70e32-1508">exceto que P é avaliada apenas uma vez.</span><span class="sxs-lookup"><span data-stu-id="70e32-1508">except that P is evaluated only once.</span></span>

*  <span data-ttu-id="70e32-1509">Caso contrário, se `E0` é classificado como nada ocorre um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1509">Otherwise, if `E0` is classified as nothing a compile-time error occurs.</span></span>

*  <span data-ttu-id="70e32-1510">Caso contrário, deixe `T0` ser o tipo de `E0`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1510">Otherwise, let `T0` be the type of `E0`.</span></span>

   *  <span data-ttu-id="70e32-1511">Se `T0` é um parâmetro de tipo que não sejam conhecido por ser um tipo de referência ou um tipo de valor não anulável, ocorre um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1511">If `T0` is a type parameter that is not known to be a reference type or a non-nullable value type, a compile-time error occurs.</span></span>

   *  <span data-ttu-id="70e32-1512">Se `T0` é um tipo de valor não nulo e, em seguida, o tipo de `E` é `T0?`e o significado de `E` é o mesmo que</span><span class="sxs-lookup"><span data-stu-id="70e32-1512">If `T0` is a non-nullable value type, then the type of `E` is `T0?`, and the meaning of `E` is the same as</span></span>

      ```csharp
      ((object)P == null) ? (T0?)null : E1
      ```

      <span data-ttu-id="70e32-1513">exceto pelo fato de `P` é avaliada apenas uma vez.</span><span class="sxs-lookup"><span data-stu-id="70e32-1513">except that `P` is evaluated only once.</span></span>

   *  <span data-ttu-id="70e32-1514">Caso contrário, o tipo de E é T0 e o significado E é o mesmo que</span><span class="sxs-lookup"><span data-stu-id="70e32-1514">Otherwise the type of E is T0, and the meaning of E is the same as</span></span>

      ```csharp
      ((object)P == null) ? null : E1
      ```

      <span data-ttu-id="70e32-1515">exceto pelo fato de `P` é avaliada apenas uma vez.</span><span class="sxs-lookup"><span data-stu-id="70e32-1515">except that `P` is evaluated only once.</span></span>

<span data-ttu-id="70e32-1516">Se `E1` em si um *null_conditional_expression*, em seguida, essas regras são aplicadas novamente, os testes para de aninhamento `null` até que não haja nenhuma outra `?`do, e a expressão foi reduzida todo o caminho para baixo a expressão de primário `E0`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1516">If `E1` is itself a *null_conditional_expression*, then these rules are applied again, nesting the tests for `null` until there are no further `?`'s, and the expression has been reduced all the way down to the primary-expression `E0`.</span></span>

<span data-ttu-id="70e32-1517">Por exemplo, se a expressão `a.b?[0]?.c()` ocorre como uma instrução de expressão, como a instrução:</span><span class="sxs-lookup"><span data-stu-id="70e32-1517">For example, if the expression `a.b?[0]?.c()` occurs as a statement-expression, as in the statement:</span></span>
```csharp
a.b?[0]?.c();
```
<span data-ttu-id="70e32-1518">seu significado é equivalente a:</span><span class="sxs-lookup"><span data-stu-id="70e32-1518">its meaning is equivalent to:</span></span>
```csharp
if (a.b != null) a.b[0]?.c();
```
<span data-ttu-id="70e32-1519">que novamente é equivalente a:</span><span class="sxs-lookup"><span data-stu-id="70e32-1519">which again is equivalent to:</span></span>
```csharp
if (a.b != null) if (a.b[0] != null) a.b[0].c();
```
<span data-ttu-id="70e32-1520">Exceto que `a.b` e `a.b[0]` são avaliadas apenas uma vez.</span><span class="sxs-lookup"><span data-stu-id="70e32-1520">Except that `a.b` and `a.b[0]` are evaluated only once.</span></span>

<span data-ttu-id="70e32-1521">Se ele ocorrer em um contexto em que seu valor é usado, como em:</span><span class="sxs-lookup"><span data-stu-id="70e32-1521">If it occurs in a context where its value is used, as in:</span></span>
```csharp
var x = a.b?[0]?.c();
```
<span data-ttu-id="70e32-1522">e supondo que o tipo da invocação do final não é um tipo de valor não anulável, seu significado é equivalente a:</span><span class="sxs-lookup"><span data-stu-id="70e32-1522">and assuming that the type of the final invocation is not a non-nullable value type, its meaning is equivalent to:</span></span>
```csharp
var x = (a.b == null) ? null : (a.b[0] == null) ? null : a.b[0].c();
```
<span data-ttu-id="70e32-1523">Exceto que `a.b` e `a.b[0]` são avaliadas apenas uma vez.</span><span class="sxs-lookup"><span data-stu-id="70e32-1523">except that `a.b` and `a.b[0]` are evaluated only once.</span></span>

#### <a name="null-conditional-expressions-as-projection-initializers"></a><span data-ttu-id="70e32-1524">Expressões de nulo-condicional como inicializadores de projeção</span><span class="sxs-lookup"><span data-stu-id="70e32-1524">Null-conditional expressions as projection initializers</span></span>

<span data-ttu-id="70e32-1525">Uma expressão condicional nulo é permitida somente como uma *member_declarator* em um *anonymous_object_creation_expression* ([expressões de criação de objeto anônimo](expressions.md#anonymous-object-creation-expressions)) se ele termina com um acesso de membro (opcionalmente nulo-condicional).</span><span class="sxs-lookup"><span data-stu-id="70e32-1525">A null-conditional expression is only allowed as a *member_declarator* in an *anonymous_object_creation_expression* ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) if it ends with an (optionally null-conditional) member access.</span></span> <span data-ttu-id="70e32-1526">Gramaticalmente, esse requisito pode ser expresso como:</span><span class="sxs-lookup"><span data-stu-id="70e32-1526">Grammatically, this requirement can be expressed as:</span></span>

```antlr
null_conditional_member_access
    : primary_expression null_conditional_operations? '?' '.' identifier type_argument_list?
    | primary_expression null_conditional_operations '.' identifier type_argument_list?
    ;
```

<span data-ttu-id="70e32-1527">Esse é um caso especial da gramática para *null_conditional_expression* acima.</span><span class="sxs-lookup"><span data-stu-id="70e32-1527">This is a special case of the grammar for *null_conditional_expression* above.</span></span> <span data-ttu-id="70e32-1528">Produção *member_declarator* na [expressões de criação de objeto anônimo](expressions.md#anonymous-object-creation-expressions) , em seguida, inclua apenas *null_conditional_member_access*.</span><span class="sxs-lookup"><span data-stu-id="70e32-1528">The production for *member_declarator* in [Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions) then includes only *null_conditional_member_access*.</span></span>

#### <a name="null-conditional-expressions-as-statement-expressions"></a><span data-ttu-id="70e32-1529">Expressões de nulo-condicional como expressões de instrução</span><span class="sxs-lookup"><span data-stu-id="70e32-1529">Null-conditional expressions as statement expressions</span></span>

<span data-ttu-id="70e32-1530">Uma expressão condicional nulo é permitida somente como uma *statement_expression* ([instruções de expressão](statements.md#expression-statements)) se ele termina com uma invocação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1530">A null-conditional expression is only allowed as a *statement_expression* ([Expression statements](statements.md#expression-statements)) if it ends with an invocation.</span></span> <span data-ttu-id="70e32-1531">Gramaticalmente, esse requisito pode ser expresso como:</span><span class="sxs-lookup"><span data-stu-id="70e32-1531">Grammatically, this requirement can be expressed as:</span></span>

```antlr
null_conditional_invocation_expression
    : primary_expression null_conditional_operations '(' argument_list? ')'
    ;
```

<span data-ttu-id="70e32-1532">Esse é um caso especial da gramática para *null_conditional_expression* acima.</span><span class="sxs-lookup"><span data-stu-id="70e32-1532">This is a special case of the grammar for *null_conditional_expression* above.</span></span> <span data-ttu-id="70e32-1533">Produção *statement_expression* na [instruções de expressão](statements.md#expression-statements) , em seguida, inclua apenas *null_conditional_invocation_expression*.</span><span class="sxs-lookup"><span data-stu-id="70e32-1533">The production for *statement_expression* in [Expression statements](statements.md#expression-statements) then includes only *null_conditional_invocation_expression*.</span></span>


### <a name="unary-plus-operator"></a><span data-ttu-id="70e32-1534">Operador unário de adição</span><span class="sxs-lookup"><span data-stu-id="70e32-1534">Unary plus operator</span></span>

<span data-ttu-id="70e32-1535">Para uma operação do formulário `+x`, resolução de sobrecarga de operador unário ([resolução de sobrecarga de operador unário](expressions.md#unary-operator-overload-resolution)) é aplicado para selecionar uma implementação do operador específico.</span><span class="sxs-lookup"><span data-stu-id="70e32-1535">For an operation of the form `+x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="70e32-1536">O operando será convertido ao tipo de parâmetro do operador selecionado e o tipo do resultado é o tipo de retorno do operador.</span><span class="sxs-lookup"><span data-stu-id="70e32-1536">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="70e32-1537">Os operadores de mais unária predefinida são:</span><span class="sxs-lookup"><span data-stu-id="70e32-1537">The predefined unary plus operators are:</span></span>

```csharp
int operator +(int x);
uint operator +(uint x);
long operator +(long x);
ulong operator +(ulong x);
float operator +(float x);
double operator +(double x);
decimal operator +(decimal x);
```

<span data-ttu-id="70e32-1538">Para cada um desses operadores, o resultado é simplesmente o valor do operando.</span><span class="sxs-lookup"><span data-stu-id="70e32-1538">For each of these operators, the result is simply the value of the operand.</span></span>

### <a name="unary-minus-operator"></a><span data-ttu-id="70e32-1539">Operador de subtração unário</span><span class="sxs-lookup"><span data-stu-id="70e32-1539">Unary minus operator</span></span>

<span data-ttu-id="70e32-1540">Para uma operação do formulário `-x`, resolução de sobrecarga de operador unário ([resolução de sobrecarga de operador unário](expressions.md#unary-operator-overload-resolution)) é aplicado para selecionar uma implementação do operador específico.</span><span class="sxs-lookup"><span data-stu-id="70e32-1540">For an operation of the form `-x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="70e32-1541">O operando será convertido ao tipo de parâmetro do operador selecionado e o tipo do resultado é o tipo de retorno do operador.</span><span class="sxs-lookup"><span data-stu-id="70e32-1541">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="70e32-1542">Os operadores de negação predefinidos são:</span><span class="sxs-lookup"><span data-stu-id="70e32-1542">The predefined negation operators are:</span></span>

*  <span data-ttu-id="70e32-1543">Negação de inteiro:</span><span class="sxs-lookup"><span data-stu-id="70e32-1543">Integer negation:</span></span>

   ```csharp
   int operator -(int x);
   long operator -(long x);
   ```

   <span data-ttu-id="70e32-1544">O resultado é calculado subtraindo `x` de zero.</span><span class="sxs-lookup"><span data-stu-id="70e32-1544">The result is computed by subtracting `x` from zero.</span></span> <span data-ttu-id="70e32-1545">Se o valor da `x` é o menor valor representável do tipo de operando (-2 ^ 31 para `int` ou -2 ^ 63 para `long`), em seguida, a negação matemática do `x` não for representável no tipo de operando.</span><span class="sxs-lookup"><span data-stu-id="70e32-1545">If the value of of `x` is the smallest representable value of the operand type (-2^31 for `int` or -2^63 for `long`), then the mathematical negation of `x` is not representable within the operand type.</span></span> <span data-ttu-id="70e32-1546">Se isso ocorrer dentro de um `checked` contexto, uma `System.OverflowException` gerada; se ele ocorrer dentro de um `unchecked` contexto, o resultado é o valor do operando, e o estouro não será relatado.</span><span class="sxs-lookup"><span data-stu-id="70e32-1546">If this occurs within a `checked` context, a `System.OverflowException` is thrown; if it occurs within an `unchecked` context, the result is the value of the operand and the overflow is not reported.</span></span>

   <span data-ttu-id="70e32-1547">Se o operando do operador de negação é do tipo `uint`, ele será convertido ao tipo `long`, e o tipo do resultado é `long`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1547">If the operand of the negation operator is of type `uint`, it is converted to type `long`, and the type of the result is `long`.</span></span> <span data-ttu-id="70e32-1548">Uma exceção é a regra que permite que o `int` valor de -2147483648 (-2 ^ 31) a ser gravado como um literal de inteiro decimal ([literais inteiros](lexical-structure.md#integer-literals)).</span><span class="sxs-lookup"><span data-stu-id="70e32-1548">An exception is the rule that permits the `int` value -2147483648 (-2^31) to be written as a decimal integer literal ([Integer literals](lexical-structure.md#integer-literals)).</span></span>

   <span data-ttu-id="70e32-1549">Se o operando do operador de negação é do tipo `ulong`, ocorre um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1549">If the operand of the negation operator is of type `ulong`, a compile-time error occurs.</span></span> <span data-ttu-id="70e32-1550">Uma exceção é a regra que permite que o `long` valor de -9223372036854775808 (-2 ^ 63) a ser gravado como um literal de inteiro decimal ([literais inteiros](lexical-structure.md#integer-literals)).</span><span class="sxs-lookup"><span data-stu-id="70e32-1550">An exception is the rule that permits the `long` value -9223372036854775808 (-2^63) to be written as a decimal integer literal ([Integer literals](lexical-structure.md#integer-literals)).</span></span>

*  <span data-ttu-id="70e32-1551">Negação de ponto flutuante:</span><span class="sxs-lookup"><span data-stu-id="70e32-1551">Floating-point negation:</span></span>

   ```csharp
   float operator -(float x);
   double operator -(double x);
   ```

   <span data-ttu-id="70e32-1552">O resultado é o valor de `x` com o sinal invertido.</span><span class="sxs-lookup"><span data-stu-id="70e32-1552">The result is the value of `x` with its sign inverted.</span></span> <span data-ttu-id="70e32-1553">Se `x` é NaN, o resultado também é NaN.</span><span class="sxs-lookup"><span data-stu-id="70e32-1553">If `x` is NaN, the result is also NaN.</span></span>

*  <span data-ttu-id="70e32-1554">Negação decimal:</span><span class="sxs-lookup"><span data-stu-id="70e32-1554">Decimal negation:</span></span>

   ```csharp
   decimal operator -(decimal x);
   ```

   <span data-ttu-id="70e32-1555">O resultado é calculado subtraindo `x` de zero.</span><span class="sxs-lookup"><span data-stu-id="70e32-1555">The result is computed by subtracting `x` from zero.</span></span> <span data-ttu-id="70e32-1556">Negação decimal é equivalente a usar o operador do tipo de menos unário `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1556">Decimal negation is equivalent to using the unary minus operator of type `System.Decimal`.</span></span>

### <a name="logical-negation-operator"></a><span data-ttu-id="70e32-1557">Operador de negação lógica</span><span class="sxs-lookup"><span data-stu-id="70e32-1557">Logical negation operator</span></span>

<span data-ttu-id="70e32-1558">Para uma operação do formulário `!x`, resolução de sobrecarga de operador unário ([resolução de sobrecarga de operador unário](expressions.md#unary-operator-overload-resolution)) é aplicado para selecionar uma implementação do operador específico.</span><span class="sxs-lookup"><span data-stu-id="70e32-1558">For an operation of the form `!x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="70e32-1559">O operando será convertido ao tipo de parâmetro do operador selecionado e o tipo do resultado é o tipo de retorno do operador.</span><span class="sxs-lookup"><span data-stu-id="70e32-1559">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="70e32-1560">Existe apenas um operador de negação lógica predefinidos:</span><span class="sxs-lookup"><span data-stu-id="70e32-1560">Only one predefined logical negation operator exists:</span></span>
```csharp
bool operator !(bool x);
```

<span data-ttu-id="70e32-1561">Esse operador calcula a negação lógica do operando: se o operando `true`, o resultado é `false`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1561">This operator computes the logical negation of the operand: If the operand is `true`, the result is `false`.</span></span> <span data-ttu-id="70e32-1562">Se o operando `false`, o resultado é `true`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1562">If the operand is `false`, the result is `true`.</span></span>

### <a name="bitwise-complement-operator"></a><span data-ttu-id="70e32-1563">Operador de complemento bit a bit</span><span class="sxs-lookup"><span data-stu-id="70e32-1563">Bitwise complement operator</span></span>

<span data-ttu-id="70e32-1564">Para uma operação do formulário `~x`, resolução de sobrecarga de operador unário ([resolução de sobrecarga de operador unário](expressions.md#unary-operator-overload-resolution)) é aplicado para selecionar uma implementação do operador específico.</span><span class="sxs-lookup"><span data-stu-id="70e32-1564">For an operation of the form `~x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="70e32-1565">O operando será convertido ao tipo de parâmetro do operador selecionado e o tipo do resultado é o tipo de retorno do operador.</span><span class="sxs-lookup"><span data-stu-id="70e32-1565">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="70e32-1566">Os operadores de complemento bit a bit predefinidos são:</span><span class="sxs-lookup"><span data-stu-id="70e32-1566">The predefined bitwise complement operators are:</span></span>
```csharp
int operator ~(int x);
uint operator ~(uint x);
long operator ~(long x);
ulong operator ~(ulong x);
```

<span data-ttu-id="70e32-1567">Para cada um desses operadores, o resultado da operação é o complemento bit a bit de `x`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1567">For each of these operators, the result of the operation is the bitwise complement of `x`.</span></span>

<span data-ttu-id="70e32-1568">Cada tipo de enumeração `E` implicitamente fornece o operador de complemento bit a bit a seguir:</span><span class="sxs-lookup"><span data-stu-id="70e32-1568">Every enumeration type `E` implicitly provides the following bitwise complement operator:</span></span>

```csharp
E operator ~(E x);
```

<span data-ttu-id="70e32-1569">O resultado da avaliação `~x`, onde `x` é uma expressão de um tipo de enumeração `E` com um tipo subjacente `U`, é exatamente o mesmo que avaliar `(E)(~(U)x)`, exceto que a conversão em `E` é sempre é executada como se estiver em um `unchecked` contexto ([os operadores marcados e desmarcados](expressions.md#the-checked-and-unchecked-operators)).</span><span class="sxs-lookup"><span data-stu-id="70e32-1569">The result of evaluating `~x`, where `x` is an expression of an enumeration type `E` with an underlying type `U`, is exactly the same as evaluating `(E)(~(U)x)`, except that the conversion to `E` is always performed as if in an `unchecked` context ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)).</span></span>

### <a name="prefix-increment-and-decrement-operators"></a><span data-ttu-id="70e32-1570">Incremento de prefixo e operadores de decremento</span><span class="sxs-lookup"><span data-stu-id="70e32-1570">Prefix increment and decrement operators</span></span>

```antlr
pre_increment_expression
    : '++' unary_expression
    ;

pre_decrement_expression
    : '--' unary_expression
    ;
```

<span data-ttu-id="70e32-1571">O operando de um incremento de prefixo ou decremento operação deve ser uma expressão classificada como uma variável, um acesso de propriedade ou um acesso do indexador.</span><span class="sxs-lookup"><span data-stu-id="70e32-1571">The operand of a prefix increment or decrement operation must be an expression classified as a variable, a property access, or an indexer access.</span></span> <span data-ttu-id="70e32-1572">O resultado da operação é um valor do mesmo tipo como o operando.</span><span class="sxs-lookup"><span data-stu-id="70e32-1572">The result of the operation is a value of the same type as the operand.</span></span>

<span data-ttu-id="70e32-1573">Se o operando de um prefixo de incrementar ou operação de decremento é uma propriedade ou o acesso do indexador, propriedade ou indexador deve ter uma `get` e um `set` acessador.</span><span class="sxs-lookup"><span data-stu-id="70e32-1573">If the operand of a prefix increment or decrement operation is a property or indexer access, the property or indexer must have both a `get` and a `set` accessor.</span></span> <span data-ttu-id="70e32-1574">Se isso não for o caso, ocorre um erro em tempo de vinculação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1574">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="70e32-1575">Resolução de sobrecarga de operador unário ([resolução de sobrecarga de operador unário](expressions.md#unary-operator-overload-resolution)) é aplicado para selecionar uma implementação do operador específico.</span><span class="sxs-lookup"><span data-stu-id="70e32-1575">Unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="70e32-1576">Predefinidas `++` e `--` operadores existem para os seguintes tipos: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char` , `float`, `double`, `decimal`e qualquer tipo de enumeração.</span><span class="sxs-lookup"><span data-stu-id="70e32-1576">Predefined `++` and `--` operators exist for the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, and any enum type.</span></span> <span data-ttu-id="70e32-1577">Predefinido `++` operadores retornam o valor produzido pela adição de 1 para o operando e predefinido `--` operadores retornam o valor produzido pela subtração de 1 de operando.</span><span class="sxs-lookup"><span data-stu-id="70e32-1577">The predefined `++` operators return the value produced by adding 1 to the operand, and the predefined `--` operators return the value produced by subtracting 1 from the operand.</span></span> <span data-ttu-id="70e32-1578">Em um `checked` contexto, se o resultado dessa adição ou subtração está fora do intervalo do tipo de resultado e o tipo de resultado é um tipo integral ou um tipo de enumeração, um `System.OverflowException` é gerada.</span><span class="sxs-lookup"><span data-stu-id="70e32-1578">In a `checked` context, if the result of this addition or subtraction is outside the range of the result type and the result type is an integral type or enum type, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="70e32-1579">O processamento de tempo de execução de um incremento de prefixo ou operação do formulário de decremento `++x` ou `--x` consiste as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="70e32-1579">The run-time processing of a prefix increment or decrement operation of the form `++x` or `--x` consists of the following steps:</span></span>

*   <span data-ttu-id="70e32-1580">Se `x` é classificado como uma variável:</span><span class="sxs-lookup"><span data-stu-id="70e32-1580">If `x` is classified as a variable:</span></span>
    * <span data-ttu-id="70e32-1581">`x` é avaliado para produzir a variável.</span><span class="sxs-lookup"><span data-stu-id="70e32-1581">`x` is evaluated to produce the variable.</span></span>
    * <span data-ttu-id="70e32-1582">O operador selecionado é invocado com o valor de `x` como seu argumento.</span><span class="sxs-lookup"><span data-stu-id="70e32-1582">The selected operator is invoked with the value of `x` as its argument.</span></span>
    * <span data-ttu-id="70e32-1583">O valor retornado pelo operador é armazenado no local fornecido pela avaliação de `x`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1583">The value returned by the operator is stored in the location given by the evaluation of `x`.</span></span>
    * <span data-ttu-id="70e32-1584">O valor retornado pelo operador torna-se o resultado da operação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1584">The value returned by the operator becomes the result of the operation.</span></span>
*   <span data-ttu-id="70e32-1585">Se `x` é classificado como um acesso de propriedade ou indexador:</span><span class="sxs-lookup"><span data-stu-id="70e32-1585">If `x` is classified as a property or indexer access:</span></span>
    * <span data-ttu-id="70e32-1586">A expressão de instância (se `x` não é `static`) e a lista de argumentos (se `x` é um indexador de acesso) associadas com `x` são avaliados, e os resultados são usados em subsequente `get` e `set` invocações do acessador.</span><span class="sxs-lookup"><span data-stu-id="70e32-1586">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `get` and `set` accessor invocations.</span></span>
    * <span data-ttu-id="70e32-1587">O `get` acessador de `x` é invocado.</span><span class="sxs-lookup"><span data-stu-id="70e32-1587">The `get` accessor of `x` is invoked.</span></span>
    * <span data-ttu-id="70e32-1588">O operador selecionado é invocado com o valor retornado pelo `get` acessador como seu argumento.</span><span class="sxs-lookup"><span data-stu-id="70e32-1588">The selected operator is invoked with the value returned by the `get` accessor as its argument.</span></span>
    * <span data-ttu-id="70e32-1589">O `set` acessador da `x` é invocado com o valor retornado pelo operador como seu `value` argumento.</span><span class="sxs-lookup"><span data-stu-id="70e32-1589">The `set` accessor of `x` is invoked with the value returned by the operator as its `value` argument.</span></span>
    * <span data-ttu-id="70e32-1590">O valor retornado pelo operador torna-se o resultado da operação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1590">The value returned by the operator becomes the result of the operation.</span></span>

<span data-ttu-id="70e32-1591">O `++` e `--` operadores também dão suporte a pós-fixação notação ([incremento de sufixo e operadores de decremento](expressions.md#postfix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="70e32-1591">The `++` and `--` operators also support postfix notation ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="70e32-1592">Normalmente, o resultado de `x++` ou `x--` é o valor da `x` antes da operação, enquanto que o resultado da `++x` ou `--x` é o valor do `x` após a operação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1592">Typically, the result of `x++` or `x--` is the value of `x` before the operation, whereas the result of `++x` or `--x` is the value of `x` after the operation.</span></span> <span data-ttu-id="70e32-1593">Em ambos os casos, `x` em si tem o mesmo valor após a operação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1593">In either case, `x` itself has the same value after the operation.</span></span>

<span data-ttu-id="70e32-1594">Uma `operator++` ou `operator--` implementação pode ser invocada usando a notação de prefixo ou sufixo.</span><span class="sxs-lookup"><span data-stu-id="70e32-1594">An `operator++` or `operator--` implementation can be invoked using either postfix or prefix notation.</span></span> <span data-ttu-id="70e32-1595">Não é possível ter implementações de operadores separados para as duas notações.</span><span class="sxs-lookup"><span data-stu-id="70e32-1595">It is not possible to have separate operator implementations for the two notations.</span></span>

### <a name="cast-expressions"></a><span data-ttu-id="70e32-1596">Expressões de conversão</span><span class="sxs-lookup"><span data-stu-id="70e32-1596">Cast expressions</span></span>

<span data-ttu-id="70e32-1597">Um *cast_expression* é usado para converter explicitamente uma expressão para um determinado tipo.</span><span class="sxs-lookup"><span data-stu-id="70e32-1597">A *cast_expression* is used to explicitly convert an expression to a given type.</span></span>

```antlr
cast_expression
    : '(' type ')' unary_expression
    ;
```

<span data-ttu-id="70e32-1598">Um *cast_expression* do formulário `(T)E`, onde `T` é um *tipo* e `E` é uma *unary_expression*, executa um explícito conversão ([conversões explícitas](conversions.md#explicit-conversions)) do valor de `E` digitar `T`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1598">A *cast_expression* of the form `(T)E`, where `T` is a *type* and `E` is a *unary_expression*, performs an explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) of the value of `E` to type `T`.</span></span> <span data-ttu-id="70e32-1599">Se nenhuma conversão explícita existe a partir `E` para `T`, ocorrerá um erro em tempo de associação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1599">If no explicit conversion exists from `E` to `T`, a binding-time error occurs.</span></span> <span data-ttu-id="70e32-1600">Caso contrário, o resultado é o valor produzido pela conversão explícita.</span><span class="sxs-lookup"><span data-stu-id="70e32-1600">Otherwise, the result is the value produced by the explicit conversion.</span></span> <span data-ttu-id="70e32-1601">O resultado sempre é classificado como um valor, mesmo se `E` denota uma variável.</span><span class="sxs-lookup"><span data-stu-id="70e32-1601">The result is always classified as a value, even if `E` denotes a variable.</span></span>

<span data-ttu-id="70e32-1602">A gramática para um *cast_expression* leva a determinados ambiguidades sintáticas.</span><span class="sxs-lookup"><span data-stu-id="70e32-1602">The grammar for a *cast_expression* leads to certain syntactic ambiguities.</span></span> <span data-ttu-id="70e32-1603">Por exemplo, a expressão `(x)-y` pode ser interpretado como um *cast_expression* (uma conversão de `-y` digitar `x`) ou como um *additive_expression* combinado com um *parenthesized_expression* (que calcula o valor `x - y)`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1603">For example, the expression `(x)-y` could either be interpreted as a *cast_expression* (a cast of `-y` to type `x`) or as an *additive_expression* combined with a *parenthesized_expression* (which computes the value `x - y)`.</span></span>

<span data-ttu-id="70e32-1604">Para resolver *cast_expression* ambiguidades, a regra a seguir existir: uma sequência de um ou mais *token*s ([espaço em branco](lexical-structure.md#white-space)) entre parênteses é considerado o início de um *cast_expression* apenas se pelo menos um dos seguintes forem verdadeiro:</span><span class="sxs-lookup"><span data-stu-id="70e32-1604">To resolve *cast_expression* ambiguities, the following rule exists: A sequence of one or more *token*s ([White space](lexical-structure.md#white-space)) enclosed in parentheses is considered the start of a *cast_expression* only if at least one of the following are true:</span></span>

*  <span data-ttu-id="70e32-1605">A sequência de tokens é a gramática correta para uma *tipo*, mas não para um *expressão*.</span><span class="sxs-lookup"><span data-stu-id="70e32-1605">The sequence of tokens is correct grammar for a *type*, but not for an *expression*.</span></span>
*  <span data-ttu-id="70e32-1606">A sequência de tokens é a gramática correta para uma *tipo*, e o token imediatamente após os parênteses de fechamento é o token "`~`", o token "`!`", o token "`(`", um  *identificador* ([sequências de escape de caractere Unicode](lexical-structure.md#unicode-character-escape-sequences)), uma *literal* ([literais](lexical-structure.md#literals)), ou qualquer *palavra-chave*([Palavras-chave](lexical-structure.md#keywords)), exceto `as` e `is`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1606">The sequence of tokens is correct grammar for a *type*, and the token immediately following the closing parentheses is the token "`~`", the token "`!`", the token "`(`", an *identifier* ([Unicode character escape sequences](lexical-structure.md#unicode-character-escape-sequences)), a *literal* ([Literals](lexical-structure.md#literals)), or any *keyword* ([Keywords](lexical-structure.md#keywords)) except `as` and `is`.</span></span>

<span data-ttu-id="70e32-1607">O termo "gramática correta" acima significa apenas que a sequência de tokens deve estar de acordo com a produção gramatical específica.</span><span class="sxs-lookup"><span data-stu-id="70e32-1607">The term "correct grammar" above means only that the sequence of tokens must conform to the particular grammatical production.</span></span> <span data-ttu-id="70e32-1608">Especificamente, ele não considera o significado real de todos os identificadores constituintes.</span><span class="sxs-lookup"><span data-stu-id="70e32-1608">It specifically does not consider the actual meaning of any constituent identifiers.</span></span> <span data-ttu-id="70e32-1609">Por exemplo, se `x` e `y` são os identificadores, em seguida, `x.y` é a gramática correta para um tipo, mesmo se `x.y` , na verdade, não denota um tipo.</span><span class="sxs-lookup"><span data-stu-id="70e32-1609">For example, if `x` and `y` are identifiers, then `x.y` is correct grammar for a type, even if `x.y` doesn't actually denote a type.</span></span>

<span data-ttu-id="70e32-1610">Da regra de Desambiguidade segue que, se `x` e `y` são identificadores, `(x)y`, `(x)(y)`, e `(x)(-y)` são *cast_expression*s, mas `(x)-y` é não, mesmo se `x` identifica um tipo.</span><span class="sxs-lookup"><span data-stu-id="70e32-1610">From the disambiguation rule it follows that, if `x` and `y` are identifiers, `(x)y`, `(x)(y)`, and `(x)(-y)` are *cast_expression*s, but `(x)-y` is not, even if `x` identifies a type.</span></span> <span data-ttu-id="70e32-1611">No entanto, se `x` é uma palavra-chave que identifica um tipo predefinido (como `int`), em seguida, todos os quatro formulários são *cast_expression*s (porque tal uma palavra-chave não pode ser uma expressão, possivelmente, por si só).</span><span class="sxs-lookup"><span data-stu-id="70e32-1611">However, if `x` is a keyword that identifies a predefined type (such as `int`), then all four forms are *cast_expression*s (because such a keyword could not possibly be an expression by itself).</span></span>

### <a name="await-expressions"></a><span data-ttu-id="70e32-1612">Expressões await</span><span class="sxs-lookup"><span data-stu-id="70e32-1612">Await expressions</span></span>

<span data-ttu-id="70e32-1613">O operador de espera é usado para suspender a avaliação da função assíncrona até que a operação assíncrona representada pelo operando seja concluída.</span><span class="sxs-lookup"><span data-stu-id="70e32-1613">The await operator is used to suspend evaluation of the enclosing async function until the asynchronous operation represented by the operand has completed.</span></span>

```antlr
await_expression
    : 'await' unary_expression
    ;
```

<span data-ttu-id="70e32-1614">Uma *await_expression* só é permitida no corpo de uma função assíncrona ([iteradores](classes.md#iterators)).</span><span class="sxs-lookup"><span data-stu-id="70e32-1614">An *await_expression* is only allowed in the body of an async function ([Iterators](classes.md#iterators)).</span></span> <span data-ttu-id="70e32-1615">Dentro do delimitador mais próximo função assíncrona, um *await_expression* pode não ocorrer nesses locais:</span><span class="sxs-lookup"><span data-stu-id="70e32-1615">Within the nearest enclosing async function, an *await_expression* may not occur in these places:</span></span>

*  <span data-ttu-id="70e32-1616">Dentro de uma função anônima do aninhada (não assíncronas)</span><span class="sxs-lookup"><span data-stu-id="70e32-1616">Inside a nested (non-async) anonymous function</span></span>
*  <span data-ttu-id="70e32-1617">Dentro do bloco de um *lock_statement*</span><span class="sxs-lookup"><span data-stu-id="70e32-1617">Inside the block of a *lock_statement*</span></span>
*  <span data-ttu-id="70e32-1618">Em um contexto inseguro</span><span class="sxs-lookup"><span data-stu-id="70e32-1618">In an unsafe context</span></span>

<span data-ttu-id="70e32-1619">Observe que um *await_expression* não pode ocorrer na maioria dos lugares dentro de uma *query_expression*, porque aquelas sintaticamente são transformadas para usar as expressões lambda não assíncronas.</span><span class="sxs-lookup"><span data-stu-id="70e32-1619">Note that an *await_expression* cannot occur in most places within a *query_expression*, because those are syntactically transformed to use non-async lambda expressions.</span></span>

<span data-ttu-id="70e32-1620">Dentro de uma função assíncrona, `await` não pode ser usado como um identificador.</span><span class="sxs-lookup"><span data-stu-id="70e32-1620">Inside of an async function, `await` cannot be used as an identifier.</span></span> <span data-ttu-id="70e32-1621">Portanto, não há nenhuma ambiguidade sintática entre expressões await e várias expressões que envolvem identificadores.</span><span class="sxs-lookup"><span data-stu-id="70e32-1621">There is therefore no syntactic ambiguity between await-expressions and various expressions involving identifiers.</span></span> <span data-ttu-id="70e32-1622">Fora de funções assíncronas, `await` atua como um identificador normal.</span><span class="sxs-lookup"><span data-stu-id="70e32-1622">Outside of async functions, `await` acts as a normal identifier.</span></span>

<span data-ttu-id="70e32-1623">O operando de um *await_expression* é chamado de ***tarefa***.</span><span class="sxs-lookup"><span data-stu-id="70e32-1623">The operand of an *await_expression* is called the ***task***.</span></span> <span data-ttu-id="70e32-1624">Ele representa uma operação assíncrona que pode ou não pode ser concluída no momento a *await_expression* é avaliada.</span><span class="sxs-lookup"><span data-stu-id="70e32-1624">It represents an asynchronous operation that may or may not be complete at the time the *await_expression* is evaluated.</span></span> <span data-ttu-id="70e32-1625">É a finalidade do operador await suspender a execução da função assíncrona até que a tarefa aguardada seja concluída e, em seguida, obter seu resultado.</span><span class="sxs-lookup"><span data-stu-id="70e32-1625">The purpose of the await operator is to suspend execution of the enclosing async function until the awaited task is complete, and then obtain its outcome.</span></span>

#### <a name="awaitable-expressions"></a><span data-ttu-id="70e32-1626">Expressões de awaitable</span><span class="sxs-lookup"><span data-stu-id="70e32-1626">Awaitable expressions</span></span>

<span data-ttu-id="70e32-1627">A tarefa de uma expressão await é necessária para ser ***aguardável***.</span><span class="sxs-lookup"><span data-stu-id="70e32-1627">The task of an await expression is required to be ***awaitable***.</span></span> <span data-ttu-id="70e32-1628">Uma expressão `t` é awaitable, se tiver mantido por um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="70e32-1628">An expression `t` is awaitable if one of the following holds:</span></span>

*  <span data-ttu-id="70e32-1629">`t` é do tipo de tempo de compilação `dynamic`</span><span class="sxs-lookup"><span data-stu-id="70e32-1629">`t` is of compile time type `dynamic`</span></span>
*  <span data-ttu-id="70e32-1630">`t` tem um método de extensão ou de instância acessível chamado `GetAwaiter` sem parâmetros e nenhum parâmetro de tipo e um tipo de retorno `A` para o qual todos os itens a seguir manter:</span><span class="sxs-lookup"><span data-stu-id="70e32-1630">`t` has an accessible instance or extension method called `GetAwaiter` with no parameters and no type parameters, and a return type `A` for which all of the following hold:</span></span>
   * <span data-ttu-id="70e32-1631">`A` implementa a interface `System.Runtime.CompilerServices.INotifyCompletion` (daqui em diante, conhecido como `INotifyCompletion` para fins de brevidade)</span><span class="sxs-lookup"><span data-stu-id="70e32-1631">`A` implements the interface `System.Runtime.CompilerServices.INotifyCompletion` (hereafter known as `INotifyCompletion` for brevity)</span></span>
   * <span data-ttu-id="70e32-1632">`A` tem uma propriedade de instância acessível e legível `IsCompleted` do tipo `bool`</span><span class="sxs-lookup"><span data-stu-id="70e32-1632">`A` has an accessible, readable instance property `IsCompleted` of type `bool`</span></span>
   * <span data-ttu-id="70e32-1633">`A` tem um método de instância acessível `GetResult` sem parâmetros e nenhum parâmetro de tipo</span><span class="sxs-lookup"><span data-stu-id="70e32-1633">`A` has an accessible instance method `GetResult` with no parameters and no type parameters</span></span>

<span data-ttu-id="70e32-1634">A finalidade de `GetAwaiter` método é obter uma ***awaiter*** para a tarefa.</span><span class="sxs-lookup"><span data-stu-id="70e32-1634">The purpose of the `GetAwaiter` method is to obtain an ***awaiter*** for the task.</span></span> <span data-ttu-id="70e32-1635">O tipo `A` é chamado de ***tipo awaiter*** para a expressão await.</span><span class="sxs-lookup"><span data-stu-id="70e32-1635">The type `A` is called the ***awaiter type*** for the await expression.</span></span>

<span data-ttu-id="70e32-1636">A finalidade de `IsCompleted` é de propriedade determinar se a tarefa já foi concluída.</span><span class="sxs-lookup"><span data-stu-id="70e32-1636">The purpose of the `IsCompleted` property is to determine if the task is already complete.</span></span> <span data-ttu-id="70e32-1637">Nesse caso, não é necessário para suspender a avaliação.</span><span class="sxs-lookup"><span data-stu-id="70e32-1637">If so, there is no need to suspend evaluation.</span></span>

<span data-ttu-id="70e32-1638">A finalidade do `INotifyCompletion.OnCompleted` método é inscrever-se de "continuação" para a tarefa; ou seja, um delegado (do tipo `System.Action`) que será invocado quando a tarefa for concluída.</span><span class="sxs-lookup"><span data-stu-id="70e32-1638">The purpose of the `INotifyCompletion.OnCompleted` method is to sign up a "continuation" to the task; i.e. a delegate (of type `System.Action`) that will be invoked once the task is complete.</span></span>

<span data-ttu-id="70e32-1639">A finalidade de `GetResult` método é obter o resultado da tarefa quando ela for concluída.</span><span class="sxs-lookup"><span data-stu-id="70e32-1639">The purpose of the `GetResult` method is to obtain the outcome of the task once it is complete.</span></span> <span data-ttu-id="70e32-1640">Esse resultado pode ser a conclusão bem-sucedida, possivelmente com um valor de resultado, ou pode ser uma exceção que é lançada pelo `GetResult` método.</span><span class="sxs-lookup"><span data-stu-id="70e32-1640">This outcome may be successful completion, possibly with a result value, or it may be an exception which is thrown by the `GetResult` method.</span></span>

#### <a name="classification-of-await-expressions"></a><span data-ttu-id="70e32-1641">Expressões de classificação de await</span><span class="sxs-lookup"><span data-stu-id="70e32-1641">Classification of await expressions</span></span>

<span data-ttu-id="70e32-1642">A expressão `await t` é classificado da mesma forma que a expressão `(t).GetAwaiter().GetResult()`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1642">The expression `await t` is classified the same way as the expression `(t).GetAwaiter().GetResult()`.</span></span> <span data-ttu-id="70e32-1643">Portanto, se o tipo de retorno de `GetResult` está `void`, o *await_expression* é classificado como nada.</span><span class="sxs-lookup"><span data-stu-id="70e32-1643">Thus, if the return type of `GetResult` is `void`, the *await_expression* is classified as nothing.</span></span> <span data-ttu-id="70e32-1644">Se ele tiver um tipo de retorno não nulo `T`, o *await_expression* é classificado como um valor do tipo `T`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1644">If it has a non-void return type `T`, the *await_expression* is classified as a value of type `T`.</span></span>

#### <a name="runtime-evaluation-of-await-expressions"></a><span data-ttu-id="70e32-1645">Expressões de avaliação do tempo de execução do await</span><span class="sxs-lookup"><span data-stu-id="70e32-1645">Runtime evaluation of await expressions</span></span>

<span data-ttu-id="70e32-1646">Em tempo de execução, a expressão `await t` é avaliada como segue:</span><span class="sxs-lookup"><span data-stu-id="70e32-1646">At runtime, the expression `await t` is evaluated as follows:</span></span>

*  <span data-ttu-id="70e32-1647">Um awaiter `a` é obtido avaliando a expressão `(t).GetAwaiter()`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1647">An awaiter `a` is obtained by evaluating the expression `(t).GetAwaiter()`.</span></span>
*  <span data-ttu-id="70e32-1648">Um `bool` `b` é obtido avaliando a expressão `(a).IsCompleted`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1648">A `bool` `b` is obtained by evaluating the expression `(a).IsCompleted`.</span></span>
*  <span data-ttu-id="70e32-1649">Se `b` está `false` e em seguida, avaliação depende `a` implementa a interface `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (daqui em diante, conhecido como `ICriticalNotifyCompletion` para fins de brevidade).</span><span class="sxs-lookup"><span data-stu-id="70e32-1649">If `b` is `false` then evaluation depends on whether `a` implements the interface `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (hereafter known as `ICriticalNotifyCompletion` for brevity).</span></span> <span data-ttu-id="70e32-1650">Essa verificação é feita no tempo; de ligação ou seja, em tempo de execução se `a` tem o tipo de tempo de compilação `dynamic`e em tempo de compilação caso contrário.</span><span class="sxs-lookup"><span data-stu-id="70e32-1650">This check is done at binding time; i.e. at runtime if `a` has the compile time type `dynamic`, and at compile time otherwise.</span></span> <span data-ttu-id="70e32-1651">Deixe `r` denotam o delegado de continuação ([iteradores](classes.md#iterators)):</span><span class="sxs-lookup"><span data-stu-id="70e32-1651">Let `r` denote the resumption delegate ([Iterators](classes.md#iterators)):</span></span>
    * <span data-ttu-id="70e32-1652">Se `a` não implementa `ICriticalNotifyCompletion`, em seguida, a expressão `(a as (INotifyCompletion)).OnCompleted(r)` é avaliada.</span><span class="sxs-lookup"><span data-stu-id="70e32-1652">If `a` does not implement `ICriticalNotifyCompletion`, then the expression `(a as (INotifyCompletion)).OnCompleted(r)` is evaluated.</span></span>
    * <span data-ttu-id="70e32-1653">Se `a` implementar `ICriticalNotifyCompletion`, em seguida, a expressão `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)` é avaliada.</span><span class="sxs-lookup"><span data-stu-id="70e32-1653">If `a` does implement `ICriticalNotifyCompletion`, then the expression `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)` is evaluated.</span></span>
    * <span data-ttu-id="70e32-1654">Avaliação, em seguida, é suspenso e controle é retornado ao chamador da função assíncrona atual.</span><span class="sxs-lookup"><span data-stu-id="70e32-1654">Evaluation is then suspended, and control is returned to the current caller of the async function.</span></span>
*  <span data-ttu-id="70e32-1655">Qualquer um dos imediatamente após (se `b` foi `true`), ou mediante posterior invocação de delegado de continuação (se `b` foi `false`), a expressão `(a).GetResult()` é avaliada.</span><span class="sxs-lookup"><span data-stu-id="70e32-1655">Either immediately after (if `b` was `true`), or upon later invocation of the resumption delegate (if `b` was `false`), the expression `(a).GetResult()` is evaluated.</span></span> <span data-ttu-id="70e32-1656">Se ela retorna um valor, esse valor é o resultado do *await_expression*.</span><span class="sxs-lookup"><span data-stu-id="70e32-1656">If it returns a value, that value is the result of the *await_expression*.</span></span> <span data-ttu-id="70e32-1657">Caso contrário, o resultado é que nada.</span><span class="sxs-lookup"><span data-stu-id="70e32-1657">Otherwise the result is nothing.</span></span>

<span data-ttu-id="70e32-1658">A implementação de um awaiter métodos da interface `INotifyCompletion.OnCompleted` e `ICriticalNotifyCompletion.UnsafeOnCompleted` deve fazer com que o delegado `r` a ser invocado no máximo uma vez.</span><span class="sxs-lookup"><span data-stu-id="70e32-1658">An awaiter's implementation of the interface methods `INotifyCompletion.OnCompleted` and `ICriticalNotifyCompletion.UnsafeOnCompleted` should cause the delegate `r` to be invoked at most once.</span></span> <span data-ttu-id="70e32-1659">Caso contrário, o comportamento da função async será indefinido.</span><span class="sxs-lookup"><span data-stu-id="70e32-1659">Otherwise, the behavior of the enclosing async function is undefined.</span></span>

## <a name="arithmetic-operators"></a><span data-ttu-id="70e32-1660">Operadores aritméticos</span><span class="sxs-lookup"><span data-stu-id="70e32-1660">Arithmetic operators</span></span>

<span data-ttu-id="70e32-1661">O `*`, `/`, `%`, `+`, e `-` operadores são chamados de operadores aritméticos.</span><span class="sxs-lookup"><span data-stu-id="70e32-1661">The `*`, `/`, `%`, `+`, and `-` operators are called the arithmetic operators.</span></span>

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

<span data-ttu-id="70e32-1662">Se um operando do operador aritmético tem o tipo de tempo de compilação `dynamic`, em seguida, a expressão está associada dinamicamente ([vinculação dinâmica](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="70e32-1662">If an operand of an arithmetic operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="70e32-1663">Nesse caso é o tipo de tempo de compilação da expressão `dynamic`, e a resolução descrita a seguir ocorrerá em tempo de execução usando o tipo de tempo de execução desses operandos que tenham o tipo de tempo de compilação `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1663">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

### <a name="multiplication-operator"></a><span data-ttu-id="70e32-1664">Operador de multiplicação</span><span class="sxs-lookup"><span data-stu-id="70e32-1664">Multiplication operator</span></span>

<span data-ttu-id="70e32-1665">Para uma operação do formulário `x * y`, resolução de sobrecarga de operador binário ([resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)) é aplicado para selecionar uma implementação do operador específico.</span><span class="sxs-lookup"><span data-stu-id="70e32-1665">For an operation of the form `x * y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="70e32-1666">Os operandos são convertidos nos tipos de parâmetro do operador selecionado e o tipo do resultado é o tipo de retorno do operador.</span><span class="sxs-lookup"><span data-stu-id="70e32-1666">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="70e32-1667">Os operadores de multiplicação predefinidos estão listados abaixo.</span><span class="sxs-lookup"><span data-stu-id="70e32-1667">The predefined multiplication operators are listed below.</span></span> <span data-ttu-id="70e32-1668">Todos os operadores compute o produto dos `x` e `y`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1668">The operators all compute the product of `x` and `y`.</span></span>

*  <span data-ttu-id="70e32-1669">Multiplicação de inteiro:</span><span class="sxs-lookup"><span data-stu-id="70e32-1669">Integer multiplication:</span></span>

   ```csharp
   int operator *(int x, int y);
   uint operator *(uint x, uint y);
   long operator *(long x, long y);
   ulong operator *(ulong x, ulong y);
   ```

   <span data-ttu-id="70e32-1670">Em um `checked` contexto, se o produto está fora do intervalo do tipo de resultado, um `System.OverflowException` é gerada.</span><span class="sxs-lookup"><span data-stu-id="70e32-1670">In a `checked` context, if the product is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="70e32-1671">Em um `unchecked` contexto, estouros não são relatados e quaisquer bits de ordem superior significativos fora do intervalo do tipo de resultado são descartados.</span><span class="sxs-lookup"><span data-stu-id="70e32-1671">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>


*  <span data-ttu-id="70e32-1672">Multiplicação de ponto flutuante:</span><span class="sxs-lookup"><span data-stu-id="70e32-1672">Floating-point multiplication:</span></span>

   ```csharp
   float operator *(float x, float y);
   double operator *(double x, double y);
   ```

   <span data-ttu-id="70e32-1673">O produto é calculado de acordo com as regras de aritmética do IEEE 754.</span><span class="sxs-lookup"><span data-stu-id="70e32-1673">The product is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="70e32-1674">A tabela a seguir lista os resultados de todas as combinações possíveis de valores Finitas diferente de zero, zeros, infinitos e do NaN.</span><span class="sxs-lookup"><span data-stu-id="70e32-1674">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="70e32-1675">Na tabela, `x` e `y` são valores positivos de finitos.</span><span class="sxs-lookup"><span data-stu-id="70e32-1675">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="70e32-1676">`z` é o resultado de `x * y`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1676">`z` is the result of `x * y`.</span></span> <span data-ttu-id="70e32-1677">Se o resultado for muito grande para o tipo de destino, `z` é infinito.</span><span class="sxs-lookup"><span data-stu-id="70e32-1677">If the result is too large for the destination type, `z` is infinity.</span></span> <span data-ttu-id="70e32-1678">Se o resultado for muito pequeno para o tipo de destino, `z` é zero.</span><span class="sxs-lookup"><span data-stu-id="70e32-1678">If the result is too small for the destination type, `z` is zero.</span></span>

   |      |      |      |     |     |      |      |     |
   |:----:|-----:|:----:|:---:|:---:|:----:|:----:|:----|
   |      | <span data-ttu-id="70e32-1679">+ y</span><span class="sxs-lookup"><span data-stu-id="70e32-1679">+y</span></span>   | <span data-ttu-id="70e32-1680">-y</span><span class="sxs-lookup"><span data-stu-id="70e32-1680">-y</span></span>   | <span data-ttu-id="70e32-1681">+0</span><span class="sxs-lookup"><span data-stu-id="70e32-1681">+0</span></span>  | <span data-ttu-id="70e32-1682">-0</span><span class="sxs-lookup"><span data-stu-id="70e32-1682">-0</span></span>  | <span data-ttu-id="70e32-1683">+inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1683">+inf</span></span> | <span data-ttu-id="70e32-1684">-inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1684">-inf</span></span> | <span data-ttu-id="70e32-1685">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1685">NaN</span></span> | 
   | <span data-ttu-id="70e32-1686">+ x</span><span class="sxs-lookup"><span data-stu-id="70e32-1686">+x</span></span>   | <span data-ttu-id="70e32-1687">+ z</span><span class="sxs-lookup"><span data-stu-id="70e32-1687">+z</span></span>   | <span data-ttu-id="70e32-1688">-z</span><span class="sxs-lookup"><span data-stu-id="70e32-1688">-z</span></span>   | <span data-ttu-id="70e32-1689">+0</span><span class="sxs-lookup"><span data-stu-id="70e32-1689">+0</span></span>  | <span data-ttu-id="70e32-1690">-0</span><span class="sxs-lookup"><span data-stu-id="70e32-1690">-0</span></span>  | <span data-ttu-id="70e32-1691">+inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1691">+inf</span></span> | <span data-ttu-id="70e32-1692">-inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1692">-inf</span></span> | <span data-ttu-id="70e32-1693">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1693">NaN</span></span> | 
   | <span data-ttu-id="70e32-1694">-x</span><span class="sxs-lookup"><span data-stu-id="70e32-1694">-x</span></span>   | <span data-ttu-id="70e32-1695">-z</span><span class="sxs-lookup"><span data-stu-id="70e32-1695">-z</span></span>   | <span data-ttu-id="70e32-1696">+ z</span><span class="sxs-lookup"><span data-stu-id="70e32-1696">+z</span></span>   | <span data-ttu-id="70e32-1697">-0</span><span class="sxs-lookup"><span data-stu-id="70e32-1697">-0</span></span>  | <span data-ttu-id="70e32-1698">+0</span><span class="sxs-lookup"><span data-stu-id="70e32-1698">+0</span></span>  | <span data-ttu-id="70e32-1699">-inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1699">-inf</span></span> | <span data-ttu-id="70e32-1700">+inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1700">+inf</span></span> | <span data-ttu-id="70e32-1701">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1701">NaN</span></span> | 
   | <span data-ttu-id="70e32-1702">+0</span><span class="sxs-lookup"><span data-stu-id="70e32-1702">+0</span></span>   | <span data-ttu-id="70e32-1703">+0</span><span class="sxs-lookup"><span data-stu-id="70e32-1703">+0</span></span>   | <span data-ttu-id="70e32-1704">-0</span><span class="sxs-lookup"><span data-stu-id="70e32-1704">-0</span></span>   | <span data-ttu-id="70e32-1705">+0</span><span class="sxs-lookup"><span data-stu-id="70e32-1705">+0</span></span>  | <span data-ttu-id="70e32-1706">-0</span><span class="sxs-lookup"><span data-stu-id="70e32-1706">-0</span></span>  | <span data-ttu-id="70e32-1707">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1707">NaN</span></span>  | <span data-ttu-id="70e32-1708">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1708">NaN</span></span>  | <span data-ttu-id="70e32-1709">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1709">NaN</span></span> | 
   | <span data-ttu-id="70e32-1710">-0</span><span class="sxs-lookup"><span data-stu-id="70e32-1710">-0</span></span>   | <span data-ttu-id="70e32-1711">-0</span><span class="sxs-lookup"><span data-stu-id="70e32-1711">-0</span></span>   | <span data-ttu-id="70e32-1712">+0</span><span class="sxs-lookup"><span data-stu-id="70e32-1712">+0</span></span>   | <span data-ttu-id="70e32-1713">-0</span><span class="sxs-lookup"><span data-stu-id="70e32-1713">-0</span></span>  | <span data-ttu-id="70e32-1714">+0</span><span class="sxs-lookup"><span data-stu-id="70e32-1714">+0</span></span>  | <span data-ttu-id="70e32-1715">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1715">NaN</span></span>  | <span data-ttu-id="70e32-1716">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1716">NaN</span></span>  | <span data-ttu-id="70e32-1717">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1717">NaN</span></span> | 
   | <span data-ttu-id="70e32-1718">+inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1718">+inf</span></span> | <span data-ttu-id="70e32-1719">+inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1719">+inf</span></span> | <span data-ttu-id="70e32-1720">-inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1720">-inf</span></span> | <span data-ttu-id="70e32-1721">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1721">NaN</span></span> | <span data-ttu-id="70e32-1722">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1722">NaN</span></span> | <span data-ttu-id="70e32-1723">+inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1723">+inf</span></span> | <span data-ttu-id="70e32-1724">-inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1724">-inf</span></span> | <span data-ttu-id="70e32-1725">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1725">NaN</span></span> | 
   | <span data-ttu-id="70e32-1726">-inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1726">-inf</span></span> | <span data-ttu-id="70e32-1727">-inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1727">-inf</span></span> | <span data-ttu-id="70e32-1728">+inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1728">+inf</span></span> | <span data-ttu-id="70e32-1729">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1729">NaN</span></span> | <span data-ttu-id="70e32-1730">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1730">NaN</span></span> | <span data-ttu-id="70e32-1731">-inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1731">-inf</span></span> | <span data-ttu-id="70e32-1732">+inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1732">+inf</span></span> | <span data-ttu-id="70e32-1733">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1733">NaN</span></span> | 
   | <span data-ttu-id="70e32-1734">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1734">NaN</span></span>  | <span data-ttu-id="70e32-1735">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1735">NaN</span></span>  | <span data-ttu-id="70e32-1736">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1736">NaN</span></span>  | <span data-ttu-id="70e32-1737">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1737">NaN</span></span> | <span data-ttu-id="70e32-1738">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1738">NaN</span></span> | <span data-ttu-id="70e32-1739">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1739">NaN</span></span>  | <span data-ttu-id="70e32-1740">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1740">NaN</span></span>  | <span data-ttu-id="70e32-1741">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1741">NaN</span></span> | 

*  <span data-ttu-id="70e32-1742">Multiplicação decimal:</span><span class="sxs-lookup"><span data-stu-id="70e32-1742">Decimal multiplication:</span></span>

   ```csharp
   decimal operator *(decimal x, decimal y);
   ```

   <span data-ttu-id="70e32-1743">Se o valor resultante é muito grande para representar o `decimal` formato, um `System.OverflowException` é gerada.</span><span class="sxs-lookup"><span data-stu-id="70e32-1743">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="70e32-1744">Se o valor do resultado for muito pequeno para representar o `decimal` formato, o resultado é zero.</span><span class="sxs-lookup"><span data-stu-id="70e32-1744">If the result value is too small to represent in the `decimal` format, the result is zero.</span></span> <span data-ttu-id="70e32-1745">A escala do resultado, antes de qualquer arredondamento, é a soma das escalas dos dois operandos.</span><span class="sxs-lookup"><span data-stu-id="70e32-1745">The scale of the result, before any rounding, is the sum of the scales of the two operands.</span></span>

   <span data-ttu-id="70e32-1746">Multiplicação decimal é equivalente a usar o operador de multiplicação do tipo `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1746">Decimal multiplication is equivalent to using the multiplication operator of type `System.Decimal`.</span></span>


### <a name="division-operator"></a><span data-ttu-id="70e32-1747">Operador de divisão</span><span class="sxs-lookup"><span data-stu-id="70e32-1747">Division operator</span></span>

<span data-ttu-id="70e32-1748">Para uma operação do formulário `x / y`, resolução de sobrecarga de operador binário ([resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)) é aplicado para selecionar uma implementação do operador específico.</span><span class="sxs-lookup"><span data-stu-id="70e32-1748">For an operation of the form `x / y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="70e32-1749">Os operandos são convertidos nos tipos de parâmetro do operador selecionado e o tipo do resultado é o tipo de retorno do operador.</span><span class="sxs-lookup"><span data-stu-id="70e32-1749">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="70e32-1750">Os operadores de divisão predefinidos estão listados abaixo.</span><span class="sxs-lookup"><span data-stu-id="70e32-1750">The predefined division operators are listed below.</span></span> <span data-ttu-id="70e32-1751">O quociente de computação de todos os operadores `x` e `y`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1751">The operators all compute the quotient of `x` and `y`.</span></span>

*  <span data-ttu-id="70e32-1752">Divisão de inteiros:</span><span class="sxs-lookup"><span data-stu-id="70e32-1752">Integer division:</span></span>

   ```csharp
   int operator /(int x, int y);
   uint operator /(uint x, uint y);
   long operator /(long x, long y);
   ulong operator /(ulong x, ulong y);
   ```

   <span data-ttu-id="70e32-1753">Se o valor do operando à direita for zero, um `System.DivideByZeroException` é gerada.</span><span class="sxs-lookup"><span data-stu-id="70e32-1753">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span>

   <span data-ttu-id="70e32-1754">A divisão Arredonda o resultado em direção a zero.</span><span class="sxs-lookup"><span data-stu-id="70e32-1754">The division rounds the result towards zero.</span></span> <span data-ttu-id="70e32-1755">Assim, o valor absoluto do resultado é o maior inteiro possível que seja menor ou igual ao valor absoluto do quociente de dois operandos.</span><span class="sxs-lookup"><span data-stu-id="70e32-1755">Thus the absolute value of the result is the largest possible integer that is less than or equal to the absolute value of the quotient of the two operands.</span></span> <span data-ttu-id="70e32-1756">O resultado é zero ou positivo quando os dois operandos têm o mesmo sinal e zero ou negativo quando os dois operandos tiverem oposta sinais.</span><span class="sxs-lookup"><span data-stu-id="70e32-1756">The result is zero or positive when the two operands have the same sign and zero or negative when the two operands have opposite signs.</span></span>

   <span data-ttu-id="70e32-1757">Se o operando esquerdo for menor representável `int` ou `long` valor e o operando direito é `-1`, ocorre um estouro.</span><span class="sxs-lookup"><span data-stu-id="70e32-1757">If the left operand is the smallest representable `int` or `long` value and the right operand is `-1`, an overflow occurs.</span></span> <span data-ttu-id="70e32-1758">Em um `checked` contexto, isso faz com que um `System.ArithmeticException` (ou uma subclasse disso) seja lançada.</span><span class="sxs-lookup"><span data-stu-id="70e32-1758">In a `checked` context, this causes a `System.ArithmeticException` (or a subclass thereof) to be thrown.</span></span> <span data-ttu-id="70e32-1759">Em um `unchecked` contexto, ele é definido pela implementação sobre se um `System.ArithmeticException` (ou uma subclasse disso) é gerada ou o estouro é relatado com o valor resultante, sendo que a do operando à esquerda.</span><span class="sxs-lookup"><span data-stu-id="70e32-1759">In an `unchecked` context, it is implementation-defined as to whether a `System.ArithmeticException` (or a subclass thereof) is thrown or the overflow goes unreported with the resulting value being that of the left operand.</span></span>

*  <span data-ttu-id="70e32-1760">Divisão de ponto flutuante:</span><span class="sxs-lookup"><span data-stu-id="70e32-1760">Floating-point division:</span></span>

   ```csharp
   float operator /(float x, float y);
   double operator /(double x, double y);
   ```

   <span data-ttu-id="70e32-1761">O quociente é calculado de acordo com as regras de aritmética do IEEE 754.</span><span class="sxs-lookup"><span data-stu-id="70e32-1761">The quotient is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="70e32-1762">A tabela a seguir lista os resultados de todas as combinações possíveis de valores Finitas diferente de zero, zeros, infinitos e do NaN.</span><span class="sxs-lookup"><span data-stu-id="70e32-1762">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="70e32-1763">Na tabela, `x` e `y` são valores positivos de finitos.</span><span class="sxs-lookup"><span data-stu-id="70e32-1763">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="70e32-1764">`z` é o resultado de `x / y`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1764">`z` is the result of `x / y`.</span></span> <span data-ttu-id="70e32-1765">Se o resultado for muito grande para o tipo de destino, `z` é infinito.</span><span class="sxs-lookup"><span data-stu-id="70e32-1765">If the result is too large for the destination type, `z` is infinity.</span></span> <span data-ttu-id="70e32-1766">Se o resultado for muito pequeno para o tipo de destino, `z` é zero.</span><span class="sxs-lookup"><span data-stu-id="70e32-1766">If the result is too small for the destination type, `z` is zero.</span></span>

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="70e32-1767">+ y</span><span class="sxs-lookup"><span data-stu-id="70e32-1767">+y</span></span>   | <span data-ttu-id="70e32-1768">-y</span><span class="sxs-lookup"><span data-stu-id="70e32-1768">-y</span></span>   | <span data-ttu-id="70e32-1769">+0</span><span class="sxs-lookup"><span data-stu-id="70e32-1769">+0</span></span>   | <span data-ttu-id="70e32-1770">-0</span><span class="sxs-lookup"><span data-stu-id="70e32-1770">-0</span></span>   | <span data-ttu-id="70e32-1771">+inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1771">+inf</span></span> | <span data-ttu-id="70e32-1772">-inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1772">-inf</span></span> | <span data-ttu-id="70e32-1773">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1773">NaN</span></span>  | 
   | <span data-ttu-id="70e32-1774">+ x</span><span class="sxs-lookup"><span data-stu-id="70e32-1774">+x</span></span>   | <span data-ttu-id="70e32-1775">+ z</span><span class="sxs-lookup"><span data-stu-id="70e32-1775">+z</span></span>   | <span data-ttu-id="70e32-1776">-z</span><span class="sxs-lookup"><span data-stu-id="70e32-1776">-z</span></span>   | <span data-ttu-id="70e32-1777">+inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1777">+inf</span></span> | <span data-ttu-id="70e32-1778">-inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1778">-inf</span></span> | <span data-ttu-id="70e32-1779">+0</span><span class="sxs-lookup"><span data-stu-id="70e32-1779">+0</span></span>   | <span data-ttu-id="70e32-1780">-0</span><span class="sxs-lookup"><span data-stu-id="70e32-1780">-0</span></span>   | <span data-ttu-id="70e32-1781">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1781">NaN</span></span>  | 
   | <span data-ttu-id="70e32-1782">-x</span><span class="sxs-lookup"><span data-stu-id="70e32-1782">-x</span></span>   | <span data-ttu-id="70e32-1783">-z</span><span class="sxs-lookup"><span data-stu-id="70e32-1783">-z</span></span>   | <span data-ttu-id="70e32-1784">+ z</span><span class="sxs-lookup"><span data-stu-id="70e32-1784">+z</span></span>   | <span data-ttu-id="70e32-1785">-inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1785">-inf</span></span> | <span data-ttu-id="70e32-1786">+inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1786">+inf</span></span> | <span data-ttu-id="70e32-1787">-0</span><span class="sxs-lookup"><span data-stu-id="70e32-1787">-0</span></span>   | <span data-ttu-id="70e32-1788">+0</span><span class="sxs-lookup"><span data-stu-id="70e32-1788">+0</span></span>   | <span data-ttu-id="70e32-1789">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1789">NaN</span></span>  | 
   | <span data-ttu-id="70e32-1790">+0</span><span class="sxs-lookup"><span data-stu-id="70e32-1790">+0</span></span>   | <span data-ttu-id="70e32-1791">+0</span><span class="sxs-lookup"><span data-stu-id="70e32-1791">+0</span></span>   | <span data-ttu-id="70e32-1792">-0</span><span class="sxs-lookup"><span data-stu-id="70e32-1792">-0</span></span>   | <span data-ttu-id="70e32-1793">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1793">NaN</span></span>  | <span data-ttu-id="70e32-1794">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1794">NaN</span></span>  | <span data-ttu-id="70e32-1795">+0</span><span class="sxs-lookup"><span data-stu-id="70e32-1795">+0</span></span>   | <span data-ttu-id="70e32-1796">-0</span><span class="sxs-lookup"><span data-stu-id="70e32-1796">-0</span></span>   | <span data-ttu-id="70e32-1797">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1797">NaN</span></span>  | 
   | <span data-ttu-id="70e32-1798">-0</span><span class="sxs-lookup"><span data-stu-id="70e32-1798">-0</span></span>   | <span data-ttu-id="70e32-1799">-0</span><span class="sxs-lookup"><span data-stu-id="70e32-1799">-0</span></span>   | <span data-ttu-id="70e32-1800">+0</span><span class="sxs-lookup"><span data-stu-id="70e32-1800">+0</span></span>   | <span data-ttu-id="70e32-1801">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1801">NaN</span></span>  | <span data-ttu-id="70e32-1802">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1802">NaN</span></span>  | <span data-ttu-id="70e32-1803">-0</span><span class="sxs-lookup"><span data-stu-id="70e32-1803">-0</span></span>   | <span data-ttu-id="70e32-1804">+0</span><span class="sxs-lookup"><span data-stu-id="70e32-1804">+0</span></span>   | <span data-ttu-id="70e32-1805">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1805">NaN</span></span>  | 
   | <span data-ttu-id="70e32-1806">+inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1806">+inf</span></span> | <span data-ttu-id="70e32-1807">+inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1807">+inf</span></span> | <span data-ttu-id="70e32-1808">-inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1808">-inf</span></span> | <span data-ttu-id="70e32-1809">+inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1809">+inf</span></span> | <span data-ttu-id="70e32-1810">-inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1810">-inf</span></span> | <span data-ttu-id="70e32-1811">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1811">NaN</span></span>  | <span data-ttu-id="70e32-1812">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1812">NaN</span></span>  | <span data-ttu-id="70e32-1813">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1813">NaN</span></span>  | 
   | <span data-ttu-id="70e32-1814">-inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1814">-inf</span></span> | <span data-ttu-id="70e32-1815">-inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1815">-inf</span></span> | <span data-ttu-id="70e32-1816">+inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1816">+inf</span></span> | <span data-ttu-id="70e32-1817">-inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1817">-inf</span></span> | <span data-ttu-id="70e32-1818">+inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1818">+inf</span></span> | <span data-ttu-id="70e32-1819">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1819">NaN</span></span>  | <span data-ttu-id="70e32-1820">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1820">NaN</span></span>  | <span data-ttu-id="70e32-1821">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1821">NaN</span></span>  | 
   | <span data-ttu-id="70e32-1822">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1822">NaN</span></span>  | <span data-ttu-id="70e32-1823">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1823">NaN</span></span>  | <span data-ttu-id="70e32-1824">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1824">NaN</span></span>  | <span data-ttu-id="70e32-1825">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1825">NaN</span></span>  | <span data-ttu-id="70e32-1826">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1826">NaN</span></span>  | <span data-ttu-id="70e32-1827">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1827">NaN</span></span>  | <span data-ttu-id="70e32-1828">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1828">NaN</span></span>  | <span data-ttu-id="70e32-1829">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1829">NaN</span></span>  | 

*  <span data-ttu-id="70e32-1830">Divisão decimal:</span><span class="sxs-lookup"><span data-stu-id="70e32-1830">Decimal division:</span></span>

   ```csharp
   decimal operator /(decimal x, decimal y);
   ```

   <span data-ttu-id="70e32-1831">Se o valor do operando à direita for zero, um `System.DivideByZeroException` é gerada.</span><span class="sxs-lookup"><span data-stu-id="70e32-1831">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span> <span data-ttu-id="70e32-1832">Se o valor resultante é muito grande para representar o `decimal` formato, um `System.OverflowException` é gerada.</span><span class="sxs-lookup"><span data-stu-id="70e32-1832">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="70e32-1833">Se o valor do resultado for muito pequeno para representar o `decimal` formato, o resultado é zero.</span><span class="sxs-lookup"><span data-stu-id="70e32-1833">If the result value is too small to represent in the `decimal` format, the result is zero.</span></span> <span data-ttu-id="70e32-1834">A escala do resultado é a escala menor que irá preservar um resultado igual ao valor decimal representável para o resultado matemático true mais próximo.</span><span class="sxs-lookup"><span data-stu-id="70e32-1834">The scale of the result is the smallest scale that will preserve a result equal to the nearest representable decimal value to the true mathematical result.</span></span>

   <span data-ttu-id="70e32-1835">Divisão decimal é equivalente a usar o operador de divisão do tipo `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1835">Decimal division is equivalent to using the division operator of type `System.Decimal`.</span></span>


### <a name="remainder-operator"></a><span data-ttu-id="70e32-1836">Operador restante</span><span class="sxs-lookup"><span data-stu-id="70e32-1836">Remainder operator</span></span>

<span data-ttu-id="70e32-1837">Para uma operação do formulário `x % y`, resolução de sobrecarga de operador binário ([resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)) é aplicado para selecionar uma implementação do operador específico.</span><span class="sxs-lookup"><span data-stu-id="70e32-1837">For an operation of the form `x % y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="70e32-1838">Os operandos são convertidos nos tipos de parâmetro do operador selecionado e o tipo do resultado é o tipo de retorno do operador.</span><span class="sxs-lookup"><span data-stu-id="70e32-1838">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="70e32-1839">Os operadores de restante predefinidos estão listados abaixo.</span><span class="sxs-lookup"><span data-stu-id="70e32-1839">The predefined remainder operators are listed below.</span></span> <span data-ttu-id="70e32-1840">Todos os operadores compute o resto da divisão entre `x` e `y`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1840">The operators all compute the remainder of the division between `x` and `y`.</span></span>

*  <span data-ttu-id="70e32-1841">Resto inteiro:</span><span class="sxs-lookup"><span data-stu-id="70e32-1841">Integer remainder:</span></span>

   ```csharp
   int operator %(int x, int y);
   uint operator %(uint x, uint y);
   long operator %(long x, long y);
   ulong operator %(ulong x, ulong y);
   ```

   <span data-ttu-id="70e32-1842">O resultado de `x % y` é o valor produzido por `x - (x / y) * y`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1842">The result of `x % y` is the value produced by `x - (x / y) * y`.</span></span> <span data-ttu-id="70e32-1843">Se `y` for zero, um `System.DivideByZeroException` é gerada.</span><span class="sxs-lookup"><span data-stu-id="70e32-1843">If `y` is zero, a `System.DivideByZeroException` is thrown.</span></span>

   <span data-ttu-id="70e32-1844">Se o operando esquerdo for menor `int` ou `long` é de valor e o operando direito `-1`, um `System.OverflowException` é gerada.</span><span class="sxs-lookup"><span data-stu-id="70e32-1844">If the left operand is the smallest `int` or `long` value and the right operand is `-1`, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="70e32-1845">Em nenhum caso faz `x % y` lançar uma exceção onde `x / y` não geraria uma exceção.</span><span class="sxs-lookup"><span data-stu-id="70e32-1845">In no case does `x % y` throw an exception where `x / y` would not throw an exception.</span></span>

*  <span data-ttu-id="70e32-1846">Ponto flutuante restante:</span><span class="sxs-lookup"><span data-stu-id="70e32-1846">Floating-point remainder:</span></span>

   ```csharp
   float operator %(float x, float y);
   double operator %(double x, double y);
   ```

   <span data-ttu-id="70e32-1847">A tabela a seguir lista os resultados de todas as combinações possíveis de valores Finitas diferente de zero, zeros, infinitos e do NaN.</span><span class="sxs-lookup"><span data-stu-id="70e32-1847">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="70e32-1848">Na tabela, `x` e `y` são valores positivos de finitos.</span><span class="sxs-lookup"><span data-stu-id="70e32-1848">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="70e32-1849">`z` é o resultado de `x % y` e é computado como `x - n * y`, onde `n` é o maior inteiro possíveis que é menor que ou igual a `x / y`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1849">`z` is the result of `x % y` and is computed as `x - n * y`, where `n` is the largest possible integer that is less than or equal to `x / y`.</span></span> <span data-ttu-id="70e32-1850">Esse método de computação o restante é análogo àquela utilizada para operandos de inteiro, mas é diferente da definição de IEEE 754 (no qual `n` é o inteiro mais próximo ao `x / y`).</span><span class="sxs-lookup"><span data-stu-id="70e32-1850">This method of computing the remainder is analogous to that used for integer operands, but differs from the IEEE 754 definition (in which `n` is the integer closest to `x / y`).</span></span>

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="70e32-1851">+ y</span><span class="sxs-lookup"><span data-stu-id="70e32-1851">+y</span></span>   | <span data-ttu-id="70e32-1852">-y</span><span class="sxs-lookup"><span data-stu-id="70e32-1852">-y</span></span>   | <span data-ttu-id="70e32-1853">+0</span><span class="sxs-lookup"><span data-stu-id="70e32-1853">+0</span></span>   | <span data-ttu-id="70e32-1854">-0</span><span class="sxs-lookup"><span data-stu-id="70e32-1854">-0</span></span>   | <span data-ttu-id="70e32-1855">+inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1855">+inf</span></span> | <span data-ttu-id="70e32-1856">-inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1856">-inf</span></span> | <span data-ttu-id="70e32-1857">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1857">NaN</span></span>  | 
   | <span data-ttu-id="70e32-1858">+ x</span><span class="sxs-lookup"><span data-stu-id="70e32-1858">+x</span></span>   | <span data-ttu-id="70e32-1859">+ z</span><span class="sxs-lookup"><span data-stu-id="70e32-1859">+z</span></span>   | <span data-ttu-id="70e32-1860">+ z</span><span class="sxs-lookup"><span data-stu-id="70e32-1860">+z</span></span>   | <span data-ttu-id="70e32-1861">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1861">NaN</span></span>  | <span data-ttu-id="70e32-1862">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1862">NaN</span></span>  | <span data-ttu-id="70e32-1863">x</span><span class="sxs-lookup"><span data-stu-id="70e32-1863">x</span></span>    | <span data-ttu-id="70e32-1864">x</span><span class="sxs-lookup"><span data-stu-id="70e32-1864">x</span></span>    | <span data-ttu-id="70e32-1865">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1865">NaN</span></span>  | 
   | <span data-ttu-id="70e32-1866">-x</span><span class="sxs-lookup"><span data-stu-id="70e32-1866">-x</span></span>   | <span data-ttu-id="70e32-1867">-z</span><span class="sxs-lookup"><span data-stu-id="70e32-1867">-z</span></span>   | <span data-ttu-id="70e32-1868">-z</span><span class="sxs-lookup"><span data-stu-id="70e32-1868">-z</span></span>   | <span data-ttu-id="70e32-1869">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1869">NaN</span></span>  | <span data-ttu-id="70e32-1870">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1870">NaN</span></span>  | <span data-ttu-id="70e32-1871">-x</span><span class="sxs-lookup"><span data-stu-id="70e32-1871">-x</span></span>   | <span data-ttu-id="70e32-1872">-x</span><span class="sxs-lookup"><span data-stu-id="70e32-1872">-x</span></span>   | <span data-ttu-id="70e32-1873">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1873">NaN</span></span>  | 
   | <span data-ttu-id="70e32-1874">+0</span><span class="sxs-lookup"><span data-stu-id="70e32-1874">+0</span></span>   | <span data-ttu-id="70e32-1875">+0</span><span class="sxs-lookup"><span data-stu-id="70e32-1875">+0</span></span>   | <span data-ttu-id="70e32-1876">+0</span><span class="sxs-lookup"><span data-stu-id="70e32-1876">+0</span></span>   | <span data-ttu-id="70e32-1877">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1877">NaN</span></span>  | <span data-ttu-id="70e32-1878">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1878">NaN</span></span>  | <span data-ttu-id="70e32-1879">+0</span><span class="sxs-lookup"><span data-stu-id="70e32-1879">+0</span></span>   | <span data-ttu-id="70e32-1880">+0</span><span class="sxs-lookup"><span data-stu-id="70e32-1880">+0</span></span>   | <span data-ttu-id="70e32-1881">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1881">NaN</span></span>  | 
   | <span data-ttu-id="70e32-1882">-0</span><span class="sxs-lookup"><span data-stu-id="70e32-1882">-0</span></span>   | <span data-ttu-id="70e32-1883">-0</span><span class="sxs-lookup"><span data-stu-id="70e32-1883">-0</span></span>   | <span data-ttu-id="70e32-1884">-0</span><span class="sxs-lookup"><span data-stu-id="70e32-1884">-0</span></span>   | <span data-ttu-id="70e32-1885">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1885">NaN</span></span>  | <span data-ttu-id="70e32-1886">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1886">NaN</span></span>  | <span data-ttu-id="70e32-1887">-0</span><span class="sxs-lookup"><span data-stu-id="70e32-1887">-0</span></span>   | <span data-ttu-id="70e32-1888">-0</span><span class="sxs-lookup"><span data-stu-id="70e32-1888">-0</span></span>   | <span data-ttu-id="70e32-1889">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1889">NaN</span></span>  | 
   | <span data-ttu-id="70e32-1890">+inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1890">+inf</span></span> | <span data-ttu-id="70e32-1891">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1891">NaN</span></span>  | <span data-ttu-id="70e32-1892">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1892">NaN</span></span>  | <span data-ttu-id="70e32-1893">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1893">NaN</span></span>  | <span data-ttu-id="70e32-1894">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1894">NaN</span></span>  | <span data-ttu-id="70e32-1895">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1895">NaN</span></span>  | <span data-ttu-id="70e32-1896">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1896">NaN</span></span>  | <span data-ttu-id="70e32-1897">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1897">NaN</span></span>  | 
   | <span data-ttu-id="70e32-1898">-inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1898">-inf</span></span> | <span data-ttu-id="70e32-1899">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1899">NaN</span></span>  | <span data-ttu-id="70e32-1900">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1900">NaN</span></span>  | <span data-ttu-id="70e32-1901">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1901">NaN</span></span>  | <span data-ttu-id="70e32-1902">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1902">NaN</span></span>  | <span data-ttu-id="70e32-1903">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1903">NaN</span></span>  | <span data-ttu-id="70e32-1904">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1904">NaN</span></span>  | <span data-ttu-id="70e32-1905">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1905">NaN</span></span>  | 
   | <span data-ttu-id="70e32-1906">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1906">NaN</span></span>  | <span data-ttu-id="70e32-1907">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1907">NaN</span></span>  | <span data-ttu-id="70e32-1908">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1908">NaN</span></span>  | <span data-ttu-id="70e32-1909">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1909">NaN</span></span>  | <span data-ttu-id="70e32-1910">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1910">NaN</span></span>  | <span data-ttu-id="70e32-1911">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1911">NaN</span></span>  | <span data-ttu-id="70e32-1912">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1912">NaN</span></span>  | <span data-ttu-id="70e32-1913">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1913">NaN</span></span>  | 

*  <span data-ttu-id="70e32-1914">Decimal restante:</span><span class="sxs-lookup"><span data-stu-id="70e32-1914">Decimal remainder:</span></span>

   ```csharp
   decimal operator %(decimal x, decimal y);
   ```

   <span data-ttu-id="70e32-1915">Se o valor do operando à direita for zero, um `System.DivideByZeroException` é gerada.</span><span class="sxs-lookup"><span data-stu-id="70e32-1915">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span> <span data-ttu-id="70e32-1916">A escala do resultado, antes de qualquer arredondamento, é o maior entre as escalas de dois operandos, e o sinal do resultado, se diferente de zero, é o mesmo da `x`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1916">The scale of the result, before any rounding, is the larger of the scales of the two operands, and the sign of the result, if non-zero, is the same as that of `x`.</span></span>

   <span data-ttu-id="70e32-1917">Decimal restante é equivalente a usar o operador de resto do tipo `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1917">Decimal remainder is equivalent to using the remainder operator of type `System.Decimal`.</span></span>


### <a name="addition-operator"></a><span data-ttu-id="70e32-1918">Operador de adição</span><span class="sxs-lookup"><span data-stu-id="70e32-1918">Addition operator</span></span>

<span data-ttu-id="70e32-1919">Para uma operação do formulário `x + y`, resolução de sobrecarga de operador binário ([resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)) é aplicado para selecionar uma implementação do operador específico.</span><span class="sxs-lookup"><span data-stu-id="70e32-1919">For an operation of the form `x + y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="70e32-1920">Os operandos são convertidos nos tipos de parâmetro do operador selecionado e o tipo do resultado é o tipo de retorno do operador.</span><span class="sxs-lookup"><span data-stu-id="70e32-1920">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="70e32-1921">Os operadores de adição predefinidas são listados abaixo.</span><span class="sxs-lookup"><span data-stu-id="70e32-1921">The predefined addition operators are listed below.</span></span> <span data-ttu-id="70e32-1922">Para tipos numéricos e de enumeração, os operadores de adição predefinidos calcular a soma dos dois operandos.</span><span class="sxs-lookup"><span data-stu-id="70e32-1922">For numeric and enumeration types, the predefined addition operators compute the sum of the two operands.</span></span> <span data-ttu-id="70e32-1923">Quando um ou ambos os operandos são do tipo cadeia de caracteres, os operadores de adição predefinidos concatenar a representação de cadeia de caracteres dos operandos.</span><span class="sxs-lookup"><span data-stu-id="70e32-1923">When one or both operands are of type string, the predefined addition operators concatenate the string representation of the operands.</span></span>

*  <span data-ttu-id="70e32-1924">Adição de inteiro:</span><span class="sxs-lookup"><span data-stu-id="70e32-1924">Integer addition:</span></span>

   ```csharp
   int operator +(int x, int y);
   uint operator +(uint x, uint y);
   long operator +(long x, long y);
   ulong operator +(ulong x, ulong y);
   ```

   <span data-ttu-id="70e32-1925">Em um `checked` contexto, se a soma está fora do intervalo do tipo de resultado, um `System.OverflowException` é gerada.</span><span class="sxs-lookup"><span data-stu-id="70e32-1925">In a `checked` context, if the sum is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="70e32-1926">Em um `unchecked` contexto, estouros não são relatados e quaisquer bits de ordem superior significativos fora do intervalo do tipo de resultado são descartados.</span><span class="sxs-lookup"><span data-stu-id="70e32-1926">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>

*  <span data-ttu-id="70e32-1927">Adição de ponto flutuante:</span><span class="sxs-lookup"><span data-stu-id="70e32-1927">Floating-point addition:</span></span>

   ```csharp
   float operator +(float x, float y);
   double operator +(double x, double y);
   ```

   <span data-ttu-id="70e32-1928">A soma é calculada de acordo com as regras de aritmética do IEEE 754.</span><span class="sxs-lookup"><span data-stu-id="70e32-1928">The sum is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="70e32-1929">A tabela a seguir lista os resultados de todas as combinações possíveis de valores Finitas diferente de zero, zeros, infinitos e do NaN.</span><span class="sxs-lookup"><span data-stu-id="70e32-1929">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="70e32-1930">Na tabela, `x` e `y` são valores Finitas diferente de zero, e `z` é o resultado de `x + y`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1930">In the table, `x` and `y` are nonzero finite values, and `z` is the result of `x + y`.</span></span> <span data-ttu-id="70e32-1931">Se `x` e `y` têm a mesma magnitude mas oposta sinais, `z` for zero positivo.</span><span class="sxs-lookup"><span data-stu-id="70e32-1931">If `x` and `y` have the same magnitude but opposite signs, `z` is positive zero.</span></span> <span data-ttu-id="70e32-1932">Se `x + y` é muito grande para ser representado no tipo de destino, `z` é infinito com o mesmo sinal que `x + y`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1932">If `x + y` is too large to represent in the destination type, `z` is an infinity with the same sign as `x + y`.</span></span>

   |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="70e32-1933">y</span><span class="sxs-lookup"><span data-stu-id="70e32-1933">y</span></span>    | <span data-ttu-id="70e32-1934">+0</span><span class="sxs-lookup"><span data-stu-id="70e32-1934">+0</span></span>   | <span data-ttu-id="70e32-1935">-0</span><span class="sxs-lookup"><span data-stu-id="70e32-1935">-0</span></span>   | <span data-ttu-id="70e32-1936">+inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1936">+inf</span></span> | <span data-ttu-id="70e32-1937">-inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1937">-inf</span></span> | <span data-ttu-id="70e32-1938">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1938">NaN</span></span>  | 
   | <span data-ttu-id="70e32-1939">x</span><span class="sxs-lookup"><span data-stu-id="70e32-1939">x</span></span>    | <span data-ttu-id="70e32-1940">z</span><span class="sxs-lookup"><span data-stu-id="70e32-1940">z</span></span>    | <span data-ttu-id="70e32-1941">x</span><span class="sxs-lookup"><span data-stu-id="70e32-1941">x</span></span>    | <span data-ttu-id="70e32-1942">x</span><span class="sxs-lookup"><span data-stu-id="70e32-1942">x</span></span>    | <span data-ttu-id="70e32-1943">+inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1943">+inf</span></span> | <span data-ttu-id="70e32-1944">-inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1944">-inf</span></span> | <span data-ttu-id="70e32-1945">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1945">NaN</span></span>  | 
   | <span data-ttu-id="70e32-1946">+0</span><span class="sxs-lookup"><span data-stu-id="70e32-1946">+0</span></span>   | <span data-ttu-id="70e32-1947">y</span><span class="sxs-lookup"><span data-stu-id="70e32-1947">y</span></span>    | <span data-ttu-id="70e32-1948">+0</span><span class="sxs-lookup"><span data-stu-id="70e32-1948">+0</span></span>   | <span data-ttu-id="70e32-1949">+0</span><span class="sxs-lookup"><span data-stu-id="70e32-1949">+0</span></span>   | <span data-ttu-id="70e32-1950">+inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1950">+inf</span></span> | <span data-ttu-id="70e32-1951">-inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1951">-inf</span></span> | <span data-ttu-id="70e32-1952">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1952">NaN</span></span>  | 
   | <span data-ttu-id="70e32-1953">-0</span><span class="sxs-lookup"><span data-stu-id="70e32-1953">-0</span></span>   | <span data-ttu-id="70e32-1954">y</span><span class="sxs-lookup"><span data-stu-id="70e32-1954">y</span></span>    | <span data-ttu-id="70e32-1955">+0</span><span class="sxs-lookup"><span data-stu-id="70e32-1955">+0</span></span>   | <span data-ttu-id="70e32-1956">-0</span><span class="sxs-lookup"><span data-stu-id="70e32-1956">-0</span></span>   | <span data-ttu-id="70e32-1957">+inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1957">+inf</span></span> | <span data-ttu-id="70e32-1958">-inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1958">-inf</span></span> | <span data-ttu-id="70e32-1959">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1959">NaN</span></span>  | 
   | <span data-ttu-id="70e32-1960">+inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1960">+inf</span></span> | <span data-ttu-id="70e32-1961">+inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1961">+inf</span></span> | <span data-ttu-id="70e32-1962">+inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1962">+inf</span></span> | <span data-ttu-id="70e32-1963">+inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1963">+inf</span></span> | <span data-ttu-id="70e32-1964">+inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1964">+inf</span></span> | <span data-ttu-id="70e32-1965">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1965">NaN</span></span>  | <span data-ttu-id="70e32-1966">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1966">NaN</span></span>  | 
   | <span data-ttu-id="70e32-1967">-inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1967">-inf</span></span> | <span data-ttu-id="70e32-1968">-inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1968">-inf</span></span> | <span data-ttu-id="70e32-1969">-inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1969">-inf</span></span> | <span data-ttu-id="70e32-1970">-inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1970">-inf</span></span> | <span data-ttu-id="70e32-1971">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1971">NaN</span></span>  | <span data-ttu-id="70e32-1972">-inf</span><span class="sxs-lookup"><span data-stu-id="70e32-1972">-inf</span></span> | <span data-ttu-id="70e32-1973">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1973">NaN</span></span>  | 
   | <span data-ttu-id="70e32-1974">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1974">NaN</span></span>  | <span data-ttu-id="70e32-1975">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1975">NaN</span></span>  | <span data-ttu-id="70e32-1976">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1976">NaN</span></span>  | <span data-ttu-id="70e32-1977">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1977">NaN</span></span>  | <span data-ttu-id="70e32-1978">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1978">NaN</span></span>  | <span data-ttu-id="70e32-1979">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1979">NaN</span></span>  | <span data-ttu-id="70e32-1980">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-1980">NaN</span></span>  | 

*  <span data-ttu-id="70e32-1981">Adição de decimal:</span><span class="sxs-lookup"><span data-stu-id="70e32-1981">Decimal addition:</span></span>

   ```csharp
   decimal operator +(decimal x, decimal y);
   ```

   <span data-ttu-id="70e32-1982">Se o valor resultante é muito grande para representar o `decimal` formato, um `System.OverflowException` é gerada.</span><span class="sxs-lookup"><span data-stu-id="70e32-1982">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="70e32-1983">A escala do resultado, antes de qualquer arredondamento, é o maior entre as escalas de dois operandos.</span><span class="sxs-lookup"><span data-stu-id="70e32-1983">The scale of the result, before any rounding, is the larger of the scales of the two operands.</span></span>

   <span data-ttu-id="70e32-1984">Adição de decimal é equivalente a usar o operador de adição do tipo `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1984">Decimal addition is equivalent to using the addition operator of type `System.Decimal`.</span></span>

*  <span data-ttu-id="70e32-1985">Adição de enumeração.</span><span class="sxs-lookup"><span data-stu-id="70e32-1985">Enumeration addition.</span></span> <span data-ttu-id="70e32-1986">Implicitamente, a cada tipo de enumeração fornece o seguinte predefinidas operadores, onde `E` é o tipo de enumeração, e `U` é o tipo subjacente de `E`:</span><span class="sxs-lookup"><span data-stu-id="70e32-1986">Every enumeration type implicitly provides the following predefined operators, where `E` is the enum type, and `U` is the underlying type of `E`:</span></span>

   ```csharp
   E operator +(E x, U y);
   E operator +(U x, E y);
   ```

   <span data-ttu-id="70e32-1987">Em tempo de execução, esses operadores são avaliados exatamente como `(E)((U)x + (U)y)`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1987">At run-time these operators are evaluated exactly as `(E)((U)x + (U)y)`.</span></span>

*  <span data-ttu-id="70e32-1988">Concatenação de cadeia de caracteres:</span><span class="sxs-lookup"><span data-stu-id="70e32-1988">String concatenation:</span></span>

   ```csharp
   string operator +(string x, string y);
   string operator +(string x, object y);
   string operator +(object x, string y);
   ```

   <span data-ttu-id="70e32-1989">Essas sobrecargas do binário `+` operador realizam a concatenação de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="70e32-1989">These overloads of the binary `+` operator perform string concatenation.</span></span> <span data-ttu-id="70e32-1990">Se um operando de concatenação de cadeia de caracteres é `null`, uma cadeia de caracteres vazia é substituída.</span><span class="sxs-lookup"><span data-stu-id="70e32-1990">If an operand of string concatenation is `null`, an empty string is substituted.</span></span> <span data-ttu-id="70e32-1991">Caso contrário, qualquer argumento de cadeia de caracteres não é convertido em sua representação de cadeia de caracteres com a invocação virtual `ToString` método herdado do tipo `object`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1991">Otherwise, any non-string argument is converted to its string representation by invoking the virtual `ToString` method inherited from type `object`.</span></span> <span data-ttu-id="70e32-1992">Se `ToString` retorna `null`, uma cadeia de caracteres vazia é substituída.</span><span class="sxs-lookup"><span data-stu-id="70e32-1992">If `ToString` returns `null`, an empty string is substituted.</span></span>

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

   O resultado do operador de concatenação de cadeia de caracteres é uma cadeia de caracteres que consiste em caracteres do operando à esquerda seguido pelos caracteres do operando à direita. O operador de concatenação de cadeia de caracteres nunca retorna um `null` valor. <span data-ttu-id="70e32-1995">Um `System.OutOfMemoryException` poderá ser gerada se não há memória suficiente disponível para alocar a cadeia de caracteres resultante.</span><span class="sxs-lookup"><span data-stu-id="70e32-1995">A `System.OutOfMemoryException` may be thrown if there is not enough memory available to allocate the resulting string.</span></span>

*  <span data-ttu-id="70e32-1996">Combinação de delegado.</span><span class="sxs-lookup"><span data-stu-id="70e32-1996">Delegate combination.</span></span> <span data-ttu-id="70e32-1997">Cada tipo de delegado implicitamente fornece o operador pré-definido seguinte, onde `D` é o tipo de delegado:</span><span class="sxs-lookup"><span data-stu-id="70e32-1997">Every delegate type implicitly provides the following predefined operator, where `D` is the delegate type:</span></span>

   ```csharp
   D operator +(D x, D y);
   ```

   <span data-ttu-id="70e32-1998">O binário `+` operador executa a combinação de delegados quando ambos os operandos são de algum tipo de delegado `D`.</span><span class="sxs-lookup"><span data-stu-id="70e32-1998">The binary `+` operator performs delegate combination when both operands are of some delegate type `D`.</span></span> <span data-ttu-id="70e32-1999">(Se os operandos tiverem tipos diferentes de delegado, ocorrerá um erro em tempo de associação.) Se for o primeiro operando `null`, o resultado da operação é o valor do segundo operando (mesmo que seja também `null`).</span><span class="sxs-lookup"><span data-stu-id="70e32-1999">(If the operands have different delegate types, a binding-time error occurs.) If the first operand is `null`, the result of the operation is the value of the second operand (even if that is also `null`).</span></span> <span data-ttu-id="70e32-2000">Caso contrário, se o segundo operando for `null`, o resultado da operação será o valor do primeiro operando.</span><span class="sxs-lookup"><span data-stu-id="70e32-2000">Otherwise, if the second operand is `null`, then the result of the operation is the value of the first operand.</span></span> <span data-ttu-id="70e32-2001">Caso contrário, o resultado da operação é uma nova instância delegada que, quando invocado, invoca o primeiro operando e, em seguida, invoca o segundo operando.</span><span class="sxs-lookup"><span data-stu-id="70e32-2001">Otherwise, the result of the operation is a new delegate instance that, when invoked, invokes the first operand and then invokes the second operand.</span></span> <span data-ttu-id="70e32-2002">Para obter exemplos de combinação de delegados, consulte [operador de subtração](expressions.md#subtraction-operator) e [invocação de delegado](delegates.md#delegate-invocation).</span><span class="sxs-lookup"><span data-stu-id="70e32-2002">For examples of delegate combination, see [Subtraction operator](expressions.md#subtraction-operator) and [Delegate invocation](delegates.md#delegate-invocation).</span></span> <span data-ttu-id="70e32-2003">Uma vez que `System.Delegate` não é um tipo de delegado `operator` `+` não está definido para ele.</span><span class="sxs-lookup"><span data-stu-id="70e32-2003">Since `System.Delegate` is not a delegate type, `operator` `+` is not defined for it.</span></span>

### <a name="subtraction-operator"></a><span data-ttu-id="70e32-2004">Operador de subtração</span><span class="sxs-lookup"><span data-stu-id="70e32-2004">Subtraction operator</span></span>

<span data-ttu-id="70e32-2005">Para uma operação do formulário `x - y`, resolução de sobrecarga de operador binário ([resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)) é aplicado para selecionar uma implementação do operador específico.</span><span class="sxs-lookup"><span data-stu-id="70e32-2005">For an operation of the form `x - y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="70e32-2006">Os operandos são convertidos nos tipos de parâmetro do operador selecionado e o tipo do resultado é o tipo de retorno do operador.</span><span class="sxs-lookup"><span data-stu-id="70e32-2006">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="70e32-2007">Os operadores de subtração predefinidas são listados abaixo.</span><span class="sxs-lookup"><span data-stu-id="70e32-2007">The predefined subtraction operators are listed below.</span></span> <span data-ttu-id="70e32-2008">Os operadores todos subtrair `y` de `x`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2008">The operators all subtract `y` from `x`.</span></span>

*  <span data-ttu-id="70e32-2009">Subtração de inteiro:</span><span class="sxs-lookup"><span data-stu-id="70e32-2009">Integer subtraction:</span></span>

   ```csharp
   int operator -(int x, int y);
   uint operator -(uint x, uint y);
   long operator -(long x, long y);
   ulong operator -(ulong x, ulong y);
   ```

   <span data-ttu-id="70e32-2010">Em um `checked` contexto, se a diferença está fora do intervalo do tipo de resultado, um `System.OverflowException` é gerada.</span><span class="sxs-lookup"><span data-stu-id="70e32-2010">In a `checked` context, if the difference is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="70e32-2011">Em um `unchecked` contexto, estouros não são relatados e quaisquer bits de ordem superior significativos fora do intervalo do tipo de resultado são descartados.</span><span class="sxs-lookup"><span data-stu-id="70e32-2011">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>

*  <span data-ttu-id="70e32-2012">Subtração de ponto flutuante:</span><span class="sxs-lookup"><span data-stu-id="70e32-2012">Floating-point subtraction:</span></span>

   ```csharp
   float operator -(float x, float y);
   double operator -(double x, double y);
   ```

   <span data-ttu-id="70e32-2013">A diferença é calculada de acordo com as regras de aritmética do IEEE 754.</span><span class="sxs-lookup"><span data-stu-id="70e32-2013">The difference is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="70e32-2014">A tabela a seguir lista os resultados de todas as combinações possíveis de valores Finitas diferente de zero, zeros, infinitos e NaNs.</span><span class="sxs-lookup"><span data-stu-id="70e32-2014">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaNs.</span></span> <span data-ttu-id="70e32-2015">Na tabela, `x` e `y` são valores Finitas diferente de zero, e `z` é o resultado de `x - y`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2015">In the table, `x` and `y` are nonzero finite values, and `z` is the result of `x - y`.</span></span> <span data-ttu-id="70e32-2016">Se `x` e `y` forem iguais, `z` for zero positivo.</span><span class="sxs-lookup"><span data-stu-id="70e32-2016">If `x` and `y` are equal, `z` is positive zero.</span></span> <span data-ttu-id="70e32-2017">Se `x - y` é muito grande para ser representado no tipo de destino, `z` é infinito com o mesmo sinal que `x - y`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2017">If `x - y` is too large to represent in the destination type, `z` is an infinity with the same sign as `x - y`.</span></span>

   |      |      |      |      |      |      |     |
   |:----:|:----:|:----:|:----:|:----:|:----:|:---:|
   | <span data-ttu-id="70e32-2018">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-2018">NaN</span></span>  | <span data-ttu-id="70e32-2019">y</span><span class="sxs-lookup"><span data-stu-id="70e32-2019">y</span></span>    | <span data-ttu-id="70e32-2020">+0</span><span class="sxs-lookup"><span data-stu-id="70e32-2020">+0</span></span>   | <span data-ttu-id="70e32-2021">-0</span><span class="sxs-lookup"><span data-stu-id="70e32-2021">-0</span></span>   | <span data-ttu-id="70e32-2022">+inf</span><span class="sxs-lookup"><span data-stu-id="70e32-2022">+inf</span></span> | <span data-ttu-id="70e32-2023">-inf</span><span class="sxs-lookup"><span data-stu-id="70e32-2023">-inf</span></span> | <span data-ttu-id="70e32-2024">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-2024">NaN</span></span> | 
   | <span data-ttu-id="70e32-2025">x</span><span class="sxs-lookup"><span data-stu-id="70e32-2025">x</span></span>    | <span data-ttu-id="70e32-2026">z</span><span class="sxs-lookup"><span data-stu-id="70e32-2026">z</span></span>    | <span data-ttu-id="70e32-2027">x</span><span class="sxs-lookup"><span data-stu-id="70e32-2027">x</span></span>    | <span data-ttu-id="70e32-2028">x</span><span class="sxs-lookup"><span data-stu-id="70e32-2028">x</span></span>    | <span data-ttu-id="70e32-2029">-inf</span><span class="sxs-lookup"><span data-stu-id="70e32-2029">-inf</span></span> | <span data-ttu-id="70e32-2030">+inf</span><span class="sxs-lookup"><span data-stu-id="70e32-2030">+inf</span></span> | <span data-ttu-id="70e32-2031">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-2031">NaN</span></span> | 
   | <span data-ttu-id="70e32-2032">+0</span><span class="sxs-lookup"><span data-stu-id="70e32-2032">+0</span></span>   | <span data-ttu-id="70e32-2033">-y</span><span class="sxs-lookup"><span data-stu-id="70e32-2033">-y</span></span>   | <span data-ttu-id="70e32-2034">+0</span><span class="sxs-lookup"><span data-stu-id="70e32-2034">+0</span></span>   | <span data-ttu-id="70e32-2035">+0</span><span class="sxs-lookup"><span data-stu-id="70e32-2035">+0</span></span>   | <span data-ttu-id="70e32-2036">-inf</span><span class="sxs-lookup"><span data-stu-id="70e32-2036">-inf</span></span> | <span data-ttu-id="70e32-2037">+inf</span><span class="sxs-lookup"><span data-stu-id="70e32-2037">+inf</span></span> | <span data-ttu-id="70e32-2038">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-2038">NaN</span></span> | 
   | <span data-ttu-id="70e32-2039">-0</span><span class="sxs-lookup"><span data-stu-id="70e32-2039">-0</span></span>   | <span data-ttu-id="70e32-2040">-y</span><span class="sxs-lookup"><span data-stu-id="70e32-2040">-y</span></span>   | <span data-ttu-id="70e32-2041">-0</span><span class="sxs-lookup"><span data-stu-id="70e32-2041">-0</span></span>   | <span data-ttu-id="70e32-2042">+0</span><span class="sxs-lookup"><span data-stu-id="70e32-2042">+0</span></span>   | <span data-ttu-id="70e32-2043">-inf</span><span class="sxs-lookup"><span data-stu-id="70e32-2043">-inf</span></span> | <span data-ttu-id="70e32-2044">+inf</span><span class="sxs-lookup"><span data-stu-id="70e32-2044">+inf</span></span> | <span data-ttu-id="70e32-2045">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-2045">NaN</span></span> | 
   | <span data-ttu-id="70e32-2046">+inf</span><span class="sxs-lookup"><span data-stu-id="70e32-2046">+inf</span></span> | <span data-ttu-id="70e32-2047">+inf</span><span class="sxs-lookup"><span data-stu-id="70e32-2047">+inf</span></span> | <span data-ttu-id="70e32-2048">+inf</span><span class="sxs-lookup"><span data-stu-id="70e32-2048">+inf</span></span> | <span data-ttu-id="70e32-2049">+inf</span><span class="sxs-lookup"><span data-stu-id="70e32-2049">+inf</span></span> | <span data-ttu-id="70e32-2050">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-2050">NaN</span></span>  | <span data-ttu-id="70e32-2051">+inf</span><span class="sxs-lookup"><span data-stu-id="70e32-2051">+inf</span></span> | <span data-ttu-id="70e32-2052">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-2052">NaN</span></span> | 
   | <span data-ttu-id="70e32-2053">-inf</span><span class="sxs-lookup"><span data-stu-id="70e32-2053">-inf</span></span> | <span data-ttu-id="70e32-2054">-inf</span><span class="sxs-lookup"><span data-stu-id="70e32-2054">-inf</span></span> | <span data-ttu-id="70e32-2055">-inf</span><span class="sxs-lookup"><span data-stu-id="70e32-2055">-inf</span></span> | <span data-ttu-id="70e32-2056">-inf</span><span class="sxs-lookup"><span data-stu-id="70e32-2056">-inf</span></span> | <span data-ttu-id="70e32-2057">-inf</span><span class="sxs-lookup"><span data-stu-id="70e32-2057">-inf</span></span> | <span data-ttu-id="70e32-2058">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-2058">NaN</span></span>  | <span data-ttu-id="70e32-2059">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-2059">NaN</span></span> | 
   | <span data-ttu-id="70e32-2060">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-2060">NaN</span></span>  | <span data-ttu-id="70e32-2061">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-2061">NaN</span></span>  | <span data-ttu-id="70e32-2062">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-2062">NaN</span></span>  | <span data-ttu-id="70e32-2063">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-2063">NaN</span></span>  | <span data-ttu-id="70e32-2064">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-2064">NaN</span></span>  | <span data-ttu-id="70e32-2065">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-2065">NaN</span></span>  | <span data-ttu-id="70e32-2066">NaN</span><span class="sxs-lookup"><span data-stu-id="70e32-2066">NaN</span></span> | 

*  <span data-ttu-id="70e32-2067">Subtração decimal:</span><span class="sxs-lookup"><span data-stu-id="70e32-2067">Decimal subtraction:</span></span>

   ```csharp
   decimal operator -(decimal x, decimal y);
   ```

   <span data-ttu-id="70e32-2068">Se o valor resultante é muito grande para representar o `decimal` formato, um `System.OverflowException` é gerada.</span><span class="sxs-lookup"><span data-stu-id="70e32-2068">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="70e32-2069">A escala do resultado, antes de qualquer arredondamento, é o maior entre as escalas de dois operandos.</span><span class="sxs-lookup"><span data-stu-id="70e32-2069">The scale of the result, before any rounding, is the larger of the scales of the two operands.</span></span>

   <span data-ttu-id="70e32-2070">Subtração de decimal é equivalente a usar o operador de subtração do tipo `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2070">Decimal subtraction is equivalent to using the subtraction operator of type `System.Decimal`.</span></span>

*  <span data-ttu-id="70e32-2071">Subtração de enumeração.</span><span class="sxs-lookup"><span data-stu-id="70e32-2071">Enumeration subtraction.</span></span> <span data-ttu-id="70e32-2072">Cada tipo de enumeração implicitamente fornece o operador pré-definido seguinte, onde `E` é o tipo de enumeração, e `U` é o tipo subjacente de `E`:</span><span class="sxs-lookup"><span data-stu-id="70e32-2072">Every enumeration type implicitly provides the following predefined operator, where `E` is the enum type, and `U` is the underlying type of `E`:</span></span>

   ```csharp
   U operator -(E x, E y);
   ```

   <span data-ttu-id="70e32-2073">Esse operador é avaliado exatamente como `(U)((U)x - (U)y)`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2073">This operator is evaluated exactly as `(U)((U)x - (U)y)`.</span></span> <span data-ttu-id="70e32-2074">Em outras palavras, o operador calcula a diferença entre os valores ordinais das `x` e `y`, e o tipo do resultado é o tipo subjacente da enumeração.</span><span class="sxs-lookup"><span data-stu-id="70e32-2074">In other words, the operator computes the difference between the ordinal values of `x` and `y`, and the type of the result is the underlying type of the enumeration.</span></span>

   ```csharp
   E operator -(E x, U y);
   ```

   <span data-ttu-id="70e32-2075">Esse operador é avaliado exatamente como `(E)((U)x - y)`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2075">This operator is evaluated exactly as `(E)((U)x - y)`.</span></span> <span data-ttu-id="70e32-2076">Em outras palavras, o operador subtrai um valor do tipo subjacente da enumeração, que rende um valor da enumeração.</span><span class="sxs-lookup"><span data-stu-id="70e32-2076">In other words, the operator subtracts a value from the underlying type of the enumeration, yielding a value of the enumeration.</span></span>

*  <span data-ttu-id="70e32-2077">Remoção de delegado.</span><span class="sxs-lookup"><span data-stu-id="70e32-2077">Delegate removal.</span></span> <span data-ttu-id="70e32-2078">Cada tipo de delegado implicitamente fornece o operador pré-definido seguinte, onde `D` é o tipo de delegado:</span><span class="sxs-lookup"><span data-stu-id="70e32-2078">Every delegate type implicitly provides the following predefined operator, where `D` is the delegate type:</span></span>

   ```csharp
   D operator -(D x, D y);
   ```

   <span data-ttu-id="70e32-2079">O binário `-` operador executa a remoção de delegado quando ambos os operandos são de algum tipo de delegado `D`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2079">The binary `-` operator performs delegate removal when both operands are of some delegate type `D`.</span></span> <span data-ttu-id="70e32-2080">Se os operandos tiverem tipos diferentes de delegado, ocorrerá um erro de tempo de associação.</span><span class="sxs-lookup"><span data-stu-id="70e32-2080">If the operands have different delegate types, a binding-time error occurs.</span></span> <span data-ttu-id="70e32-2081">Se for o primeiro operando `null`, o resultado da operação é `null`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2081">If the first operand is `null`, the result of the operation is `null`.</span></span> <span data-ttu-id="70e32-2082">Caso contrário, se o segundo operando for `null`, o resultado da operação será o valor do primeiro operando.</span><span class="sxs-lookup"><span data-stu-id="70e32-2082">Otherwise, if the second operand is `null`, then the result of the operation is the value of the first operand.</span></span> <span data-ttu-id="70e32-2083">Caso contrário, ambos os operandos representam listas de invocação ([declarações de delegado](delegates.md#delegate-declarations)) ter uma ou mais entradas e o resultado é uma nova lista de invocação, consistindo de lista do primeiro operando com entradas do segundo operando removidas do forneceu a lista do segundo operando é uma sublista contígua adequada do primeiro.</span><span class="sxs-lookup"><span data-stu-id="70e32-2083">Otherwise, both operands represent invocation lists ([Delegate declarations](delegates.md#delegate-declarations)) having one or more entries, and the result is a new invocation list consisting of the first operand's list with the second operand's entries removed from it, provided the second operand's list is a proper contiguous sublist of the first's.</span></span>     <span data-ttu-id="70e32-2084">(Para determinar igualdade sublista, as entradas correspondentes são comparadas como para o operador de igualdade de delegado ([delegar operadores de igualdade](expressions.md#delegate-equality-operators)).) Caso contrário, o resultado é o valor do operando à esquerda.</span><span class="sxs-lookup"><span data-stu-id="70e32-2084">(To determine sublist equality, corresponding entries are compared as for the delegate equality operator ([Delegate equality operators](expressions.md#delegate-equality-operators)).) Otherwise, the result is the value of the left operand.</span></span> <span data-ttu-id="70e32-2085">Nenhuma das listas dos operandos é alterada no processo.</span><span class="sxs-lookup"><span data-stu-id="70e32-2085">Neither of the operands' lists is changed in the process.</span></span> <span data-ttu-id="70e32-2086">Se a lista do segundo operando corresponder a várias sublistas contíguos entradas da lista do primeiro operando, a sublista de correspondência mais à direita das entradas contíguas é removida.</span><span class="sxs-lookup"><span data-stu-id="70e32-2086">If the second operand's list matches multiple sublists of contiguous entries in the first operand's list, the right-most matching sublist of contiguous entries is removed.</span></span> <span data-ttu-id="70e32-2087">Se os resultados da remoção em uma lista vazia, o resultado será `null`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2087">If removal results in an empty list, the result is `null`.</span></span> <span data-ttu-id="70e32-2088">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="70e32-2088">For example:</span></span>

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

## <a name="shift-operators"></a><span data-ttu-id="70e32-2089">Operadores shift</span><span class="sxs-lookup"><span data-stu-id="70e32-2089">Shift operators</span></span>

<span data-ttu-id="70e32-2090">O `<<` e `>>` operadores são usados para executar operações de mudança de bits.</span><span class="sxs-lookup"><span data-stu-id="70e32-2090">The `<<` and `>>` operators are used to perform bit shifting operations.</span></span>

```antlr
shift_expression
    : additive_expression
    | shift_expression '<<' additive_expression
    | shift_expression right_shift additive_expression
    ;
```

<span data-ttu-id="70e32-2091">Se um operando de um *shift_expression* tem o tipo de tempo de compilação `dynamic`, em seguida, a expressão está associada dinamicamente ([vinculação dinâmica](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="70e32-2091">If an operand of a *shift_expression* has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="70e32-2092">Nesse caso é o tipo de tempo de compilação da expressão `dynamic`, e a resolução descrita a seguir ocorrerá em tempo de execução usando o tipo de tempo de execução desses operandos que tenham o tipo de tempo de compilação `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2092">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="70e32-2093">Para uma operação do formulário `x << count` ou `x >> count`, resolução de sobrecarga de operador binário ([resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)) é aplicado para selecionar uma implementação do operador específico.</span><span class="sxs-lookup"><span data-stu-id="70e32-2093">For an operation of the form `x << count` or `x >> count`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="70e32-2094">Os operandos são convertidos nos tipos de parâmetro do operador selecionado e o tipo do resultado é o tipo de retorno do operador.</span><span class="sxs-lookup"><span data-stu-id="70e32-2094">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="70e32-2095">Ao declarar um operador de deslocamento sobrecarregado, o tipo do primeiro operando sempre deve ser a classe ou struct que contém a declaração do operador e o tipo do segundo operando deve ser sempre `int`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2095">When declaring an overloaded shift operator, the type of the first operand must always be the class or struct containing the operator declaration, and the type of the second operand must always be `int`.</span></span>

<span data-ttu-id="70e32-2096">Os operadores shift predefinidas são listados abaixo.</span><span class="sxs-lookup"><span data-stu-id="70e32-2096">The predefined shift operators are listed below.</span></span>

*  <span data-ttu-id="70e32-2097">Usar SHIFT-left:</span><span class="sxs-lookup"><span data-stu-id="70e32-2097">Shift left:</span></span>

   ```csharp
   int operator <<(int x, int count);
   uint operator <<(uint x, int count);
   long operator <<(long x, int count);
   ulong operator <<(ulong x, int count);
   ```

   <span data-ttu-id="70e32-2098">O `<<` turnos de operador `x` à esquerda por um número de bits calculado conforme descrito abaixo.</span><span class="sxs-lookup"><span data-stu-id="70e32-2098">The `<<` operator shifts `x` left by a number of bits computed as described below.</span></span>

   <span data-ttu-id="70e32-2099">Os bits de ordem superior fora do intervalo do tipo de resultado `x` são descartados, os bits restantes são deslocados para a esquerda e as posições de bits vazios de ordem inferior são definidas como zero.</span><span class="sxs-lookup"><span data-stu-id="70e32-2099">The high-order bits outside the range of the result type of `x` are discarded, the remaining bits are shifted left, and the low-order empty bit positions are set to zero.</span></span>

*  <span data-ttu-id="70e32-2100">SHIFT direito:</span><span class="sxs-lookup"><span data-stu-id="70e32-2100">Shift right:</span></span>

   ```csharp
   int operator >>(int x, int count);
   uint operator >>(uint x, int count);
   long operator >>(long x, int count);
   ulong operator >>(ulong x, int count);
   ```

   <span data-ttu-id="70e32-2101">O `>>` turnos de operador `x` à direita por um número de bits calculado conforme descrito abaixo.</span><span class="sxs-lookup"><span data-stu-id="70e32-2101">The `>>` operator shifts `x` right by a number of bits computed as described below.</span></span>

   <span data-ttu-id="70e32-2102">Quando `x` é do tipo `int` ou `long`, os bits de ordem inferior do `x` são descartados, os bits restantes são deslocados para a direita, e as posições de bits vazios de ordem superior são definidas como zero se `x` é não negativo e será definida para um, se `x` é negativo.</span><span class="sxs-lookup"><span data-stu-id="70e32-2102">When `x` is of type `int` or `long`, the low-order bits of `x` are discarded, the remaining bits are shifted right, and the high-order empty bit positions are set to zero if `x` is non-negative and set to one if `x` is negative.</span></span>

   <span data-ttu-id="70e32-2103">Quando `x` é do tipo `uint` ou `ulong`, os bits de ordem inferior de `x` são descartados, os bits restantes são deslocados para a direita, e as posições de bits vazios de ordem superior são definidas como zero.</span><span class="sxs-lookup"><span data-stu-id="70e32-2103">When `x` is of type `uint` or `ulong`, the low-order bits of `x` are discarded, the remaining bits are shifted right, and the high-order empty bit positions are set to zero.</span></span>

<span data-ttu-id="70e32-2104">Para os operadores predefinidos, o número de bits a deslocar é calculado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="70e32-2104">For the predefined operators, the number of bits to shift is computed as follows:</span></span>

*  <span data-ttu-id="70e32-2105">Quando o tipo de `x` está `int` ou `uint`, a contagem de shift é determinada pelos cinco bits inferiores de `count`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2105">When the type of `x` is `int` or `uint`, the shift count is given by the low-order five bits of `count`.</span></span> <span data-ttu-id="70e32-2106">Em outras palavras, a contagem de deslocamento é computada a partir `count & 0x1F`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2106">In other words, the shift count is computed from `count & 0x1F`.</span></span>
*  <span data-ttu-id="70e32-2107">Quando o tipo de `x` está `long` ou `ulong`, a contagem de deslocamento será determinada pelos seis bits inferiores de `count`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2107">When the type of `x` is `long` or `ulong`, the shift count is given by the low-order six bits of `count`.</span></span> <span data-ttu-id="70e32-2108">Em outras palavras, a contagem de deslocamento é computada a partir `count & 0x3F`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2108">In other words, the shift count is computed from `count & 0x3F`.</span></span>

<span data-ttu-id="70e32-2109">Se a contagem de deslocamento resultante for zero, os operadores shift simplesmente retornam o valor de `x`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2109">If the resulting shift count is zero, the shift operators simply return the value of `x`.</span></span>

<span data-ttu-id="70e32-2110">Operações de deslocamento nunca causam estouros e produzem os mesmos resultados no `checked` e `unchecked` contextos.</span><span class="sxs-lookup"><span data-stu-id="70e32-2110">Shift operations never cause overflows and produce the same results in `checked` and `unchecked` contexts.</span></span>

<span data-ttu-id="70e32-2111">Quando o operando da esquerda a `>>` operador é de um tipo integral com sinal, o operador executa um deslocamento aritmético à direita no qual o valor do bit mais significativo (o bit de sinal) do operando é propagado para as posições de bit de ordem superior vazio.</span><span class="sxs-lookup"><span data-stu-id="70e32-2111">When the left operand of the `>>` operator is of a signed integral type, the operator performs an arithmetic shift right wherein the value of the most significant bit (the sign bit) of the operand is propagated to the high-order empty bit positions.</span></span> <span data-ttu-id="70e32-2112">Quando o operando da esquerda a `>>` operador é de um tipo integral sem sinal, o operador executa um deslocamento lógico à direita no qual as posições de bit de ordem superior vazias são sempre definidas como zero.</span><span class="sxs-lookup"><span data-stu-id="70e32-2112">When the left operand of the `>>` operator is of an unsigned integral type, the operator performs a logical shift right wherein high-order empty bit positions are always set to zero.</span></span> <span data-ttu-id="70e32-2113">Para executar a operação oposta do que é inferido do tipo de operando, conversões explícitas podem ser usados.</span><span class="sxs-lookup"><span data-stu-id="70e32-2113">To perform the opposite operation of that inferred from the operand type, explicit casts can be used.</span></span> <span data-ttu-id="70e32-2114">Por exemplo, se `x` é uma variável do tipo `int`, a operação `unchecked((int)((uint)x >> y))` executa um deslocamento lógico à direita do `x`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2114">For example, if `x` is a variable of type `int`, the operation `unchecked((int)((uint)x >> y))` performs a logical shift right of `x`.</span></span>

## <a name="relational-and-type-testing-operators"></a><span data-ttu-id="70e32-2115">Operadores relacionais e de teste de tipo</span><span class="sxs-lookup"><span data-stu-id="70e32-2115">Relational and type-testing operators</span></span>

<span data-ttu-id="70e32-2116">O `==`, `!=`, `<`, `>`, `<=`, `>=`, `is` e `as` operadores são chamados de operadores relacionais e de teste de tipo.</span><span class="sxs-lookup"><span data-stu-id="70e32-2116">The `==`, `!=`, `<`, `>`, `<=`, `>=`, `is` and `as` operators are called the relational and type-testing operators.</span></span>

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

<span data-ttu-id="70e32-2117">O `is` operador é descrito em [a é o operador](expressions.md#the-is-operator) e o `as` operador é descrito na [o operador](expressions.md#the-as-operator).</span><span class="sxs-lookup"><span data-stu-id="70e32-2117">The `is` operator is described in [The is operator](expressions.md#the-is-operator) and the `as` operator is described in [The as operator](expressions.md#the-as-operator).</span></span>

<span data-ttu-id="70e32-2118">O `==`, `!=`, `<`, `>`, `<=` e `>=` operadores são ***operadores de comparação***.</span><span class="sxs-lookup"><span data-stu-id="70e32-2118">The `==`, `!=`, `<`, `>`, `<=` and `>=` operators are ***comparison operators***.</span></span>

<span data-ttu-id="70e32-2119">Se um operando do operador de comparação tem o tipo de tempo de compilação `dynamic`, em seguida, a expressão está associada dinamicamente ([vinculação dinâmica](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="70e32-2119">If an operand of a comparison operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="70e32-2120">Nesse caso é o tipo de tempo de compilação da expressão `dynamic`, e a resolução descrita a seguir ocorrerá em tempo de execução usando o tipo de tempo de execução desses operandos que tenham o tipo de tempo de compilação `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2120">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="70e32-2121">Para uma operação do formulário `x` *op* `y`, onde *op* é um operador de comparação, a resolução de sobrecarga ([deresoluçãodesobrecargadeoperadorbinário](expressions.md#binary-operator-overload-resolution)) é aplicado para selecionar uma implementação do operador específico.</span><span class="sxs-lookup"><span data-stu-id="70e32-2121">For an operation of the form `x` *op* `y`, where *op* is a comparison operator, overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="70e32-2122">Os operandos são convertidos nos tipos de parâmetro do operador selecionado e o tipo do resultado é o tipo de retorno do operador.</span><span class="sxs-lookup"><span data-stu-id="70e32-2122">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="70e32-2123">Os operadores de comparação predefinidos são descritos nas seções a seguir.</span><span class="sxs-lookup"><span data-stu-id="70e32-2123">The predefined comparison operators are described in the following sections.</span></span> <span data-ttu-id="70e32-2124">Todos os operadores de comparação predefinido retornam um resultado do tipo `bool`, conforme descrito na tabela a seguir.</span><span class="sxs-lookup"><span data-stu-id="70e32-2124">All predefined comparison operators return a result of type `bool`, as described in the following table.</span></span>


| <span data-ttu-id="70e32-2125">__operação__</span><span class="sxs-lookup"><span data-stu-id="70e32-2125">__Operation__</span></span> | <span data-ttu-id="70e32-2126">__Result__</span><span class="sxs-lookup"><span data-stu-id="70e32-2126">__Result__</span></span>                                                       |
|---------------|------------------------------------------------------------------|
| `x == y`      | <span data-ttu-id="70e32-2127">`true` Se `x` é igual a `y`, `false` caso contrário,</span><span class="sxs-lookup"><span data-stu-id="70e32-2127">`true` if `x` is equal to `y`, `false` otherwise</span></span>                 | 
| `x != y`      | <span data-ttu-id="70e32-2128">`true` Se `x` não é igual a `y`, `false` caso contrário,</span><span class="sxs-lookup"><span data-stu-id="70e32-2128">`true` if `x` is not equal to `y`, `false` otherwise</span></span>             | 
| `x < y`       | <span data-ttu-id="70e32-2129">`true` Se `x` é menor que `y`, `false` caso contrário,</span><span class="sxs-lookup"><span data-stu-id="70e32-2129">`true` if `x` is less than `y`, `false` otherwise</span></span>                | 
| `x > y`       | <span data-ttu-id="70e32-2130">`true` Se `x` é maior que `y`, `false` caso contrário,</span><span class="sxs-lookup"><span data-stu-id="70e32-2130">`true` if `x` is greater than `y`, `false` otherwise</span></span>             | 
| `x <= y`      | <span data-ttu-id="70e32-2131">`true` Se `x` é menor que ou igual a `y`, `false` caso contrário,</span><span class="sxs-lookup"><span data-stu-id="70e32-2131">`true` if `x` is less than or equal to `y`, `false` otherwise</span></span>    | 
| `x >= y`      | <span data-ttu-id="70e32-2132">`true` Se `x` é maior que ou igual a `y`, `false` caso contrário,</span><span class="sxs-lookup"><span data-stu-id="70e32-2132">`true` if `x` is greater than or equal to `y`, `false` otherwise</span></span> | 

### <a name="integer-comparison-operators"></a><span data-ttu-id="70e32-2133">Operadores de comparação de inteiro</span><span class="sxs-lookup"><span data-stu-id="70e32-2133">Integer comparison operators</span></span>

<span data-ttu-id="70e32-2134">Os operadores de comparação de inteiro predefinidos são:</span><span class="sxs-lookup"><span data-stu-id="70e32-2134">The predefined integer comparison operators are:</span></span>
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

<span data-ttu-id="70e32-2135">Cada um desses operadores compara os valores numéricos dos operandos dois inteiros e retorna um `bool` valor que indica se a relação específica `true` ou `false`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2135">Each of these operators compares the numeric values of the two integer operands and returns a `bool` value that indicates whether the particular relation is `true` or `false`.</span></span>

### <a name="floating-point-comparison-operators"></a><span data-ttu-id="70e32-2136">Operadores de comparação de ponto flutuante</span><span class="sxs-lookup"><span data-stu-id="70e32-2136">Floating-point comparison operators</span></span>

<span data-ttu-id="70e32-2137">Os operadores de comparação de ponto flutuante predefinidos são:</span><span class="sxs-lookup"><span data-stu-id="70e32-2137">The predefined floating-point comparison operators are:</span></span>
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

<span data-ttu-id="70e32-2138">Os operadores comparam os operandos de acordo com as regras do padrão IEEE 754:</span><span class="sxs-lookup"><span data-stu-id="70e32-2138">The operators compare the operands according to the rules of the IEEE 754 standard:</span></span>

*  <span data-ttu-id="70e32-2139">Se qualquer operando for NaN, o resultado será `false` para todos os operadores exceto `!=`, para que o resultado é `true`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2139">If either operand is NaN, the result is `false` for all operators except `!=`, for which the result is `true`.</span></span> <span data-ttu-id="70e32-2140">Para qualquer dois operandos, `x != y` sempre produz o mesmo resultado que `!(x == y)`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2140">For any two operands, `x != y` always produces the same result as `!(x == y)`.</span></span> <span data-ttu-id="70e32-2141">No entanto, quando um ou ambos os operandos são NaN, a `<`, `>`, `<=`, e `>=` operadores não produzem os mesmos resultados que a negação lógica do operador oposto.</span><span class="sxs-lookup"><span data-stu-id="70e32-2141">However, when one or both operands are NaN, the `<`, `>`, `<=`, and `>=` operators do not produce the same results as the logical negation of the opposite operator.</span></span> <span data-ttu-id="70e32-2142">Por exemplo, se qualquer uma de `x` e `y` for NaN, em seguida, `x < y` é `false`, mas `!(x >= y)` é `true`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2142">For example, if either of `x` and `y` is NaN, then `x < y` is `false`, but `!(x >= y)` is `true`.</span></span>
*  <span data-ttu-id="70e32-2143">Quando nenhum dos operandos for NaN, os operadores comparam os valores dos dois operandos de ponto flutuantes em relação a ordenação</span><span class="sxs-lookup"><span data-stu-id="70e32-2143">When neither operand is NaN, the operators compare the values of the two floating-point operands with respect to the ordering</span></span>

   ```
   -inf < -max < ... < -min < -0.0 == +0.0 < +min < ... < +max < +inf
   ```

   <span data-ttu-id="70e32-2144">em que `min` e `max` são os menores e maiores finitos valores positivos que podem ser representados no formato de ponto flutuante fornecido.</span><span class="sxs-lookup"><span data-stu-id="70e32-2144">where `min` and `max` are the smallest and largest positive finite values that can be represented in the given floating-point format.</span></span> <span data-ttu-id="70e32-2145">Efeitos notáveis essa ordenação são:</span><span class="sxs-lookup"><span data-stu-id="70e32-2145">Notable effects of this ordering are:</span></span>
   * <span data-ttu-id="70e32-2146">Zeros positivos e negativos são considerados iguais.</span><span class="sxs-lookup"><span data-stu-id="70e32-2146">Negative and positive zeros are considered equal.</span></span>
   * <span data-ttu-id="70e32-2147">Um número infinito negativo é considerado menor do que todos os outros valores, mas igual a outro infinito negativo.</span><span class="sxs-lookup"><span data-stu-id="70e32-2147">A negative infinity is considered less than all other values, but equal to another negative infinity.</span></span>
   * <span data-ttu-id="70e32-2148">Um número infinito positivo é considerado maior do que todos os outros valores, mas igual ao outro infinito positivo.</span><span class="sxs-lookup"><span data-stu-id="70e32-2148">A positive infinity is considered greater than all other values, but equal to another positive infinity.</span></span>

### <a name="decimal-comparison-operators"></a><span data-ttu-id="70e32-2149">Operadores de comparação decimal</span><span class="sxs-lookup"><span data-stu-id="70e32-2149">Decimal comparison operators</span></span>

<span data-ttu-id="70e32-2150">Os operadores de comparação de decimal predefinidos são:</span><span class="sxs-lookup"><span data-stu-id="70e32-2150">The predefined decimal comparison operators are:</span></span>
```csharp
bool operator ==(decimal x, decimal y);
bool operator !=(decimal x, decimal y);
bool operator <(decimal x, decimal y);
bool operator >(decimal x, decimal y);
bool operator <=(decimal x, decimal y);
bool operator >=(decimal x, decimal y);
```

<span data-ttu-id="70e32-2151">Cada um desses operadores compara os valores numéricos dos dois operandos de decimais e retorna um `bool` valor que indica se a relação específica `true` ou `false`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2151">Each of these operators compares the numeric values of the two decimal operands and returns a `bool` value that indicates whether the particular relation is `true` or `false`.</span></span> <span data-ttu-id="70e32-2152">Cada comparação decimal é equivalente a usar o operador de igualdade do tipo ou correspondente relacional `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2152">Each decimal comparison is equivalent to using the corresponding relational or equality operator of type `System.Decimal`.</span></span>

### <a name="boolean-equality-operators"></a><span data-ttu-id="70e32-2153">Operadores de igualdade booliana</span><span class="sxs-lookup"><span data-stu-id="70e32-2153">Boolean equality operators</span></span>

<span data-ttu-id="70e32-2154">Os operadores de igualdade booliana predefinidos são:</span><span class="sxs-lookup"><span data-stu-id="70e32-2154">The predefined boolean equality operators are:</span></span>
```csharp
bool operator ==(bool x, bool y);
bool operator !=(bool x, bool y);
```

<span data-ttu-id="70e32-2155">O resultado de `==` está `true` se ambos `x` e `y` são `true` ou se ambos os `x` e `y` são `false`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2155">The result of `==` is `true` if both `x` and `y` are `true` or if both `x` and `y` are `false`.</span></span> <span data-ttu-id="70e32-2156">Caso contrário, o resultado é `false`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2156">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="70e32-2157">O resultado de `!=` está `false` se ambos `x` e `y` são `true` ou se ambos os `x` e `y` são `false`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2157">The result of `!=` is `false` if both `x` and `y` are `true` or if both `x` and `y` are `false`.</span></span> <span data-ttu-id="70e32-2158">Caso contrário, o resultado é `true`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2158">Otherwise, the result is `true`.</span></span> <span data-ttu-id="70e32-2159">Quando os operandos forem do tipo `bool`, o `!=` operador produz o mesmo resultado que o `^` operador.</span><span class="sxs-lookup"><span data-stu-id="70e32-2159">When the operands are of type `bool`, the `!=` operator produces the same result as the `^` operator.</span></span>

### <a name="enumeration-comparison-operators"></a><span data-ttu-id="70e32-2160">Operadores de comparação de enumeração</span><span class="sxs-lookup"><span data-stu-id="70e32-2160">Enumeration comparison operators</span></span>

<span data-ttu-id="70e32-2161">Cada tipo de enumeração implicitamente fornece os seguintes operadores de comparação predefinidos:</span><span class="sxs-lookup"><span data-stu-id="70e32-2161">Every enumeration type implicitly provides the following predefined comparison operators:</span></span>
```csharp
bool operator ==(E x, E y);
bool operator !=(E x, E y);
bool operator <(E x, E y);
bool operator >(E x, E y);
bool operator <=(E x, E y);
bool operator >=(E x, E y);
```

<span data-ttu-id="70e32-2162">O resultado da avaliação `x op y`, onde `x` e `y` são expressões de um tipo de enumeração `E` com um tipo subjacente `U`, e `op` é um dos operadores de comparação, é exatamente o mesmo que Avaliando `((U)x) op ((U)y)`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2162">The result of evaluating `x op y`, where `x` and `y` are expressions of an enumeration type `E` with an underlying type `U`, and `op` is one of the comparison operators, is exactly the same as evaluating `((U)x) op ((U)y)`.</span></span> <span data-ttu-id="70e32-2163">Em outras palavras, os operadores de comparação de tipo de enumeração simplesmente comparam os valores integrais subjacentes dos dois operandos.</span><span class="sxs-lookup"><span data-stu-id="70e32-2163">In other words, the enumeration type comparison operators simply compare the underlying integral values of the two operands.</span></span>

### <a name="reference-type-equality-operators"></a><span data-ttu-id="70e32-2164">Operadores de igualdade de tipo de referência</span><span class="sxs-lookup"><span data-stu-id="70e32-2164">Reference type equality operators</span></span>

<span data-ttu-id="70e32-2165">Os operadores de igualdade do tipo de referência predefinidos são:</span><span class="sxs-lookup"><span data-stu-id="70e32-2165">The predefined reference type equality operators are:</span></span>
```csharp
bool operator ==(object x, object y);
bool operator !=(object x, object y);
```

<span data-ttu-id="70e32-2166">Os operadores retornam o resultado de comparar as duas referências para igualdade ou não igualdade.</span><span class="sxs-lookup"><span data-stu-id="70e32-2166">The operators return the result of comparing the two references for equality or non-equality.</span></span>

<span data-ttu-id="70e32-2167">Uma vez que os operadores de igualdade do tipo de referência predefinidos aceitam operandos do tipo `object`, eles se aplicam a todos os tipos que não declaram aplicáveis `operator ==` e `operator !=` membros.</span><span class="sxs-lookup"><span data-stu-id="70e32-2167">Since the predefined reference type equality operators accept operands of type `object`, they apply to all types that do not declare applicable `operator ==` and `operator !=` members.</span></span> <span data-ttu-id="70e32-2168">Por outro lado, qualquer operadores de igualdade aplicável definidos pelo usuário efetivamente ocultam os operadores de igualdade do tipo de referência predefinidos.</span><span class="sxs-lookup"><span data-stu-id="70e32-2168">Conversely, any applicable user-defined equality operators effectively hide the predefined reference type equality operators.</span></span>

<span data-ttu-id="70e32-2169">Os operadores de igualdade do tipo de referência predefinidos exigem um destes procedimentos:</span><span class="sxs-lookup"><span data-stu-id="70e32-2169">The predefined reference type equality operators require one of the following:</span></span>

*  <span data-ttu-id="70e32-2170">Ambos os operandos forem um valor de um tipo conhecido por ser um *reference_type* ou o literal `null`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2170">Both operands are a value of a type known to be a *reference_type* or the literal `null`.</span></span> <span data-ttu-id="70e32-2171">Além disso, uma conversão de referência explícita ([conversões de referência explícita](conversions.md#explicit-reference-conversions)) existe do tipo de ambos os operandos para o tipo de outro operando.</span><span class="sxs-lookup"><span data-stu-id="70e32-2171">Furthermore, an explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) exists from the type of either operand to the type of the other operand.</span></span>
*  <span data-ttu-id="70e32-2172">Um operando for um valor do tipo `T` onde `T` é um *type_parameter* e o outro operando é o literal `null`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2172">One operand is a value of type `T` where `T` is a *type_parameter* and the other operand is the literal `null`.</span></span> <span data-ttu-id="70e32-2173">Além disso `T` não tem a restrição de tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="70e32-2173">Furthermore `T` does not have the value type constraint.</span></span>

<span data-ttu-id="70e32-2174">A menos que uma das seguintes condições forem verdadeiras, ocorrerá um erro de tempo de associação.</span><span class="sxs-lookup"><span data-stu-id="70e32-2174">Unless one of these conditions are true, a binding-time error occurs.</span></span> <span data-ttu-id="70e32-2175">Implicações importantes dessas regras são:</span><span class="sxs-lookup"><span data-stu-id="70e32-2175">Notable implications of these rules are:</span></span>

*  <span data-ttu-id="70e32-2176">Ele é um erro de tempo de associação a usar os operadores de igualdade do tipo de referência predefinidos para comparar duas referências que são conhecidas por ser diferente em tempo de associação.</span><span class="sxs-lookup"><span data-stu-id="70e32-2176">It is a binding-time error to use the predefined reference type equality operators to compare two references that are known to be different at binding-time.</span></span> <span data-ttu-id="70e32-2177">Por exemplo, se os tipos de tempo de associação de operandos são dois tipos de classe `A` e `B`e caso nem `A` nem `B` deriva a outra, em seguida, seria impossível para os dois operandos referenciar o mesmo objeto.</span><span class="sxs-lookup"><span data-stu-id="70e32-2177">For example, if the binding-time types of the operands are two class types `A` and `B`, and if neither `A` nor `B` derives from the other, then it would be impossible for the two operands to reference the same object.</span></span> <span data-ttu-id="70e32-2178">Portanto, a operação é considerada um erro de tempo de associação.</span><span class="sxs-lookup"><span data-stu-id="70e32-2178">Thus, the operation is considered a binding-time error.</span></span>
*  <span data-ttu-id="70e32-2179">Os operadores de igualdade do tipo de referência predefinidos não permitem valor operandos do tipo a ser comparado.</span><span class="sxs-lookup"><span data-stu-id="70e32-2179">The predefined reference type equality operators do not permit value type operands to be compared.</span></span> <span data-ttu-id="70e32-2180">Portanto, a menos que um tipo de struct declara seus próprios operadores de igualdade, não é possível comparar os valores desse tipo de struct.</span><span class="sxs-lookup"><span data-stu-id="70e32-2180">Therefore, unless a struct type declares its own equality operators, it is not possible to compare values of that struct type.</span></span>
*  <span data-ttu-id="70e32-2181">Os operadores de igualdade do tipo de referência predefinidos nunca causam operações de conversão boxing ocorra para seus operandos.</span><span class="sxs-lookup"><span data-stu-id="70e32-2181">The predefined reference type equality operators never cause boxing operations to occur for their operands.</span></span> <span data-ttu-id="70e32-2182">Ele não teria sentido executar essas operações de conversão boxing, já que as referências às instâncias recém-alocada demarcadas necessariamente seriam diferente de todas as outras referências.</span><span class="sxs-lookup"><span data-stu-id="70e32-2182">It would be meaningless to perform such boxing operations, since references to the newly allocated boxed instances would necessarily differ from all other references.</span></span>
*  <span data-ttu-id="70e32-2183">Se um operando de um tipo de parâmetro de tipo `T` é comparado ao `null`e o tipo de tempo de execução do `T` é um tipo de valor, o resultado da comparação é `false`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2183">If an operand of a type parameter type `T` is compared to `null`, and the run-time type of `T` is a value type, the result of the comparison is `false`.</span></span>

<span data-ttu-id="70e32-2184">O exemplo a seguir verifica se um argumento de um tipo de parâmetro de tipo sem restrição é `null`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2184">The following example checks whether an argument of an unconstrained type parameter type is `null`.</span></span>
```csharp
class C<T>
{
    void F(T x) {
        if (x == null) throw new ArgumentNullException();
        ...
    }
}
```

<span data-ttu-id="70e32-2185">O `x == null` constructo é permitido, embora `T` pode representar um tipo de valor e o resultado é simplesmente definido para ser `false` quando `T` é um tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="70e32-2185">The `x == null` construct is permitted even though `T` could represent a value type, and the result is simply defined to be `false` when `T` is a value type.</span></span>

<span data-ttu-id="70e32-2186">Para uma operação do formulário `x == y` ou `x != y`, se qualquer aplicável `operator ==` ou `operator !=` existir, a resolução de sobrecarga de operador ([resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)) selecionará as regras que operador em vez do operador de igualdade de tipo de referência predefinidos.</span><span class="sxs-lookup"><span data-stu-id="70e32-2186">For an operation of the form `x == y` or `x != y`, if any applicable `operator ==` or `operator !=` exists, the operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) rules will select that operator instead of the predefined reference type equality operator.</span></span> <span data-ttu-id="70e32-2187">No entanto, sempre é possível selecionar o operador de igualdade do tipo de referência predefinidos convertendo explicitamente um ou ambos os operandos para o tipo `object`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2187">However, it is always possible to select the predefined reference type equality operator by explicitly casting one or both of the operands to type `object`.</span></span> <span data-ttu-id="70e32-2188">O exemplo</span><span class="sxs-lookup"><span data-stu-id="70e32-2188">The example</span></span>
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
<span data-ttu-id="70e32-2189">produz a saída</span><span class="sxs-lookup"><span data-stu-id="70e32-2189">produces the output</span></span>
```
True
False
False
False
```

<span data-ttu-id="70e32-2190">O `s` e `t` variáveis se referirem a dois diferentes `string` instâncias que contém os mesmos caracteres.</span><span class="sxs-lookup"><span data-stu-id="70e32-2190">The `s` and `t` variables refer to two distinct `string` instances containing the same characters.</span></span> <span data-ttu-id="70e32-2191">A primeira comparação gera `True` porque o operador de igualdade de cadeia de caracteres predefinida ([operadores de igualdade de cadeia de caracteres](expressions.md#string-equality-operators)) é selecionado quando ambos os operandos forem do tipo `string`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2191">The first comparison outputs `True` because the predefined string equality operator ([String equality operators](expressions.md#string-equality-operators)) is selected when both operands are of type `string`.</span></span> <span data-ttu-id="70e32-2192">Todas as comparações restantes de saída `False` porque o operador de igualdade do tipo de referência predefinidos é selecionado quando um ou ambos os operandos forem do tipo `object`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2192">The remaining comparisons all output `False` because the predefined reference type equality operator is selected when one or both of the operands are of type `object`.</span></span>

<span data-ttu-id="70e32-2193">Observe que a técnica acima não é significativa para tipos de valor.</span><span class="sxs-lookup"><span data-stu-id="70e32-2193">Note that the above technique is not meaningful for value types.</span></span> <span data-ttu-id="70e32-2194">O exemplo</span><span class="sxs-lookup"><span data-stu-id="70e32-2194">The example</span></span>
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
<span data-ttu-id="70e32-2195">gera `False` porque as conversões criem referências para duas instâncias separadas do box `int` valores.</span><span class="sxs-lookup"><span data-stu-id="70e32-2195">outputs `False` because the casts create references to two separate instances of boxed `int` values.</span></span>

### <a name="string-equality-operators"></a><span data-ttu-id="70e32-2196">Operadores de igualdade de cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="70e32-2196">String equality operators</span></span>

<span data-ttu-id="70e32-2197">Os operadores de igualdade de cadeia de caracteres predefinidas são:</span><span class="sxs-lookup"><span data-stu-id="70e32-2197">The predefined string equality operators are:</span></span>
```csharp
bool operator ==(string x, string y);
bool operator !=(string x, string y);
```

<span data-ttu-id="70e32-2198">Dois `string` valores são considerados iguais quando uma das seguintes opções for verdadeira:</span><span class="sxs-lookup"><span data-stu-id="70e32-2198">Two `string` values are considered equal when one of the following is true:</span></span>

*  <span data-ttu-id="70e32-2199">Os dois valores forem `null`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2199">Both values are `null`.</span></span>
*  <span data-ttu-id="70e32-2200">Ambos os valores são nulos referências às instâncias de cadeia de caracteres que têm caracteres idênticos e comprimentos idênticos em cada posição do caractere.</span><span class="sxs-lookup"><span data-stu-id="70e32-2200">Both values are non-null references to string instances that have identical lengths and identical characters in each character position.</span></span>

<span data-ttu-id="70e32-2201">Os operadores de igualdade de cadeia de caracteres comparam valores de cadeia de caracteres em vez de referências de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="70e32-2201">The string equality operators compare string values rather than string references.</span></span> <span data-ttu-id="70e32-2202">Quando duas instâncias de cadeia de caracteres separada contêm a mesma sequência exata de caracteres, os valores das cadeias de caracteres são iguais, mas as referências são diferentes.</span><span class="sxs-lookup"><span data-stu-id="70e32-2202">When two separate string instances contain the exact same sequence of characters, the values of the strings are equal, but the references are different.</span></span> <span data-ttu-id="70e32-2203">Conforme descrito em [operadores de igualdade de tipo de referência](expressions.md#reference-type-equality-operators), os operadores de igualdade do tipo de referência podem ser usados para comparar as referências de cadeia de caracteres em vez de valores de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="70e32-2203">As described in [Reference type equality operators](expressions.md#reference-type-equality-operators), the reference type equality operators can be used to compare string references instead of string values.</span></span>

### <a name="delegate-equality-operators"></a><span data-ttu-id="70e32-2204">Operadores de igualdade de delegado</span><span class="sxs-lookup"><span data-stu-id="70e32-2204">Delegate equality operators</span></span>

<span data-ttu-id="70e32-2205">Cada tipo de delegado implicitamente fornece os seguintes operadores de comparação predefinidos:</span><span class="sxs-lookup"><span data-stu-id="70e32-2205">Every delegate type implicitly provides the following predefined comparison operators:</span></span>

```csharp
bool operator ==(System.Delegate x, System.Delegate y);
bool operator !=(System.Delegate x, System.Delegate y);
```

<span data-ttu-id="70e32-2206">Delegado duas instâncias são consideradas iguais da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="70e32-2206">Two delegate instances are considered equal as follows:</span></span>

*  <span data-ttu-id="70e32-2207">Se qualquer uma das instâncias de delegado for `null`, eles são iguais se e somente se ambos forem `null`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2207">If either of the delegate instances is `null`, they are equal if and only if both are `null`.</span></span>
*  <span data-ttu-id="70e32-2208">Se os delegados tem um tipo diferente de tempo de execução, eles nunca são iguais.</span><span class="sxs-lookup"><span data-stu-id="70e32-2208">If the delegates have different run-time type they are never equal.</span></span>
*  <span data-ttu-id="70e32-2209">Se ambas as instâncias de delegado têm uma lista de invocação ([declarações de delegado](delegates.md#delegate-declarations)), essas instâncias são iguais se e somente se suas listas de invocação têm o mesmo comprimento, e cada entrada na lista de invocação de um é igual a (conforme definido abaixo) para a entrada correspondente, em ordem, na lista de invocação do outro.</span><span class="sxs-lookup"><span data-stu-id="70e32-2209">If both of the delegate instances have an invocation list ([Delegate declarations](delegates.md#delegate-declarations)), those instances are equal if and only if their invocation lists are the same length, and each entry in one's invocation list is equal (as defined below) to the corresponding entry, in order, in the other's invocation list.</span></span>

<span data-ttu-id="70e32-2210">As seguintes regras regem a igualdade de entradas da lista de invocação:</span><span class="sxs-lookup"><span data-stu-id="70e32-2210">The following rules govern the equality of invocation list entries:</span></span>

*  <span data-ttu-id="70e32-2211">Se dois invocação entradas da lista os dois se referirem ao mesmo estático método e em seguida, as entradas são iguais.</span><span class="sxs-lookup"><span data-stu-id="70e32-2211">If two invocation list entries both refer to the same static method then the entries are equal.</span></span>
*  <span data-ttu-id="70e32-2212">Se dois invocação entradas da lista os dois se referirem ao mesmo método não estático no mesmo objeto de destino (conforme definido pelos operadores de igualdade de referência), em seguida, as entradas são iguais.</span><span class="sxs-lookup"><span data-stu-id="70e32-2212">If two invocation list entries both refer to the same non-static method on the same target object (as defined by the reference equality operators) then the entries are equal.</span></span>
*  <span data-ttu-id="70e32-2213">Entradas da lista de invocação produzido da avaliação de semanticamente idêntico *anonymous_method_expression*s ou *lambda_expression*s com o mesmo conjunto (possivelmente vazio) de variável externa capturada instâncias são permitidas (mas não obrigatórios) são iguais.</span><span class="sxs-lookup"><span data-stu-id="70e32-2213">Invocation list entries produced from evaluation of semantically identical *anonymous_method_expression*s or *lambda_expression*s with the same (possibly empty) set of captured outer variable instances are permitted (but not required) to be equal.</span></span>

### <a name="equality-operators-and-null"></a><span data-ttu-id="70e32-2214">NULL e operadores de igualdade</span><span class="sxs-lookup"><span data-stu-id="70e32-2214">Equality operators and null</span></span>

<span data-ttu-id="70e32-2215">O `==` e `!=` operadores permitem um operando para ser um valor de um tipo anulável e outra para ser o `null` literal, mesmo se não existe nenhum operador predefinido ou definidos pelo usuário (no unlifted ou retirados de formulário) para a operação.</span><span class="sxs-lookup"><span data-stu-id="70e32-2215">The `==` and `!=` operators permit one operand to be a value of a nullable type and the other to be the `null` literal, even if no predefined or user-defined operator (in unlifted or lifted form) exists for the operation.</span></span>

<span data-ttu-id="70e32-2216">Para uma operação de uma das formas</span><span class="sxs-lookup"><span data-stu-id="70e32-2216">For an operation of one of the forms</span></span>
```csharp
x == null
null == x
x != null
null != x
```
<span data-ttu-id="70e32-2217">em que `x` é uma expressão de um tipo que permite valor nulo, se a resolução de sobrecarga de operador ([resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)) Falha ao localizar um operador aplicável, o resultado em vez disso, é computada a partir de `HasValue` propriedade de `x`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2217">where `x` is an expression of a nullable type, if operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) fails to find an applicable operator, the result is instead computed from the `HasValue` property of `x`.</span></span> <span data-ttu-id="70e32-2218">Especificamente, os primeiros dois formulários são convertidos em `!x.HasValue`, e os últimos dois formulários são convertidos em `x.HasValue`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2218">Specifically, the first two forms are translated into `!x.HasValue`, and last two forms are translated into `x.HasValue`.</span></span>

### <a name="the-is-operator"></a><span data-ttu-id="70e32-2219">O operador is</span><span class="sxs-lookup"><span data-stu-id="70e32-2219">The is operator</span></span>

<span data-ttu-id="70e32-2220">O `is` operador é usado para verificar dinamicamente se o tipo de tempo de execução de um objeto é compatível com um determinado tipo.</span><span class="sxs-lookup"><span data-stu-id="70e32-2220">The `is` operator is used to dynamically check if the run-time type of an object is compatible with a given type.</span></span> <span data-ttu-id="70e32-2221">O resultado da operação `E is T`, onde `E` é uma expressão e `T` é um tipo, é um booleano valor que indica se `E` pode ser convertido com êxito para o tipo `T` por uma conversão de referência, uma conversão boxing conversão, ou uma conversão unboxing.</span><span class="sxs-lookup"><span data-stu-id="70e32-2221">The result of the operation `E is T`, where `E` is an expression and `T` is a type, is a boolean value indicating whether `E` can successfully be converted to type `T` by a reference conversion, a boxing conversion, or an unboxing conversion.</span></span> <span data-ttu-id="70e32-2222">A operação é avaliada como a seguir, depois de argumentos de tipo tiverem sido substituídos para todos os parâmetros de tipo:</span><span class="sxs-lookup"><span data-stu-id="70e32-2222">The operation is evaluated as follows, after type arguments have been substituted for all type parameters:</span></span>

*  <span data-ttu-id="70e32-2223">Se `E` é uma função anônima, ocorre um erro de tempo de compilação</span><span class="sxs-lookup"><span data-stu-id="70e32-2223">If `E` is an anonymous function, a compile-time error occurs</span></span>
*  <span data-ttu-id="70e32-2224">Se `E` é um grupo de método ou o `null` literal, se o tipo de `E` é um tipo de referência ou um tipo anulável e o valor de `E` é nulo, o resultado é false.</span><span class="sxs-lookup"><span data-stu-id="70e32-2224">If `E` is a method group or the `null` literal, of if the type of `E` is a reference type or a nullable type and the value of `E` is null, the result is false.</span></span>
*  <span data-ttu-id="70e32-2225">Caso contrário, deixe `D` representam o tipo dinâmico de `E` da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="70e32-2225">Otherwise, let `D` represent the dynamic type of `E` as follows:</span></span>
   * <span data-ttu-id="70e32-2226">Se o tipo de `E` é um tipo de referência `D` é o tipo de tempo de execução da referência de instância por `E`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2226">If the type of `E` is a reference type, `D` is the run-time type of the instance reference by `E`.</span></span>
   * <span data-ttu-id="70e32-2227">Se o tipo de `E` é um tipo anulável, `D` é o tipo subjacente desse tipo que permite valor nulo.</span><span class="sxs-lookup"><span data-stu-id="70e32-2227">If the type of `E` is a nullable type, `D` is the underlying type of that nullable type.</span></span>
   * <span data-ttu-id="70e32-2228">Se o tipo de `E` é um tipo de valor não anulável `D` é o tipo de `E`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2228">If the type of `E` is a non-nullable value type, `D` is the type of `E`.</span></span>
*  <span data-ttu-id="70e32-2229">O resultado da operação depende `D` e `T` da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="70e32-2229">The result of the operation depends on `D` and `T` as follows:</span></span>
   * <span data-ttu-id="70e32-2230">Se `T` é um tipo de referência, o resultado será true se `D` e `T` são do mesmo tipo, se `D` é um tipo de referência e uma conversão de referência implícita da `D` para `T` existe, ou se `D` é um tipo de valor e uma conversão de uma conversão boxing `D` para `T` existe.</span><span class="sxs-lookup"><span data-stu-id="70e32-2230">If `T` is a reference type, the result is true if `D` and `T` are the same type, if `D` is a reference type and an implicit reference conversion from `D` to `T` exists, or if `D` is a value type and a boxing conversion from `D` to `T` exists.</span></span>
   * <span data-ttu-id="70e32-2231">Se `T` é um tipo anulável, o resultado será true se `D` é o tipo subjacente de `T`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2231">If `T` is a nullable type, the result is true if `D` is the underlying type of `T`.</span></span>
   * <span data-ttu-id="70e32-2232">Se `T` é um tipo de valor não anulável, o resultado será true se `D` e `T` são do mesmo tipo.</span><span class="sxs-lookup"><span data-stu-id="70e32-2232">If `T` is a non-nullable value type, the result is true if `D` and `T` are the same type.</span></span>
   * <span data-ttu-id="70e32-2233">Caso contrário, o resultado é false.</span><span class="sxs-lookup"><span data-stu-id="70e32-2233">Otherwise, the result is false.</span></span>

<span data-ttu-id="70e32-2234">Observe que as conversões definidas pelo usuário, não são consideradas pelo `is` operador.</span><span class="sxs-lookup"><span data-stu-id="70e32-2234">Note that user defined conversions, are not considered by the `is` operator.</span></span>

### <a name="the-as-operator"></a><span data-ttu-id="70e32-2235">O operador</span><span class="sxs-lookup"><span data-stu-id="70e32-2235">The as operator</span></span>

<span data-ttu-id="70e32-2236">O `as` operador é usado para converter explicitamente um valor para um tipo de referência fornecida ou tipo que permite valor nulo.</span><span class="sxs-lookup"><span data-stu-id="70e32-2236">The `as` operator is used to explicitly convert a value to a given reference type or nullable type.</span></span> <span data-ttu-id="70e32-2237">Ao contrário de uma expressão de conversão ([expressões de conversão](expressions.md#cast-expressions)), o `as` operador nunca gera uma exceção.</span><span class="sxs-lookup"><span data-stu-id="70e32-2237">Unlike a cast expression ([Cast expressions](expressions.md#cast-expressions)), the `as` operator never throws an exception.</span></span> <span data-ttu-id="70e32-2238">Em vez disso, se a conversão indicada não for possível, o valor resultante é `null`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2238">Instead, if the indicated conversion is not possible, the resulting value is `null`.</span></span>

<span data-ttu-id="70e32-2239">Em uma operação do formulário `E as T`, `E` deve ser uma expressão e `T` deve ser um tipo de referência, um parâmetro de tipo conhecido por ser um tipo de referência ou um tipo anulável.</span><span class="sxs-lookup"><span data-stu-id="70e32-2239">In an operation of the form `E as T`, `E` must be an expression and `T` must be a reference type, a type parameter known to be a reference type, or a nullable type.</span></span> <span data-ttu-id="70e32-2240">Além disso, pelo menos uma das seguintes opções deve ser verdadeira ou, caso contrário, ocorrerá um erro de tempo de compilação:</span><span class="sxs-lookup"><span data-stu-id="70e32-2240">Furthermore, at least one of the following must be true, or otherwise a compile-time error occurs:</span></span>

*  <span data-ttu-id="70e32-2241">Uma identidade ([conversão de identidade](conversions.md#identity-conversion)), implícita que permitem valor nulo ([conversões implícitas de anuláveis](conversions.md#implicit-nullable-conversions)), a referência implícita ([conversões de referência implícita](conversions.md#implicit-reference-conversions)), conversão boxing ([ Conversões boxing](conversions.md#boxing-conversions)), explícita que permite valor nulo ([conversões que permitem valor nulas explícitas](conversions.md#explicit-nullable-conversions)), referência explícita ([conversões de referência explícita](conversions.md#explicit-reference-conversions)), ou conversão unboxing ([Conversões de conversão Unboxing](conversions.md#unboxing-conversions)) existe conversão de `E` para `T`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2241">An identity ([Identity conversion](conversions.md#identity-conversion)), implicit nullable ([Implicit nullable conversions](conversions.md#implicit-nullable-conversions)), implicit reference ([Implicit reference conversions](conversions.md#implicit-reference-conversions)), boxing ([Boxing conversions](conversions.md#boxing-conversions)), explicit nullable ([Explicit nullable conversions](conversions.md#explicit-nullable-conversions)), explicit reference ([Explicit reference conversions](conversions.md#explicit-reference-conversions)), or unboxing ([Unboxing conversions](conversions.md#unboxing-conversions)) conversion exists from `E` to `T`.</span></span>
*  <span data-ttu-id="70e32-2242">O tipo de `E` ou `T` é um tipo aberto.</span><span class="sxs-lookup"><span data-stu-id="70e32-2242">The type of `E` or `T` is an open type.</span></span>
*  <span data-ttu-id="70e32-2243">`E` é o `null` literal.</span><span class="sxs-lookup"><span data-stu-id="70e32-2243">`E` is the `null` literal.</span></span>

<span data-ttu-id="70e32-2244">Se o tipo de tempo de compilação do `E` não é `dynamic`, a operação `E as T` produz o mesmo resultado que</span><span class="sxs-lookup"><span data-stu-id="70e32-2244">If the compile-time type of `E` is not `dynamic`, the operation `E as T` produces the same result as</span></span>
```csharp
E is T ? (T)(E) : (T)null
```
<span data-ttu-id="70e32-2245">exceto que `E` é avaliado apenas uma vez.</span><span class="sxs-lookup"><span data-stu-id="70e32-2245">except that `E` is only evaluated once.</span></span> <span data-ttu-id="70e32-2246">O compilador pode ser esperado para otimizar `E as T` para executar no máximo uma verificação de tipo dinâmico em vez das duas verificações de tipo dinâmico implicado pela expansão acima.</span><span class="sxs-lookup"><span data-stu-id="70e32-2246">The compiler can be expected to optimize `E as T` to perform at most one dynamic type check as opposed to the two dynamic type checks implied by the expansion above.</span></span>

<span data-ttu-id="70e32-2247">Se o tipo de tempo de compilação do `E` é `dynamic`, diferentemente do operador cast as `as` operador não está vinculado dinamicamente ([associação dinâmica](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="70e32-2247">If the compile-time type of `E` is `dynamic`, unlike the cast operator the `as` operator is not dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="70e32-2248">Portanto, a expansão nesse caso é:</span><span class="sxs-lookup"><span data-stu-id="70e32-2248">Therefore the expansion in this case is:</span></span>
```csharp
E is T ? (T)(object)(E) : (T)null
```

<span data-ttu-id="70e32-2249">Observe que algumas conversões, como conversões definidas pelo usuário, não são possíveis com o `as` operador e, em vez disso, deve ser executada usando expressões de conversão.</span><span class="sxs-lookup"><span data-stu-id="70e32-2249">Note that some conversions, such as user defined conversions, are not possible with the `as` operator and should instead be performed using cast expressions.</span></span>

<span data-ttu-id="70e32-2250">No exemplo</span><span class="sxs-lookup"><span data-stu-id="70e32-2250">In the example</span></span>
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
<span data-ttu-id="70e32-2251">o parâmetro de tipo `T` de `G` é conhecido por ser um tipo de referência, porque ele tem a restrição de classe.</span><span class="sxs-lookup"><span data-stu-id="70e32-2251">the type parameter `T` of `G` is known to be a reference type, because it has the class constraint.</span></span> <span data-ttu-id="70e32-2252">O parâmetro de tipo `U` dos `H` não está no entanto; portanto, o uso das `as` operador em `H` não é permitido.</span><span class="sxs-lookup"><span data-stu-id="70e32-2252">The type parameter `U` of `H` is not however; hence the use of the `as` operator in `H` is disallowed.</span></span>

## <a name="logical-operators"></a><span data-ttu-id="70e32-2253">Operadores lógicos</span><span class="sxs-lookup"><span data-stu-id="70e32-2253">Logical operators</span></span>

<span data-ttu-id="70e32-2254">O `&`, `^`, e `|` operadores são chamados de operadores lógicos.</span><span class="sxs-lookup"><span data-stu-id="70e32-2254">The `&`, `^`, and `|` operators are called the logical operators.</span></span>

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

<span data-ttu-id="70e32-2255">Se um operando de um operador lógico tem o tipo de tempo de compilação `dynamic`, em seguida, a expressão está associada dinamicamente ([vinculação dinâmica](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="70e32-2255">If an operand of a logical operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="70e32-2256">Nesse caso é o tipo de tempo de compilação da expressão `dynamic`, e a resolução descrita a seguir ocorrerá em tempo de execução usando o tipo de tempo de execução desses operandos que tenham o tipo de tempo de compilação `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2256">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="70e32-2257">Para uma operação do formulário `x op y`, onde `op` é um dos operadores lógicos, resolução de sobrecarga ([resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)) é aplicado para selecionar uma implementação do operador específico.</span><span class="sxs-lookup"><span data-stu-id="70e32-2257">For an operation of the form `x op y`, where `op` is one of the logical operators, overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="70e32-2258">Os operandos são convertidos nos tipos de parâmetro do operador selecionado e o tipo do resultado é o tipo de retorno do operador.</span><span class="sxs-lookup"><span data-stu-id="70e32-2258">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="70e32-2259">Os operadores lógicos predefinidos são descritos nas seções a seguir.</span><span class="sxs-lookup"><span data-stu-id="70e32-2259">The predefined logical operators are described in the following sections.</span></span>

### <a name="integer-logical-operators"></a><span data-ttu-id="70e32-2260">Operadores lógicos de inteiro</span><span class="sxs-lookup"><span data-stu-id="70e32-2260">Integer logical operators</span></span>

<span data-ttu-id="70e32-2261">Os operadores lógicos de inteiro predefinidos são:</span><span class="sxs-lookup"><span data-stu-id="70e32-2261">The predefined integer logical operators are:</span></span>
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

<span data-ttu-id="70e32-2262">O `&` operador calcula o bit a bit lógicos `AND` dos dois operandos, o `|` operador calcula o bit a bit lógicos `OR` dos dois operandos e o `^` operador calcula o exclusivo lógico bit a bit `OR` dos dois operandos.</span><span class="sxs-lookup"><span data-stu-id="70e32-2262">The `&` operator computes the bitwise logical `AND` of the two operands, the `|` operator computes the bitwise logical `OR` of the two operands, and the `^` operator computes the bitwise logical exclusive `OR` of the two operands.</span></span> <span data-ttu-id="70e32-2263">Não há estouros são possíveis dessas operações.</span><span class="sxs-lookup"><span data-stu-id="70e32-2263">No overflows are possible from these operations.</span></span>

### <a name="enumeration-logical-operators"></a><span data-ttu-id="70e32-2264">Operadores lógicos de enumeração</span><span class="sxs-lookup"><span data-stu-id="70e32-2264">Enumeration logical operators</span></span>

<span data-ttu-id="70e32-2265">Cada tipo de enumeração `E` implicitamente oferece operadores lógicos de predefinidas a seguir:</span><span class="sxs-lookup"><span data-stu-id="70e32-2265">Every enumeration type `E` implicitly provides the following predefined logical operators:</span></span>

```csharp
E operator &(E x, E y);
E operator |(E x, E y);
E operator ^(E x, E y);
```

<span data-ttu-id="70e32-2266">O resultado da avaliação `x op y`, onde `x` e `y` são expressões de um tipo de enumeração `E` com um tipo subjacente `U`, e `op` é um dos operadores lógicos, é exatamente o mesmo que Avaliando `(E)((U)x op (U)y)`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2266">The result of evaluating `x op y`, where `x` and `y` are expressions of an enumeration type `E` with an underlying type `U`, and `op` is one of the logical operators, is exactly the same as evaluating `(E)((U)x op (U)y)`.</span></span> <span data-ttu-id="70e32-2267">Em outras palavras, os operadores lógicos de tipo de enumeração simplesmente executam a operação lógica no tipo subjacente dos dois operandos.</span><span class="sxs-lookup"><span data-stu-id="70e32-2267">In other words, the enumeration type logical operators simply perform the logical operation on the underlying type of the two operands.</span></span>

### <a name="boolean-logical-operators"></a><span data-ttu-id="70e32-2268">Operadores lógicos boolianos</span><span class="sxs-lookup"><span data-stu-id="70e32-2268">Boolean logical operators</span></span>

<span data-ttu-id="70e32-2269">Os operadores lógicos boolianos predefinidos são:</span><span class="sxs-lookup"><span data-stu-id="70e32-2269">The predefined boolean logical operators are:</span></span>
```csharp
bool operator &(bool x, bool y);
bool operator |(bool x, bool y);
bool operator ^(bool x, bool y);
```

<span data-ttu-id="70e32-2270">O resultado de `x & y` está `true` se ambos `x` e `y` são `true`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2270">The result of `x & y` is `true` if both `x` and `y` are `true`.</span></span> <span data-ttu-id="70e32-2271">Caso contrário, o resultado é `false`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2271">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="70e32-2272">O resultado de `x | y` está `true` se qualquer um dos `x` ou `y` é `true`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2272">The result of `x | y` is `true` if either `x` or `y` is `true`.</span></span> <span data-ttu-id="70e32-2273">Caso contrário, o resultado é `false`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2273">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="70e32-2274">O resultado de `x ^ y` está `true` se `x` é `true` e `y` é `false`, ou `x` é `false` e `y` é `true`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2274">The result of `x ^ y` is `true` if `x` is `true` and `y` is `false`, or `x` is `false` and `y` is `true`.</span></span> <span data-ttu-id="70e32-2275">Caso contrário, o resultado é `false`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2275">Otherwise, the result is `false`.</span></span> <span data-ttu-id="70e32-2276">Quando os operandos forem do tipo `bool`, o `^` operador calcula o mesmo resultado que o `!=` operador.</span><span class="sxs-lookup"><span data-stu-id="70e32-2276">When the operands are of type `bool`, the `^` operator computes the same result as the `!=` operator.</span></span>

### <a name="nullable-boolean-logical-operators"></a><span data-ttu-id="70e32-2277">Operadores lógicos boolianos que permitem valor nulos</span><span class="sxs-lookup"><span data-stu-id="70e32-2277">Nullable boolean logical operators</span></span>

<span data-ttu-id="70e32-2278">O tipo de booliano anulável `bool?` pode representar os três valores, `true`, `false`, e `null`e é conceitualmente semelhante ao tipo de três valores usado para expressões Boolianas em SQL.</span><span class="sxs-lookup"><span data-stu-id="70e32-2278">The nullable boolean type `bool?` can represent three values, `true`, `false`, and `null`, and is conceptually similar to the three-valued type used for boolean expressions in SQL.</span></span> <span data-ttu-id="70e32-2279">Para garantir que os resultados produzidos pelos `&` e `|` operadores para `bool?` operandos são consistentes com a lógica de três valores do SQL, os seguintes operadores predefinidos são fornecidos:</span><span class="sxs-lookup"><span data-stu-id="70e32-2279">To ensure that the results produced by the `&` and `|` operators for `bool?` operands are consistent with SQL's three-valued logic, the following predefined operators are provided:</span></span>

```csharp
bool? operator &(bool? x, bool? y);
bool? operator |(bool? x, bool? y);
```

<span data-ttu-id="70e32-2280">A tabela a seguir lista os resultados produzidos por esses operadores para todas as combinações dos valores `true`, `false`, e `null`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2280">The following table lists the results produced by these operators for all combinations of the values `true`, `false`, and `null`.</span></span>

| `x`     | `y`     | `x & y` | <span data-ttu-id="70e32-2281">' x</span><span class="sxs-lookup"><span data-stu-id="70e32-2281">\`x</span></span> | <span data-ttu-id="70e32-2282">y'</span><span class="sxs-lookup"><span data-stu-id="70e32-2282">y\`</span></span> |
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

## <a name="conditional-logical-operators"></a><span data-ttu-id="70e32-2283">Operadores lógicos condicionais</span><span class="sxs-lookup"><span data-stu-id="70e32-2283">Conditional logical operators</span></span>

<span data-ttu-id="70e32-2284">O `&&` e `||` operadores são chamados de operadores lógicos condicionais.</span><span class="sxs-lookup"><span data-stu-id="70e32-2284">The `&&` and `||` operators are called the conditional logical operators.</span></span> <span data-ttu-id="70e32-2285">Eles também são chamados de operadores lógicos "curto-circuito".</span><span class="sxs-lookup"><span data-stu-id="70e32-2285">They are also called the "short-circuiting" logical operators.</span></span>

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

<span data-ttu-id="70e32-2286">O `&&` e `||` operadores são versões condicionais da `&` e `|` operadores:</span><span class="sxs-lookup"><span data-stu-id="70e32-2286">The `&&` and `||` operators are conditional versions of the `&` and `|` operators:</span></span>

*  <span data-ttu-id="70e32-2287">A operação `x && y` corresponde à operação `x & y`, exceto pelo fato `y` é avaliada apenas se `x` não é `false`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2287">The operation `x && y` corresponds to the operation `x & y`, except that `y` is evaluated only if `x` is not `false`.</span></span>
*  <span data-ttu-id="70e32-2288">A operação `x || y` corresponde à operação `x | y`, exceto pelo fato `y` é avaliada apenas se `x` não é `true`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2288">The operation `x || y` corresponds to the operation `x | y`, except that `y` is evaluated only if `x` is not `true`.</span></span>

<span data-ttu-id="70e32-2289">Se um operando de um operador lógico condicional tem o tipo de tempo de compilação `dynamic`, em seguida, a expressão está associada dinamicamente ([vinculação dinâmica](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="70e32-2289">If an operand of a conditional logical operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="70e32-2290">Nesse caso é o tipo de tempo de compilação da expressão `dynamic`, e a resolução descrita a seguir ocorrerá em tempo de execução usando o tipo de tempo de execução desses operandos que tenham o tipo de tempo de compilação `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2290">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="70e32-2291">Uma operação do formulário `x && y` ou `x || y` é processada por meio da aplicação de resolução de sobrecarga ([resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)) como se a operação foi escrita `x & y` ou `x | y`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2291">An operation of the form `x && y` or `x || y` is processed by applying overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) as if the operation was written `x & y` or `x | y`.</span></span> <span data-ttu-id="70e32-2292">Em seguida,</span><span class="sxs-lookup"><span data-stu-id="70e32-2292">Then,</span></span>

*  <span data-ttu-id="70e32-2293">Se a resolução de sobrecarga não consegue localizar um único operador melhor, ou se a resolução de sobrecarga seleciona um dos operadores lógicos de inteiro predefinidos, ocorrerá um erro de tempo de associação.</span><span class="sxs-lookup"><span data-stu-id="70e32-2293">If overload resolution fails to find a single best operator, or if overload resolution selects one of the predefined integer logical operators, a binding-time error occurs.</span></span>
*  <span data-ttu-id="70e32-2294">Caso contrário, se o operador selecionado é um dos operadores lógicos predefinidos booleanos ([operadores lógicos boolianos](expressions.md#boolean-logical-operators)) ou os operadores lógicos boolianos anuláveis ([operadores lógicos de boolianos anuláveis](expressions.md#nullable-boolean-logical-operators)), o a operação é processada, conforme descrito em [Boolean operadores lógicos condicionais](expressions.md#boolean-conditional-logical-operators).</span><span class="sxs-lookup"><span data-stu-id="70e32-2294">Otherwise, if the selected operator is one of the predefined boolean logical operators ([Boolean logical operators](expressions.md#boolean-logical-operators)) or nullable boolean logical operators ([Nullable boolean logical operators](expressions.md#nullable-boolean-logical-operators)), the operation is processed as described in [Boolean conditional logical operators](expressions.md#boolean-conditional-logical-operators).</span></span>
*  <span data-ttu-id="70e32-2295">Caso contrário, o operador selecionado é um operador definido pelo usuário e a operação é processada, conforme descrito em [operadores lógicos condicionais definidos pelo usuário](expressions.md#user-defined-conditional-logical-operators).</span><span class="sxs-lookup"><span data-stu-id="70e32-2295">Otherwise, the selected operator is a user-defined operator, and the operation is processed as described in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators).</span></span>

<span data-ttu-id="70e32-2296">Não é possível sobrecarregar diretamente os operadores lógicos condicionais.</span><span class="sxs-lookup"><span data-stu-id="70e32-2296">It is not possible to directly overload the conditional logical operators.</span></span> <span data-ttu-id="70e32-2297">No entanto, porque os operadores lógicos condicionais são avaliados em termos de operadores lógicos regulares, sobrecargas dos operadores lógicos regulares, com algumas restrições, também são consideradas sobrecargas dos operadores lógicos condicionais.</span><span class="sxs-lookup"><span data-stu-id="70e32-2297">However, because the conditional logical operators are evaluated in terms of the regular logical operators, overloads of the regular logical operators are, with certain restrictions, also considered overloads of the conditional logical operators.</span></span> <span data-ttu-id="70e32-2298">Isso é descrito posteriormente em [operadores lógicos condicionais definidos pelo usuário](expressions.md#user-defined-conditional-logical-operators).</span><span class="sxs-lookup"><span data-stu-id="70e32-2298">This is described further in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators).</span></span>

### <a name="boolean-conditional-logical-operators"></a><span data-ttu-id="70e32-2299">Boolianos operadores lógicos condicionais</span><span class="sxs-lookup"><span data-stu-id="70e32-2299">Boolean conditional logical operators</span></span>

<span data-ttu-id="70e32-2300">Quando os operandos de `&&` ou `||` são do tipo `bool`, ou quando os operandos forem de tipos que não definem um aplicável `operator &` ou `operator |`, mas definir conversões implícitas para `bool`, a operação é processado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="70e32-2300">When the operands of `&&` or `||` are of type `bool`, or when the operands are of types that do not define an applicable `operator &` or `operator |`, but do define implicit conversions to `bool`, the operation is processed as follows:</span></span>

*  <span data-ttu-id="70e32-2301">A operação `x && y` é avaliada como `x ? y : false`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2301">The operation `x && y` is evaluated as `x ? y : false`.</span></span> <span data-ttu-id="70e32-2302">Em outras palavras, `x` é avaliado primeiro e convertido no tipo `bool`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2302">In other words, `x` is first evaluated and converted to type `bool`.</span></span> <span data-ttu-id="70e32-2303">Então, se `x` está `true`, `y` é avaliada e convertido no tipo `bool`, e isso se torna o resultado da operação.</span><span class="sxs-lookup"><span data-stu-id="70e32-2303">Then, if `x` is `true`, `y` is evaluated and converted to type `bool`, and this becomes the result of the operation.</span></span> <span data-ttu-id="70e32-2304">Caso contrário, o resultado da operação é `false`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2304">Otherwise, the result of the operation is `false`.</span></span>
*  <span data-ttu-id="70e32-2305">A operação `x || y` é avaliada como `x ? true : y`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2305">The operation `x || y` is evaluated as `x ? true : y`.</span></span> <span data-ttu-id="70e32-2306">Em outras palavras, `x` é avaliado primeiro e convertido no tipo `bool`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2306">In other words, `x` is first evaluated and converted to type `bool`.</span></span> <span data-ttu-id="70e32-2307">Então, se `x` está `true`, o resultado da operação é `true`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2307">Then, if `x` is `true`, the result of the operation is `true`.</span></span> <span data-ttu-id="70e32-2308">Caso contrário, `y` é avaliado e convertido no tipo `bool`, e isso se torna o resultado da operação.</span><span class="sxs-lookup"><span data-stu-id="70e32-2308">Otherwise, `y` is evaluated and converted to type `bool`, and this becomes the result of the operation.</span></span>

### <a name="user-defined-conditional-logical-operators"></a><span data-ttu-id="70e32-2309">Operadores lógicos condicionais definidos pelo usuário</span><span class="sxs-lookup"><span data-stu-id="70e32-2309">User-defined conditional logical operators</span></span>

<span data-ttu-id="70e32-2310">Quando os operandos de `&&` ou `||` são de tipos que declaram um aplicável definidos pelo usuário `operator &` ou `operator |`, ambos os seguintes procedimentos devem ser verdadeiras, onde `T` é o tipo no qual o operador selecionado é declarado:</span><span class="sxs-lookup"><span data-stu-id="70e32-2310">When the operands of `&&` or `||` are of types that declare an applicable user-defined `operator &` or `operator |`, both of the following must be true, where `T` is the type in which the selected operator is declared:</span></span>

*  <span data-ttu-id="70e32-2311">O tipo de retorno e o tipo de cada parâmetro do operador selecionado devem ser `T`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2311">The return type and the type of each parameter of the selected operator must be `T`.</span></span> <span data-ttu-id="70e32-2312">Em outras palavras, o operador deve calcular o lógico `AND` ou a lógica `OR` de dois operandos do tipo `T`e deve retornar um resultado do tipo `T`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2312">In other words, the operator must compute the logical `AND` or the logical `OR` of two operands of type `T`, and must return a result of type `T`.</span></span>
*  <span data-ttu-id="70e32-2313">`T` deve conter declarações de `operator true` e `operator false`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2313">`T` must contain declarations of `operator true` and `operator false`.</span></span>

<span data-ttu-id="70e32-2314">Ocorrerá um erro de tempo de associação se esses requisitos não for atendida.</span><span class="sxs-lookup"><span data-stu-id="70e32-2314">A binding-time error occurs if either of these requirements is not satisfied.</span></span> <span data-ttu-id="70e32-2315">Caso contrário, o `&&` ou `||` operação é calculada pela combinação de definido pelo usuário `operator true` ou `operator false` com o operador selecionado definido pelo usuário:</span><span class="sxs-lookup"><span data-stu-id="70e32-2315">Otherwise, the `&&` or `||` operation is evaluated by combining the user-defined `operator true` or `operator false` with the selected user-defined operator:</span></span>

*  <span data-ttu-id="70e32-2316">A operação `x && y` é avaliado como `T.false(x) ? x : T.&(x, y)`, onde `T.false(x)` é uma invocação dos `operator false` declarado no `T`, e `T.&(x, y)` é uma invocação do selecionado `operator &`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2316">The operation `x && y` is evaluated as `T.false(x) ? x : T.&(x, y)`, where `T.false(x)` is an invocation of the `operator false` declared in `T`, and `T.&(x, y)` is an invocation of the selected `operator &`.</span></span> <span data-ttu-id="70e32-2317">Em outras palavras, `x` é avaliado primeiro e `operator false` é invocado no resultado para determinar se `x` é, definitivamente, false.</span><span class="sxs-lookup"><span data-stu-id="70e32-2317">In other words, `x` is first evaluated and `operator false` is invoked on the result to determine if `x` is definitely false.</span></span> <span data-ttu-id="70e32-2318">Então, se `x` é definitivamente false, o resultado da operação é o valor calculado anteriormente para `x`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2318">Then, if `x` is definitely false, the result of the operation is the value previously computed for `x`.</span></span> <span data-ttu-id="70e32-2319">Caso contrário, `y` é avaliada e selecionado `operator &` é invocado no valor calculado anteriormente para `x` e o valor calculado para `y` para produzir o resultado da operação.</span><span class="sxs-lookup"><span data-stu-id="70e32-2319">Otherwise, `y` is evaluated, and the selected `operator &` is invoked on the value previously computed for `x` and the value computed for `y` to produce the result of the operation.</span></span>
*  <span data-ttu-id="70e32-2320">A operação `x || y` é avaliado como `T.true(x) ? x : T.|(x, y)`, onde `T.true(x)` é uma invocação dos `operator true` declarado no `T`, e `T.|(x,y)` é uma invocação do selecionado `operator|`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2320">The operation `x || y` is evaluated as `T.true(x) ? x : T.|(x, y)`, where `T.true(x)` is an invocation of the `operator true` declared in `T`, and `T.|(x,y)` is an invocation of the selected `operator|`.</span></span> <span data-ttu-id="70e32-2321">Em outras palavras, `x` é avaliado primeiro e `operator true` é invocado no resultado para determinar se `x` é definitivamente verdade.</span><span class="sxs-lookup"><span data-stu-id="70e32-2321">In other words, `x` is first evaluated and `operator true` is invoked on the result to determine if `x` is definitely true.</span></span> <span data-ttu-id="70e32-2322">Então, se `x` é definitivamente verdade, o resultado da operação é o valor calculado anteriormente para `x`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2322">Then, if `x` is definitely true, the result of the operation is the value previously computed for `x`.</span></span> <span data-ttu-id="70e32-2323">Caso contrário, `y` é avaliada e selecionado `operator |` é invocado no valor calculado anteriormente para `x` e o valor calculado para `y` para produzir o resultado da operação.</span><span class="sxs-lookup"><span data-stu-id="70e32-2323">Otherwise, `y` is evaluated, and the selected `operator |` is invoked on the value previously computed for `x` and the value computed for `y` to produce the result of the operation.</span></span>

<span data-ttu-id="70e32-2324">Em qualquer uma dessas operações, a expressão fornecida pelo `x` só é avaliada uma vez e a expressão fornecida pelo `y` não é avaliada ou avaliada apenas uma vez.</span><span class="sxs-lookup"><span data-stu-id="70e32-2324">In either of these operations, the expression given by `x` is only evaluated once, and the expression given by `y` is either not evaluated or evaluated exactly once.</span></span>

<span data-ttu-id="70e32-2325">Para obter um exemplo de um tipo que implementa `operator true` e `operator false`, consulte [banco de dados tipo booliano](structs.md#database-boolean-type).</span><span class="sxs-lookup"><span data-stu-id="70e32-2325">For an example of a type that implements `operator true` and `operator false`, see [Database boolean type](structs.md#database-boolean-type).</span></span>

## <a name="the-null-coalescing-operator"></a><span data-ttu-id="70e32-2326">O operador de união nulo</span><span class="sxs-lookup"><span data-stu-id="70e32-2326">The null coalescing operator</span></span>

<span data-ttu-id="70e32-2327">O `??` operador é chamado de operador de união nulo.</span><span class="sxs-lookup"><span data-stu-id="70e32-2327">The `??` operator is called the null coalescing operator.</span></span>

```antlr
null_coalescing_expression
    : conditional_or_expression
    | conditional_or_expression '??' null_coalescing_expression
    ;
```

<span data-ttu-id="70e32-2328">Uma expressão de união nula do formulário `a ?? b` requer `a` para ser de um tipo que permite valor nulo de tipo ou referência.</span><span class="sxs-lookup"><span data-stu-id="70e32-2328">A null coalescing expression of the form `a ?? b` requires `a` to be of a nullable type or reference type.</span></span> <span data-ttu-id="70e32-2329">Se `a` não for nulo, o resultado da `a ?? b` é `a`; caso contrário, o resultado é `b`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2329">If `a` is non-null, the result of `a ?? b` is `a`; otherwise, the result is `b`.</span></span> <span data-ttu-id="70e32-2330">A operação avalia `b` somente se `a` é nulo.</span><span class="sxs-lookup"><span data-stu-id="70e32-2330">The operation evaluates `b` only if `a` is null.</span></span>

<span data-ttu-id="70e32-2331">O operador de união nulo é associativo à direita, o que significa que as operações são agrupadas da direita para esquerda.</span><span class="sxs-lookup"><span data-stu-id="70e32-2331">The null coalescing operator is right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="70e32-2332">Por exemplo, uma expressão do formulário `a ?? b ?? c` é avaliada como `a ?? (b ?? c)`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2332">For example, an expression of the form `a ?? b ?? c` is evaluated as `a ?? (b ?? c)`.</span></span> <span data-ttu-id="70e32-2333">Termos em geral, uma expressão do formulário `E1 ?? E2 ?? ... ?? En` retorna o primeiro dos operandos for não nulo, ou nulo se todos os operandos são nulos.</span><span class="sxs-lookup"><span data-stu-id="70e32-2333">In general terms, an expression of the form `E1 ?? E2 ?? ... ?? En` returns the first of the operands that is non-null, or null if all operands are null.</span></span>

<span data-ttu-id="70e32-2334">O tipo da expressão `a ?? b` depende de quais conversões implícitas estão disponíveis nos operandos.</span><span class="sxs-lookup"><span data-stu-id="70e32-2334">The type of the expression `a ?? b` depends on which implicit conversions are available on the operands.</span></span> <span data-ttu-id="70e32-2335">Ordem de preferência, o tipo de `a ?? b` está `A0`, `A`, ou `B`, onde `A` é o tipo de `a` (desde que `a` tem um tipo), `B` é o tipo de `b` ( desde que `b` tem um tipo), e `A0` é o tipo subjacente de `A` se `A` é um tipo anulável, ou `A` caso contrário.</span><span class="sxs-lookup"><span data-stu-id="70e32-2335">In order of preference, the type of `a ?? b` is `A0`, `A`, or `B`, where `A` is the type of `a` (provided that `a` has a type), `B` is the type of `b` (provided that `b` has a type), and `A0` is the underlying type of `A` if `A` is a nullable type, or `A` otherwise.</span></span> <span data-ttu-id="70e32-2336">Especificamente, `a ?? b` é processado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="70e32-2336">Specifically, `a ?? b` is processed as follows:</span></span>

*  <span data-ttu-id="70e32-2337">Se `A` existe e não é um tipo anulável ou um tipo de referência, ocorre um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-2337">If `A` exists and is not a nullable type or a reference type, a compile-time error occurs.</span></span>
*  <span data-ttu-id="70e32-2338">Se `b` é uma expressão dinâmica, o tipo de resultado é `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2338">If `b` is a dynamic expression, the result type is `dynamic`.</span></span> <span data-ttu-id="70e32-2339">Em tempo de execução, `a` é avaliada primeiro.</span><span class="sxs-lookup"><span data-stu-id="70e32-2339">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="70e32-2340">Se `a` não for nulo, `a` é convertido para dinâmico, e isso se torna o resultado.</span><span class="sxs-lookup"><span data-stu-id="70e32-2340">If `a` is not null, `a` is converted to dynamic, and this becomes the result.</span></span> <span data-ttu-id="70e32-2341">Caso contrário, `b` é avaliada, e isso se torna o resultado.</span><span class="sxs-lookup"><span data-stu-id="70e32-2341">Otherwise, `b` is evaluated, and this becomes the result.</span></span>
*  <span data-ttu-id="70e32-2342">Caso contrário, se `A` existe e é um tipo anulável e existe uma conversão implícita da `b` à `A0`, o tipo de resultado é `A0`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2342">Otherwise, if `A` exists and is a nullable type and an implicit conversion exists from `b` to `A0`, the result type is `A0`.</span></span> <span data-ttu-id="70e32-2343">Em tempo de execução, `a` é avaliada primeiro.</span><span class="sxs-lookup"><span data-stu-id="70e32-2343">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="70e32-2344">Se `a` não for null, `a` é desencapsulado para o tipo `A0`, e isso se torna o resultado.</span><span class="sxs-lookup"><span data-stu-id="70e32-2344">If `a` is not null, `a` is unwrapped to type `A0`, and this becomes the result.</span></span> <span data-ttu-id="70e32-2345">Caso contrário, `b` é avaliado e convertido no tipo `A0`, e isso se torna o resultado.</span><span class="sxs-lookup"><span data-stu-id="70e32-2345">Otherwise, `b` is evaluated and converted to type `A0`, and this becomes the result.</span></span>
*  <span data-ttu-id="70e32-2346">Caso contrário, se `A` existe e existe uma conversão implícita da `b` à `A`, o tipo de resultado é `A`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2346">Otherwise, if `A` exists and an implicit conversion exists from `b` to `A`, the result type is `A`.</span></span> <span data-ttu-id="70e32-2347">Em tempo de execução, `a` é avaliada primeiro.</span><span class="sxs-lookup"><span data-stu-id="70e32-2347">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="70e32-2348">Se `a` não for nulo, `a` se tornará o resultado.</span><span class="sxs-lookup"><span data-stu-id="70e32-2348">If `a` is not null, `a` becomes the result.</span></span> <span data-ttu-id="70e32-2349">Caso contrário, `b` é avaliado e convertido no tipo `A`, e isso se torna o resultado.</span><span class="sxs-lookup"><span data-stu-id="70e32-2349">Otherwise, `b` is evaluated and converted to type `A`, and this becomes the result.</span></span>
*  <span data-ttu-id="70e32-2350">Caso contrário, se `b` tem um tipo `B` e existe uma conversão implícita de `a` à `B`, o tipo de resultado é `B`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2350">Otherwise, if `b` has a type `B` and an implicit conversion exists from `a` to `B`, the result type is `B`.</span></span> <span data-ttu-id="70e32-2351">Em tempo de execução, `a` é avaliada primeiro.</span><span class="sxs-lookup"><span data-stu-id="70e32-2351">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="70e32-2352">Se `a` não for null, `a` é desencapsulado para o tipo `A0` (se `A` existe e é anulável) e convertido para o tipo `B`, e isso se torna o resultado.</span><span class="sxs-lookup"><span data-stu-id="70e32-2352">If `a` is not null, `a` is unwrapped to type `A0` (if `A` exists and is nullable) and converted to type `B`, and this becomes the result.</span></span> <span data-ttu-id="70e32-2353">Caso contrário, `b` é avaliada e torna-se o resultado.</span><span class="sxs-lookup"><span data-stu-id="70e32-2353">Otherwise, `b` is evaluated and becomes the result.</span></span>
*  <span data-ttu-id="70e32-2354">Caso contrário, `a` e `b` são incompatíveis e um erro de tempo de compilação ocorre.</span><span class="sxs-lookup"><span data-stu-id="70e32-2354">Otherwise, `a` and `b` are incompatible, and a compile-time error occurs.</span></span>

## <a name="conditional-operator"></a><span data-ttu-id="70e32-2355">Operador condicional</span><span class="sxs-lookup"><span data-stu-id="70e32-2355">Conditional operator</span></span>

<span data-ttu-id="70e32-2356">O `?:` operador é chamado de operador condicional.</span><span class="sxs-lookup"><span data-stu-id="70e32-2356">The `?:` operator is called the conditional operator.</span></span> <span data-ttu-id="70e32-2357">Ele às vezes também é chamado de operador ternário.</span><span class="sxs-lookup"><span data-stu-id="70e32-2357">It is at times also called the ternary operator.</span></span>

```antlr
conditional_expression
    : null_coalescing_expression
    | null_coalescing_expression '?' expression ':' expression
    ;
```

<span data-ttu-id="70e32-2358">Uma expressão condicional do formulário `b ? x : y` primeiro avalia a condição `b`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2358">A conditional expression of the form `b ? x : y` first evaluates the condition `b`.</span></span> <span data-ttu-id="70e32-2359">Então, se `b` está `true`, `x` é avaliada e torna-se o resultado da operação.</span><span class="sxs-lookup"><span data-stu-id="70e32-2359">Then, if `b` is `true`, `x` is evaluated and becomes the result of the operation.</span></span> <span data-ttu-id="70e32-2360">Caso contrário, `y` é avaliada e torna-se o resultado da operação.</span><span class="sxs-lookup"><span data-stu-id="70e32-2360">Otherwise, `y` is evaluated and becomes the result of the operation.</span></span> <span data-ttu-id="70e32-2361">Uma expressão condicional é avaliada nunca ambos `x` e `y`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2361">A conditional expression never evaluates both `x` and `y`.</span></span>

<span data-ttu-id="70e32-2362">O operador condicional é associativo à direita, o que significa que as operações são agrupadas da direita para esquerda.</span><span class="sxs-lookup"><span data-stu-id="70e32-2362">The conditional operator is right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="70e32-2363">Por exemplo, uma expressão do formulário `a ? b : c ? d : e` é avaliada como `a ? b : (c ? d : e)`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2363">For example, an expression of the form `a ? b : c ? d : e` is evaluated as `a ? b : (c ? d : e)`.</span></span>

<span data-ttu-id="70e32-2364">O primeiro operando do `?:` operador deve ser uma expressão que pode ser convertida implicitamente em `bool`, ou uma expressão de um tipo que implementa `operator true`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2364">The first operand of the `?:` operator must be an expression that can be implicitly converted to `bool`, or an expression of a type that implements `operator true`.</span></span> <span data-ttu-id="70e32-2365">Se nenhum desses requisitos é atendido, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-2365">If neither of these requirements is satisfied, a compile-time error occurs.</span></span>

<span data-ttu-id="70e32-2366">O segundo e terceiro operandos, `x` e `y`, da `?:` operador controlam o tipo da expressão condicional.</span><span class="sxs-lookup"><span data-stu-id="70e32-2366">The second and third operands, `x` and `y`, of the `?:` operator control the type of the conditional expression.</span></span>

*  <span data-ttu-id="70e32-2367">Se `x` tem o tipo `X` e `y` tem o tipo `Y` , em seguida,</span><span class="sxs-lookup"><span data-stu-id="70e32-2367">If `x` has type `X` and `y` has type `Y` then</span></span>
   * <span data-ttu-id="70e32-2368">Se uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)) existe a partir de `X` para `Y`, mas não de `Y` para `X`, em seguida, `Y` é o tipo da expressão condicional.</span><span class="sxs-lookup"><span data-stu-id="70e32-2368">If an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from `X` to `Y`, but not from `Y` to `X`, then `Y` is the type of the conditional expression.</span></span>
   * <span data-ttu-id="70e32-2369">Se uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)) existe a partir de `Y` para `X`, mas não de `X` para `Y`, em seguida, `X` é o tipo da expressão condicional.</span><span class="sxs-lookup"><span data-stu-id="70e32-2369">If an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from `Y` to `X`, but not from `X` to `Y`, then `X` is the type of the conditional expression.</span></span>
   * <span data-ttu-id="70e32-2370">Caso contrário, nenhum tipo de expressão pode ser determinado, e ocorre um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-2370">Otherwise, no expression type can be determined, and a compile-time error occurs.</span></span>
*  <span data-ttu-id="70e32-2371">Se apenas um dos `x` e `y` tem um tipo e ambos `x` e `y`, de são implicitamente conversíveis para esse tipo, em seguida, o que é o tipo da expressão condicional.</span><span class="sxs-lookup"><span data-stu-id="70e32-2371">If only one of `x` and `y` has a type, and both `x` and `y`, of are implicitly convertible to that type, then that is the type of the conditional expression.</span></span>
*  <span data-ttu-id="70e32-2372">Caso contrário, nenhum tipo de expressão pode ser determinado, e ocorre um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-2372">Otherwise, no expression type can be determined, and a compile-time error occurs.</span></span>

<span data-ttu-id="70e32-2373">O processamento de tempo de execução de uma expressão condicional do formulário `b ? x : y` consiste as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="70e32-2373">The run-time processing of a conditional expression of the form `b ? x : y` consists of the following steps:</span></span>

*  <span data-ttu-id="70e32-2374">Primeiro, `b` é avaliada e o `bool` valor `b` é determinado:</span><span class="sxs-lookup"><span data-stu-id="70e32-2374">First, `b` is evaluated, and the `bool` value of `b` is determined:</span></span>
   * <span data-ttu-id="70e32-2375">Se uma conversão implícita do tipo de `b` à `bool` existir, essa conversão implícita é executada para produzir um `bool` valor.</span><span class="sxs-lookup"><span data-stu-id="70e32-2375">If an implicit conversion from the type of `b` to `bool` exists, then this implicit conversion is performed to produce a `bool` value.</span></span>
   * <span data-ttu-id="70e32-2376">Caso contrário, o `operator true` definidos pelo tipo de `b` é invocado para produzir um `bool` valor.</span><span class="sxs-lookup"><span data-stu-id="70e32-2376">Otherwise, the `operator true` defined by the type of `b` is invoked to produce a `bool` value.</span></span>
*  <span data-ttu-id="70e32-2377">Se o `bool` valor produzido pela etapa anterior está `true`, em seguida, `x` é avaliada e convertida no tipo de expressão condicional, e isso se torna o resultado da expressão condicional.</span><span class="sxs-lookup"><span data-stu-id="70e32-2377">If the `bool` value produced by the step above is `true`, then `x` is evaluated and converted to the type of the conditional expression, and this becomes the result of the conditional expression.</span></span>
*  <span data-ttu-id="70e32-2378">Caso contrário, `y` é avaliada e convertida no tipo de expressão condicional, e isso se torna o resultado da expressão condicional.</span><span class="sxs-lookup"><span data-stu-id="70e32-2378">Otherwise, `y` is evaluated and converted to the type of the conditional expression, and this becomes the result of the conditional expression.</span></span>

## <a name="anonymous-function-expressions"></a><span data-ttu-id="70e32-2379">Expressões de função anônima</span><span class="sxs-lookup"><span data-stu-id="70e32-2379">Anonymous function expressions</span></span>

<span data-ttu-id="70e32-2380">Uma ***função anônima*** é uma expressão que representa uma definição de método "na linha".</span><span class="sxs-lookup"><span data-stu-id="70e32-2380">An ***anonymous function*** is an expression that represents an "in-line" method definition.</span></span> <span data-ttu-id="70e32-2381">Uma função anônima não tem um valor ou tipo por si só, mas pode ser convertido em um tipo de árvore de expressão ou delegado compatível.</span><span class="sxs-lookup"><span data-stu-id="70e32-2381">An anonymous function does not have a value or type in and of itself, but is convertible to a compatible delegate or expression tree type.</span></span> <span data-ttu-id="70e32-2382">A avaliação de uma conversão de função anônima depende do tipo de destino da conversão: se for um tipo de delegado, a conversão é avaliada como um valor de delegado fizer referência ao método que define a função anônima.</span><span class="sxs-lookup"><span data-stu-id="70e32-2382">The evaluation of an anonymous function conversion depends on the target type of the conversion: If it is a delegate type, the conversion evaluates to a delegate value referencing the method which the anonymous function defines.</span></span> <span data-ttu-id="70e32-2383">Se for um tipo de árvore de expressão, a conversão é avaliada como uma árvore de expressão que representa a estrutura do método como uma estrutura de objeto.</span><span class="sxs-lookup"><span data-stu-id="70e32-2383">If it is an expression tree type, the conversion evaluates to an expression tree which represents the structure of the method as an object structure.</span></span>

<span data-ttu-id="70e32-2384">Por razões históricas existem dois tipos sintáticos de funções anônimas, ou seja, *lambda_expression*s e *anonymous_method_expression*s.</span><span class="sxs-lookup"><span data-stu-id="70e32-2384">For historical reasons there are two syntactic flavors of anonymous functions, namely *lambda_expression*s and *anonymous_method_expression*s.</span></span> <span data-ttu-id="70e32-2385">Para fins de quase todos os *lambda_expression*s são mais concisas e expressivo que *anonymous_method_expression*s, que permanecem no idioma para compatibilidade com versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="70e32-2385">For almost all purposes, *lambda_expression*s are more concise and expressive than *anonymous_method_expression*s, which remain in the language for backwards compatibility.</span></span>

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

<span data-ttu-id="70e32-2386">O `=>` operador tem a mesma precedência que a atribuição (`=`) e é associativo à direita.</span><span class="sxs-lookup"><span data-stu-id="70e32-2386">The `=>` operator has the same precedence as assignment (`=`) and is right-associative.</span></span>

<span data-ttu-id="70e32-2387">Uma função anônima com o `async` modificador é uma função assíncrona e segue as regras descritas em [iteradores](classes.md#iterators).</span><span class="sxs-lookup"><span data-stu-id="70e32-2387">An anonymous function with the `async` modifier is an async function and follows the rules described in [Iterators](classes.md#iterators).</span></span>

<span data-ttu-id="70e32-2388">Os parâmetros de uma função anônima na forma de um *lambda_expression* podem ser explicitamente ou implicitamente digitados.</span><span class="sxs-lookup"><span data-stu-id="70e32-2388">The parameters of an anonymous function in the form of a *lambda_expression* can be explicitly or implicitly typed.</span></span> <span data-ttu-id="70e32-2389">Em uma lista de parâmetros de tipo explícito, o tipo de cada parâmetro é explicitamente declarado.</span><span class="sxs-lookup"><span data-stu-id="70e32-2389">In an explicitly typed parameter list, the type of each parameter is explicitly stated.</span></span> <span data-ttu-id="70e32-2390">Em uma lista de parâmetros de tipo implícito, os tipos dos parâmetros são inferidos do contexto no qual ocorre a função anônima — especificamente, quando a função anônima é convertida em um tipo de delegado compatível ou tipo de árvore de expressão, que fornece o tipo os tipos de parâmetro ([conversões de função anônima](conversions.md#anonymous-function-conversions)).</span><span class="sxs-lookup"><span data-stu-id="70e32-2390">In an implicitly typed parameter list, the types of the parameters are inferred from the context in which the anonymous function occurs—specifically, when the anonymous function is converted to a compatible delegate type or expression tree type, that type provides the parameter types ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>

<span data-ttu-id="70e32-2391">Em uma função anônima com um parâmetro de tipo implícito, único, os parênteses que podem ser omitidos da lista de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="70e32-2391">In an anonymous function with a single, implicitly typed parameter, the parentheses may be omitted from the parameter list.</span></span> <span data-ttu-id="70e32-2392">Em outras palavras, uma função anônima do formulário</span><span class="sxs-lookup"><span data-stu-id="70e32-2392">In other words, an anonymous function of the form</span></span>
```csharp
( param ) => expr
```
<span data-ttu-id="70e32-2393">pode ser abreviado como</span><span class="sxs-lookup"><span data-stu-id="70e32-2393">can be abbreviated to</span></span>
```csharp
param => expr
```

<span data-ttu-id="70e32-2394">Lista de parâmetros de uma função anônima na forma de um *anonymous_method_expression* é opcional.</span><span class="sxs-lookup"><span data-stu-id="70e32-2394">The parameter list of an anonymous function in the form of an *anonymous_method_expression* is optional.</span></span> <span data-ttu-id="70e32-2395">Se for especificado, os parâmetros devem ser digitados explicitamente.</span><span class="sxs-lookup"><span data-stu-id="70e32-2395">If given, the parameters must be explicitly typed.</span></span> <span data-ttu-id="70e32-2396">Se não, a função anônima é convertida em um delegado com qualquer parâmetro de lista não contêm `out` parâmetros.</span><span class="sxs-lookup"><span data-stu-id="70e32-2396">If not, the anonymous function is convertible to a delegate with any parameter list not containing `out` parameters.</span></span>

<span data-ttu-id="70e32-2397">Um *bloco* corpo de uma função anônima é acessível ([pontos de extremidade e acessibilidade](statements.md#end-points-and-reachability)), a menos que a função anônima ocorre dentro de uma instrução inacessível.</span><span class="sxs-lookup"><span data-stu-id="70e32-2397">A *block* body of an anonymous function is reachable ([End points and reachability](statements.md#end-points-and-reachability)) unless the anonymous function occurs inside an unreachable statement.</span></span>

<span data-ttu-id="70e32-2398">Seguem alguns exemplos de funções anônimas abaixo:</span><span class="sxs-lookup"><span data-stu-id="70e32-2398">Some examples of anonymous functions follow below:</span></span>

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

<span data-ttu-id="70e32-2399">O comportamento de *lambda_expression*s e *anonymous_method_expression*s é o mesmo, exceto para os seguintes pontos:</span><span class="sxs-lookup"><span data-stu-id="70e32-2399">The behavior of *lambda_expression*s and *anonymous_method_expression*s is the same except for the following points:</span></span>

*  <span data-ttu-id="70e32-2400">*anonymous_method_expression*s permitir que a lista de parâmetros a serem omitidos totalmente, produzindo a convertibilidade de tipos de qualquer lista de parâmetros com valor de delegado.</span><span class="sxs-lookup"><span data-stu-id="70e32-2400">*anonymous_method_expression*s permit the parameter list to be omitted entirely, yielding convertibility to delegate types of any list of value parameters.</span></span>
*  <span data-ttu-id="70e32-2401">*lambda_expression*s permite que os tipos de parâmetro seja omitido e inferido, enquanto *anonymous_method_expression*s exigem tipos de parâmetro a ser declarado explicitamente.</span><span class="sxs-lookup"><span data-stu-id="70e32-2401">*lambda_expression*s permit parameter types to be omitted and inferred whereas *anonymous_method_expression*s require parameter types to be explicitly stated.</span></span>
*  <span data-ttu-id="70e32-2402">O corpo de uma *lambda_expression* pode ser uma expressão ou um bloco de instruções enquanto o corpo de uma *anonymous_method_expression* deve ser um bloco de instruções.</span><span class="sxs-lookup"><span data-stu-id="70e32-2402">The body of a *lambda_expression* can be an expression or a statement block whereas the body of an *anonymous_method_expression* must be a statement block.</span></span>
*  <span data-ttu-id="70e32-2403">Somente *lambda_expression*s têm conversões para tipos de árvore de expressão compatível ([tipos de árvore de expressão](types.md#expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="70e32-2403">Only *lambda_expression*s have conversions to compatible expression tree types ([Expression tree types](types.md#expression-tree-types)).</span></span>

### <a name="anonymous-function-signatures"></a><span data-ttu-id="70e32-2404">Assinaturas de função anônima</span><span class="sxs-lookup"><span data-stu-id="70e32-2404">Anonymous function signatures</span></span>

<span data-ttu-id="70e32-2405">Opcional *anonymous_function_signature* de uma função anônima define os nomes e, opcionalmente, os tipos dos parâmetros formais para a função anônima.</span><span class="sxs-lookup"><span data-stu-id="70e32-2405">The optional *anonymous_function_signature* of an anonymous function defines the names and optionally the types of the formal parameters for the anonymous function.</span></span> <span data-ttu-id="70e32-2406">O escopo dos parâmetros de função anônima que é o *anonymous_function_body*.</span><span class="sxs-lookup"><span data-stu-id="70e32-2406">The scope of the parameters of the anonymous function is the *anonymous_function_body*.</span></span> <span data-ttu-id="70e32-2407">([Escopos](basic-concepts.md#scopes)) junto com a lista de parâmetros (se fornecida) anônima-corpo do método-constitui um espaço de declaração ([declarações](basic-concepts.md#declarations)).</span><span class="sxs-lookup"><span data-stu-id="70e32-2407">([Scopes](basic-concepts.md#scopes)) Together with the parameter list (if given) the anonymous-method-body constitutes a declaration space ([Declarations](basic-concepts.md#declarations)).</span></span> <span data-ttu-id="70e32-2408">Ele é, portanto, um erro de tempo de compilação para o nome de um parâmetro da função anônima para corresponder ao nome de uma variável local, a constante local ou parâmetro cujo escopo inclui o *anonymous_method_expression* ou *lambda_ expressão*.</span><span class="sxs-lookup"><span data-stu-id="70e32-2408">It is thus a compile-time error for the name of a parameter of the anonymous function to match the name of a local variable, local constant or parameter whose scope includes the *anonymous_method_expression* or *lambda_expression*.</span></span>

<span data-ttu-id="70e32-2409">Se uma função anônima tem uma *explicit_anonymous_function_signature*, em seguida, o conjunto de tipos de delegado compatível e tipos de árvore de expressão é restrito para aqueles que têm os mesmos tipos de parâmetro e modificadores na mesma ordem.</span><span class="sxs-lookup"><span data-stu-id="70e32-2409">If an anonymous function has an *explicit_anonymous_function_signature*, then the set of compatible delegate types and expression tree types is restricted to those that have the same parameter types and modifiers in the same order.</span></span> <span data-ttu-id="70e32-2410">Em contraste com conversões de grupo de método ([conversões de grupo de método](conversions.md#method-group-conversions)), não há suporte para a contravariância dos tipos de parâmetro de função anônima.</span><span class="sxs-lookup"><span data-stu-id="70e32-2410">In contrast to method group conversions ([Method group conversions](conversions.md#method-group-conversions)), contra-variance of anonymous function parameter types is not supported.</span></span> <span data-ttu-id="70e32-2411">Se uma função anônima não tem um *anonymous_function_signature*, em seguida, o conjunto de tipos de delegado compatível e tipos de árvore de expressão é restrito para aqueles que não têm nenhum `out` parâmetros.</span><span class="sxs-lookup"><span data-stu-id="70e32-2411">If an anonymous function does not have an *anonymous_function_signature*, then the set of compatible delegate types and expression tree types is restricted to those that have no `out` parameters.</span></span>

<span data-ttu-id="70e32-2412">Observe que um *anonymous_function_signature* não pode incluir atributos ou uma matriz de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="70e32-2412">Note that an *anonymous_function_signature* cannot include attributes or a parameter array.</span></span> <span data-ttu-id="70e32-2413">No entanto, uma *anonymous_function_signature* podem ser compatíveis com um tipo de delegado cuja lista de parâmetros contém uma matriz de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="70e32-2413">Nevertheless, an *anonymous_function_signature* may be compatible with a delegate type whose parameter list contains a parameter array.</span></span>

<span data-ttu-id="70e32-2414">Observe também que conversão para um tipo de árvore de expressão, mesmo se compatível, ainda pode falhar em tempo de compilação ([tipos de árvore de expressão](types.md#expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="70e32-2414">Note also that conversion to an expression tree type, even if compatible, may still fail at compile-time ([Expression tree types](types.md#expression-tree-types)).</span></span>

### <a name="anonymous-function-bodies"></a><span data-ttu-id="70e32-2415">Corpos de função anônima</span><span class="sxs-lookup"><span data-stu-id="70e32-2415">Anonymous function bodies</span></span>

<span data-ttu-id="70e32-2416">O corpo (*expressão* ou *bloco*) de uma função anônima é sujeito às seguintes regras:</span><span class="sxs-lookup"><span data-stu-id="70e32-2416">The body (*expression* or *block*) of an anonymous function is subject to the following rules:</span></span>

*  <span data-ttu-id="70e32-2417">Se a função anônima inclui uma assinatura, os parâmetros especificados na assinatura estão disponíveis no corpo.</span><span class="sxs-lookup"><span data-stu-id="70e32-2417">If the anonymous function includes a signature, the parameters specified in the signature are available in the body.</span></span> <span data-ttu-id="70e32-2418">Se a função anônima não tiver uma assinatura pode ser convertido para um tipo de delegado ou ter parâmetros de tipo de expressão ([conversões de função anônima](conversions.md#anonymous-function-conversions)), mas os parâmetros não podem ser acessados no corpo.</span><span class="sxs-lookup"><span data-stu-id="70e32-2418">If the anonymous function has no signature it can be converted to a delegate type or expression type having parameters ([Anonymous function conversions](conversions.md#anonymous-function-conversions)), but the parameters cannot be accessed in the body.</span></span>
*  <span data-ttu-id="70e32-2419">Exceto para `ref` ou `out` parâmetros especificados na assinatura (se houver) do delimitador mais próximo função anônima, ele é um erro de tempo de compilação para o corpo acessar um `ref` ou `out` parâmetro.</span><span class="sxs-lookup"><span data-stu-id="70e32-2419">Except for `ref` or `out` parameters specified in the signature (if any) of the nearest enclosing anonymous function, it is a compile-time error for the body to access a `ref` or `out` parameter.</span></span>
*  <span data-ttu-id="70e32-2420">Quando o tipo de `this` é um tipo de struct, ele é um erro de tempo de compilação para o corpo acessar `this`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2420">When the type of `this` is a struct type, it is a compile-time error for the body to access `this`.</span></span> <span data-ttu-id="70e32-2421">Isso é verdadeiro se o acesso é explícito (como em `this.x`) ou implícita (como na `x` onde `x` é um membro de instância do struct).</span><span class="sxs-lookup"><span data-stu-id="70e32-2421">This is true whether the access is explicit (as in `this.x`) or implicit (as in `x` where `x` is an instance member of the struct).</span></span> <span data-ttu-id="70e32-2422">Essa regra simplesmente proíbe tal acesso e não afeta se a pesquisa de membro resulta em um membro do struct.</span><span class="sxs-lookup"><span data-stu-id="70e32-2422">This rule simply prohibits such access and does not affect whether member lookup results in a member of the struct.</span></span>
*  <span data-ttu-id="70e32-2423">O corpo da tem acesso a variáveis externas ([Outer variáveis](expressions.md#outer-variables)) da função anônima.</span><span class="sxs-lookup"><span data-stu-id="70e32-2423">The body has access to the outer variables ([Outer variables](expressions.md#outer-variables)) of the anonymous function.</span></span> <span data-ttu-id="70e32-2424">Acesso de uma variável externa fará referência a instância da variável que está ativo no momento a *lambda_expression* ou *anonymous_method_expression* é avaliada ([avaliação do expressões de função anônima](expressions.md#evaluation-of-anonymous-function-expressions)).</span><span class="sxs-lookup"><span data-stu-id="70e32-2424">Access of an outer variable will reference the instance of the variable that is active at the time the *lambda_expression* or *anonymous_method_expression* is evaluated ([Evaluation of anonymous function expressions](expressions.md#evaluation-of-anonymous-function-expressions)).</span></span>
*  <span data-ttu-id="70e32-2425">É um erro de tempo de compilação para o corpo conter um `goto` instrução `break` instrução, ou `continue` cujo destino está fora do corpo ou dentro do corpo de uma função anônima contida.</span><span class="sxs-lookup"><span data-stu-id="70e32-2425">It is a compile-time error for the body to contain a `goto` statement, `break` statement, or `continue` statement whose target is outside the body or within the body of a contained anonymous function.</span></span>
*  <span data-ttu-id="70e32-2426">Um `return` no corpo da declaração de uma invocação de delimitadora mais próxima função anônima, não do membro da função de circunscrição.</span><span class="sxs-lookup"><span data-stu-id="70e32-2426">A `return` statement in the body returns control from an invocation of the nearest enclosing anonymous function, not from the enclosing function member.</span></span> <span data-ttu-id="70e32-2427">Uma expressão especificada em uma `return` instrução deve ser implicitamente conversível para o tipo de retorno do tipo de delegado ou tipo de árvore de expressão para o qual delimitadora mais próxima *lambda_expression* ou *anonymous_ method_expression* é convertido ([conversões de função anônima](conversions.md#anonymous-function-conversions)).</span><span class="sxs-lookup"><span data-stu-id="70e32-2427">An expression specified in a `return` statement must be implicitly convertible to the return type of the delegate type or expression tree type to which the nearest enclosing *lambda_expression* or *anonymous_method_expression* is converted ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>

<span data-ttu-id="70e32-2428">Ele não é especificado explicitamente se há alguma maneira de executar o bloco de uma função anônima diferente por meio de avaliação e invocação do *lambda_expression* ou *anonymous_method_expression*.</span><span class="sxs-lookup"><span data-stu-id="70e32-2428">It is explicitly unspecified whether there is any way to execute the block of an anonymous function other than through evaluation and invocation of the *lambda_expression* or *anonymous_method_expression*.</span></span> <span data-ttu-id="70e32-2429">Em particular, o compilador pode optar por implementar uma função anônima por sintetizar um ou mais nomeadas tipos ou métodos.</span><span class="sxs-lookup"><span data-stu-id="70e32-2429">In particular, the compiler may choose to implement an anonymous function by synthesizing one or more named methods or types.</span></span> <span data-ttu-id="70e32-2430">Os nomes de todos esses elementos sintetizados devem ser de um formulário reservado para uso pelo compilador.</span><span class="sxs-lookup"><span data-stu-id="70e32-2430">The names of any such synthesized elements must be of a form reserved for compiler use.</span></span>

### <a name="overload-resolution-and-anonymous-functions"></a><span data-ttu-id="70e32-2431">Resolução de sobrecarga e funções anônimas</span><span class="sxs-lookup"><span data-stu-id="70e32-2431">Overload resolution and anonymous functions</span></span>

<span data-ttu-id="70e32-2432">Funções anônimas em uma lista de argumentos participarem de inferência de tipo e resolução de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="70e32-2432">Anonymous functions in an argument list participate in type inference and overload resolution.</span></span> <span data-ttu-id="70e32-2433">Consulte a [inferência](expressions.md#type-inference) e [resolução de sobrecarga](expressions.md#overload-resolution) para as regras exatas.</span><span class="sxs-lookup"><span data-stu-id="70e32-2433">Please refer to [Type inference](expressions.md#type-inference) and [Overload resolution](expressions.md#overload-resolution) for the exact rules.</span></span>

<span data-ttu-id="70e32-2434">O exemplo a seguir ilustra o efeito de funções anônimas em resolução de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="70e32-2434">The following example illustrates the effect of anonymous functions on overload resolution.</span></span>

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

<span data-ttu-id="70e32-2435">O `ItemList<T>` classe tem duas `Sum` métodos.</span><span class="sxs-lookup"><span data-stu-id="70e32-2435">The `ItemList<T>` class has two `Sum` methods.</span></span> <span data-ttu-id="70e32-2436">Cada um leva um `selector` argumento, que extrai o valor a soma ao longo de um item de lista.</span><span class="sxs-lookup"><span data-stu-id="70e32-2436">Each takes a `selector` argument, which extracts the value to sum over from a list item.</span></span> <span data-ttu-id="70e32-2437">O valor extraído pode ser um `int` ou um `double` e a soma resultante é da mesma forma uma `int` ou um `double`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2437">The extracted value can be either an `int` or a `double` and the resulting sum is likewise either an `int` or a `double`.</span></span>

<span data-ttu-id="70e32-2438">O `Sum` métodos, por exemplo poderiam ser usados para calcular somas em uma lista de linhas de detalhes em uma ordem.</span><span class="sxs-lookup"><span data-stu-id="70e32-2438">The `Sum` methods could for example be used to compute sums from a list of detail lines in an order.</span></span>

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

<span data-ttu-id="70e32-2439">Na primeira invocação de `orderDetails.Sum`, ambos `Sum` métodos são aplicáveis porque a função anônima `d => d. UnitCount` é compatível com ambos `Func<Detail,int>` e `Func<Detail,double>`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2439">In the first invocation of `orderDetails.Sum`, both `Sum` methods are applicable because the anonymous function `d => d. UnitCount` is compatible with both `Func<Detail,int>` and `Func<Detail,double>`.</span></span> <span data-ttu-id="70e32-2440">No entanto, a resolução de sobrecarga seleciona a primeira `Sum` método porque a conversão para o `Func<Detail,int>` é melhor do que a conversão em `Func<Detail,double>`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2440">However, overload resolution picks the first `Sum` method because the conversion to `Func<Detail,int>` is better than the conversion to `Func<Detail,double>`.</span></span>

<span data-ttu-id="70e32-2441">Na segunda chamada de `orderDetails.Sum`, apenas o segundo `Sum` método é aplicável porque a função anônima `d => d.UnitPrice * d.UnitCount` produz um valor do tipo `double`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2441">In the second invocation of `orderDetails.Sum`, only the second `Sum` method is applicable because the anonymous function `d => d.UnitPrice * d.UnitCount` produces a value of type `double`.</span></span> <span data-ttu-id="70e32-2442">Portanto, sobrecarregar resolução escolhe o segundo `Sum` método para essa invocação.</span><span class="sxs-lookup"><span data-stu-id="70e32-2442">Thus, overload resolution picks the second `Sum` method for that invocation.</span></span>

### <a name="anonymous-functions-and-dynamic-binding"></a><span data-ttu-id="70e32-2443">Funções anônimas e vinculação dinâmica</span><span class="sxs-lookup"><span data-stu-id="70e32-2443">Anonymous functions and dynamic binding</span></span>

<span data-ttu-id="70e32-2444">Uma função anônima não pode ser um receptor, o argumento ou operando de uma operação dinâmica associada.</span><span class="sxs-lookup"><span data-stu-id="70e32-2444">An anonymous function cannot be a receiver, argument or operand of a dynamically bound operation.</span></span>

### <a name="outer-variables"></a><span data-ttu-id="70e32-2445">Variáveis externas</span><span class="sxs-lookup"><span data-stu-id="70e32-2445">Outer variables</span></span>

<span data-ttu-id="70e32-2446">Qualquer variável local, um parâmetro de valor ou uma matriz de parâmetros cujo escopo inclui o *lambda_expression* ou *anonymous_method_expression* é chamado um ***variável externa*** da função anônima.</span><span class="sxs-lookup"><span data-stu-id="70e32-2446">Any local variable, value parameter, or parameter array whose scope includes the *lambda_expression* or *anonymous_method_expression* is called an ***outer variable*** of the anonymous function.</span></span> <span data-ttu-id="70e32-2447">Em um membro da função de instância de uma classe, o `this` valor é considerado um parâmetro de valor e é uma variável externa de qualquer função anônima contida dentro do membro da função.</span><span class="sxs-lookup"><span data-stu-id="70e32-2447">In an instance function member of a class, the `this` value is considered a value parameter and is an outer variable of any anonymous function contained within the function member.</span></span>

#### <a name="captured-outer-variables"></a><span data-ttu-id="70e32-2448">Capturado variáveis externas</span><span class="sxs-lookup"><span data-stu-id="70e32-2448">Captured outer variables</span></span>

<span data-ttu-id="70e32-2449">Quando uma variável externa é referenciada por uma função anônima, a variável externa deve ter sido ***capturados*** pela função anônima.</span><span class="sxs-lookup"><span data-stu-id="70e32-2449">When an outer variable is referenced by an anonymous function, the outer variable is said to have been ***captured*** by the anonymous function.</span></span> <span data-ttu-id="70e32-2450">Normalmente, o tempo de vida de uma variável local é limitado à execução de instrução ao qual ele está associado ou bloco ([variáveis locais](variables.md#local-variables)).</span><span class="sxs-lookup"><span data-stu-id="70e32-2450">Ordinarily, the lifetime of a local variable is limited to execution of the block or statement with which it is associated ([Local variables](variables.md#local-variables)).</span></span> <span data-ttu-id="70e32-2451">No entanto, o tempo de vida de uma variável externa capturada é estendido pelo menos até que o delegado ou árvore de expressão criada a partir da função anônima se qualifique para coleta de lixo.</span><span class="sxs-lookup"><span data-stu-id="70e32-2451">However, the lifetime of a captured outer variable is extended at least until the delegate or expression tree created from the anonymous function becomes eligible for garbage collection.</span></span>

<span data-ttu-id="70e32-2452">No exemplo</span><span class="sxs-lookup"><span data-stu-id="70e32-2452">In the example</span></span>
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
<span data-ttu-id="70e32-2453">a variável local `x` for capturada, a função anônima e a vida útil do `x` é estendido pelo menos até que o delegado retornado `F` se qualifique para coleta de lixo (que não acontece até o final do o programa).</span><span class="sxs-lookup"><span data-stu-id="70e32-2453">the local variable `x` is captured by the anonymous function, and the lifetime of `x` is extended at least until the delegate returned from `F` becomes eligible for garbage collection (which doesn't happen until the very end of the program).</span></span> <span data-ttu-id="70e32-2454">Uma vez que cada invocação de função anônima que opera na mesma instância de `x`, a saída do exemplo é:</span><span class="sxs-lookup"><span data-stu-id="70e32-2454">Since each invocation of the anonymous function operates on the same instance of `x`, the output of the example is:</span></span>
```
1
2
3
```

<span data-ttu-id="70e32-2455">Quando uma variável local ou um parâmetro de valor é capturado por uma função anônima, o parâmetro ou variável local não é mais considerado para ser uma variável fixa ([fixo e variáveis moveable](unsafe-code.md#fixed-and-moveable-variables)), mas em vez disso, é considerado como um moveable variável.</span><span class="sxs-lookup"><span data-stu-id="70e32-2455">When a local variable or a value parameter is captured by an anonymous function, the local variable or parameter is no longer considered to be a fixed variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)), but is instead considered to be a moveable variable.</span></span> <span data-ttu-id="70e32-2456">Assim, qualquer `unsafe` código que usa o endereço de uma variável externa capturada primeiro deve usar o `fixed` instrução para corrigir a variável.</span><span class="sxs-lookup"><span data-stu-id="70e32-2456">Thus any `unsafe` code that takes the address of a captured outer variable must first use the `fixed` statement to fix the variable.</span></span>

<span data-ttu-id="70e32-2457">Observe que, ao contrário de uma variável uncaptured, uma variável de local capturada pode ser exposta simultaneamente para vários threads de execução.</span><span class="sxs-lookup"><span data-stu-id="70e32-2457">Note that unlike an uncaptured variable, a captured local variable can be simultaneously exposed to multiple threads of execution.</span></span>

#### <a name="instantiation-of-local-variables"></a><span data-ttu-id="70e32-2458">Instanciação de variáveis locais</span><span class="sxs-lookup"><span data-stu-id="70e32-2458">Instantiation of local variables</span></span>

<span data-ttu-id="70e32-2459">Uma variável local é considerada ***instanciado*** quando execução entra no escopo da variável.</span><span class="sxs-lookup"><span data-stu-id="70e32-2459">A local variable is considered to be ***instantiated*** when execution enters the scope of the variable.</span></span> <span data-ttu-id="70e32-2460">Por exemplo, quando o método a seguir é invocado, a variável local `x` é instanciada e inicializado três vezes — uma vez para cada iteração do loop.</span><span class="sxs-lookup"><span data-stu-id="70e32-2460">For example, when the following method is invoked, the local variable `x` is instantiated and initialized three times—once for each iteration of the loop.</span></span>

```csharp
static void F() {
    for (int i = 0; i < 3; i++) {
        int x = i * 2 + 1;
        ...
    }
}
```

<span data-ttu-id="70e32-2461">No entanto, mover a declaração de `x` fora os resultados de loop em uma única instanciação de `x`:</span><span class="sxs-lookup"><span data-stu-id="70e32-2461">However, moving the declaration of `x` outside the loop results in a single instantiation of `x`:</span></span>
```csharp
static void F() {
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        ...
    }
}
```

<span data-ttu-id="70e32-2462">Quando não capturada, não há nenhuma maneira de observar exatamente a frequência com que uma variável local é instanciada — como os tempos de vida as instanciações são não contíguos, é possível para cada instanciação simplesmente usar o mesmo local de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="70e32-2462">When not captured, there is no way to observe exactly how often a local variable is instantiated—because the lifetimes of the instantiations are disjoint, it is possible for each instantiation to simply use the same storage location.</span></span> <span data-ttu-id="70e32-2463">No entanto, quando uma função anônima captura uma variável local, os efeitos de instanciação se tornam aparentes.</span><span class="sxs-lookup"><span data-stu-id="70e32-2463">However, when an anonymous function captures a local variable, the effects of instantiation become apparent.</span></span>

<span data-ttu-id="70e32-2464">O exemplo</span><span class="sxs-lookup"><span data-stu-id="70e32-2464">The example</span></span>
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
<span data-ttu-id="70e32-2465">produz a saída:</span><span class="sxs-lookup"><span data-stu-id="70e32-2465">produces the output:</span></span>
```
1
3
5
```

<span data-ttu-id="70e32-2466">No entanto, quando a declaração de `x` é movido para fora do loop:</span><span class="sxs-lookup"><span data-stu-id="70e32-2466">However, when the declaration of `x` is moved outside the loop:</span></span>
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
<span data-ttu-id="70e32-2467">A saída é:</span><span class="sxs-lookup"><span data-stu-id="70e32-2467">the output is:</span></span>
```
5
5
5
```

<span data-ttu-id="70e32-2468">Se um loop for declarar uma variável de iteração, essa variável em si é considerado declarado fora do loop.</span><span class="sxs-lookup"><span data-stu-id="70e32-2468">If a for-loop declares an iteration variable, that variable itself is considered to be declared outside of the loop.</span></span> <span data-ttu-id="70e32-2469">Portanto, se o exemplo é modificado para capturar a variável de iteração:</span><span class="sxs-lookup"><span data-stu-id="70e32-2469">Thus, if the example is changed to capture the iteration variable itself:</span></span>

```csharp
static D[] F() {
    D[] result = new D[3];
    for (int i = 0; i < 3; i++) {
        result[i] = () => { Console.WriteLine(i); };
    }
    return result;
}
```
<span data-ttu-id="70e32-2470">apenas uma instância de variável de iteração é capturada, que produz a saída:</span><span class="sxs-lookup"><span data-stu-id="70e32-2470">only one instance of the iteration variable is captured, which produces the output:</span></span>
```
3
3
3
```

<span data-ttu-id="70e32-2471">É possível que os delegados de função anônima para compartilhar algumas variáveis capturadas ainda tem instâncias separadas de outras pessoas.</span><span class="sxs-lookup"><span data-stu-id="70e32-2471">It is possible for anonymous function delegates to share some captured variables yet have separate instances of others.</span></span> <span data-ttu-id="70e32-2472">Por exemplo, se `F` é alterado para</span><span class="sxs-lookup"><span data-stu-id="70e32-2472">For example, if `F` is changed to</span></span>
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
<span data-ttu-id="70e32-2473">os três delegados capturam a mesma instância do `x` mas instâncias separadas do `y`, e a saída é:</span><span class="sxs-lookup"><span data-stu-id="70e32-2473">the three delegates capture the same instance of `x` but separate instances of `y`, and the output is:</span></span>
```
1 1
2 1
3 1
```

<span data-ttu-id="70e32-2474">Funções anônimas separadas podem capturar a mesma instância de uma variável externa.</span><span class="sxs-lookup"><span data-stu-id="70e32-2474">Separate anonymous functions can capture the same instance of an outer variable.</span></span> <span data-ttu-id="70e32-2475">No exemplo:</span><span class="sxs-lookup"><span data-stu-id="70e32-2475">In the example:</span></span>
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
<span data-ttu-id="70e32-2476">as duas funções anônimas capturam a mesma instância da variável local `x`, e eles podem, portanto, "se comunicar" por meio dessa variável.</span><span class="sxs-lookup"><span data-stu-id="70e32-2476">the two anonymous functions capture the same instance of the local variable `x`, and they can thus "communicate" through that variable.</span></span> <span data-ttu-id="70e32-2477">A saída do exemplo é:</span><span class="sxs-lookup"><span data-stu-id="70e32-2477">The output of the example is:</span></span>
```
5
10
```

### <a name="evaluation-of-anonymous-function-expressions"></a><span data-ttu-id="70e32-2478">Avaliação de expressões de função anônima</span><span class="sxs-lookup"><span data-stu-id="70e32-2478">Evaluation of anonymous function expressions</span></span>

<span data-ttu-id="70e32-2479">Uma função anônima `F` sempre deve ser convertido em um tipo de delegado `D` ou um tipo de árvore de expressão `E`, diretamente ou por meio da execução de uma expressão de criação de delegado `new D(F)`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2479">An anonymous function `F` must always be converted to a delegate type `D` or an expression tree type `E`, either directly or through the execution of a delegate creation expression `new D(F)`.</span></span> <span data-ttu-id="70e32-2480">Essa conversão determina o resultado da função anônima, conforme descrito na [conversões de função anônima](conversions.md#anonymous-function-conversions).</span><span class="sxs-lookup"><span data-stu-id="70e32-2480">This conversion determines the result of the anonymous function, as described in [Anonymous function conversions](conversions.md#anonymous-function-conversions).</span></span>

## <a name="query-expressions"></a><span data-ttu-id="70e32-2481">Expressões de consulta</span><span class="sxs-lookup"><span data-stu-id="70e32-2481">Query expressions</span></span>

<span data-ttu-id="70e32-2482">***Expressões de consulta*** fornecem uma sintaxe de linguagem integrada para consultas que é semelhante a linguagens de consulta relacionais e hierárquicas, como SQL e XQuery.</span><span class="sxs-lookup"><span data-stu-id="70e32-2482">***Query expressions*** provide a language integrated syntax for queries that is similar to relational and hierarchical query languages such as SQL and XQuery.</span></span>

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

<span data-ttu-id="70e32-2483">Uma expressão de consulta começa com um `from` cláusula e termina com um uma `select` ou `group` cláusula.</span><span class="sxs-lookup"><span data-stu-id="70e32-2483">A query expression begins with a `from` clause and ends with either a `select` or `group` clause.</span></span> <span data-ttu-id="70e32-2484">Inicial `from` cláusula pode ser seguida por zero ou mais `from`, `let`, `where`, `join` ou `orderby` cláusulas.</span><span class="sxs-lookup"><span data-stu-id="70e32-2484">The initial `from` clause can be followed by zero or more `from`, `let`, `where`, `join` or `orderby` clauses.</span></span> <span data-ttu-id="70e32-2485">Cada `from` cláusula é um gerador de introduzir uma ***variável de intervalo*** que abrange os elementos de uma ***sequência***.</span><span class="sxs-lookup"><span data-stu-id="70e32-2485">Each `from` clause is a generator introducing a ***range variable*** which ranges over the elements of a ***sequence***.</span></span> <span data-ttu-id="70e32-2486">Cada `let` cláusula introduz uma variável de intervalo que representa um valor calculado por meio de variáveis de intervalo anterior.</span><span class="sxs-lookup"><span data-stu-id="70e32-2486">Each `let` clause introduces a range variable representing a value computed by means of previous range variables.</span></span> <span data-ttu-id="70e32-2487">Cada `where` cláusula é um filtro que exclui itens de resultado.</span><span class="sxs-lookup"><span data-stu-id="70e32-2487">Each `where` clause is a filter that excludes items from the result.</span></span> <span data-ttu-id="70e32-2488">Cada `join` cláusula compara as chaves especificadas da sequência de origem com chaves de outra sequência, resultando em pares correspondentes.</span><span class="sxs-lookup"><span data-stu-id="70e32-2488">Each `join` clause compares specified keys of the source sequence with keys of another sequence, yielding matching pairs.</span></span> <span data-ttu-id="70e32-2489">Cada `orderby` cláusula reordena os itens de acordo com os critérios especificados. O último `select` ou `group` cláusula Especifica a forma do resultado em termos de variáveis de intervalo.</span><span class="sxs-lookup"><span data-stu-id="70e32-2489">Each `orderby` clause reorders items according to specified criteria.The final `select` or `group` clause specifies the shape of the result in terms of the range variables.</span></span> <span data-ttu-id="70e32-2490">Por fim, um `into` cláusula pode ser usada para "unir" consultas, tratando os resultados de uma consulta como um gerador em uma consulta subsequente.</span><span class="sxs-lookup"><span data-stu-id="70e32-2490">Finally, an `into` clause can be used to "splice" queries by treating the results of one query as a generator in a subsequent query.</span></span>

### <a name="ambiguities-in-query-expressions"></a><span data-ttu-id="70e32-2491">Ambiguidades em expressões de consulta</span><span class="sxs-lookup"><span data-stu-id="70e32-2491">Ambiguities in query expressions</span></span>

<span data-ttu-id="70e32-2492">Expressões de consulta contêm um número de "palavras-chave contextuais", ou seja, os identificadores que têm significado especial em um determinado contexto.</span><span class="sxs-lookup"><span data-stu-id="70e32-2492">Query expressions contain a number of "contextual keywords", i.e., identifiers that have special meaning in a given context.</span></span> <span data-ttu-id="70e32-2493">Especificamente, esses são `from`, `where`, `join`, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, `select`, `group` e `by`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2493">Specifically these are `from`, `where`, `join`, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, `select`, `group` and `by`.</span></span> <span data-ttu-id="70e32-2494">Para evitar ambiguidades em expressões de consulta causadas pelo uso misto desses identificadores como nomes simples ou palavras-chave, esses identificadores são considerados palavras-chave quando que ocorrem em qualquer lugar dentro de uma expressão de consulta.</span><span class="sxs-lookup"><span data-stu-id="70e32-2494">In order to avoid ambiguities in query expressions caused by mixed use of these identifiers as keywords or simple names, these identifiers are considered keywords when occurring anywhere within a query expression.</span></span>

<span data-ttu-id="70e32-2495">Para essa finalidade, uma expressão de consulta é qualquer expressão que começa com "`from identifier`"seguido por qualquer token exceto"`;`","`=`"ou"`,`".</span><span class="sxs-lookup"><span data-stu-id="70e32-2495">For this purpose, a query expression is any expression that starts with "`from identifier`" followed by any token except "`;`", "`=`" or "`,`".</span></span>

<span data-ttu-id="70e32-2496">Para usar essas palavras como identificadores em uma expressão de consulta, elas podem ser prefixadas com "`@`" ([identificadores](lexical-structure.md#identifiers)).</span><span class="sxs-lookup"><span data-stu-id="70e32-2496">In order to use these words as identifiers within a query expression, they can be prefixed with "`@`" ([Identifiers](lexical-structure.md#identifiers)).</span></span>

### <a name="query-expression-translation"></a><span data-ttu-id="70e32-2497">Conversão de expressão de consulta</span><span class="sxs-lookup"><span data-stu-id="70e32-2497">Query expression translation</span></span>

<span data-ttu-id="70e32-2498">A linguagem c# não especifica a semântica de execução de expressões de consulta.</span><span class="sxs-lookup"><span data-stu-id="70e32-2498">The C# language does not specify the execution semantics of query expressions.</span></span> <span data-ttu-id="70e32-2499">Em vez disso, as expressões de consulta são traduzidas para invocações de métodos que seguem a *padrão de expressão de consulta* ([o padrão de expressão de consulta](expressions.md#the-query-expression-pattern)).</span><span class="sxs-lookup"><span data-stu-id="70e32-2499">Rather, query expressions are translated into invocations of methods that adhere to the *query expression pattern* ([The query expression pattern](expressions.md#the-query-expression-pattern)).</span></span> <span data-ttu-id="70e32-2500">Especificamente, as expressões de consulta são convertidas em chamadas de métodos chamados `Where`, `Select`, `SelectMany`, `Join`, `GroupJoin`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `GroupBy`, e `Cast`. Esses métodos devem ter assinaturas específicas e tipos de resultado, conforme descrito em [o padrão de expressão de consulta](expressions.md#the-query-expression-pattern).</span><span class="sxs-lookup"><span data-stu-id="70e32-2500">Specifically, query expressions are translated into invocations of methods named `Where`, `Select`, `SelectMany`, `Join`, `GroupJoin`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `GroupBy`, and `Cast`.These methods are expected to have particular signatures and result types, as described in [The query expression pattern](expressions.md#the-query-expression-pattern).</span></span> <span data-ttu-id="70e32-2501">Esses métodos podem ser métodos de instância do objeto que está sendo consultada ou métodos de extensão que são externos ao objeto, e elas implementam a execução real da consulta.</span><span class="sxs-lookup"><span data-stu-id="70e32-2501">These methods can be instance methods of the object being queried or extension methods that are external to the object, and they implement the actual execution of the query.</span></span>

<span data-ttu-id="70e32-2502">A tradução de expressões de consulta para invocações de método é um mapeamento sintático que ocorre antes de qualquer tipo de associação ou resolução de sobrecarga foi executada.</span><span class="sxs-lookup"><span data-stu-id="70e32-2502">The translation from query expressions to method invocations is a syntactic mapping that occurs before any type binding or overload resolution has been performed.</span></span> <span data-ttu-id="70e32-2503">A tradução é garantida para ser sintaticamente correto, mas não é garantido para produzir o código c# semanticamente correto.</span><span class="sxs-lookup"><span data-stu-id="70e32-2503">The translation is guaranteed to be syntactically correct, but it is not guaranteed to produce semantically correct C# code.</span></span> <span data-ttu-id="70e32-2504">Após a conversão de expressões de consulta, invocações de método resultantes são processadas como invocações de método normal, e isso por sua vez pode revelar erros, por exemplo, se os métodos não existem, se os argumentos têm tipos errados ou se os métodos são genéricos e Falha de inferência de tipo.</span><span class="sxs-lookup"><span data-stu-id="70e32-2504">Following translation of query expressions, the resulting method invocations are processed as regular method invocations, and this may in turn uncover errors, for example if the methods do not exist, if arguments have wrong types, or if the methods are generic and type inference fails.</span></span>

<span data-ttu-id="70e32-2505">Uma expressão de consulta é processada aplicando as seguintes traduções repetidamente até que nenhum reduções ainda maiores são possíveis.</span><span class="sxs-lookup"><span data-stu-id="70e32-2505">A query expression is processed by repeatedly applying the following translations until no further reductions are possible.</span></span> <span data-ttu-id="70e32-2506">As traduções estão listadas na ordem do aplicativo: cada seção pressupõe que as traduções nas seções anteriores foram executadas exaustivamente e depois que esgotar, uma seção será não mais tarde revista no processamento da mesma expressão de consulta.</span><span class="sxs-lookup"><span data-stu-id="70e32-2506">The translations are listed in order of application: each section assumes that the translations in the preceding sections have been performed exhaustively, and once exhausted, a section will not later be revisited in the processing of the same query expression.</span></span>

<span data-ttu-id="70e32-2507">Atribuição a variáveis de intervalo não é permitida em expressões de consulta.</span><span class="sxs-lookup"><span data-stu-id="70e32-2507">Assignment to range variables is not allowed in query expressions.</span></span> <span data-ttu-id="70e32-2508">No entanto, uma implementação c# é permitida para nem sempre impõe esta restrição, uma vez que isso pode às vezes, não ser possível com o esquema de tradução sintática apresentado aqui.</span><span class="sxs-lookup"><span data-stu-id="70e32-2508">However a C# implementation is permitted to not always enforce this restriction, since this may sometimes not be possible with the syntactic translation scheme presented here.</span></span>

<span data-ttu-id="70e32-2509">Determinadas conversões inserir variáveis de intervalo com identificadores transparentes indicados por `*`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2509">Certain translations inject range variables with transparent identifiers denoted by `*`.</span></span> <span data-ttu-id="70e32-2510">As propriedades especiais de transparentes identificadores são discutidas mais detalhadamente em [transparentes identificadores](expressions.md#transparent-identifiers).</span><span class="sxs-lookup"><span data-stu-id="70e32-2510">The special properties of transparent identifiers are discussed further in [Transparent identifiers](expressions.md#transparent-identifiers).</span></span>

#### <a name="select-and-groupby-clauses-with-continuations"></a><span data-ttu-id="70e32-2511">Cláusulas SELECT e groupby com continuações</span><span class="sxs-lookup"><span data-stu-id="70e32-2511">Select and groupby clauses with continuations</span></span>

<span data-ttu-id="70e32-2512">Uma expressão de consulta com uma continuação</span><span class="sxs-lookup"><span data-stu-id="70e32-2512">A query expression with a continuation</span></span>
```csharp
from ... into x ...
```
<span data-ttu-id="70e32-2513">é convertido em</span><span class="sxs-lookup"><span data-stu-id="70e32-2513">is translated into</span></span>
```csharp
from x in ( from ... ) ...
```

<span data-ttu-id="70e32-2514">As traduções nas seções a seguir supõem que não têm consultas `into` continuações.</span><span class="sxs-lookup"><span data-stu-id="70e32-2514">The translations in the following sections assume that queries have no `into` continuations.</span></span>

<span data-ttu-id="70e32-2515">O exemplo</span><span class="sxs-lookup"><span data-stu-id="70e32-2515">The example</span></span>
```csharp
from c in customers
group c by c.Country into g
select new { Country = g.Key, CustCount = g.Count() }
```
<span data-ttu-id="70e32-2516">é convertido em</span><span class="sxs-lookup"><span data-stu-id="70e32-2516">is translated into</span></span>
```csharp
from g in
    from c in customers
    group c by c.Country
select new { Country = g.Key, CustCount = g.Count() }
```
<span data-ttu-id="70e32-2517">a tradução final que é</span><span class="sxs-lookup"><span data-stu-id="70e32-2517">the final translation of which is</span></span>
```csharp
customers.
GroupBy(c => c.Country).
Select(g => new { Country = g.Key, CustCount = g.Count() })
```

#### <a name="explicit-range-variable-types"></a><span data-ttu-id="70e32-2518">Tipos de variável de intervalo explícito</span><span class="sxs-lookup"><span data-stu-id="70e32-2518">Explicit range variable types</span></span>

<span data-ttu-id="70e32-2519">Um `from` cláusula que especifica explicitamente um tipo de variável de intervalo</span><span class="sxs-lookup"><span data-stu-id="70e32-2519">A `from` clause that explicitly specifies a range variable type</span></span>
```csharp
from T x in e
```
<span data-ttu-id="70e32-2520">é convertido em</span><span class="sxs-lookup"><span data-stu-id="70e32-2520">is translated into</span></span>
```csharp
from x in ( e ) . Cast < T > ( )
```

<span data-ttu-id="70e32-2521">Um `join` cláusula que especifica explicitamente um tipo de variável de intervalo</span><span class="sxs-lookup"><span data-stu-id="70e32-2521">A `join` clause that explicitly specifies a range variable type</span></span>
```
join T x in e on k1 equals k2
```
<span data-ttu-id="70e32-2522">é convertido em</span><span class="sxs-lookup"><span data-stu-id="70e32-2522">is translated into</span></span>
```
join x in ( e ) . Cast < T > ( ) on k1 equals k2
```

<span data-ttu-id="70e32-2523">As traduções nas seções a seguir pressupõem que o consultas não tem nenhum tipo de variável de intervalo explícito.</span><span class="sxs-lookup"><span data-stu-id="70e32-2523">The translations in the following sections assume that queries have no explicit range variable types.</span></span>

<span data-ttu-id="70e32-2524">O exemplo</span><span class="sxs-lookup"><span data-stu-id="70e32-2524">The example</span></span>
```csharp
from Customer c in customers
where c.City == "London"
select c
```
<span data-ttu-id="70e32-2525">é convertido em</span><span class="sxs-lookup"><span data-stu-id="70e32-2525">is translated into</span></span>
```csharp
from c in customers.Cast<Customer>()
where c.City == "London"
select c
```
<span data-ttu-id="70e32-2526">a tradução final que é</span><span class="sxs-lookup"><span data-stu-id="70e32-2526">the final translation of which is</span></span>
```csharp
customers.
Cast<Customer>().
Where(c => c.City == "London")
```

<span data-ttu-id="70e32-2527">Tipos de variável de intervalo explícitas são úteis para consultar coleções que implementam não genéricas `IEnumerable` interface, mas não genérica `IEnumerable<T>` interface.</span><span class="sxs-lookup"><span data-stu-id="70e32-2527">Explicit range variable types are useful for querying collections that implement the non-generic `IEnumerable` interface, but not the generic `IEnumerable<T>` interface.</span></span> <span data-ttu-id="70e32-2528">No exemplo acima, isso seria o caso se `customers` eram do tipo `ArrayList`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2528">In the example above, this would be the case if `customers` were of type `ArrayList`.</span></span>

#### <a name="degenerate-query-expressions"></a><span data-ttu-id="70e32-2529">Expressões de consulta de degeneração</span><span class="sxs-lookup"><span data-stu-id="70e32-2529">Degenerate query expressions</span></span>

<span data-ttu-id="70e32-2530">Uma expressão de consulta do formulário</span><span class="sxs-lookup"><span data-stu-id="70e32-2530">A query expression of the form</span></span>
```csharp
from x in e select x
```
<span data-ttu-id="70e32-2531">é convertido em</span><span class="sxs-lookup"><span data-stu-id="70e32-2531">is translated into</span></span>
```csharp
( e ) . Select ( x => x )
```

<span data-ttu-id="70e32-2532">O exemplo</span><span class="sxs-lookup"><span data-stu-id="70e32-2532">The example</span></span>
```csharp
from c in customers
select c
```
<span data-ttu-id="70e32-2533">é convertido em</span><span class="sxs-lookup"><span data-stu-id="70e32-2533">is translated into</span></span>
```csharp
customers.Select(c => c)
```

<span data-ttu-id="70e32-2534">Uma expressão de consulta degenerado é aquele que trivialmente seleciona os elementos da fonte.</span><span class="sxs-lookup"><span data-stu-id="70e32-2534">A degenerate query expression is one that trivially selects the elements of the source.</span></span> <span data-ttu-id="70e32-2535">Uma fase posterior da tradução remove degeneradas consultas introduzidas por outras etapas de tradução, substituindo-os à sua origem.</span><span class="sxs-lookup"><span data-stu-id="70e32-2535">A later phase of the translation removes degenerate queries introduced by other translation steps by replacing them with their source.</span></span> <span data-ttu-id="70e32-2536">É importante no entanto, para garantir que o resultado de uma consulta de expressão nunca é o objeto de origem em si, como o que poderia revelar o tipo e a identidade da origem para o cliente da consulta.</span><span class="sxs-lookup"><span data-stu-id="70e32-2536">It is important however to ensure that the result of a query expression is never the source object itself, as that would reveal the type and identity of the source to the client of the query.</span></span> <span data-ttu-id="70e32-2537">Nesta etapa, portanto, protege os degenerado consultas gravadas diretamente no código-fonte chamando explicitamente `Select` na origem.</span><span class="sxs-lookup"><span data-stu-id="70e32-2537">Therefore this step protects degenerate queries written directly in source code by explicitly calling `Select` on the source.</span></span> <span data-ttu-id="70e32-2538">Ele cabe, em seguida, os implementadores de `Select` e outros operadores de consulta para garantir que esses métodos nunca retornam o objeto de origem.</span><span class="sxs-lookup"><span data-stu-id="70e32-2538">It is then up to the implementers of `Select` and other query operators to ensure that these methods never return the source object itself.</span></span>

#### <a name="from-let-where-join-and-orderby-clauses"></a><span data-ttu-id="70e32-2539">Do, where, orderby e junção cláusulas let</span><span class="sxs-lookup"><span data-stu-id="70e32-2539">From, let, where, join and orderby clauses</span></span>

<span data-ttu-id="70e32-2540">Uma expressão de consulta com uma segunda `from` cláusula seguido por um `select` cláusula</span><span class="sxs-lookup"><span data-stu-id="70e32-2540">A query expression with a second `from` clause followed by a `select` clause</span></span>
```csharp
from x1 in e1
from x2 in e2
select v
```
<span data-ttu-id="70e32-2541">é convertido em</span><span class="sxs-lookup"><span data-stu-id="70e32-2541">is translated into</span></span>
```csharp
( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => v )
```

<span data-ttu-id="70e32-2542">Uma expressão de consulta com uma segunda `from` cláusula seguido por algo diferente de um `select` cláusula:</span><span class="sxs-lookup"><span data-stu-id="70e32-2542">A query expression with a second `from` clause followed by something other than a `select` clause:</span></span>

```csharp
from x1 in e1
from x2 in e2
...
```
<span data-ttu-id="70e32-2543">é convertido em</span><span class="sxs-lookup"><span data-stu-id="70e32-2543">is translated into</span></span>
```csharp
from * in ( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => new { x1 , x2 } )
...
```

<span data-ttu-id="70e32-2544">Uma expressão de consulta com um `let` cláusula</span><span class="sxs-lookup"><span data-stu-id="70e32-2544">A query expression with a `let` clause</span></span>
```csharp
from x in e
let y = f
...
```
<span data-ttu-id="70e32-2545">é convertido em</span><span class="sxs-lookup"><span data-stu-id="70e32-2545">is translated into</span></span>
```csharp
from * in ( e ) . Select ( x => new { x , y = f } )
...
```

<span data-ttu-id="70e32-2546">Uma expressão de consulta com um `where` cláusula</span><span class="sxs-lookup"><span data-stu-id="70e32-2546">A query expression with a `where` clause</span></span>
```csharp
from x in e
where f
...
```
<span data-ttu-id="70e32-2547">é convertido em</span><span class="sxs-lookup"><span data-stu-id="70e32-2547">is translated into</span></span>
```csharp
from x in ( e ) . Where ( x => f )
...
```

<span data-ttu-id="70e32-2548">Uma expressão de consulta com um `join` cláusula sem uma `into` seguido por um `select` cláusula</span><span class="sxs-lookup"><span data-stu-id="70e32-2548">A query expression with a `join` clause without an `into` followed by a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
select v
```
<span data-ttu-id="70e32-2549">é convertido em</span><span class="sxs-lookup"><span data-stu-id="70e32-2549">is translated into</span></span>
```csharp
( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => v )
```

<span data-ttu-id="70e32-2550">Uma expressão de consulta com um `join` cláusula sem uma `into` seguido por algo diferente de um `select` cláusula</span><span class="sxs-lookup"><span data-stu-id="70e32-2550">A query expression with a `join` clause without an `into` followed by something other than a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
...
```
<span data-ttu-id="70e32-2551">é convertido em</span><span class="sxs-lookup"><span data-stu-id="70e32-2551">is translated into</span></span>
```csharp
from * in ( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => new { x1 , x2 })
...
```

<span data-ttu-id="70e32-2552">Uma expressão de consulta com um `join` cláusula com um `into` seguido por um `select` cláusula</span><span class="sxs-lookup"><span data-stu-id="70e32-2552">A query expression with a `join` clause with an `into` followed by a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
select v
```
<span data-ttu-id="70e32-2553">é convertido em</span><span class="sxs-lookup"><span data-stu-id="70e32-2553">is translated into</span></span>
```csharp
( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => v )
```

<span data-ttu-id="70e32-2554">Uma expressão de consulta com um `join` cláusula com um `into` seguido por algo diferente de um `select` cláusula</span><span class="sxs-lookup"><span data-stu-id="70e32-2554">A query expression with a `join` clause with an `into` followed by something other than a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
...
```
<span data-ttu-id="70e32-2555">é convertido em</span><span class="sxs-lookup"><span data-stu-id="70e32-2555">is translated into</span></span>
```csharp
from * in ( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => new { x1 , g })
...
```

<span data-ttu-id="70e32-2556">Uma expressão de consulta com um `orderby` cláusula</span><span class="sxs-lookup"><span data-stu-id="70e32-2556">A query expression with an `orderby` clause</span></span>
```csharp
from x in e
orderby k1 , k2 , ..., kn
...
```
<span data-ttu-id="70e32-2557">é convertido em</span><span class="sxs-lookup"><span data-stu-id="70e32-2557">is translated into</span></span>
```csharp
from x in ( e ) . 
OrderBy ( x => k1 ) . 
ThenBy ( x => k2 ) .
... .
ThenBy ( x => kn )
...
```

<span data-ttu-id="70e32-2558">Se uma ordenação cláusula Especifica um `descending` indicador de direção, uma invocação de `OrderByDescending` ou `ThenByDescending` é produzido em vez disso.</span><span class="sxs-lookup"><span data-stu-id="70e32-2558">If an ordering clause specifies a `descending` direction indicator, an invocation of `OrderByDescending` or `ThenByDescending` is produced instead.</span></span>

<span data-ttu-id="70e32-2559">As traduções a seguir supõem que não há nenhum `let`, `where`, `join` ou `orderby` cláusulas e não mais do que aquele inicial `from` cláusula em cada expressão de consulta.</span><span class="sxs-lookup"><span data-stu-id="70e32-2559">The following translations assume that there are no `let`, `where`, `join` or `orderby` clauses, and no more than the one initial `from` clause in each query expression.</span></span>

<span data-ttu-id="70e32-2560">O exemplo</span><span class="sxs-lookup"><span data-stu-id="70e32-2560">The example</span></span>
```csharp
from c in customers
from o in c.Orders
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="70e32-2561">é convertido em</span><span class="sxs-lookup"><span data-stu-id="70e32-2561">is translated into</span></span>
```csharp
customers.
SelectMany(c => c.Orders,
     (c,o) => new { c.Name, o.OrderID, o.Total }
)
```

<span data-ttu-id="70e32-2562">O exemplo</span><span class="sxs-lookup"><span data-stu-id="70e32-2562">The example</span></span>
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="70e32-2563">é convertido em</span><span class="sxs-lookup"><span data-stu-id="70e32-2563">is translated into</span></span>
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="70e32-2564">a tradução final que é</span><span class="sxs-lookup"><span data-stu-id="70e32-2564">the final translation of which is</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.OrderID, x.o.Total })
```
<span data-ttu-id="70e32-2565">onde `x` é um identificador gerado pelo compilador que de outra forma inacessíveis e invisível.</span><span class="sxs-lookup"><span data-stu-id="70e32-2565">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="70e32-2566">O exemplo</span><span class="sxs-lookup"><span data-stu-id="70e32-2566">The example</span></span>
```csharp
from o in orders
let t = o.Details.Sum(d => d.UnitPrice * d.Quantity)
where t >= 1000
select new { o.OrderID, Total = t }
```
<span data-ttu-id="70e32-2567">é convertido em</span><span class="sxs-lookup"><span data-stu-id="70e32-2567">is translated into</span></span>
```csharp
from * in orders.
    Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) })
where t >= 1000 
select new { o.OrderID, Total = t }
```
<span data-ttu-id="70e32-2568">a tradução final que é</span><span class="sxs-lookup"><span data-stu-id="70e32-2568">the final translation of which is</span></span>
```csharp
orders.
Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) }).
Where(x => x.t >= 1000).
Select(x => new { x.o.OrderID, Total = x.t })
```
<span data-ttu-id="70e32-2569">onde `x` é um identificador gerado pelo compilador que de outra forma inacessíveis e invisível.</span><span class="sxs-lookup"><span data-stu-id="70e32-2569">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="70e32-2570">O exemplo</span><span class="sxs-lookup"><span data-stu-id="70e32-2570">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
select new { c.Name, o.OrderDate, o.Total }
```
<span data-ttu-id="70e32-2571">é convertido em</span><span class="sxs-lookup"><span data-stu-id="70e32-2571">is translated into</span></span>
```csharp
customers.Join(orders, c => c.CustomerID, o => o.CustomerID,
    (c, o) => new { c.Name, o.OrderDate, o.Total })
```

<span data-ttu-id="70e32-2572">O exemplo</span><span class="sxs-lookup"><span data-stu-id="70e32-2572">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID into co
let n = co.Count()
where n >= 10
select new { c.Name, OrderCount = n }
```
<span data-ttu-id="70e32-2573">é convertido em</span><span class="sxs-lookup"><span data-stu-id="70e32-2573">is translated into</span></span>
```csharp
from * in customers.
    GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
        (c, co) => new { c, co })
let n = co.Count()
where n >= 10 
select new { c.Name, OrderCount = n }
```
<span data-ttu-id="70e32-2574">a tradução final que é</span><span class="sxs-lookup"><span data-stu-id="70e32-2574">the final translation of which is</span></span>
```csharp
customers.
GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
    (c, co) => new { c, co }).
Select(x => new { x, n = x.co.Count() }).
Where(y => y.n >= 10).
Select(y => new { y.x.c.Name, OrderCount = y.n)
```
<span data-ttu-id="70e32-2575">em que `x` e `y` são identificadores gerado pelo compilador que estariam invisíveis e inacessíveis.</span><span class="sxs-lookup"><span data-stu-id="70e32-2575">where `x` and `y` are compiler generated identifiers that are otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="70e32-2576">O exemplo</span><span class="sxs-lookup"><span data-stu-id="70e32-2576">The example</span></span>
```csharp
from o in orders
orderby o.Customer.Name, o.Total descending
select o
```
<span data-ttu-id="70e32-2577">tem a tradução final</span><span class="sxs-lookup"><span data-stu-id="70e32-2577">has the final translation</span></span>
```csharp
orders.
OrderBy(o => o.Customer.Name).
ThenByDescending(o => o.Total)
```

#### <a name="select-clauses"></a><span data-ttu-id="70e32-2578">Cláusulas SELECT</span><span class="sxs-lookup"><span data-stu-id="70e32-2578">Select clauses</span></span>

<span data-ttu-id="70e32-2579">Uma expressão de consulta do formulário</span><span class="sxs-lookup"><span data-stu-id="70e32-2579">A query expression of the form</span></span>
```csharp
from x in e select v
```
<span data-ttu-id="70e32-2580">é convertido em</span><span class="sxs-lookup"><span data-stu-id="70e32-2580">is translated into</span></span>
```csharp
( e ) . Select ( x => v )
```
<span data-ttu-id="70e32-2581">exceto quando v é o identificador de x, a tradução é simplesmente</span><span class="sxs-lookup"><span data-stu-id="70e32-2581">except when v is the identifier x, the translation is simply</span></span>
```csharp
( e )
```

<span data-ttu-id="70e32-2582">Por exemplo</span><span class="sxs-lookup"><span data-stu-id="70e32-2582">For example</span></span>
```csharp
from c in customers.Where(c => c.City == "London")
select c
```
<span data-ttu-id="70e32-2583">simplesmente é traduzido em</span><span class="sxs-lookup"><span data-stu-id="70e32-2583">is simply translated into</span></span>
```csharp
customers.Where(c => c.City == "London")
```

#### <a name="groupby-clauses"></a><span data-ttu-id="70e32-2584">Cláusulas de GroupBy</span><span class="sxs-lookup"><span data-stu-id="70e32-2584">Groupby clauses</span></span>

<span data-ttu-id="70e32-2585">Uma expressão de consulta do formulário</span><span class="sxs-lookup"><span data-stu-id="70e32-2585">A query expression of the form</span></span>
```csharp
from x in e group v by k
```
<span data-ttu-id="70e32-2586">é convertido em</span><span class="sxs-lookup"><span data-stu-id="70e32-2586">is translated into</span></span>
```csharp
( e ) . GroupBy ( x => k , x => v )
```
<span data-ttu-id="70e32-2587">exceto quando v é o identificador de x, a tradução é</span><span class="sxs-lookup"><span data-stu-id="70e32-2587">except when v is the identifier x, the translation is</span></span>
```csharp
( e ) . GroupBy ( x => k )
```

<span data-ttu-id="70e32-2588">O exemplo</span><span class="sxs-lookup"><span data-stu-id="70e32-2588">The example</span></span>
```csharp
from c in customers
group c.Name by c.Country
```
<span data-ttu-id="70e32-2589">é convertido em</span><span class="sxs-lookup"><span data-stu-id="70e32-2589">is translated into</span></span>
```csharp
customers.
GroupBy(c => c.Country, c => c.Name)
```

#### <a name="transparent-identifiers"></a><span data-ttu-id="70e32-2590">Identificadores transparentes</span><span class="sxs-lookup"><span data-stu-id="70e32-2590">Transparent identifiers</span></span>

<span data-ttu-id="70e32-2591">Determinadas conversões de injetar as variáveis de intervalo com ***identificadores transparentes*** indicada por `*`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2591">Certain translations inject range variables with ***transparent identifiers*** denoted by `*`.</span></span> <span data-ttu-id="70e32-2592">Identificadores transparentes não são um recurso de idioma apropriados; elas existem somente como uma etapa intermediária no processo de conversão de expressão de consulta.</span><span class="sxs-lookup"><span data-stu-id="70e32-2592">Transparent identifiers are not a proper language feature; they exist only as an intermediate step in the query expression translation process.</span></span>

<span data-ttu-id="70e32-2593">Quando uma conversão de consulta injeta um identificador transparente, ainda mais etapas de conversão propagam o identificador transparente para funções anônimas e inicializadores de objeto anônimos.</span><span class="sxs-lookup"><span data-stu-id="70e32-2593">When a query translation injects a transparent identifier, further translation steps propagate the transparent identifier into anonymous functions and anonymous object initializers.</span></span> <span data-ttu-id="70e32-2594">Nesses contextos, transparentes identificadores têm o seguinte comportamento:</span><span class="sxs-lookup"><span data-stu-id="70e32-2594">In those contexts, transparent identifiers have the following behavior:</span></span>

*  <span data-ttu-id="70e32-2595">Quando ocorre um identificador transparente como um parâmetro em uma função anônima, os membros do tipo anônimo associado são automaticamente no escopo no corpo da função anônima.</span><span class="sxs-lookup"><span data-stu-id="70e32-2595">When a transparent identifier occurs as a parameter in an anonymous function, the members of the associated anonymous type are automatically in scope in the body of the anonymous function.</span></span>
*  <span data-ttu-id="70e32-2596">Quando um membro com um identificador transparente está no escopo, os membros desse membro estão no escopo também.</span><span class="sxs-lookup"><span data-stu-id="70e32-2596">When a member with a transparent identifier is in scope, the members of that member are in scope as well.</span></span>
*  <span data-ttu-id="70e32-2597">Quando ocorre um identificador transparente como um Declarador de membro de um inicializador de objeto anônimo, ele introduz um membro com um identificador transparente.</span><span class="sxs-lookup"><span data-stu-id="70e32-2597">When a transparent identifier occurs as a member declarator in an anonymous object initializer, it introduces a member with a transparent identifier.</span></span>
*  <span data-ttu-id="70e32-2598">As etapas de conversão descritas acima, sempre são introduzidos identificadores transparentes junto com os tipos anônimos, com a intenção de capturar várias variáveis de intervalo como membros de um único objeto.</span><span class="sxs-lookup"><span data-stu-id="70e32-2598">In the translation steps described above, transparent identifiers are always introduced together with anonymous types, with the intent of capturing multiple range variables as members of a single object.</span></span> <span data-ttu-id="70e32-2599">Uma implementação da linguagem c# tem permissão para usar um mecanismo diferente de tipos anônimos para agrupar diversas variáveis de intervalo.</span><span class="sxs-lookup"><span data-stu-id="70e32-2599">An implementation of C# is permitted to use a different mechanism than anonymous types to group together multiple range variables.</span></span> <span data-ttu-id="70e32-2600">Os exemplos de conversão a seguir pressupõem que tipos anônimos são usados e mostram como transparentes identificadores podem ser convertidos imediatamente.</span><span class="sxs-lookup"><span data-stu-id="70e32-2600">The following translation examples assume that anonymous types are used, and show how transparent identifiers can be translated away.</span></span>

<span data-ttu-id="70e32-2601">O exemplo</span><span class="sxs-lookup"><span data-stu-id="70e32-2601">The example</span></span>
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.Total }
```
<span data-ttu-id="70e32-2602">é convertido em</span><span class="sxs-lookup"><span data-stu-id="70e32-2602">is translated into</span></span>
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.Total }
```

<span data-ttu-id="70e32-2603">que é ainda mais convertidas em</span><span class="sxs-lookup"><span data-stu-id="70e32-2603">which is further translated into</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(* => o.Total).
Select(* => new { c.Name, o.Total })
```
<span data-ttu-id="70e32-2604">que, quando são apagados identificadores transparentes, é equivalente a</span><span class="sxs-lookup"><span data-stu-id="70e32-2604">which, when transparent identifiers are erased, is equivalent to</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.Total })
```
<span data-ttu-id="70e32-2605">onde `x` é um identificador gerado pelo compilador que de outra forma inacessíveis e invisível.</span><span class="sxs-lookup"><span data-stu-id="70e32-2605">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="70e32-2606">O exemplo</span><span class="sxs-lookup"><span data-stu-id="70e32-2606">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
<span data-ttu-id="70e32-2607">é convertido em</span><span class="sxs-lookup"><span data-stu-id="70e32-2607">is translated into</span></span>
```csharp
from * in customers.
    Join(orders, c => c.CustomerID, o => o.CustomerID, 
        (c, o) => new { c, o })
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
<span data-ttu-id="70e32-2608">que é ainda mais reduzida para</span><span class="sxs-lookup"><span data-stu-id="70e32-2608">which is further reduced to</span></span>
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID, (c, o) => new { c, o }).
Join(details, * => o.OrderID, d => d.OrderID, (*, d) => new { *, d }).
Join(products, * => d.ProductID, p => p.ProductID, (*, p) => new { *, p }).
Select(* => new { c.Name, o.OrderDate, p.ProductName })
```
<span data-ttu-id="70e32-2609">a tradução final que é</span><span class="sxs-lookup"><span data-stu-id="70e32-2609">the final translation of which is</span></span>
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
<span data-ttu-id="70e32-2610">em que `x`, `y`, e `z` são identificadores gerado pelo compilador que estariam invisíveis e inacessíveis.</span><span class="sxs-lookup"><span data-stu-id="70e32-2610">where `x`, `y`, and `z` are compiler generated identifiers that are otherwise invisible and inaccessible.</span></span>

### <a name="the-query-expression-pattern"></a><span data-ttu-id="70e32-2611">O padrão de expressão de consulta</span><span class="sxs-lookup"><span data-stu-id="70e32-2611">The query expression pattern</span></span>

<span data-ttu-id="70e32-2612">O ***padrão de expressão de consulta*** estabelece um padrão de métodos de tipos podem implementar para dar suporte a expressões de consulta.</span><span class="sxs-lookup"><span data-stu-id="70e32-2612">The ***Query expression pattern*** establishes a pattern of methods that types can implement to support query expressions.</span></span> <span data-ttu-id="70e32-2613">Como as expressões de consulta são traduzidas para invocações de método por meio de um mapeamento sintático, tipos têm flexibilidade considerável no modo como implementam o padrão de expressão de consulta.</span><span class="sxs-lookup"><span data-stu-id="70e32-2613">Because query expressions are translated to method invocations by means of a syntactic mapping, types have considerable flexibility in how they implement the query expression pattern.</span></span> <span data-ttu-id="70e32-2614">Por exemplo, os métodos do padrão podem ser implementados como métodos de instância ou como métodos de extensão porque os dois têm a mesma sintaxe de invocação e os métodos podem solicitar delegados ou árvores de expressão porque funções anônimas podem ser convertidas para ambos.</span><span class="sxs-lookup"><span data-stu-id="70e32-2614">For example, the methods of the pattern can be implemented as instance methods or as extension methods because the two have the same invocation syntax, and the methods can request delegates or expression trees because anonymous functions are convertible to both.</span></span>

<span data-ttu-id="70e32-2615">A forma recomendada de um tipo genérico `C<T>` que dá suporte ao padrão da expressão de consulta é mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="70e32-2615">The recommended shape of a generic type `C<T>` that supports the query expression pattern is shown below.</span></span> <span data-ttu-id="70e32-2616">Um tipo genérico é usado para ilustrar as relações corretas entre tipos de parâmetro e resultado, mas é possível implementar o padrão para tipos não genéricos também.</span><span class="sxs-lookup"><span data-stu-id="70e32-2616">A generic type is used in order to illustrate the proper relationships between parameter and result types, but it is possible to implement the pattern for non-generic types as well.</span></span>

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

<span data-ttu-id="70e32-2617">Os métodos acima usam os tipos de delegado genérico `Func<T1,R>` e `Func<T1,T2,R>`, mas pode igualmente bem ter usados outros tipos de árvore de delegado ou expressão com as mesmas relações nos tipos de parâmetro e resultado.</span><span class="sxs-lookup"><span data-stu-id="70e32-2617">The methods above use the generic delegate types `Func<T1,R>` and `Func<T1,T2,R>`, but they could equally well have used other delegate or expression tree types with the same relationships in parameter and result types.</span></span>

<span data-ttu-id="70e32-2618">Observe a relação recomendada entre `C<T>` e `O<T>` que garante que o `ThenBy` e `ThenByDescending` métodos estão disponíveis somente no resultado de um `OrderBy` ou `OrderByDescending`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2618">Notice the recommended relationship between `C<T>` and `O<T>` which ensures that the `ThenBy` and `ThenByDescending` methods are available only on the result of an `OrderBy` or `OrderByDescending`.</span></span> <span data-ttu-id="70e32-2619">Observe também a forma recomendada de resultado de `GroupBy` – uma sequência de sequências, onde cada sequência interna tem mais `Key` propriedade.</span><span class="sxs-lookup"><span data-stu-id="70e32-2619">Also notice the recommended shape of the result of `GroupBy` -- a sequence of sequences, where each inner sequence has an additional `Key` property.</span></span>

<span data-ttu-id="70e32-2620">O `System.Linq` namespace fornece uma implementação do padrão de operador de consulta para qualquer tipo que implementa o `System.Collections.Generic.IEnumerable<T>` interface.</span><span class="sxs-lookup"><span data-stu-id="70e32-2620">The `System.Linq` namespace provides an implementation of the query operator pattern for any type that implements the `System.Collections.Generic.IEnumerable<T>` interface.</span></span>

## <a name="assignment-operators"></a><span data-ttu-id="70e32-2621">Operadores de atribuição</span><span class="sxs-lookup"><span data-stu-id="70e32-2621">Assignment operators</span></span>

<span data-ttu-id="70e32-2622">Os operadores de atribuição atribui um novo valor para uma variável, uma propriedade, um evento ou um elemento do indexador.</span><span class="sxs-lookup"><span data-stu-id="70e32-2622">The assignment operators assign a new value to a variable, a property, an event, or an indexer element.</span></span>

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

<span data-ttu-id="70e32-2623">O operando esquerdo de uma atribuição deve ser uma expressão classificada como uma variável, um acesso de propriedade, um acesso de indexador ou um acesso de evento.</span><span class="sxs-lookup"><span data-stu-id="70e32-2623">The left operand of an assignment must be an expression classified as a variable, a property access, an indexer access, or an event access.</span></span>

<span data-ttu-id="70e32-2624">O `=` é chamado de operador de ***operador de atribuição simples***.</span><span class="sxs-lookup"><span data-stu-id="70e32-2624">The `=` operator is called the ***simple assignment operator***.</span></span> <span data-ttu-id="70e32-2625">Ele atribui o valor do operando à direita para o elemento de variável, propriedade ou indexador fornecido pelo operando à esquerda.</span><span class="sxs-lookup"><span data-stu-id="70e32-2625">It assigns the value of the right operand to the variable, property, or indexer element given by the left operand.</span></span> <span data-ttu-id="70e32-2626">O operando esquerdo do operador de atribuição simples pode não ser um acesso de evento (exceto conforme descrito em [eventos semelhantes a campo](classes.md#field-like-events)).</span><span class="sxs-lookup"><span data-stu-id="70e32-2626">The left operand of the simple assignment operator may not be an event access (except as described in [Field-like events](classes.md#field-like-events)).</span></span> <span data-ttu-id="70e32-2627">O operador de atribuição simples é descrito em [atribuição simples](expressions.md#simple-assignment).</span><span class="sxs-lookup"><span data-stu-id="70e32-2627">The simple assignment operator is described in [Simple assignment](expressions.md#simple-assignment).</span></span>

<span data-ttu-id="70e32-2628">Os operadores de atribuição que o `=` operador são chamados de ***operadores de atribuição composta***.</span><span class="sxs-lookup"><span data-stu-id="70e32-2628">The assignment operators other than the `=` operator are called the ***compound assignment operators***.</span></span> <span data-ttu-id="70e32-2629">Esses operadores de executam a operação indicada em dois operandos e, em seguida, atribua o valor resultante para o elemento de variável, propriedade ou indexador fornecido pelo operando à esquerda.</span><span class="sxs-lookup"><span data-stu-id="70e32-2629">These operators perform the indicated operation on the two operands, and then assign the resulting value to the variable, property, or indexer element given by the left operand.</span></span> <span data-ttu-id="70e32-2630">Os operadores de atribuição composta são descritos em [atribuição composta](expressions.md#compound-assignment).</span><span class="sxs-lookup"><span data-stu-id="70e32-2630">The compound assignment operators are described in [Compound assignment](expressions.md#compound-assignment).</span></span>

<span data-ttu-id="70e32-2631">O `+=` e `-=` são chamados de operadores com uma expressão de acesso de evento como o operando esquerdo a *operadores de atribuição de evento*.</span><span class="sxs-lookup"><span data-stu-id="70e32-2631">The `+=` and `-=` operators with an event access expression as the left operand are called the *event assignment operators*.</span></span> <span data-ttu-id="70e32-2632">Nenhum outro operador de atribuição é válido com acesso de um evento que o operando esquerdo.</span><span class="sxs-lookup"><span data-stu-id="70e32-2632">No other assignment operator is valid with an event access as the left operand.</span></span> <span data-ttu-id="70e32-2633">Os operadores de atribuição de evento são descritos em [atribuição de evento](expressions.md#event-assignment).</span><span class="sxs-lookup"><span data-stu-id="70e32-2633">The event assignment operators are described in [Event assignment](expressions.md#event-assignment).</span></span>

<span data-ttu-id="70e32-2634">Os operadores de atribuição são associativos à direita, o que significa que as operações são agrupadas da direita para esquerda.</span><span class="sxs-lookup"><span data-stu-id="70e32-2634">The assignment operators are right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="70e32-2635">Por exemplo, uma expressão do formulário `a = b = c` é avaliada como `a = (b = c)`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2635">For example, an expression of the form `a = b = c` is evaluated as `a = (b = c)`.</span></span>

### <a name="simple-assignment"></a><span data-ttu-id="70e32-2636">Atribuição simples</span><span class="sxs-lookup"><span data-stu-id="70e32-2636">Simple assignment</span></span>

<span data-ttu-id="70e32-2637">O `=` operador é chamado de operador de atribuição simples.</span><span class="sxs-lookup"><span data-stu-id="70e32-2637">The `=` operator is called the simple assignment operator.</span></span>

<span data-ttu-id="70e32-2638">Se o operando esquerdo de uma atribuição simples tem o formato `E.P` ou `E[Ei]` onde `E` tem o tipo de tempo de compilação `dynamic`, em seguida, a atribuição é associada dinamicamente ([vinculação dinâmica](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="70e32-2638">If the left operand of a simple assignment is of the form `E.P` or `E[Ei]` where `E` has the compile-time type `dynamic`, then the assignment is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="70e32-2639">Nesse caso é o tipo de tempo de compilação da expressão de atribuição `dynamic`, e a resolução descrita a seguir ocorrerá em tempo de execução com base no tipo de tempo de execução do `E`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2639">In this case the compile-time type of the assignment expression is `dynamic`, and the resolution described below will take place at run-time based on the run-time type of `E`.</span></span>

<span data-ttu-id="70e32-2640">Em uma atribuição simples, o operando direito deve ser uma expressão que é implicitamente conversível para o tipo do operando esquerdo.</span><span class="sxs-lookup"><span data-stu-id="70e32-2640">In a simple assignment, the right operand must be an expression that is implicitly convertible to the type of the left operand.</span></span> <span data-ttu-id="70e32-2641">A operação atribui o valor do operando à direita para o elemento de variável, propriedade ou indexador fornecido pelo operando à esquerda.</span><span class="sxs-lookup"><span data-stu-id="70e32-2641">The operation assigns the value of the right operand to the variable, property, or indexer element given by the left operand.</span></span>

<span data-ttu-id="70e32-2642">O resultado de uma expressão de atribuição simples é o valor atribuído ao operando esquerdo.</span><span class="sxs-lookup"><span data-stu-id="70e32-2642">The result of a simple assignment expression is the value assigned to the left operand.</span></span> <span data-ttu-id="70e32-2643">O resultado tem o mesmo tipo que o operando esquerdo e sempre é classificado como um valor.</span><span class="sxs-lookup"><span data-stu-id="70e32-2643">The result has the same type as the left operand and is always classified as a value.</span></span>

<span data-ttu-id="70e32-2644">Se o operando esquerdo é um acesso de propriedade ou indexador, propriedade ou indexador deve ter um `set` acessador.</span><span class="sxs-lookup"><span data-stu-id="70e32-2644">If the left operand is a property or indexer access, the property or indexer must have a `set` accessor.</span></span> <span data-ttu-id="70e32-2645">Se isso não for o caso, ocorre um erro em tempo de vinculação.</span><span class="sxs-lookup"><span data-stu-id="70e32-2645">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="70e32-2646">O processamento de tempo de execução de uma atribuição simples do formulário `x = y` consiste as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="70e32-2646">The run-time processing of a simple assignment of the form `x = y` consists of the following steps:</span></span>

*  <span data-ttu-id="70e32-2647">Se `x` é classificado como uma variável:</span><span class="sxs-lookup"><span data-stu-id="70e32-2647">If `x` is classified as a variable:</span></span>
   * <span data-ttu-id="70e32-2648">`x` é avaliado para produzir a variável.</span><span class="sxs-lookup"><span data-stu-id="70e32-2648">`x` is evaluated to produce the variable.</span></span>
   * <span data-ttu-id="70e32-2649">`y` é avaliado e, se necessário, convertido no tipo de `x` por meio de uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="70e32-2649">`y` is evaluated and, if required, converted to the type of `x` through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
   * <span data-ttu-id="70e32-2650">Se a variável fornecida pelo `x` é um elemento de matriz de um *reference_type*, é realizada uma verificação de tempo de execução para garantir que o valor calculado para `y` é compatível com a instância de matriz da qual `x` é um elemento.</span><span class="sxs-lookup"><span data-stu-id="70e32-2650">If the variable given by `x` is an array element of a *reference_type*, a run-time check is performed to ensure that the value computed for `y` is compatible with the array instance of which `x` is an element.</span></span> <span data-ttu-id="70e32-2651">A verificação for bem-sucedida se `y` está `null`, ou se uma conversão implícita de referência ([conversões de referência implícita](conversions.md#implicit-reference-conversions)) existe do tipo real da instância referenciada por `y` ao tipo de elemento real de que a instância de matriz que contém `x`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2651">The check succeeds if `y` is `null`, or if an implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from the actual type of the instance referenced by `y` to the actual element type of the array instance containing `x`.</span></span> <span data-ttu-id="70e32-2652">Caso contrário, uma `System.ArrayTypeMismatchException` será gerada.</span><span class="sxs-lookup"><span data-stu-id="70e32-2652">Otherwise, a `System.ArrayTypeMismatchException` is thrown.</span></span>
   * <span data-ttu-id="70e32-2653">O valor resultante da avaliação e a conversão de `y` é armazenado no local fornecido pela avaliação de `x`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2653">The value resulting from the evaluation and conversion of `y` is stored into the location given by the evaluation of `x`.</span></span>
*  <span data-ttu-id="70e32-2654">Se `x` é classificado como um acesso de propriedade ou indexador:</span><span class="sxs-lookup"><span data-stu-id="70e32-2654">If `x` is classified as a property or indexer access:</span></span>
   * <span data-ttu-id="70e32-2655">A expressão de instância (se `x` não é `static`) e a lista de argumentos (se `x` é um indexador de acesso) associadas com `x` são avaliados, e os resultados são usados em subsequente `set` invocação do acessador.</span><span class="sxs-lookup"><span data-stu-id="70e32-2655">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `set` accessor invocation.</span></span>
   * <span data-ttu-id="70e32-2656">`y` é avaliado e, se necessário, convertido no tipo de `x` por meio de uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="70e32-2656">`y` is evaluated and, if required, converted to the type of `x` through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
   * <span data-ttu-id="70e32-2657">O `set` acessador da `x` é invocado com o valor calculado para `y` como seu `value` argumento.</span><span class="sxs-lookup"><span data-stu-id="70e32-2657">The `set` accessor of `x` is invoked with the value computed for `y` as its `value` argument.</span></span>

<span data-ttu-id="70e32-2658">As regras de variação conjunta de matriz ([covariância de matriz](arrays.md#array-covariance)) permite um valor de um tipo de matriz `A[]` seja uma referência a uma instância de um tipo de matriz `B[]`, desde que existe uma conversão de referência implícita de `B` para `A`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2658">The array co-variance rules ([Array covariance](arrays.md#array-covariance)) permit a value of an array type `A[]` to be a reference to an instance of an array type `B[]`, provided an implicit reference conversion exists from `B` to `A`.</span></span> <span data-ttu-id="70e32-2659">Devido a essas regras, a atribuição a um elemento de matriz de um *reference_type* requer uma verificação de tempo de execução para garantir que o valor que está sendo atribuído é compatível com a instância de matriz.</span><span class="sxs-lookup"><span data-stu-id="70e32-2659">Because of these rules, assignment to an array element of a *reference_type* requires a run-time check to ensure that the value being assigned is compatible with the array instance.</span></span> <span data-ttu-id="70e32-2660">No exemplo</span><span class="sxs-lookup"><span data-stu-id="70e32-2660">In the example</span></span>
```csharp
string[] sa = new string[10];
object[] oa = sa;

oa[0] = null;               // Ok
oa[1] = "Hello";            // Ok
oa[2] = new ArrayList();    // ArrayTypeMismatchException
```
<span data-ttu-id="70e32-2661">faz com que a última atribuição de um `System.ArrayTypeMismatchException` ser lançada porque uma instância do `ArrayList` não podem ser armazenados em um elemento de um `string[]`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2661">the last assignment causes a `System.ArrayTypeMismatchException` to be thrown because an instance of `ArrayList` cannot be stored in an element of a `string[]`.</span></span>

<span data-ttu-id="70e32-2662">Quando uma propriedade ou indexador declarado em uma *struct_type* é o destino de uma atribuição, a expressão de instância associado à propriedade ou o acesso ao indexador deve ser classificado como uma variável.</span><span class="sxs-lookup"><span data-stu-id="70e32-2662">When a property or indexer declared in a *struct_type* is the target of an assignment, the instance expression associated with the property or indexer access must be classified as a variable.</span></span> <span data-ttu-id="70e32-2663">Se a expressão de instância é classificada como um valor, ocorrerá um erro de tempo de associação.</span><span class="sxs-lookup"><span data-stu-id="70e32-2663">If the instance expression is classified as a value, a binding-time error occurs.</span></span> <span data-ttu-id="70e32-2664">Por causa da [acesso de membro](expressions.md#member-access), a mesma regra também se aplica aos campos.</span><span class="sxs-lookup"><span data-stu-id="70e32-2664">Because of [Member access](expressions.md#member-access), the same rule also applies to fields.</span></span>

<span data-ttu-id="70e32-2665">Dadas as declarações:</span><span class="sxs-lookup"><span data-stu-id="70e32-2665">Given the declarations:</span></span>
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
<span data-ttu-id="70e32-2666">No exemplo</span><span class="sxs-lookup"><span data-stu-id="70e32-2666">in the example</span></span>
```csharp
Point p = new Point();
p.X = 100;
p.Y = 100;
Rectangle r = new Rectangle();
r.A = new Point(10, 10);
r.B = p;
```
<span data-ttu-id="70e32-2667">as atribuições para `p.X`, `p.Y`, `r.A`, e `r.B` são permitidas porque `p` e `r` são variáveis.</span><span class="sxs-lookup"><span data-stu-id="70e32-2667">the assignments to `p.X`, `p.Y`, `r.A`, and `r.B` are permitted because `p` and `r` are variables.</span></span> <span data-ttu-id="70e32-2668">No entanto, no exemplo</span><span class="sxs-lookup"><span data-stu-id="70e32-2668">However, in the example</span></span>
```csharp
Rectangle r = new Rectangle();
r.A.X = 10;
r.A.Y = 10;
r.B.X = 100;
r.B.Y = 100;
```
<span data-ttu-id="70e32-2669">as atribuições são todos inválidas, desde `r.A` e `r.B` não são variáveis.</span><span class="sxs-lookup"><span data-stu-id="70e32-2669">the assignments are all invalid, since `r.A` and `r.B` are not variables.</span></span>

### <a name="compound-assignment"></a><span data-ttu-id="70e32-2670">Atribuição composta</span><span class="sxs-lookup"><span data-stu-id="70e32-2670">Compound assignment</span></span>

<span data-ttu-id="70e32-2671">Se o operando esquerdo de uma atribuição composta tem o formato `E.P` ou `E[Ei]` onde `E` tem o tipo de tempo de compilação `dynamic`, em seguida, a atribuição é associada dinamicamente ([vinculação dinâmica](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="70e32-2671">If the left operand of a compound assignment is of the form `E.P` or `E[Ei]` where `E` has the compile-time type `dynamic`, then the assignment is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="70e32-2672">Nesse caso é o tipo de tempo de compilação da expressão de atribuição `dynamic`, e a resolução descrita a seguir ocorrerá em tempo de execução com base no tipo de tempo de execução do `E`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2672">In this case the compile-time type of the assignment expression is `dynamic`, and the resolution described below will take place at run-time based on the run-time type of `E`.</span></span>

<span data-ttu-id="70e32-2673">Uma operação do formulário `x op= y` é processado por meio da aplicação de resolução de sobrecarga de operador binário ([resolução de sobrecarga de operador binário](expressions.md#binary-operator-overload-resolution)) como se a operação foi escrita `x op y`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2673">An operation of the form `x op= y` is processed by applying binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) as if the operation was written `x op y`.</span></span> <span data-ttu-id="70e32-2674">Em seguida,</span><span class="sxs-lookup"><span data-stu-id="70e32-2674">Then,</span></span>

*  <span data-ttu-id="70e32-2675">Se o tipo de retorno do operador selecionado é implicitamente conversível para o tipo de `x`, a operação é avaliada como `x = x op y`, exceto que `x` é avaliada apenas uma vez.</span><span class="sxs-lookup"><span data-stu-id="70e32-2675">If the return type of the selected operator is implicitly convertible to the type of `x`, the operation is evaluated as `x = x op y`, except that `x` is evaluated only once.</span></span>
*  <span data-ttu-id="70e32-2676">Caso contrário, se o operador selecionado é um operador predefinido, se o tipo de retorno do operador selecionado é explicitamente convertido no tipo de `x`e se `y` é implicitamente conversível para o tipo de `x` ou o operador é um operador, de deslocamento, em seguida, a operação é avaliada como `x = (T)(x op y)`, onde `T` é o tipo de `x`, exceto que `x` é avaliada apenas uma vez.</span><span class="sxs-lookup"><span data-stu-id="70e32-2676">Otherwise, if the selected operator is a predefined operator, if the return type of the selected operator is explicitly convertible to the type of `x`, and if `y` is implicitly convertible to the type of `x` or the operator is a shift operator, then the operation is evaluated as `x = (T)(x op y)`, where `T` is the type of `x`, except that `x` is evaluated only once.</span></span>
*  <span data-ttu-id="70e32-2677">Caso contrário, a atribuição composta é inválida e ocorre um erro em tempo de vinculação.</span><span class="sxs-lookup"><span data-stu-id="70e32-2677">Otherwise, the compound assignment is invalid, and a binding-time error occurs.</span></span>

<span data-ttu-id="70e32-2678">O termo "avaliado apenas uma vez" significa que, na avaliação de `x op y`, os resultados de todas as expressões de constituintes `x` são temporariamente salvos e reutilizados, em seguida, ao executar a atribuição ao `x`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2678">The term "evaluated only once" means that in the evaluation of `x op y`, the results of any constituent expressions of `x` are temporarily saved and then reused when performing the assignment to `x`.</span></span> <span data-ttu-id="70e32-2679">Por exemplo, na atribuição `A()[B()] += C()`, onde `A` é um método que retorna `int[]`, e `B` e `C` são métodos que retornam `int`, os métodos são chamados apenas uma vez, na ordem `A`, `B`, `C`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2679">For example, in the assignment `A()[B()] += C()`, where `A` is a method returning `int[]`, and `B` and `C` are methods returning `int`, the methods are invoked only once, in the order `A`, `B`, `C`.</span></span>

<span data-ttu-id="70e32-2680">Quando o operando esquerdo de uma atribuição composta é um acesso de propriedade ou o acesso do indexador, propriedade ou indexador deve ter uma `get` acessador e um `set` acessador.</span><span class="sxs-lookup"><span data-stu-id="70e32-2680">When the left operand of a compound assignment is a property access or indexer access, the property or indexer must have both a `get` accessor and a `set` accessor.</span></span> <span data-ttu-id="70e32-2681">Se isso não for o caso, ocorre um erro em tempo de vinculação.</span><span class="sxs-lookup"><span data-stu-id="70e32-2681">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="70e32-2682">A segunda regra acima permite `x op= y` a ser avaliada como `x = (T)(x op y)` em determinados contextos.</span><span class="sxs-lookup"><span data-stu-id="70e32-2682">The second rule above permits `x op= y` to be evaluated as `x = (T)(x op y)` in certain contexts.</span></span> <span data-ttu-id="70e32-2683">A regra existe, de modo que a operadores predefinidos podem ser usados como operadores compostos quando o operando esquerdo for do tipo `sbyte`, `byte`, `short`, `ushort`, ou `char`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2683">The rule exists such that the predefined operators can be used as compound operators when the left operand is of type `sbyte`, `byte`, `short`, `ushort`, or `char`.</span></span> <span data-ttu-id="70e32-2684">Até mesmo quando ambos os argumentos forem de um desses tipos, os operadores predefinidos produzem um resultado do tipo `int`, conforme descrito em [promoções numéricas binárias](expressions.md#binary-numeric-promotions).</span><span class="sxs-lookup"><span data-stu-id="70e32-2684">Even when both arguments are of one of those types, the predefined operators produce a result of type `int`, as described in [Binary numeric promotions](expressions.md#binary-numeric-promotions).</span></span> <span data-ttu-id="70e32-2685">Portanto, sem uma conversão não seria possível atribuir o resultado ao operando esquerdo.</span><span class="sxs-lookup"><span data-stu-id="70e32-2685">Thus, without a cast it would not be possible to assign the result to the left operand.</span></span>

<span data-ttu-id="70e32-2686">O efeito intuitivo da regra para operadores predefinidos é simplesmente que `x op= y` é permitido se os dois dos `x op y` e `x = y` são permitidos.</span><span class="sxs-lookup"><span data-stu-id="70e32-2686">The intuitive effect of the rule for predefined operators is simply that `x op= y` is permitted if both of `x op y` and `x = y` are permitted.</span></span> <span data-ttu-id="70e32-2687">No exemplo</span><span class="sxs-lookup"><span data-stu-id="70e32-2687">In the example</span></span>
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
<span data-ttu-id="70e32-2688">o motivo intuitivo para cada erro é que uma simples atribuição correspondente também seria um erro.</span><span class="sxs-lookup"><span data-stu-id="70e32-2688">the intuitive reason for each error is that a corresponding simple assignment would also have been an error.</span></span>

<span data-ttu-id="70e32-2689">Isso também significa que dar suporte a operações de atribuição composta eliminada operações.</span><span class="sxs-lookup"><span data-stu-id="70e32-2689">This also means that compound assignment operations support lifted operations.</span></span> <span data-ttu-id="70e32-2690">No exemplo</span><span class="sxs-lookup"><span data-stu-id="70e32-2690">In the example</span></span>
```csharp
int? i = 0;
i += 1;             // Ok
```
<span data-ttu-id="70e32-2691">o operador elevado `+(int?,int?)` é usado.</span><span class="sxs-lookup"><span data-stu-id="70e32-2691">the lifted operator `+(int?,int?)` is used.</span></span>

### <a name="event-assignment"></a><span data-ttu-id="70e32-2692">Atribuição de evento</span><span class="sxs-lookup"><span data-stu-id="70e32-2692">Event assignment</span></span>

<span data-ttu-id="70e32-2693">Se o operando esquerdo de uma `+=` ou `-=` operador é classificado como um acesso de evento e, em seguida, a expressão é avaliada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="70e32-2693">If the left operand of a `+=` or `-=` operator is classified as an event access, then the expression is evaluated as follows:</span></span>

*  <span data-ttu-id="70e32-2694">A expressão de instância, se houver, do acesso do evento é avaliada.</span><span class="sxs-lookup"><span data-stu-id="70e32-2694">The instance expression, if any, of the event access is evaluated.</span></span>
*  <span data-ttu-id="70e32-2695">O operando direito dos `+=` ou `-=` operador é avaliado e, se necessário, convertido para o tipo do operando à esquerda por meio de uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="70e32-2695">The right operand of the `+=` or `-=` operator is evaluated, and, if required, converted to the type of the left operand through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
*  <span data-ttu-id="70e32-2696">Um acessador de evento do evento é invocado, com a lista de argumentos que consiste do operando à direita, após a avaliação e, se necessário, conversão.</span><span class="sxs-lookup"><span data-stu-id="70e32-2696">An event accessor of the event is invoked, with argument list consisting of the right operand, after evaluation and, if necessary, conversion.</span></span> <span data-ttu-id="70e32-2697">Se o operador foi `+=`, o `add` acessador é invocado; se o operador foi `-=`, o `remove` acessador é invocado.</span><span class="sxs-lookup"><span data-stu-id="70e32-2697">If the operator was `+=`, the `add` accessor is invoked; if the operator was `-=`, the `remove` accessor is invoked.</span></span>

<span data-ttu-id="70e32-2698">Uma expressão de atribuição de evento não produz um valor.</span><span class="sxs-lookup"><span data-stu-id="70e32-2698">An event assignment expression does not yield a value.</span></span> <span data-ttu-id="70e32-2699">Portanto, uma expressão de atribuição de evento é válida somente no contexto de um *statement_expression* ([instruções de expressão](statements.md#expression-statements)).</span><span class="sxs-lookup"><span data-stu-id="70e32-2699">Thus, an event assignment expression is valid only in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>

## <a name="expression"></a><span data-ttu-id="70e32-2700">Expressão</span><span class="sxs-lookup"><span data-stu-id="70e32-2700">Expression</span></span>

<span data-ttu-id="70e32-2701">Uma *expressão* é um *non_assignment_expression* ou uma *atribuição*.</span><span class="sxs-lookup"><span data-stu-id="70e32-2701">An *expression* is either a *non_assignment_expression* or an *assignment*.</span></span>

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

## <a name="constant-expressions"></a><span data-ttu-id="70e32-2702">Expressões constantes</span><span class="sxs-lookup"><span data-stu-id="70e32-2702">Constant expressions</span></span>

<span data-ttu-id="70e32-2703">Um *constant_expression* é uma expressão que pode ser completamente avaliada em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-2703">A *constant_expression* is an expression that can be fully evaluated at compile-time.</span></span>

```antlr
constant_expression
    : expression
    ;
```

<span data-ttu-id="70e32-2704">Uma expressão constante deve ser o `null` literal ou um valor com um dos seguintes tipos: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char` , `float`, `double`, `decimal`, `bool`, `object`, `string`, ou qualquer tipo de enumeração.</span><span class="sxs-lookup"><span data-stu-id="70e32-2704">A constant expression must be the `null` literal or a value with one of  the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `object`, `string`, or any enumeration type.</span></span> <span data-ttu-id="70e32-2705">Somente as seguintes construções são permitidas em expressões constantes:</span><span class="sxs-lookup"><span data-stu-id="70e32-2705">Only the following constructs are permitted in constant expressions:</span></span>

*  <span data-ttu-id="70e32-2706">Literais (incluindo o `null` literal).</span><span class="sxs-lookup"><span data-stu-id="70e32-2706">Literals (including the `null` literal).</span></span>
*  <span data-ttu-id="70e32-2707">As referências a `const` membros de tipos de classe e struct.</span><span class="sxs-lookup"><span data-stu-id="70e32-2707">References to `const` members of class and struct types.</span></span>
*  <span data-ttu-id="70e32-2708">Referências aos membros de tipos de enumeração.</span><span class="sxs-lookup"><span data-stu-id="70e32-2708">References to members of enumeration types.</span></span>
*  <span data-ttu-id="70e32-2709">As referências a `const` parâmetros ou variáveis locais</span><span class="sxs-lookup"><span data-stu-id="70e32-2709">References to `const` parameters or local variables</span></span>
*  <span data-ttu-id="70e32-2710">Entre parênteses subexpressões, que são as próprias expressões de constante.</span><span class="sxs-lookup"><span data-stu-id="70e32-2710">Parenthesized sub-expressions, which are themselves constant expressions.</span></span>
*  <span data-ttu-id="70e32-2711">Expressões de conversão, fornecidas o tipo de destino é um dos tipos listados acima.</span><span class="sxs-lookup"><span data-stu-id="70e32-2711">Cast expressions, provided the target type is one of the types listed above.</span></span>
*  <span data-ttu-id="70e32-2712">`checked` e `unchecked` expressões</span><span class="sxs-lookup"><span data-stu-id="70e32-2712">`checked` and `unchecked` expressions</span></span>
*  <span data-ttu-id="70e32-2713">Expressões de valor padrão</span><span class="sxs-lookup"><span data-stu-id="70e32-2713">Default value expressions</span></span>
*  <span data-ttu-id="70e32-2714">Expressões Nameof</span><span class="sxs-lookup"><span data-stu-id="70e32-2714">Nameof expressions</span></span>
*  <span data-ttu-id="70e32-2715">Predefinido `+`, `-`, `!`, e `~` operadores unários.</span><span class="sxs-lookup"><span data-stu-id="70e32-2715">The predefined `+`, `-`, `!`, and `~` unary operators.</span></span>
*  <span data-ttu-id="70e32-2716">Predefinido `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `|`, `^`, `&&`, `||`, `==`, `!=`, `<`, `>`, `<=`, e `>=` operadores binários, desde que cada operando seja de um tipo listado acima.</span><span class="sxs-lookup"><span data-stu-id="70e32-2716">The predefined `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `|`, `^`, `&&`, `||`, `==`, `!=`, `<`, `>`, `<=`, and `>=` binary operators, provided each operand is of a type listed above.</span></span>
*  <span data-ttu-id="70e32-2717">O `?:` operador condicional.</span><span class="sxs-lookup"><span data-stu-id="70e32-2717">The `?:` conditional operator.</span></span>

<span data-ttu-id="70e32-2718">As conversões a seguir são permitidas em expressões constantes:</span><span class="sxs-lookup"><span data-stu-id="70e32-2718">The following conversions are permitted in constant expressions:</span></span>

*  <span data-ttu-id="70e32-2719">Conversões de identidade</span><span class="sxs-lookup"><span data-stu-id="70e32-2719">Identity conversions</span></span>
*  <span data-ttu-id="70e32-2720">Conversões numéricas</span><span class="sxs-lookup"><span data-stu-id="70e32-2720">Numeric conversions</span></span>
*  <span data-ttu-id="70e32-2721">Conversões de enumeração</span><span class="sxs-lookup"><span data-stu-id="70e32-2721">Enumeration conversions</span></span>
*  <span data-ttu-id="70e32-2722">Conversões de expressão de constante</span><span class="sxs-lookup"><span data-stu-id="70e32-2722">Constant expression conversions</span></span>
*  <span data-ttu-id="70e32-2723">Conversões de referência implícita e explícita, fornecidas a fonte das conversões é uma expressão constante que é avaliada como o valor nulo.</span><span class="sxs-lookup"><span data-stu-id="70e32-2723">Implicit and explicit reference conversions, provided that the source of the conversions is a constant expression that evaluates to the null value.</span></span>

<span data-ttu-id="70e32-2724">Outras conversões, incluindo a conversão boxing, conversão unboxing e referência conversões implícitas de valores não nulos não são permitidas em expressões de constante.</span><span class="sxs-lookup"><span data-stu-id="70e32-2724">Other conversions including boxing, unboxing and implicit reference conversions of non-null values are not permitted in constant expressions.</span></span> <span data-ttu-id="70e32-2725">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="70e32-2725">For example:</span></span>
```csharp
class C {
    const object i = 5;         // error: boxing conversion not permitted
    const object str = "hello"; // error: implicit reference conversion
}
```
<span data-ttu-id="70e32-2726">a inicialização de i é um erro, pois uma conversão boxing é necessária.</span><span class="sxs-lookup"><span data-stu-id="70e32-2726">the initialization of i is an error because a boxing conversion is required.</span></span> <span data-ttu-id="70e32-2727">A inicialização de str é um erro porque uma conversão de referência implícita de um valor não nulo é necessária.</span><span class="sxs-lookup"><span data-stu-id="70e32-2727">The initialization of str is an error because an implicit reference conversion from a non-null value is required.</span></span>

<span data-ttu-id="70e32-2728">Sempre que uma expressão cumpre os requisitos listados acima, a expressão é avaliada em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-2728">Whenever an expression fulfills the requirements listed above, the expression is evaluated at compile-time.</span></span> <span data-ttu-id="70e32-2729">Isso é verdadeiro mesmo se a expressão for uma subexpressão de uma expressão maior que contém construções não constante.</span><span class="sxs-lookup"><span data-stu-id="70e32-2729">This is true even if the expression is a sub-expression of a larger expression that contains non-constant constructs.</span></span>

<span data-ttu-id="70e32-2730">A avaliação do tempo de compilação de expressões de constante usa as mesmas regras como tempo de execução de avaliação de expressões não constantes, exceto que, em que a avaliação de tempo de execução teria sido gerada uma exceção, a avaliação do tempo de compilação faz com que ocorra um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-2730">The compile-time evaluation of constant expressions uses the same rules as run-time evaluation of non-constant expressions, except that where run-time evaluation would have thrown an exception, compile-time evaluation causes a compile-time error to occur.</span></span>

<span data-ttu-id="70e32-2731">A menos que uma expressão de constante seja explicitamente colocada em um `unchecked` contexto, estouros que ocorrem em operações aritméticas de tipo integral e conversões durante a avaliação do tempo de compilação da expressão sempre causar erros de tempo de compilação ([Expressões de constante](expressions.md#constant-expressions)).</span><span class="sxs-lookup"><span data-stu-id="70e32-2731">Unless a constant expression is explicitly placed in an `unchecked` context, overflows that occur in integral-type arithmetic operations and conversions during the compile-time evaluation of the expression always cause compile-time errors ([Constant expressions](expressions.md#constant-expressions)).</span></span>

<span data-ttu-id="70e32-2732">Expressões de constante ocorrerem nos contextos listados abaixo.</span><span class="sxs-lookup"><span data-stu-id="70e32-2732">Constant expressions occur in the contexts listed below.</span></span> <span data-ttu-id="70e32-2733">Nesses contextos, ocorrerá um erro de tempo de compilação se uma expressão não pode ser completamente avaliada em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="70e32-2733">In these contexts, a compile-time error occurs if an expression cannot be fully evaluated at compile-time.</span></span>

*  <span data-ttu-id="70e32-2734">Declarações de constante ([constantes](classes.md#constants)).</span><span class="sxs-lookup"><span data-stu-id="70e32-2734">Constant declarations ([Constants](classes.md#constants)).</span></span>
*  <span data-ttu-id="70e32-2735">Declarações de membro de enumeração ([membros de Enum](enums.md#enum-members)).</span><span class="sxs-lookup"><span data-stu-id="70e32-2735">Enumeration member declarations ([Enum members](enums.md#enum-members)).</span></span>
*  <span data-ttu-id="70e32-2736">Argumentos de parâmetro formal listas padrão ([parâmetros de método](classes.md#method-parameters))</span><span class="sxs-lookup"><span data-stu-id="70e32-2736">Default arguments of formal parameter lists ([Method parameters](classes.md#method-parameters))</span></span>
*  <span data-ttu-id="70e32-2737">`case` rótulos de um `switch` instrução ([da instrução switch](statements.md#the-switch-statement)).</span><span class="sxs-lookup"><span data-stu-id="70e32-2737">`case` labels of a `switch` statement ([The switch statement](statements.md#the-switch-statement)).</span></span>
*  <span data-ttu-id="70e32-2738">`goto case` instruções ([a instrução goto](statements.md#the-goto-statement)).</span><span class="sxs-lookup"><span data-stu-id="70e32-2738">`goto case` statements ([The goto statement](statements.md#the-goto-statement)).</span></span>
*  <span data-ttu-id="70e32-2739">Os tamanhos em uma expressão de criação de matriz da dimensão ([expressões de criação de matriz](expressions.md#array-creation-expressions)) que inclui um inicializador.</span><span class="sxs-lookup"><span data-stu-id="70e32-2739">Dimension lengths in an array creation expression ([Array creation expressions](expressions.md#array-creation-expressions)) that includes an initializer.</span></span>
*  <span data-ttu-id="70e32-2740">Atributos ([atributos](attributes.md)).</span><span class="sxs-lookup"><span data-stu-id="70e32-2740">Attributes ([Attributes](attributes.md)).</span></span>

<span data-ttu-id="70e32-2741">Uma conversão implícita de expressão de constante ([conversões implícitas de expressão de constante](conversions.md#implicit-constant-expression-conversions)) permite que uma expressão constante do tipo `int` a ser convertido em `sbyte`, `byte`, `short`, `ushort`, `uint`, ou `ulong`, desde que o valor da expressão constante é dentro do intervalo de tipo de destino.</span><span class="sxs-lookup"><span data-stu-id="70e32-2741">An implicit constant expression conversion ([Implicit constant expression conversions](conversions.md#implicit-constant-expression-conversions)) permits a constant expression of type `int` to be converted to `sbyte`, `byte`, `short`, `ushort`, `uint`, or `ulong`, provided the value of the constant expression is within the range of the destination type.</span></span>

## <a name="boolean-expressions"></a><span data-ttu-id="70e32-2742">expressões Boolianas</span><span class="sxs-lookup"><span data-stu-id="70e32-2742">Boolean expressions</span></span>

<span data-ttu-id="70e32-2743">Um *boolean_expression* é uma expressão que produz um resultado do tipo `bool`; qualquer diretamente ou por meio da aplicação de `operator true` em determinados contextos conforme especificado no exemplo a seguir.</span><span class="sxs-lookup"><span data-stu-id="70e32-2743">A *boolean_expression* is an expression that yields a result of type `bool`; either directly or through application of `operator true` in certain contexts as specified in the following.</span></span>

```antlr
boolean_expression
    : expression
    ;
```

<span data-ttu-id="70e32-2744">A expressão condicional de controle de um *if_statement* ([if instrução](statements.md#the-if-statement)), *while_statement* ([a instrução while](statements.md#the-while-statement)), *do_statement* ([a instrução](statements.md#the-do-statement)), ou *for_statement* ([a instrução](statements.md#the-for-statement)) é um *boolean_ expressão*.</span><span class="sxs-lookup"><span data-stu-id="70e32-2744">The controlling conditional expression of an *if_statement* ([The if statement](statements.md#the-if-statement)), *while_statement* ([The while statement](statements.md#the-while-statement)), *do_statement* ([The do statement](statements.md#the-do-statement)), or *for_statement* ([The for statement](statements.md#the-for-statement)) is a *boolean_expression*.</span></span> <span data-ttu-id="70e32-2745">A expressão condicional de controle do `?:` operador ([operador condicional](expressions.md#conditional-operator)) segue as mesmas regras que um *boolean_expression*, mas por motivos de operador de precedência é classificada como uma *conditional_or_expression*.</span><span class="sxs-lookup"><span data-stu-id="70e32-2745">The controlling conditional expression of the `?:` operator ([Conditional operator](expressions.md#conditional-operator)) follows the same rules as a *boolean_expression*, but for reasons of operator precedence is classified as a *conditional_or_expression*.</span></span>

<span data-ttu-id="70e32-2746">Um *boolean_expression* `E` deve ser capaz de produzir um valor do tipo `bool`, da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="70e32-2746">A *boolean_expression* `E` is required to be able to produce a value of type `bool`, as follows:</span></span>

*  <span data-ttu-id="70e32-2747">Se `E` é implicitamente conversível para `bool` e em seguida, em tempo de execução que a conversão implícita é aplicada.</span><span class="sxs-lookup"><span data-stu-id="70e32-2747">If `E` is implicitly convertible to `bool` then at runtime that implicit conversion is applied.</span></span>
*  <span data-ttu-id="70e32-2748">Caso contrário, o operador unário de resolução de sobrecarga ([resolução de sobrecarga de operador unário](expressions.md#unary-operator-overload-resolution)) é usado para localizar uma melhor implementação exclusiva de operador `true` em `E`, e que a implementação é aplicada em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="70e32-2748">Otherwise, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is used to find a unique best implementation of operator `true` on `E`, and that implementation is applied at runtime.</span></span>
*  <span data-ttu-id="70e32-2749">Se nenhum operador for encontrado, ocorrerá um erro de tempo de associação.</span><span class="sxs-lookup"><span data-stu-id="70e32-2749">If no such operator is found, a binding-time error occurs.</span></span>

<span data-ttu-id="70e32-2750">O `DBBool` tipo de struct no [banco de dados tipo booliano](structs.md#database-boolean-type) fornece um exemplo de um tipo que implementa `operator true` e `operator false`.</span><span class="sxs-lookup"><span data-stu-id="70e32-2750">The `DBBool` struct type in [Database boolean type](structs.md#database-boolean-type) provides an example of a type that implements `operator true` and `operator false`.</span></span>
