---
ms.openlocfilehash: ac2b233eb703b5eea3bd2dfdbeeadd7494b0c695
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484667"
---
# <a name="non-trailing-named-arguments"></a><span data-ttu-id="24e50-101">Argumentos nomeados que não estejam à direita</span><span class="sxs-lookup"><span data-stu-id="24e50-101">Non-trailing named arguments</span></span>

## <a name="summary"></a><span data-ttu-id="24e50-102">Resumo</span><span class="sxs-lookup"><span data-stu-id="24e50-102">Summary</span></span>
[summary]: #summary
<span data-ttu-id="24e50-103">Permitir que os argumentos nomeados sejam usados em posição não à direita, desde que eles sejam usados na posição correta.</span><span class="sxs-lookup"><span data-stu-id="24e50-103">Allow named arguments to be used in non-trailing position, as long as they are used in their correct position.</span></span> <span data-ttu-id="24e50-104">Por exemplo: `DoSomething(isEmployed:true, name, age);`.</span><span class="sxs-lookup"><span data-stu-id="24e50-104">For example: `DoSomething(isEmployed:true, name, age);`.</span></span>

## <a name="motivation"></a><span data-ttu-id="24e50-105">Motivação</span><span class="sxs-lookup"><span data-stu-id="24e50-105">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="24e50-106">A principal motivação é evitar digitar informações redundantes.</span><span class="sxs-lookup"><span data-stu-id="24e50-106">The main motivation is to avoid typing redundant information.</span></span> <span data-ttu-id="24e50-107">É comum nomear um argumento que é um literal (como `null`, `true`) com a finalidade de esclarecer o código, em vez de passar argumentos fora de ordem.</span><span class="sxs-lookup"><span data-stu-id="24e50-107">It is common to name an argument that is a literal (such as `null`, `true`) for the purpose of clarifying the code, rather than of passing arguments out-of-order.</span></span>
<span data-ttu-id="24e50-108">Que atualmente não é permitido (`CS1738`), a menos que todos os argumentos a seguir também sejam nomeados.</span><span class="sxs-lookup"><span data-stu-id="24e50-108">That is currently disallowed (`CS1738`) unless all the following arguments are also named.</span></span>

```csharp
DoSomething(isEmployed:true, name, age); // currently disallowed, even though all arguments are in position
// CS1738 "Named argument specifications must appear after all fixed arguments have been specified"
```

<span data-ttu-id="24e50-109">Alguns exemplos adicionais:</span><span class="sxs-lookup"><span data-stu-id="24e50-109">Some additional examples:</span></span>
```csharp
public void DoSomething(bool isEmployed, string personName, int personAge) { ... }

DoSomething(isEmployed:true, name, age); // currently CS1738, but would become legal
DoSomething(true, personName:name, age); // currently CS1738, but would become legal
DoSomething(name, isEmployed:true, age); // remains illegal
DoSomething(name, age, isEmployed:true); // remains illegal
DoSomething(true, personAge:age, personName:name); // already legal
```

<span data-ttu-id="24e50-110">Isso também funcionaria com params:</span><span class="sxs-lookup"><span data-stu-id="24e50-110">This would also work with params:</span></span>
```csharp
public class Task
{
    public static Task When(TaskStatus all, TaskStatus any, params Task[] tasks);
}
Task.When(all: TaskStatus.RanToCompletion, any: TaskStatus.Faulted, task1, task2)
```

## <a name="detailed-design"></a><span data-ttu-id="24e50-111">Design detalhado</span><span class="sxs-lookup"><span data-stu-id="24e50-111">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="24e50-112">Em § 7.5.1 (listas de argumentos), a especificação atualmente diz:</span><span class="sxs-lookup"><span data-stu-id="24e50-112">In §7.5.1 (Argument lists), the spec currently says:</span></span>
> <span data-ttu-id="24e50-113">Um *argumento* com um *nome de argumento* é conhecido como um __argumento nomeado__, enquanto que um *argumento* sem um *nome de argumento* é um __argumento posicional__.</span><span class="sxs-lookup"><span data-stu-id="24e50-113">An *argument* with an *argument-name* is referred to as a __named argument__, whereas an *argument* without an *argument-name* is a __positional argument__.</span></span> <span data-ttu-id="24e50-114">É um erro para que um argumento posicional apareça após um argumento nomeado em uma *lista de argumentos*.</span><span class="sxs-lookup"><span data-stu-id="24e50-114">It is an error for a positional argument to appear after a named argument in an *argument-list*.</span></span>

<span data-ttu-id="24e50-115">A proposta é remover esse erro e atualizar as regras para localizar o parâmetro correspondente para um argumento (§ 7.5.1.1):</span><span class="sxs-lookup"><span data-stu-id="24e50-115">The proposal is to remove this error and update the rules for finding the corresponding parameter for an argument (§7.5.1.1):</span></span>

<span data-ttu-id="24e50-116">Argumentos no argumento-lista de construtores de instância, métodos, indexadores e delegados:</span><span class="sxs-lookup"><span data-stu-id="24e50-116">Arguments in the argument-list of instance constructors, methods, indexers and delegates:</span></span>
- <span data-ttu-id="24e50-117">[regras existentes]</span><span class="sxs-lookup"><span data-stu-id="24e50-117">[existing rules]</span></span>
- <span data-ttu-id="24e50-118">Um argumento sem nome corresponde a nenhum parâmetro quando é depois de um argumento nomeado fora de posição ou um argumento chamado params.</span><span class="sxs-lookup"><span data-stu-id="24e50-118">An unnamed argument corresponds to no parameter when it is after an out-of-position named argument or a named params argument.</span></span>

<span data-ttu-id="24e50-119">Em particular, isso impede a invocação de `void M(bool a = true, bool b = true, bool c = true, );` com `M(c: false, valueB);`.</span><span class="sxs-lookup"><span data-stu-id="24e50-119">In particular, this prevents invoking `void M(bool a = true, bool b = true, bool c = true, );` with `M(c: false, valueB);`.</span></span> <span data-ttu-id="24e50-120">O primeiro argumento é usado fora de posição (o argumento é usado na primeira posição, mas o parâmetro chamado "c" está na terceira posição), portanto, os argumentos a seguir devem ser nomeados.</span><span class="sxs-lookup"><span data-stu-id="24e50-120">The first argument is used out-of-position (the argument is used in first position, but the parameter named "c" is in third position), so the following arguments should be named.</span></span>

<span data-ttu-id="24e50-121">Em outras palavras, argumentos nomeados não à direita são permitidos somente quando o nome e a posição resultam na localização do mesmo parâmetro correspondente.</span><span class="sxs-lookup"><span data-stu-id="24e50-121">In other words, non-trailing named arguments are only allowed when the name and the position result in finding the same corresponding parameter.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="24e50-122">Desvantagens</span><span class="sxs-lookup"><span data-stu-id="24e50-122">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="24e50-123">Essa proposta exacerba as sutilezas existentes com argumentos nomeados na resolução de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="24e50-123">This proposal exacerbates existing subtleties with named arguments in overload resolution.</span></span> <span data-ttu-id="24e50-124">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="24e50-124">For instance:</span></span>

```csharp
void M(int x, int y) { }
void M<T>(T y, int x) { }

void M2()
{
    M(3, 4);
    M(y: 3, x: 4); // Invokes M(int, int)
    M(y: 3, 4); // Invokes M<T>(T, int)
}
```

<span data-ttu-id="24e50-125">Você pode obter essa situação hoje alternando os parâmetros:</span><span class="sxs-lookup"><span data-stu-id="24e50-125">You could get this situation today by swapping the parameters:</span></span>

```csharp
void M(int y, int x) { }
void M<T>(int x, T y) { }

void M2()
{
    M(3, 4);
    M(x: 3, y: 4); // Invokes M(int, int)
    M(3, y: 4); // Invokes M<T>(int, T)
}
```

<span data-ttu-id="24e50-126">Da mesma forma, se você tiver dois métodos `void M(int a, int b)` e `void M(int x, string y)`, a invocação errada `M(x: 1, 2)` produzirá um diagnóstico baseado na segunda sobrecarga ("não é possível converter de ' int ' em ' String '").</span><span class="sxs-lookup"><span data-stu-id="24e50-126">Similarly, if you have two methods `void M(int a, int b)` and `void M(int x, string y)`, the mistaken invocation `M(x: 1, 2)` will produce a diagnostic based on the second overload ("cannot convert from 'int' to 'string'").</span></span> <span data-ttu-id="24e50-127">Esse problema já existe quando o argumento nomeado é usado em uma posição à direita.</span><span class="sxs-lookup"><span data-stu-id="24e50-127">This problem already exists when the named argument is used in a trailing position.</span></span>

## <a name="alternatives"></a><span data-ttu-id="24e50-128">Alternativas</span><span class="sxs-lookup"><span data-stu-id="24e50-128">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="24e50-129">Há algumas alternativas a serem consideradas:</span><span class="sxs-lookup"><span data-stu-id="24e50-129">There are a couple of alternatives to consider:</span></span>

- <span data-ttu-id="24e50-130">O status quo</span><span class="sxs-lookup"><span data-stu-id="24e50-130">The status quo</span></span>
- <span data-ttu-id="24e50-131">Fornecer assistência IDE para preencher todos os nomes de argumentos à direita quando você digita um nome específico no meio.</span><span class="sxs-lookup"><span data-stu-id="24e50-131">Providing IDE assistance to fill-in all the names of trailing arguments when you type specific a name in the middle.</span></span>

<span data-ttu-id="24e50-132">Ambos sofrem com mais detalhes, pois eles introduzem vários argumentos nomeados, mesmo que você precise apenas de um nome de um literal no início da lista de argumentos.</span><span class="sxs-lookup"><span data-stu-id="24e50-132">Both of those suffer from more verbosity, as they introduce multiple named arguments even if you just need one name of a literal at the beginning of the argument list.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="24e50-133">Perguntas não resolvidas</span><span class="sxs-lookup"><span data-stu-id="24e50-133">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a><span data-ttu-id="24e50-134">Criar reuniões</span><span class="sxs-lookup"><span data-stu-id="24e50-134">Design meetings</span></span>
[ldm]: #ldm
<span data-ttu-id="24e50-135">O recurso foi brevemente discutido no LDM em 16 de maio de 2017, com aprovação no princípio (OK para mudar para a proposta/protótipo).</span><span class="sxs-lookup"><span data-stu-id="24e50-135">The feature was briefly discussed in LDM on May 16th 2017, with approval in principle (ok to move to proposal/prototype).</span></span> <span data-ttu-id="24e50-136">Ele também foi brevemente discutido em 28 de junho de 2017.</span><span class="sxs-lookup"><span data-stu-id="24e50-136">It was also briefly discussed on June 28th 2017.</span></span>

<span data-ttu-id="24e50-137">Relacionado à discussão inicial https://github.com/dotnet/csharplang/issues/518 relacionado ao problema procedo https://github.com/dotnet/csharplang/issues/570</span><span class="sxs-lookup"><span data-stu-id="24e50-137">Relates to initial discussion https://github.com/dotnet/csharplang/issues/518 Relates to championed issue https://github.com/dotnet/csharplang/issues/570</span></span>
