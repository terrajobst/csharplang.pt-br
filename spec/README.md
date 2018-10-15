<a name="c-language-specification"></a><span data-ttu-id="4fa4c-101">Especificação da Linguagem C#</span><span class="sxs-lookup"><span data-stu-id="4fa4c-101">C# Language Specification</span></span>
===========================

<span data-ttu-id="4fa4c-102">__Versão 6__</span><span class="sxs-lookup"><span data-stu-id="4fa4c-102">__Version 6__</span></span>

<span data-ttu-id="4fa4c-103">Esse é um rascunho não oficial, postado aqui para sua conveniência.</span><span class="sxs-lookup"><span data-stu-id="4fa4c-103">This is an unofficial draft, posted here for convenience.</span></span> <span data-ttu-id="4fa4c-104">Quando podemos enviar uma proposta de especificações de c# 6.0 para ECMA (que atualmente está finalizando o c# 5.0 Standard), ele será compartilhado aqui.</span><span class="sxs-lookup"><span data-stu-id="4fa4c-104">When we submit a C# 6.0 spec proposal to ECMA (who is currently finalizing the C# 5.0 Standard), it will be shared here.</span></span>

<!--
(This document is also available for download: [csharp.pdf](CSharp%20Language%20Specification.pdf?raw=true) and [csharp.docx](CSharp%20Language%20Specification.docx?raw=true))
-->

* [<span data-ttu-id="4fa4c-105">Introdução</span><span class="sxs-lookup"><span data-stu-id="4fa4c-105">Introduction</span></span>](introduction.md)
    * [<span data-ttu-id="4fa4c-106">Olá, Mundo</span><span class="sxs-lookup"><span data-stu-id="4fa4c-106">Hello world</span></span>](introduction.md#hello-world)
    * [<span data-ttu-id="4fa4c-107">Estrutura do programa</span><span class="sxs-lookup"><span data-stu-id="4fa4c-107">Program structure</span></span>](introduction.md#program-structure)
    * [<span data-ttu-id="4fa4c-108">Tipos e variáveis</span><span class="sxs-lookup"><span data-stu-id="4fa4c-108">Types and variables</span></span>](introduction.md#types-and-variables)
    * [<span data-ttu-id="4fa4c-109">Expressões</span><span class="sxs-lookup"><span data-stu-id="4fa4c-109">Expressions</span></span>](introduction.md#expressions)
    * [<span data-ttu-id="4fa4c-110">Instruções</span><span class="sxs-lookup"><span data-stu-id="4fa4c-110">Statements</span></span>](introduction.md#statements)
    * [<span data-ttu-id="4fa4c-111">Classes e objetos</span><span class="sxs-lookup"><span data-stu-id="4fa4c-111">Classes and objects</span></span>](introduction.md#classes-and-objects)
* [<span data-ttu-id="4fa4c-112">Estrutura lexical</span><span class="sxs-lookup"><span data-stu-id="4fa4c-112">Lexical structure</span></span>](lexical-structure.md)
    * [<span data-ttu-id="4fa4c-113">Programas</span><span class="sxs-lookup"><span data-stu-id="4fa4c-113">Programs</span></span>](lexical-structure.md#programs)
    * [<span data-ttu-id="4fa4c-114">Gramática</span><span class="sxs-lookup"><span data-stu-id="4fa4c-114">Grammars</span></span>](lexical-structure.md#grammars)
    * [<span data-ttu-id="4fa4c-115">Análise lexical</span><span class="sxs-lookup"><span data-stu-id="4fa4c-115">Lexical analysis</span></span>](lexical-structure.md#lexical-analysis)
    * [<span data-ttu-id="4fa4c-116">Tokens</span><span class="sxs-lookup"><span data-stu-id="4fa4c-116">Tokens</span></span>](lexical-structure.md#tokens)
    * [<span data-ttu-id="4fa4c-117">Diretivas de pré-processamento</span><span class="sxs-lookup"><span data-stu-id="4fa4c-117">Pre-processing directives</span></span>](lexical-structure.md#pre-processing-directives)
* [<span data-ttu-id="4fa4c-118">Conceitos básicos</span><span class="sxs-lookup"><span data-stu-id="4fa4c-118">Basic concepts</span></span>](basic-concepts.md)
    * [<span data-ttu-id="4fa4c-119">Inicialização de aplicativos</span><span class="sxs-lookup"><span data-stu-id="4fa4c-119">Application Startup</span></span>](basic-concepts.md#application-startup)
    * [<span data-ttu-id="4fa4c-120">Terminação de aplicativos</span><span class="sxs-lookup"><span data-stu-id="4fa4c-120">Application termination</span></span>](basic-concepts.md#application-termination)
    * [<span data-ttu-id="4fa4c-121">Declarações</span><span class="sxs-lookup"><span data-stu-id="4fa4c-121">Declarations</span></span>](basic-concepts.md#declarations)
    * [<span data-ttu-id="4fa4c-122">Membros</span><span class="sxs-lookup"><span data-stu-id="4fa4c-122">Members</span></span>](basic-concepts.md#members)
    * [<span data-ttu-id="4fa4c-123">Acesso de membros</span><span class="sxs-lookup"><span data-stu-id="4fa4c-123">Member access</span></span>](basic-concepts.md#member-access)
    * [<span data-ttu-id="4fa4c-124">Assinaturas e sobrecarga</span><span class="sxs-lookup"><span data-stu-id="4fa4c-124">Signatures and overloading</span></span>](basic-concepts.md#signatures-and-overloading)
    * [<span data-ttu-id="4fa4c-125">Escopos</span><span class="sxs-lookup"><span data-stu-id="4fa4c-125">Scopes</span></span>](basic-concepts.md#scopes)
    * [<span data-ttu-id="4fa4c-126">Namespace e nomes de tipos</span><span class="sxs-lookup"><span data-stu-id="4fa4c-126">Namespace and type names</span></span>](basic-concepts.md#namespace-and-type-names)
    * [<span data-ttu-id="4fa4c-127">Gerenciamento Automático de Memória</span><span class="sxs-lookup"><span data-stu-id="4fa4c-127">Automatic memory management</span></span>](basic-concepts.md#automatic-memory-management)
    * [<span data-ttu-id="4fa4c-128">Ordem de execução</span><span class="sxs-lookup"><span data-stu-id="4fa4c-128">Execution order</span></span>](basic-concepts.md#execution-order)
* [<span data-ttu-id="4fa4c-129">Tipos</span><span class="sxs-lookup"><span data-stu-id="4fa4c-129">Types</span></span>](types.md)
    * [<span data-ttu-id="4fa4c-130">Tipos de valor</span><span class="sxs-lookup"><span data-stu-id="4fa4c-130">Value types</span></span>](types.md#value-types)
    * [<span data-ttu-id="4fa4c-131">Tipos de referência</span><span class="sxs-lookup"><span data-stu-id="4fa4c-131">Reference types</span></span>](types.md#reference-types)
    * [<span data-ttu-id="4fa4c-132">Conversão boxing e unboxing</span><span class="sxs-lookup"><span data-stu-id="4fa4c-132">Boxing and unboxing</span></span>](types.md#boxing-and-unboxing)
    * [<span data-ttu-id="4fa4c-133">Tipos construídos</span><span class="sxs-lookup"><span data-stu-id="4fa4c-133">Constructed types</span></span>](types.md#constructed-types)
    * [<span data-ttu-id="4fa4c-134">Parâmetros de tipo</span><span class="sxs-lookup"><span data-stu-id="4fa4c-134">Type parameters</span></span>](types.md#type-parameters)
    * [<span data-ttu-id="4fa4c-135">Tipos de árvore de expressão</span><span class="sxs-lookup"><span data-stu-id="4fa4c-135">Expression tree types</span></span>](types.md#expression-tree-types)
    * [<span data-ttu-id="4fa4c-136">O tipo dinâmico</span><span class="sxs-lookup"><span data-stu-id="4fa4c-136">The dynamic type</span></span>](types.md#the-dynamic-type)
* [<span data-ttu-id="4fa4c-137">Variáveis</span><span class="sxs-lookup"><span data-stu-id="4fa4c-137">Variables</span></span>](variables.md)
    * [<span data-ttu-id="4fa4c-138">Categorias variáveis</span><span class="sxs-lookup"><span data-stu-id="4fa4c-138">Variable categories</span></span>](variables.md#variable-categories)
    * [<span data-ttu-id="4fa4c-139">Valores padrão</span><span class="sxs-lookup"><span data-stu-id="4fa4c-139">Default values</span></span>](variables.md#default-values)
    * [<span data-ttu-id="4fa4c-140">Atribuição definida</span><span class="sxs-lookup"><span data-stu-id="4fa4c-140">Definite assignment</span></span>](variables.md#definite-assignment)
    * [<span data-ttu-id="4fa4c-141">Referências de variáveis</span><span class="sxs-lookup"><span data-stu-id="4fa4c-141">Variable references</span></span>](variables.md#variable-references)
    * [<span data-ttu-id="4fa4c-142">Atomicidade de referências de variáveis</span><span class="sxs-lookup"><span data-stu-id="4fa4c-142">Atomicity of variable references</span></span>](variables.md#atomicity-of-variable-references)
* [<span data-ttu-id="4fa4c-143">Conversões</span><span class="sxs-lookup"><span data-stu-id="4fa4c-143">Conversions</span></span>](conversions.md)
    * [<span data-ttu-id="4fa4c-144">Conversões implícitas</span><span class="sxs-lookup"><span data-stu-id="4fa4c-144">Implicit conversions</span></span>](conversions.md#implicit-conversions)
    * [<span data-ttu-id="4fa4c-145">Conversões explícitas</span><span class="sxs-lookup"><span data-stu-id="4fa4c-145">Explicit conversions</span></span>](conversions.md#explicit-conversions)
    * [<span data-ttu-id="4fa4c-146">Conversões padrão</span><span class="sxs-lookup"><span data-stu-id="4fa4c-146">Standard conversions</span></span>](conversions.md#standard-conversions)
    * [<span data-ttu-id="4fa4c-147">Conversões Definidas pelo Usuário</span><span class="sxs-lookup"><span data-stu-id="4fa4c-147">User-defined conversions</span></span>](conversions.md#user-defined-conversions)
    * [<span data-ttu-id="4fa4c-148">Conversões de função anônima</span><span class="sxs-lookup"><span data-stu-id="4fa4c-148">Anonymous function conversions</span></span>](conversions.md#anonymous-function-conversions)
    * [<span data-ttu-id="4fa4c-149">Conversões de grupo de métodos</span><span class="sxs-lookup"><span data-stu-id="4fa4c-149">Method group conversions</span></span>](conversions.md#method-group-conversions)
* [<span data-ttu-id="4fa4c-150">Expressões</span><span class="sxs-lookup"><span data-stu-id="4fa4c-150">Expressions</span></span>](expressions.md)
    * [<span data-ttu-id="4fa4c-151">Classificações de expressão</span><span class="sxs-lookup"><span data-stu-id="4fa4c-151">Expression classifications</span></span>](expressions.md#expression-classification)
    * [<span data-ttu-id="4fa4c-152">Associação estática e dinâmica</span><span class="sxs-lookup"><span data-stu-id="4fa4c-152">Static and Dynamic Binding</span></span>](expressions.md#static-and-dynamic-binding)
    * [<span data-ttu-id="4fa4c-153">Operadores</span><span class="sxs-lookup"><span data-stu-id="4fa4c-153">Operators</span></span>](expressions.md#operators)
    * [<span data-ttu-id="4fa4c-154">Pesquisa de membros</span><span class="sxs-lookup"><span data-stu-id="4fa4c-154">Member lookup</span></span>](expressions.md#member-lookup)
    * [<span data-ttu-id="4fa4c-155">Membros da função</span><span class="sxs-lookup"><span data-stu-id="4fa4c-155">Function members</span></span>](expressions.md#function-members)
    * [<span data-ttu-id="4fa4c-156">Expressões primárias</span><span class="sxs-lookup"><span data-stu-id="4fa4c-156">Primary expressions</span></span>](expressions.md#primary-expressions)
    * [<span data-ttu-id="4fa4c-157">Operadores unários</span><span class="sxs-lookup"><span data-stu-id="4fa4c-157">Unary operators</span></span>](expressions.md#unary-operators)
    * [<span data-ttu-id="4fa4c-158">Operadores aritméticos</span><span class="sxs-lookup"><span data-stu-id="4fa4c-158">Arithmetic operators</span></span>](expressions.md#arithmetic-operators)
    * [<span data-ttu-id="4fa4c-159">Operadores shift</span><span class="sxs-lookup"><span data-stu-id="4fa4c-159">Shift operators</span></span>](expressions.md#shift-operators)
    * [<span data-ttu-id="4fa4c-160">Operadores de teste de tipo e relacional</span><span class="sxs-lookup"><span data-stu-id="4fa4c-160">Relational and type-testing operators</span></span>](expressions.md#relational-and-type-testing-operators)
    * [<span data-ttu-id="4fa4c-161">Operadores lógicos</span><span class="sxs-lookup"><span data-stu-id="4fa4c-161">Logical operators</span></span>](expressions.md#logical-operators)
    * [<span data-ttu-id="4fa4c-162">Operadores lógicos condicionais</span><span class="sxs-lookup"><span data-stu-id="4fa4c-162">Conditional logical operators</span></span>](expressions.md#conditional-logical-operators)
    * [<span data-ttu-id="4fa4c-163">O operador de coalescência nula</span><span class="sxs-lookup"><span data-stu-id="4fa4c-163">The null coalescing operator</span></span>](expressions.md#the-null-coalescing-operator)
    * [<span data-ttu-id="4fa4c-164">Operador condicional</span><span class="sxs-lookup"><span data-stu-id="4fa4c-164">Conditional operator</span></span>](expressions.md#conditional-operator)
    * [<span data-ttu-id="4fa4c-165">Expressões de função anônima</span><span class="sxs-lookup"><span data-stu-id="4fa4c-165">Anonymous function expressions</span></span>](expressions.md#anonymous-function-expressions)
    * [<span data-ttu-id="4fa4c-166">Expressões de consulta</span><span class="sxs-lookup"><span data-stu-id="4fa4c-166">Query expressions</span></span>](expressions.md#query-expressions)
    * [<span data-ttu-id="4fa4c-167">Operadores de atribuição</span><span class="sxs-lookup"><span data-stu-id="4fa4c-167">Assignment operators</span></span>](expressions.md#assignment-operators)
    * [<span data-ttu-id="4fa4c-168">Expressão</span><span class="sxs-lookup"><span data-stu-id="4fa4c-168">Expression</span></span>](expressions.md#expression)
    * [<span data-ttu-id="4fa4c-169">Expressões constantes</span><span class="sxs-lookup"><span data-stu-id="4fa4c-169">Constant expressions</span></span>](expressions.md#constant-expressions)
    * [<span data-ttu-id="4fa4c-170">Expressões boolianas</span><span class="sxs-lookup"><span data-stu-id="4fa4c-170">Boolean expressions</span></span>](expressions.md#boolean-expressions)
* [<span data-ttu-id="4fa4c-171">Instruções</span><span class="sxs-lookup"><span data-stu-id="4fa4c-171">Statements</span></span>](statements.md)
    * [<span data-ttu-id="4fa4c-172">Pontos de extremidade e acessibilidade</span><span class="sxs-lookup"><span data-stu-id="4fa4c-172">End points and reachability</span></span>](statements.md#end-points-and-reachability)
    * [<span data-ttu-id="4fa4c-173">Blocos</span><span class="sxs-lookup"><span data-stu-id="4fa4c-173">Blocks</span></span>](statements.md#blocks)
    * [<span data-ttu-id="4fa4c-174">A instrução vazia</span><span class="sxs-lookup"><span data-stu-id="4fa4c-174">The empty statement</span></span>](statements.md#the-empty-statement)
    * [<span data-ttu-id="4fa4c-175">Instruções rotuladas</span><span class="sxs-lookup"><span data-stu-id="4fa4c-175">Labeled statements</span></span>](statements.md#labeled-statements)
    * [<span data-ttu-id="4fa4c-176">Instruções de declaração</span><span class="sxs-lookup"><span data-stu-id="4fa4c-176">Declaration statements</span></span>](statements.md#declaration-statements)
    * [<span data-ttu-id="4fa4c-177">Instruções de expressão</span><span class="sxs-lookup"><span data-stu-id="4fa4c-177">Expression statements</span></span>](statements.md#expression-statements)
    * [<span data-ttu-id="4fa4c-178">Instruções de seleção</span><span class="sxs-lookup"><span data-stu-id="4fa4c-178">Selection statements</span></span>](statements.md#selection-statements)
    * [<span data-ttu-id="4fa4c-179">Instruções de iteração</span><span class="sxs-lookup"><span data-stu-id="4fa4c-179">Iteration statements</span></span>](statements.md#iteration-statements)
    * [<span data-ttu-id="4fa4c-180">Instruções de hiperlink</span><span class="sxs-lookup"><span data-stu-id="4fa4c-180">Jump statements</span></span>](statements.md#jump-statements)
    * [<span data-ttu-id="4fa4c-181">A instrução try</span><span class="sxs-lookup"><span data-stu-id="4fa4c-181">The try statement</span></span>](statements.md#the-try-statement)
    * [<span data-ttu-id="4fa4c-182">As instruções marcadas e desmarcadas</span><span class="sxs-lookup"><span data-stu-id="4fa4c-182">The checked and unchecked statements</span></span>](statements.md#the-checked-and-unchecked-statements)
    * [<span data-ttu-id="4fa4c-183">A instrução lock</span><span class="sxs-lookup"><span data-stu-id="4fa4c-183">The lock statement</span></span>](statements.md#the-lock-statement)
    * [<span data-ttu-id="4fa4c-184">A instrução using</span><span class="sxs-lookup"><span data-stu-id="4fa4c-184">The using statement</span></span>](statements.md#the-using-statement)
    * [<span data-ttu-id="4fa4c-185">A instrução yield</span><span class="sxs-lookup"><span data-stu-id="4fa4c-185">The yield statement</span></span>](statements.md#the-yield-statement)
* [<span data-ttu-id="4fa4c-186">Namespaces</span><span class="sxs-lookup"><span data-stu-id="4fa4c-186">Namespaces</span></span>](namespaces.md)
    * [<span data-ttu-id="4fa4c-187">Unidades de compilação</span><span class="sxs-lookup"><span data-stu-id="4fa4c-187">Compilation units</span></span>](namespaces.md#compilation-units)
    * [<span data-ttu-id="4fa4c-188">Declarações de namespace</span><span class="sxs-lookup"><span data-stu-id="4fa4c-188">Namespace declarations</span></span>](namespaces.md#namespace-declarations)
    * [<span data-ttu-id="4fa4c-189">Aliases extern</span><span class="sxs-lookup"><span data-stu-id="4fa4c-189">Extern aliases</span></span>](namespaces.md#extern-aliases)
    * [<span data-ttu-id="4fa4c-190">Uso de diretivas</span><span class="sxs-lookup"><span data-stu-id="4fa4c-190">Using directives</span></span>](namespaces.md#using-directives)
    * [<span data-ttu-id="4fa4c-191">Membros de namespace</span><span class="sxs-lookup"><span data-stu-id="4fa4c-191">Namespace members</span></span>](namespaces.md#namespace-members)
    * [<span data-ttu-id="4fa4c-192">Declarações de tipo</span><span class="sxs-lookup"><span data-stu-id="4fa4c-192">Type declarations</span></span>](namespaces.md#type-declarations)
    * [<span data-ttu-id="4fa4c-193">Qualificadores alias de namespace</span><span class="sxs-lookup"><span data-stu-id="4fa4c-193">Namespace alias qualifiers</span></span>](namespaces.md#namespace-alias-qualifiers)
* [<span data-ttu-id="4fa4c-194">Classes</span><span class="sxs-lookup"><span data-stu-id="4fa4c-194">Classes</span></span>](classes.md)
    * [<span data-ttu-id="4fa4c-195">Declarações de classe</span><span class="sxs-lookup"><span data-stu-id="4fa4c-195">Class declarations</span></span>](classes.md#class-declarations)
    * [<span data-ttu-id="4fa4c-196">Tipos parciais</span><span class="sxs-lookup"><span data-stu-id="4fa4c-196">Partial types</span></span>](classes.md#partial-types)
    * [<span data-ttu-id="4fa4c-197">Membros de classe</span><span class="sxs-lookup"><span data-stu-id="4fa4c-197">Class members</span></span>](classes.md#class-members)
    * [<span data-ttu-id="4fa4c-198">Constantes</span><span class="sxs-lookup"><span data-stu-id="4fa4c-198">Constants</span></span>](classes.md#constants)
    * [<span data-ttu-id="4fa4c-199">Campos</span><span class="sxs-lookup"><span data-stu-id="4fa4c-199">Fields</span></span>](classes.md#fields)
    * [<span data-ttu-id="4fa4c-200">Métodos</span><span class="sxs-lookup"><span data-stu-id="4fa4c-200">Methods</span></span>](classes.md#methods)
    * [<span data-ttu-id="4fa4c-201">Propriedades</span><span class="sxs-lookup"><span data-stu-id="4fa4c-201">Properties</span></span>](classes.md#properties)
    * [<span data-ttu-id="4fa4c-202">Eventos</span><span class="sxs-lookup"><span data-stu-id="4fa4c-202">Events</span></span>](classes.md#events)
    * [<span data-ttu-id="4fa4c-203">Indexadores</span><span class="sxs-lookup"><span data-stu-id="4fa4c-203">Indexers</span></span>](classes.md#indexers)
    * [<span data-ttu-id="4fa4c-204">Operadores</span><span class="sxs-lookup"><span data-stu-id="4fa4c-204">Operators</span></span>](classes.md#operators)
    * [<span data-ttu-id="4fa4c-205">Construtores de instância</span><span class="sxs-lookup"><span data-stu-id="4fa4c-205">Instance constructors</span></span>](classes.md#instance-constructors)
    * [<span data-ttu-id="4fa4c-206">Construtores estáticos</span><span class="sxs-lookup"><span data-stu-id="4fa4c-206">Static constructors</span></span>](classes.md#static-constructors)
    * [<span data-ttu-id="4fa4c-207">Destruidores</span><span class="sxs-lookup"><span data-stu-id="4fa4c-207">Destructors</span></span>](classes.md#destructors)
    * [<span data-ttu-id="4fa4c-208">Iteradores</span><span class="sxs-lookup"><span data-stu-id="4fa4c-208">Iterators</span></span>](classes.md#iterators)
    * [<span data-ttu-id="4fa4c-209">Funções assíncronas</span><span class="sxs-lookup"><span data-stu-id="4fa4c-209">Async functions</span></span>](classes.md#async-functions)
* [<span data-ttu-id="4fa4c-210">Estruturas</span><span class="sxs-lookup"><span data-stu-id="4fa4c-210">Structs</span></span>](structs.md)
    * [<span data-ttu-id="4fa4c-211">Declarações de struct</span><span class="sxs-lookup"><span data-stu-id="4fa4c-211">Struct declarations</span></span>](structs.md#struct-declarations)
    * [<span data-ttu-id="4fa4c-212">Membros de struct</span><span class="sxs-lookup"><span data-stu-id="4fa4c-212">Struct members</span></span>](structs.md#struct-members)
    * [<span data-ttu-id="4fa4c-213">Diferenças de classe e struct</span><span class="sxs-lookup"><span data-stu-id="4fa4c-213">Class and struct differences</span></span>](structs.md#class-and-struct-differences)
    * [<span data-ttu-id="4fa4c-214">Exemplos de struct</span><span class="sxs-lookup"><span data-stu-id="4fa4c-214">Struct examples</span></span>](structs.md#struct-examples)
* [<span data-ttu-id="4fa4c-215">Matrizes</span><span class="sxs-lookup"><span data-stu-id="4fa4c-215">Arrays</span></span>](arrays.md)
    * [<span data-ttu-id="4fa4c-216">Tipos de matriz</span><span class="sxs-lookup"><span data-stu-id="4fa4c-216">Array types</span></span>](arrays.md#array-types)
    * [<span data-ttu-id="4fa4c-217">Criação de matriz</span><span class="sxs-lookup"><span data-stu-id="4fa4c-217">Array creation</span></span>](arrays.md#array-creation)
    * [<span data-ttu-id="4fa4c-218">Acesso de elemento de matriz</span><span class="sxs-lookup"><span data-stu-id="4fa4c-218">Array element access</span></span>](arrays.md#array-element-access)
    * [<span data-ttu-id="4fa4c-219">Membros de matriz</span><span class="sxs-lookup"><span data-stu-id="4fa4c-219">Array members</span></span>](arrays.md#array-members)
    * [<span data-ttu-id="4fa4c-220">Covariância de matriz</span><span class="sxs-lookup"><span data-stu-id="4fa4c-220">Array covariance</span></span>](arrays.md#array-covariance)
    * [<span data-ttu-id="4fa4c-221">Inicializadores de matriz</span><span class="sxs-lookup"><span data-stu-id="4fa4c-221">Array initializers</span></span>](arrays.md#array-initializers)
* [<span data-ttu-id="4fa4c-222">Interfaces</span><span class="sxs-lookup"><span data-stu-id="4fa4c-222">Interfaces</span></span>](interfaces.md)
    * [<span data-ttu-id="4fa4c-223">Declarações de interface</span><span class="sxs-lookup"><span data-stu-id="4fa4c-223">Interface declarations</span></span>](interfaces.md#interface-declarations)
    * [<span data-ttu-id="4fa4c-224">Membros da interface</span><span class="sxs-lookup"><span data-stu-id="4fa4c-224">Interface members</span></span>](interfaces.md#interface-members)
    * [<span data-ttu-id="4fa4c-225">Nomes de membros de interface totalmente qualificados</span><span class="sxs-lookup"><span data-stu-id="4fa4c-225">Fully qualified interface member names</span></span>](interfaces.md#fully-qualified-interface-member-names)
    * [<span data-ttu-id="4fa4c-226">Implementações de interfaces</span><span class="sxs-lookup"><span data-stu-id="4fa4c-226">Interface implementations</span></span>](interfaces.md#interface-implementations)
* [<span data-ttu-id="4fa4c-227">Enums</span><span class="sxs-lookup"><span data-stu-id="4fa4c-227">Enums</span></span>](enums.md)
    * [<span data-ttu-id="4fa4c-228">Declarações de enum</span><span class="sxs-lookup"><span data-stu-id="4fa4c-228">Enum declarations</span></span>](enums.md#enum-declarations)
    * [<span data-ttu-id="4fa4c-229">Modificadores de enum</span><span class="sxs-lookup"><span data-stu-id="4fa4c-229">Enum modifiers</span></span>](enums.md#enum-modifiers)
    * [<span data-ttu-id="4fa4c-230">Membros de enum</span><span class="sxs-lookup"><span data-stu-id="4fa4c-230">Enum members</span></span>](enums.md#enum-members)
    * [<span data-ttu-id="4fa4c-231">O tipo de System.Enum</span><span class="sxs-lookup"><span data-stu-id="4fa4c-231">The System.Enum type</span></span>](enums.md#the-systemenum-type)
    * [<span data-ttu-id="4fa4c-232">Operações e valores de enum</span><span class="sxs-lookup"><span data-stu-id="4fa4c-232">Enum values and operations</span></span>](enums.md#enum-values-and-operations)
* [<span data-ttu-id="4fa4c-233">Delegados</span><span class="sxs-lookup"><span data-stu-id="4fa4c-233">Delegates</span></span>](delegates.md)
    * [<span data-ttu-id="4fa4c-234">Declarações de delegado</span><span class="sxs-lookup"><span data-stu-id="4fa4c-234">Delegate declarations</span></span>](delegates.md#delegate-declarations)
    * [<span data-ttu-id="4fa4c-235">Compatibilidade de delegado</span><span class="sxs-lookup"><span data-stu-id="4fa4c-235">Delegate compatibility</span></span>](delegates.md#delegate-compatibility)
    * [<span data-ttu-id="4fa4c-236">Instanciação de delegado</span><span class="sxs-lookup"><span data-stu-id="4fa4c-236">Delegate instantiation</span></span>](delegates.md#delegate-instantiation)
    * [<span data-ttu-id="4fa4c-237">Invocação de delegado</span><span class="sxs-lookup"><span data-stu-id="4fa4c-237">Delegate invocation</span></span>](delegates.md#delegate-invocation)
* [<span data-ttu-id="4fa4c-238">Exceções</span><span class="sxs-lookup"><span data-stu-id="4fa4c-238">Exceptions</span></span>](exceptions.md)
    * [<span data-ttu-id="4fa4c-239">Causas de exceções</span><span class="sxs-lookup"><span data-stu-id="4fa4c-239">Causes of exceptions</span></span>](exceptions.md#causes-of-exceptions)
    * [<span data-ttu-id="4fa4c-240">A classe System.Exception</span><span class="sxs-lookup"><span data-stu-id="4fa4c-240">The System.Exception class</span></span>](exceptions.md#the-systemexception-class)
    * [<span data-ttu-id="4fa4c-241">Como as exceções são tratadas</span><span class="sxs-lookup"><span data-stu-id="4fa4c-241">How exceptions are handled</span></span>](exceptions.md#how-exceptions-are-handled)
    * [<span data-ttu-id="4fa4c-242">Classes de exceção comuns</span><span class="sxs-lookup"><span data-stu-id="4fa4c-242">Common Exception Classes</span></span>](exceptions.md#common-exception-classes)
* [<span data-ttu-id="4fa4c-243">Atributos</span><span class="sxs-lookup"><span data-stu-id="4fa4c-243">Attributes</span></span>](attributes.md)
    * [<span data-ttu-id="4fa4c-244">Classes de atributos</span><span class="sxs-lookup"><span data-stu-id="4fa4c-244">Attribute classes</span></span>](attributes.md#attribute-classes)
    * [<span data-ttu-id="4fa4c-245">Especificação de atributo</span><span class="sxs-lookup"><span data-stu-id="4fa4c-245">Attribute specification</span></span>](attributes.md#attribute-specification)
    * [<span data-ttu-id="4fa4c-246">Instâncias de atributos</span><span class="sxs-lookup"><span data-stu-id="4fa4c-246">Attribute instances</span></span>](attributes.md#attribute-instances)
    * [<span data-ttu-id="4fa4c-247">Atributos reservados</span><span class="sxs-lookup"><span data-stu-id="4fa4c-247">Reserved attributes</span></span>](attributes.md#reserved-attributes)
    * [<span data-ttu-id="4fa4c-248">Atributos para interoperação</span><span class="sxs-lookup"><span data-stu-id="4fa4c-248">Attributes for Interoperation</span></span>](attributes.md#attributes-for-interoperation)
* [<span data-ttu-id="4fa4c-249">Código não seguro</span><span class="sxs-lookup"><span data-stu-id="4fa4c-249">Unsafe code</span></span>](unsafe-code.md)
    * [<span data-ttu-id="4fa4c-250">Contextos não seguros</span><span class="sxs-lookup"><span data-stu-id="4fa4c-250">Unsafe contexts</span></span>](unsafe-code.md#unsafe-contexts)
    * [<span data-ttu-id="4fa4c-251">Tipos de ponteiro</span><span class="sxs-lookup"><span data-stu-id="4fa4c-251">Pointer types</span></span>](unsafe-code.md#pointer-types)
    * [<span data-ttu-id="4fa4c-252">Variáveis fixas e móveis</span><span class="sxs-lookup"><span data-stu-id="4fa4c-252">Fixed and moveable variables</span></span>](unsafe-code.md#fixed-and-moveable-variables)
    * [<span data-ttu-id="4fa4c-253">Conversões de ponteiro</span><span class="sxs-lookup"><span data-stu-id="4fa4c-253">Pointer conversions</span></span>](unsafe-code.md#pointer-conversions)
    * [<span data-ttu-id="4fa4c-254">Ponteiros em expressões</span><span class="sxs-lookup"><span data-stu-id="4fa4c-254">Pointers in expressions</span></span>](unsafe-code.md#pointers-in-expressions)
    * [<span data-ttu-id="4fa4c-255">A instrução fixed</span><span class="sxs-lookup"><span data-stu-id="4fa4c-255">The fixed statement</span></span>](unsafe-code.md#the-fixed-statement)
    * [<span data-ttu-id="4fa4c-256">Buffers de tamanho fixo</span><span class="sxs-lookup"><span data-stu-id="4fa4c-256">Fixed size buffers</span></span>](unsafe-code.md#fixed-size-buffers)
    * [<span data-ttu-id="4fa4c-257">Alocação da pilha</span><span class="sxs-lookup"><span data-stu-id="4fa4c-257">Stack allocation</span></span>](unsafe-code.md#stack-allocation)
    * [<span data-ttu-id="4fa4c-258">Alocação de memória dinâmica</span><span class="sxs-lookup"><span data-stu-id="4fa4c-258">Dynamic memory allocation</span></span>](unsafe-code.md#dynamic-memory-allocation)
* [<span data-ttu-id="4fa4c-259">Comentários de documentação</span><span class="sxs-lookup"><span data-stu-id="4fa4c-259">Documentation comments</span></span>](documentation-comments.md)
    * [<span data-ttu-id="4fa4c-260">Introdução</span><span class="sxs-lookup"><span data-stu-id="4fa4c-260">Introduction</span></span>](documentation-comments.md#introduction)
    * [<span data-ttu-id="4fa4c-261">Marcações recomendadas</span><span class="sxs-lookup"><span data-stu-id="4fa4c-261">Recommended tags</span></span>](documentation-comments.md#recommended-tags)
    * [<span data-ttu-id="4fa4c-262">Processando o arquivo de documentação</span><span class="sxs-lookup"><span data-stu-id="4fa4c-262">Processing the documentation file</span></span>](documentation-comments.md#processing-the-documentation-file)
    * [<span data-ttu-id="4fa4c-263">Um exemplo</span><span class="sxs-lookup"><span data-stu-id="4fa4c-263">An example</span></span>](documentation-comments.md#an-example)

<!--
* Grammar: [csharp.html](http://ljw1004.github.io/csharpspec/csharp.html). Or download in ANTLR format: [csharp.g4](csharp.g4?raw=true). 
-->