---
ms.openlocfilehash: a0d80afc47e9f0073237db9b8d7a4f0b045c1b0b
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484506"
---
# <a name="out-variable-declarations"></a><span data-ttu-id="99a50-101">Declarações de variável out</span><span class="sxs-lookup"><span data-stu-id="99a50-101">Out variable declarations</span></span>

<span data-ttu-id="99a50-102">O recurso de *declaração de variável out* permite que uma variável seja declarada no local que está sendo passado como um argumento `out`.</span><span class="sxs-lookup"><span data-stu-id="99a50-102">The *out variable declaration* feature enables a variable to be declared at the location that it is being passed as an `out` argument.</span></span>

```antlr
argument_value
    : 'out' type identifier
    | ...
    ;
```

<span data-ttu-id="99a50-103">Uma variável declarada dessa forma é chamada de uma *variável out*.</span><span class="sxs-lookup"><span data-stu-id="99a50-103">A variable declared this way is called an *out variable*.</span></span> <span data-ttu-id="99a50-104">Você pode usar a palavra-chave contextual `var` para o tipo da variável.</span><span class="sxs-lookup"><span data-stu-id="99a50-104">You may use the contextual keyword `var` for the variable's type.</span></span> <span data-ttu-id="99a50-105">O escopo será o mesmo para uma variável de *padrão* introduzida por meio de correspondência de padrões.</span><span class="sxs-lookup"><span data-stu-id="99a50-105">The scope will be the same as for a *pattern-variable* introduced via pattern-matching.</span></span>

<span data-ttu-id="99a50-106">De acordo com a especificação da linguagem (seção acesso ao elemento 7.6.7), a lista de argumentos de um acesso de elemento (expressão de indexação) não contém argumentos ref ou out.</span><span class="sxs-lookup"><span data-stu-id="99a50-106">According to Language Specification (section 7.6.7 Element access) the argument-list of an element-access (indexing expression) does not contain ref or out arguments.</span></span> <span data-ttu-id="99a50-107">No entanto, eles são permitidos pelo compilador para vários cenários, por exemplo indexadores declarados em metadados que aceitam `out`.</span><span class="sxs-lookup"><span data-stu-id="99a50-107">However, they are permitted by the compiler for various scenarios, for example indexers declared in metadata that accept `out`.</span></span>

<span data-ttu-id="99a50-108">Dentro do escopo de uma variável local introduzida por um argument_value, é um erro de tempo de compilação para se referir a essa variável local em uma posição textual que precede sua declaração.</span><span class="sxs-lookup"><span data-stu-id="99a50-108">Within the scope of a local variable introduced by an argument_value, it is a compile-time error to refer to that local variable in a textual position that precedes its declaration.</span></span>

<span data-ttu-id="99a50-109">Também é um erro fazer referência a uma variável de saída de tipo implícito (§ 8.5.1) na mesma lista de argumentos que contém imediatamente sua declaração.</span><span class="sxs-lookup"><span data-stu-id="99a50-109">It is also an error to reference an implicitly-typed (§8.5.1) out variable in the same argument list that immediately contains its declaration.</span></span>

<span data-ttu-id="99a50-110">A resolução de sobrecarga é modificada da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="99a50-110">Overload resolution is modified as follows:</span></span>

<span data-ttu-id="99a50-111">Adicionamos uma nova conversão:</span><span class="sxs-lookup"><span data-stu-id="99a50-111">We add a new conversion:</span></span>

> <span data-ttu-id="99a50-112">Há uma *conversão de expressão* de uma declaração de variável digitada implicitamente para cada tipo.</span><span class="sxs-lookup"><span data-stu-id="99a50-112">There is a *conversion from expression* from an implicitly-typed out variable declaration to every type.</span></span>

<span data-ttu-id="99a50-113">Também</span><span class="sxs-lookup"><span data-stu-id="99a50-113">Also</span></span>

> <span data-ttu-id="99a50-114">O tipo de um argumento de variável explicitamente digitado é o tipo declarado.</span><span class="sxs-lookup"><span data-stu-id="99a50-114">The type of an explicitly-typed out variable argument is the declared type.</span></span>

<span data-ttu-id="99a50-115">e</span><span class="sxs-lookup"><span data-stu-id="99a50-115">and</span></span>

> <span data-ttu-id="99a50-116">Um argumento de variável de tipo implícito não tem nenhum tipo.</span><span class="sxs-lookup"><span data-stu-id="99a50-116">An implicitly-typed out variable argument has no type.</span></span>

<span data-ttu-id="99a50-117">A *conversão de expressão* de uma declaração de variável de tipo implícito não é considerada melhor do que qualquer outra *conversão da expressão*.</span><span class="sxs-lookup"><span data-stu-id="99a50-117">The *conversion from expression* from an implicitly-typed out variable declaration is not considered better than any other *conversion from expression*.</span></span>

<span data-ttu-id="99a50-118">O tipo de uma variável de digitação implícita é o tipo do parâmetro correspondente na assinatura do método selecionado pela resolução de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="99a50-118">The type of an implicitly-typed out variable is the type of the corresponding parameter in the signature of the method selected by overload resolution.</span></span>

<span data-ttu-id="99a50-119">O novo nó de sintaxe `DeclarationExpressionSyntax` é adicionado para representar a declaração em um argumento var out.</span><span class="sxs-lookup"><span data-stu-id="99a50-119">The new syntax node `DeclarationExpressionSyntax` is added to represent the declaration in an out var argument.</span></span>
