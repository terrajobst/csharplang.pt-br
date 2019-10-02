---
ms.openlocfilehash: 7248a91976c479dc1b6b64b799639635617a7bec
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704040"
---
# <a name="statements"></a><span data-ttu-id="3f94b-101">Instruções</span><span class="sxs-lookup"><span data-stu-id="3f94b-101">Statements</span></span>

<span data-ttu-id="3f94b-102">C#fornece uma variedade de instruções.</span><span class="sxs-lookup"><span data-stu-id="3f94b-102">C# provides a variety of statements.</span></span> <span data-ttu-id="3f94b-103">A maioria dessas instruções será familiar para os desenvolvedores que programaram em C e C++.</span><span class="sxs-lookup"><span data-stu-id="3f94b-103">Most of these statements will be familiar to developers who have programmed in C and C++.</span></span>

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

<span data-ttu-id="3f94b-104">O *embedded_statement* não terminal é usado para instruções que aparecem dentro de outras instruções.</span><span class="sxs-lookup"><span data-stu-id="3f94b-104">The *embedded_statement* nonterminal is used for statements that appear within other statements.</span></span> <span data-ttu-id="3f94b-105">O uso de *embedded_statement* em vez de *instrução* exclui o uso de instruções de declaração e instruções rotuladas nesses contextos.</span><span class="sxs-lookup"><span data-stu-id="3f94b-105">The use of *embedded_statement* rather than *statement* excludes the use of declaration statements and labeled statements in these contexts.</span></span> <span data-ttu-id="3f94b-106">O exemplo</span><span class="sxs-lookup"><span data-stu-id="3f94b-106">The example</span></span>
```csharp
void F(bool b) {
    if (b)
        int i = 44;
}
```
<span data-ttu-id="3f94b-107">resulta em um erro de tempo de compilação porque uma instrução `if` requer um *embedded_statement* em vez de uma *instrução* para sua ramificação If.</span><span class="sxs-lookup"><span data-stu-id="3f94b-107">results in a compile-time error because an `if` statement requires an *embedded_statement* rather than a *statement* for its if branch.</span></span> <span data-ttu-id="3f94b-108">Se esse código fosse permitido, a variável `i` seria declarada, mas nunca poderia ser usada.</span><span class="sxs-lookup"><span data-stu-id="3f94b-108">If this code were permitted, then the variable `i` would be declared, but it could never be used.</span></span> <span data-ttu-id="3f94b-109">No entanto, observe que, `i`colocando a declaração de em um bloco, o exemplo é válido.</span><span class="sxs-lookup"><span data-stu-id="3f94b-109">Note, however, that by placing `i`'s declaration in a block, the example is valid.</span></span>

## <a name="end-points-and-reachability"></a><span data-ttu-id="3f94b-110">Pontos de extremidade e acessibilidade</span><span class="sxs-lookup"><span data-stu-id="3f94b-110">End points and reachability</span></span>

<span data-ttu-id="3f94b-111">Cada instrução tem um ***ponto de extremidade***.</span><span class="sxs-lookup"><span data-stu-id="3f94b-111">Every statement has an ***end point***.</span></span> <span data-ttu-id="3f94b-112">Em termos intuitivos, o ponto de extremidade de uma instrução é o local que segue imediatamente a instrução.</span><span class="sxs-lookup"><span data-stu-id="3f94b-112">In intuitive terms, the end point of a statement is the location that immediately follows the statement.</span></span> <span data-ttu-id="3f94b-113">As regras de execução para instruções compostas (instruções que contêm instruções inseridas) especificam a ação que é executada quando o controle atinge o ponto de extremidade de uma instrução inserida.</span><span class="sxs-lookup"><span data-stu-id="3f94b-113">The execution rules for composite statements (statements that contain embedded statements) specify the action that is taken when control reaches the end point of an embedded statement.</span></span> <span data-ttu-id="3f94b-114">Por exemplo, quando o controle atinge o ponto de extremidade de uma instrução em um bloco, o controle é transferido para a próxima instrução no bloco.</span><span class="sxs-lookup"><span data-stu-id="3f94b-114">For example, when control reaches the end point of a statement in a block, control is transferred to the next statement in the block.</span></span>

<span data-ttu-id="3f94b-115">Se uma instrução puder ser alcançada pela execução, a instrução será considerada ***acessível***.</span><span class="sxs-lookup"><span data-stu-id="3f94b-115">If a statement can possibly be reached by execution, the statement is said to be ***reachable***.</span></span> <span data-ttu-id="3f94b-116">Por outro lado, se não houver nenhuma possibilidade de que uma instrução seja executada, a instrução será considerada ***inacessível***.</span><span class="sxs-lookup"><span data-stu-id="3f94b-116">Conversely, if there is no possibility that a statement will be executed, the statement is said to be ***unreachable***.</span></span>

<span data-ttu-id="3f94b-117">No exemplo</span><span class="sxs-lookup"><span data-stu-id="3f94b-117">In the example</span></span>
```csharp
void F() {
    Console.WriteLine("reachable");
    goto Label;
    Console.WriteLine("unreachable");
    Label:
    Console.WriteLine("reachable");
}
```
<span data-ttu-id="3f94b-118">a segunda invocação de `Console.WriteLine` está inacessível porque não há possibilidade de que a instrução seja executada.</span><span class="sxs-lookup"><span data-stu-id="3f94b-118">the second invocation of `Console.WriteLine` is unreachable because there is no possibility that the statement will be executed.</span></span>

<span data-ttu-id="3f94b-119">Um aviso será relatado se o compilador determinar que uma instrução está inacessível.</span><span class="sxs-lookup"><span data-stu-id="3f94b-119">A warning is reported if the compiler determines that a statement is unreachable.</span></span> <span data-ttu-id="3f94b-120">Especificamente, não é um erro para que uma instrução fique inacessível.</span><span class="sxs-lookup"><span data-stu-id="3f94b-120">It is specifically not an error for a statement to be unreachable.</span></span>

<span data-ttu-id="3f94b-121">Para determinar se uma determinada instrução ou ponto de extremidade pode ser acessado, o compilador executa a análise de fluxo de acordo com as regras de acessibilidade definidas para cada instrução.</span><span class="sxs-lookup"><span data-stu-id="3f94b-121">To determine whether a particular statement or end point is reachable, the compiler performs flow analysis according to the reachability rules defined for each statement.</span></span> <span data-ttu-id="3f94b-122">A análise de fluxo leva em conta os valores de expressões constantes ([expressões constantes](expressions.md#constant-expressions)) que controlam o comportamento de instruções, mas os valores possíveis de expressões não constantes não são considerados.</span><span class="sxs-lookup"><span data-stu-id="3f94b-122">The flow analysis takes into account the values of constant expressions ([Constant expressions](expressions.md#constant-expressions)) that control the behavior of statements, but the possible values of non-constant expressions are not considered.</span></span> <span data-ttu-id="3f94b-123">Em outras palavras, para fins de análise de fluxo de controle, uma expressão não constante de um determinado tipo é considerada com qualquer valor possível desse tipo.</span><span class="sxs-lookup"><span data-stu-id="3f94b-123">In other words, for purposes of control flow analysis, a non-constant expression of a given type is considered to have any possible value of that type.</span></span>

<span data-ttu-id="3f94b-124">No exemplo</span><span class="sxs-lookup"><span data-stu-id="3f94b-124">In the example</span></span>
```csharp
void F() {
    const int i = 1;
    if (i == 2) Console.WriteLine("unreachable");
}
```
<span data-ttu-id="3f94b-125">a expressão booliana da `if` instrução é uma expressão constante porque ambos os operandos `==` do operador são constantes.</span><span class="sxs-lookup"><span data-stu-id="3f94b-125">the boolean expression of the `if` statement is a constant expression because both operands of the `==` operator are constants.</span></span> <span data-ttu-id="3f94b-126">Como a expressão constante é avaliada em tempo de compilação, produzindo `false`o valor `Console.WriteLine` , a invocação é considerada inacessível.</span><span class="sxs-lookup"><span data-stu-id="3f94b-126">As the constant expression is evaluated at compile-time, producing the value `false`, the `Console.WriteLine` invocation is considered unreachable.</span></span> <span data-ttu-id="3f94b-127">No entanto `i` , se for alterado para ser uma variável local</span><span class="sxs-lookup"><span data-stu-id="3f94b-127">However, if `i` is changed to be a local variable</span></span>
```csharp
void F() {
    int i = 1;
    if (i == 2) Console.WriteLine("reachable");
}
```
<span data-ttu-id="3f94b-128">a `Console.WriteLine` invocação é considerada acessível, apesar de, na realidade, nunca será executada.</span><span class="sxs-lookup"><span data-stu-id="3f94b-128">the `Console.WriteLine` invocation is considered reachable, even though, in reality, it will never be executed.</span></span>

<span data-ttu-id="3f94b-129">O *bloco* de um membro de função é sempre considerado acessível.</span><span class="sxs-lookup"><span data-stu-id="3f94b-129">The *block* of a function member is always considered reachable.</span></span> <span data-ttu-id="3f94b-130">Ao avaliar sucessivamente as regras de acessibilidade de cada instrução em um bloco, a acessibilidade de qualquer instrução específica pode ser determinada.</span><span class="sxs-lookup"><span data-stu-id="3f94b-130">By successively evaluating the reachability rules of each statement in a block, the reachability of any given statement can be determined.</span></span>

<span data-ttu-id="3f94b-131">No exemplo</span><span class="sxs-lookup"><span data-stu-id="3f94b-131">In the example</span></span>
```csharp
void F(int x) {
    Console.WriteLine("start");
    if (x < 0) Console.WriteLine("negative");
}
```
<span data-ttu-id="3f94b-132">a acessibilidade do segundo `Console.WriteLine` é determinada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="3f94b-132">the reachability of the second `Console.WriteLine` is determined as follows:</span></span>

*  <span data-ttu-id="3f94b-133">A primeira `Console.WriteLine` instrução `F` de expressão pode ser acessada porque o bloco do método está acessível.</span><span class="sxs-lookup"><span data-stu-id="3f94b-133">The first `Console.WriteLine` expression statement is reachable because the block of the `F` method is reachable.</span></span>
*  <span data-ttu-id="3f94b-134">O ponto de extremidade da primeira `Console.WriteLine` instrução de expressão pode ser acessado porque essa instrução pode ser acessada.</span><span class="sxs-lookup"><span data-stu-id="3f94b-134">The end point of the first `Console.WriteLine` expression statement is reachable because that statement is reachable.</span></span>
*  <span data-ttu-id="3f94b-135">A `if` instrução pode ser acessada porque o ponto final `Console.WriteLine` da primeira instrução de expressão está acessível.</span><span class="sxs-lookup"><span data-stu-id="3f94b-135">The `if` statement is reachable because the end point of the first `Console.WriteLine` expression statement is reachable.</span></span>
*  <span data-ttu-id="3f94b-136">A segunda `Console.WriteLine` instrução `if` de expressão está acessível porque a expressão booliana da instrução não tem o valor `false`constante.</span><span class="sxs-lookup"><span data-stu-id="3f94b-136">The second `Console.WriteLine` expression statement is reachable because the boolean expression of the `if` statement does not have the constant value `false`.</span></span>

<span data-ttu-id="3f94b-137">Há duas situações em que é um erro de tempo de compilação para o ponto de extremidade de uma instrução ser acessível:</span><span class="sxs-lookup"><span data-stu-id="3f94b-137">There are two situations in which it is a compile-time error for the end point of a statement to be reachable:</span></span>

*  <span data-ttu-id="3f94b-138">Como a `switch` instrução não permite que uma seção de comutador "passe" para a próxima seção switch, é um erro de tempo de compilação para o ponto final da lista de instruções de uma seção de switch ser acessível.</span><span class="sxs-lookup"><span data-stu-id="3f94b-138">Because the `switch` statement does not permit a switch section to "fall through" to the next switch section, it is a compile-time error for the end point of the statement list of a switch section to be reachable.</span></span> <span data-ttu-id="3f94b-139">Se esse erro ocorrer, normalmente é uma indicação de que uma `break` instrução está ausente.</span><span class="sxs-lookup"><span data-stu-id="3f94b-139">If this error occurs, it is typically an indication that a `break` statement is missing.</span></span>
*  <span data-ttu-id="3f94b-140">É um erro de tempo de compilação para o ponto de extremidade do bloco de um membro de função que computa um valor a ser acessado.</span><span class="sxs-lookup"><span data-stu-id="3f94b-140">It is a compile-time error for the end point of the block of a function member that computes a value to be reachable.</span></span> <span data-ttu-id="3f94b-141">Se esse erro ocorrer, normalmente é uma indicação de que uma `return` instrução está ausente.</span><span class="sxs-lookup"><span data-stu-id="3f94b-141">If this error occurs, it typically is an indication that a `return` statement is missing.</span></span>

## <a name="blocks"></a><span data-ttu-id="3f94b-142">Blocos</span><span class="sxs-lookup"><span data-stu-id="3f94b-142">Blocks</span></span>

<span data-ttu-id="3f94b-143">Um *bloco* permite a produção de várias instruções em contextos nos quais uma única instrução é permitida.</span><span class="sxs-lookup"><span data-stu-id="3f94b-143">A *block* permits multiple statements to be written in contexts where a single statement is allowed.</span></span>

```antlr
block
    : '{' statement_list? '}'
    ;
```

<span data-ttu-id="3f94b-144">Um *bloco* consiste em um *statement_list* opcional ([listas de instruções](statements.md#statement-lists)), entre chaves.</span><span class="sxs-lookup"><span data-stu-id="3f94b-144">A *block* consists of an optional *statement_list* ([Statement lists](statements.md#statement-lists)), enclosed in braces.</span></span> <span data-ttu-id="3f94b-145">Se a lista de instruções for omitida, o bloco será considerado vazio.</span><span class="sxs-lookup"><span data-stu-id="3f94b-145">If the statement list is omitted, the block is said to be empty.</span></span>

<span data-ttu-id="3f94b-146">Um bloco pode conter instruções de declaração ([instruções de declaração](statements.md#declaration-statements)).</span><span class="sxs-lookup"><span data-stu-id="3f94b-146">A block may contain declaration statements ([Declaration statements](statements.md#declaration-statements)).</span></span> <span data-ttu-id="3f94b-147">O escopo de uma variável local ou constante declarada em um bloco é o bloco.</span><span class="sxs-lookup"><span data-stu-id="3f94b-147">The scope of a local variable or constant declared in a block is the block.</span></span>

<span data-ttu-id="3f94b-148">Um bloco é executado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="3f94b-148">A block is executed as follows:</span></span>

*  <span data-ttu-id="3f94b-149">Se o bloco estiver vazio, o controle será transferido para o ponto de extremidade do bloco.</span><span class="sxs-lookup"><span data-stu-id="3f94b-149">If the block is empty, control is transferred to the end point of the block.</span></span>
*  <span data-ttu-id="3f94b-150">Se o bloco não estiver vazio, o controle será transferido para a lista de instruções.</span><span class="sxs-lookup"><span data-stu-id="3f94b-150">If the block is not empty, control is transferred to the statement list.</span></span> <span data-ttu-id="3f94b-151">Quando e se o controle atingir o ponto de extremidade da lista de instruções, o controle será transferido para o ponto de extremidade do bloco.</span><span class="sxs-lookup"><span data-stu-id="3f94b-151">When and if control reaches the end point of the statement list, control is transferred to the end point of the block.</span></span>

<span data-ttu-id="3f94b-152">A lista de instruções de um bloco pode ser acessada se o próprio bloco estiver acessível.</span><span class="sxs-lookup"><span data-stu-id="3f94b-152">The statement list of a block is reachable if the block itself is reachable.</span></span>

<span data-ttu-id="3f94b-153">O ponto de extremidade de um bloco pode ser acessado se o bloco estiver vazio ou se o ponto final da lista de instruções estiver acessível.</span><span class="sxs-lookup"><span data-stu-id="3f94b-153">The end point of a block is reachable if the block is empty or if the end point of the statement list is reachable.</span></span>

<span data-ttu-id="3f94b-154">Um *bloco* que contém uma ou mais `yield` instruções ([a instrução yield](statements.md#the-yield-statement)) é chamado de bloco de iterador.</span><span class="sxs-lookup"><span data-stu-id="3f94b-154">A *block* that contains one or more `yield` statements ([The yield statement](statements.md#the-yield-statement)) is called an iterator block.</span></span> <span data-ttu-id="3f94b-155">Os blocos de iteradores são usados para implementar membros de função como iteradores ([iteradores](classes.md#iterators)).</span><span class="sxs-lookup"><span data-stu-id="3f94b-155">Iterator blocks are used to implement function members as iterators ([Iterators](classes.md#iterators)).</span></span> <span data-ttu-id="3f94b-156">Algumas restrições adicionais se aplicam a blocos de iteradores:</span><span class="sxs-lookup"><span data-stu-id="3f94b-156">Some additional restrictions apply to iterator blocks:</span></span>

*  <span data-ttu-id="3f94b-157">É um erro de tempo de compilação para que `return` uma instrução apareça em um bloco de iterador `yield return` (mas as instruções são permitidas).</span><span class="sxs-lookup"><span data-stu-id="3f94b-157">It is a compile-time error for a `return` statement to appear in an iterator block (but `yield return` statements are permitted).</span></span>
*  <span data-ttu-id="3f94b-158">É um erro de tempo de compilação para um bloco de iterador conter um contexto não seguro ([contextos não seguros](unsafe-code.md#unsafe-contexts)).</span><span class="sxs-lookup"><span data-stu-id="3f94b-158">It is a compile-time error for an iterator block to contain an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span> <span data-ttu-id="3f94b-159">Um bloco do iterador sempre define um contexto seguro, mesmo quando sua declaração está aninhada em um contexto sem segurança.</span><span class="sxs-lookup"><span data-stu-id="3f94b-159">An iterator block always defines a safe context, even when its declaration is nested in an unsafe context.</span></span>

### <a name="statement-lists"></a><span data-ttu-id="3f94b-160">Listas de instruções</span><span class="sxs-lookup"><span data-stu-id="3f94b-160">Statement lists</span></span>

<span data-ttu-id="3f94b-161">Uma ***lista*** de instruções consiste em uma ou mais instruções escritas em sequência.</span><span class="sxs-lookup"><span data-stu-id="3f94b-161">A ***statement list*** consists of one or more statements written in sequence.</span></span> <span data-ttu-id="3f94b-162">As listas de instruções *ocorrem em blocos*s ([blocos](statements.md#blocks)) e em *switch_block*s ([a instrução switch](statements.md#the-switch-statement)).</span><span class="sxs-lookup"><span data-stu-id="3f94b-162">Statement lists occur in *block*s ([Blocks](statements.md#blocks)) and in *switch_block*s ([The switch statement](statements.md#the-switch-statement)).</span></span>

```antlr
statement_list
    : statement+
    ;
```

<span data-ttu-id="3f94b-163">Uma lista de instruções é executada transferindo o controle para a primeira instrução.</span><span class="sxs-lookup"><span data-stu-id="3f94b-163">A statement list is executed by transferring control to the first statement.</span></span> <span data-ttu-id="3f94b-164">Quando e se o controle atingir o ponto de extremidade de uma instrução, o controle será transferido para a próxima instrução.</span><span class="sxs-lookup"><span data-stu-id="3f94b-164">When and if control reaches the end point of a statement, control is transferred to the next statement.</span></span> <span data-ttu-id="3f94b-165">Quando e se o controle atingir o ponto de extremidade da última instrução, o controle será transferido para o ponto de extremidade da lista de instruções.</span><span class="sxs-lookup"><span data-stu-id="3f94b-165">When and if control reaches the end point of the last statement, control is transferred to the end point of the statement list.</span></span>

<span data-ttu-id="3f94b-166">Uma instrução em uma lista de instruções pode ser acessada se pelo menos uma das seguintes opções for verdadeira:</span><span class="sxs-lookup"><span data-stu-id="3f94b-166">A statement in a statement list is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="3f94b-167">A instrução é a primeira instrução e a própria lista de instruções pode ser acessada.</span><span class="sxs-lookup"><span data-stu-id="3f94b-167">The statement is the first statement and the statement list itself is reachable.</span></span>
*  <span data-ttu-id="3f94b-168">O ponto de extremidade da instrução anterior pode ser acessado.</span><span class="sxs-lookup"><span data-stu-id="3f94b-168">The end point of the preceding statement is reachable.</span></span>
*  <span data-ttu-id="3f94b-169">A instrução é uma instrução rotulada e o rótulo é referenciado por uma `goto` instrução alcançável.</span><span class="sxs-lookup"><span data-stu-id="3f94b-169">The statement is a labeled statement and the label is referenced by a reachable `goto` statement.</span></span>

<span data-ttu-id="3f94b-170">O ponto de extremidade de uma lista de instruções pode ser acessado se o ponto final da última instrução na lista estiver acessível.</span><span class="sxs-lookup"><span data-stu-id="3f94b-170">The end point of a statement list is reachable if the end point of the last statement in the list is reachable.</span></span>

## <a name="the-empty-statement"></a><span data-ttu-id="3f94b-171">A instrução vazia</span><span class="sxs-lookup"><span data-stu-id="3f94b-171">The empty statement</span></span>

<span data-ttu-id="3f94b-172">Um *empty_statement* não faz nada.</span><span class="sxs-lookup"><span data-stu-id="3f94b-172">An *empty_statement* does nothing.</span></span>

```antlr
empty_statement
    : ';'
    ;
```

<span data-ttu-id="3f94b-173">Uma instrução vazia é usada quando não há nenhuma operação a ser executada em um contexto em que uma instrução é necessária.</span><span class="sxs-lookup"><span data-stu-id="3f94b-173">An empty statement is used when there are no operations to perform in a context where a statement is required.</span></span>

<span data-ttu-id="3f94b-174">A execução de uma instrução vazia simplesmente transfere o controle para o ponto de extremidade da instrução.</span><span class="sxs-lookup"><span data-stu-id="3f94b-174">Execution of an empty statement simply transfers control to the end point of the statement.</span></span> <span data-ttu-id="3f94b-175">Assim, o ponto de extremidade de uma instrução vazia estará acessível se a instrução vazia puder ser acessada.</span><span class="sxs-lookup"><span data-stu-id="3f94b-175">Thus, the end point of an empty statement is reachable if the empty statement is reachable.</span></span>

<span data-ttu-id="3f94b-176">Uma instrução vazia pode ser usada ao gravar uma `while` instrução com um corpo nulo:</span><span class="sxs-lookup"><span data-stu-id="3f94b-176">An empty statement can be used when writing a `while` statement with a null body:</span></span>
```csharp
bool ProcessMessage() {...}

void ProcessMessages() {
    while (ProcessMessage())
        ;
}
```

<span data-ttu-id="3f94b-177">Além disso, uma instrução vazia pode ser usada para declarar um rótulo logo antes do fechamento`}`"" de um bloco:</span><span class="sxs-lookup"><span data-stu-id="3f94b-177">Also, an empty statement can be used to declare a label just before the closing "`}`" of a block:</span></span>
```csharp
void F() {
    ...
    if (done) goto exit;
    ...
    exit: ;
}
```

## <a name="labeled-statements"></a><span data-ttu-id="3f94b-178">Instruções rotuladas</span><span class="sxs-lookup"><span data-stu-id="3f94b-178">Labeled statements</span></span>

<span data-ttu-id="3f94b-179">Um *labeled_statement* permite que uma instrução seja prefixada por um rótulo.</span><span class="sxs-lookup"><span data-stu-id="3f94b-179">A *labeled_statement* permits a statement to be prefixed by a label.</span></span> <span data-ttu-id="3f94b-180">Instruções rotuladas são permitidas em blocos, mas não são permitidas como instruções inseridas.</span><span class="sxs-lookup"><span data-stu-id="3f94b-180">Labeled statements are permitted in blocks, but are not permitted as embedded statements.</span></span>

```antlr
labeled_statement
    : identifier ':' statement
    ;
```

<span data-ttu-id="3f94b-181">Uma instrução rotulada declara um rótulo com o nome fornecido pelo *identificador*.</span><span class="sxs-lookup"><span data-stu-id="3f94b-181">A labeled statement declares a label with the name given by the *identifier*.</span></span> <span data-ttu-id="3f94b-182">O escopo de um rótulo é o bloco inteiro no qual o rótulo é declarado, incluindo qualquer bloco aninhado.</span><span class="sxs-lookup"><span data-stu-id="3f94b-182">The scope of a label is the whole block in which the label is declared, including any nested blocks.</span></span> <span data-ttu-id="3f94b-183">É um erro de tempo de compilação para dois rótulos com o mesmo nome para ter escopos sobrepostos.</span><span class="sxs-lookup"><span data-stu-id="3f94b-183">It is a compile-time error for two labels with the same name to have overlapping scopes.</span></span>

<span data-ttu-id="3f94b-184">Um rótulo pode ser referenciado `goto` de instruções ([a instrução goto](statements.md#the-goto-statement)) dentro do escopo do rótulo.</span><span class="sxs-lookup"><span data-stu-id="3f94b-184">A label can be referenced from `goto` statements ([The goto statement](statements.md#the-goto-statement)) within the scope of the label.</span></span> <span data-ttu-id="3f94b-185">Isso significa que `goto` as instruções podem transferir o controle dentro de blocos e fora de blocos, mas nunca em blocos.</span><span class="sxs-lookup"><span data-stu-id="3f94b-185">This means that `goto` statements can transfer control within blocks and out of blocks, but never into blocks.</span></span>

<span data-ttu-id="3f94b-186">Os rótulos têm seu próprio espaço de declaração e não interferem em outros identificadores.</span><span class="sxs-lookup"><span data-stu-id="3f94b-186">Labels have their own declaration space and do not interfere with other identifiers.</span></span> <span data-ttu-id="3f94b-187">O exemplo</span><span class="sxs-lookup"><span data-stu-id="3f94b-187">The example</span></span>
```csharp
int F(int x) {
    if (x >= 0) goto x;
    x = -x;
    x: return x;
}
```
<span data-ttu-id="3f94b-188">é válido e usa o nome `x` como um parâmetro e um rótulo.</span><span class="sxs-lookup"><span data-stu-id="3f94b-188">is valid and uses the name `x` as both a parameter and a label.</span></span>

<span data-ttu-id="3f94b-189">A execução de uma instrução rotulada corresponde exatamente à execução da instrução após o rótulo.</span><span class="sxs-lookup"><span data-stu-id="3f94b-189">Execution of a labeled statement corresponds exactly to execution of the statement following the label.</span></span>

<span data-ttu-id="3f94b-190">Além da acessibilidade fornecida pelo fluxo de controle normal, uma instrução rotulada pode ser acessada se o rótulo for referenciado por uma `goto` instrução alcançável.</span><span class="sxs-lookup"><span data-stu-id="3f94b-190">In addition to the reachability provided by normal flow of control, a labeled statement is reachable if the label is referenced by a reachable `goto` statement.</span></span> <span data-ttu-id="3f94b-191">Exception Se uma `goto` instrução estiver dentro de `try` um que inclui `finally` um bloco, e a instrução rotulada estiver fora `try` `finally` do, e o ponto final do bloco estiver inacessível, a instrução rotulada não será acessível de Essa `goto` instrução.)</span><span class="sxs-lookup"><span data-stu-id="3f94b-191">(Exception: If a `goto` statement is inside a `try` that includes a `finally` block, and the labeled statement is outside the `try`, and the end point of the `finally` block is unreachable, then the labeled statement is not reachable from that `goto` statement.)</span></span>

## <a name="declaration-statements"></a><span data-ttu-id="3f94b-192">Instruções de declaração</span><span class="sxs-lookup"><span data-stu-id="3f94b-192">Declaration statements</span></span>

<span data-ttu-id="3f94b-193">Um *declaration_statement* declara uma variável ou constante local.</span><span class="sxs-lookup"><span data-stu-id="3f94b-193">A *declaration_statement* declares a local variable or constant.</span></span> <span data-ttu-id="3f94b-194">Instruções de declaração são permitidas em blocos, mas não são permitidas como instruções inseridas.</span><span class="sxs-lookup"><span data-stu-id="3f94b-194">Declaration statements are permitted in blocks, but are not permitted as embedded statements.</span></span>

```antlr
declaration_statement
    : local_variable_declaration ';'
    | local_constant_declaration ';'
    ;
```

### <a name="local-variable-declarations"></a><span data-ttu-id="3f94b-195">Declarações de variáveis locais</span><span class="sxs-lookup"><span data-stu-id="3f94b-195">Local variable declarations</span></span>

<span data-ttu-id="3f94b-196">Um *local_variable_declaration* declara uma ou mais variáveis locais.</span><span class="sxs-lookup"><span data-stu-id="3f94b-196">A *local_variable_declaration* declares one or more local variables.</span></span>

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

<span data-ttu-id="3f94b-197">O *local_variable_type* de um *local_variable_declaration* especifica diretamente o tipo das variáveis introduzidas pela declaração ou indica com o identificador `var` que o tipo deve ser inferido com base em um inicializador.</span><span class="sxs-lookup"><span data-stu-id="3f94b-197">The *local_variable_type* of a *local_variable_declaration* either directly specifies the type of the variables introduced by the declaration, or indicates with the identifier `var` that the type should be inferred based on an initializer.</span></span> <span data-ttu-id="3f94b-198">O tipo é seguido por uma lista de *local_variable_declarator*s, cada um dos quais introduz uma nova variável.</span><span class="sxs-lookup"><span data-stu-id="3f94b-198">The type is followed by a list of *local_variable_declarator*s, each of which introduces a new variable.</span></span> <span data-ttu-id="3f94b-199">Um *local_variable_declarator* consiste em um *identificador* que nomeia a variável, opcionalmente seguido por um token "`=`" e um *local_variable_initializer* que fornece o valor inicial da variável.</span><span class="sxs-lookup"><span data-stu-id="3f94b-199">A *local_variable_declarator* consists of an *identifier* that names the variable, optionally followed by an "`=`" token and a *local_variable_initializer* that gives the initial value of the variable.</span></span>

<span data-ttu-id="3f94b-200">No contexto de uma declaração de variável local, o identificador var age como uma palavra-chave contextual ([palavras-chave](lexical-structure.md#keywords)). Quando *local_variable_type* é especificado como `var` e nenhum tipo chamado `var` está no escopo, a declaração é uma ***declaração de variável local implicitamente tipada***, cujo tipo é inferido do tipo da expressão de inicializador associada.</span><span class="sxs-lookup"><span data-stu-id="3f94b-200">In the context of a local variable declaration, the identifier var acts as a contextual keyword ([Keywords](lexical-structure.md#keywords)).When the *local_variable_type* is specified as `var` and no type named `var` is in scope, the declaration is an ***implicitly typed local variable declaration***, whose type is inferred from the type of the associated initializer expression.</span></span> <span data-ttu-id="3f94b-201">As declarações de variáveis locais digitadas implicitamente estão sujeitas às seguintes restrições:</span><span class="sxs-lookup"><span data-stu-id="3f94b-201">Implicitly typed local variable declarations are subject to the following restrictions:</span></span>

*  <span data-ttu-id="3f94b-202">O *local_variable_declaration* não pode incluir vários *local_variable_declarator*s.</span><span class="sxs-lookup"><span data-stu-id="3f94b-202">The *local_variable_declaration* cannot include multiple *local_variable_declarator*s.</span></span>
*  <span data-ttu-id="3f94b-203">O *local_variable_declarator* deve incluir um *local_variable_initializer*.</span><span class="sxs-lookup"><span data-stu-id="3f94b-203">The *local_variable_declarator* must include a *local_variable_initializer*.</span></span>
*  <span data-ttu-id="3f94b-204">O *local_variable_initializer* deve ser uma *expressão*.</span><span class="sxs-lookup"><span data-stu-id="3f94b-204">The *local_variable_initializer* must be an *expression*.</span></span>
*  <span data-ttu-id="3f94b-205">A *expressão* do inicializador deve ter um tipo de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="3f94b-205">The initializer *expression* must have a compile-time type.</span></span>
*  <span data-ttu-id="3f94b-206">A *expressão* do inicializador não pode se referir à própria variável declarada</span><span class="sxs-lookup"><span data-stu-id="3f94b-206">The initializer *expression* cannot refer to the declared variable itself</span></span>

<span data-ttu-id="3f94b-207">Veja a seguir exemplos de declarações de variáveis locais incorretas de tipo implícito:</span><span class="sxs-lookup"><span data-stu-id="3f94b-207">The following are examples of incorrect implicitly typed local variable declarations:</span></span>

```csharp
var x;               // Error, no initializer to infer type from
var y = {1, 2, 3};   // Error, array initializer not permitted
var z = null;        // Error, null does not have a type
var u = x => x + 1;  // Error, anonymous functions do not have a type
var v = v++;         // Error, initializer cannot refer to variable itself
```

<span data-ttu-id="3f94b-208">O valor de uma variável local é obtido em uma expressão usando um *Simple_name* ([nomes simples](expressions.md#simple-names)) e o valor de uma variável local é modificado usando uma *atribuição* ([operadores de atribuição](expressions.md#assignment-operators)).</span><span class="sxs-lookup"><span data-stu-id="3f94b-208">The value of a local variable is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)), and the value of a local variable is modified using an *assignment* ([Assignment operators](expressions.md#assignment-operators)).</span></span> <span data-ttu-id="3f94b-209">Uma variável local deve ser definitivamente atribuída ([atribuição definitiva](variables.md#definite-assignment)) em cada local em que seu valor é obtido.</span><span class="sxs-lookup"><span data-stu-id="3f94b-209">A local variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) at each location where its value is obtained.</span></span>

<span data-ttu-id="3f94b-210">O escopo de uma variável local declarada em um *local_variable_declaration* é o bloco no qual a declaração ocorre.</span><span class="sxs-lookup"><span data-stu-id="3f94b-210">The scope of a local variable declared in a *local_variable_declaration* is the block in which the declaration occurs.</span></span> <span data-ttu-id="3f94b-211">É um erro fazer referência a uma variável local em uma posição textual que precede o *local_variable_declarator* da variável local.</span><span class="sxs-lookup"><span data-stu-id="3f94b-211">It is an error to refer to a local variable in a textual position that precedes the *local_variable_declarator* of the local variable.</span></span> <span data-ttu-id="3f94b-212">Dentro do escopo de uma variável local, é um erro de tempo de compilação para declarar outra variável local ou constante com o mesmo nome.</span><span class="sxs-lookup"><span data-stu-id="3f94b-212">Within the scope of a local variable, it is a compile-time error to declare another local variable or constant with the same name.</span></span>

<span data-ttu-id="3f94b-213">Uma declaração de variável local que declara várias variáveis é equivalente a várias declarações de variáveis únicas com o mesmo tipo.</span><span class="sxs-lookup"><span data-stu-id="3f94b-213">A local variable declaration that declares multiple variables is equivalent to multiple declarations of single variables with the same type.</span></span> <span data-ttu-id="3f94b-214">Além disso, um inicializador de variável em uma declaração de variável local corresponde exatamente a uma instrução de atribuição que é inserida imediatamente após a declaração.</span><span class="sxs-lookup"><span data-stu-id="3f94b-214">Furthermore, a variable initializer in a local variable declaration corresponds exactly to an assignment statement that is inserted immediately after the declaration.</span></span>

<span data-ttu-id="3f94b-215">O exemplo</span><span class="sxs-lookup"><span data-stu-id="3f94b-215">The example</span></span>
```csharp
void F() {
    int x = 1, y, z = x * 2;
}
```
<span data-ttu-id="3f94b-216">corresponde exatamente a</span><span class="sxs-lookup"><span data-stu-id="3f94b-216">corresponds exactly to</span></span>
```csharp
void F() {
    int x; x = 1;
    int y;
    int z; z = x * 2;
}
```

<span data-ttu-id="3f94b-217">Em uma declaração de variável local digitada implicitamente, o tipo da variável local que está sendo declarada é usado como sendo o mesmo que o tipo da expressão usada para inicializar a variável.</span><span class="sxs-lookup"><span data-stu-id="3f94b-217">In an implicitly typed local variable declaration, the type of the local variable being declared is taken to be the same as the type of the expression used to initialize the variable.</span></span> <span data-ttu-id="3f94b-218">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="3f94b-218">For example:</span></span>
```csharp
var i = 5;
var s = "Hello";
var d = 1.0;
var numbers = new int[] {1, 2, 3};
var orders = new Dictionary<int,Order>();
```

<span data-ttu-id="3f94b-219">As declarações de variável local digitadas implicitamente acima são exatamente equivalentes às seguintes declarações tipadas explicitamente:</span><span class="sxs-lookup"><span data-stu-id="3f94b-219">The implicitly typed local variable declarations above are precisely equivalent to the following explicitly typed declarations:</span></span>
```csharp
int i = 5;
string s = "Hello";
double d = 1.0;
int[] numbers = new int[] {1, 2, 3};
Dictionary<int,Order> orders = new Dictionary<int,Order>();
```

### <a name="local-constant-declarations"></a><span data-ttu-id="3f94b-220">Declarações de constantes locais</span><span class="sxs-lookup"><span data-stu-id="3f94b-220">Local constant declarations</span></span>

<span data-ttu-id="3f94b-221">Um *local_constant_declaration* declara uma ou mais constantes locais.</span><span class="sxs-lookup"><span data-stu-id="3f94b-221">A *local_constant_declaration* declares one or more local constants.</span></span>

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

<span data-ttu-id="3f94b-222">O *tipo* de um *local_constant_declaration* especifica o tipo das constantes introduzidas pela declaração.</span><span class="sxs-lookup"><span data-stu-id="3f94b-222">The *type* of a *local_constant_declaration* specifies the type of the constants introduced by the declaration.</span></span> <span data-ttu-id="3f94b-223">O tipo é seguido por uma lista de *constant_declarator*s, cada um dos quais introduz uma nova constante.</span><span class="sxs-lookup"><span data-stu-id="3f94b-223">The type is followed by a list of *constant_declarator*s, each of which introduces a new constant.</span></span> <span data-ttu-id="3f94b-224">Um *constant_declarator* consiste em um *identificador* que nomeia a constante, seguido por um token "`=`", seguido por um *constant_expression* ([expressões constantes](expressions.md#constant-expressions)) que fornece o valor da constante.</span><span class="sxs-lookup"><span data-stu-id="3f94b-224">A *constant_declarator* consists of an *identifier* that names the constant, followed by an "`=`" token, followed by a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) that gives the value of the constant.</span></span>

<span data-ttu-id="3f94b-225">O *tipo* e *constant_expression* de uma declaração de constante local devem seguir as mesmas regras que as de uma declaração de membro constante ([constantes](classes.md#constants)).</span><span class="sxs-lookup"><span data-stu-id="3f94b-225">The *type* and *constant_expression* of a local constant declaration must follow the same rules as those of a constant member declaration ([Constants](classes.md#constants)).</span></span>

<span data-ttu-id="3f94b-226">O valor de uma constante local é obtido em uma expressão usando um *Simple_name* ([nomes simples](expressions.md#simple-names)).</span><span class="sxs-lookup"><span data-stu-id="3f94b-226">The value of a local constant is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)).</span></span>

<span data-ttu-id="3f94b-227">O escopo de uma constante local é o bloco no qual a declaração ocorre.</span><span class="sxs-lookup"><span data-stu-id="3f94b-227">The scope of a local constant is the block in which the declaration occurs.</span></span> <span data-ttu-id="3f94b-228">É um erro fazer referência a uma constante local em uma posição textual que precede seu *constant_declarator*.</span><span class="sxs-lookup"><span data-stu-id="3f94b-228">It is an error to refer to a local constant in a textual position that precedes its *constant_declarator*.</span></span> <span data-ttu-id="3f94b-229">Dentro do escopo de uma constante local, é um erro de tempo de compilação para declarar outra variável local ou constante com o mesmo nome.</span><span class="sxs-lookup"><span data-stu-id="3f94b-229">Within the scope of a local constant, it is a compile-time error to declare another local variable or constant with the same name.</span></span>

<span data-ttu-id="3f94b-230">Uma declaração de constante local que declara várias constantes é equivalente a várias declarações de constantes únicas com o mesmo tipo.</span><span class="sxs-lookup"><span data-stu-id="3f94b-230">A local constant declaration that declares multiple constants is equivalent to multiple declarations of single constants with the same type.</span></span>

## <a name="expression-statements"></a><span data-ttu-id="3f94b-231">Instruções de expressão</span><span class="sxs-lookup"><span data-stu-id="3f94b-231">Expression statements</span></span>

<span data-ttu-id="3f94b-232">Um *expression_statement* avalia uma determinada expressão.</span><span class="sxs-lookup"><span data-stu-id="3f94b-232">An *expression_statement* evaluates a given expression.</span></span> <span data-ttu-id="3f94b-233">O valor calculado pela expressão, se houver, é Descartado.</span><span class="sxs-lookup"><span data-stu-id="3f94b-233">The value computed by the expression, if any, is discarded.</span></span>

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

<span data-ttu-id="3f94b-234">Nem todas as expressões são permitidas como instruções.</span><span class="sxs-lookup"><span data-stu-id="3f94b-234">Not all expressions are permitted as statements.</span></span> <span data-ttu-id="3f94b-235">Em particular, as `x + y` expressões como e `x == 1` que meramente computam um valor (que será Descartado) não são permitidas como instruções.</span><span class="sxs-lookup"><span data-stu-id="3f94b-235">In particular, expressions such as `x + y` and `x == 1` that merely compute a value (which will be discarded), are not permitted as statements.</span></span>

<span data-ttu-id="3f94b-236">A execução de um *expression_statement* avalia a expressão contida e, em seguida, transfere o controle para o ponto de extremidade do *expression_statement*.</span><span class="sxs-lookup"><span data-stu-id="3f94b-236">Execution of an *expression_statement* evaluates the contained expression and then transfers control to the end point of the *expression_statement*.</span></span> <span data-ttu-id="3f94b-237">O ponto de extremidade de um *expression_statement* será acessível se o *expression_statement* estiver acessível.</span><span class="sxs-lookup"><span data-stu-id="3f94b-237">The end point of an *expression_statement* is reachable if that *expression_statement* is reachable.</span></span>

## <a name="selection-statements"></a><span data-ttu-id="3f94b-238">Instruções de seleção</span><span class="sxs-lookup"><span data-stu-id="3f94b-238">Selection statements</span></span>

<span data-ttu-id="3f94b-239">Instruções de seleção selecione um número de possíveis instruções para execução com base no valor de alguma expressão.</span><span class="sxs-lookup"><span data-stu-id="3f94b-239">Selection statements select one of a number of possible statements for execution based on the value of some expression.</span></span>

```antlr
selection_statement
    : if_statement
    | switch_statement
    ;
```

### <a name="the-if-statement"></a><span data-ttu-id="3f94b-240">A instrução If</span><span class="sxs-lookup"><span data-stu-id="3f94b-240">The if statement</span></span>

<span data-ttu-id="3f94b-241">A `if` instrução seleciona uma instrução para execução com base no valor de uma expressão booliana.</span><span class="sxs-lookup"><span data-stu-id="3f94b-241">The `if` statement selects a statement for execution based on the value of a boolean expression.</span></span>

```antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

<span data-ttu-id="3f94b-242">Uma `else` parte é associada ao precedentes `if` lexicalmente mais próximo que é permitido pela sintaxe.</span><span class="sxs-lookup"><span data-stu-id="3f94b-242">An `else` part is associated with the lexically nearest preceding `if` that is allowed by the syntax.</span></span> <span data-ttu-id="3f94b-243">Portanto, uma `if` instrução do formulário</span><span class="sxs-lookup"><span data-stu-id="3f94b-243">Thus, an `if` statement of the form</span></span>
```csharp
if (x) if (y) F(); else G();
```
<span data-ttu-id="3f94b-244">equivale a</span><span class="sxs-lookup"><span data-stu-id="3f94b-244">is equivalent to</span></span>
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

<span data-ttu-id="3f94b-245">Uma `if` instrução é executada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="3f94b-245">An `if` statement is executed as follows:</span></span>

*  <span data-ttu-id="3f94b-246">O *Boolean_expression* ([expressões booleanas](expressions.md#boolean-expressions)) é avaliado.</span><span class="sxs-lookup"><span data-stu-id="3f94b-246">The *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span>
*  <span data-ttu-id="3f94b-247">Se a expressão booliana produz `true`, o controle é transferido para a primeira instrução inserida.</span><span class="sxs-lookup"><span data-stu-id="3f94b-247">If the boolean expression yields `true`, control is transferred to the first embedded statement.</span></span> <span data-ttu-id="3f94b-248">Quando e se o controle atingir o ponto final dessa instrução, o controle será transferido para o ponto de extremidade `if` da instrução.</span><span class="sxs-lookup"><span data-stu-id="3f94b-248">When and if control reaches the end point of that statement, control is transferred to the end point of the `if` statement.</span></span>
*  <span data-ttu-id="3f94b-249">Se a expressão booliana produz `false` e se uma `else` parte estiver presente, o controle será transferido para a segunda instrução inserida.</span><span class="sxs-lookup"><span data-stu-id="3f94b-249">If the boolean expression yields `false` and if an `else` part is present, control is transferred to the second embedded statement.</span></span> <span data-ttu-id="3f94b-250">Quando e se o controle atingir o ponto final dessa instrução, o controle será transferido para o ponto de extremidade `if` da instrução.</span><span class="sxs-lookup"><span data-stu-id="3f94b-250">When and if control reaches the end point of that statement, control is transferred to the end point of the `if` statement.</span></span>
*  <span data-ttu-id="3f94b-251">Se a expressão booliana produz `false` e se uma `else` parte não estiver presente, o controle será transferido para `if` o ponto final da instrução.</span><span class="sxs-lookup"><span data-stu-id="3f94b-251">If the boolean expression yields `false` and if an `else` part is not present, control is transferred to the end point of the `if` statement.</span></span>

<span data-ttu-id="3f94b-252">A primeira instrução inserida de `if` uma instrução pode ser acessada se a `if` instrução estiver acessível e a expressão booliana não `false`tiver o valor constante.</span><span class="sxs-lookup"><span data-stu-id="3f94b-252">The first embedded statement of an `if` statement is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `false`.</span></span>

<span data-ttu-id="3f94b-253">A segunda instrução inserida de `if` uma instrução, se presente, estará acessível se `if` a instrução estiver acessível e a expressão booliana não tiver o valor `true`constante.</span><span class="sxs-lookup"><span data-stu-id="3f94b-253">The second embedded statement of an `if` statement, if present, is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

<span data-ttu-id="3f94b-254">O ponto de extremidade de `if` uma instrução pode ser acessado se o ponto de extremidade de pelo menos uma de suas instruções inseridas estiver acessível.</span><span class="sxs-lookup"><span data-stu-id="3f94b-254">The end point of an `if` statement is reachable if the end point of at least one of its embedded statements is reachable.</span></span> <span data-ttu-id="3f94b-255">Além disso, o ponto de extremidade de `if` uma instrução sem `else` nenhuma parte será acessível se `if` a instrução estiver acessível e a expressão booliana não tiver o valor `true`constante.</span><span class="sxs-lookup"><span data-stu-id="3f94b-255">In addition, the end point of an `if` statement with no `else` part is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-switch-statement"></a><span data-ttu-id="3f94b-256">A instrução switch</span><span class="sxs-lookup"><span data-stu-id="3f94b-256">The switch statement</span></span>

<span data-ttu-id="3f94b-257">A instrução switch seleciona a execução de uma lista de instruções com um rótulo de comutador associado que corresponde ao valor da expressão switch.</span><span class="sxs-lookup"><span data-stu-id="3f94b-257">The switch statement selects for execution a statement list having an associated switch label that corresponds to the value of the switch expression.</span></span>

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

<span data-ttu-id="3f94b-258">Um *switch_statement* consiste na palavra-chave `switch`, seguida por uma expressão entre parênteses (chamada de expressão switch), seguida por um *switch_block*.</span><span class="sxs-lookup"><span data-stu-id="3f94b-258">A *switch_statement* consists of the keyword `switch`, followed by a parenthesized expression (called the switch expression), followed by a *switch_block*.</span></span> <span data-ttu-id="3f94b-259">O *switch_block* consiste em zero ou mais *switch_section*s, entre chaves.</span><span class="sxs-lookup"><span data-stu-id="3f94b-259">The *switch_block* consists of zero or more *switch_section*s, enclosed in braces.</span></span> <span data-ttu-id="3f94b-260">Cada *switch_section* consiste em um ou mais *switch_label*, seguido por um *statement_list* ([listas de instruções](statements.md#statement-lists)).</span><span class="sxs-lookup"><span data-stu-id="3f94b-260">Each *switch_section* consists of one or more *switch_label*s followed by a *statement_list* ([Statement lists](statements.md#statement-lists)).</span></span>

<span data-ttu-id="3f94b-261">O ***tipo de controle*** de uma `switch` instrução é estabelecido pela expressão switch.</span><span class="sxs-lookup"><span data-stu-id="3f94b-261">The ***governing type*** of a `switch` statement is established by the switch expression.</span></span>

*  <span data-ttu-id="3f94b-262">Se o tipo da expressão switch for `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `bool`, `char`, 0 ou um *enum_type*, ou se for o tipo anulável correspondente a um desses tipos , esse é o tipo regulador da instrução 2.</span><span class="sxs-lookup"><span data-stu-id="3f94b-262">If the type of the switch expression is `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `bool`, `char`, `string`, or an *enum_type*, or if it is the nullable type corresponding to one of these types, then that is the governing type of the `switch` statement.</span></span>
*  <span data-ttu-id="3f94b-263">Caso contrário, exatamente uma conversão implícita definida pelo usuário[(conversões definidas pelo usuário](conversions.md#user-defined-conversions)) deve existir a partir do tipo da expressão switch para um dos seguintes tipos de controle possíveis: `sbyte`, `byte`,, `ushort` `short` `int` ,,`uint`,,,, ou, um tipo anulável correspondente a um desses tipos. `long` `ulong` `char` `string`</span><span class="sxs-lookup"><span data-stu-id="3f94b-263">Otherwise, exactly one user-defined implicit conversion ([User-defined conversions](conversions.md#user-defined-conversions)) must exist from the type of the switch expression to one of the following possible governing types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `string`, or,  a nullable type corresponding to one of those types.</span></span>
*  <span data-ttu-id="3f94b-264">Caso contrário, se nenhuma conversão implícita existir, ou se houver mais de uma conversão implícita, ocorrerá um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="3f94b-264">Otherwise, if no such implicit conversion exists, or if more than one such implicit conversion exists, a compile-time error occurs.</span></span>

<span data-ttu-id="3f94b-265">A expressão constante de cada `case` rótulo deve indicar um valor que é implicitamente conversível ([conversões implícitas](conversions.md#implicit-conversions)) para o tipo regulador da `switch` instrução.</span><span class="sxs-lookup"><span data-stu-id="3f94b-265">The constant expression of each `case` label must denote a value that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the governing type of the `switch` statement.</span></span> <span data-ttu-id="3f94b-266">Um erro de tempo de compilação ocorre se dois ou `case` mais rótulos na mesma `switch` instrução especificarem o mesmo valor de constante.</span><span class="sxs-lookup"><span data-stu-id="3f94b-266">A compile-time error occurs if two or more `case` labels in the same `switch` statement specify the same constant value.</span></span>

<span data-ttu-id="3f94b-267">Pode haver no máximo um `default` rótulo em uma instrução switch.</span><span class="sxs-lookup"><span data-stu-id="3f94b-267">There can be at most one `default` label in a switch statement.</span></span>

<span data-ttu-id="3f94b-268">Uma `switch` instrução é executada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="3f94b-268">A `switch` statement is executed as follows:</span></span>

*  <span data-ttu-id="3f94b-269">A expressão switch é avaliada e convertida no tipo regulador.</span><span class="sxs-lookup"><span data-stu-id="3f94b-269">The switch expression is evaluated and converted to the governing type.</span></span>
*  <span data-ttu-id="3f94b-270">Se uma das constantes especificadas em um `case` rótulo na mesma `switch` instrução for igual ao valor da expressão switch, o controle será transferido para a lista de instruções após o rótulo correspondente `case` .</span><span class="sxs-lookup"><span data-stu-id="3f94b-270">If one of the constants specified in a `case` label in the same `switch` statement is equal to the value of the switch expression, control is transferred to the statement list following the matched `case` label.</span></span>
*  <span data-ttu-id="3f94b-271">Se nenhuma `case` das constantes especificadas em rótulos na mesma `switch` instrução for igual ao valor da expressão switch e, se um `default` rótulo estiver presente, o controle será transferido para a lista de instruções após o `default` chamada.</span><span class="sxs-lookup"><span data-stu-id="3f94b-271">If none of the constants specified in `case` labels in the same `switch` statement is equal to the value of the switch expression, and if a `default` label is present, control is transferred to the statement list following the `default` label.</span></span>
*  <span data-ttu-id="3f94b-272">Se nenhuma `case` das constantes especificadas em rótulos na mesma `switch` instrução for igual ao valor da expressão switch e, se nenhum `default` rótulo estiver presente, o controle `switch` será transferido para o ponto final da instrução.</span><span class="sxs-lookup"><span data-stu-id="3f94b-272">If none of the constants specified in `case` labels in the same `switch` statement is equal to the value of the switch expression, and if no `default` label is present, control is transferred to the end point of the `switch` statement.</span></span>

<span data-ttu-id="3f94b-273">Se o ponto de extremidade da lista de instruções de uma seção de comutador estiver acessível, ocorrerá um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="3f94b-273">If the end point of the statement list of a switch section is reachable, a compile-time error occurs.</span></span> <span data-ttu-id="3f94b-274">Isso é conhecido como a regra "não cair".</span><span class="sxs-lookup"><span data-stu-id="3f94b-274">This is known as the "no fall through" rule.</span></span> <span data-ttu-id="3f94b-275">O exemplo</span><span class="sxs-lookup"><span data-stu-id="3f94b-275">The example</span></span>
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
<span data-ttu-id="3f94b-276">é válido porque nenhuma seção de opção tem um ponto de extremidade acessível.</span><span class="sxs-lookup"><span data-stu-id="3f94b-276">is valid because no switch section has a reachable end point.</span></span> <span data-ttu-id="3f94b-277">Ao contrário de C++C e, a execução de uma seção switch não é permitida para "passar" para a próxima seção switch e o exemplo</span><span class="sxs-lookup"><span data-stu-id="3f94b-277">Unlike C and C++, execution of a switch section is not permitted to "fall through" to the next switch section, and the example</span></span>
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
<span data-ttu-id="3f94b-278">resulta em um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="3f94b-278">results in a compile-time error.</span></span> <span data-ttu-id="3f94b-279">Quando a execução de uma seção switch é seguida pela execução de outra seção switch, uma instrução or `goto case` `goto default` explícita deve ser usada:</span><span class="sxs-lookup"><span data-stu-id="3f94b-279">When execution of a switch section is to be followed by execution of another switch section, an explicit `goto case` or `goto default` statement must be used:</span></span>
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

<span data-ttu-id="3f94b-280">Vários rótulos são permitidos em um *switch_section*.</span><span class="sxs-lookup"><span data-stu-id="3f94b-280">Multiple labels are permitted in a *switch_section*.</span></span> <span data-ttu-id="3f94b-281">O exemplo</span><span class="sxs-lookup"><span data-stu-id="3f94b-281">The example</span></span>
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
<span data-ttu-id="3f94b-282">é válido.</span><span class="sxs-lookup"><span data-stu-id="3f94b-282">is valid.</span></span> <span data-ttu-id="3f94b-283">O exemplo não viola a regra "no enquadramento" porque os rótulos `case 2:` e `default:` fazem parte do mesmo *switch_section*.</span><span class="sxs-lookup"><span data-stu-id="3f94b-283">The example does not violate the "no fall through" rule because the labels `case 2:` and `default:` are part of the same *switch_section*.</span></span>

<span data-ttu-id="3f94b-284">A regra "no enquadramento" impede uma classe comum de bugs que ocorrem em C C++ e `break` quando as instruções são omitidas acidentalmente.</span><span class="sxs-lookup"><span data-stu-id="3f94b-284">The "no fall through" rule prevents a common class of bugs that occur in C and C++ when `break` statements are accidentally omitted.</span></span> <span data-ttu-id="3f94b-285">Além disso, devido a essa regra, as seções switch de uma `switch` instrução podem ser reorganizadas arbitrariamente sem afetar o comportamento da instrução.</span><span class="sxs-lookup"><span data-stu-id="3f94b-285">In addition, because of this rule, the switch sections of a `switch` statement can be arbitrarily rearranged without affecting the behavior of the statement.</span></span> <span data-ttu-id="3f94b-286">Por exemplo, as seções da `switch` instrução acima podem ser revertidas sem afetar o comportamento da instrução:</span><span class="sxs-lookup"><span data-stu-id="3f94b-286">For example, the sections of the `switch` statement above can be reversed without affecting the behavior of the statement:</span></span>
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

<span data-ttu-id="3f94b-287">A lista de instruções de uma seção de alternância normalmente termina `break`em `goto case`uma instrução `goto default` , ou, mas qualquer construção que renderiza o ponto final da lista de instrução inacessível é permitida.</span><span class="sxs-lookup"><span data-stu-id="3f94b-287">The statement list of a switch section typically ends in a `break`, `goto case`, or `goto default` statement, but any construct that renders the end point of the statement list unreachable is permitted.</span></span> <span data-ttu-id="3f94b-288">Por exemplo, uma `while` instrução controlada pela expressão `true` booliana é conhecida como nunca atingir seu ponto de extremidade.</span><span class="sxs-lookup"><span data-stu-id="3f94b-288">For example, a `while` statement controlled by the boolean expression `true` is known to never reach its end point.</span></span> <span data-ttu-id="3f94b-289">Da mesma forma `throw` , `return` uma instrução or sempre transfere o controle em outro lugar e nunca atinge seu ponto de extremidade.</span><span class="sxs-lookup"><span data-stu-id="3f94b-289">Likewise, a `throw` or `return` statement always transfers control elsewhere and never reaches its end point.</span></span> <span data-ttu-id="3f94b-290">Portanto, o exemplo a seguir é válido:</span><span class="sxs-lookup"><span data-stu-id="3f94b-290">Thus, the following example is valid:</span></span>
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

<span data-ttu-id="3f94b-291">O tipo regulador de uma `switch` instrução pode ser o tipo `string`.</span><span class="sxs-lookup"><span data-stu-id="3f94b-291">The governing type of a `switch` statement may be the type `string`.</span></span> <span data-ttu-id="3f94b-292">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="3f94b-292">For example:</span></span>
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

<span data-ttu-id="3f94b-293">Como os operadores de igualdade de cadeia de caracteres (operadores de `switch` igualdade de cadeia de[caracteres](expressions.md#string-equality-operators)), a instrução diferencia maiúsculas de minúsculas e executará uma determinada seção de comutador somente se a cadeia de caracteres de expressão switch corresponder exatamente a uma constante `case`</span><span class="sxs-lookup"><span data-stu-id="3f94b-293">Like the string equality operators ([String equality operators](expressions.md#string-equality-operators)), the `switch` statement is case sensitive and will execute a given switch section only if the switch expression string exactly matches a `case` label constant.</span></span>

<span data-ttu-id="3f94b-294">Quando o tipo regulador de uma `switch` instrução é `string`, o valor `null` é permitido como uma constante de rótulo de caso.</span><span class="sxs-lookup"><span data-stu-id="3f94b-294">When the governing type of a `switch` statement is `string`, the value `null` is permitted as a case label constant.</span></span>

<span data-ttu-id="3f94b-295">Os *statement_list*s de um *switch_block* podem conter instruções de declaração ([instruções de declaração](statements.md#declaration-statements)).</span><span class="sxs-lookup"><span data-stu-id="3f94b-295">The *statement_list*s of a *switch_block* may contain declaration statements ([Declaration statements](statements.md#declaration-statements)).</span></span> <span data-ttu-id="3f94b-296">O escopo de uma variável local ou constante declarada em um bloco switch é o bloco switch.</span><span class="sxs-lookup"><span data-stu-id="3f94b-296">The scope of a local variable or constant declared in a switch block is the switch block.</span></span>

<span data-ttu-id="3f94b-297">A lista de instruções de uma determinada seção de comutador pode `switch` ser acessada se a instrução estiver acessível e pelo menos uma das seguintes opções for verdadeira:</span><span class="sxs-lookup"><span data-stu-id="3f94b-297">The statement list of a given switch section is reachable if the `switch` statement is reachable and at least one of the following is true:</span></span>

*  <span data-ttu-id="3f94b-298">A expressão switch é um valor não constante.</span><span class="sxs-lookup"><span data-stu-id="3f94b-298">The switch expression is a non-constant value.</span></span>
*  <span data-ttu-id="3f94b-299">A expressão switch é um valor constante que corresponde a `case` um rótulo na seção switch.</span><span class="sxs-lookup"><span data-stu-id="3f94b-299">The switch expression is a constant value that matches a `case` label in the switch section.</span></span>
*  <span data-ttu-id="3f94b-300">A expressão switch é um valor constante que não corresponde a `case` nenhum rótulo e a seção switch contém o `default` rótulo.</span><span class="sxs-lookup"><span data-stu-id="3f94b-300">The switch expression is a constant value that doesn't match any `case` label, and the switch section contains the `default` label.</span></span>
*  <span data-ttu-id="3f94b-301">Um rótulo de switch da seção switch é referenciado por uma `goto case` instrução `goto default` ou alcançável.</span><span class="sxs-lookup"><span data-stu-id="3f94b-301">A switch label of the switch section is referenced by a reachable `goto case` or `goto default` statement.</span></span>

<span data-ttu-id="3f94b-302">O ponto de extremidade de `switch` uma instrução pode ser acessado se pelo menos uma das seguintes opções for verdadeira:</span><span class="sxs-lookup"><span data-stu-id="3f94b-302">The end point of a `switch` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="3f94b-303">A `switch` instrução contém uma instrução `break` alcançável que sai da `switch` instrução.</span><span class="sxs-lookup"><span data-stu-id="3f94b-303">The `switch` statement contains a reachable `break` statement that exits the `switch` statement.</span></span>
*  <span data-ttu-id="3f94b-304">A `switch` instrução está acessível, a expressão switch é um valor não constante e nenhum `default` rótulo está presente.</span><span class="sxs-lookup"><span data-stu-id="3f94b-304">The `switch` statement is reachable, the switch expression is a non-constant value, and no `default` label is present.</span></span>
*  <span data-ttu-id="3f94b-305">A `switch` instrução está acessível, a expressão switch é um valor constante que não corresponde a `case` nenhum rótulo e nenhum `default` rótulo está presente.</span><span class="sxs-lookup"><span data-stu-id="3f94b-305">The `switch` statement is reachable, the switch expression is a constant value that doesn't match any `case` label, and no `default` label is present.</span></span>

## <a name="iteration-statements"></a><span data-ttu-id="3f94b-306">Instruções de iteração</span><span class="sxs-lookup"><span data-stu-id="3f94b-306">Iteration statements</span></span>

<span data-ttu-id="3f94b-307">Instruções de iteração executa repetidamente uma instrução inserida.</span><span class="sxs-lookup"><span data-stu-id="3f94b-307">Iteration statements repeatedly execute an embedded statement.</span></span>

```antlr
iteration_statement
    : while_statement
    | do_statement
    | for_statement
    | foreach_statement
    ;
```

### <a name="the-while-statement"></a><span data-ttu-id="3f94b-308">A instrução while</span><span class="sxs-lookup"><span data-stu-id="3f94b-308">The while statement</span></span>

<span data-ttu-id="3f94b-309">A `while` instrução Executa condicionalmente uma instrução inserida zero ou mais vezes.</span><span class="sxs-lookup"><span data-stu-id="3f94b-309">The `while` statement conditionally executes an embedded statement zero or more times.</span></span>

```antlr
while_statement
    : 'while' '(' boolean_expression ')' embedded_statement
    ;
```

<span data-ttu-id="3f94b-310">Uma `while` instrução é executada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="3f94b-310">A `while` statement is executed as follows:</span></span>

*  <span data-ttu-id="3f94b-311">O *Boolean_expression* ([expressões booleanas](expressions.md#boolean-expressions)) é avaliado.</span><span class="sxs-lookup"><span data-stu-id="3f94b-311">The *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span>
*  <span data-ttu-id="3f94b-312">Se a expressão booliana produz `true`, o controle é transferido para a instrução inserida.</span><span class="sxs-lookup"><span data-stu-id="3f94b-312">If the boolean expression yields `true`, control is transferred to the embedded statement.</span></span> <span data-ttu-id="3f94b-313">Quando e se o controle atingir o ponto de extremidade da instrução inserida (possivelmente da execução `continue` de uma instrução), o controle será transferido para o `while` início da instrução.</span><span class="sxs-lookup"><span data-stu-id="3f94b-313">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), control is transferred to the beginning of the `while` statement.</span></span>
*  <span data-ttu-id="3f94b-314">Se a expressão booliana produz `false`, o controle é transferido para o ponto final `while` da instrução.</span><span class="sxs-lookup"><span data-stu-id="3f94b-314">If the boolean expression yields `false`, control is transferred to the end point of the `while` statement.</span></span>

<span data-ttu-id="3f94b-315">Dentro da instrução inserida de `while` uma instrução, `break` uma instrução ([a instrução break](statements.md#the-break-statement)) pode ser usada para transferir o `while` controle para o ponto final da instrução (encerrando assim a iteração da instrução inserida) e um `continue` a instrução ([a instrução Continue](statements.md#the-continue-statement)) pode ser usada para transferir o controle para o ponto final da instrução inserida (assim, executando outra `while` iteração da instrução).</span><span class="sxs-lookup"><span data-stu-id="3f94b-315">Within the embedded statement of a `while` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `while` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement (thus performing another iteration of the `while` statement).</span></span>

<span data-ttu-id="3f94b-316">A instrução inserida de `while` uma instrução pode ser acessada se a `while` instrução estiver acessível e a expressão booliana não `false`tiver o valor constante.</span><span class="sxs-lookup"><span data-stu-id="3f94b-316">The embedded statement of a `while` statement is reachable if the `while` statement is reachable and the boolean expression does not have the constant value `false`.</span></span>

<span data-ttu-id="3f94b-317">O ponto de extremidade de `while` uma instrução pode ser acessado se pelo menos uma das seguintes opções for verdadeira:</span><span class="sxs-lookup"><span data-stu-id="3f94b-317">The end point of a `while` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="3f94b-318">A `while` instrução contém uma instrução `break` alcançável que sai da `while` instrução.</span><span class="sxs-lookup"><span data-stu-id="3f94b-318">The `while` statement contains a reachable `break` statement that exits the `while` statement.</span></span>
*  <span data-ttu-id="3f94b-319">A `while` instrução está acessível e a expressão booliana não tem o valor `true`constante.</span><span class="sxs-lookup"><span data-stu-id="3f94b-319">The `while` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-do-statement"></a><span data-ttu-id="3f94b-320">A instrução do</span><span class="sxs-lookup"><span data-stu-id="3f94b-320">The do statement</span></span>

<span data-ttu-id="3f94b-321">A `do` instrução Executa condicionalmente uma instrução inserida uma ou mais vezes.</span><span class="sxs-lookup"><span data-stu-id="3f94b-321">The `do` statement conditionally executes an embedded statement one or more times.</span></span>

```antlr
do_statement
    : 'do' embedded_statement 'while' '(' boolean_expression ')' ';'
    ;
```

<span data-ttu-id="3f94b-322">Uma `do` instrução é executada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="3f94b-322">A `do` statement is executed as follows:</span></span>

*  <span data-ttu-id="3f94b-323">O controle é transferido para a instrução inserida.</span><span class="sxs-lookup"><span data-stu-id="3f94b-323">Control is transferred to the embedded statement.</span></span>
*  <span data-ttu-id="3f94b-324">Quando e se o controle atingir o ponto de extremidade da instrução inserida (possivelmente da execução de uma instrução `continue`), o *Boolean_expression* ([expressões booleanas](expressions.md#boolean-expressions)) será avaliado.</span><span class="sxs-lookup"><span data-stu-id="3f94b-324">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), the *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span> <span data-ttu-id="3f94b-325">Se a expressão booliana produz `true`, o controle é transferido para o início `do` da instrução.</span><span class="sxs-lookup"><span data-stu-id="3f94b-325">If the boolean expression yields `true`, control is transferred to the beginning of the `do` statement.</span></span> <span data-ttu-id="3f94b-326">Caso contrário, o controle será transferido para o ponto de `do` extremidade da instrução.</span><span class="sxs-lookup"><span data-stu-id="3f94b-326">Otherwise, control is transferred to the end point of the `do` statement.</span></span>

<span data-ttu-id="3f94b-327">Dentro da instrução inserida de `do` uma instrução, `break` uma instrução ([a instrução break](statements.md#the-break-statement)) pode ser usada para transferir o `do` controle para o ponto final da instrução (encerrando assim a iteração da instrução inserida) e um `continue` a instrução ([a instrução Continue](statements.md#the-continue-statement)) pode ser usada para transferir o controle para o ponto final da instrução inserida.</span><span class="sxs-lookup"><span data-stu-id="3f94b-327">Within the embedded statement of a `do` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `do` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement.</span></span>

<span data-ttu-id="3f94b-328">A instrução inserida de `do` uma instrução pode ser acessada se a `do` instrução estiver acessível.</span><span class="sxs-lookup"><span data-stu-id="3f94b-328">The embedded statement of a `do` statement is reachable if the `do` statement is reachable.</span></span>

<span data-ttu-id="3f94b-329">O ponto de extremidade de `do` uma instrução pode ser acessado se pelo menos uma das seguintes opções for verdadeira:</span><span class="sxs-lookup"><span data-stu-id="3f94b-329">The end point of a `do` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="3f94b-330">A `do` instrução contém uma instrução `break` alcançável que sai da `do` instrução.</span><span class="sxs-lookup"><span data-stu-id="3f94b-330">The `do` statement contains a reachable `break` statement that exits the `do` statement.</span></span>
*  <span data-ttu-id="3f94b-331">O ponto de extremidade da instrução inserida pode ser acessado e a expressão booliana não `true`tem o valor constante.</span><span class="sxs-lookup"><span data-stu-id="3f94b-331">The end point of the embedded statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-for-statement"></a><span data-ttu-id="3f94b-332">A instrução for</span><span class="sxs-lookup"><span data-stu-id="3f94b-332">The for statement</span></span>

<span data-ttu-id="3f94b-333">A `for` instrução avalia uma sequência de expressões de inicialização e, em seguida, enquanto uma condição é verdadeira, executa repetidamente uma instrução inserida e avalia uma sequência de expressões de iteração.</span><span class="sxs-lookup"><span data-stu-id="3f94b-333">The `for` statement evaluates a sequence of initialization expressions and then, while a condition is true, repeatedly executes an embedded statement and evaluates a sequence of iteration expressions.</span></span>

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

<span data-ttu-id="3f94b-334">O *for_initializer*, se presente, consiste em um *local_variable_declaration* ([declarações de variável local](statements.md#local-variable-declarations)) ou uma lista de *statement_expression*s ([instruções de expressão](statements.md#expression-statements)) separadas por vírgulas.</span><span class="sxs-lookup"><span data-stu-id="3f94b-334">The *for_initializer*, if present, consists of either a *local_variable_declaration* ([Local variable declarations](statements.md#local-variable-declarations)) or a list of *statement_expression*s ([Expression statements](statements.md#expression-statements)) separated by commas.</span></span> <span data-ttu-id="3f94b-335">O escopo de uma variável local declarada por um *for_initializer* começa no *local_variable_declarator* para a variável e se estende ao final da instrução inserida.</span><span class="sxs-lookup"><span data-stu-id="3f94b-335">The scope of a local variable declared by a *for_initializer* starts at the *local_variable_declarator* for the variable and extends to the end of the embedded statement.</span></span> <span data-ttu-id="3f94b-336">O escopo inclui o *for_condition* e o *for_iterator*.</span><span class="sxs-lookup"><span data-stu-id="3f94b-336">The scope includes the *for_condition* and the *for_iterator*.</span></span>

<span data-ttu-id="3f94b-337">O *for_condition*, se presente, deve ser um *Boolean_expression* ([expressões booleanas](expressions.md#boolean-expressions)).</span><span class="sxs-lookup"><span data-stu-id="3f94b-337">The *for_condition*, if present, must be a *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)).</span></span>

<span data-ttu-id="3f94b-338">O *for_iterator*, se presente, consiste em uma lista de *statement_expression*s ([instruções de expressão](statements.md#expression-statements)) separadas por vírgulas.</span><span class="sxs-lookup"><span data-stu-id="3f94b-338">The *for_iterator*, if present, consists of a list of *statement_expression*s ([Expression statements](statements.md#expression-statements)) separated by commas.</span></span>

<span data-ttu-id="3f94b-339">Uma instrução for é executada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="3f94b-339">A for statement is executed as follows:</span></span>

*  <span data-ttu-id="3f94b-340">Se um *for_initializer* estiver presente, os inicializadores de variável ou as expressões de instrução serão executadas na ordem em que são gravadas.</span><span class="sxs-lookup"><span data-stu-id="3f94b-340">If a *for_initializer* is present, the variable initializers or statement expressions are executed in the order they are written.</span></span> <span data-ttu-id="3f94b-341">Esta etapa é executada apenas uma vez.</span><span class="sxs-lookup"><span data-stu-id="3f94b-341">This step is only performed once.</span></span>
*  <span data-ttu-id="3f94b-342">Se um *for_condition* estiver presente, ele será avaliado.</span><span class="sxs-lookup"><span data-stu-id="3f94b-342">If a *for_condition* is present, it is evaluated.</span></span>
*  <span data-ttu-id="3f94b-343">Se o *for_condition* não estiver presente ou se a avaliação resultar em `true`, o controle será transferido para a instrução inserida.</span><span class="sxs-lookup"><span data-stu-id="3f94b-343">If the *for_condition* is not present or if the evaluation yields `true`, control is transferred to the embedded statement.</span></span> <span data-ttu-id="3f94b-344">Quando e se o controle atingir o ponto de extremidade da instrução inserida (possivelmente da execução de uma instrução `continue`), as expressões de *for_iterator*, se houver, serão avaliadas em sequência e outra iteração será executada, começando com avaliação do *for_condition* na etapa anterior.</span><span class="sxs-lookup"><span data-stu-id="3f94b-344">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), the expressions of the *for_iterator*, if any, are evaluated in sequence, and then another iteration is performed, starting with evaluation of the *for_condition* in the step above.</span></span>
*  <span data-ttu-id="3f94b-345">Se o *for_condition* estiver presente e a avaliação produzir `false`, o controle será transferido para o ponto de extremidade da instrução `for`.</span><span class="sxs-lookup"><span data-stu-id="3f94b-345">If the *for_condition* is present and the evaluation yields `false`, control is transferred to the end point of the `for` statement.</span></span>

<span data-ttu-id="3f94b-346">Dentro da instrução inserida de uma instrução `for`, uma instrução `break` ([a instrução break](statements.md#the-break-statement)) pode ser usada para transferir o controle para o ponto final da instrução `for` (encerrando a iteração da instrução inserida) e uma instrução `continue` ([ A instrução Continue](statements.md#the-continue-statement)) pode ser usada para transferir o controle para o ponto final da instrução incorporada (executando, assim, a *for_iterator* e executando outra iteração da instrução `for`, começando com o *for_condition*).</span><span class="sxs-lookup"><span data-stu-id="3f94b-346">Within the embedded statement of a `for` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `for` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement (thus executing the *for_iterator* and performing another iteration of the `for` statement, starting with the *for_condition*).</span></span>

<span data-ttu-id="3f94b-347">A instrução inserida de `for` uma instrução pode ser acessada se uma das seguintes opções for verdadeira:</span><span class="sxs-lookup"><span data-stu-id="3f94b-347">The embedded statement of a `for` statement is reachable if one of the following is true:</span></span>

*  <span data-ttu-id="3f94b-348">A instrução `for` está acessível e nenhum *for_condition* está presente.</span><span class="sxs-lookup"><span data-stu-id="3f94b-348">The `for` statement is reachable and no *for_condition* is present.</span></span>
*  <span data-ttu-id="3f94b-349">A instrução `for` está acessível e um *for_condition* está presente e não tem o valor constante `false`.</span><span class="sxs-lookup"><span data-stu-id="3f94b-349">The `for` statement is reachable and a *for_condition* is present and does not have the constant value `false`.</span></span>

<span data-ttu-id="3f94b-350">O ponto de extremidade de `for` uma instrução pode ser acessado se pelo menos uma das seguintes opções for verdadeira:</span><span class="sxs-lookup"><span data-stu-id="3f94b-350">The end point of a `for` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="3f94b-351">A `for` instrução contém uma instrução `break` alcançável que sai da `for` instrução.</span><span class="sxs-lookup"><span data-stu-id="3f94b-351">The `for` statement contains a reachable `break` statement that exits the `for` statement.</span></span>
*  <span data-ttu-id="3f94b-352">A instrução `for` está acessível e um *for_condition* está presente e não tem o valor constante `true`.</span><span class="sxs-lookup"><span data-stu-id="3f94b-352">The `for` statement is reachable and a *for_condition* is present and does not have the constant value `true`.</span></span>

### <a name="the-foreach-statement"></a><span data-ttu-id="3f94b-353">A instrução foreach</span><span class="sxs-lookup"><span data-stu-id="3f94b-353">The foreach statement</span></span>

<span data-ttu-id="3f94b-354">A `foreach` instrução enumera os elementos de uma coleção, executando uma instrução incorporada para cada elemento da coleção.</span><span class="sxs-lookup"><span data-stu-id="3f94b-354">The `foreach` statement enumerates the elements of a collection, executing an embedded statement for each element of the collection.</span></span>

```antlr
foreach_statement
    : 'foreach' '(' local_variable_type identifier 'in' expression ')' embedded_statement
    ;
```

<span data-ttu-id="3f94b-355">O *tipo* e o *identificador* de `foreach` uma instrução declaram a ***variável de iteração*** da instrução.</span><span class="sxs-lookup"><span data-stu-id="3f94b-355">The *type* and *identifier* of a `foreach` statement declare the ***iteration variable*** of the statement.</span></span> <span data-ttu-id="3f94b-356">Se o identificador `var` for fornecido como *local_variable_type*e nenhum tipo chamado `var` estiver no escopo, a variável de iteração será considerada uma variável de ***iteração digitada implicitamente***e seu tipo será usado para ser o tipo de elemento do `foreach` , conforme especificado abaixo.</span><span class="sxs-lookup"><span data-stu-id="3f94b-356">If the `var` identifier is given as the *local_variable_type*, and no type named `var` is in scope, the iteration variable is said to be an ***implicitly typed iteration variable***, and its type is taken to be the element type of the `foreach` statement, as specified below.</span></span> <span data-ttu-id="3f94b-357">A variável de iteração corresponde a uma variável local somente leitura com um escopo que se estende pela instrução incorporada.</span><span class="sxs-lookup"><span data-stu-id="3f94b-357">The iteration variable corresponds to a read-only local variable with a scope that extends over the embedded statement.</span></span> <span data-ttu-id="3f94b-358">Durante a execução de `foreach` uma instrução, a variável de iteração representa o elemento de coleção para o qual uma iteração está sendo executada no momento.</span><span class="sxs-lookup"><span data-stu-id="3f94b-358">During execution of a `foreach` statement, the iteration variable represents the collection element for which an iteration is currently being performed.</span></span> <span data-ttu-id="3f94b-359">Ocorrerá um erro de tempo de compilação se a instrução inserida tentar modificar a variável de iteração ( `++` por `--` meio de atribuição ou os operadores e) ou `ref` passar `out` a variável de iteração como um parâmetro ou.</span><span class="sxs-lookup"><span data-stu-id="3f94b-359">A compile-time error occurs if the embedded statement attempts to modify the iteration variable (via assignment or the `++` and `--` operators) or pass the iteration variable as a `ref` or `out` parameter.</span></span>

<span data-ttu-id="3f94b-360">A seguir, para fins de brevidade `IEnumerable`, `IEnumerator` `IEnumerable<T>` , e `IEnumerator<T>` consulte os tipos correspondentes nos namespaces `System.Collections` e `System.Collections.Generic`.</span><span class="sxs-lookup"><span data-stu-id="3f94b-360">In the following, for brevity, `IEnumerable`, `IEnumerator`, `IEnumerable<T>` and `IEnumerator<T>` refer to the corresponding types in the namespaces `System.Collections` and `System.Collections.Generic`.</span></span>

<span data-ttu-id="3f94b-361">O processamento em tempo de compilação de uma instrução foreach primeiro determina o ***tipo de coleção***, o tipo de ***enumerador*** e o ***tipo de elemento*** da expressão.</span><span class="sxs-lookup"><span data-stu-id="3f94b-361">The compile-time processing of a foreach statement first determines the ***collection type***, ***enumerator type*** and ***element type*** of the expression.</span></span> <span data-ttu-id="3f94b-362">Essa determinação continua da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="3f94b-362">This determination proceeds as follows:</span></span>

*  <span data-ttu-id="3f94b-363">Se o tipo `X` de *expressão* for um tipo de matriz, haverá uma conversão de referência implícita `X` de para `IEnumerable` a interface ( `System.Array` desde que implementa essa interface).</span><span class="sxs-lookup"><span data-stu-id="3f94b-363">If the type `X` of *expression* is an array type then there is an implicit reference conversion from `X` to the `IEnumerable` interface (since `System.Array` implements this interface).</span></span> <span data-ttu-id="3f94b-364">O ***tipo de coleção*** é `IEnumerable` a interface, ***o tipo de enumerador*** é `IEnumerator` a interface e o tipo de ***elemento*** é o tipo de `X`elemento do tipo de matriz.</span><span class="sxs-lookup"><span data-stu-id="3f94b-364">The ***collection type*** is the `IEnumerable` interface, the ***enumerator type*** is the `IEnumerator` interface and the ***element type*** is the element type of the array type `X`.</span></span>
*  <span data-ttu-id="3f94b-365">Se o tipo `X` de *expressão* for `dynamic` , haverá uma conversão implícita de *expression* para a `IEnumerable` interface ([conversões dinâmicas implícitas](conversions.md#implicit-dynamic-conversions)).</span><span class="sxs-lookup"><span data-stu-id="3f94b-365">If the type `X` of *expression* is `dynamic` then there is an implicit conversion from *expression* to the `IEnumerable` interface ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions)).</span></span> <span data-ttu-id="3f94b-366">O ***tipo de coleção*** é `IEnumerable` a interface e o ***tipo de enumerador*** é a `IEnumerator` interface.</span><span class="sxs-lookup"><span data-stu-id="3f94b-366">The ***collection type*** is the `IEnumerable` interface and the ***enumerator type*** is the `IEnumerator` interface.</span></span> <span data-ttu-id="3f94b-367">Se o identificador `var` for fornecido como o *local_variable_type* , o ***tipo de elemento*** será `dynamic`, caso contrário, será `object`.</span><span class="sxs-lookup"><span data-stu-id="3f94b-367">If the `var` identifier is given as the *local_variable_type* then the ***element type*** is `dynamic`, otherwise it is `object`.</span></span>
*  <span data-ttu-id="3f94b-368">Caso contrário, determine se o `X` tipo tem um `GetEnumerator` método apropriado:</span><span class="sxs-lookup"><span data-stu-id="3f94b-368">Otherwise, determine whether the type `X` has an appropriate `GetEnumerator` method:</span></span>
   * <span data-ttu-id="3f94b-369">Executar pesquisa de membro no tipo `X` com identificador `GetEnumerator` e nenhum argumento de tipo.</span><span class="sxs-lookup"><span data-stu-id="3f94b-369">Perform member lookup on the type `X` with identifier `GetEnumerator` and no type arguments.</span></span> <span data-ttu-id="3f94b-370">Se a pesquisa de membro não produzir uma correspondência ou se gerar uma ambiguidade ou produzir uma correspondência que não seja um grupo de métodos, verifique se há uma interface enumerável, conforme descrito abaixo.</span><span class="sxs-lookup"><span data-stu-id="3f94b-370">If the member lookup does not produce a match, or it produces an ambiguity, or produces a match that is not a method group, check for an enumerable interface as described below.</span></span> <span data-ttu-id="3f94b-371">É recomendável que um aviso seja emitido se a pesquisa de membros produzir qualquer coisa, exceto um grupo de métodos ou nenhuma correspondência.</span><span class="sxs-lookup"><span data-stu-id="3f94b-371">It is recommended that a warning be issued if member lookup produces anything except a method group or no match.</span></span>
   * <span data-ttu-id="3f94b-372">Execute a resolução de sobrecarga usando o grupo de métodos resultante e uma lista de argumentos vazia.</span><span class="sxs-lookup"><span data-stu-id="3f94b-372">Perform overload resolution using the resulting method group and an empty argument list.</span></span> <span data-ttu-id="3f94b-373">Se a resolução de sobrecarga não resultar em métodos aplicáveis, resultar em uma ambiguidade ou resultar em um único método melhor, mas o método for estático ou não público, verifique se há uma interface enumerável, conforme descrito abaixo.</span><span class="sxs-lookup"><span data-stu-id="3f94b-373">If overload resolution results in no applicable methods, results in an ambiguity, or results in a single best method but that method is either static or not public, check for an enumerable interface as described below.</span></span> <span data-ttu-id="3f94b-374">É recomendável que um aviso seja emitido se a resolução de sobrecarga produzir algo, exceto um método de instância pública não ambígua, ou nenhum dos métodos aplicáveis.</span><span class="sxs-lookup"><span data-stu-id="3f94b-374">It is recommended that a warning be issued if overload resolution produces anything except an unambiguous public instance method or no applicable methods.</span></span>
   * <span data-ttu-id="3f94b-375">Se o tipo `E` `GetEnumerator` de retorno do método não for uma classe, struct ou tipo de interface, um erro será produzido e nenhuma etapa adicional será executada.</span><span class="sxs-lookup"><span data-stu-id="3f94b-375">If the return type `E` of the `GetEnumerator` method is not a class, struct or interface type, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="3f94b-376">A pesquisa de membros é `E` executada em com `Current` o identificador e nenhum argumento de tipo.</span><span class="sxs-lookup"><span data-stu-id="3f94b-376">Member lookup is performed on `E` with the identifier `Current` and no type arguments.</span></span> <span data-ttu-id="3f94b-377">Se a pesquisa de membro não produzir nenhuma correspondência, o resultado é um erro ou o resultado é qualquer coisa, exceto uma propriedade de instância pública que permite a leitura, um erro é produzido e nenhuma etapa adicional é executada.</span><span class="sxs-lookup"><span data-stu-id="3f94b-377">If the member lookup produces no match, the result is an error, or the result is anything except a public instance property that permits reading, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="3f94b-378">A pesquisa de membros é `E` executada em com `MoveNext` o identificador e nenhum argumento de tipo.</span><span class="sxs-lookup"><span data-stu-id="3f94b-378">Member lookup is performed on `E` with the identifier `MoveNext` and no type arguments.</span></span> <span data-ttu-id="3f94b-379">Se a pesquisa de membro não produzir nenhuma correspondência, o resultado é um erro ou o resultado é qualquer coisa, exceto um grupo de métodos, um erro é produzido e nenhuma etapa adicional é executada.</span><span class="sxs-lookup"><span data-stu-id="3f94b-379">If the member lookup produces no match, the result is an error, or the result is anything except a method group, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="3f94b-380">A resolução de sobrecarga é executada no grupo de métodos com uma lista de argumentos vazia.</span><span class="sxs-lookup"><span data-stu-id="3f94b-380">Overload resolution is performed on the method group with an empty argument list.</span></span> <span data-ttu-id="3f94b-381">Se a resolução de sobrecarga não resultar em métodos aplicáveis, resultar em uma ambiguidade ou resultar em um único método melhor, mas esse método for estático ou não público, ou seu tipo de retorno não `bool`for, um erro será produzido e nenhuma etapa adicional será executada.</span><span class="sxs-lookup"><span data-stu-id="3f94b-381">If overload resolution results in no applicable methods, results in an ambiguity, or results in a single best method but that method is either static or not public, or its return type is not `bool`, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="3f94b-382">O ***tipo*** de coleção `X`é, o tipo de `E` ***enumerador*** é, `Current` e o ***tipo de elemento*** é o tipo da propriedade.</span><span class="sxs-lookup"><span data-stu-id="3f94b-382">The ***collection type*** is `X`, the ***enumerator type*** is `E`, and the ***element type*** is the type of the `Current` property.</span></span>

*  <span data-ttu-id="3f94b-383">Caso contrário, verifique se há uma interface enumerável:</span><span class="sxs-lookup"><span data-stu-id="3f94b-383">Otherwise, check for an enumerable interface:</span></span>
   * <span data-ttu-id="3f94b-384">Se entre todos os tipos `Ti` para os quais há uma conversão implícita de `X` para `IEnumerable<Ti>`, há um tipo `T` exclusivo, que `T` não `dynamic` é e para todos os outros `Ti` , há um conversão implícita de `IEnumerable<T>` para `IEnumerable<Ti>`, o ***tipo de coleção*** é a interface `IEnumerable<T>`, o ***tipo de enumerador*** é `IEnumerator<T>`a interface e o ***tipo de elemento*** é `T`.</span><span class="sxs-lookup"><span data-stu-id="3f94b-384">If among all the types `Ti` for which there is an implicit conversion from `X` to `IEnumerable<Ti>`, there is a unique type `T` such that `T` is not `dynamic` and for all the other `Ti` there is an implicit conversion from `IEnumerable<T>` to `IEnumerable<Ti>`, then the ***collection type*** is the interface `IEnumerable<T>`, the ***enumerator type*** is the interface `IEnumerator<T>`, and the ***element type*** is `T`.</span></span>
   * <span data-ttu-id="3f94b-385">Caso contrário, se houver mais de um desses tipos `T`, um erro será produzido e nenhuma etapa adicional será executada.</span><span class="sxs-lookup"><span data-stu-id="3f94b-385">Otherwise, if there is more than one such type `T`, then an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="3f94b-386">Caso contrário, se houver uma conversão implícita de `X` `System.Collections.IEnumerable` na interface, o tipo de ***coleção*** será essa interface, o ***tipo de enumerador*** será a `System.Collections.IEnumerator`interface e o ***tipo de elemento*** será `object`.</span><span class="sxs-lookup"><span data-stu-id="3f94b-386">Otherwise, if there is an implicit conversion from `X` to the `System.Collections.IEnumerable` interface, then the ***collection type*** is this interface, the ***enumerator type*** is the interface `System.Collections.IEnumerator`, and the ***element type*** is `object`.</span></span>
   * <span data-ttu-id="3f94b-387">Caso contrário, um erro é produzido e nenhuma etapa adicional é executada.</span><span class="sxs-lookup"><span data-stu-id="3f94b-387">Otherwise, an error is produced and no further steps are taken.</span></span>

<span data-ttu-id="3f94b-388">As etapas acima, se forem bem-sucedidas, produzirão um tipo `C`de coleção, tipo `E` de enumerador e `T`tipo de elemento de forma inequívoca.</span><span class="sxs-lookup"><span data-stu-id="3f94b-388">The above steps, if successful, unambiguously produce a collection type `C`, enumerator type `E` and element type `T`.</span></span> <span data-ttu-id="3f94b-389">Uma instrução foreach do formulário</span><span class="sxs-lookup"><span data-stu-id="3f94b-389">A foreach statement of the form</span></span>
```csharp
foreach (V v in x) embedded_statement
```
<span data-ttu-id="3f94b-390">é então expandido para:</span><span class="sxs-lookup"><span data-stu-id="3f94b-390">is then expanded to:</span></span>
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

<span data-ttu-id="3f94b-391">A variável `e` não é visível ou acessível à expressão `x` ou à instrução incorporada ou a qualquer outro código-fonte do programa.</span><span class="sxs-lookup"><span data-stu-id="3f94b-391">The variable `e` is not visible to or accessible to the expression `x` or the embedded statement or any other source code of the program.</span></span> <span data-ttu-id="3f94b-392">A variável `v` é somente leitura na instrução incorporada.</span><span class="sxs-lookup"><span data-stu-id="3f94b-392">The variable `v` is read-only in the embedded statement.</span></span> <span data-ttu-id="3f94b-393">Se não houver uma conversão explícita ([conversões explícitas](conversions.md#explicit-conversions)) do `T` (o tipo de elemento) para `V` (o *local_variable_type* na instrução foreach), um erro será produzido e nenhuma etapa adicional será executada.</span><span class="sxs-lookup"><span data-stu-id="3f94b-393">If there is not an explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) from `T` (the element type) to `V` (the *local_variable_type* in the foreach statement), an error is produced and no further steps are taken.</span></span> <span data-ttu-id="3f94b-394">Se `x` tiver o valor `null`, um `System.NullReferenceException` será lançado em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="3f94b-394">If `x` has the value `null`, a `System.NullReferenceException` is thrown at run-time.</span></span>

<span data-ttu-id="3f94b-395">Uma implementação tem permissão para implementar uma determinada instrução foreach de forma diferente, por exemplo, por motivos de desempenho, desde que o comportamento seja consistente com a expansão acima.</span><span class="sxs-lookup"><span data-stu-id="3f94b-395">An implementation is permitted to implement a given foreach-statement differently, e.g. for performance reasons, as long as the behavior is consistent with the above expansion.</span></span>

<span data-ttu-id="3f94b-396">O posicionamento de `v` dentro do loop while é importante para a forma como ele é capturado por qualquer função anônima que ocorra no *embedded_statement*.</span><span class="sxs-lookup"><span data-stu-id="3f94b-396">The placement of `v` inside the while loop is important for how it is captured by any anonymous function occurring in the *embedded_statement*.</span></span>

<span data-ttu-id="3f94b-397">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="3f94b-397">For example:</span></span>
```csharp
int[] values = { 7, 9, 13 };
Action f = null;

foreach (var value in values)
{
    if (f == null) f = () => Console.WriteLine("First value: " + value);
}

f();
```
<span data-ttu-id="3f94b-398">Se `v` foi declarado fora do loop while, ele seria compartilhado entre todas as iterações e seu valor após o loop for seria o valor final, `13`, que é o `f` que a invocação imprimiria.</span><span class="sxs-lookup"><span data-stu-id="3f94b-398">If `v` was declared outside of the while loop, it would be shared among all iterations, and its value after the for loop would be the final value, `13`, which is what the invocation of `f` would print.</span></span> <span data-ttu-id="3f94b-399">Em vez disso, como cada iteração tem `v`sua própria variável, aquela `f` capturada pela primeira iteração continuará a manter `7`o valor, que é o que será impresso.</span><span class="sxs-lookup"><span data-stu-id="3f94b-399">Instead, because each iteration has its own variable `v`, the one captured by `f` in the first iteration will continue to hold the value `7`, which is what will be printed.</span></span> <span data-ttu-id="3f94b-400">(Observação: versões anteriores do C# declaradas `v` fora do loop While.)</span><span class="sxs-lookup"><span data-stu-id="3f94b-400">(Note: earlier versions of C# declared `v` outside of the while loop.)</span></span>

<span data-ttu-id="3f94b-401">O corpo do bloco finally é construído de acordo com as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="3f94b-401">The body of the finally block is constructed according to the following steps:</span></span>

*  <span data-ttu-id="3f94b-402">Se houver uma conversão implícita do `E` `System.IDisposable` na interface, então</span><span class="sxs-lookup"><span data-stu-id="3f94b-402">If there is an implicit conversion from `E` to the `System.IDisposable` interface, then</span></span>
   *  <span data-ttu-id="3f94b-403">Se `E` for um tipo de valor não anulável, a cláusula finally será expandida para o equivalente semântico de:</span><span class="sxs-lookup"><span data-stu-id="3f94b-403">If `E` is a non-nullable value type then the finally clause is expanded to the semantic equivalent  of:</span></span>

      ```csharp
      finally {
          ((System.IDisposable)e).Dispose();
      }
      ```

   *  <span data-ttu-id="3f94b-404">Caso contrário, a cláusula finally será expandida para o equivalente semântico de:</span><span class="sxs-lookup"><span data-stu-id="3f94b-404">Otherwise the finally clause is expanded to the semantic equivalent of:</span></span>

      ```csharp
      finally {
          if (e != null) ((System.IDisposable)e).Dispose();
      }
      ```

   <span data-ttu-id="3f94b-405">Exceto que se `E` for um tipo de valor ou um parâmetro de tipo instanciado para um tipo de valor, a conversão `e` de `System.IDisposable` para não fará com que a Boxing ocorra.</span><span class="sxs-lookup"><span data-stu-id="3f94b-405">except that if `E` is a value type, or a type parameter instantiated to a value type, then the cast of `e` to `System.IDisposable` will not cause boxing to occur.</span></span>

*  <span data-ttu-id="3f94b-406">Caso contrário, `E` se for um tipo lacrado, a cláusula finally será expandida para um bloco vazio:</span><span class="sxs-lookup"><span data-stu-id="3f94b-406">Otherwise, if `E` is a sealed type, the finally clause is expanded to an empty block:</span></span>

   ```csharp
   finally {
   }
   ```

*  <span data-ttu-id="3f94b-407">Caso contrário, a cláusula finally será expandida para:</span><span class="sxs-lookup"><span data-stu-id="3f94b-407">Otherwise, the finally clause is expanded to:</span></span>

   ```csharp
   finally {
       System.IDisposable d = e as System.IDisposable;
       if (d != null) d.Dispose();
   }
   ```    

   <span data-ttu-id="3f94b-408">A variável `d` local não é visível ou acessível a nenhum código de usuário.</span><span class="sxs-lookup"><span data-stu-id="3f94b-408">The local variable `d` is not visible to or accessible to any user code.</span></span> <span data-ttu-id="3f94b-409">Em particular, ele não entra em conflito com nenhuma outra variável cujo escopo inclui o bloco finally.</span><span class="sxs-lookup"><span data-stu-id="3f94b-409">In particular, it does not conflict with any other variable whose scope includes the finally block.</span></span>

<span data-ttu-id="3f94b-410">A ordem na qual `foreach` percorre os elementos de uma matriz é a seguinte: Para elementos de matrizes unidimensionais são atravessados em ordem de índice crescente, `0` começando com index e `Length - 1`terminando com index.</span><span class="sxs-lookup"><span data-stu-id="3f94b-410">The order in which `foreach` traverses the elements of an array, is as follows: For single-dimensional arrays elements are traversed in increasing index order, starting with index `0` and ending with index `Length - 1`.</span></span> <span data-ttu-id="3f94b-411">Para matrizes multidimensionais, os elementos são percorridos de forma que os índices da dimensão mais à direita sejam aumentados primeiro, depois a dimensão à esquerda e assim por diante à esquerda.</span><span class="sxs-lookup"><span data-stu-id="3f94b-411">For multi-dimensional arrays, elements are traversed such that the indices of the rightmost dimension are increased first, then the next left dimension, and so on to the left.</span></span>

<span data-ttu-id="3f94b-412">O exemplo a seguir imprime cada valor em uma matriz bidimensional, na ordem do elemento:</span><span class="sxs-lookup"><span data-stu-id="3f94b-412">The following example prints out each value in a two-dimensional array, in element order:</span></span>
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
<span data-ttu-id="3f94b-413">A saída produzida é a seguinte:</span><span class="sxs-lookup"><span data-stu-id="3f94b-413">The output produced is as follows:</span></span>
```console
1.2 2.3 3.4 4.5 5.6 6.7 7.8 8.9
```

<span data-ttu-id="3f94b-414">No exemplo</span><span class="sxs-lookup"><span data-stu-id="3f94b-414">In the example</span></span>
```csharp
int[] numbers = { 1, 3, 5, 7, 9 };
foreach (var n in numbers) Console.WriteLine(n);
```
<span data-ttu-id="3f94b-415">o tipo de `n` é inferido `int`como, o tipo de elemento de `numbers`.</span><span class="sxs-lookup"><span data-stu-id="3f94b-415">the type of `n` is inferred to be `int`, the element type of `numbers`.</span></span>

## <a name="jump-statements"></a><span data-ttu-id="3f94b-416">Instruções de atalho</span><span class="sxs-lookup"><span data-stu-id="3f94b-416">Jump statements</span></span>

<span data-ttu-id="3f94b-417">As instruções de salto transferem o controle incondicionalmente.</span><span class="sxs-lookup"><span data-stu-id="3f94b-417">Jump statements unconditionally transfer control.</span></span>

```antlr
jump_statement
    : break_statement
    | continue_statement
    | goto_statement
    | return_statement
    | throw_statement
    ;
```

<span data-ttu-id="3f94b-418">O local para o qual uma instrução de salto transfere o controle é chamado de ***destino*** da instrução de salto.</span><span class="sxs-lookup"><span data-stu-id="3f94b-418">The location to which a jump statement transfers control is called the ***target*** of the jump statement.</span></span>

<span data-ttu-id="3f94b-419">Quando uma instrução de salto ocorre dentro de um bloco e o destino dessa instrução de salto está fora desse bloco, a instrução de salto é chamada para ***sair*** do bloco.</span><span class="sxs-lookup"><span data-stu-id="3f94b-419">When a jump statement occurs within a block, and the target of that jump statement is outside that block, the jump statement is said to ***exit*** the block.</span></span> <span data-ttu-id="3f94b-420">Embora uma instrução de salto possa transferir o controle para fora de um bloco, ela nunca pode transferir o controle para um bloco.</span><span class="sxs-lookup"><span data-stu-id="3f94b-420">While a jump statement may transfer control out of a block, it can never transfer control into a block.</span></span>

<span data-ttu-id="3f94b-421">A execução de instruções de salto é complicada pela presença de `try` instruções intermediárias.</span><span class="sxs-lookup"><span data-stu-id="3f94b-421">Execution of jump statements is complicated by the presence of intervening `try` statements.</span></span> <span data-ttu-id="3f94b-422">Na ausência de tais `try` instruções, uma instrução de salto transfere incondicionalmente o controle da instrução de salto para seu destino.</span><span class="sxs-lookup"><span data-stu-id="3f94b-422">In the absence of such `try` statements, a jump statement unconditionally transfers control from the jump statement to its target.</span></span> <span data-ttu-id="3f94b-423">Na presença dessas `try` instruções intermediárias, a execução é mais complexa.</span><span class="sxs-lookup"><span data-stu-id="3f94b-423">In the presence of such intervening `try` statements, execution is more complex.</span></span> <span data-ttu-id="3f94b-424">Se a instrução de salto sair de um ou `try` mais blocos com `finally` blocos associados, o controle será transferido inicialmente `finally` para o bloco da `try` instrução mais interna.</span><span class="sxs-lookup"><span data-stu-id="3f94b-424">If the jump statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="3f94b-425">Quando e se o controle atingir o ponto de extremidade `finally` de um bloco, o controle será `finally` transferido para o bloco da próxima `try` instrução de circunscrição.</span><span class="sxs-lookup"><span data-stu-id="3f94b-425">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="3f94b-426">Esse processo é repetido até `finally` que os blocos de todas `try` as instruções intermediárias tenham sido executados.</span><span class="sxs-lookup"><span data-stu-id="3f94b-426">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>

<span data-ttu-id="3f94b-427">No exemplo</span><span class="sxs-lookup"><span data-stu-id="3f94b-427">In the example</span></span>
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
<span data-ttu-id="3f94b-428">os `finally` blocos associados a duas `try` instruções são executados antes de o controle ser transferido para o destino da instrução de salto.</span><span class="sxs-lookup"><span data-stu-id="3f94b-428">the `finally` blocks associated with two `try` statements are executed before control is transferred to the target of the jump statement.</span></span>

<span data-ttu-id="3f94b-429">A saída produzida é a seguinte:</span><span class="sxs-lookup"><span data-stu-id="3f94b-429">The output produced is as follows:</span></span>
```console
Before break
Innermost finally block
Outermost finally block
After break
```

### <a name="the-break-statement"></a><span data-ttu-id="3f94b-430">A instrução break</span><span class="sxs-lookup"><span data-stu-id="3f94b-430">The break statement</span></span>

<span data-ttu-id="3f94b-431">A `break` instrução sai da instrução `while` `switch`, `do`,, ou`foreach` delimitadora mais próxima. `for`</span><span class="sxs-lookup"><span data-stu-id="3f94b-431">The `break` statement exits the nearest enclosing `switch`, `while`, `do`, `for`, or `foreach` statement.</span></span>

```antlr
break_statement
    : 'break' ';'
    ;
```

<span data-ttu-id="3f94b-432">O destino de uma `break` instrução é o ponto final da instrução, `do` `while`,, `for`ou `switch` `foreach` delimitadora mais próxima.</span><span class="sxs-lookup"><span data-stu-id="3f94b-432">The target of a `break` statement is the end point of the nearest enclosing `switch`, `while`, `do`, `for`, or `foreach` statement.</span></span> <span data-ttu-id="3f94b-433">Se uma `break` instrução não estiver entre uma `switch`instrução, `while` `do`,, `for`ou `foreach` , ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="3f94b-433">If a `break` statement is not enclosed by a `switch`, `while`, `do`, `for`, or `foreach` statement, a compile-time error occurs.</span></span>

<span data-ttu-id="3f94b-434">Quando várias `switch`instruções `while`, `do`, ,`for` ou`foreach` são aninhadas umas nas outras, uma `break` instrução aplica-se somente à instrução mais interna.</span><span class="sxs-lookup"><span data-stu-id="3f94b-434">When multiple `switch`, `while`, `do`, `for`, or `foreach` statements are nested within each other, a `break` statement applies only to the innermost statement.</span></span> <span data-ttu-id="3f94b-435">Para transferir o controle entre vários níveis de aninhamento `goto` , uma instrução ([instrução goto](statements.md#the-goto-statement)) deve ser usada.</span><span class="sxs-lookup"><span data-stu-id="3f94b-435">To transfer control across multiple nesting levels, a `goto` statement ([The goto statement](statements.md#the-goto-statement)) must be used.</span></span>

<span data-ttu-id="3f94b-436">Uma `break` instrução não pode sair `finally` de um bloco ([a instrução try](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="3f94b-436">A `break` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="3f94b-437">Quando uma `break` instrução ocorre dentro de `finally` um bloco `break` , o destino da instrução deve estar dentro do mesmo `finally` bloco; caso contrário, ocorrerá um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="3f94b-437">When a `break` statement occurs within a `finally` block, the target of the `break` statement must be within the same `finally` block; otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="3f94b-438">Uma `break` instrução é executada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="3f94b-438">A `break` statement is executed as follows:</span></span>

*  <span data-ttu-id="3f94b-439">Se a `break` instrução sair de um ou mais `try` blocos com blocos `finally` associados, o controle será transferido inicialmente para `finally` o bloco da instrução `try` mais interna.</span><span class="sxs-lookup"><span data-stu-id="3f94b-439">If the `break` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="3f94b-440">Quando e se o controle atingir o ponto de extremidade `finally` de um bloco, o controle será `finally` transferido para o bloco da próxima `try` instrução de circunscrição.</span><span class="sxs-lookup"><span data-stu-id="3f94b-440">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="3f94b-441">Esse processo é repetido até `finally` que os blocos de todas `try` as instruções intermediárias tenham sido executados.</span><span class="sxs-lookup"><span data-stu-id="3f94b-441">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="3f94b-442">O controle é transferido para o destino da `break` instrução.</span><span class="sxs-lookup"><span data-stu-id="3f94b-442">Control is transferred to the target of the `break` statement.</span></span>

<span data-ttu-id="3f94b-443">Como uma `break` instrução transfere incondicionalmente o controle em outro lugar, o `break` ponto de extremidade de uma instrução nunca é acessível.</span><span class="sxs-lookup"><span data-stu-id="3f94b-443">Because a `break` statement unconditionally transfers control elsewhere, the end point of a `break` statement is never reachable.</span></span>

### <a name="the-continue-statement"></a><span data-ttu-id="3f94b-444">A instrução Continue</span><span class="sxs-lookup"><span data-stu-id="3f94b-444">The continue statement</span></span>

<span data-ttu-id="3f94b-445">A `continue` instrução inicia uma nova iteração da instrução,, ou `while` `do` `foreach` delimitadora `for`mais próxima.</span><span class="sxs-lookup"><span data-stu-id="3f94b-445">The `continue` statement starts a new iteration of the nearest enclosing `while`, `do`, `for`, or `foreach` statement.</span></span>

```antlr
continue_statement
    : 'continue' ';'
    ;
```

<span data-ttu-id="3f94b-446">O destino de uma `continue` instrução é o ponto final da instrução inserida da instrução, `do`, `for`ou `foreach` delimitadora `while`mais próxima.</span><span class="sxs-lookup"><span data-stu-id="3f94b-446">The target of a `continue` statement is the end point of the embedded statement of the nearest enclosing `while`, `do`, `for`, or `foreach` statement.</span></span> <span data-ttu-id="3f94b-447">Se uma `continue` instrução não estiver delimitada por `while`uma `do`instrução `for`,, `foreach` ou, ocorrerá um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="3f94b-447">If a `continue` statement is not enclosed by a `while`, `do`, `for`, or `foreach` statement, a compile-time error occurs.</span></span>

<span data-ttu-id="3f94b-448">Quando várias `while`instruções `do`, `for`, `continue` ou `foreach` são aninhadas umas nas outras, uma instrução aplica-se somente à instrução mais interna.</span><span class="sxs-lookup"><span data-stu-id="3f94b-448">When multiple `while`, `do`, `for`, or `foreach` statements are nested within each other, a `continue` statement applies only to the innermost statement.</span></span> <span data-ttu-id="3f94b-449">Para transferir o controle entre vários níveis de aninhamento `goto` , uma instrução ([instrução goto](statements.md#the-goto-statement)) deve ser usada.</span><span class="sxs-lookup"><span data-stu-id="3f94b-449">To transfer control across multiple nesting levels, a `goto` statement ([The goto statement](statements.md#the-goto-statement)) must be used.</span></span>

<span data-ttu-id="3f94b-450">Uma `continue` instrução não pode sair `finally` de um bloco ([a instrução try](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="3f94b-450">A `continue` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="3f94b-451">Quando uma `continue` instrução ocorre dentro de `finally` um bloco `continue` , o destino da instrução deve estar dentro do mesmo `finally` bloco; caso contrário, ocorrerá um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="3f94b-451">When a `continue` statement occurs within a `finally` block, the target of the `continue` statement must be within the same `finally` block; otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="3f94b-452">Uma `continue` instrução é executada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="3f94b-452">A `continue` statement is executed as follows:</span></span>

*  <span data-ttu-id="3f94b-453">Se a `continue` instrução sair de um ou mais `try` blocos com blocos `finally` associados, o controle será transferido inicialmente para `finally` o bloco da instrução `try` mais interna.</span><span class="sxs-lookup"><span data-stu-id="3f94b-453">If the `continue` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="3f94b-454">Quando e se o controle atingir o ponto de extremidade `finally` de um bloco, o controle será `finally` transferido para o bloco da próxima `try` instrução de circunscrição.</span><span class="sxs-lookup"><span data-stu-id="3f94b-454">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="3f94b-455">Esse processo é repetido até `finally` que os blocos de todas `try` as instruções intermediárias tenham sido executados.</span><span class="sxs-lookup"><span data-stu-id="3f94b-455">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="3f94b-456">O controle é transferido para o destino da `continue` instrução.</span><span class="sxs-lookup"><span data-stu-id="3f94b-456">Control is transferred to the target of the `continue` statement.</span></span>

<span data-ttu-id="3f94b-457">Como uma `continue` instrução transfere incondicionalmente o controle em outro lugar, o `continue` ponto de extremidade de uma instrução nunca é acessível.</span><span class="sxs-lookup"><span data-stu-id="3f94b-457">Because a `continue` statement unconditionally transfers control elsewhere, the end point of a `continue` statement is never reachable.</span></span>

### <a name="the-goto-statement"></a><span data-ttu-id="3f94b-458">A instrução goto</span><span class="sxs-lookup"><span data-stu-id="3f94b-458">The goto statement</span></span>

<span data-ttu-id="3f94b-459">A `goto` instrução transfere o controle para uma instrução que é marcada por um rótulo.</span><span class="sxs-lookup"><span data-stu-id="3f94b-459">The `goto` statement transfers control to a statement that is marked by a label.</span></span>

```antlr
goto_statement
    : 'goto' identifier ';'
    | 'goto' 'case' constant_expression ';'
    | 'goto' 'default' ';'
    ;
```

<span data-ttu-id="3f94b-460">O destino de uma `goto` instrução de *identificador* é a instrução rotulada com o rótulo fornecido.</span><span class="sxs-lookup"><span data-stu-id="3f94b-460">The target of a `goto` *identifier* statement is the labeled statement with the given label.</span></span> <span data-ttu-id="3f94b-461">Se um rótulo com o nome fornecido não existir no membro da função atual ou se a `goto` instrução não estiver dentro do escopo do rótulo, ocorrerá um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="3f94b-461">If a label with the given name does not exist in the current function member, or if the `goto` statement is not within the scope of the label, a compile-time error occurs.</span></span> <span data-ttu-id="3f94b-462">Essa regra permite o uso de uma `goto` instrução para transferir o controle de um escopo aninhado, mas não para um escopo aninhado.</span><span class="sxs-lookup"><span data-stu-id="3f94b-462">This rule permits the use of a `goto` statement to transfer control out of a nested scope, but not into a nested scope.</span></span> <span data-ttu-id="3f94b-463">No exemplo</span><span class="sxs-lookup"><span data-stu-id="3f94b-463">In the example</span></span>
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
<span data-ttu-id="3f94b-464">uma `goto` instrução é usada para transferir o controle de um escopo aninhado.</span><span class="sxs-lookup"><span data-stu-id="3f94b-464">a `goto` statement is used to transfer control out of a nested scope.</span></span>

<span data-ttu-id="3f94b-465">O destino de uma `goto case` instrução é a lista de instruções na instrução de circunscrição `switch` imediatamente ([a instrução switch](statements.md#the-switch-statement)), que contém `case` um rótulo com o valor constante fornecido.</span><span class="sxs-lookup"><span data-stu-id="3f94b-465">The target of a `goto case` statement is the statement list in the immediately enclosing `switch` statement ([The switch statement](statements.md#the-switch-statement)), which contains a `case` label with the given constant value.</span></span> <span data-ttu-id="3f94b-466">Se a instrução `goto case` não estiver delimitada por uma instrução `switch`, se *constant_expression* não for implicitamente conversível ([conversões implícitas](conversions.md#implicit-conversions)) para o tipo regulador da instrução `switch` de fechamento mais próximo, ou se o delimitador mais próximo a instrução `switch` não contém um rótulo de `case` com o valor constante fornecido, ocorre um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="3f94b-466">If the `goto case` statement is not enclosed by a `switch` statement, if the *constant_expression* is not implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the governing type of the nearest enclosing `switch` statement, or if the nearest enclosing `switch` statement does not contain a `case` label with the given constant value, a compile-time error occurs.</span></span>

<span data-ttu-id="3f94b-467">O destino de uma `goto default` instrução é a lista de instruções na instrução de circunscrição `switch` imediatamente ([a instrução switch](statements.md#the-switch-statement)), que contém `default` um rótulo.</span><span class="sxs-lookup"><span data-stu-id="3f94b-467">The target of a `goto default` statement is the statement list in the immediately enclosing `switch` statement ([The switch statement](statements.md#the-switch-statement)), which contains a `default` label.</span></span> <span data-ttu-id="3f94b-468">Se a `goto default` instrução não estiver delimitada por `switch` uma instrução ou se a instrução delimitadora `switch` mais próxima não contiver `default` um rótulo, ocorrerá um erro em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="3f94b-468">If the `goto default` statement is not enclosed by a `switch` statement, or if the nearest enclosing `switch` statement does not contain a `default` label, a compile-time error occurs.</span></span>

<span data-ttu-id="3f94b-469">Uma `goto` instrução não pode sair `finally` de um bloco ([a instrução try](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="3f94b-469">A `goto` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="3f94b-470">Quando uma `goto` instrução ocorre dentro de `finally` um bloco `goto` , o destino da instrução deve estar dentro do mesmo `finally` bloco ou ocorre um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="3f94b-470">When a `goto` statement occurs within a `finally` block, the target of the `goto` statement must be within the same `finally` block, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="3f94b-471">Uma `goto` instrução é executada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="3f94b-471">A `goto` statement is executed as follows:</span></span>

*  <span data-ttu-id="3f94b-472">Se a `goto` instrução sair de um ou mais `try` blocos com blocos `finally` associados, o controle será transferido inicialmente para `finally` o bloco da instrução `try` mais interna.</span><span class="sxs-lookup"><span data-stu-id="3f94b-472">If the `goto` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="3f94b-473">Quando e se o controle atingir o ponto de extremidade `finally` de um bloco, o controle será `finally` transferido para o bloco da próxima `try` instrução de circunscrição.</span><span class="sxs-lookup"><span data-stu-id="3f94b-473">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="3f94b-474">Esse processo é repetido até `finally` que os blocos de todas `try` as instruções intermediárias tenham sido executados.</span><span class="sxs-lookup"><span data-stu-id="3f94b-474">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="3f94b-475">O controle é transferido para o destino da `goto` instrução.</span><span class="sxs-lookup"><span data-stu-id="3f94b-475">Control is transferred to the target of the `goto` statement.</span></span>

<span data-ttu-id="3f94b-476">Como uma `goto` instrução transfere incondicionalmente o controle em outro lugar, o `goto` ponto de extremidade de uma instrução nunca é acessível.</span><span class="sxs-lookup"><span data-stu-id="3f94b-476">Because a `goto` statement unconditionally transfers control elsewhere, the end point of a `goto` statement is never reachable.</span></span>

### <a name="the-return-statement"></a><span data-ttu-id="3f94b-477">A instrução return</span><span class="sxs-lookup"><span data-stu-id="3f94b-477">The return statement</span></span>

<span data-ttu-id="3f94b-478">A `return` instrução retorna o controle para o chamador atual da função na qual a `return` instrução é exibida.</span><span class="sxs-lookup"><span data-stu-id="3f94b-478">The `return` statement returns control to the current caller of the function in which the `return` statement appears.</span></span>

```antlr
return_statement
    : 'return' expression? ';'
    ;
```

<span data-ttu-id="3f94b-479">Uma `return` instrução sem expressão só pode ser usada em um membro de função que não Compute um valor, ou seja, um método com o tipo de resultado ([corpo](classes.md#method-body)do método `void`), `set` o acessador de uma propriedade ou um `add` indexador, o e `remove` acessadores de um evento, um construtor de instância, um construtor estático ou um destruidor.</span><span class="sxs-lookup"><span data-stu-id="3f94b-479">A `return` statement with no expression can be used only in a function member that does not compute a value, that is, a method with the result type ([Method body](classes.md#method-body)) `void`, the `set` accessor of a property or indexer, the `add` and `remove` accessors of an event, an instance constructor, a static constructor, or a destructor.</span></span>

<span data-ttu-id="3f94b-480">Uma `return` instrução com uma expressão só pode ser usada em um membro de função que computa um valor, ou seja, um método com um tipo de resultado não void, o `get` acessador de uma propriedade ou um indexador ou um operador definido pelo usuário.</span><span class="sxs-lookup"><span data-stu-id="3f94b-480">A `return` statement with an expression can only be used in a function member that computes a value, that is, a method with a non-void result type, the `get` accessor of a property or indexer, or a user-defined operator.</span></span> <span data-ttu-id="3f94b-481">Uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)) deve existir do tipo da expressão para o tipo de retorno do membro da função que a contém.</span><span class="sxs-lookup"><span data-stu-id="3f94b-481">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) must exist from the type of the expression to the return type of the containing function member.</span></span>

<span data-ttu-id="3f94b-482">Instruções de retorno também podem ser usadas no corpo de expressões de função anônimas ([expressões de função anônimas](expressions.md#anonymous-function-expressions)) e participam da determinação de quais conversões existem para essas funções.</span><span class="sxs-lookup"><span data-stu-id="3f94b-482">Return statements can also be used in the body of anonymous function expressions ([Anonymous function expressions](expressions.md#anonymous-function-expressions)), and participate in determining which conversions exist for those functions.</span></span>

<span data-ttu-id="3f94b-483">É um erro de tempo de compilação para que `return` uma instrução apareça em um `finally` bloco ([a instrução try](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="3f94b-483">It is a compile-time error for a `return` statement to appear in a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span>

<span data-ttu-id="3f94b-484">Uma `return` instrução é executada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="3f94b-484">A `return` statement is executed as follows:</span></span>

*  <span data-ttu-id="3f94b-485">Se a `return` instrução especificar uma expressão, a expressão será avaliada e o valor resultante será convertido no tipo de retorno da função que a contém por uma conversão implícita.</span><span class="sxs-lookup"><span data-stu-id="3f94b-485">If the `return` statement specifies an expression, the expression is evaluated and the resulting value is converted to the return type of the containing function by an implicit conversion.</span></span> <span data-ttu-id="3f94b-486">O resultado da conversão se torna o valor de resultado produzido pela função.</span><span class="sxs-lookup"><span data-stu-id="3f94b-486">The result of the conversion becomes the result value produced by the function.</span></span>
*  <span data-ttu-id="3f94b-487">Se a `return` instrução estiver entre um ou mais `try` blocos ou `catch` com blocos associados `finally` , o controle será transferido inicialmente para o `finally` bloco da instrução mais `try` interna.</span><span class="sxs-lookup"><span data-stu-id="3f94b-487">If the `return` statement is enclosed by one or more `try` or `catch` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="3f94b-488">Quando e se o controle atingir o ponto de extremidade `finally` de um bloco, o controle será `finally` transferido para o bloco da próxima `try` instrução de circunscrição.</span><span class="sxs-lookup"><span data-stu-id="3f94b-488">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="3f94b-489">Esse processo é repetido até `finally` que os blocos de todas `try` as instruções de circunscrição tenham sido executados.</span><span class="sxs-lookup"><span data-stu-id="3f94b-489">This process is repeated until the `finally` blocks of all enclosing `try` statements have been executed.</span></span>
*  <span data-ttu-id="3f94b-490">Se a função que a contém não for uma função assíncrona, o controle será retornado para o chamador da função recipiente junto com o valor de resultado, se houver.</span><span class="sxs-lookup"><span data-stu-id="3f94b-490">If the containing function is not an async function, control is returned to the caller of the containing function along with the result value, if any.</span></span>
*  <span data-ttu-id="3f94b-491">Se a função que a contém for uma função assíncrona, o controle será retornado para o chamador atual e o valor de resultado, se houver, será registrado na tarefa de retorno conforme descrito em ([interfaces do enumerador](classes.md#enumerator-interfaces)).</span><span class="sxs-lookup"><span data-stu-id="3f94b-491">If the containing function is an async function, control is returned to the current caller, and the result value, if any, is recorded in the return task as described in ([Enumerator interfaces](classes.md#enumerator-interfaces)).</span></span>

<span data-ttu-id="3f94b-492">Como uma `return` instrução transfere incondicionalmente o controle em outro lugar, o `return` ponto de extremidade de uma instrução nunca é acessível.</span><span class="sxs-lookup"><span data-stu-id="3f94b-492">Because a `return` statement unconditionally transfers control elsewhere, the end point of a `return` statement is never reachable.</span></span>

### <a name="the-throw-statement"></a><span data-ttu-id="3f94b-493">A instrução Throw</span><span class="sxs-lookup"><span data-stu-id="3f94b-493">The throw statement</span></span>

<span data-ttu-id="3f94b-494">A `throw` instrução gera uma exceção.</span><span class="sxs-lookup"><span data-stu-id="3f94b-494">The `throw` statement throws an exception.</span></span>

```antlr
throw_statement
    : 'throw' expression? ';'
    ;
```

<span data-ttu-id="3f94b-495">Uma `throw` instrução com uma expressão lança o valor produzido avaliando a expressão.</span><span class="sxs-lookup"><span data-stu-id="3f94b-495">A `throw` statement with an expression throws the value produced by evaluating the expression.</span></span> <span data-ttu-id="3f94b-496">A expressão deve indicar um valor do tipo `System.Exception`de classe, de um tipo de classe que deriva de `System.Exception` ou de um tipo de parâmetro de tipo que tenha `System.Exception` (ou uma subclasse dele) como sua classe base efetiva.</span><span class="sxs-lookup"><span data-stu-id="3f94b-496">The expression must denote a value of the class type `System.Exception`, of a class type that derives from `System.Exception` or of a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span> <span data-ttu-id="3f94b-497">Se a avaliação da expressão produzir `null`, um `System.NullReferenceException` será lançado em vez disso.</span><span class="sxs-lookup"><span data-stu-id="3f94b-497">If evaluation of the expression produces `null`, a `System.NullReferenceException` is thrown instead.</span></span>

<span data-ttu-id="3f94b-498">Uma `throw` instrução sem expressão só pode ser usada em um `catch` bloco, caso em que a instrução relança a exceção que está sendo manipulada por esse `catch` bloco no momento.</span><span class="sxs-lookup"><span data-stu-id="3f94b-498">A `throw` statement with no expression can be used only in a `catch` block, in which case that statement re-throws the exception that is currently being handled by that `catch` block.</span></span>

<span data-ttu-id="3f94b-499">Como uma `throw` instrução transfere incondicionalmente o controle em outro lugar, o `throw` ponto de extremidade de uma instrução nunca é acessível.</span><span class="sxs-lookup"><span data-stu-id="3f94b-499">Because a `throw` statement unconditionally transfers control elsewhere, the end point of a `throw` statement is never reachable.</span></span>

<span data-ttu-id="3f94b-500">Quando uma exceção é lançada, o controle é transferido para a `catch` primeira cláusula em uma instrução `try` delimitadora que pode manipular a exceção.</span><span class="sxs-lookup"><span data-stu-id="3f94b-500">When an exception is thrown, control is transferred to the first `catch` clause in an enclosing `try` statement that can handle the exception.</span></span> <span data-ttu-id="3f94b-501">O processo que ocorre desde o ponto da exceção sendo gerada para o ponto de transferência do controle para um manipulador de exceção adequado é conhecido como ***propagação de exceção***.</span><span class="sxs-lookup"><span data-stu-id="3f94b-501">The process that takes place from the point of the exception being thrown to the point of transferring control to a suitable exception handler is known as ***exception propagation***.</span></span> <span data-ttu-id="3f94b-502">A propagação de uma exceção consiste em avaliar repetidamente as etapas a seguir `catch` até que uma cláusula correspondente à exceção seja encontrada.</span><span class="sxs-lookup"><span data-stu-id="3f94b-502">Propagation of an exception consists of repeatedly evaluating the following steps until a `catch` clause that matches the exception is found.</span></span> <span data-ttu-id="3f94b-503">Nesta descrição, o ***ponto de lançamento*** é inicialmente o local no qual a exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="3f94b-503">In this description, the ***throw point*** is initially the location at which the exception is thrown.</span></span>

*  <span data-ttu-id="3f94b-504">No membro da função atual, cada `try` instrução que se aproxima do ponto de lançamento é examinada.</span><span class="sxs-lookup"><span data-stu-id="3f94b-504">In the current function member, each `try` statement that encloses the throw point is examined.</span></span> <span data-ttu-id="3f94b-505">Para cada instrução `S`, começando com a instrução `try` mais interna e terminando com `try` a instrução mais externa, as etapas a seguir são avaliadas:</span><span class="sxs-lookup"><span data-stu-id="3f94b-505">For each statement `S`, starting with the innermost `try` statement and ending with the outermost `try` statement, the following steps are evaluated:</span></span>

   * <span data-ttu-id="3f94b-506">Se o `try` bloco de `S` fechar o ponto de lançamento e se S tiver uma ou mais `catch` cláusulas, as `catch` cláusulas serão examinadas em ordem de aparência para localizar um manipulador adequado para a exceção, de acordo com as regras especificadas em Seção [a instrução try](statements.md#the-try-statement).</span><span class="sxs-lookup"><span data-stu-id="3f94b-506">If the `try` block of `S` encloses the throw point and if S has one or more `catch` clauses, the `catch` clauses are examined in order of appearance to locate a suitable handler for the exception, according to the rules specified in Section [The try statement](statements.md#the-try-statement).</span></span> <span data-ttu-id="3f94b-507">Se uma cláusula `catch` correspondente estiver localizada, a propagação de exceção será concluída transferindo o controle para o bloco `catch` dessa cláusula.</span><span class="sxs-lookup"><span data-stu-id="3f94b-507">If a matching `catch` clause is located, the exception propagation is completed by transferring control to the block of that `catch` clause.</span></span>

   * <span data-ttu-id="3f94b-508">Caso contrário, se `try` o bloco ou `catch` um bloco `S` de fechar o ponto de lançamento e se `S` tiver um `finally` bloco, o controle será transferido para `finally` o bloco.</span><span class="sxs-lookup"><span data-stu-id="3f94b-508">Otherwise, if the `try` block or a `catch` block of `S` encloses the throw point and if `S` has a `finally` block, control is transferred to the `finally` block.</span></span> <span data-ttu-id="3f94b-509">Se o `finally` bloco lançar outra exceção, o processamento da exceção atual será encerrado.</span><span class="sxs-lookup"><span data-stu-id="3f94b-509">If the `finally` block throws another exception, processing of the current exception is terminated.</span></span> <span data-ttu-id="3f94b-510">Caso contrário, quando o controle atingir o ponto de `finally` extremidade do bloco, o processamento da exceção atual será continuado.</span><span class="sxs-lookup"><span data-stu-id="3f94b-510">Otherwise, when control reaches the end point of the `finally` block, processing of the current exception is continued.</span></span>

*  <span data-ttu-id="3f94b-511">Se um manipulador de exceção não estiver localizado na invocação de função atual, a invocação de função será encerrada e uma das seguintes situações ocorrerá:</span><span class="sxs-lookup"><span data-stu-id="3f94b-511">If an exception handler was not located in the current function invocation, the function invocation is terminated, and one of the following occurs:</span></span>

   * <span data-ttu-id="3f94b-512">Se a função atual for não Async, as etapas acima serão repetidas para o chamador da função com um ponto de geração correspondente à instrução da qual o membro da função foi invocado.</span><span class="sxs-lookup"><span data-stu-id="3f94b-512">If the current function is non-async, the steps above are repeated for the caller of the function with a throw point corresponding to the statement from which the function member was invoked.</span></span>

   * <span data-ttu-id="3f94b-513">Se a função atual for Async e retornando Task, a exceção será registrada na tarefa de retorno, que será colocada em um estado falho ou cancelado, conforme descrito em [interfaces do enumerador](classes.md#enumerator-interfaces).</span><span class="sxs-lookup"><span data-stu-id="3f94b-513">If the current function is async and task-returning, the exception is recorded in the return task, which is put into a faulted or cancelled state as described in [Enumerator interfaces](classes.md#enumerator-interfaces).</span></span>

   * <span data-ttu-id="3f94b-514">Se a função atual for Async e void-retornando, o contexto de sincronização do thread atual será notificado conforme descrito em [interfaces enumeráveis](classes.md#enumerable-interfaces).</span><span class="sxs-lookup"><span data-stu-id="3f94b-514">If the current function is async and void-returning, the synchronization context of the current thread is notified as described in [Enumerable interfaces](classes.md#enumerable-interfaces).</span></span>

*  <span data-ttu-id="3f94b-515">Se o processamento de exceções terminar todas as invocações de membro de função no thread atual, indicando que o thread não tem nenhum manipulador para a exceção, o thread será encerrado.</span><span class="sxs-lookup"><span data-stu-id="3f94b-515">If the exception processing terminates all function member invocations in the current thread, indicating that the thread has no handler for the exception, then the thread is itself terminated.</span></span> <span data-ttu-id="3f94b-516">O impacto dessa terminação é definido pela implementação.</span><span class="sxs-lookup"><span data-stu-id="3f94b-516">The impact of such termination is implementation-defined.</span></span>

## <a name="the-try-statement"></a><span data-ttu-id="3f94b-517">A instrução try</span><span class="sxs-lookup"><span data-stu-id="3f94b-517">The try statement</span></span>

<span data-ttu-id="3f94b-518">A `try` instrução fornece um mecanismo para capturar exceções que ocorrem durante a execução de um bloco.</span><span class="sxs-lookup"><span data-stu-id="3f94b-518">The `try` statement provides a mechanism for catching exceptions that occur during execution of a block.</span></span> <span data-ttu-id="3f94b-519">Além disso, `try` a instrução fornece a capacidade de especificar um bloco de código que é sempre executado quando o controle `try` sai da instrução.</span><span class="sxs-lookup"><span data-stu-id="3f94b-519">Furthermore, the `try` statement provides the ability to specify a block of code that is always executed when control leaves the `try` statement.</span></span>

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

<span data-ttu-id="3f94b-520">Há três formas de `try` instruções possíveis:</span><span class="sxs-lookup"><span data-stu-id="3f94b-520">There are three possible forms of `try` statements:</span></span>

*  <span data-ttu-id="3f94b-521">Um `try` bloco seguido por um ou mais `catch` blocos.</span><span class="sxs-lookup"><span data-stu-id="3f94b-521">A `try` block followed by one or more `catch` blocks.</span></span>
*  <span data-ttu-id="3f94b-522">Um `try` bloco seguido por um `finally` bloco.</span><span class="sxs-lookup"><span data-stu-id="3f94b-522">A `try` block followed by a `finally` block.</span></span>
*  <span data-ttu-id="3f94b-523">Um `try` bloco seguido por um ou mais `catch` blocos seguidos por `finally` um bloco.</span><span class="sxs-lookup"><span data-stu-id="3f94b-523">A `try` block followed by one or more `catch` blocks followed by a `finally` block.</span></span>

<span data-ttu-id="3f94b-524">Quando uma cláusula `catch` especifica um *exception_specifier*, o tipo deve ser `System.Exception`, um tipo derivado de `System.Exception` ou um tipo de parâmetro de tipo que tenha `System.Exception` (ou uma subclasse dele) como sua classe base efetiva.</span><span class="sxs-lookup"><span data-stu-id="3f94b-524">When a `catch` clause specifies an *exception_specifier*, the type must be `System.Exception`, a type that derives from `System.Exception` or a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span>

<span data-ttu-id="3f94b-525">Quando uma cláusula `catch` especifica um *exception_specifier* com um *identificador*, uma ***variável de exceção*** do nome e tipo fornecido é declarada.</span><span class="sxs-lookup"><span data-stu-id="3f94b-525">When a `catch` clause specifies both an *exception_specifier* with an *identifier*, an ***exception variable*** of the given name and type is declared.</span></span> <span data-ttu-id="3f94b-526">A variável de exceção corresponde a uma variável local com um escopo que se estende `catch` pela cláusula.</span><span class="sxs-lookup"><span data-stu-id="3f94b-526">The exception variable corresponds to a local variable with a scope that extends over the `catch` clause.</span></span> <span data-ttu-id="3f94b-527">Durante a execução do *exception_filter* e do *bloco*, a variável de exceção representa a exceção que está sendo manipulada no momento.</span><span class="sxs-lookup"><span data-stu-id="3f94b-527">During execution of the *exception_filter* and *block*, the exception variable represents the exception currently being handled.</span></span> <span data-ttu-id="3f94b-528">Para fins de verificação de atribuição definitiva, a variável de exceção é considerada definitivamente atribuída em seu escopo inteiro.</span><span class="sxs-lookup"><span data-stu-id="3f94b-528">For purposes of definite assignment checking, the exception variable is considered definitely assigned in its entire scope.</span></span>

<span data-ttu-id="3f94b-529">A menos `catch` que uma cláusula inclua um nome de variável de exceção, é impossível acessar o objeto de exceção no `catch` filtro e no bloco.</span><span class="sxs-lookup"><span data-stu-id="3f94b-529">Unless a `catch` clause includes an exception variable name, it is impossible to access the exception object in the filter and `catch` block.</span></span>

<span data-ttu-id="3f94b-530">Uma cláusula `catch` que não especifica um *exception_specifier* é chamada de cláusula geral `catch`.</span><span class="sxs-lookup"><span data-stu-id="3f94b-530">A `catch` clause that does not specify an *exception_specifier* is called a general `catch` clause.</span></span>

<span data-ttu-id="3f94b-531">Algumas linguagens de programação podem dar suporte a exceções que não podem ser representes `System.Exception`como um objeto derivado de, embora essas exceções C# nunca pudessem ser geradas pelo código.</span><span class="sxs-lookup"><span data-stu-id="3f94b-531">Some programming languages may support exceptions that are not representable as an object derived from `System.Exception`, although such exceptions could never be generated by C# code.</span></span> <span data-ttu-id="3f94b-532">Uma cláusula `catch` geral pode ser usada para capturar essas exceções.</span><span class="sxs-lookup"><span data-stu-id="3f94b-532">A general `catch` clause may be used to catch such exceptions.</span></span> <span data-ttu-id="3f94b-533">Portanto, uma cláusula `catch` geral é semanticamente diferente de uma que especifica o tipo `System.Exception`, pois a primeira também pode detectar exceções de outras linguagens.</span><span class="sxs-lookup"><span data-stu-id="3f94b-533">Thus, a general `catch` clause is semantically different from one that specifies the type `System.Exception`, in that the former may also catch exceptions from other languages.</span></span>

<span data-ttu-id="3f94b-534">Para localizar um manipulador de uma exceção, `catch` as cláusulas são examinadas em ordem lexical.</span><span class="sxs-lookup"><span data-stu-id="3f94b-534">In order to locate a handler for an exception, `catch` clauses are examined in lexical order.</span></span> <span data-ttu-id="3f94b-535">Se uma `catch` cláusula Especifica um tipo, mas nenhum filtro de exceção, é um erro de tempo de compilação para `catch` uma cláusula posterior na `try` mesma instrução para especificar um tipo que seja igual ou derivado desse tipo.</span><span class="sxs-lookup"><span data-stu-id="3f94b-535">If a `catch` clause specifies a type but no exception filter, it is a compile-time error for a later `catch` clause in the same `try` statement to specify a type that is the same as, or is derived from, that type.</span></span> <span data-ttu-id="3f94b-536">Se uma `catch` cláusula não especificar nenhum tipo e nenhum filtro, ela deverá ser a `catch` última cláusula para `try` essa instrução.</span><span class="sxs-lookup"><span data-stu-id="3f94b-536">If a `catch` clause specifies no type and no filter, it must be the last `catch` clause for that `try` statement.</span></span>

<span data-ttu-id="3f94b-537">Dentro de `catch` um bloco, `throw` uma instrução ([a instrução Throw](statements.md#the-throw-statement)) sem expressão pode ser usada para relançar `catch` a exceção que foi detectada pelo bloco.</span><span class="sxs-lookup"><span data-stu-id="3f94b-537">Within a `catch` block, a `throw` statement ([The throw statement](statements.md#the-throw-statement)) with no expression can be used to re-throw the exception that was caught by the `catch` block.</span></span> <span data-ttu-id="3f94b-538">As atribuições a uma variável de exceção não alteram a exceção gerada novamente.</span><span class="sxs-lookup"><span data-stu-id="3f94b-538">Assignments to an exception variable do not alter the exception that is re-thrown.</span></span>

<span data-ttu-id="3f94b-539">No exemplo</span><span class="sxs-lookup"><span data-stu-id="3f94b-539">In the example</span></span>
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
<span data-ttu-id="3f94b-540">o método `F` captura uma exceção, grava algumas informações de diagnóstico no console, altera a variável de exceção e lança novamente a exceção.</span><span class="sxs-lookup"><span data-stu-id="3f94b-540">the method `F` catches an exception, writes some diagnostic information to the console, alters the exception variable, and re-throws the exception.</span></span> <span data-ttu-id="3f94b-541">A exceção gerada novamente é a exceção original, portanto, a saída produzida é:</span><span class="sxs-lookup"><span data-stu-id="3f94b-541">The exception that is re-thrown is the original exception, so the output produced is:</span></span>
```console
Exception in F: G
Exception in Main: G
```

<span data-ttu-id="3f94b-542">Se o primeiro bloco catch tiver sido `e` lançado em vez de relançar a exceção atual, a saída produzida seria a seguinte:</span><span class="sxs-lookup"><span data-stu-id="3f94b-542">If the first catch block had thrown `e` instead of rethrowing the current exception, the output produced would be as follows:</span></span>
```console
Exception in F: G
Exception in Main: F
```

<span data-ttu-id="3f94b-543">É um erro de tempo de compilação para uma `break`instrução `continue`, ou `goto` para transferir o controle de um `finally` bloco.</span><span class="sxs-lookup"><span data-stu-id="3f94b-543">It is a compile-time error for a `break`, `continue`, or `goto` statement to transfer control out of a `finally` block.</span></span> <span data-ttu-id="3f94b-544">Quando uma `break`instrução `continue`,, `goto` ou ocorre em um `finally` bloco, o destino da instrução deve estar dentro do mesmo `finally` bloco ou, caso contrário, ocorrerá um erro de tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="3f94b-544">When a `break`, `continue`, or `goto` statement occurs in a `finally` block, the target of the statement must be within the same `finally` block, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="3f94b-545">É um erro de tempo de compilação para que `return` uma instrução ocorra em um `finally` bloco.</span><span class="sxs-lookup"><span data-stu-id="3f94b-545">It is a compile-time error for a `return` statement to occur in a `finally` block.</span></span>

<span data-ttu-id="3f94b-546">Uma `try` instrução é executada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="3f94b-546">A `try` statement is executed as follows:</span></span>

*  <span data-ttu-id="3f94b-547">O controle é transferido para `try` o bloco.</span><span class="sxs-lookup"><span data-stu-id="3f94b-547">Control is transferred to the `try` block.</span></span>
*  <span data-ttu-id="3f94b-548">Quando e se o controle atingir o ponto de extremidade `try` do bloco:</span><span class="sxs-lookup"><span data-stu-id="3f94b-548">When and if control reaches the end point of the `try` block:</span></span>
   *  <span data-ttu-id="3f94b-549">Se a `try` instrução tiver um `finally` bloco, o `finally` bloco será executado.</span><span class="sxs-lookup"><span data-stu-id="3f94b-549">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
   *  <span data-ttu-id="3f94b-550">O controle é transferido para o ponto de extremidade `try` da instrução.</span><span class="sxs-lookup"><span data-stu-id="3f94b-550">Control is transferred to the end point of the `try` statement.</span></span>

*  <span data-ttu-id="3f94b-551">Se uma exceção for propagada para `try` a instrução durante a `try` execução do bloco:</span><span class="sxs-lookup"><span data-stu-id="3f94b-551">If an exception is propagated to the `try` statement during execution of the `try` block:</span></span>
   *  <span data-ttu-id="3f94b-552">As `catch` cláusulas, se houver, são examinadas em ordem de aparência para localizar um manipulador adequado para a exceção.</span><span class="sxs-lookup"><span data-stu-id="3f94b-552">The `catch` clauses, if any, are examined in order of appearance to locate a suitable handler for the exception.</span></span> <span data-ttu-id="3f94b-553">Se uma `catch` cláusula não especificar um tipo, ou especificar o tipo de exceção ou um tipo base do tipo de exceção:</span><span class="sxs-lookup"><span data-stu-id="3f94b-553">If a `catch` clause does not specify a type, or specifies the exception type or a base type of the exception type:</span></span>
      *  <span data-ttu-id="3f94b-554">Se a `catch` cláusula declarar uma variável de exceção, o objeto de exceção será atribuído à variável de exceção.</span><span class="sxs-lookup"><span data-stu-id="3f94b-554">If the `catch` clause declares an exception variable, the exception object is assigned to the exception variable.</span></span>
      *  <span data-ttu-id="3f94b-555">Se a `catch` cláusula declarar um filtro de exceção, o filtro será avaliado.</span><span class="sxs-lookup"><span data-stu-id="3f94b-555">If the `catch` clause declares an exception filter, the filter is evaluated.</span></span> <span data-ttu-id="3f94b-556">Se ele for avaliado como `false`, a cláusula catch não será uma correspondência e a pesquisa continuará em todas as `catch` cláusulas subsequentes para um manipulador adequado.</span><span class="sxs-lookup"><span data-stu-id="3f94b-556">If it evaluates to `false`, the catch clause is not a match, and the search continues through any subsequent `catch` clauses for a suitable handler.</span></span>
      *  <span data-ttu-id="3f94b-557">Caso contrário, `catch` a cláusula será considerada uma correspondência e o controle será transferido para o `catch` bloco correspondente.</span><span class="sxs-lookup"><span data-stu-id="3f94b-557">Otherwise, the `catch` clause is considered a match, and control is transferred to the matching `catch` block.</span></span>
      *  <span data-ttu-id="3f94b-558">Quando e se o controle atingir o ponto de extremidade `catch` do bloco:</span><span class="sxs-lookup"><span data-stu-id="3f94b-558">When and if control reaches the end point of the `catch` block:</span></span>
         * <span data-ttu-id="3f94b-559">Se a `try` instrução tiver um `finally` bloco, o `finally` bloco será executado.</span><span class="sxs-lookup"><span data-stu-id="3f94b-559">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
         * <span data-ttu-id="3f94b-560">O controle é transferido para o ponto de extremidade `try` da instrução.</span><span class="sxs-lookup"><span data-stu-id="3f94b-560">Control is transferred to the end point of the `try` statement.</span></span>
      *  <span data-ttu-id="3f94b-561">Se uma exceção for propagada para `try` a instrução durante a `catch` execução do bloco:</span><span class="sxs-lookup"><span data-stu-id="3f94b-561">If an exception is propagated to the `try` statement during execution of the `catch` block:</span></span>
         *  <span data-ttu-id="3f94b-562">Se a `try` instrução tiver um `finally` bloco, o `finally` bloco será executado.</span><span class="sxs-lookup"><span data-stu-id="3f94b-562">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
         *  <span data-ttu-id="3f94b-563">A exceção é propagada para a próxima instrução `try` de circunscrição.</span><span class="sxs-lookup"><span data-stu-id="3f94b-563">The exception is propagated to the next enclosing `try` statement.</span></span>
   *  <span data-ttu-id="3f94b-564">Se a `try` instrução não `catch` tiver cláusulas ou se nenhuma `catch` cláusula corresponder à exceção:</span><span class="sxs-lookup"><span data-stu-id="3f94b-564">If the `try` statement has no `catch` clauses or if no `catch` clause matches the exception:</span></span>
      *  <span data-ttu-id="3f94b-565">Se a `try` instrução tiver um `finally` bloco, o `finally` bloco será executado.</span><span class="sxs-lookup"><span data-stu-id="3f94b-565">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
      *  <span data-ttu-id="3f94b-566">A exceção é propagada para a próxima instrução `try` de circunscrição.</span><span class="sxs-lookup"><span data-stu-id="3f94b-566">The exception is propagated to the next enclosing `try` statement.</span></span>

<span data-ttu-id="3f94b-567">As instruções de um `finally` bloco são sempre executadas quando o controle sai `try` de uma instrução.</span><span class="sxs-lookup"><span data-stu-id="3f94b-567">The statements of a `finally` block are always executed when control leaves a `try` statement.</span></span> <span data-ttu-id="3f94b-568">Isso é verdadeiro se a transferência de controle ocorre como resultado da execução normal, como resultado da execução de uma `break`instrução `continue`, `goto`, ou `return` , ou `try` como resultado da propagação de uma exceção do privacidade.</span><span class="sxs-lookup"><span data-stu-id="3f94b-568">This is true whether the control transfer occurs as a result of normal execution, as a result of executing a `break`, `continue`, `goto`, or `return` statement, or as a result of propagating an exception out of the `try` statement.</span></span>

<span data-ttu-id="3f94b-569">Se uma exceção for lançada durante a execução de `finally` um bloco e não for detectada no mesmo bloco finally, a exceção será propagada para a próxima instrução `try` de circunscrição.</span><span class="sxs-lookup"><span data-stu-id="3f94b-569">If an exception is thrown during execution of a `finally` block, and is not caught within the same finally block, the exception is propagated to the next enclosing `try` statement.</span></span> <span data-ttu-id="3f94b-570">Se outra exceção estava no processo de ser propagada, essa exceção é perdida.</span><span class="sxs-lookup"><span data-stu-id="3f94b-570">If another exception was in the process of being propagated, that exception is lost.</span></span> <span data-ttu-id="3f94b-571">O processo de propagação de uma exceção é abordado mais detalhadamente na descrição `throw` da instrução ([a instrução Throw](statements.md#the-throw-statement)).</span><span class="sxs-lookup"><span data-stu-id="3f94b-571">The process of propagating an exception is discussed further in the description of the `throw` statement ([The throw statement](statements.md#the-throw-statement)).</span></span>

<span data-ttu-id="3f94b-572">O `try` bloco de uma `try` instrução estará acessível se a `try` instrução puder ser acessada.</span><span class="sxs-lookup"><span data-stu-id="3f94b-572">The `try` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="3f94b-573">Um `catch` bloco de uma `try` instrução pode ser acessado se a `try` instrução estiver acessível.</span><span class="sxs-lookup"><span data-stu-id="3f94b-573">A `catch` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="3f94b-574">O `finally` bloco de uma `try` instrução estará acessível se a `try` instrução puder ser acessada.</span><span class="sxs-lookup"><span data-stu-id="3f94b-574">The `finally` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="3f94b-575">O ponto de extremidade de `try` uma instrução estará acessível se as seguintes opções forem verdadeiras:</span><span class="sxs-lookup"><span data-stu-id="3f94b-575">The end point of a `try` statement is reachable if both of the following are true:</span></span>

*  <span data-ttu-id="3f94b-576">O ponto de extremidade do `try` bloco é acessível ou o ponto de extremidade de pelo menos `catch` um bloco está acessível.</span><span class="sxs-lookup"><span data-stu-id="3f94b-576">The end point of the `try` block is reachable or the end point of at least one `catch` block is reachable.</span></span>
*  <span data-ttu-id="3f94b-577">Se um `finally` bloco estiver presente, o ponto final `finally` do bloco estará acessível.</span><span class="sxs-lookup"><span data-stu-id="3f94b-577">If a `finally` block is present, the end point of the `finally` block is reachable.</span></span>

## <a name="the-checked-and-unchecked-statements"></a><span data-ttu-id="3f94b-578">As instruções marcadas e não verificadas</span><span class="sxs-lookup"><span data-stu-id="3f94b-578">The checked and unchecked statements</span></span>

<span data-ttu-id="3f94b-579">As `checked` instruções `unchecked` e são usadas para controlar o ***contexto de verificação de estouro*** para operações aritméticas de tipo integral e conversões.</span><span class="sxs-lookup"><span data-stu-id="3f94b-579">The `checked` and `unchecked` statements are used to control the ***overflow checking context*** for integral-type arithmetic operations and conversions.</span></span>

```antlr
checked_statement
    : 'checked' block
    ;

unchecked_statement
    : 'unchecked' block
    ;
```

<span data-ttu-id="3f94b-580">A `checked` instrução faz com que todas as expressões no *bloco* sejam avaliadas em um contexto selecionado, `unchecked` e a instrução faz com que todas as expressões no *bloco* sejam avaliadas em um contexto desmarcado.</span><span class="sxs-lookup"><span data-stu-id="3f94b-580">The `checked` statement causes all expressions in the *block* to be evaluated in a checked context, and the `unchecked` statement causes all expressions in the *block* to be evaluated in an unchecked context.</span></span>

<span data-ttu-id="3f94b-581">As `checked` instruções `unchecked` e`checked` são precisamente equivalentes aos operadores `unchecked` e ([os operadores marcados e desmarcados](expressions.md#the-checked-and-unchecked-operators)), exceto que operam em blocos em vez de expressões.</span><span class="sxs-lookup"><span data-stu-id="3f94b-581">The `checked` and `unchecked` statements are precisely equivalent to the `checked` and `unchecked` operators ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)), except that they operate on blocks instead of expressions.</span></span>

## <a name="the-lock-statement"></a><span data-ttu-id="3f94b-582">A instrução Lock</span><span class="sxs-lookup"><span data-stu-id="3f94b-582">The lock statement</span></span>

<span data-ttu-id="3f94b-583">A `lock` instrução Obtém o bloqueio de mutual-exclusion para um determinado objeto, executa uma instrução e, em seguida, libera o bloqueio.</span><span class="sxs-lookup"><span data-stu-id="3f94b-583">The `lock` statement obtains the mutual-exclusion lock for a given object, executes a statement, and then releases the lock.</span></span>

```antlr
lock_statement
    : 'lock' '(' expression ')' embedded_statement
    ;
```

<span data-ttu-id="3f94b-584">A expressão de uma instrução `lock` deve indicar um valor de um tipo conhecido como *reference_type*.</span><span class="sxs-lookup"><span data-stu-id="3f94b-584">The expression of a `lock` statement must denote a value of a type known to be a *reference_type*.</span></span> <span data-ttu-id="3f94b-585">Nenhuma conversão de Boxing implícita ([conversões Boxing](conversions.md#boxing-conversions)) é executada para a expressão de uma instrução `lock` e, portanto, é um erro de tempo de compilação para a expressão denotar um valor de um *value_type*.</span><span class="sxs-lookup"><span data-stu-id="3f94b-585">No implicit boxing conversion ([Boxing conversions](conversions.md#boxing-conversions)) is ever performed for the expression of a `lock` statement, and thus it is a compile-time error for the expression to denote a value of a *value_type*.</span></span>

<span data-ttu-id="3f94b-586">Uma `lock` instrução do formulário</span><span class="sxs-lookup"><span data-stu-id="3f94b-586">A `lock` statement of the form</span></span>
```csharp
lock (x) ...
```
<span data-ttu-id="3f94b-587">onde `x` é uma expressão de um *reference_type*, é precisamente equivalente a</span><span class="sxs-lookup"><span data-stu-id="3f94b-587">where `x` is an expression of a *reference_type*, is precisely equivalent to</span></span>
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
<span data-ttu-id="3f94b-588">exceto que `x` é avaliado apenas uma vez.</span><span class="sxs-lookup"><span data-stu-id="3f94b-588">except that `x` is only evaluated once.</span></span>

<span data-ttu-id="3f94b-589">Enquanto um bloqueio de exclusão mútua é mantido, o código em execução no mesmo thread de execução também pode obter e liberar o bloqueio.</span><span class="sxs-lookup"><span data-stu-id="3f94b-589">While a mutual-exclusion lock is held, code executing in the same execution thread can also obtain and release the lock.</span></span> <span data-ttu-id="3f94b-590">No entanto, o código em execução em outros threads é impedido de obter o bloqueio até que o bloqueio seja liberado.</span><span class="sxs-lookup"><span data-stu-id="3f94b-590">However, code executing in other threads is blocked from obtaining the lock until the lock is released.</span></span>

<span data-ttu-id="3f94b-591">O `System.Type` bloqueio de objetos para sincronizar o acesso a dados estáticos não é recomendado.</span><span class="sxs-lookup"><span data-stu-id="3f94b-591">Locking `System.Type` objects in order to synchronize access to static data is not recommended.</span></span> <span data-ttu-id="3f94b-592">Outro código pode bloquear no mesmo tipo, o que pode resultar em deadlock.</span><span class="sxs-lookup"><span data-stu-id="3f94b-592">Other code might lock on the same type, which can result in deadlock.</span></span> <span data-ttu-id="3f94b-593">Uma abordagem melhor é sincronizar o acesso a dados estáticos bloqueando um objeto estático privado.</span><span class="sxs-lookup"><span data-stu-id="3f94b-593">A better approach is to synchronize access to static data by locking a private static object.</span></span> <span data-ttu-id="3f94b-594">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="3f94b-594">For example:</span></span>
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

## <a name="the-using-statement"></a><span data-ttu-id="3f94b-595">A instrução using</span><span class="sxs-lookup"><span data-stu-id="3f94b-595">The using statement</span></span>

<span data-ttu-id="3f94b-596">A `using` instrução Obtém um ou mais recursos, executa uma instrução e, em seguida, descarta o recurso.</span><span class="sxs-lookup"><span data-stu-id="3f94b-596">The `using` statement obtains one or more resources, executes a statement, and then disposes of the resource.</span></span>

```antlr
using_statement
    : 'using' '(' resource_acquisition ')' embedded_statement
    ;

resource_acquisition
    : local_variable_declaration
    | expression
    ;
```

<span data-ttu-id="3f94b-597">Um ***recurso*** é uma classe ou struct que implementa `System.IDisposable`, que inclui um único método sem parâmetros chamado `Dispose`.</span><span class="sxs-lookup"><span data-stu-id="3f94b-597">A ***resource*** is a class or struct that implements `System.IDisposable`, which includes a single parameterless method named `Dispose`.</span></span> <span data-ttu-id="3f94b-598">O código que está usando um recurso pode `Dispose` chamar para indicar que o recurso não é mais necessário.</span><span class="sxs-lookup"><span data-stu-id="3f94b-598">Code that is using a resource can call `Dispose` to indicate that the resource is no longer needed.</span></span> <span data-ttu-id="3f94b-599">Se `Dispose` não for chamado, a eliminação automática eventualmente ocorrerá como consequência da coleta de lixo.</span><span class="sxs-lookup"><span data-stu-id="3f94b-599">If `Dispose` is not called, then automatic disposal eventually occurs as a consequence of garbage collection.</span></span>

<span data-ttu-id="3f94b-600">Se a forma de *resource_acquisition* for *local_variable_declaration* , o tipo de *local_variable_declaration* deverá ser `dynamic` ou um tipo que possa ser convertido implicitamente em `System.IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="3f94b-600">If the form of *resource_acquisition* is *local_variable_declaration* then the type of the *local_variable_declaration* must be either `dynamic` or a type that can be implicitly converted to `System.IDisposable`.</span></span> <span data-ttu-id="3f94b-601">Se a forma de *resource_acquisition* for *expression* , essa expressão deverá ser conversível implicitamente em `System.IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="3f94b-601">If the form of *resource_acquisition* is *expression* then this expression must be implicitly convertible to `System.IDisposable`.</span></span>

<span data-ttu-id="3f94b-602">Variáveis locais declaradas em um *resource_acquisition* são somente leitura e devem incluir um inicializador.</span><span class="sxs-lookup"><span data-stu-id="3f94b-602">Local variables declared in a *resource_acquisition* are read-only, and must include an initializer.</span></span> <span data-ttu-id="3f94b-603">Ocorrerá um erro de tempo de compilação se a instrução inserida tentar modificar essas variáveis locais (por meio `++` de `--` atribuição ou os operadores e), pegar o endereço delas ou passá `ref` - `out` las como parâmetros ou.</span><span class="sxs-lookup"><span data-stu-id="3f94b-603">A compile-time error occurs if the embedded statement attempts to modify these local variables (via assignment or the `++` and `--` operators) , take the address of them, or pass them as `ref` or `out` parameters.</span></span>

<span data-ttu-id="3f94b-604">Uma `using` instrução é convertida em três partes: aquisição, uso e alienação.</span><span class="sxs-lookup"><span data-stu-id="3f94b-604">A `using` statement is translated into three parts: acquisition, usage, and disposal.</span></span> <span data-ttu-id="3f94b-605">O uso do recurso é implicitamente incluído em uma `try` instrução que inclui uma `finally` cláusula.</span><span class="sxs-lookup"><span data-stu-id="3f94b-605">Usage of the resource is implicitly enclosed in a `try` statement that includes a `finally` clause.</span></span> <span data-ttu-id="3f94b-606">Essa `finally` cláusula descarta o recurso.</span><span class="sxs-lookup"><span data-stu-id="3f94b-606">This `finally` clause disposes of the resource.</span></span> <span data-ttu-id="3f94b-607">Se um `null` recurso for adquirido, nenhuma chamada para `Dispose` será feita e nenhuma exceção será gerada.</span><span class="sxs-lookup"><span data-stu-id="3f94b-607">If a `null` resource is acquired, then no call to `Dispose` is made, and no exception is thrown.</span></span> <span data-ttu-id="3f94b-608">Se o recurso for do tipo `dynamic` , ele será convertido dinamicamente por meio de uma conversão dinâmica implícita ([conversões dinâmicas implícitas](conversions.md#implicit-dynamic-conversions)) `IDisposable` durante a aquisição para garantir que a conversão seja bem-sucedida antes do uso e liberação.</span><span class="sxs-lookup"><span data-stu-id="3f94b-608">If the resource is of type `dynamic` it is dynamically converted through an implicit dynamic conversion ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions)) to `IDisposable` during acquisition in order to ensure that the conversion is successful before the usage and disposal.</span></span>

<span data-ttu-id="3f94b-609">Uma `using` instrução do formulário</span><span class="sxs-lookup"><span data-stu-id="3f94b-609">A `using` statement of the form</span></span>
```csharp
using (ResourceType resource = expression) statement
```
<span data-ttu-id="3f94b-610">corresponde a uma das três expansões possíveis.</span><span class="sxs-lookup"><span data-stu-id="3f94b-610">corresponds to one of three possible expansions.</span></span> <span data-ttu-id="3f94b-611">Quando `ResourceType` é um tipo de valor não anulável, a expansão é</span><span class="sxs-lookup"><span data-stu-id="3f94b-611">When `ResourceType` is a non-nullable value type, the expansion is</span></span>
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

<span data-ttu-id="3f94b-612">Caso contrário, `ResourceType` quando for um tipo de valor anulável ou um tipo de `dynamic`referência diferente de, a expansão será</span><span class="sxs-lookup"><span data-stu-id="3f94b-612">Otherwise, when `ResourceType` is a nullable value type or a reference type other than `dynamic`, the expansion is</span></span>
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

<span data-ttu-id="3f94b-613">Caso contrário, `ResourceType` quando `dynamic`for, a expansão será</span><span class="sxs-lookup"><span data-stu-id="3f94b-613">Otherwise, when `ResourceType` is `dynamic`, the expansion is</span></span>
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

<span data-ttu-id="3f94b-614">Em qualquer expansão, a `resource` variável é somente leitura na instrução incorporada, e a `d` variável é inacessível em, e invisível para, a instrução inserida.</span><span class="sxs-lookup"><span data-stu-id="3f94b-614">In either expansion, the `resource` variable is read-only in the embedded statement, and the `d` variable is inaccessible in, and invisible to, the embedded statement.</span></span>

<span data-ttu-id="3f94b-615">Uma implementação tem permissão para implementar uma determinada instrução using de forma diferente, por exemplo, por motivos de desempenho, desde que o comportamento seja consistente com a expansão acima.</span><span class="sxs-lookup"><span data-stu-id="3f94b-615">An implementation is permitted to implement a given using-statement differently, e.g. for performance reasons, as long as the behavior is consistent with the above expansion.</span></span>

<span data-ttu-id="3f94b-616">Uma `using` instrução do formulário</span><span class="sxs-lookup"><span data-stu-id="3f94b-616">A `using` statement of the form</span></span>
```csharp
using (expression) statement
```
<span data-ttu-id="3f94b-617">tem as mesmas três expansões possíveis.</span><span class="sxs-lookup"><span data-stu-id="3f94b-617">has the same three possible expansions.</span></span> <span data-ttu-id="3f94b-618">Nesse caso `ResourceType` `expression`, é implicitamente o tipo de tempo de compilação do, se tiver um.</span><span class="sxs-lookup"><span data-stu-id="3f94b-618">In this case `ResourceType` is implicitly the compile-time type of the `expression`, if it has one.</span></span> <span data-ttu-id="3f94b-619">Caso contrário, `IDisposable` a própria interface será usada `ResourceType`como o.</span><span class="sxs-lookup"><span data-stu-id="3f94b-619">Otherwise the interface `IDisposable` itself is used as the `ResourceType`.</span></span> <span data-ttu-id="3f94b-620">A `resource` variável é inacessível em, e invisível para, a instrução inserida.</span><span class="sxs-lookup"><span data-stu-id="3f94b-620">The `resource` variable is inaccessible in, and invisible to, the embedded statement.</span></span>

<span data-ttu-id="3f94b-621">Quando um *resource_acquisition* assume a forma de um *local_variable_declaration*, é possível adquirir vários recursos de um determinado tipo.</span><span class="sxs-lookup"><span data-stu-id="3f94b-621">When a *resource_acquisition* takes the form of a *local_variable_declaration*, it is possible to acquire multiple resources of a given type.</span></span> <span data-ttu-id="3f94b-622">Uma `using` instrução do formulário</span><span class="sxs-lookup"><span data-stu-id="3f94b-622">A `using` statement of the form</span></span>
```csharp
using (ResourceType r1 = e1, r2 = e2, ..., rN = eN) statement
```
<span data-ttu-id="3f94b-623">é precisamente equivalente a uma sequência de instruções `using` aninhadas:</span><span class="sxs-lookup"><span data-stu-id="3f94b-623">is precisely equivalent to a sequence of nested `using` statements:</span></span>
```csharp
using (ResourceType r1 = e1)
    using (ResourceType r2 = e2)
        ...
            using (ResourceType rN = eN)
                statement
```

<span data-ttu-id="3f94b-624">O exemplo a seguir cria um arquivo `log.txt` chamado e grava duas linhas de texto no arquivo.</span><span class="sxs-lookup"><span data-stu-id="3f94b-624">The example below creates a file named `log.txt` and writes two lines of text to the file.</span></span> <span data-ttu-id="3f94b-625">Em seguida, o exemplo abre o mesmo arquivo para ler e copiar as linhas de texto contidas para o console.</span><span class="sxs-lookup"><span data-stu-id="3f94b-625">The example then opens that same file for reading and copies the contained lines of text to the console.</span></span>
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

<span data-ttu-id="3f94b-626">Como as `TextWriter` classes `TextReader` e implementam `IDisposable` a interface, o exemplo pode `using` usar instruções para garantir que o arquivo subjacente seja fechado corretamente seguindo as operações de gravação ou leitura.</span><span class="sxs-lookup"><span data-stu-id="3f94b-626">Since the `TextWriter` and `TextReader` classes implement the `IDisposable` interface, the example can use `using` statements to ensure that the underlying file is properly closed following the write or read operations.</span></span>

## <a name="the-yield-statement"></a><span data-ttu-id="3f94b-627">A instrução yield</span><span class="sxs-lookup"><span data-stu-id="3f94b-627">The yield statement</span></span>

<span data-ttu-id="3f94b-628">A `yield` instrução é usada em um bloco do iterador ([blocos](statements.md#blocks)) para gerar um valor para o objeto do enumerador ([objetos do enumerador](classes.md#enumerator-objects)) ou objeto enumerável ([objetos enumeráveis](classes.md#enumerable-objects)) de um iterador ou para sinalizar o final da iteração.</span><span class="sxs-lookup"><span data-stu-id="3f94b-628">The `yield` statement is used in an iterator block ([Blocks](statements.md#blocks)) to yield a value to the enumerator object ([Enumerator objects](classes.md#enumerator-objects)) or enumerable object ([Enumerable objects](classes.md#enumerable-objects)) of an iterator or to signal the end of the iteration.</span></span>

```antlr
yield_statement
    : 'yield' 'return' expression ';'
    | 'yield' 'break' ';'
    ;
```

<span data-ttu-id="3f94b-629">`yield`Não é uma palavra reservada; Ele tem um significado especial somente quando usado imediatamente antes `return` de `break` uma palavra-chave ou.</span><span class="sxs-lookup"><span data-stu-id="3f94b-629">`yield` is not a reserved word; it has special meaning only when used immediately before a `return` or `break` keyword.</span></span> <span data-ttu-id="3f94b-630">Em outros contextos, `yield` pode ser usado como um identificador.</span><span class="sxs-lookup"><span data-stu-id="3f94b-630">In other contexts, `yield` can be used as an identifier.</span></span>

<span data-ttu-id="3f94b-631">Há várias restrições sobre onde uma `yield` instrução pode aparecer, conforme descrito a seguir.</span><span class="sxs-lookup"><span data-stu-id="3f94b-631">There are several restrictions on where a `yield` statement can appear, as described in the following.</span></span>

*  <span data-ttu-id="3f94b-632">É um erro de tempo de compilação para uma instrução `yield` (de qualquer forma) aparecer fora de um *method_body*, *operator_body* ou *accessor_body*</span><span class="sxs-lookup"><span data-stu-id="3f94b-632">It is a compile-time error for a `yield` statement (of either form) to appear outside a *method_body*, *operator_body* or *accessor_body*</span></span>
*  <span data-ttu-id="3f94b-633">É um erro de tempo de compilação para uma `yield` instrução (de qualquer forma) aparecer dentro de uma função anônima.</span><span class="sxs-lookup"><span data-stu-id="3f94b-633">It is a compile-time error for a `yield` statement (of either form) to appear inside an anonymous function.</span></span>
*  <span data-ttu-id="3f94b-634">É um erro de tempo de compilação para uma `yield` instrução (de qualquer forma) aparecer `finally` na cláusula de uma `try` instrução.</span><span class="sxs-lookup"><span data-stu-id="3f94b-634">It is a compile-time error for a `yield` statement (of either form) to appear in the `finally` clause of a `try` statement.</span></span>
*  <span data-ttu-id="3f94b-635">É um erro de tempo de compilação para uma `yield return` instrução aparecer em qualquer lugar em `try` uma instrução que contenha qualquer `catch` cláusula.</span><span class="sxs-lookup"><span data-stu-id="3f94b-635">It is a compile-time error for a `yield return` statement to appear anywhere in a `try` statement that contains any `catch` clauses.</span></span>

<span data-ttu-id="3f94b-636">O exemplo a seguir mostra alguns usos válidos e inválidos de `yield` instruções.</span><span class="sxs-lookup"><span data-stu-id="3f94b-636">The following example shows some valid and invalid uses of `yield` statements.</span></span>

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

<span data-ttu-id="3f94b-637">Uma conversão implícita ([conversões implícitas](conversions.md#implicit-conversions)) deve existir do tipo da expressão na `yield return` instrução para o tipo yield ([tipo yield](classes.md#yield-type)) do iterador.</span><span class="sxs-lookup"><span data-stu-id="3f94b-637">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) must exist from the type of the expression in the `yield return` statement to the yield type ([Yield type](classes.md#yield-type)) of the iterator.</span></span>

<span data-ttu-id="3f94b-638">Uma `yield return` instrução é executada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="3f94b-638">A `yield return` statement is executed as follows:</span></span>

*  <span data-ttu-id="3f94b-639">A expressão fornecida na instrução é avaliada, implicitamente convertida para o tipo yield e atribuída à `Current` Propriedade do objeto Enumerator.</span><span class="sxs-lookup"><span data-stu-id="3f94b-639">The expression given in the statement is evaluated, implicitly converted to the yield type, and assigned to the `Current` property of the enumerator object.</span></span>
*  <span data-ttu-id="3f94b-640">A execução do bloco do iterador está suspensa.</span><span class="sxs-lookup"><span data-stu-id="3f94b-640">Execution of the iterator block is suspended.</span></span> <span data-ttu-id="3f94b-641">Se a `yield return` instrução estiver dentro de um ou `try` mais blocos, os `finally` blocos associados não serão executados neste momento.</span><span class="sxs-lookup"><span data-stu-id="3f94b-641">If the `yield return` statement is within one or more `try` blocks, the associated `finally` blocks are not executed at this time.</span></span>
*  <span data-ttu-id="3f94b-642">O `MoveNext` método do objeto enumerador retorna `true` para seu chamador, indicando que o objeto enumerador foi avançado com êxito para o próximo item.</span><span class="sxs-lookup"><span data-stu-id="3f94b-642">The `MoveNext` method of the enumerator object returns `true` to its caller, indicating that the enumerator object successfully advanced to the next item.</span></span>

<span data-ttu-id="3f94b-643">A próxima chamada para o método do `MoveNext` objeto enumerador retoma a execução do bloco iterador de onde ele foi suspenso pela última vez.</span><span class="sxs-lookup"><span data-stu-id="3f94b-643">The next call to the enumerator object's `MoveNext` method resumes execution of the iterator block from where it was last suspended.</span></span>

<span data-ttu-id="3f94b-644">Uma `yield break` instrução é executada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="3f94b-644">A `yield break` statement is executed as follows:</span></span>

*  <span data-ttu-id="3f94b-645">Se a `yield break` instrução estiver contida em um ou mais `try` blocos com blocos `finally` associados, o controle será transferido inicialmente para `finally` o bloco da instrução `try` mais interna.</span><span class="sxs-lookup"><span data-stu-id="3f94b-645">If the `yield break` statement is enclosed by one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="3f94b-646">Quando e se o controle atingir o ponto de extremidade `finally` de um bloco, o controle será `finally` transferido para o bloco da próxima `try` instrução de circunscrição.</span><span class="sxs-lookup"><span data-stu-id="3f94b-646">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="3f94b-647">Esse processo é repetido até `finally` que os blocos de todas `try` as instruções de circunscrição tenham sido executados.</span><span class="sxs-lookup"><span data-stu-id="3f94b-647">This process is repeated until the `finally` blocks of all enclosing `try` statements have been executed.</span></span>
*  <span data-ttu-id="3f94b-648">O controle é retornado para o chamador do bloco do iterador.</span><span class="sxs-lookup"><span data-stu-id="3f94b-648">Control is returned to the caller of the iterator block.</span></span> <span data-ttu-id="3f94b-649">Esse é o `MoveNext` método ou `Dispose` método do objeto enumerador.</span><span class="sxs-lookup"><span data-stu-id="3f94b-649">This is either the `MoveNext` method or `Dispose` method of the enumerator object.</span></span>

<span data-ttu-id="3f94b-650">Como uma `yield break` instrução transfere incondicionalmente o controle em outro lugar, o `yield break` ponto de extremidade de uma instrução nunca é acessível.</span><span class="sxs-lookup"><span data-stu-id="3f94b-650">Because a `yield break` statement unconditionally transfers control elsewhere, the end point of a `yield break` statement is never reachable.</span></span>
