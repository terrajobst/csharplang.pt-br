# <a name="statements"></a><span data-ttu-id="0b841-101">Instruções</span><span class="sxs-lookup"><span data-stu-id="0b841-101">Statements</span></span>

<span data-ttu-id="0b841-102">C# fornece uma variedade de instruções.</span><span class="sxs-lookup"><span data-stu-id="0b841-102">C# provides a variety of statements.</span></span> <span data-ttu-id="0b841-103">A maioria dessas instruções será familiar aos desenvolvedores que já programou em C e C++.</span><span class="sxs-lookup"><span data-stu-id="0b841-103">Most of these statements will be familiar to developers who have programmed in C and C++.</span></span>

```antlr
statement
    : labeled_statement
    | declaration_statement
    | embedded_statement
    ;

embedded_statement
    : block
    | empty_statement
    | expression_statement
    | selection_statement
    | iteration_statement
    | jump_statement
    | try_statement
    | checked_statement
    | unchecked_statement
    | lock_statement
    | using_statement
    | yield_statement
    | embedded_statement_unsafe
    ;
```

<span data-ttu-id="0b841-104">O *embedded_statement* não terminal é usado para as instruções que aparecem dentro de outras instruções.</span><span class="sxs-lookup"><span data-stu-id="0b841-104">The *embedded_statement* nonterminal is used for statements that appear within other statements.</span></span> <span data-ttu-id="0b841-105">O uso de *embedded_statement* vez *instrução* exclui o uso de instruções de declaração e instruções rotuladas nesses contextos.</span><span class="sxs-lookup"><span data-stu-id="0b841-105">The use of *embedded_statement* rather than *statement* excludes the use of declaration statements and labeled statements in these contexts.</span></span> <span data-ttu-id="0b841-106">O exemplo</span><span class="sxs-lookup"><span data-stu-id="0b841-106">The example</span></span>
```csharp
void F(bool b) {
    if (b)
        int i = 44;
}
```
<span data-ttu-id="0b841-107">resulta em um erro de tempo de compilação porque uma `if` instrução requer uma *embedded_statement* em vez de uma *instrução* , se seu branch.</span><span class="sxs-lookup"><span data-stu-id="0b841-107">results in a compile-time error because an `if` statement requires an *embedded_statement* rather than a *statement* for its if branch.</span></span> <span data-ttu-id="0b841-108">Se esse código eram permitidas, em seguida, a variável `i` será declarada, mas nunca pode ser usada.</span><span class="sxs-lookup"><span data-stu-id="0b841-108">If this code were permitted, then the variable `i` would be declared, but it could never be used.</span></span> <span data-ttu-id="0b841-109">No entanto, observe que, colocando `i`da declaração em um bloco, o exemplo é válido.</span><span class="sxs-lookup"><span data-stu-id="0b841-109">Note, however, that by placing `i`'s declaration in a block, the example is valid.</span></span>

## <a name="end-points-and-reachability"></a><span data-ttu-id="0b841-110">Pontos de extremidade e acessibilidade</span><span class="sxs-lookup"><span data-stu-id="0b841-110">End points and reachability</span></span>

<span data-ttu-id="0b841-111">Cada instrução tem um ***ponto de extremidade***.</span><span class="sxs-lookup"><span data-stu-id="0b841-111">Every statement has an ***end point***.</span></span> <span data-ttu-id="0b841-112">Em termos de intuitivos, o ponto de extremidade de uma instrução é o local imediatamente após a instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-112">In intuitive terms, the end point of a statement is the location that immediately follows the statement.</span></span> <span data-ttu-id="0b841-113">As regras de execução de instruções compostas (instruções que contêm instruções inseridas) especificam a ação que é executada quando o controle atinge o ponto de extremidade de uma declaração inserido.</span><span class="sxs-lookup"><span data-stu-id="0b841-113">The execution rules for composite statements (statements that contain embedded statements) specify the action that is taken when control reaches the end point of an embedded statement.</span></span> <span data-ttu-id="0b841-114">Por exemplo, quando o controle atinge o ponto de extremidade de uma instrução em um bloco, o controle é transferido para a próxima instrução no bloco.</span><span class="sxs-lookup"><span data-stu-id="0b841-114">For example, when control reaches the end point of a statement in a block, control is transferred to the next statement in the block.</span></span>

<span data-ttu-id="0b841-115">Se uma instrução, possivelmente, pode ser alcançada pela execução, a instrução será considerada ***acessível***.</span><span class="sxs-lookup"><span data-stu-id="0b841-115">If a statement can possibly be reached by execution, the statement is said to be ***reachable***.</span></span> <span data-ttu-id="0b841-116">Por outro lado, se não houver nenhuma possibilidade de que uma instrução for executada, a instrução será considerada ***inacessível***.</span><span class="sxs-lookup"><span data-stu-id="0b841-116">Conversely, if there is no possibility that a statement will be executed, the statement is said to be ***unreachable***.</span></span>

<span data-ttu-id="0b841-117">No exemplo</span><span class="sxs-lookup"><span data-stu-id="0b841-117">In the example</span></span>
```csharp
void F() {
    Console.WriteLine("reachable");
    goto Label;
    Console.WriteLine("unreachable");
    Label:
    Console.WriteLine("reachable");
}
```
<span data-ttu-id="0b841-118">a segunda chamada de `Console.WriteLine` está inacessível porque não há nenhuma possibilidade de que a instrução será executada.</span><span class="sxs-lookup"><span data-stu-id="0b841-118">the second invocation of `Console.WriteLine` is unreachable because there is no possibility that the statement will be executed.</span></span>

<span data-ttu-id="0b841-119">Um aviso será relatado se o compilador determina que uma instrução está inacessível.</span><span class="sxs-lookup"><span data-stu-id="0b841-119">A warning is reported if the compiler determines that a statement is unreachable.</span></span> <span data-ttu-id="0b841-120">É especificamente não um erro para uma instrução como inacessíveis.</span><span class="sxs-lookup"><span data-stu-id="0b841-120">It is specifically not an error for a statement to be unreachable.</span></span>

<span data-ttu-id="0b841-121">Para determinar se uma determinada instrução ou um ponto de extremidade é acessível, o compilador executa a análise de fluxo de acordo com as regras de acessibilidade definidos para cada instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-121">To determine whether a particular statement or end point is reachable, the compiler performs flow analysis according to the reachability rules defined for each statement.</span></span> <span data-ttu-id="0b841-122">A análise de fluxo leva em consideração os valores das expressões de constante ([expressões constantes](expressions.md#constant-expressions)) que controlam o comportamento das instruções, mas os valores possíveis de expressões não constantes não são considerados.</span><span class="sxs-lookup"><span data-stu-id="0b841-122">The flow analysis takes into account the values of constant expressions ([Constant expressions](expressions.md#constant-expressions)) that control the behavior of statements, but the possible values of non-constant expressions are not considered.</span></span> <span data-ttu-id="0b841-123">Em outras palavras, para fins de análise de fluxo de controle, uma expressão não constante de um determinado tipo é considerada para ter qualquer possível valor desse tipo.</span><span class="sxs-lookup"><span data-stu-id="0b841-123">In other words, for purposes of control flow analysis, a non-constant expression of a given type is considered to have any possible value of that type.</span></span>

<span data-ttu-id="0b841-124">No exemplo</span><span class="sxs-lookup"><span data-stu-id="0b841-124">In the example</span></span>
```csharp
void F() {
    const int i = 1;
    if (i == 2) Console.WriteLine("unreachable");
}
```
<span data-ttu-id="0b841-125">a expressão booliana do `if` instrução é uma expressão constante, porque ambos os operandos do `==` operador são constantes.</span><span class="sxs-lookup"><span data-stu-id="0b841-125">the boolean expression of the `if` statement is a constant expression because both operands of the `==` operator are constants.</span></span> <span data-ttu-id="0b841-126">Como a expressão de constante é avaliada em tempo de compilação, produzindo o valor `false`, o `Console.WriteLine` invocação é considerada inacessível.</span><span class="sxs-lookup"><span data-stu-id="0b841-126">As the constant expression is evaluated at compile-time, producing the value `false`, the `Console.WriteLine` invocation is considered unreachable.</span></span> <span data-ttu-id="0b841-127">No entanto, se `i` é alterada para ser uma variável local</span><span class="sxs-lookup"><span data-stu-id="0b841-127">However, if `i` is changed to be a local variable</span></span>
```csharp
void F() {
    int i = 1;
    if (i == 2) Console.WriteLine("reachable");
}
```
<span data-ttu-id="0b841-128">o `Console.WriteLine` invocação é considerada acessível, mesmo que, na realidade, ele nunca será executado.</span><span class="sxs-lookup"><span data-stu-id="0b841-128">the `Console.WriteLine` invocation is considered reachable, even though, in reality, it will never be executed.</span></span>

<span data-ttu-id="0b841-129">O *bloco* de uma função membro sempre é considerado acessível.</span><span class="sxs-lookup"><span data-stu-id="0b841-129">The *block* of a function member is always considered reachable.</span></span> <span data-ttu-id="0b841-130">Avaliando sucessivamente as regras de acessibilidade de cada instrução em um bloco, a acessibilidade de qualquer instrução fornecida pode ser determinada.</span><span class="sxs-lookup"><span data-stu-id="0b841-130">By successively evaluating the reachability rules of each statement in a block, the reachability of any given statement can be determined.</span></span>

<span data-ttu-id="0b841-131">No exemplo</span><span class="sxs-lookup"><span data-stu-id="0b841-131">In the example</span></span>
```csharp
void F(int x) {
    Console.WriteLine("start");
    if (x < 0) Console.WriteLine("negative");
}
```
<span data-ttu-id="0b841-132">o problema de acessibilidade do segundo `Console.WriteLine` é determinado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="0b841-132">the reachability of the second `Console.WriteLine` is determined as follows:</span></span>

*  <span data-ttu-id="0b841-133">A primeira `Console.WriteLine` instrução de expressão está acessível porque o bloco do `F` método está acessível.</span><span class="sxs-lookup"><span data-stu-id="0b841-133">The first `Console.WriteLine` expression statement is reachable because the block of the `F` method is reachable.</span></span>
*  <span data-ttu-id="0b841-134">O ponto de extremidade do primeiro `Console.WriteLine` instrução de expressão está acessível porque essa instrução está acessível.</span><span class="sxs-lookup"><span data-stu-id="0b841-134">The end point of the first `Console.WriteLine` expression statement is reachable because that statement is reachable.</span></span>
*  <span data-ttu-id="0b841-135">O `if` instrução está acessível porque o ponto de final do primeiro `Console.WriteLine` instrução de expressão está acessível.</span><span class="sxs-lookup"><span data-stu-id="0b841-135">The `if` statement is reachable because the end point of the first `Console.WriteLine` expression statement is reachable.</span></span>
*  <span data-ttu-id="0b841-136">A segunda `Console.WriteLine` instrução de expressão está acessível porque a expressão booliana do `if` instrução não tem o valor da constante `false`.</span><span class="sxs-lookup"><span data-stu-id="0b841-136">The second `Console.WriteLine` expression statement is reachable because the boolean expression of the `if` statement does not have the constant value `false`.</span></span>

<span data-ttu-id="0b841-137">Há duas situações em que é um erro de tempo de compilação para o ponto de extremidade de uma instrução pode ser alcançado:</span><span class="sxs-lookup"><span data-stu-id="0b841-137">There are two situations in which it is a compile-time error for the end point of a statement to be reachable:</span></span>

*  <span data-ttu-id="0b841-138">Porque o `switch` instrução não permite que uma seção switch para "passar" para a próxima seção switch, é um erro de tempo de compilação para o ponto de extremidade da lista de instruções de uma seção switch pode ser alcançado.</span><span class="sxs-lookup"><span data-stu-id="0b841-138">Because the `switch` statement does not permit a switch section to "fall through" to the next switch section, it is a compile-time error for the end point of the statement list of a switch section to be reachable.</span></span> <span data-ttu-id="0b841-139">Se esse erro ocorrer, ele costuma ser uma indicação de que um `break` instrução está ausente.</span><span class="sxs-lookup"><span data-stu-id="0b841-139">If this error occurs, it is typically an indication that a `break` statement is missing.</span></span>
*  <span data-ttu-id="0b841-140">É um erro de tempo de compilação para o ponto de extremidade do bloco de um membro de função que calcula um valor para ser acessível.</span><span class="sxs-lookup"><span data-stu-id="0b841-140">It is a compile-time error for the end point of the block of a function member that computes a value to be reachable.</span></span> <span data-ttu-id="0b841-141">Se esse erro ocorrer, normalmente é uma indicação de que um `return` instrução está ausente.</span><span class="sxs-lookup"><span data-stu-id="0b841-141">If this error occurs, it typically is an indication that a `return` statement is missing.</span></span>

## <a name="blocks"></a><span data-ttu-id="0b841-142">Blocos</span><span class="sxs-lookup"><span data-stu-id="0b841-142">Blocks</span></span>

<span data-ttu-id="0b841-143">Um *bloco* permite a produção de várias instruções em contextos nos quais uma única instrução é permitida.</span><span class="sxs-lookup"><span data-stu-id="0b841-143">A *block* permits multiple statements to be written in contexts where a single statement is allowed.</span></span>

```antlr
block
    : '{' statement_list? '}'
    ;
```

<span data-ttu-id="0b841-144">Um *bloco* consiste em um recurso opcional *statement_list* ([instrução lista](statements.md#statement-lists)), colocado entre chaves.</span><span class="sxs-lookup"><span data-stu-id="0b841-144">A *block* consists of an optional *statement_list* ([Statement lists](statements.md#statement-lists)), enclosed in braces.</span></span> <span data-ttu-id="0b841-145">Se a lista de instruções for omitida, o bloco deve ficar vazio.</span><span class="sxs-lookup"><span data-stu-id="0b841-145">If the statement list is omitted, the block is said to be empty.</span></span>

<span data-ttu-id="0b841-146">Um bloco pode conter instruções de declaração ([instruções de declaração](statements.md#declaration-statements)).</span><span class="sxs-lookup"><span data-stu-id="0b841-146">A block may contain declaration statements ([Declaration statements](statements.md#declaration-statements)).</span></span> <span data-ttu-id="0b841-147">O escopo de um local constante ou variável declarada em um bloco é o bloco.</span><span class="sxs-lookup"><span data-stu-id="0b841-147">The scope of a local variable or constant declared in a block is the block.</span></span>

<span data-ttu-id="0b841-148">Um bloco é executado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="0b841-148">A block is executed as follows:</span></span>

*  <span data-ttu-id="0b841-149">Se o bloco estiver vazio, o controle é transferido para o ponto de extremidade do bloco.</span><span class="sxs-lookup"><span data-stu-id="0b841-149">If the block is empty, control is transferred to the end point of the block.</span></span>
*  <span data-ttu-id="0b841-150">Se o bloco não estiver vazio, o controle é transferido para a lista de instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-150">If the block is not empty, control is transferred to the statement list.</span></span> <span data-ttu-id="0b841-151">Quando e se o controle atinge o ponto de extremidade da lista de instrução, o controle é transferido para o ponto de extremidade do bloco.</span><span class="sxs-lookup"><span data-stu-id="0b841-151">When and if control reaches the end point of the statement list, control is transferred to the end point of the block.</span></span>

<span data-ttu-id="0b841-152">A lista de instrução de um bloco é acessível se o bloco propriamente dito é acessível.</span><span class="sxs-lookup"><span data-stu-id="0b841-152">The statement list of a block is reachable if the block itself is reachable.</span></span>

<span data-ttu-id="0b841-153">O ponto de extremidade de um bloco é acessível se o bloco estiver vazio ou se o ponto de extremidade da lista de instrução estiver acessível.</span><span class="sxs-lookup"><span data-stu-id="0b841-153">The end point of a block is reachable if the block is empty or if the end point of the statement list is reachable.</span></span>

<span data-ttu-id="0b841-154">Um *bloco* que contém um ou mais `yield` instruções ([a instrução yield](statements.md#the-yield-statement)) é chamado de um bloco de iteradores.</span><span class="sxs-lookup"><span data-stu-id="0b841-154">A *block* that contains one or more `yield` statements ([The yield statement](statements.md#the-yield-statement)) is called an iterator block.</span></span> <span data-ttu-id="0b841-155">Blocos de iteradores são usados para implementar membros de função como iteradores ([iteradores](classes.md#iterators)).</span><span class="sxs-lookup"><span data-stu-id="0b841-155">Iterator blocks are used to implement function members as iterators ([Iterators](classes.md#iterators)).</span></span> <span data-ttu-id="0b841-156">Algumas restrições adicionais se aplicam aos blocos de iterador:</span><span class="sxs-lookup"><span data-stu-id="0b841-156">Some additional restrictions apply to iterator blocks:</span></span>

*  <span data-ttu-id="0b841-157">É um erro de tempo de compilação para um `return` instrução para aparecer em um bloco de iteradores (mas `yield return` as instruções são permitidas).</span><span class="sxs-lookup"><span data-stu-id="0b841-157">It is a compile-time error for a `return` statement to appear in an iterator block (but `yield return` statements are permitted).</span></span>
*  <span data-ttu-id="0b841-158">É um erro de tempo de compilação para um bloco de iteradores conter um contexto sem segurança ([contextos não seguros](unsafe-code.md#unsafe-contexts)).</span><span class="sxs-lookup"><span data-stu-id="0b841-158">It is a compile-time error for an iterator block to contain an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span> <span data-ttu-id="0b841-159">Um bloco de iteradores sempre define o contexto de segurança, mesmo quando a sua declaração é aninhada em um contexto sem segurança.</span><span class="sxs-lookup"><span data-stu-id="0b841-159">An iterator block always defines a safe context, even when its declaration is nested in an unsafe context.</span></span>

### <a name="statement-lists"></a><span data-ttu-id="0b841-160">Listas de instrução</span><span class="sxs-lookup"><span data-stu-id="0b841-160">Statement lists</span></span>

<span data-ttu-id="0b841-161">Um ***lista de instruções*** consiste em uma ou mais instruções escritas em sequência.</span><span class="sxs-lookup"><span data-stu-id="0b841-161">A ***statement list*** consists of one or more statements written in sequence.</span></span> <span data-ttu-id="0b841-162">Listas de instrução ocorrem na *bloco*s ([blocos](statements.md#blocks)) e, na *switch_block*s ([da instrução switch](statements.md#the-switch-statement)).</span><span class="sxs-lookup"><span data-stu-id="0b841-162">Statement lists occur in *block*s ([Blocks](statements.md#blocks)) and in *switch_block*s ([The switch statement](statements.md#the-switch-statement)).</span></span>

```antlr
statement_list
    : statement+
    ;
```

<span data-ttu-id="0b841-163">Uma lista de instrução é executada, transferindo o controle para a primeira instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-163">A statement list is executed by transferring control to the first statement.</span></span> <span data-ttu-id="0b841-164">Quando e se o controle atinge o ponto de extremidade de uma instrução, o controle é transferido para a próxima instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-164">When and if control reaches the end point of a statement, control is transferred to the next statement.</span></span> <span data-ttu-id="0b841-165">Quando e se o controle atinge o ponto de extremidade da última instrução, o controle é transferido para o ponto de extremidade da lista de instruções.</span><span class="sxs-lookup"><span data-stu-id="0b841-165">When and if control reaches the end point of the last statement, control is transferred to the end point of the statement list.</span></span>

<span data-ttu-id="0b841-166">Uma instrução em uma lista de instrução é acessível se pelo menos uma das seguintes opções for verdadeira:</span><span class="sxs-lookup"><span data-stu-id="0b841-166">A statement in a statement list is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="0b841-167">A instrução for a primeira instrução e a lista de instrução em si está acessível.</span><span class="sxs-lookup"><span data-stu-id="0b841-167">The statement is the first statement and the statement list itself is reachable.</span></span>
*  <span data-ttu-id="0b841-168">O ponto de extremidade da instrução anterior está acessível.</span><span class="sxs-lookup"><span data-stu-id="0b841-168">The end point of the preceding statement is reachable.</span></span>
*  <span data-ttu-id="0b841-169">A instrução é uma instrução rotulada e o rótulo é referenciado por uma acessível `goto` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-169">The statement is a labeled statement and the label is referenced by a reachable `goto` statement.</span></span>

<span data-ttu-id="0b841-170">O ponto de extremidade de uma lista de instrução é acessível se o ponto de extremidade da última instrução na lista puder ser acessado.</span><span class="sxs-lookup"><span data-stu-id="0b841-170">The end point of a statement list is reachable if the end point of the last statement in the list is reachable.</span></span>

## <a name="the-empty-statement"></a><span data-ttu-id="0b841-171">A instrução vazia</span><span class="sxs-lookup"><span data-stu-id="0b841-171">The empty statement</span></span>

<span data-ttu-id="0b841-172">Uma *empty_statement* não faz nada.</span><span class="sxs-lookup"><span data-stu-id="0b841-172">An *empty_statement* does nothing.</span></span>

```antlr
empty_statement
    : ';'
    ;
```

<span data-ttu-id="0b841-173">Uma instrução vazia é usada quando não houver nenhuma operação para executar em um contexto em que uma instrução é necessária.</span><span class="sxs-lookup"><span data-stu-id="0b841-173">An empty statement is used when there are no operations to perform in a context where a statement is required.</span></span>

<span data-ttu-id="0b841-174">Execução de uma instrução vazia simplesmente transfere o controle para o ponto de extremidade da instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-174">Execution of an empty statement simply transfers control to the end point of the statement.</span></span> <span data-ttu-id="0b841-175">Portanto, o ponto de extremidade de uma instrução vazia é acessível se a instrução vazia está acessível.</span><span class="sxs-lookup"><span data-stu-id="0b841-175">Thus, the end point of an empty statement is reachable if the empty statement is reachable.</span></span>

<span data-ttu-id="0b841-176">Uma instrução vazia pode ser usada ao gravar um `while` instrução com um corpo nulo:</span><span class="sxs-lookup"><span data-stu-id="0b841-176">An empty statement can be used when writing a `while` statement with a null body:</span></span>
```csharp
bool ProcessMessage() {...}

void ProcessMessages() {
    while (ProcessMessage())
        ;
}
```

<span data-ttu-id="0b841-177">Além disso, uma instrução vazia pode ser usada para declarar um rótulo antes do fechamento "`}`" de um bloco:</span><span class="sxs-lookup"><span data-stu-id="0b841-177">Also, an empty statement can be used to declare a label just before the closing "`}`" of a block:</span></span>
```csharp
void F() {
    ...
    if (done) goto exit;
    ...
    exit: ;
}
```

## <a name="labeled-statements"></a><span data-ttu-id="0b841-178">Instruções rotuladas</span><span class="sxs-lookup"><span data-stu-id="0b841-178">Labeled statements</span></span>

<span data-ttu-id="0b841-179">Um *labeled_statement* permite que uma instrução ser precedidos por um rótulo.</span><span class="sxs-lookup"><span data-stu-id="0b841-179">A *labeled_statement* permits a statement to be prefixed by a label.</span></span> <span data-ttu-id="0b841-180">Instruções rotuladas são permitidas em blocos, mas não são permitidas como instruções inseridas.</span><span class="sxs-lookup"><span data-stu-id="0b841-180">Labeled statements are permitted in blocks, but are not permitted as embedded statements.</span></span>

```antlr
labeled_statement
    : identifier ':' statement
    ;
```

<span data-ttu-id="0b841-181">Uma instrução rotulada declara um rótulo com o nome fornecido o *identificador*.</span><span class="sxs-lookup"><span data-stu-id="0b841-181">A labeled statement declares a label with the name given by the *identifier*.</span></span> <span data-ttu-id="0b841-182">O escopo de um rótulo é o bloco inteiro no qual o rótulo é declarado, incluindo quaisquer blocos aninhados.</span><span class="sxs-lookup"><span data-stu-id="0b841-182">The scope of a label is the whole block in which the label is declared, including any nested blocks.</span></span> <span data-ttu-id="0b841-183">É um erro de tempo de compilação para dois rótulos com o mesmo nome com escopos sobrepostos.</span><span class="sxs-lookup"><span data-stu-id="0b841-183">It is a compile-time error for two labels with the same name to have overlapping scopes.</span></span>

<span data-ttu-id="0b841-184">Um rótulo pode ser referenciado em `goto` instruções ([instrução goto](statements.md#the-goto-statement)) dentro do escopo do rótulo.</span><span class="sxs-lookup"><span data-stu-id="0b841-184">A label can be referenced from `goto` statements ([The goto statement](statements.md#the-goto-statement)) within the scope of the label.</span></span> <span data-ttu-id="0b841-185">Isso significa que `goto` instruções podem transferir o controle dentro de blocos e fora de blocos, mas nunca em blocos.</span><span class="sxs-lookup"><span data-stu-id="0b841-185">This means that `goto` statements can transfer control within blocks and out of blocks, but never into blocks.</span></span>

<span data-ttu-id="0b841-186">Rótulos tenham seu próprio espaço de declaração e não interfiram com outros identificadores.</span><span class="sxs-lookup"><span data-stu-id="0b841-186">Labels have their own declaration space and do not interfere with other identifiers.</span></span> <span data-ttu-id="0b841-187">O exemplo</span><span class="sxs-lookup"><span data-stu-id="0b841-187">The example</span></span>
```csharp
int F(int x) {
    if (x >= 0) goto x;
    x = -x;
    x: return x;
}
```
<span data-ttu-id="0b841-188">é válido e usa o nome `x` como um parâmetro e um rótulo.</span><span class="sxs-lookup"><span data-stu-id="0b841-188">is valid and uses the name `x` as both a parameter and a label.</span></span>

<span data-ttu-id="0b841-189">Execução de uma instrução rotulada corresponde exatamente à execução da instrução a seguir o rótulo.</span><span class="sxs-lookup"><span data-stu-id="0b841-189">Execution of a labeled statement corresponds exactly to execution of the statement following the label.</span></span>

<span data-ttu-id="0b841-190">O problema de acessibilidade fornecido pelo fluxo de controle normal, além de uma instrução rotulada é acessível se o rótulo é referenciado por uma acessível `goto` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-190">In addition to the reachability provided by normal flow of control, a labeled statement is reachable if the label is referenced by a reachable `goto` statement.</span></span> <span data-ttu-id="0b841-191">(Exceção: se um `goto` instrução está dentro de uma `try` que inclui um `finally` é de bloco e a instrução rotulada fora o `try`e o ponto de extremidade do `finally` bloco está inacessível e, em seguida, a instrução rotulada não é acessível do que `goto` instrução.)</span><span class="sxs-lookup"><span data-stu-id="0b841-191">(Exception: If a `goto` statement is inside a `try` that includes a `finally` block, and the labeled statement is outside the `try`, and the end point of the `finally` block is unreachable, then the labeled statement is not reachable from that `goto` statement.)</span></span>

## <a name="declaration-statements"></a><span data-ttu-id="0b841-192">Instruções de declaração</span><span class="sxs-lookup"><span data-stu-id="0b841-192">Declaration statements</span></span>

<span data-ttu-id="0b841-193">Um *declaration_statement* declara uma variável local ou uma constante.</span><span class="sxs-lookup"><span data-stu-id="0b841-193">A *declaration_statement* declares a local variable or constant.</span></span> <span data-ttu-id="0b841-194">Instruções de declaração são permitidas em blocos, mas não são permitidas como instruções inseridas.</span><span class="sxs-lookup"><span data-stu-id="0b841-194">Declaration statements are permitted in blocks, but are not permitted as embedded statements.</span></span>

```antlr
declaration_statement
    : local_variable_declaration ';'
    | local_constant_declaration ';'
    ;
```

### <a name="local-variable-declarations"></a><span data-ttu-id="0b841-195">Declarações de variável local</span><span class="sxs-lookup"><span data-stu-id="0b841-195">Local variable declarations</span></span>

<span data-ttu-id="0b841-196">Um *local_variable_declaration* declara um ou mais variáveis locais.</span><span class="sxs-lookup"><span data-stu-id="0b841-196">A *local_variable_declaration* declares one or more local variables.</span></span>

```antlr
local_variable_declaration
    : local_variable_type local_variable_declarators
    ;

local_variable_type
    : type
    | 'var'
    ;

local_variable_declarators
    : local_variable_declarator
    | local_variable_declarators ',' local_variable_declarator
    ;

local_variable_declarator
    : identifier
    | identifier '=' local_variable_initializer
    ;

local_variable_initializer
    : expression
    | array_initializer
    | local_variable_initializer_unsafe
    ;
```

<span data-ttu-id="0b841-197">O *local_variable_type* de uma *local_variable_declaration* diretamente Especifica o tipo das variáveis introduzidas pela declaração, ou indica com o identificador `var` que o tipo deve ser inferido com base em um inicializador.</span><span class="sxs-lookup"><span data-stu-id="0b841-197">The *local_variable_type* of a *local_variable_declaration* either directly specifies the type of the variables introduced by the declaration, or indicates with the identifier `var` that the type should be inferred based on an initializer.</span></span> <span data-ttu-id="0b841-198">O tipo é seguido por uma lista de *local_variable_declarator*s, cada uma delas apresenta uma nova variável.</span><span class="sxs-lookup"><span data-stu-id="0b841-198">The type is followed by a list of *local_variable_declarator*s, each of which introduces a new variable.</span></span> <span data-ttu-id="0b841-199">Um *local_variable_declarator* consiste em um *identificador* que nomeia a variável, opcionalmente seguida por um "`=`" token e um *local_variable_initializer* que fornece o valor inicial da variável.</span><span class="sxs-lookup"><span data-stu-id="0b841-199">A *local_variable_declarator* consists of an *identifier* that names the variable, optionally followed by an "`=`" token and a *local_variable_initializer* that gives the initial value of the variable.</span></span>

<span data-ttu-id="0b841-200">No contexto de uma declaração de variável local, o var de identificador atua como uma palavra-chave contextual ([palavras-chave](lexical-structure.md#keywords)). Quando o *local_variable_type* é especificado como `var` e nenhum tipo denominado `var` está no escopo, a declaração é um ***declaração de variável local de tipo implícito***, cujo tipo é inferido do tipo da expressão de inicializador associado.</span><span class="sxs-lookup"><span data-stu-id="0b841-200">In the context of a local variable declaration, the identifier var acts as a contextual keyword ([Keywords](lexical-structure.md#keywords)).When the *local_variable_type* is specified as `var` and no type named `var` is in scope, the declaration is an ***implicitly typed local variable declaration***, whose type is inferred from the type of the associated initializer expression.</span></span> <span data-ttu-id="0b841-201">Declarações de variável local digitadas implicitamente estão sujeitos às seguintes restrições:</span><span class="sxs-lookup"><span data-stu-id="0b841-201">Implicitly typed local variable declarations are subject to the following restrictions:</span></span>

*  <span data-ttu-id="0b841-202">O *local_variable_declaration* não é possível incluir vários *local_variable_declarator*s.</span><span class="sxs-lookup"><span data-stu-id="0b841-202">The *local_variable_declaration* cannot include multiple *local_variable_declarator*s.</span></span>
*  <span data-ttu-id="0b841-203">O *local_variable_declarator* deve incluir um *local_variable_initializer*.</span><span class="sxs-lookup"><span data-stu-id="0b841-203">The *local_variable_declarator* must include a *local_variable_initializer*.</span></span>
*  <span data-ttu-id="0b841-204">O *local_variable_initializer* deve ser um *expressão*.</span><span class="sxs-lookup"><span data-stu-id="0b841-204">The *local_variable_initializer* must be an *expression*.</span></span>
*  <span data-ttu-id="0b841-205">O inicializador *expressão* deve ter um tipo de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="0b841-205">The initializer *expression* must have a compile-time type.</span></span>
*  <span data-ttu-id="0b841-206">O inicializador *expressão* não pode se referir à variável declarada em si</span><span class="sxs-lookup"><span data-stu-id="0b841-206">The initializer *expression* cannot refer to the declared variable itself</span></span>

<span data-ttu-id="0b841-207">A seguir está exemplos de declarações de variável local digitadas implicitamente incorretos:</span><span class="sxs-lookup"><span data-stu-id="0b841-207">The following are examples of incorrect implicitly typed local variable declarations:</span></span>

```csharp
var x;               // Error, no initializer to infer type from
var y = {1, 2, 3};   // Error, array initializer not permitted
var z = null;        // Error, null does not have a type
var u = x => x + 1;  // Error, anonymous functions do not have a type
var v = v++;         // Error, initializer cannot refer to variable itself
```

<span data-ttu-id="0b841-208">O valor de uma variável local é obtido em uma expressão que usa um *simple_name* ([nomes simples](expressions.md#simple-names)), e o valor de uma variável local é modificado usando um *atribuição* ( [Operadores de atribuição](expressions.md#assignment-operators)).</span><span class="sxs-lookup"><span data-stu-id="0b841-208">The value of a local variable is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)), and the value of a local variable is modified using an *assignment* ([Assignment operators](expressions.md#assignment-operators)).</span></span> <span data-ttu-id="0b841-209">Uma variável local deve ser definitivamente atribuída ([atribuição definitiva](variables.md#definite-assignment)) em cada local em que seu valor é obtido.</span><span class="sxs-lookup"><span data-stu-id="0b841-209">A local variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) at each location where its value is obtained.</span></span>

<span data-ttu-id="0b841-210">O escopo de uma variável local declarada em um *local_variable_declaration* é o bloco no qual ocorre a declaração.</span><span class="sxs-lookup"><span data-stu-id="0b841-210">The scope of a local variable declared in a *local_variable_declaration* is the block in which the declaration occurs.</span></span> <span data-ttu-id="0b841-211">É um erro se referir a uma variável local em uma posição textual que precede o *local_variable_declarator* da variável local.</span><span class="sxs-lookup"><span data-stu-id="0b841-211">It is an error to refer to a local variable in a textual position that precedes the *local_variable_declarator* of the local variable.</span></span> <span data-ttu-id="0b841-212">Dentro do escopo de uma variável local, ele é um erro de tempo de compilação para declarar outro local, variável ou constante com o mesmo nome.</span><span class="sxs-lookup"><span data-stu-id="0b841-212">Within the scope of a local variable, it is a compile-time error to declare another local variable or constant with the same name.</span></span>

<span data-ttu-id="0b841-213">Uma declaração de variável local que declara várias variáveis é equivalente a várias declarações de variáveis únicas com o mesmo tipo.</span><span class="sxs-lookup"><span data-stu-id="0b841-213">A local variable declaration that declares multiple variables is equivalent to multiple declarations of single variables with the same type.</span></span> <span data-ttu-id="0b841-214">Além disso, um inicializador de variável em uma declaração de variável local corresponde exatamente a uma instrução de atribuição é inserida imediatamente após a declaração.</span><span class="sxs-lookup"><span data-stu-id="0b841-214">Furthermore, a variable initializer in a local variable declaration corresponds exactly to an assignment statement that is inserted immediately after the declaration.</span></span>

<span data-ttu-id="0b841-215">O exemplo</span><span class="sxs-lookup"><span data-stu-id="0b841-215">The example</span></span>
```csharp
void F() {
    int x = 1, y, z = x * 2;
}
```
<span data-ttu-id="0b841-216">corresponde exatamente à</span><span class="sxs-lookup"><span data-stu-id="0b841-216">corresponds exactly to</span></span>
```csharp
void F() {
    int x; x = 1;
    int y;
    int z; z = x * 2;
}
```

<span data-ttu-id="0b841-217">Em uma tipada implícito declaração de variável local, o tipo da variável local que está sendo declarado é feito para ser o mesmo que o tipo da expressão usada para inicializar a variável.</span><span class="sxs-lookup"><span data-stu-id="0b841-217">In an implicitly typed local variable declaration, the type of the local variable being declared is taken to be the same as the type of the expression used to initialize the variable.</span></span> <span data-ttu-id="0b841-218">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="0b841-218">For example:</span></span>
```csharp
var i = 5;
var s = "Hello";
var d = 1.0;
var numbers = new int[] {1, 2, 3};
var orders = new Dictionary<int,Order>();
```

<span data-ttu-id="0b841-219">As tipadas implícito declarações de variável local acima são exatamente equivalentes às seguintes declarações digitadas explicitamente:</span><span class="sxs-lookup"><span data-stu-id="0b841-219">The implicitly typed local variable declarations above are precisely equivalent to the following explicitly typed declarations:</span></span>
```csharp
int i = 5;
string s = "Hello";
double d = 1.0;
int[] numbers = new int[] {1, 2, 3};
Dictionary<int,Order> orders = new Dictionary<int,Order>();
```

### <a name="local-constant-declarations"></a><span data-ttu-id="0b841-220">Declarações de constante local</span><span class="sxs-lookup"><span data-stu-id="0b841-220">Local constant declarations</span></span>

<span data-ttu-id="0b841-221">Um *local_constant_declaration* declara um ou mais constantes locais.</span><span class="sxs-lookup"><span data-stu-id="0b841-221">A *local_constant_declaration* declares one or more local constants.</span></span>

```antlr
local_constant_declaration
    : 'const' type constant_declarators
    ;

constant_declarators
    : constant_declarator (',' constant_declarator)*
    ;

constant_declarator
    : identifier '=' constant_expression
    ;
```

<span data-ttu-id="0b841-222">O *tipo* de uma *local_constant_declaration* Especifica o tipo das constantes introduzidas pela declaração.</span><span class="sxs-lookup"><span data-stu-id="0b841-222">The *type* of a *local_constant_declaration* specifies the type of the constants introduced by the declaration.</span></span> <span data-ttu-id="0b841-223">O tipo é seguido por uma lista de *constant_declarator*s, cada uma delas apresenta uma nova constante.</span><span class="sxs-lookup"><span data-stu-id="0b841-223">The type is followed by a list of *constant_declarator*s, each of which introduces a new constant.</span></span> <span data-ttu-id="0b841-224">Um *constant_declarator* consiste em um *identificador* que indica que a constante, seguido por um "`=`" token, seguido por um *constant_expression* ([ Expressões de constante](expressions.md#constant-expressions)) que fornece o valor da constante.</span><span class="sxs-lookup"><span data-stu-id="0b841-224">A *constant_declarator* consists of an *identifier* that names the constant, followed by an "`=`" token, followed by a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) that gives the value of the constant.</span></span>

<span data-ttu-id="0b841-225">O *tipo* e *constant_expression* de uma declaração de constante local deve seguir as mesmas regras que aquelas de uma declaração de membro constante ([constantes](classes.md#constants)).</span><span class="sxs-lookup"><span data-stu-id="0b841-225">The *type* and *constant_expression* of a local constant declaration must follow the same rules as those of a constant member declaration ([Constants](classes.md#constants)).</span></span>

<span data-ttu-id="0b841-226">O valor de uma constante de local é obtido em uma expressão que usa um *simple_name* ([nomes simples](expressions.md#simple-names)).</span><span class="sxs-lookup"><span data-stu-id="0b841-226">The value of a local constant is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)).</span></span>

<span data-ttu-id="0b841-227">O escopo de uma constante local é o bloco no qual ocorre a declaração.</span><span class="sxs-lookup"><span data-stu-id="0b841-227">The scope of a local constant is the block in which the declaration occurs.</span></span> <span data-ttu-id="0b841-228">É um erro para se referir a uma constante local em uma posição textual que precede seus *constant_declarator*.</span><span class="sxs-lookup"><span data-stu-id="0b841-228">It is an error to refer to a local constant in a textual position that precedes its *constant_declarator*.</span></span> <span data-ttu-id="0b841-229">Dentro do escopo de uma constante local, ele é um erro de tempo de compilação para declarar outro local, variável ou constante com o mesmo nome.</span><span class="sxs-lookup"><span data-stu-id="0b841-229">Within the scope of a local constant, it is a compile-time error to declare another local variable or constant with the same name.</span></span>

<span data-ttu-id="0b841-230">Uma declaração de constante local que declara várias constantes é equivalente a várias declarações de constantes únicos com o mesmo tipo.</span><span class="sxs-lookup"><span data-stu-id="0b841-230">A local constant declaration that declares multiple constants is equivalent to multiple declarations of single constants with the same type.</span></span>

## <a name="expression-statements"></a><span data-ttu-id="0b841-231">Instruções de expressão</span><span class="sxs-lookup"><span data-stu-id="0b841-231">Expression statements</span></span>

<span data-ttu-id="0b841-232">Uma *expression_statement* avalia uma expressão fornecida.</span><span class="sxs-lookup"><span data-stu-id="0b841-232">An *expression_statement* evaluates a given expression.</span></span> <span data-ttu-id="0b841-233">O valor computado pela expressão, se houver, serão descartados.</span><span class="sxs-lookup"><span data-stu-id="0b841-233">The value computed by the expression, if any, is discarded.</span></span>

```antlr
expression_statement
    : statement_expression ';'
    ;

statement_expression
    : invocation_expression
    | null_conditional_invocation_expression
    | object_creation_expression
    | assignment
    | post_increment_expression
    | post_decrement_expression
    | pre_increment_expression
    | pre_decrement_expression
    | await_expression
    ;
```

<span data-ttu-id="0b841-234">Nem todas as expressões são permitidas como instruções.</span><span class="sxs-lookup"><span data-stu-id="0b841-234">Not all expressions are permitted as statements.</span></span> <span data-ttu-id="0b841-235">Em particular, expressões, como `x + y` e `x == 1` que simplesmente calcular um valor (que será descartado), não são permitidos como instruções.</span><span class="sxs-lookup"><span data-stu-id="0b841-235">In particular, expressions such as `x + y` and `x == 1` that merely compute a value (which will be discarded), are not permitted as statements.</span></span>

<span data-ttu-id="0b841-236">Execução de um *expression_statement* avalia a expressão independente e, em seguida, transfere o controle para o ponto de extremidade da *expression_statement*.</span><span class="sxs-lookup"><span data-stu-id="0b841-236">Execution of an *expression_statement* evaluates the contained expression and then transfers control to the end point of the *expression_statement*.</span></span> <span data-ttu-id="0b841-237">O ponto de extremidade de uma *expression_statement* é acessível se esse *expression_statement* está acessível.</span><span class="sxs-lookup"><span data-stu-id="0b841-237">The end point of an *expression_statement* is reachable if that *expression_statement* is reachable.</span></span>

## <a name="selection-statements"></a><span data-ttu-id="0b841-238">Instruções de seleção</span><span class="sxs-lookup"><span data-stu-id="0b841-238">Selection statements</span></span>

<span data-ttu-id="0b841-239">Instruções de seleção Selecionar uma das várias instruções possíveis para execução com base no valor de alguma expressão.</span><span class="sxs-lookup"><span data-stu-id="0b841-239">Selection statements select one of a number of possible statements for execution based on the value of some expression.</span></span>

```antlr
selection_statement
    : if_statement
    | switch_statement
    ;
```

### <a name="the-if-statement"></a><span data-ttu-id="0b841-240">If instrução</span><span class="sxs-lookup"><span data-stu-id="0b841-240">The if statement</span></span>

<span data-ttu-id="0b841-241">O `if` instrução seleciona uma instrução para execução com base no valor de uma expressão booliana.</span><span class="sxs-lookup"><span data-stu-id="0b841-241">The `if` statement selects a statement for execution based on the value of a boolean expression.</span></span>

```antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

<span data-ttu-id="0b841-242">Uma `else` part está associada precedente mais próximo lexicalmente `if` que é permitido pela sintaxe.</span><span class="sxs-lookup"><span data-stu-id="0b841-242">An `else` part is associated with the lexically nearest preceding `if` that is allowed by the syntax.</span></span> <span data-ttu-id="0b841-243">Portanto, um `if` instrução do formulário</span><span class="sxs-lookup"><span data-stu-id="0b841-243">Thus, an `if` statement of the form</span></span>
```csharp
if (x) if (y) F(); else G();
```
<span data-ttu-id="0b841-244">equivale a</span><span class="sxs-lookup"><span data-stu-id="0b841-244">is equivalent to</span></span>
```csharp
if (x) {
    if (y) {
        F();
    }
    else {
        G();
    }
}
```

<span data-ttu-id="0b841-245">Um `if` instrução é executada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="0b841-245">An `if` statement is executed as follows:</span></span>

*  <span data-ttu-id="0b841-246">O *boolean_expression* ([expressões Boolianas](expressions.md#boolean-expressions)) é avaliada.</span><span class="sxs-lookup"><span data-stu-id="0b841-246">The *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span>
*  <span data-ttu-id="0b841-247">Se a expressão booliana retorna `true`, o controle é transferido para a primeira instrução inserida.</span><span class="sxs-lookup"><span data-stu-id="0b841-247">If the boolean expression yields `true`, control is transferred to the first embedded statement.</span></span> <span data-ttu-id="0b841-248">Quando e se o controle atinge o ponto de extremidade dessa instrução, o controle é transferido para o ponto de extremidade do `if` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-248">When and if control reaches the end point of that statement, control is transferred to the end point of the `if` statement.</span></span>
*  <span data-ttu-id="0b841-249">Se a expressão booliana produz `false` e, se um `else` parte estiver presente, o controle é transferido para a segunda instrução inserida.</span><span class="sxs-lookup"><span data-stu-id="0b841-249">If the boolean expression yields `false` and if an `else` part is present, control is transferred to the second embedded statement.</span></span> <span data-ttu-id="0b841-250">Quando e se o controle atinge o ponto de extremidade dessa instrução, o controle é transferido para o ponto de extremidade do `if` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-250">When and if control reaches the end point of that statement, control is transferred to the end point of the `if` statement.</span></span>
*  <span data-ttu-id="0b841-251">Se a expressão booliana produz `false` e, se um `else` parte não estiver presente, o controle é transferido para o ponto de extremidade do `if` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-251">If the boolean expression yields `false` and if an `else` part is not present, control is transferred to the end point of the `if` statement.</span></span>

<span data-ttu-id="0b841-252">A primeira incorporado a declaração de um `if` instrução é acessível se o `if` instrução está acessível e se a expressão booliana não tem o valor da constante `false`.</span><span class="sxs-lookup"><span data-stu-id="0b841-252">The first embedded statement of an `if` statement is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `false`.</span></span>

<span data-ttu-id="0b841-253">O segundo inseridos instrução de um `if` instrução, se presente, é acessível se o `if` instrução está acessível e se a expressão booliana não tem o valor da constante `true`.</span><span class="sxs-lookup"><span data-stu-id="0b841-253">The second embedded statement of an `if` statement, if present, is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

<span data-ttu-id="0b841-254">O ponto de extremidade de um `if` instrução é acessível se o ponto de extremidade de pelo menos uma das suas instruções inseridas está acessível.</span><span class="sxs-lookup"><span data-stu-id="0b841-254">The end point of an `if` statement is reachable if the end point of at least one of its embedded statements is reachable.</span></span> <span data-ttu-id="0b841-255">Além disso, do ponto final de uma `if` instrução sem nenhum `else` parte é acessível se o `if` instrução está acessível e se a expressão booliana não tem o valor da constante `true`.</span><span class="sxs-lookup"><span data-stu-id="0b841-255">In addition, the end point of an `if` statement with no `else` part is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-switch-statement"></a><span data-ttu-id="0b841-256">A instrução switch</span><span class="sxs-lookup"><span data-stu-id="0b841-256">The switch statement</span></span>

<span data-ttu-id="0b841-257">A instrução switch seleciona para execução de uma lista de instruções com um rótulo de comutador associado corresponde ao valor da expressão switch.</span><span class="sxs-lookup"><span data-stu-id="0b841-257">The switch statement selects for execution a statement list having an associated switch label that corresponds to the value of the switch expression.</span></span>

```antlr
switch_statement
    : 'switch' '(' expression ')' switch_block
    ;

switch_block
    : '{' switch_section* '}'
    ;

switch_section
    : switch_label+ statement_list
    ;

switch_label
    : 'case' constant_expression ':'
    | 'default' ':'
    ;
```

<span data-ttu-id="0b841-258">Um *switch_statement* consiste na palavra-chave `switch`, seguido por uma expressão entre parênteses (chamada expressão switch), seguido por um *switch_block*.</span><span class="sxs-lookup"><span data-stu-id="0b841-258">A *switch_statement* consists of the keyword `switch`, followed by a parenthesized expression (called the switch expression), followed by a *switch_block*.</span></span> <span data-ttu-id="0b841-259">O *switch_block* consiste de zero ou mais *switch_section*s, colocado entre chaves.</span><span class="sxs-lookup"><span data-stu-id="0b841-259">The *switch_block* consists of zero or more *switch_section*s, enclosed in braces.</span></span> <span data-ttu-id="0b841-260">Cada *switch_section* consiste em um ou mais *switch_label*s seguido por um *statement_list* ([instrução lista](statements.md#statement-lists)).</span><span class="sxs-lookup"><span data-stu-id="0b841-260">Each *switch_section* consists of one or more *switch_label*s followed by a *statement_list* ([Statement lists](statements.md#statement-lists)).</span></span>

<span data-ttu-id="0b841-261">O ***que controlam o tipo*** de um `switch` instrução é estabelecida com a expressão switch.</span><span class="sxs-lookup"><span data-stu-id="0b841-261">The ***governing type*** of a `switch` statement is established by the switch expression.</span></span>

*  <span data-ttu-id="0b841-262">Se for o tipo da expressão switch `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `bool`, `char`, `string`, ou um *enum_type*, ou se é o tipo que permite valor nulo correspondente a um desses tipos, que é o que controlam o tipo do `switch` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-262">If the type of the switch expression is `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `bool`, `char`, `string`, or an *enum_type*, or if it is the nullable type corresponding to one of these types, then that is the governing type of the `switch` statement.</span></span>
*  <span data-ttu-id="0b841-263">Caso contrário, exatamente um definido pelo usuário a conversão implícita ([conversões definidas pelo usuário](conversions.md#user-defined-conversions)) deve existir do tipo da expressão switch para um dos seguintes possíveis que controlam tipos: `sbyte`, `byte`, `short` , `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `string`, ou então, um tipo anulável correspondente a um desses tipos.</span><span class="sxs-lookup"><span data-stu-id="0b841-263">Otherwise, exactly one user-defined implicit conversion ([User-defined conversions](conversions.md#user-defined-conversions)) must exist from the type of the switch expression to one of the following possible governing types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `string`, or,  a nullable type corresponding to one of those types.</span></span>
*  <span data-ttu-id="0b841-264">Caso contrário, se nenhuma conversão implícita existe, ou se mais de uma conversão implícita existe, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="0b841-264">Otherwise, if no such implicit conversion exists, or if more than one such implicit conversion exists, a compile-time error occurs.</span></span>

<span data-ttu-id="0b841-265">A expressão de constante de cada `case` rótulo deve indicar um valor que é implicitamente conversível ([conversões implícitas](conversions.md#implicit-conversions)) para o tipo que governam do `switch` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-265">The constant expression of each `case` label must denote a value that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the governing type of the `switch` statement.</span></span> <span data-ttu-id="0b841-266">Ocorrerá um erro de tempo de compilação se dois ou mais `case` rótulos na mesma `switch` instrução especificar o mesmo valor constante.</span><span class="sxs-lookup"><span data-stu-id="0b841-266">A compile-time error occurs if two or more `case` labels in the same `switch` statement specify the same constant value.</span></span>

<span data-ttu-id="0b841-267">Pode haver no máximo um `default` rótulo em uma instrução switch.</span><span class="sxs-lookup"><span data-stu-id="0b841-267">There can be at most one `default` label in a switch statement.</span></span>

<span data-ttu-id="0b841-268">Um `switch` instrução é executada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="0b841-268">A `switch` statement is executed as follows:</span></span>

*  <span data-ttu-id="0b841-269">A expressão de switch é avaliada e convertida no tipo que governam.</span><span class="sxs-lookup"><span data-stu-id="0b841-269">The switch expression is evaluated and converted to the governing type.</span></span>
*  <span data-ttu-id="0b841-270">Se uma das constantes especificado em uma `case` rótulo na mesma `switch` instrução é igual ao valor da expressão switch, o controle é transferido para a lista de instrução correspondente a seguir `case` rótulo.</span><span class="sxs-lookup"><span data-stu-id="0b841-270">If one of the constants specified in a `case` label in the same `switch` statement is equal to the value of the switch expression, control is transferred to the statement list following the matched `case` label.</span></span>
*  <span data-ttu-id="0b841-271">Se nenhuma das constantes especificadas na `case` rótulos na mesma `switch` instrução é igual ao valor da expressão switch e se um `default` rótulo estiver presente, o controle é transferido para o seguinte lista de instrução de `default` rótulo.</span><span class="sxs-lookup"><span data-stu-id="0b841-271">If none of the constants specified in `case` labels in the same `switch` statement is equal to the value of the switch expression, and if a `default` label is present, control is transferred to the statement list following the `default` label.</span></span>
*  <span data-ttu-id="0b841-272">Se nenhuma das constantes especificadas na `case` rótulos na mesma `switch` instrução é igual ao valor da expressão switch e se nenhum `default` rótulo estiver presente, o controle é transferido para o ponto de extremidade do `switch` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-272">If none of the constants specified in `case` labels in the same `switch` statement is equal to the value of the switch expression, and if no `default` label is present, control is transferred to the end point of the `switch` statement.</span></span>

<span data-ttu-id="0b841-273">Se o ponto de extremidade da lista de instruções de uma seção switch estiver acessível, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="0b841-273">If the end point of the statement list of a switch section is reachable, a compile-time error occurs.</span></span> <span data-ttu-id="0b841-274">Isso é conhecido como a regra "sem queda".</span><span class="sxs-lookup"><span data-stu-id="0b841-274">This is known as the "no fall through" rule.</span></span> <span data-ttu-id="0b841-275">O exemplo</span><span class="sxs-lookup"><span data-stu-id="0b841-275">The example</span></span>
```csharp
switch (i) {
case 0:
    CaseZero();
    break;
case 1:
    CaseOne();
    break;
default:
    CaseOthers();
    break;
}
```
<span data-ttu-id="0b841-276">é válido porque não há seção switch tem um ponto de extremidade acessível.</span><span class="sxs-lookup"><span data-stu-id="0b841-276">is valid because no switch section has a reachable end point.</span></span> <span data-ttu-id="0b841-277">Diferentemente do C e C++, execução de uma seção switch não é permitida para "passar" para a próxima seção de comutador e o exemplo</span><span class="sxs-lookup"><span data-stu-id="0b841-277">Unlike C and C++, execution of a switch section is not permitted to "fall through" to the next switch section, and the example</span></span>
```csharp
switch (i) {
case 0:
    CaseZero();
case 1:
    CaseZeroOrOne();
default:
    CaseAny();
}
```
<span data-ttu-id="0b841-278">resultados em um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="0b841-278">results in a compile-time error.</span></span> <span data-ttu-id="0b841-279">Quando a execução de uma seção switch está a ser seguido pela execução de outra seção de opção explícita `goto case` ou `goto default` declaração deve ser usada:</span><span class="sxs-lookup"><span data-stu-id="0b841-279">When execution of a switch section is to be followed by execution of another switch section, an explicit `goto case` or `goto default` statement must be used:</span></span>
```csharp
switch (i) {
case 0:
    CaseZero();
    goto case 1;
case 1:
    CaseZeroOrOne();
    goto default;
default:
    CaseAny();
    break;
}
```

<span data-ttu-id="0b841-280">Vários rótulos são permitidos em uma *switch_section*.</span><span class="sxs-lookup"><span data-stu-id="0b841-280">Multiple labels are permitted in a *switch_section*.</span></span> <span data-ttu-id="0b841-281">O exemplo</span><span class="sxs-lookup"><span data-stu-id="0b841-281">The example</span></span>
```csharp
switch (i) {
case 0:
    CaseZero();
    break;
case 1:
    CaseOne();
    break;
case 2:
default:
    CaseTwo();
    break;
}
```
<span data-ttu-id="0b841-282">é válido.</span><span class="sxs-lookup"><span data-stu-id="0b841-282">is valid.</span></span> <span data-ttu-id="0b841-283">O exemplo não viola a regra "sem queda" porque os rótulos `case 2:` e `default:` fazem parte do mesmo *switch_section*.</span><span class="sxs-lookup"><span data-stu-id="0b841-283">The example does not violate the "no fall through" rule because the labels `case 2:` and `default:` are part of the same *switch_section*.</span></span>

<span data-ttu-id="0b841-284">A regra "sem queda" impede que uma classe comum de erros que ocorrem em C e C++ quando `break` instruções acidentalmente são omitidas.</span><span class="sxs-lookup"><span data-stu-id="0b841-284">The "no fall through" rule prevents a common class of bugs that occur in C and C++ when `break` statements are accidentally omitted.</span></span> <span data-ttu-id="0b841-285">Além disso, devido a essa regra, as seções switch de uma `switch` instrução pode ser reorganizada arbitrariamente sem afetar o comportamento da instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-285">In addition, because of this rule, the switch sections of a `switch` statement can be arbitrarily rearranged without affecting the behavior of the statement.</span></span> <span data-ttu-id="0b841-286">Por exemplo, as seções do `switch` instrução acima pode ser revertida sem afetar o comportamento da instrução:</span><span class="sxs-lookup"><span data-stu-id="0b841-286">For example, the sections of the `switch` statement above can be reversed without affecting the behavior of the statement:</span></span>
```csharp
switch (i) {
default:
    CaseAny();
    break;
case 1:
    CaseZeroOrOne();
    goto default;
case 0:
    CaseZero();
    goto case 1;
}
```

<span data-ttu-id="0b841-287">A lista de instrução de uma seção switch normalmente termina em um `break`, `goto case`, ou `goto default` instrução, mas qualquer construção que renderiza o ponto de extremidade da lista de instruções inacessível é permitido.</span><span class="sxs-lookup"><span data-stu-id="0b841-287">The statement list of a switch section typically ends in a `break`, `goto case`, or `goto default` statement, but any construct that renders the end point of the statement list unreachable is permitted.</span></span> <span data-ttu-id="0b841-288">Por exemplo, uma `while` instrução controlada por expressão booliana `true` é conhecida por nunca alcance seu ponto de extremidade.</span><span class="sxs-lookup"><span data-stu-id="0b841-288">For example, a `while` statement controlled by the boolean expression `true` is known to never reach its end point.</span></span> <span data-ttu-id="0b841-289">Da mesma forma, uma `throw` ou `return` declaração sempre será transferido em outro lugar do controle e nunca atinge seu ponto de extremidade.</span><span class="sxs-lookup"><span data-stu-id="0b841-289">Likewise, a `throw` or `return` statement always transfers control elsewhere and never reaches its end point.</span></span> <span data-ttu-id="0b841-290">Assim, o exemplo a seguir é válido:</span><span class="sxs-lookup"><span data-stu-id="0b841-290">Thus, the following example is valid:</span></span>
```csharp
switch (i) {
case 0:
    while (true) F();
case 1:
    throw new ArgumentException();
case 2:
    return;
}
```

<span data-ttu-id="0b841-291">O tipo de regulador de uma `switch` instrução pode ser do tipo `string`.</span><span class="sxs-lookup"><span data-stu-id="0b841-291">The governing type of a `switch` statement may be the type `string`.</span></span> <span data-ttu-id="0b841-292">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="0b841-292">For example:</span></span>
```csharp
void DoCommand(string command) {
    switch (command.ToLower()) {
    case "run":
        DoRun();
        break;
    case "save":
        DoSave();
        break;
    case "quit":
        DoQuit();
        break;
    default:
        InvalidCommand(command);
        break;
    }
}
```

<span data-ttu-id="0b841-293">Como os operadores de igualdade de cadeia de caracteres ([operadores de igualdade de cadeia de caracteres](expressions.md#string-equality-operators)), o `switch` instrução diferencia maiusculas de minúsculas e executará uma seção de comutador específico somente se a cadeia de caracteres de expressão de switch corresponde exatamente um `case` rótulo constante.</span><span class="sxs-lookup"><span data-stu-id="0b841-293">Like the string equality operators ([String equality operators](expressions.md#string-equality-operators)), the `switch` statement is case sensitive and will execute a given switch section only if the switch expression string exactly matches a `case` label constant.</span></span>

<span data-ttu-id="0b841-294">Quando o controle de tipo de um `switch` instrução está `string`, o valor `null` é permitido como uma constante de rótulo de caso.</span><span class="sxs-lookup"><span data-stu-id="0b841-294">When the governing type of a `switch` statement is `string`, the value `null` is permitted as a case label constant.</span></span>

<span data-ttu-id="0b841-295">O *statement_list*s de uma *switch_block* pode conter instruções de declaração ([instruções de declaração](statements.md#declaration-statements)).</span><span class="sxs-lookup"><span data-stu-id="0b841-295">The *statement_list*s of a *switch_block* may contain declaration statements ([Declaration statements](statements.md#declaration-statements)).</span></span> <span data-ttu-id="0b841-296">O escopo de um local constante ou variável declarada em um bloco switch é o bloco de comutador.</span><span class="sxs-lookup"><span data-stu-id="0b841-296">The scope of a local variable or constant declared in a switch block is the switch block.</span></span>

<span data-ttu-id="0b841-297">A lista de instrução de uma seção de comutador específico é acessível se o `switch` instrução é acessível e pelo menos uma das seguintes opções for verdadeira:</span><span class="sxs-lookup"><span data-stu-id="0b841-297">The statement list of a given switch section is reachable if the `switch` statement is reachable and at least one of the following is true:</span></span>

*  <span data-ttu-id="0b841-298">A expressão de switch é um valor não constante.</span><span class="sxs-lookup"><span data-stu-id="0b841-298">The switch expression is a non-constant value.</span></span>
*  <span data-ttu-id="0b841-299">A expressão switch é um valor constante que corresponde a um `case` rótulo na seção de opção.</span><span class="sxs-lookup"><span data-stu-id="0b841-299">The switch expression is a constant value that matches a `case` label in the switch section.</span></span>
*  <span data-ttu-id="0b841-300">A expressão switch é um valor constante que não corresponde a qualquer `case` label e seção switch contém o `default` rótulo.</span><span class="sxs-lookup"><span data-stu-id="0b841-300">The switch expression is a constant value that doesn't match any `case` label, and the switch section contains the `default` label.</span></span>
*  <span data-ttu-id="0b841-301">Um rótulo de switch da seção switch é referenciado por uma acessível `goto case` ou `goto default` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-301">A switch label of the switch section is referenced by a reachable `goto case` or `goto default` statement.</span></span>

<span data-ttu-id="0b841-302">O ponto de extremidade de um `switch` instrução é acessível se pelo menos uma das seguintes opções for verdadeira:</span><span class="sxs-lookup"><span data-stu-id="0b841-302">The end point of a `switch` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="0b841-303">O `switch` instrução contém uma acessível `break` instrução que sai do `switch` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-303">The `switch` statement contains a reachable `break` statement that exits the `switch` statement.</span></span>
*  <span data-ttu-id="0b841-304">O `switch` instrução é acessível, a expressão switch é um valor não constante e não `default` rótulo estiver presente.</span><span class="sxs-lookup"><span data-stu-id="0b841-304">The `switch` statement is reachable, the switch expression is a non-constant value, and no `default` label is present.</span></span>
*  <span data-ttu-id="0b841-305">O `switch` instrução é acessível, a expressão switch é um valor constante que não corresponde a qualquer `case` rótulo e não `default` rótulo estiver presente.</span><span class="sxs-lookup"><span data-stu-id="0b841-305">The `switch` statement is reachable, the switch expression is a constant value that doesn't match any `case` label, and no `default` label is present.</span></span>

## <a name="iteration-statements"></a><span data-ttu-id="0b841-306">Instruções de iteração</span><span class="sxs-lookup"><span data-stu-id="0b841-306">Iteration statements</span></span>

<span data-ttu-id="0b841-307">Instruções de iteração executar repetidamente uma instrução inserida.</span><span class="sxs-lookup"><span data-stu-id="0b841-307">Iteration statements repeatedly execute an embedded statement.</span></span>

```antlr
iteration_statement
    : while_statement
    | do_statement
    | for_statement
    | foreach_statement
    ;
```

### <a name="the-while-statement"></a><span data-ttu-id="0b841-308">A instrução while</span><span class="sxs-lookup"><span data-stu-id="0b841-308">The while statement</span></span>

<span data-ttu-id="0b841-309">O `while` instrução executa condicionalmente uma declaração inserido zero ou mais vezes.</span><span class="sxs-lookup"><span data-stu-id="0b841-309">The `while` statement conditionally executes an embedded statement zero or more times.</span></span>

```antlr
while_statement
    : 'while' '(' boolean_expression ')' embedded_statement
    ;
```

<span data-ttu-id="0b841-310">Um `while` instrução é executada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="0b841-310">A `while` statement is executed as follows:</span></span>

*  <span data-ttu-id="0b841-311">O *boolean_expression* ([expressões Boolianas](expressions.md#boolean-expressions)) é avaliada.</span><span class="sxs-lookup"><span data-stu-id="0b841-311">The *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span>
*  <span data-ttu-id="0b841-312">Se a expressão booliana retorna `true`, o controle é transferido para a instrução inserida.</span><span class="sxs-lookup"><span data-stu-id="0b841-312">If the boolean expression yields `true`, control is transferred to the embedded statement.</span></span> <span data-ttu-id="0b841-313">Quando e se o controle atinge o ponto de extremidade da instrução inserida (possivelmente de execução de um `continue` instrução), o controle é transferido para o início do `while` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-313">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), control is transferred to the beginning of the `while` statement.</span></span>
*  <span data-ttu-id="0b841-314">Se a expressão booliana produz `false`, o controle é transferido para o ponto de extremidade do `while` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-314">If the boolean expression yields `false`, control is transferred to the end point of the `while` statement.</span></span>

<span data-ttu-id="0b841-315">Dentro da instrução inserida de um `while` instrução, uma `break` instrução ([a instrução break](statements.md#the-break-statement)) pode ser usado para transferir o controle para o ponto de extremidade do `while` instrução (terminando iteração do inserido instrução) e um `continue` instrução ([a instrução continue](statements.md#the-continue-statement)) pode ser usado para transferir o controle para o ponto de extremidade da instrução inserida (, além de executar outra iteração do `while` instrução).</span><span class="sxs-lookup"><span data-stu-id="0b841-315">Within the embedded statement of a `while` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `while` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement (thus performing another iteration of the `while` statement).</span></span>

<span data-ttu-id="0b841-316">A instrução inserida de um `while` instrução é acessível se o `while` instrução está acessível e se a expressão booliana não tem o valor da constante `false`.</span><span class="sxs-lookup"><span data-stu-id="0b841-316">The embedded statement of a `while` statement is reachable if the `while` statement is reachable and the boolean expression does not have the constant value `false`.</span></span>

<span data-ttu-id="0b841-317">O ponto de extremidade de um `while` instrução é acessível se pelo menos uma das seguintes opções for verdadeira:</span><span class="sxs-lookup"><span data-stu-id="0b841-317">The end point of a `while` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="0b841-318">O `while` instrução contém uma acessível `break` instrução que sai do `while` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-318">The `while` statement contains a reachable `break` statement that exits the `while` statement.</span></span>
*  <span data-ttu-id="0b841-319">O `while` instrução está acessível e se a expressão booliana não tem o valor da constante `true`.</span><span class="sxs-lookup"><span data-stu-id="0b841-319">The `while` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-do-statement"></a><span data-ttu-id="0b841-320">A instrução do</span><span class="sxs-lookup"><span data-stu-id="0b841-320">The do statement</span></span>

<span data-ttu-id="0b841-321">O `do` instrução condicionalmente executa uma instrução inserida uma ou mais vezes.</span><span class="sxs-lookup"><span data-stu-id="0b841-321">The `do` statement conditionally executes an embedded statement one or more times.</span></span>

```antlr
do_statement
    : 'do' embedded_statement 'while' '(' boolean_expression ')' ';'
    ;
```

<span data-ttu-id="0b841-322">Um `do` instrução é executada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="0b841-322">A `do` statement is executed as follows:</span></span>

*  <span data-ttu-id="0b841-323">Controle é transferido para a instrução inserida.</span><span class="sxs-lookup"><span data-stu-id="0b841-323">Control is transferred to the embedded statement.</span></span>
*  <span data-ttu-id="0b841-324">Quando e se o controle atinge o ponto de extremidade da instrução inserida (possivelmente de execução de um `continue` instrução), o *boolean_expression* ([expressões Boolianas](expressions.md#boolean-expressions)) é avaliada.</span><span class="sxs-lookup"><span data-stu-id="0b841-324">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), the *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span> <span data-ttu-id="0b841-325">Se a expressão booliana produz `true`, o controle é transferido para o início do `do` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-325">If the boolean expression yields `true`, control is transferred to the beginning of the `do` statement.</span></span> <span data-ttu-id="0b841-326">Caso contrário, o controle é transferido para o ponto de extremidade do `do` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-326">Otherwise, control is transferred to the end point of the `do` statement.</span></span>

<span data-ttu-id="0b841-327">Dentro da instrução inserida de um `do` instrução, uma `break` instrução ([a instrução break](statements.md#the-break-statement)) pode ser usado para transferir o controle para o ponto de extremidade do `do` instrução (terminando iteração do inserido instrução) e um `continue` instrução ([a instrução continue](statements.md#the-continue-statement)) pode ser usado para transferir o controle para o ponto de extremidade da instrução inserida.</span><span class="sxs-lookup"><span data-stu-id="0b841-327">Within the embedded statement of a `do` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `do` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement.</span></span>

<span data-ttu-id="0b841-328">A instrução inserida de um `do` instrução é acessível se o `do` instrução é acessível.</span><span class="sxs-lookup"><span data-stu-id="0b841-328">The embedded statement of a `do` statement is reachable if the `do` statement is reachable.</span></span>

<span data-ttu-id="0b841-329">O ponto de extremidade de um `do` instrução é acessível se pelo menos uma das seguintes opções for verdadeira:</span><span class="sxs-lookup"><span data-stu-id="0b841-329">The end point of a `do` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="0b841-330">O `do` instrução contém uma acessível `break` instrução que sai do `do` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-330">The `do` statement contains a reachable `break` statement that exits the `do` statement.</span></span>
*  <span data-ttu-id="0b841-331">O ponto de extremidade da instrução inserida está acessível e se a expressão booliana não tem o valor da constante `true`.</span><span class="sxs-lookup"><span data-stu-id="0b841-331">The end point of the embedded statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-for-statement"></a><span data-ttu-id="0b841-332">A instrução for</span><span class="sxs-lookup"><span data-stu-id="0b841-332">The for statement</span></span>

<span data-ttu-id="0b841-333">O `for` instrução avalia uma sequência de expressões de inicialização e em seguida, enquanto uma condição for true, repetidamente executa uma declaração inserido e avalia uma sequência de expressões de iteração.</span><span class="sxs-lookup"><span data-stu-id="0b841-333">The `for` statement evaluates a sequence of initialization expressions and then, while a condition is true, repeatedly executes an embedded statement and evaluates a sequence of iteration expressions.</span></span>

```antlr
for_statement
    : 'for' '(' for_initializer? ';' for_condition? ';' for_iterator? ')' embedded_statement
    ;

for_initializer
    : local_variable_declaration
    | statement_expression_list
    ;

for_condition
    : boolean_expression
    ;

for_iterator
    : statement_expression_list
    ;

statement_expression_list
    : statement_expression (',' statement_expression)*
    ;
```

<span data-ttu-id="0b841-334">O *for_initializer*, se presente, consiste em um *local_variable_declaration* ([declarações de variável Local](statements.md#local-variable-declarations)) ou uma lista de *statement_ expressão*s ([instruções de expressão](statements.md#expression-statements)) separados por vírgulas.</span><span class="sxs-lookup"><span data-stu-id="0b841-334">The *for_initializer*, if present, consists of either a *local_variable_declaration* ([Local variable declarations](statements.md#local-variable-declarations)) or a list of *statement_expression*s ([Expression statements](statements.md#expression-statements)) separated by commas.</span></span> <span data-ttu-id="0b841-335">O escopo de uma variável local declarada com um *for_initializer* começa na *local_variable_declarator* para a variável e se estende até o final da instrução inserida.</span><span class="sxs-lookup"><span data-stu-id="0b841-335">The scope of a local variable declared by a *for_initializer* starts at the *local_variable_declarator* for the variable and extends to the end of the embedded statement.</span></span> <span data-ttu-id="0b841-336">O escopo inclui o *for_condition* e o *for_iterator*.</span><span class="sxs-lookup"><span data-stu-id="0b841-336">The scope includes the *for_condition* and the *for_iterator*.</span></span>

<span data-ttu-id="0b841-337">O *for_condition*, se presente, deve ser um *boolean_expression* ([expressões Boolianas](expressions.md#boolean-expressions)).</span><span class="sxs-lookup"><span data-stu-id="0b841-337">The *for_condition*, if present, must be a *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)).</span></span>

<span data-ttu-id="0b841-338">O *for_iterator*, se presente, consiste em uma lista de *statement_expression*s ([instruções de expressão](statements.md#expression-statements)) separados por vírgulas.</span><span class="sxs-lookup"><span data-stu-id="0b841-338">The *for_iterator*, if present, consists of a list of *statement_expression*s ([Expression statements](statements.md#expression-statements)) separated by commas.</span></span>

<span data-ttu-id="0b841-339">A instrução é executada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="0b841-339">A for statement is executed as follows:</span></span>

*  <span data-ttu-id="0b841-340">Se um *for_initializer* está presente, os inicializadores de variável ou expressões de instrução são executadas na ordem em que elas são gravadas.</span><span class="sxs-lookup"><span data-stu-id="0b841-340">If a *for_initializer* is present, the variable initializers or statement expressions are executed in the order they are written.</span></span> <span data-ttu-id="0b841-341">Esta etapa é executada apenas uma vez.</span><span class="sxs-lookup"><span data-stu-id="0b841-341">This step is only performed once.</span></span>
*  <span data-ttu-id="0b841-342">Se um *for_condition* estiver presente, ela é avaliada.</span><span class="sxs-lookup"><span data-stu-id="0b841-342">If a *for_condition* is present, it is evaluated.</span></span>
*  <span data-ttu-id="0b841-343">Se o *for_condition* não estiver presente ou se a avaliação produz `true`, o controle é transferido para a instrução inserida.</span><span class="sxs-lookup"><span data-stu-id="0b841-343">If the *for_condition* is not present or if the evaluation yields `true`, control is transferred to the embedded statement.</span></span> <span data-ttu-id="0b841-344">Quando e se o controle atinge o ponto de extremidade da instrução inserida (possivelmente de execução de um `continue` instrução), as expressões do *for_iterator*, se houver, são avaliadas em sequência, e, em seguida, outra iteração é executada, começando com a avaliação do *for_condition* na etapa anterior.</span><span class="sxs-lookup"><span data-stu-id="0b841-344">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), the expressions of the *for_iterator*, if any, are evaluated in sequence, and then another iteration is performed, starting with evaluation of the *for_condition* in the step above.</span></span>
*  <span data-ttu-id="0b841-345">Se o *for_condition* está presente e a avaliação produz `false`, o controle é transferido para o ponto de extremidade do `for` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-345">If the *for_condition* is present and the evaluation yields `false`, control is transferred to the end point of the `for` statement.</span></span>

<span data-ttu-id="0b841-346">Dentro da instrução inserida de um `for` instrução, uma `break` instrução ([a instrução break](statements.md#the-break-statement)) pode ser usado para transferir o controle para o ponto de extremidade do `for` instrução (terminando iteração do inserido instrução) e um `continue` instrução ([a instrução continue](statements.md#the-continue-statement)) pode ser usado para transferir o controle para o ponto de extremidade da instrução incorporado (em execução, portanto, o *for_iterator* e executar outra iteração do `for` instrução, começando com o *for_condition*).</span><span class="sxs-lookup"><span data-stu-id="0b841-346">Within the embedded statement of a `for` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `for` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement (thus executing the *for_iterator* and performing another iteration of the `for` statement, starting with the *for_condition*).</span></span>

<span data-ttu-id="0b841-347">A instrução inserida de um `for` instrução é acessível se uma das seguintes opções for verdadeira:</span><span class="sxs-lookup"><span data-stu-id="0b841-347">The embedded statement of a `for` statement is reachable if one of the following is true:</span></span>

*  <span data-ttu-id="0b841-348">O `for` instrução é acessível e não há *for_condition* está presente.</span><span class="sxs-lookup"><span data-stu-id="0b841-348">The `for` statement is reachable and no *for_condition* is present.</span></span>
*  <span data-ttu-id="0b841-349">O `for` instrução é acessível e uma *for_condition* está presente e não tem o valor da constante `false`.</span><span class="sxs-lookup"><span data-stu-id="0b841-349">The `for` statement is reachable and a *for_condition* is present and does not have the constant value `false`.</span></span>

<span data-ttu-id="0b841-350">O ponto de extremidade de um `for` instrução é acessível se pelo menos uma das seguintes opções for verdadeira:</span><span class="sxs-lookup"><span data-stu-id="0b841-350">The end point of a `for` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="0b841-351">O `for` instrução contém uma acessível `break` instrução que sai do `for` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-351">The `for` statement contains a reachable `break` statement that exits the `for` statement.</span></span>
*  <span data-ttu-id="0b841-352">O `for` instrução é acessível e uma *for_condition* está presente e não tem o valor da constante `true`.</span><span class="sxs-lookup"><span data-stu-id="0b841-352">The `for` statement is reachable and a *for_condition* is present and does not have the constant value `true`.</span></span>

### <a name="the-foreach-statement"></a><span data-ttu-id="0b841-353">A instrução foreach</span><span class="sxs-lookup"><span data-stu-id="0b841-353">The foreach statement</span></span>

<span data-ttu-id="0b841-354">O `foreach` instrução enumera os elementos de uma coleção, executando uma instrução inserido para cada elemento da coleção.</span><span class="sxs-lookup"><span data-stu-id="0b841-354">The `foreach` statement enumerates the elements of a collection, executing an embedded statement for each element of the collection.</span></span>

```antlr
foreach_statement
    : 'foreach' '(' local_variable_type identifier 'in' expression ')' embedded_statement
    ;
```

<span data-ttu-id="0b841-355">O *tipo* e *identificador* de um `foreach` instrução declare o ***variável de iteração*** da instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-355">The *type* and *identifier* of a `foreach` statement declare the ***iteration variable*** of the statement.</span></span> <span data-ttu-id="0b841-356">Se o `var` identificador é fornecido como o *local_variable_type*e nenhum tipo denominado `var` está no escopo, a variável de iteração deve ser um ***variável de iteração de tipo implícito***, e seu tipo será considerado como o tipo de elemento de `foreach` instrução, conforme especificado abaixo.</span><span class="sxs-lookup"><span data-stu-id="0b841-356">If the `var` identifier is given as the *local_variable_type*, and no type named `var` is in scope, the iteration variable is said to be an ***implicitly typed iteration variable***, and its type is taken to be the element type of the `foreach` statement, as specified below.</span></span> <span data-ttu-id="0b841-357">A variável de iteração corresponde a uma variável local somente leitura com um escopo que se estende pela instrução inserida.</span><span class="sxs-lookup"><span data-stu-id="0b841-357">The iteration variable corresponds to a read-only local variable with a scope that extends over the embedded statement.</span></span> <span data-ttu-id="0b841-358">Durante a execução de um `foreach` instrução, a variável de iteração representa o elemento da coleção para a qual uma iteração está sendo executada atualmente.</span><span class="sxs-lookup"><span data-stu-id="0b841-358">During execution of a `foreach` statement, the iteration variable represents the collection element for which an iteration is currently being performed.</span></span> <span data-ttu-id="0b841-359">Um erro de tempo de compilação ocorrerá se a instrução inserida tenta modificar a variável de iteração (por meio da atribuição ou o `++` e `--` operadores) ou passe a variável de iteração como uma `ref` ou `out` parâmetro.</span><span class="sxs-lookup"><span data-stu-id="0b841-359">A compile-time error occurs if the embedded statement attempts to modify the iteration variable (via assignment or the `++` and `--` operators) or pass the iteration variable as a `ref` or `out` parameter.</span></span>

<span data-ttu-id="0b841-360">A seguir, para fins de brevidade, `IEnumerable`, `IEnumerator`, `IEnumerable<T>` e `IEnumerator<T>` consultar os tipos correspondentes nos namespaces `System.Collections` e `System.Collections.Generic`.</span><span class="sxs-lookup"><span data-stu-id="0b841-360">In the following, for brevity, `IEnumerable`, `IEnumerator`, `IEnumerable<T>` and `IEnumerator<T>` refer to the corresponding types in the namespaces `System.Collections` and `System.Collections.Generic`.</span></span>

<span data-ttu-id="0b841-361">O processamento de tempo de compilação de uma instrução foreach primeiro determina a ***tipo de coleção***, ***tipo de enumerador*** e ***tipo de elemento*** da expressão.</span><span class="sxs-lookup"><span data-stu-id="0b841-361">The compile-time processing of a foreach statement first determines the ***collection type***, ***enumerator type*** and ***element type*** of the expression.</span></span> <span data-ttu-id="0b841-362">Essa determinação procede da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="0b841-362">This determination proceeds as follows:</span></span>

*  <span data-ttu-id="0b841-363">Se o tipo `X` dos *expressão* é um tipo de matriz, em seguida, há uma conversão de referência implícita de `X` para o `IEnumerable` interface (como `System.Array` implementa essa interface).</span><span class="sxs-lookup"><span data-stu-id="0b841-363">If the type `X` of *expression* is an array type then there is an implicit reference conversion from `X` to the `IEnumerable` interface (since `System.Array` implements this interface).</span></span> <span data-ttu-id="0b841-364">O ***tipo de coleção*** é o `IEnumerable` interface, o ***tipo de enumerador*** é o `IEnumerator` interface e o ***tipo de elemento*** é o tipo de elemento da tipo de matriz `X`.</span><span class="sxs-lookup"><span data-stu-id="0b841-364">The ***collection type*** is the `IEnumerable` interface, the ***enumerator type*** is the `IEnumerator` interface and the ***element type*** is the element type of the array type `X`.</span></span>
*  <span data-ttu-id="0b841-365">Se o tipo `X` dos *expressão* é `dynamic` e em seguida, há uma conversão implícita de *expressão* para o `IEnumerable` interface ([dinâmico implícito conversões](conversions.md#implicit-dynamic-conversions)).</span><span class="sxs-lookup"><span data-stu-id="0b841-365">If the type `X` of *expression* is `dynamic` then there is an implicit conversion from *expression* to the `IEnumerable` interface ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions)).</span></span> <span data-ttu-id="0b841-366">O ***tipo de coleção*** é o `IEnumerable` interface e o ***tipo de enumerador*** é o `IEnumerator` interface.</span><span class="sxs-lookup"><span data-stu-id="0b841-366">The ***collection type*** is the `IEnumerable` interface and the ***enumerator type*** is the `IEnumerator` interface.</span></span> <span data-ttu-id="0b841-367">Se o `var` identificador é fornecido como o *local_variable_type* o ***tipo de elemento*** é `dynamic`, caso contrário, será `object`.</span><span class="sxs-lookup"><span data-stu-id="0b841-367">If the `var` identifier is given as the *local_variable_type* then the ***element type*** is `dynamic`, otherwise it is `object`.</span></span>
*  <span data-ttu-id="0b841-368">Caso contrário, determinar se o tipo `X` tem um apropriado `GetEnumerator` método:</span><span class="sxs-lookup"><span data-stu-id="0b841-368">Otherwise, determine whether the type `X` has an appropriate `GetEnumerator` method:</span></span>
   * <span data-ttu-id="0b841-369">Executar a pesquisa de membro no tipo `X` com o identificador `GetEnumerator` sem nenhum argumento de tipo.</span><span class="sxs-lookup"><span data-stu-id="0b841-369">Perform member lookup on the type `X` with identifier `GetEnumerator` and no type arguments.</span></span> <span data-ttu-id="0b841-370">Se a pesquisa de membro não produz uma correspondência, ou ele gera uma ambiguidade ou produz uma correspondência que não é um grupo de método, procure uma interface enumerável conforme descrito abaixo.</span><span class="sxs-lookup"><span data-stu-id="0b841-370">If the member lookup does not produce a match, or it produces an ambiguity, or produces a match that is not a method group, check for an enumerable interface as described below.</span></span> <span data-ttu-id="0b841-371">É recomendável que um aviso ser emitido se a pesquisa de membro produz nada, exceto um grupo de métodos ou nenhuma correspondência.</span><span class="sxs-lookup"><span data-stu-id="0b841-371">It is recommended that a warning be issued if member lookup produces anything except a method group or no match.</span></span>
   * <span data-ttu-id="0b841-372">Execute a resolução de sobrecarga usando o grupo resultante do método e uma lista de argumentos vazia.</span><span class="sxs-lookup"><span data-stu-id="0b841-372">Perform overload resolution using the resulting method group and an empty argument list.</span></span> <span data-ttu-id="0b841-373">Se a resolução de sobrecarga resulta em nenhum método aplicável, resulta em uma ambiguidade ou resulta em um único método melhor, mas esse método é estático ou não público, procure uma interface enumerável, conforme descrito abaixo.</span><span class="sxs-lookup"><span data-stu-id="0b841-373">If overload resolution results in no applicable methods, results in an ambiguity, or results in a single best method but that method is either static or not public, check for an enumerable interface as described below.</span></span> <span data-ttu-id="0b841-374">É recomendável que um aviso ser emitido se a resolução de sobrecarga produzir qualquer coisa, exceto um método de instância pública não ambíguos ou nenhum método aplicável.</span><span class="sxs-lookup"><span data-stu-id="0b841-374">It is recommended that a warning be issued if overload resolution produces anything except an unambiguous public instance method or no applicable methods.</span></span>
   * <span data-ttu-id="0b841-375">Se o tipo de retorno `E` do `GetEnumerator` método não é uma classe, struct ou interface, tipo, um erro é gerado e nenhuma outra etapa será efetuada.</span><span class="sxs-lookup"><span data-stu-id="0b841-375">If the return type `E` of the `GetEnumerator` method is not a class, struct or interface type, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="0b841-376">Pesquisa de membro é executada em `E` com o identificador `Current` sem nenhum argumento de tipo.</span><span class="sxs-lookup"><span data-stu-id="0b841-376">Member lookup is performed on `E` with the identifier `Current` and no type arguments.</span></span> <span data-ttu-id="0b841-377">Se a pesquisa de membro não produz nenhuma correspondência, o resultado é um erro ou o resultado é que qualquer coisa, exceto uma propriedade de instância pública que permite a leitura, um erro é produzido e nenhuma outra etapa será efetuada.</span><span class="sxs-lookup"><span data-stu-id="0b841-377">If the member lookup produces no match, the result is an error, or the result is anything except a public instance property that permits reading, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="0b841-378">Pesquisa de membro é executada em `E` com o identificador `MoveNext` sem nenhum argumento de tipo.</span><span class="sxs-lookup"><span data-stu-id="0b841-378">Member lookup is performed on `E` with the identifier `MoveNext` and no type arguments.</span></span> <span data-ttu-id="0b841-379">Se a pesquisa de membro não produz nenhuma correspondência, o resultado é um erro ou o resultado é que qualquer coisa, exceto de um grupo de métodos, um erro é produzido e nenhuma outra etapa será efetuada.</span><span class="sxs-lookup"><span data-stu-id="0b841-379">If the member lookup produces no match, the result is an error, or the result is anything except a method group, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="0b841-380">Resolução de sobrecarga é executada no grupo de método com uma lista de argumentos vazia.</span><span class="sxs-lookup"><span data-stu-id="0b841-380">Overload resolution is performed on the method group with an empty argument list.</span></span> <span data-ttu-id="0b841-381">Se os resultados da resolução de sobrecarga em nenhum métodos aplicáveis, resulta em uma ambiguidade ou resulta em um único método melhor, mas esse método é estático ou não público ou seu tipo de retorno não é `bool`, um erro é produzido e nenhuma outra etapa será efetuada.</span><span class="sxs-lookup"><span data-stu-id="0b841-381">If overload resolution results in no applicable methods, results in an ambiguity, or results in a single best method but that method is either static or not public, or its return type is not `bool`, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="0b841-382">O ***tipo de coleção*** é `X`, o ***tipo de enumerador*** é `E`e o ***tipo de elemento*** é o tipo dos `Current` propriedade.</span><span class="sxs-lookup"><span data-stu-id="0b841-382">The ***collection type*** is `X`, the ***enumerator type*** is `E`, and the ***element type*** is the type of the `Current` property.</span></span>

*  <span data-ttu-id="0b841-383">Caso contrário, verifique uma interface enumerável:</span><span class="sxs-lookup"><span data-stu-id="0b841-383">Otherwise, check for an enumerable interface:</span></span>
   * <span data-ttu-id="0b841-384">Se entre todos os tipos `Ti` para as quais há uma conversão implícita de `X` ao `IEnumerable<Ti>`, há um tipo exclusivo `T` , de modo que `T` não é `dynamic` e para todos os outros `Ti` há um conversão implícita de `IEnumerable<T>` à `IEnumerable<Ti>`, em seguida, a ***tipo de coleção*** é a interface `IEnumerable<T>`, o ***tipo de enumerador*** é a interface `IEnumerator<T>`e o ***tipo de elemento*** é `T`.</span><span class="sxs-lookup"><span data-stu-id="0b841-384">If among all the types `Ti` for which there is an implicit conversion from `X` to `IEnumerable<Ti>`, there is a unique type `T` such that `T` is not `dynamic` and for all the other `Ti` there is an implicit conversion from `IEnumerable<T>` to `IEnumerable<Ti>`, then the ***collection type*** is the interface `IEnumerable<T>`, the ***enumerator type*** is the interface `IEnumerator<T>`, and the ***element type*** is `T`.</span></span>
   * <span data-ttu-id="0b841-385">Caso contrário, se houver mais de um tal tipo `T`, em seguida, um erro é produzido e nenhuma outra etapa será efetuada.</span><span class="sxs-lookup"><span data-stu-id="0b841-385">Otherwise, if there is more than one such type `T`, then an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="0b841-386">Caso contrário, se houver uma conversão implícita de `X` para o `System.Collections.IEnumerable` interface, em seguida, a ***tipo de coleção*** é essa interface, o ***tipo de enumerador*** é a interface `System.Collections.IEnumerator`e o ***tipo de elemento*** é `object`.</span><span class="sxs-lookup"><span data-stu-id="0b841-386">Otherwise, if there is an implicit conversion from `X` to the `System.Collections.IEnumerable` interface, then the ***collection type*** is this interface, the ***enumerator type*** is the interface `System.Collections.IEnumerator`, and the ***element type*** is `object`.</span></span>
   * <span data-ttu-id="0b841-387">Caso contrário, um erro é produzido e nenhuma outra etapa será efetuada.</span><span class="sxs-lookup"><span data-stu-id="0b841-387">Otherwise, an error is produced and no further steps are taken.</span></span>

<span data-ttu-id="0b841-388">As etapas acima, se for bem-sucedido, produzirem um tipo de coleção inequivocamente `C`, tipo de enumerador `E` e o tipo de elemento `T`.</span><span class="sxs-lookup"><span data-stu-id="0b841-388">The above steps, if successful, unambiguously produce a collection type `C`, enumerator type `E` and element type `T`.</span></span> <span data-ttu-id="0b841-389">Uma instrução foreach do formulário</span><span class="sxs-lookup"><span data-stu-id="0b841-389">A foreach statement of the form</span></span>
```csharp
foreach (V v in x) embedded_statement
```
<span data-ttu-id="0b841-390">em seguida, é expandido para:</span><span class="sxs-lookup"><span data-stu-id="0b841-390">is then expanded to:</span></span>
```csharp
{
    E e = ((C)(x)).GetEnumerator();
    try {
        while (e.MoveNext()) {
            V v = (V)(T)e.Current;
            embedded_statement
        }
    }
    finally {
        ... // Dispose e
    }
}
```

<span data-ttu-id="0b841-391">A variável `e` não é visível ou acessível para a expressão `x` ou a instrução inserida ou qualquer outro código-fonte do programa.</span><span class="sxs-lookup"><span data-stu-id="0b841-391">The variable `e` is not visible to or accessible to the expression `x` or the embedded statement or any other source code of the program.</span></span> <span data-ttu-id="0b841-392">A variável `v` é somente leitura na instrução inserida.</span><span class="sxs-lookup"><span data-stu-id="0b841-392">The variable `v` is read-only in the embedded statement.</span></span> <span data-ttu-id="0b841-393">Se não houver uma conversão explícita ([conversões explícitas](conversions.md#explicit-conversions)) do `T` (o tipo de elemento) para `V` (o *local_variable_type* na instrução foreach), um erro é produzido e nenhuma outra etapa será efetuada.</span><span class="sxs-lookup"><span data-stu-id="0b841-393">If there is not an explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) from `T` (the element type) to `V` (the *local_variable_type* in the foreach statement), an error is produced and no further steps are taken.</span></span> <span data-ttu-id="0b841-394">Se `x` tem o valor `null`, um `System.NullReferenceException` é gerada em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="0b841-394">If `x` has the value `null`, a `System.NullReferenceException` is thrown at run-time.</span></span>

<span data-ttu-id="0b841-395">Uma implementação tem permissão para implementar uma determinada instrução foreach diferente, por exemplo, por motivos de desempenho, desde que o comportamento é consistente com a expansão acima.</span><span class="sxs-lookup"><span data-stu-id="0b841-395">An implementation is permitted to implement a given foreach-statement differently, e.g. for performance reasons, as long as the behavior is consistent with the above expansion.</span></span>

<span data-ttu-id="0b841-396">O posicionamento dos `v` dentro do while loop é importante para como ele é capturado por qualquer função anônima que ocorrem na *embedded_statement*.</span><span class="sxs-lookup"><span data-stu-id="0b841-396">The placement of `v` inside the while loop is important for how it is captured by any anonymous function occurring in the *embedded_statement*.</span></span>

<span data-ttu-id="0b841-397">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="0b841-397">For example:</span></span>
```csharp
int[] values = { 7, 9, 13 };
Action f = null;

foreach (var value in values)
{
    if (f == null) f = () => Console.WriteLine("First value: " + value);
}

f();
```
<span data-ttu-id="0b841-398">Se `v` foi declarado fora do while loop, seriam compartilhado entre todas as iterações e seu valor após o para loop seria o valor final, `13`, que é o que a invocação do `f` seria impressa.</span><span class="sxs-lookup"><span data-stu-id="0b841-398">If `v` was declared outside of the while loop, it would be shared among all iterations, and its value after the for loop would be the final value, `13`, which is what the invocation of `f` would print.</span></span> <span data-ttu-id="0b841-399">Em vez disso, porque cada iteração tem sua própria variável `v`, aquela capturados pelo `f` na primeira iteração continuará conter o valor `7`, que é o que será impresso.</span><span class="sxs-lookup"><span data-stu-id="0b841-399">Instead, because each iteration has its own variable `v`, the one captured by `f` in the first iteration will continue to hold the value `7`, which is what will be printed.</span></span> <span data-ttu-id="0b841-400">(Observação: as versões anteriores do c# é declarado `v` externa do while loop.)</span><span class="sxs-lookup"><span data-stu-id="0b841-400">(Note: earlier versions of C# declared `v` outside of the while loop.)</span></span>

<span data-ttu-id="0b841-401">O corpo da, por fim, o bloco é construído acordo com as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="0b841-401">The body of the finally block is constructed according to the following steps:</span></span>

*  <span data-ttu-id="0b841-402">Se não houver uma conversão implícita da `E` para o `System.IDisposable` interface, em seguida,</span><span class="sxs-lookup"><span data-stu-id="0b841-402">If there is an implicit conversion from `E` to the `System.IDisposable` interface, then</span></span>
   *  <span data-ttu-id="0b841-403">Se `E` é um tipo de valor não nulo, a, por fim, a cláusula é expandida para o equivalente a semântico:</span><span class="sxs-lookup"><span data-stu-id="0b841-403">If `E` is a non-nullable value type then the finally clause is expanded to the semantic equivalent  of:</span></span>

      ```csharp
      finally {
          ((System.IDisposable)e).Dispose();
      }
      ```

   *  <span data-ttu-id="0b841-404">Caso contrário, o, por fim, a cláusula é expandida para o equivalente a semântico:</span><span class="sxs-lookup"><span data-stu-id="0b841-404">Otherwise the finally clause is expanded to the semantic equivalent of:</span></span>

      ```csharp
      finally {
          if (e != null) ((System.IDisposable)e).Dispose();
      }
      ```

   <span data-ttu-id="0b841-405">exceto se `E` é um tipo de valor ou um parâmetro de tipo instanciado para um tipo de valor e, em seguida, a conversão de `e` para `System.IDisposable` não fará com que a conversão boxing ocorra.</span><span class="sxs-lookup"><span data-stu-id="0b841-405">except that if `E` is a value type, or a type parameter instantiated to a value type, then the cast of `e` to `System.IDisposable` will not cause boxing to occur.</span></span>

*  <span data-ttu-id="0b841-406">Caso contrário, se `E` é um tipo selado, o, por fim, a cláusula é expandida para um bloco vazio:</span><span class="sxs-lookup"><span data-stu-id="0b841-406">Otherwise, if `E` is a sealed type, the finally clause is expanded to an empty block:</span></span>

   ```csharp
   finally {
   }
   ```

*  <span data-ttu-id="0b841-407">Caso contrário, o, por fim, a cláusula é expandida para:</span><span class="sxs-lookup"><span data-stu-id="0b841-407">Otherwise, the finally clause is expanded to:</span></span>

   ```csharp
   finally {
       System.IDisposable d = e as System.IDisposable;
       if (d != null) d.Dispose();
   }
   ```    

   <span data-ttu-id="0b841-408">A variável local `d` não é visível ou acessível para qualquer código do usuário.</span><span class="sxs-lookup"><span data-stu-id="0b841-408">The local variable `d` is not visible to or accessible to any user code.</span></span> <span data-ttu-id="0b841-409">Em particular, ele não entra em conflito com qualquer outra variável cujo escopo inclui o bloco finally.</span><span class="sxs-lookup"><span data-stu-id="0b841-409">In particular, it does not conflict with any other variable whose scope includes the finally block.</span></span>

<span data-ttu-id="0b841-410">A ordem na qual `foreach` percorre os elementos de uma matriz, é da seguinte maneira: para elementos de matrizes unidimensionais são percorridos em ordem de índice crescente, começando com o índice `0` e terminando com um índice `Length - 1`.</span><span class="sxs-lookup"><span data-stu-id="0b841-410">The order in which `foreach` traverses the elements of an array, is as follows: For single-dimensional arrays elements are traversed in increasing index order, starting with index `0` and ending with index `Length - 1`.</span></span> <span data-ttu-id="0b841-411">Para matrizes multidimensionais, os elementos são percorridos, de modo que os índices da dimensão mais à direita são primeiro maior, em seguida, a próxima dimensão à esquerda, e assim por diante para a esquerda.</span><span class="sxs-lookup"><span data-stu-id="0b841-411">For multi-dimensional arrays, elements are traversed such that the indices of the rightmost dimension are increased first, then the next left dimension, and so on to the left.</span></span>

<span data-ttu-id="0b841-412">O exemplo a seguir imprime cada valor em uma matriz bidimensional, na ordem dos elementos:</span><span class="sxs-lookup"><span data-stu-id="0b841-412">The following example prints out each value in a two-dimensional array, in element order:</span></span>
```csharp
using System;

class Test
{
    static void Main() {
        double[,] values = {
            {1.2, 2.3, 3.4, 4.5},
            {5.6, 6.7, 7.8, 8.9}
        };

        foreach (double elementValue in values)
            Console.Write("{0} ", elementValue);

        Console.WriteLine();
    }
}
```
<span data-ttu-id="0b841-413">A saída produzida é o seguinte:</span><span class="sxs-lookup"><span data-stu-id="0b841-413">The output produced is as follows:</span></span>
```csharp
1.2 2.3 3.4 4.5 5.6 6.7 7.8 8.9
```

<span data-ttu-id="0b841-414">No exemplo</span><span class="sxs-lookup"><span data-stu-id="0b841-414">In the example</span></span>
```csharp
int[] numbers = { 1, 3, 5, 7, 9 };
foreach (var n in numbers) Console.WriteLine(n);
```
<span data-ttu-id="0b841-415">o tipo de `n` será inferida para ser `int`, o tipo de elemento `numbers`.</span><span class="sxs-lookup"><span data-stu-id="0b841-415">the type of `n` is inferred to be `int`, the element type of `numbers`.</span></span>

## <a name="jump-statements"></a><span data-ttu-id="0b841-416">Instruções de hiperlink</span><span class="sxs-lookup"><span data-stu-id="0b841-416">Jump statements</span></span>

<span data-ttu-id="0b841-417">Instruções de hiperlink incondicionalmente transferir o controle.</span><span class="sxs-lookup"><span data-stu-id="0b841-417">Jump statements unconditionally transfer control.</span></span>

```antlr
jump_statement
    : break_statement
    | continue_statement
    | goto_statement
    | return_statement
    | throw_statement
    ;
```

<span data-ttu-id="0b841-418">O local para o qual uma instrução jump transfere o controle é chamado de ***destino*** da instrução de salto.</span><span class="sxs-lookup"><span data-stu-id="0b841-418">The location to which a jump statement transfers control is called the ***target*** of the jump statement.</span></span>

<span data-ttu-id="0b841-419">Quando uma instrução de salto ocorre dentro de um bloco, e o destino da instrução jump estiver fora desse bloco, a instrução de salto é considerada ***sair*** o bloco.</span><span class="sxs-lookup"><span data-stu-id="0b841-419">When a jump statement occurs within a block, and the target of that jump statement is outside that block, the jump statement is said to ***exit*** the block.</span></span> <span data-ttu-id="0b841-420">Enquanto uma instrução de salto pode transferir o controle para fora de um bloco, ele nunca pode transferir o controle em um bloco.</span><span class="sxs-lookup"><span data-stu-id="0b841-420">While a jump statement may transfer control out of a block, it can never transfer control into a block.</span></span>

<span data-ttu-id="0b841-421">Execução de instruções de salto é complicada pela presença de intervenção `try` instruções.</span><span class="sxs-lookup"><span data-stu-id="0b841-421">Execution of jump statements is complicated by the presence of intervening `try` statements.</span></span> <span data-ttu-id="0b841-422">Na ausência de tais `try` instruções, uma instrução jump transfere o controle incondicionalmente da instrução de salto para seu destino.</span><span class="sxs-lookup"><span data-stu-id="0b841-422">In the absence of such `try` statements, a jump statement unconditionally transfers control from the jump statement to its target.</span></span> <span data-ttu-id="0b841-423">Na presença de tais intermediária `try` instruções, a execução é mais complexa.</span><span class="sxs-lookup"><span data-stu-id="0b841-423">In the presence of such intervening `try` statements, execution is more complex.</span></span> <span data-ttu-id="0b841-424">Se a instrução de salto sai de um ou mais `try` blocos com associados `finally` inicialmente, blocos de controle é transferido para o `finally` bloco de mais interna `try` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-424">If the jump statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="0b841-425">Quando e se o controle atinge o ponto de extremidade de uma `finally` bloco, o controle é transferido para o `finally` bloco de circunscrição próxima `try` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-425">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="0b841-426">Esse processo é repetido até que o `finally` blocos de todos os intermediários `try` instruções foram executadas.</span><span class="sxs-lookup"><span data-stu-id="0b841-426">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>

<span data-ttu-id="0b841-427">No exemplo</span><span class="sxs-lookup"><span data-stu-id="0b841-427">In the example</span></span>
```csharp
using System;

class Test
{
    static void Main() {
        while (true) {
            try {
                try {
                    Console.WriteLine("Before break");
                    break;
                }
                finally {
                    Console.WriteLine("Innermost finally block");
                }
            }
            finally {
                Console.WriteLine("Outermost finally block");
            }
        }
        Console.WriteLine("After break");
    }
}
```
<span data-ttu-id="0b841-428">o `finally` blocos associados com dois `try` as instruções são executadas antes do controle é transferido para o destino da instrução de salto.</span><span class="sxs-lookup"><span data-stu-id="0b841-428">the `finally` blocks associated with two `try` statements are executed before control is transferred to the target of the jump statement.</span></span>

<span data-ttu-id="0b841-429">A saída produzida é o seguinte:</span><span class="sxs-lookup"><span data-stu-id="0b841-429">The output produced is as follows:</span></span>
```
Before break
Innermost finally block
Outermost finally block
After break
```

### <a name="the-break-statement"></a><span data-ttu-id="0b841-430">A instrução break</span><span class="sxs-lookup"><span data-stu-id="0b841-430">The break statement</span></span>

<span data-ttu-id="0b841-431">O `break` instrução sai delimitadora mais próxima `switch`, `while`, `do`, `for`, ou `foreach` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-431">The `break` statement exits the nearest enclosing `switch`, `while`, `do`, `for`, or `foreach` statement.</span></span>

```antlr
break_statement
    : 'break' ';'
    ;
```

<span data-ttu-id="0b841-432">O destino de uma `break` instrução é o ponto de extremidade de delimitadora mais próxima `switch`, `while`, `do`, `for`, ou `foreach` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-432">The target of a `break` statement is the end point of the nearest enclosing `switch`, `while`, `do`, `for`, or `foreach` statement.</span></span> <span data-ttu-id="0b841-433">Se um `break` instrução não é incluída por um `switch`, `while`, `do`, `for`, ou `foreach` declaração, um erro de tempo de compilação ocorre.</span><span class="sxs-lookup"><span data-stu-id="0b841-433">If a `break` statement is not enclosed by a `switch`, `while`, `do`, `for`, or `foreach` statement, a compile-time error occurs.</span></span>

<span data-ttu-id="0b841-434">Quando vários `switch`, `while`, `do`, `for`, ou `foreach` instruções estão aninhadas dentro uns dos outros, um `break` instrução se aplica somente à instrução mais interna.</span><span class="sxs-lookup"><span data-stu-id="0b841-434">When multiple `switch`, `while`, `do`, `for`, or `foreach` statements are nested within each other, a `break` statement applies only to the innermost statement.</span></span> <span data-ttu-id="0b841-435">Para transferir o controle em vários níveis de aninhamento, uma `goto` instrução ([instrução goto](statements.md#the-goto-statement)) deve ser usado.</span><span class="sxs-lookup"><span data-stu-id="0b841-435">To transfer control across multiple nesting levels, a `goto` statement ([The goto statement](statements.md#the-goto-statement)) must be used.</span></span>

<span data-ttu-id="0b841-436">Um `break` instrução não é possível sair um `finally` bloco ([a instrução try](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="0b841-436">A `break` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="0b841-437">Quando um `break` instrução ocorre dentro de uma `finally` bloquear, o destino da `break` instrução deve ser dentro do mesmo `finally` bloquear; caso contrário, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="0b841-437">When a `break` statement occurs within a `finally` block, the target of the `break` statement must be within the same `finally` block; otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="0b841-438">Um `break` instrução é executada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="0b841-438">A `break` statement is executed as follows:</span></span>

*  <span data-ttu-id="0b841-439">Se o `break` instrução sai de um ou mais `try` blocos com associados `finally` inicialmente, blocos de controle é transferido para o `finally` bloco de mais interna `try` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-439">If the `break` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="0b841-440">Quando e se o controle atinge o ponto de extremidade de uma `finally` bloco, o controle é transferido para o `finally` bloco de circunscrição próxima `try` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-440">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="0b841-441">Esse processo é repetido até que o `finally` blocos de todos os intermediários `try` instruções foram executadas.</span><span class="sxs-lookup"><span data-stu-id="0b841-441">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="0b841-442">O controle é transferido para o destino do `break` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-442">Control is transferred to the target of the `break` statement.</span></span>

<span data-ttu-id="0b841-443">Como uma `break` instrução incondicionalmente transfere o controle em outro lugar, o ponto de extremidade de um `break` instrução nunca é acessível.</span><span class="sxs-lookup"><span data-stu-id="0b841-443">Because a `break` statement unconditionally transfers control elsewhere, the end point of a `break` statement is never reachable.</span></span>

### <a name="the-continue-statement"></a><span data-ttu-id="0b841-444">A instrução continue</span><span class="sxs-lookup"><span data-stu-id="0b841-444">The continue statement</span></span>

<span data-ttu-id="0b841-445">O `continue` instrução inicia uma nova iteração do delimitadora mais próxima `while`, `do`, `for`, ou `foreach` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-445">The `continue` statement starts a new iteration of the nearest enclosing `while`, `do`, `for`, or `foreach` statement.</span></span>

```antlr
continue_statement
    : 'continue' ';'
    ;
```

<span data-ttu-id="0b841-446">O destino de uma `continue` instrução é o ponto de extremidade da instrução inserida de delimitadora mais próxima `while`, `do`, `for`, ou `foreach` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-446">The target of a `continue` statement is the end point of the embedded statement of the nearest enclosing `while`, `do`, `for`, or `foreach` statement.</span></span> <span data-ttu-id="0b841-447">Se um `continue` instrução não é incluída por um `while`, `do`, `for`, ou `foreach` declaração, um erro de tempo de compilação ocorre.</span><span class="sxs-lookup"><span data-stu-id="0b841-447">If a `continue` statement is not enclosed by a `while`, `do`, `for`, or `foreach` statement, a compile-time error occurs.</span></span>

<span data-ttu-id="0b841-448">Quando vários `while`, `do`, `for`, ou `foreach` instruções estão aninhadas dentro uns dos outros, um `continue` instrução se aplica somente à instrução mais interna.</span><span class="sxs-lookup"><span data-stu-id="0b841-448">When multiple `while`, `do`, `for`, or `foreach` statements are nested within each other, a `continue` statement applies only to the innermost statement.</span></span> <span data-ttu-id="0b841-449">Para transferir o controle em vários níveis de aninhamento, uma `goto` instrução ([instrução goto](statements.md#the-goto-statement)) deve ser usado.</span><span class="sxs-lookup"><span data-stu-id="0b841-449">To transfer control across multiple nesting levels, a `goto` statement ([The goto statement](statements.md#the-goto-statement)) must be used.</span></span>

<span data-ttu-id="0b841-450">Um `continue` instrução não é possível sair um `finally` bloco ([a instrução try](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="0b841-450">A `continue` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="0b841-451">Quando um `continue` instrução ocorre dentro de uma `finally` bloquear, o destino da `continue` instrução deve ser dentro do mesmo `finally` bloquear; caso contrário, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="0b841-451">When a `continue` statement occurs within a `finally` block, the target of the `continue` statement must be within the same `finally` block; otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="0b841-452">Um `continue` instrução é executada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="0b841-452">A `continue` statement is executed as follows:</span></span>

*  <span data-ttu-id="0b841-453">Se o `continue` instrução sai de um ou mais `try` blocos com associados `finally` inicialmente, blocos de controle é transferido para o `finally` bloco de mais interna `try` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-453">If the `continue` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="0b841-454">Quando e se o controle atinge o ponto de extremidade de uma `finally` bloco, o controle é transferido para o `finally` bloco de circunscrição próxima `try` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-454">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="0b841-455">Esse processo é repetido até que o `finally` blocos de todos os intermediários `try` instruções foram executadas.</span><span class="sxs-lookup"><span data-stu-id="0b841-455">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="0b841-456">O controle é transferido para o destino do `continue` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-456">Control is transferred to the target of the `continue` statement.</span></span>

<span data-ttu-id="0b841-457">Como uma `continue` instrução incondicionalmente transfere o controle em outro lugar, o ponto de extremidade de um `continue` instrução nunca é acessível.</span><span class="sxs-lookup"><span data-stu-id="0b841-457">Because a `continue` statement unconditionally transfers control elsewhere, the end point of a `continue` statement is never reachable.</span></span>

### <a name="the-goto-statement"></a><span data-ttu-id="0b841-458">A instrução goto</span><span class="sxs-lookup"><span data-stu-id="0b841-458">The goto statement</span></span>

<span data-ttu-id="0b841-459">O `goto` transfere o controle para uma declaração que é marcada por um rótulo.</span><span class="sxs-lookup"><span data-stu-id="0b841-459">The `goto` statement transfers control to a statement that is marked by a label.</span></span>

```antlr
goto_statement
    : 'goto' identifier ';'
    | 'goto' 'case' constant_expression ';'
    | 'goto' 'default' ';'
    ;
```

<span data-ttu-id="0b841-460">O destino de uma `goto` *identificador* instrução é a instrução rotulada com o rótulo fornecido.</span><span class="sxs-lookup"><span data-stu-id="0b841-460">The target of a `goto` *identifier* statement is the labeled statement with the given label.</span></span> <span data-ttu-id="0b841-461">Se um rótulo com o nome fornecido não existe no membro da função atual, ou se o `goto` instrução não está dentro do escopo do rótulo, ocorre um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="0b841-461">If a label with the given name does not exist in the current function member, or if the `goto` statement is not within the scope of the label, a compile-time error occurs.</span></span> <span data-ttu-id="0b841-462">Essa regra permite o uso de um `goto` instrução para transferir o controle fora de um escopo aninhado, mas não em um escopo aninhado.</span><span class="sxs-lookup"><span data-stu-id="0b841-462">This rule permits the use of a `goto` statement to transfer control out of a nested scope, but not into a nested scope.</span></span> <span data-ttu-id="0b841-463">No exemplo</span><span class="sxs-lookup"><span data-stu-id="0b841-463">In the example</span></span>
```csharp
using System;

class Test
{
    static void Main(string[] args) {
        string[,] table = {
            {"Red", "Blue", "Green"},
            {"Monday", "Wednesday", "Friday"}
        };

        foreach (string str in args) {
            int row, colm;
            for (row = 0; row <= 1; ++row)
                for (colm = 0; colm <= 2; ++colm)
                    if (str == table[row,colm])
                         goto done;

            Console.WriteLine("{0} not found", str);
            continue;
    done:
            Console.WriteLine("Found {0} at [{1}][{2}]", str, row, colm);
        }
    }
}
```
<span data-ttu-id="0b841-464">um `goto` instrução é usada para transferir o controle para fora de um escopo aninhado.</span><span class="sxs-lookup"><span data-stu-id="0b841-464">a `goto` statement is used to transfer control out of a nested scope.</span></span>

<span data-ttu-id="0b841-465">O destino de uma `goto case` instrução é a lista de instrução em imediatamente delimitador `switch` instrução ([da instrução switch](statements.md#the-switch-statement)), que contém um `case` rótulo com o valor constante fornecido.</span><span class="sxs-lookup"><span data-stu-id="0b841-465">The target of a `goto case` statement is the statement list in the immediately enclosing `switch` statement ([The switch statement](statements.md#the-switch-statement)), which contains a `case` label with the given constant value.</span></span> <span data-ttu-id="0b841-466">Se o `goto case` instrução não é incluída por um `switch` instrução, se o *constant_expression* não é implicitamente conversível ([conversões implícitas](conversions.md#implicit-conversions)) para o tipo que governam do mais próximo de circunscrição `switch` instrução, ou se delimitadora mais próxima `switch` instrução não contém um `case` rótulo com o valor constante fornecido, ocorre um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="0b841-466">If the `goto case` statement is not enclosed by a `switch` statement, if the *constant_expression* is not implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the governing type of the nearest enclosing `switch` statement, or if the nearest enclosing `switch` statement does not contain a `case` label with the given constant value, a compile-time error occurs.</span></span>

<span data-ttu-id="0b841-467">O destino de uma `goto default` instrução é a lista de instrução em imediatamente delimitador `switch` instrução ([da instrução switch](statements.md#the-switch-statement)), que contém um `default` rótulo.</span><span class="sxs-lookup"><span data-stu-id="0b841-467">The target of a `goto default` statement is the statement list in the immediately enclosing `switch` statement ([The switch statement](statements.md#the-switch-statement)), which contains a `default` label.</span></span> <span data-ttu-id="0b841-468">Se o `goto default` instrução não é incluída por um `switch` instrução, ou se delimitadora mais próxima `switch` instrução não contém um `default` rotular, ocorre um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="0b841-468">If the `goto default` statement is not enclosed by a `switch` statement, or if the nearest enclosing `switch` statement does not contain a `default` label, a compile-time error occurs.</span></span>

<span data-ttu-id="0b841-469">Um `goto` instrução não é possível sair um `finally` bloco ([a instrução try](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="0b841-469">A `goto` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="0b841-470">Quando um `goto` instrução ocorre dentro de uma `finally` bloquear, o destino da `goto` instrução deve ser dentro do mesmo `finally` bloco, ou caso contrário, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="0b841-470">When a `goto` statement occurs within a `finally` block, the target of the `goto` statement must be within the same `finally` block, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="0b841-471">Um `goto` instrução é executada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="0b841-471">A `goto` statement is executed as follows:</span></span>

*  <span data-ttu-id="0b841-472">Se o `goto` instrução sai de um ou mais `try` blocos com associados `finally` inicialmente, blocos de controle é transferido para o `finally` bloco de mais interna `try` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-472">If the `goto` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="0b841-473">Quando e se o controle atinge o ponto de extremidade de uma `finally` bloco, o controle é transferido para o `finally` bloco de circunscrição próxima `try` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-473">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="0b841-474">Esse processo é repetido até que o `finally` blocos de todos os intermediários `try` instruções foram executadas.</span><span class="sxs-lookup"><span data-stu-id="0b841-474">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="0b841-475">O controle é transferido para o destino do `goto` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-475">Control is transferred to the target of the `goto` statement.</span></span>

<span data-ttu-id="0b841-476">Como uma `goto` instrução incondicionalmente transfere o controle em outro lugar, o ponto de extremidade de um `goto` instrução nunca é acessível.</span><span class="sxs-lookup"><span data-stu-id="0b841-476">Because a `goto` statement unconditionally transfers control elsewhere, the end point of a `goto` statement is never reachable.</span></span>

### <a name="the-return-statement"></a><span data-ttu-id="0b841-477">A instrução return</span><span class="sxs-lookup"><span data-stu-id="0b841-477">The return statement</span></span>

<span data-ttu-id="0b841-478">O `return` instrução retorna o controle para o chamador atual da função na qual o `return` instrução aparece.</span><span class="sxs-lookup"><span data-stu-id="0b841-478">The `return` statement returns control to the current caller of the function in which the `return` statement appears.</span></span>

```antlr
return_statement
    : 'return' expression? ';'
    ;
```

<span data-ttu-id="0b841-479">Um `return` instrução com nenhuma expressão pode ser usada somente em um membro da função que não computa um valor, ou seja, um método com o tipo de resultado ([corpo do método](classes.md#method-body)) `void`, o `set` acessador de uma propriedade ou indexador, o `add` e `remove` acessadores de um evento, um construtor de instância, um construtor estático ou um destruidor.</span><span class="sxs-lookup"><span data-stu-id="0b841-479">A `return` statement with no expression can be used only in a function member that does not compute a value, that is, a method with the result type ([Method body](classes.md#method-body)) `void`, the `set` accessor of a property or indexer, the `add` and `remove` accessors of an event, an instance constructor, a static constructor, or a destructor.</span></span>

<span data-ttu-id="0b841-480">Um `return` instrução com uma expressão somente pode ser usada em um membro da função que calcula um valor, ou seja, um método com um tipo de resultado não nulo, o `get` acessador de uma propriedade ou indexador ou um operador definido pelo usuário.</span><span class="sxs-lookup"><span data-stu-id="0b841-480">A `return` statement with an expression can only be used in a function member that computes a value, that is, a method with a non-void result type, the `get` accessor of a property or indexer, or a user-defined operator.</span></span> <span data-ttu-id="0b841-481">Uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)) deve existir do tipo da expressão para o tipo de retorno do membro da função que contém.</span><span class="sxs-lookup"><span data-stu-id="0b841-481">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) must exist from the type of the expression to the return type of the containing function member.</span></span>

<span data-ttu-id="0b841-482">Retornar declarações também podem ser usadas no corpo das expressões de função anônima ([expressões de função anônima](expressions.md#anonymous-function-expressions)) e participar de determinar quais conversões existem para essas funções.</span><span class="sxs-lookup"><span data-stu-id="0b841-482">Return statements can also be used in the body of anonymous function expressions ([Anonymous function expressions](expressions.md#anonymous-function-expressions)), and participate in determining which conversions exist for those functions.</span></span>

<span data-ttu-id="0b841-483">É um erro de tempo de compilação para um `return` instrução para aparecer em um `finally` bloco ([a instrução try](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="0b841-483">It is a compile-time error for a `return` statement to appear in a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span>

<span data-ttu-id="0b841-484">Um `return` instrução é executada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="0b841-484">A `return` statement is executed as follows:</span></span>

*  <span data-ttu-id="0b841-485">Se o `return` declaração especifica uma expressão, a expressão é avaliada e o valor resultante é convertido para o tipo de retorno da função que contém por uma conversão implícita.</span><span class="sxs-lookup"><span data-stu-id="0b841-485">If the `return` statement specifies an expression, the expression is evaluated and the resulting value is converted to the return type of the containing function by an implicit conversion.</span></span> <span data-ttu-id="0b841-486">O resultado da conversão torna-se o valor de resultado produzido pela função.</span><span class="sxs-lookup"><span data-stu-id="0b841-486">The result of the conversion becomes the result value produced by the function.</span></span>
*  <span data-ttu-id="0b841-487">Se o `return` instrução é incluída por um ou mais `try` ou `catch` blocos com associados `finally` inicialmente, blocos de controle é transferido para o `finally` bloco de mais interna `try` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-487">If the `return` statement is enclosed by one or more `try` or `catch` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="0b841-488">Quando e se o controle atinge o ponto de extremidade de uma `finally` bloco, o controle é transferido para o `finally` bloco de circunscrição próxima `try` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-488">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="0b841-489">Esse processo é repetido até que o `finally` blocos de inclusão de todos os `try` instruções foram executadas.</span><span class="sxs-lookup"><span data-stu-id="0b841-489">This process is repeated until the `finally` blocks of all enclosing `try` statements have been executed.</span></span>
*  <span data-ttu-id="0b841-490">Se a função recipiente não é uma função assíncrona, o controle é retornado ao chamador da função que contém, juntamente com o valor do resultado, se houver.</span><span class="sxs-lookup"><span data-stu-id="0b841-490">If the containing function is not an async function, control is returned to the caller of the containing function along with the result value, if any.</span></span>
*  <span data-ttu-id="0b841-491">Se a função recipiente é uma função assíncrona, controle é retornado ao chamador atual e o valor do resultado, se houver, será registrado no task de retorno conforme descrito em ([interfaces de enumerador](classes.md#enumerator-interfaces)).</span><span class="sxs-lookup"><span data-stu-id="0b841-491">If the containing function is an async function, control is returned to the current caller, and the result value, if any, is recorded in the return task as described in ([Enumerator interfaces](classes.md#enumerator-interfaces)).</span></span>

<span data-ttu-id="0b841-492">Como uma `return` instrução incondicionalmente transfere o controle em outro lugar, o ponto de extremidade de um `return` instrução nunca é acessível.</span><span class="sxs-lookup"><span data-stu-id="0b841-492">Because a `return` statement unconditionally transfers control elsewhere, the end point of a `return` statement is never reachable.</span></span>

### <a name="the-throw-statement"></a><span data-ttu-id="0b841-493">A instrução throw</span><span class="sxs-lookup"><span data-stu-id="0b841-493">The throw statement</span></span>

<span data-ttu-id="0b841-494">O `throw` instrução gera uma exceção.</span><span class="sxs-lookup"><span data-stu-id="0b841-494">The `throw` statement throws an exception.</span></span>

```antlr
throw_statement
    : 'throw' expression? ';'
    ;
```

<span data-ttu-id="0b841-495">Um `throw` instrução com uma expressão gera o valor produzido pela avaliação da expressão.</span><span class="sxs-lookup"><span data-stu-id="0b841-495">A `throw` statement with an expression throws the value produced by evaluating the expression.</span></span> <span data-ttu-id="0b841-496">A expressão deve indicar um valor do tipo de classe `System.Exception`, de um tipo de classe que deriva de `System.Exception` ou de um tipo de parâmetro de tipo que tem `System.Exception` (ou uma subclasse disso) como sua classe base em vigor.</span><span class="sxs-lookup"><span data-stu-id="0b841-496">The expression must denote a value of the class type `System.Exception`, of a class type that derives from `System.Exception` or of a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span> <span data-ttu-id="0b841-497">Se a avaliação da expressão produz `null`, um `System.NullReferenceException` é acionada em vez disso.</span><span class="sxs-lookup"><span data-stu-id="0b841-497">If evaluation of the expression produces `null`, a `System.NullReferenceException` is thrown instead.</span></span>

<span data-ttu-id="0b841-498">Um `throw` instrução com nenhuma expressão pode ser usada somente em um `catch` bloquear, caso em que essa instrução gera novamente a exceção que está sendo manipulada no momento, por que `catch` bloco.</span><span class="sxs-lookup"><span data-stu-id="0b841-498">A `throw` statement with no expression can be used only in a `catch` block, in which case that statement re-throws the exception that is currently being handled by that `catch` block.</span></span>

<span data-ttu-id="0b841-499">Como uma `throw` instrução incondicionalmente transfere o controle em outro lugar, o ponto de extremidade de um `throw` instrução nunca é acessível.</span><span class="sxs-lookup"><span data-stu-id="0b841-499">Because a `throw` statement unconditionally transfers control elsewhere, the end point of a `throw` statement is never reachable.</span></span>

<span data-ttu-id="0b841-500">Quando uma exceção é lançada, o controle é transferido para a primeira `catch` cláusula em um delimitador `try` instrução que pode lidar com a exceção.</span><span class="sxs-lookup"><span data-stu-id="0b841-500">When an exception is thrown, control is transferred to the first `catch` clause in an enclosing `try` statement that can handle the exception.</span></span> <span data-ttu-id="0b841-501">O processo que ocorre a partir do ponto da exceção sendo lançada para o ponto de transferência de controle para um manipulador de exceção adequada é conhecido como ***propagação de exceção***.</span><span class="sxs-lookup"><span data-stu-id="0b841-501">The process that takes place from the point of the exception being thrown to the point of transferring control to a suitable exception handler is known as ***exception propagation***.</span></span> <span data-ttu-id="0b841-502">Propagação de uma exceção consiste em avaliar repetidamente as etapas seguintes até que um `catch` cláusula que corresponde a exceção é encontrada.</span><span class="sxs-lookup"><span data-stu-id="0b841-502">Propagation of an exception consists of repeatedly evaluating the following steps until a `catch` clause that matches the exception is found.</span></span> <span data-ttu-id="0b841-503">Nessa Descrição, o ***throw ponto*** inicialmente é o local em que a exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="0b841-503">In this description, the ***throw point*** is initially the location at which the exception is thrown.</span></span>

*  <span data-ttu-id="0b841-504">No membro da função atual, cada `try` instrução que inclui o ponto de lançamento é examinada.</span><span class="sxs-lookup"><span data-stu-id="0b841-504">In the current function member, each `try` statement that encloses the throw point is examined.</span></span> <span data-ttu-id="0b841-505">Instrução for each `S`, começando com o mais interno `try` instrução e terminando com mais externo `try` instrução, as etapas a seguir são avaliadas:</span><span class="sxs-lookup"><span data-stu-id="0b841-505">For each statement `S`, starting with the innermost `try` statement and ending with the outermost `try` statement, the following steps are evaluated:</span></span>

   * <span data-ttu-id="0b841-506">Se o `try` bloco do `S` circunscreve o ponto de lançamento e se S tem um ou mais `catch` cláusulas, a `catch` cláusulas são examinadas em ordem de aparência para localizar um manipulador adequado para a exceção, de acordo com as regras especificadas na Seção [a instrução try](statements.md#the-try-statement).</span><span class="sxs-lookup"><span data-stu-id="0b841-506">If the `try` block of `S` encloses the throw point and if S has one or more `catch` clauses, the `catch` clauses are examined in order of appearance to locate a suitable handler for the exception, according to the rules specified in Section [The try statement](statements.md#the-try-statement).</span></span> <span data-ttu-id="0b841-507">Se encontrar uma correspondência `catch` cláusula está localizada, a propagação de exceção é concluída, transferindo o controle para o bloco de que `catch` cláusula.</span><span class="sxs-lookup"><span data-stu-id="0b841-507">If a matching `catch` clause is located, the exception propagation is completed by transferring control to the block of that `catch` clause.</span></span>

   * <span data-ttu-id="0b841-508">Caso contrário, se o `try` bloco ou uma `catch` block de `S` circunscreve o ponto de lançamento e se `S` tem um `finally` bloco, o controle é transferido para o `finally` bloco.</span><span class="sxs-lookup"><span data-stu-id="0b841-508">Otherwise, if the `try` block or a `catch` block of `S` encloses the throw point and if `S` has a `finally` block, control is transferred to the `finally` block.</span></span> <span data-ttu-id="0b841-509">Se o `finally` bloco lança a exceção de outro, o processamento da exceção atual será encerrado.</span><span class="sxs-lookup"><span data-stu-id="0b841-509">If the `finally` block throws another exception, processing of the current exception is terminated.</span></span> <span data-ttu-id="0b841-510">Caso contrário, quando o controle atinge o ponto de extremidade do `finally` bloco, o processamento da exceção atual é continuado.</span><span class="sxs-lookup"><span data-stu-id="0b841-510">Otherwise, when control reaches the end point of the `finally` block, processing of the current exception is continued.</span></span>

*  <span data-ttu-id="0b841-511">Se um manipulador de exceção não foi localizado na invocação da função atual, a invocação de função é encerrada e ocorre um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="0b841-511">If an exception handler was not located in the current function invocation, the function invocation is terminated, and one of the following occurs:</span></span>

   * <span data-ttu-id="0b841-512">Se a função atual é não assíncronas, as etapas acima são repetidas para o chamador da função com um ponto de lançamento correspondente à instrução a partir do qual o membro da função foi invocado.</span><span class="sxs-lookup"><span data-stu-id="0b841-512">If the current function is non-async, the steps above are repeated for the caller of the function with a throw point corresponding to the statement from which the function member was invoked.</span></span>

   * <span data-ttu-id="0b841-513">Se a função atual é assíncrono e o retorno de tarefa, a exceção é registrada no task de retorno, que é colocado em um estado com falha ou cancelado, conforme descrito em [interfaces de enumerador](classes.md#enumerator-interfaces).</span><span class="sxs-lookup"><span data-stu-id="0b841-513">If the current function is async and task-returning, the exception is recorded in the return task, which is put into a faulted or cancelled state as described in [Enumerator interfaces](classes.md#enumerator-interfaces).</span></span>

   * <span data-ttu-id="0b841-514">Se a função atual é assíncrono e retorna void, o contexto de sincronização do thread atual é notificado, conforme descrito em [interfaces enumeráveis](classes.md#enumerable-interfaces).</span><span class="sxs-lookup"><span data-stu-id="0b841-514">If the current function is async and void-returning, the synchronization context of the current thread is notified as described in [Enumerable interfaces](classes.md#enumerable-interfaces).</span></span>

*  <span data-ttu-id="0b841-515">Se o processamento de exceção encerra todas as invocações de membro de função no thread atual, indicando que o thread não tem nenhum manipulador para a exceção, em seguida, o thread está em si encerrada.</span><span class="sxs-lookup"><span data-stu-id="0b841-515">If the exception processing terminates all function member invocations in the current thread, indicating that the thread has no handler for the exception, then the thread is itself terminated.</span></span> <span data-ttu-id="0b841-516">O impacto de tal rescisão é definido pela implementação.</span><span class="sxs-lookup"><span data-stu-id="0b841-516">The impact of such termination is implementation-defined.</span></span>

## <a name="the-try-statement"></a><span data-ttu-id="0b841-517">A instrução try</span><span class="sxs-lookup"><span data-stu-id="0b841-517">The try statement</span></span>

<span data-ttu-id="0b841-518">O `try` instrução fornece um mecanismo para capturar exceções que ocorrem durante a execução de um bloco.</span><span class="sxs-lookup"><span data-stu-id="0b841-518">The `try` statement provides a mechanism for catching exceptions that occur during execution of a block.</span></span> <span data-ttu-id="0b841-519">Além disso, o `try` instrução fornece a capacidade de especificar um bloco de código que sempre é executado quando o controle deixa o `try` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-519">Furthermore, the `try` statement provides the ability to specify a block of code that is always executed when control leaves the `try` statement.</span></span>

```antlr
try_statement
    : 'try' block catch_clause+
    | 'try' block finally_clause
    | 'try' block catch_clause+ finally_clause
    ;

catch_clause
    : 'catch' exception_specifier? exception_filter?  block
    ;

exception_specifier
    : '(' type identifier? ')'
    ;

exception_filter
    : 'when' '(' expression ')'
    ;

finally_clause
    : 'finally' block
    ;
```

<span data-ttu-id="0b841-520">Há três formas possíveis de `try` instruções:</span><span class="sxs-lookup"><span data-stu-id="0b841-520">There are three possible forms of `try` statements:</span></span>

*  <span data-ttu-id="0b841-521">Um `try` bloco seguido por um ou mais `catch` blocos.</span><span class="sxs-lookup"><span data-stu-id="0b841-521">A `try` block followed by one or more `catch` blocks.</span></span>
*  <span data-ttu-id="0b841-522">Um `try` bloco seguido por um `finally` bloco.</span><span class="sxs-lookup"><span data-stu-id="0b841-522">A `try` block followed by a `finally` block.</span></span>
*  <span data-ttu-id="0b841-523">Um `try` bloco seguido por um ou mais `catch` blocos seguido por um `finally` bloco.</span><span class="sxs-lookup"><span data-stu-id="0b841-523">A `try` block followed by one or more `catch` blocks followed by a `finally` block.</span></span>

<span data-ttu-id="0b841-524">Quando um `catch` cláusula Especifica um *exception_specifier*, o tipo deve ser `System.Exception`, um tipo que deriva `System.Exception` ou um tipo de parâmetro de tipo que tem `System.Exception` (ou uma subclasse disso) como seu efetiva classe base.</span><span class="sxs-lookup"><span data-stu-id="0b841-524">When a `catch` clause specifies an *exception_specifier*, the type must be `System.Exception`, a type that derives from `System.Exception` or a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span>

<span data-ttu-id="0b841-525">Quando um `catch` cláusula especifica tanto uma *exception_specifier* com um *identificador*, uma ***variável de exceção*** do determinado nome e tipo é declarado.</span><span class="sxs-lookup"><span data-stu-id="0b841-525">When a `catch` clause specifies both an *exception_specifier* with an *identifier*, an ***exception variable*** of the given name and type is declared.</span></span> <span data-ttu-id="0b841-526">A variável de exceção corresponde a uma variável local com um escopo que se estende pelo `catch` cláusula.</span><span class="sxs-lookup"><span data-stu-id="0b841-526">The exception variable corresponds to a local variable with a scope that extends over the `catch` clause.</span></span> <span data-ttu-id="0b841-527">Durante a execução do *exception_filter* e *bloco*, a variável de exceção representa a exceção que está sendo manipulada no momento.</span><span class="sxs-lookup"><span data-stu-id="0b841-527">During execution of the *exception_filter* and *block*, the exception variable represents the exception currently being handled.</span></span> <span data-ttu-id="0b841-528">Para fins de verificação de atribuição definitiva, a variável de exceção é considerada definitivamente atribuída no seu escopo inteiro.</span><span class="sxs-lookup"><span data-stu-id="0b841-528">For purposes of definite assignment checking, the exception variable is considered definitely assigned in its entire scope.</span></span>

<span data-ttu-id="0b841-529">A menos que um `catch` cláusula inclui um nome de variável de exceção, é impossível acessar o objeto de exceção no filtro e `catch` bloco.</span><span class="sxs-lookup"><span data-stu-id="0b841-529">Unless a `catch` clause includes an exception variable name, it is impossible to access the exception object in the filter and `catch` block.</span></span>

<span data-ttu-id="0b841-530">Um `catch` cláusula que não especifica um *exception_specifier* é chamado um geral `catch` cláusula.</span><span class="sxs-lookup"><span data-stu-id="0b841-530">A `catch` clause that does not specify an *exception_specifier* is called a general `catch` clause.</span></span>

<span data-ttu-id="0b841-531">Algumas linguagens de programação podem dar suporte a exceções que não são representáveis como um objeto derivado de `System.Exception`, embora essas exceções nunca pudessem ser geradas pelo código em c#.</span><span class="sxs-lookup"><span data-stu-id="0b841-531">Some programming languages may support exceptions that are not representable as an object derived from `System.Exception`, although such exceptions could never be generated by C# code.</span></span> <span data-ttu-id="0b841-532">Geral `catch` cláusula pode ser usada para capturar essas exceções.</span><span class="sxs-lookup"><span data-stu-id="0b841-532">A general `catch` clause may be used to catch such exceptions.</span></span> <span data-ttu-id="0b841-533">Assim, um general `catch` cláusula é semanticamente diferente dos que especifica o tipo `System.Exception`, em que o primeiro pode capturar exceções de outros idiomas de também.</span><span class="sxs-lookup"><span data-stu-id="0b841-533">Thus, a general `catch` clause is semantically different from one that specifies the type `System.Exception`, in that the former may also catch exceptions from other languages.</span></span>

<span data-ttu-id="0b841-534">Para localizar um manipulador para uma exceção, `catch` cláusulas são examinadas em ordem léxica.</span><span class="sxs-lookup"><span data-stu-id="0b841-534">In order to locate a handler for an exception, `catch` clauses are examined in lexical order.</span></span> <span data-ttu-id="0b841-535">Se um `catch` cláusula Especifica um tipo, mas nenhum filtro de exceção, ele é um erro de tempo de compilação para uma posterior `catch` cláusula na mesma `try` instrução para especificar um tipo que é igual ou é derivado de, digite.</span><span class="sxs-lookup"><span data-stu-id="0b841-535">If a `catch` clause specifies a type but no exception filter, it is a compile-time error for a later `catch` clause in the same `try` statement to specify a type that is the same as, or is derived from, that type.</span></span> <span data-ttu-id="0b841-536">Se um `catch` cláusula especifica nenhum tipo e nenhum filtro, ele deve ser o último `catch` cláusula para que `try` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-536">If a `catch` clause specifies no type and no filter, it must be the last `catch` clause for that `try` statement.</span></span>

<span data-ttu-id="0b841-537">Dentro de um `catch` bloco, um `throw` instrução ([a instrução throw](statements.md#the-throw-statement)) com nenhuma expressão pode ser usado para gerar novamente a exceção que foi detectada pelo `catch` bloco.</span><span class="sxs-lookup"><span data-stu-id="0b841-537">Within a `catch` block, a `throw` statement ([The throw statement](statements.md#the-throw-statement)) with no expression can be used to re-throw the exception that was caught by the `catch` block.</span></span> <span data-ttu-id="0b841-538">As atribuições para uma variável de exceção não alteram a exceção que é lançada novamente.</span><span class="sxs-lookup"><span data-stu-id="0b841-538">Assignments to an exception variable do not alter the exception that is re-thrown.</span></span>

<span data-ttu-id="0b841-539">No exemplo</span><span class="sxs-lookup"><span data-stu-id="0b841-539">In the example</span></span>
```csharp
using System;

class Test
{
    static void F() {
        try {
            G();
        }
        catch (Exception e) {
            Console.WriteLine("Exception in F: " + e.Message);
            e = new Exception("F");
            throw;                // re-throw
        }
    }

    static void G() {
        throw new Exception("G");
    }

    static void Main() {
        try {
            F();
        }
        catch (Exception e) {
            Console.WriteLine("Exception in Main: " + e.Message);
        }
    }
}
```
<span data-ttu-id="0b841-540">o método `F` capturar uma exceção, grava algumas informações de diagnóstico no console, altera a variável de exceção e gera novamente a exceção.</span><span class="sxs-lookup"><span data-stu-id="0b841-540">the method `F` catches an exception, writes some diagnostic information to the console, alters the exception variable, and re-throws the exception.</span></span> <span data-ttu-id="0b841-541">A exceção que é lançada novamente é a exceção original, portanto, a saída produzida é:</span><span class="sxs-lookup"><span data-stu-id="0b841-541">The exception that is re-thrown is the original exception, so the output produced is:</span></span>
```
Exception in F: G
Exception in Main: G
```

<span data-ttu-id="0b841-542">Se o primeiro bloco catch tinha lançada `e` em vez de relançar a exceção atual, a saída produzida seria da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="0b841-542">If the first catch block had thrown `e` instead of rethrowing the current exception, the output produced would be as follows:</span></span>
```csharp
Exception in F: G
Exception in Main: F
```

<span data-ttu-id="0b841-543">É um erro de tempo de compilação para um `break`, `continue`, ou `goto` instrução para transferir o controle de um `finally` bloco.</span><span class="sxs-lookup"><span data-stu-id="0b841-543">It is a compile-time error for a `break`, `continue`, or `goto` statement to transfer control out of a `finally` block.</span></span> <span data-ttu-id="0b841-544">Quando um `break`, `continue`, ou `goto` instrução ocorre em um `finally` bloco, o destino da instrução deve ser dentro do mesmo `finally` bloco, ou caso contrário, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="0b841-544">When a `break`, `continue`, or `goto` statement occurs in a `finally` block, the target of the statement must be within the same `finally` block, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="0b841-545">É um erro de tempo de compilação para um `return` instrução ocorra em um `finally` bloco.</span><span class="sxs-lookup"><span data-stu-id="0b841-545">It is a compile-time error for a `return` statement to occur in a `finally` block.</span></span>

<span data-ttu-id="0b841-546">Um `try` instrução é executada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="0b841-546">A `try` statement is executed as follows:</span></span>

*  <span data-ttu-id="0b841-547">O controle é transferido para o `try` bloco.</span><span class="sxs-lookup"><span data-stu-id="0b841-547">Control is transferred to the `try` block.</span></span>
*  <span data-ttu-id="0b841-548">Quando e se o controle atinge o ponto de extremidade do `try` bloco:</span><span class="sxs-lookup"><span data-stu-id="0b841-548">When and if control reaches the end point of the `try` block:</span></span>
   *  <span data-ttu-id="0b841-549">Se o `try` instrução tem uma `finally` bloco, o `finally` bloco é executado.</span><span class="sxs-lookup"><span data-stu-id="0b841-549">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
   *  <span data-ttu-id="0b841-550">O controle é transferido para o ponto de extremidade do `try` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-550">Control is transferred to the end point of the `try` statement.</span></span>

*  <span data-ttu-id="0b841-551">Se uma exceção é propagada para o `try` instrução durante a execução do `try` bloco:</span><span class="sxs-lookup"><span data-stu-id="0b841-551">If an exception is propagated to the `try` statement during execution of the `try` block:</span></span>
   *  <span data-ttu-id="0b841-552">O `catch` cláusulas, se houver, são examinadas em ordem de aparência para localizar um manipulador adequado para a exceção.</span><span class="sxs-lookup"><span data-stu-id="0b841-552">The `catch` clauses, if any, are examined in order of appearance to locate a suitable handler for the exception.</span></span> <span data-ttu-id="0b841-553">Se um `catch` cláusula não especifica um tipo ou especifica o tipo de exceção ou um tipo base do tipo de exceção:</span><span class="sxs-lookup"><span data-stu-id="0b841-553">If a `catch` clause does not specify a type, or specifies the exception type or a base type of the exception type:</span></span>
      *  <span data-ttu-id="0b841-554">Se o `catch` cláusula declara uma variável de exceção, o objeto de exceção é atribuído à variável de exceção.</span><span class="sxs-lookup"><span data-stu-id="0b841-554">If the `catch` clause declares an exception variable, the exception object is assigned to the exception variable.</span></span>
      *  <span data-ttu-id="0b841-555">Se o `catch` cláusula declara um filtro de exceção, o filtro é avaliado.</span><span class="sxs-lookup"><span data-stu-id="0b841-555">If the `catch` clause declares an exception filter, the filter is evaluated.</span></span> <span data-ttu-id="0b841-556">Se for avaliada como `false`, a cláusula catch não é uma correspondência e a pesquisa continua por meio de qualquer subsequentes `catch` cláusulas para um manipulador adequado.</span><span class="sxs-lookup"><span data-stu-id="0b841-556">If it evaluates to `false`, the catch clause is not a match, and the search continues through any subsequent `catch` clauses for a suitable handler.</span></span>
      *  <span data-ttu-id="0b841-557">Caso contrário, o `catch` cláusula é considerada uma correspondência, e o controle é transferido para a correspondência `catch` bloco.</span><span class="sxs-lookup"><span data-stu-id="0b841-557">Otherwise, the `catch` clause is considered a match, and control is transferred to the matching `catch` block.</span></span>
      *  <span data-ttu-id="0b841-558">Quando e se o controle atinge o ponto de extremidade do `catch` bloco:</span><span class="sxs-lookup"><span data-stu-id="0b841-558">When and if control reaches the end point of the `catch` block:</span></span>
         * <span data-ttu-id="0b841-559">Se o `try` instrução tem uma `finally` bloco, o `finally` bloco é executado.</span><span class="sxs-lookup"><span data-stu-id="0b841-559">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
         * <span data-ttu-id="0b841-560">O controle é transferido para o ponto de extremidade do `try` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-560">Control is transferred to the end point of the `try` statement.</span></span>
      *  <span data-ttu-id="0b841-561">Se uma exceção é propagada para o `try` instrução durante a execução do `catch` bloco:</span><span class="sxs-lookup"><span data-stu-id="0b841-561">If an exception is propagated to the `try` statement during execution of the `catch` block:</span></span>
         *  <span data-ttu-id="0b841-562">Se o `try` instrução tem uma `finally` bloco, o `finally` bloco é executado.</span><span class="sxs-lookup"><span data-stu-id="0b841-562">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
         *  <span data-ttu-id="0b841-563">A exceção é propagada para a próxima delimitação `try` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-563">The exception is propagated to the next enclosing `try` statement.</span></span>
   *  <span data-ttu-id="0b841-564">Se o `try` instrução não tem nenhum `catch` cláusulas ou se nenhum `catch` cláusula coincide com a exceção:</span><span class="sxs-lookup"><span data-stu-id="0b841-564">If the `try` statement has no `catch` clauses or if no `catch` clause matches the exception:</span></span>
      *  <span data-ttu-id="0b841-565">Se o `try` instrução tem uma `finally` bloco, o `finally` bloco é executado.</span><span class="sxs-lookup"><span data-stu-id="0b841-565">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
      *  <span data-ttu-id="0b841-566">A exceção é propagada para a próxima delimitação `try` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-566">The exception is propagated to the next enclosing `try` statement.</span></span>

<span data-ttu-id="0b841-567">As declarações de um `finally` bloco são sempre executadas quando o controle deixa uma `try` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-567">The statements of a `finally` block are always executed when control leaves a `try` statement.</span></span> <span data-ttu-id="0b841-568">Isso é verdadeiro se a transferência de controle ocorre como resultado da execução normal, como resultado da execução de um `break`, `continue`, `goto`, ou `return` instrução, ou como resultado de propagar uma exceção do `try` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-568">This is true whether the control transfer occurs as a result of normal execution, as a result of executing a `break`, `continue`, `goto`, or `return` statement, or as a result of propagating an exception out of the `try` statement.</span></span>

<span data-ttu-id="0b841-569">Se uma exceção for gerada durante a execução de um `finally` bloquear e não é capturada dentro do mesmo bloco finally, a exceção é propagada para a próxima delimitação `try` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-569">If an exception is thrown during execution of a `finally` block, and is not caught within the same finally block, the exception is propagated to the next enclosing `try` statement.</span></span> <span data-ttu-id="0b841-570">Se outra exceção estava no processo de propagação, essa exceção será perdida.</span><span class="sxs-lookup"><span data-stu-id="0b841-570">If another exception was in the process of being propagated, that exception is lost.</span></span> <span data-ttu-id="0b841-571">O processo de propagação de uma exceção é discutido mais detalhes na descrição do `throw` instrução ([a instrução throw](statements.md#the-throw-statement)).</span><span class="sxs-lookup"><span data-stu-id="0b841-571">The process of propagating an exception is discussed further in the description of the `throw` statement ([The throw statement](statements.md#the-throw-statement)).</span></span>

<span data-ttu-id="0b841-572">O `try` block de um `try` instrução é acessível se o `try` instrução é acessível.</span><span class="sxs-lookup"><span data-stu-id="0b841-572">The `try` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="0b841-573">Um `catch` block de um `try` instrução é acessível se o `try` instrução é acessível.</span><span class="sxs-lookup"><span data-stu-id="0b841-573">A `catch` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="0b841-574">O `finally` block de um `try` instrução é acessível se o `try` instrução é acessível.</span><span class="sxs-lookup"><span data-stu-id="0b841-574">The `finally` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="0b841-575">O ponto de extremidade de um `try` instrução é acessível se as duas seguintes forem verdadeiras:</span><span class="sxs-lookup"><span data-stu-id="0b841-575">The end point of a `try` statement is reachable if both of the following are true:</span></span>

*  <span data-ttu-id="0b841-576">O ponto de extremidade do `try` bloco está acessível ou ponto de final pelo menos um `catch` bloco está acessível.</span><span class="sxs-lookup"><span data-stu-id="0b841-576">The end point of the `try` block is reachable or the end point of at least one `catch` block is reachable.</span></span>
*  <span data-ttu-id="0b841-577">Se um `finally` bloco estiver presente, o ponto de extremidade do `finally` bloco está acessível.</span><span class="sxs-lookup"><span data-stu-id="0b841-577">If a `finally` block is present, the end point of the `finally` block is reachable.</span></span>

## <a name="the-checked-and-unchecked-statements"></a><span data-ttu-id="0b841-578">As instruções checked e unchecked</span><span class="sxs-lookup"><span data-stu-id="0b841-578">The checked and unchecked statements</span></span>

<span data-ttu-id="0b841-579">O `checked` e `unchecked` instruções são usadas para controlar a ***contexto de verificação de estouro*** para conversões e operações aritméticas de tipo integral.</span><span class="sxs-lookup"><span data-stu-id="0b841-579">The `checked` and `unchecked` statements are used to control the ***overflow checking context*** for integral-type arithmetic operations and conversions.</span></span>

```antlr
checked_statement
    : 'checked' block
    ;

unchecked_statement
    : 'unchecked' block
    ;
```

<span data-ttu-id="0b841-580">O `checked` instrução faz com que todas as expressões na *bloco* a ser avaliada em um contexto verificado e o `unchecked` instrução faz com que todas as expressões no *bloco* a ser avaliada em um contexto desmarcado.</span><span class="sxs-lookup"><span data-stu-id="0b841-580">The `checked` statement causes all expressions in the *block* to be evaluated in a checked context, and the `unchecked` statement causes all expressions in the *block* to be evaluated in an unchecked context.</span></span>

<span data-ttu-id="0b841-581">O `checked` e `unchecked` instruções são precisamente equivalentes para o `checked` e `unchecked` operadores ([os operadores marcados e desmarcados](expressions.md#the-checked-and-unchecked-operators)), exceto que eles operam em blocos, em vez de expressões .</span><span class="sxs-lookup"><span data-stu-id="0b841-581">The `checked` and `unchecked` statements are precisely equivalent to the `checked` and `unchecked` operators ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)), except that they operate on blocks instead of expressions.</span></span>

## <a name="the-lock-statement"></a><span data-ttu-id="0b841-582">A instrução lock</span><span class="sxs-lookup"><span data-stu-id="0b841-582">The lock statement</span></span>

<span data-ttu-id="0b841-583">O `lock` instrução obtém o bloqueio de exclusão mútua para um determinado objeto, executa uma instrução e, em seguida, libera o bloqueio.</span><span class="sxs-lookup"><span data-stu-id="0b841-583">The `lock` statement obtains the mutual-exclusion lock for a given object, executes a statement, and then releases the lock.</span></span>

```antlr
lock_statement
    : 'lock' '(' expression ')' embedded_statement
    ;
```

<span data-ttu-id="0b841-584">A expressão de uma `lock` instrução deve indicar um valor de um tipo conhecido por ser um *reference_type*.</span><span class="sxs-lookup"><span data-stu-id="0b841-584">The expression of a `lock` statement must denote a value of a type known to be a *reference_type*.</span></span> <span data-ttu-id="0b841-585">Nenhuma conversão boxing implícita ([conversões Boxing](conversions.md#boxing-conversions)) nunca é executada para a expressão de uma `lock` instrução e, portanto, ele é um erro de tempo de compilação para a expressão indicar um valor de um *value_type*.</span><span class="sxs-lookup"><span data-stu-id="0b841-585">No implicit boxing conversion ([Boxing conversions](conversions.md#boxing-conversions)) is ever performed for the expression of a `lock` statement, and thus it is a compile-time error for the expression to denote a value of a *value_type*.</span></span>

<span data-ttu-id="0b841-586">Um `lock` instrução do formulário</span><span class="sxs-lookup"><span data-stu-id="0b841-586">A `lock` statement of the form</span></span>
```csharp
lock (x) ...
```
<span data-ttu-id="0b841-587">em que `x` é uma expressão de uma *reference_type*, é precisamente equivalente a</span><span class="sxs-lookup"><span data-stu-id="0b841-587">where `x` is an expression of a *reference_type*, is precisely equivalent to</span></span>
```csharp
bool __lockWasTaken = false;
try {
    System.Threading.Monitor.Enter(x, ref __lockWasTaken);
    ...
}
finally {
    if (__lockWasTaken) System.Threading.Monitor.Exit(x);
}
```
<span data-ttu-id="0b841-588">exceto que `x` é avaliado apenas uma vez.</span><span class="sxs-lookup"><span data-stu-id="0b841-588">except that `x` is only evaluated once.</span></span>

<span data-ttu-id="0b841-589">Enquanto um bloqueio de exclusão mútua é mantido, código em execução no mesmo thread de execução pode também obter e liberar o bloqueio.</span><span class="sxs-lookup"><span data-stu-id="0b841-589">While a mutual-exclusion lock is held, code executing in the same execution thread can also obtain and release the lock.</span></span> <span data-ttu-id="0b841-590">No entanto, o código em execução em outros threads é impedido de obter o bloqueio até que o bloqueio seja liberado.</span><span class="sxs-lookup"><span data-stu-id="0b841-590">However, code executing in other threads is blocked from obtaining the lock until the lock is released.</span></span>

<span data-ttu-id="0b841-591">Bloqueio `System.Type` objetos para sincronizar o acesso a dados estáticos não é recomendado.</span><span class="sxs-lookup"><span data-stu-id="0b841-591">Locking `System.Type` objects in order to synchronize access to static data is not recommended.</span></span> <span data-ttu-id="0b841-592">Outro código pode bloquear no mesmo tipo, que pode resultar em deadlock.</span><span class="sxs-lookup"><span data-stu-id="0b841-592">Other code might lock on the same type, which can result in deadlock.</span></span> <span data-ttu-id="0b841-593">Uma abordagem melhor é sincronizar o acesso aos dados estáticos, bloqueando a um objeto estático privado.</span><span class="sxs-lookup"><span data-stu-id="0b841-593">A better approach is to synchronize access to static data by locking a private static object.</span></span> <span data-ttu-id="0b841-594">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="0b841-594">For example:</span></span>
```csharp
class Cache
{
    private static readonly object synchronizationObject = new object();

    public static void Add(object x) {
        lock (Cache.synchronizationObject) {
            ...
        }
    }

    public static void Remove(object x) {
        lock (Cache.synchronizationObject) {
            ...
        }
    }
}
```

## <a name="the-using-statement"></a><span data-ttu-id="0b841-595">A instrução using</span><span class="sxs-lookup"><span data-stu-id="0b841-595">The using statement</span></span>

<span data-ttu-id="0b841-596">O `using` instrução obtém um ou mais recursos, executa uma instrução e, em seguida, descarta o recurso.</span><span class="sxs-lookup"><span data-stu-id="0b841-596">The `using` statement obtains one or more resources, executes a statement, and then disposes of the resource.</span></span>

```antlr
using_statement
    : 'using' '(' resource_acquisition ')' embedded_statement
    ;

resource_acquisition
    : local_variable_declaration
    | expression
    ;
```

<span data-ttu-id="0b841-597">Um ***resource*** é uma classe ou struct que implementa `System.IDisposable`, que inclui um único método sem parâmetros chamado `Dispose`.</span><span class="sxs-lookup"><span data-stu-id="0b841-597">A ***resource*** is a class or struct that implements `System.IDisposable`, which includes a single parameterless method named `Dispose`.</span></span> <span data-ttu-id="0b841-598">O código que está usando um recurso pode chamar `Dispose` para indicar que o recurso não for mais necessário.</span><span class="sxs-lookup"><span data-stu-id="0b841-598">Code that is using a resource can call `Dispose` to indicate that the resource is no longer needed.</span></span> <span data-ttu-id="0b841-599">Se `Dispose` não for chamado, em seguida, descarte automático ocorre, eventualmente, como consequência de coleta de lixo.</span><span class="sxs-lookup"><span data-stu-id="0b841-599">If `Dispose` is not called, then automatic disposal eventually occurs as a consequence of garbage collection.</span></span>

<span data-ttu-id="0b841-600">Se a forma de *resource_acquisition* é *local_variable_declaration* , em seguida, o tipo dos *local_variable_declaration* deve ser um `dynamic` ou um tipo que pode ser convertido implicitamente em `System.IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="0b841-600">If the form of *resource_acquisition* is *local_variable_declaration* then the type of the *local_variable_declaration* must be either `dynamic` or a type that can be implicitly converted to `System.IDisposable`.</span></span> <span data-ttu-id="0b841-601">Se o formulário da *resource_acquisition* é *expressão* e em seguida, essa expressão deve ser implicitamente conversível para `System.IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="0b841-601">If the form of *resource_acquisition* is *expression* then this expression must be implicitly convertible to `System.IDisposable`.</span></span>

<span data-ttu-id="0b841-602">Variáveis locais declaradas em uma *resource_acquisition* são somente leitura e deve incluir um inicializador.</span><span class="sxs-lookup"><span data-stu-id="0b841-602">Local variables declared in a *resource_acquisition* are read-only, and must include an initializer.</span></span> <span data-ttu-id="0b841-603">Um erro de tempo de compilação ocorrerá se a instrução inserida tenta modificar essas variáveis locais (por meio da atribuição ou o `++` e `--` operadores), obter o endereço de-los ou passá-los como `ref` ou `out` parâmetros.</span><span class="sxs-lookup"><span data-stu-id="0b841-603">A compile-time error occurs if the embedded statement attempts to modify these local variables (via assignment or the `++` and `--` operators) , take the address of them, or pass them as `ref` or `out` parameters.</span></span>

<span data-ttu-id="0b841-604">Um `using` instrução é convertida em três partes: aquisição, uso e descarte.</span><span class="sxs-lookup"><span data-stu-id="0b841-604">A `using` statement is translated into three parts: acquisition, usage, and disposal.</span></span> <span data-ttu-id="0b841-605">Uso do recurso está contido implicitamente em um `try` instrução que inclui um `finally` cláusula.</span><span class="sxs-lookup"><span data-stu-id="0b841-605">Usage of the resource is implicitly enclosed in a `try` statement that includes a `finally` clause.</span></span> <span data-ttu-id="0b841-606">Isso `finally` cláusula descarta o recurso.</span><span class="sxs-lookup"><span data-stu-id="0b841-606">This `finally` clause disposes of the resource.</span></span> <span data-ttu-id="0b841-607">Se um `null` adquirir o recurso, em seguida, nenhuma chamada para `Dispose` é feita, e nenhuma exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="0b841-607">If a `null` resource is acquired, then no call to `Dispose` is made, and no exception is thrown.</span></span> <span data-ttu-id="0b841-608">Se o recurso é do tipo `dynamic` dinamicamente, ele será convertido por meio de uma conversão implícita de dinâmica ([conversões implícitas de dinâmicas](conversions.md#implicit-dynamic-conversions)) para `IDisposable` durante a aquisição para garantir que a conversão seja concluída com êxito antes do uso e o descarte.</span><span class="sxs-lookup"><span data-stu-id="0b841-608">If the resource is of type `dynamic` it is dynamically converted through an implicit dynamic conversion ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions)) to `IDisposable` during acquisition in order to ensure that the conversion is successful before the usage and disposal.</span></span>

<span data-ttu-id="0b841-609">Um `using` instrução do formulário</span><span class="sxs-lookup"><span data-stu-id="0b841-609">A `using` statement of the form</span></span>
```csharp
using (ResourceType resource = expression) statement
```
<span data-ttu-id="0b841-610">corresponde a um dos três possíveis expansões.</span><span class="sxs-lookup"><span data-stu-id="0b841-610">corresponds to one of three possible expansions.</span></span> <span data-ttu-id="0b841-611">Quando `ResourceType` é um tipo de valor não nulo, a expansão é</span><span class="sxs-lookup"><span data-stu-id="0b841-611">When `ResourceType` is a non-nullable value type, the expansion is</span></span>
```csharp
{
    ResourceType resource = expression;
    try {
        statement;
    }
    finally {
        ((IDisposable)resource).Dispose();
    }
}
```

<span data-ttu-id="0b841-612">Caso contrário, quando `ResourceType` é um tipo de valor anulável ou um tipo de referência diferente de `dynamic`, a expansão é</span><span class="sxs-lookup"><span data-stu-id="0b841-612">Otherwise, when `ResourceType` is a nullable value type or a reference type other than `dynamic`, the expansion is</span></span>
```csharp
{
    ResourceType resource = expression;
    try {
        statement;
    }
    finally {
        if (resource != null) ((IDisposable)resource).Dispose();
    }
}
```

<span data-ttu-id="0b841-613">Caso contrário, quando `ResourceType` é `dynamic`, a expansão é</span><span class="sxs-lookup"><span data-stu-id="0b841-613">Otherwise, when `ResourceType` is `dynamic`, the expansion is</span></span>
```csharp
{
    ResourceType resource = expression;
    IDisposable d = (IDisposable)resource;
    try {
        statement;
    }
    finally {
        if (d != null) d.Dispose();
    }
}
```

<span data-ttu-id="0b841-614">Em qualquer expansão, o `resource` variável é somente leitura na instrução inserida e o `d` variável está inacessível no e é invisível para a instrução inserida.</span><span class="sxs-lookup"><span data-stu-id="0b841-614">In either expansion, the `resource` variable is read-only in the embedded statement, and the `d` variable is inaccessible in, and invisible to, the embedded statement.</span></span>

<span data-ttu-id="0b841-615">Uma implementação tem permissão para implementar uma determinada usando-instrução de forma diferente, por exemplo, por motivos de desempenho, desde que o comportamento é consistente com a expansão acima.</span><span class="sxs-lookup"><span data-stu-id="0b841-615">An implementation is permitted to implement a given using-statement differently, e.g. for performance reasons, as long as the behavior is consistent with the above expansion.</span></span>

<span data-ttu-id="0b841-616">Um `using` instrução do formulário</span><span class="sxs-lookup"><span data-stu-id="0b841-616">A `using` statement of the form</span></span>
```csharp
using (expression) statement
```
<span data-ttu-id="0b841-617">tem as mesmas três possíveis expansões.</span><span class="sxs-lookup"><span data-stu-id="0b841-617">has the same three possible expansions.</span></span> <span data-ttu-id="0b841-618">Nesse caso `ResourceType` é implicitamente o tipo de tempo de compilação do `expression`, se ele tiver um.</span><span class="sxs-lookup"><span data-stu-id="0b841-618">In this case `ResourceType` is implicitly the compile-time type of the `expression`, if it has one.</span></span> <span data-ttu-id="0b841-619">Caso contrário, a interface `IDisposable` em si é usada como o `ResourceType`.</span><span class="sxs-lookup"><span data-stu-id="0b841-619">Otherwise the interface `IDisposable` itself is used as the `ResourceType`.</span></span> <span data-ttu-id="0b841-620">O `resource` variável está inacessível no e é invisível para a instrução inserida.</span><span class="sxs-lookup"><span data-stu-id="0b841-620">The `resource` variable is inaccessible in, and invisible to, the embedded statement.</span></span>

<span data-ttu-id="0b841-621">Quando um *resource_acquisition* assumem a forma de uma *local_variable_declaration*, é possível que você adquira vários recursos de um determinado tipo.</span><span class="sxs-lookup"><span data-stu-id="0b841-621">When a *resource_acquisition* takes the form of a *local_variable_declaration*, it is possible to acquire multiple resources of a given type.</span></span> <span data-ttu-id="0b841-622">Um `using` instrução do formulário</span><span class="sxs-lookup"><span data-stu-id="0b841-622">A `using` statement of the form</span></span>
```csharp
using (ResourceType r1 = e1, r2 = e2, ..., rN = eN) statement
```
<span data-ttu-id="0b841-623">é precisamente equivalente a uma sequência de aninhado `using` instruções:</span><span class="sxs-lookup"><span data-stu-id="0b841-623">is precisely equivalent to a sequence of nested `using` statements:</span></span>
```csharp
using (ResourceType r1 = e1)
    using (ResourceType r2 = e2)
        ...
            using (ResourceType rN = eN)
                statement
```

<span data-ttu-id="0b841-624">O exemplo a seguir cria um arquivo chamado `log.txt` e grava duas linhas de texto no arquivo.</span><span class="sxs-lookup"><span data-stu-id="0b841-624">The example below creates a file named `log.txt` and writes two lines of text to the file.</span></span> <span data-ttu-id="0b841-625">O exemplo, em seguida, abre esse mesmo arquivo para leitura e copia as linhas independentes de texto para o console.</span><span class="sxs-lookup"><span data-stu-id="0b841-625">The example then opens that same file for reading and copies the contained lines of text to the console.</span></span>
```csharp
using System;
using System.IO;

class Test
{
    static void Main() {
        using (TextWriter w = File.CreateText("log.txt")) {
            w.WriteLine("This is line one");
            w.WriteLine("This is line two");
        }

        using (TextReader r = File.OpenText("log.txt")) {
            string s;
            while ((s = r.ReadLine()) != null) {
                Console.WriteLine(s);
            }

        }
    }
}
```

<span data-ttu-id="0b841-626">Uma vez que o `TextWriter` e `TextReader` classes implementam a `IDisposable` interface, o exemplo pode usar `using` instruções para garantir que o arquivo subjacente é fechado corretamente após a gravação ou operações de leitura.</span><span class="sxs-lookup"><span data-stu-id="0b841-626">Since the `TextWriter` and `TextReader` classes implement the `IDisposable` interface, the example can use `using` statements to ensure that the underlying file is properly closed following the write or read operations.</span></span>

## <a name="the-yield-statement"></a><span data-ttu-id="0b841-627">A instrução yield</span><span class="sxs-lookup"><span data-stu-id="0b841-627">The yield statement</span></span>

<span data-ttu-id="0b841-628">O `yield` instrução é usada em um bloco iterador ([blocos](statements.md#blocks)) para produzir um valor para o objeto de enumerador ([objetos de enumerador](classes.md#enumerator-objects)) ou o objeto enumerável ([objetos enumeráveis](classes.md#enumerable-objects)) de um iterador ou para sinalizar o final da iteração.</span><span class="sxs-lookup"><span data-stu-id="0b841-628">The `yield` statement is used in an iterator block ([Blocks](statements.md#blocks)) to yield a value to the enumerator object ([Enumerator objects](classes.md#enumerator-objects)) or enumerable object ([Enumerable objects](classes.md#enumerable-objects)) of an iterator or to signal the end of the iteration.</span></span>

```antlr
yield_statement
    : 'yield' 'return' expression ';'
    | 'yield' 'break' ';'
    ;
```

<span data-ttu-id="0b841-629">`yield` não é uma palavra reservada; ele tem um significado especial somente quando usado imediatamente antes de uma `return` ou `break` palavra-chave.</span><span class="sxs-lookup"><span data-stu-id="0b841-629">`yield` is not a reserved word; it has special meaning only when used immediately before a `return` or `break` keyword.</span></span> <span data-ttu-id="0b841-630">Em outros contextos, `yield` pode ser usado como um identificador.</span><span class="sxs-lookup"><span data-stu-id="0b841-630">In other contexts, `yield` can be used as an identifier.</span></span>

<span data-ttu-id="0b841-631">Há várias restrições sobre onde um `yield` declaração pode aparecer, conforme descrito a seguir.</span><span class="sxs-lookup"><span data-stu-id="0b841-631">There are several restrictions on where a `yield` statement can appear, as described in the following.</span></span>

*  <span data-ttu-id="0b841-632">É um erro de tempo de compilação para um `yield` instrução (de qualquer forma) para aparecer fora de uma *method_body*, *operator_body* ou *accessor_body*</span><span class="sxs-lookup"><span data-stu-id="0b841-632">It is a compile-time error for a `yield` statement (of either form) to appear outside a *method_body*, *operator_body* or *accessor_body*</span></span>
*  <span data-ttu-id="0b841-633">É um erro de tempo de compilação para um `yield` instrução (de qualquer forma) para aparecer dentro de uma função anônima.</span><span class="sxs-lookup"><span data-stu-id="0b841-633">It is a compile-time error for a `yield` statement (of either form) to appear inside an anonymous function.</span></span>
*  <span data-ttu-id="0b841-634">É um erro de tempo de compilação para um `yield` instrução (de qualquer forma) apareçam na `finally` cláusula de um `try` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-634">It is a compile-time error for a `yield` statement (of either form) to appear in the `finally` clause of a `try` statement.</span></span>
*  <span data-ttu-id="0b841-635">É um erro de tempo de compilação para um `yield return` instrução para aparecer em qualquer lugar em um `try` instrução que contém qualquer `catch` cláusulas.</span><span class="sxs-lookup"><span data-stu-id="0b841-635">It is a compile-time error for a `yield return` statement to appear anywhere in a `try` statement that contains any `catch` clauses.</span></span>

<span data-ttu-id="0b841-636">O exemplo a seguir mostra alguns usos válidos e inválidos de `yield` instruções.</span><span class="sxs-lookup"><span data-stu-id="0b841-636">The following example shows some valid and invalid uses of `yield` statements.</span></span>

```csharp
delegate IEnumerable<int> D();

IEnumerator<int> GetEnumerator() {
    try {
        yield return 1;        // Ok
        yield break;           // Ok
    }
    finally {
        yield return 2;        // Error, yield in finally
        yield break;           // Error, yield in finally
    }

    try {
        yield return 3;        // Error, yield return in try...catch
        yield break;           // Ok
    }
    catch {
        yield return 4;        // Error, yield return in try...catch
        yield break;           // Ok
    }

    D d = delegate { 
        yield return 5;        // Error, yield in an anonymous function
    }; 
}

int MyMethod() {
    yield return 1;            // Error, wrong return type for an iterator block
}
```

<span data-ttu-id="0b841-637">Uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)) deve existir do tipo da expressão na `yield return` instrução para o tipo de yield ([geram tipo](classes.md#yield-type)) do iterador.</span><span class="sxs-lookup"><span data-stu-id="0b841-637">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) must exist from the type of the expression in the `yield return` statement to the yield type ([Yield type](classes.md#yield-type)) of the iterator.</span></span>

<span data-ttu-id="0b841-638">Um `yield return` instrução é executada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="0b841-638">A `yield return` statement is executed as follows:</span></span>

*  <span data-ttu-id="0b841-639">A expressão fornecida na instrução é avaliada implicitamente convertida no tipo de rendimento e atribuída ao `Current` propriedade do objeto enumerador.</span><span class="sxs-lookup"><span data-stu-id="0b841-639">The expression given in the statement is evaluated, implicitly converted to the yield type, and assigned to the `Current` property of the enumerator object.</span></span>
*  <span data-ttu-id="0b841-640">Execução do bloco iterador é suspenso.</span><span class="sxs-lookup"><span data-stu-id="0b841-640">Execution of the iterator block is suspended.</span></span> <span data-ttu-id="0b841-641">Se o `yield return` instrução está dentro de um ou mais `try` bloqueia associado `finally` blocos não são executados no momento.</span><span class="sxs-lookup"><span data-stu-id="0b841-641">If the `yield return` statement is within one or more `try` blocks, the associated `finally` blocks are not executed at this time.</span></span>
*  <span data-ttu-id="0b841-642">O `MoveNext` método do objeto enumerador retornar `true` para seu chamador, que indica que o objeto de enumerador avançou com êxito para o próximo item.</span><span class="sxs-lookup"><span data-stu-id="0b841-642">The `MoveNext` method of the enumerator object returns `true` to its caller, indicating that the enumerator object successfully advanced to the next item.</span></span>

<span data-ttu-id="0b841-643">A próxima chamada para o objeto de enumerador `MoveNext` método retoma a execução do bloco de iterador de onde ele foi suspenso pela última vez.</span><span class="sxs-lookup"><span data-stu-id="0b841-643">The next call to the enumerator object's `MoveNext` method resumes execution of the iterator block from where it was last suspended.</span></span>

<span data-ttu-id="0b841-644">Um `yield break` instrução é executada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="0b841-644">A `yield break` statement is executed as follows:</span></span>

*  <span data-ttu-id="0b841-645">Se o `yield break` instrução é incluída por um ou mais `try` blocos com associados `finally` inicialmente, blocos de controle é transferido para o `finally` bloco de mais interna `try` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-645">If the `yield break` statement is enclosed by one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="0b841-646">Quando e se o controle atinge o ponto de extremidade de uma `finally` bloco, o controle é transferido para o `finally` bloco de circunscrição próxima `try` instrução.</span><span class="sxs-lookup"><span data-stu-id="0b841-646">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="0b841-647">Esse processo é repetido até que o `finally` blocos de inclusão de todos os `try` instruções foram executadas.</span><span class="sxs-lookup"><span data-stu-id="0b841-647">This process is repeated until the `finally` blocks of all enclosing `try` statements have been executed.</span></span>
*  <span data-ttu-id="0b841-648">Controle é retornado ao chamador do bloco de iterador.</span><span class="sxs-lookup"><span data-stu-id="0b841-648">Control is returned to the caller of the iterator block.</span></span> <span data-ttu-id="0b841-649">Isso é o `MoveNext` método ou `Dispose` método do objeto enumerador.</span><span class="sxs-lookup"><span data-stu-id="0b841-649">This is either the `MoveNext` method or `Dispose` method of the enumerator object.</span></span>

<span data-ttu-id="0b841-650">Como uma `yield break` instrução incondicionalmente transfere o controle em outro lugar, o ponto de extremidade de um `yield break` instrução nunca é acessível.</span><span class="sxs-lookup"><span data-stu-id="0b841-650">Because a `yield break` statement unconditionally transfers control elsewhere, the end point of a `yield break` statement is never reachable.</span></span>
