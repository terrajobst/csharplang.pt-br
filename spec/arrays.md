---
ms.openlocfilehash: 155c1beecddfdfcce2e7948bcb8d6b80428fbd7a
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229490"
---
# <a name="arrays"></a><span data-ttu-id="03d86-101">Matrizes</span><span class="sxs-lookup"><span data-stu-id="03d86-101">Arrays</span></span>

<span data-ttu-id="03d86-102">Uma matriz é uma estrutura de dados que contém um número de variáveis que são acessados por meio de índices calculados.</span><span class="sxs-lookup"><span data-stu-id="03d86-102">An array is a data structure that contains a number of variables which are accessed through computed indices.</span></span> <span data-ttu-id="03d86-103">As variáveis contidas em uma matriz, também chamada de elementos da matriz, são todos do mesmo tipo, e esse tipo é chamado o tipo de elemento da matriz.</span><span class="sxs-lookup"><span data-stu-id="03d86-103">The variables contained in an array, also called the elements of the array, are all of the same type, and this type is called the element type of the array.</span></span>

<span data-ttu-id="03d86-104">Uma matriz tem uma classificação que determina o número de índices associados com cada elemento da matriz.</span><span class="sxs-lookup"><span data-stu-id="03d86-104">An array has a rank which determines the number of indices associated with each array element.</span></span> <span data-ttu-id="03d86-105">A classificação de uma matriz também é chamada como as dimensões da matriz.</span><span class="sxs-lookup"><span data-stu-id="03d86-105">The rank of an array is also referred to as the dimensions of the array.</span></span> <span data-ttu-id="03d86-106">Uma matriz com uma classificação de um é chamada um ***matriz unidimensional***.</span><span class="sxs-lookup"><span data-stu-id="03d86-106">An array with a rank of one is called a ***single-dimensional array***.</span></span> <span data-ttu-id="03d86-107">Uma matriz com uma classificação maior do que uma é chamada de um ***matriz multidimensional***.</span><span class="sxs-lookup"><span data-stu-id="03d86-107">An array with a rank greater than one is called a ***multi-dimensional array***.</span></span> <span data-ttu-id="03d86-108">Matrizes multidimensionais de tamanhos específicos são geralmente denominados matrizes bidimensionais, matrizes tridimensionais e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="03d86-108">Specific sized multi-dimensional arrays are often referred to as two-dimensional arrays, three-dimensional arrays, and so on.</span></span>

<span data-ttu-id="03d86-109">Cada dimensão de uma matriz tem um comprimento associado que é um número inteiro maior ou igual a zero.</span><span class="sxs-lookup"><span data-stu-id="03d86-109">Each dimension of an array has an associated length which is an integral number greater than or equal to zero.</span></span> <span data-ttu-id="03d86-110">Os tamanhos da dimensão não fazem parte do tipo de matriz, mas em vez disso, são definidos quando uma instância do tipo de matriz é criada em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="03d86-110">The dimension lengths are not part of the type of the array, but rather are established when an instance of the array type is created at run-time.</span></span> <span data-ttu-id="03d86-111">O comprimento de uma dimensão determina o intervalo válido de índices para a dimensão: Para uma dimensão de comprimento `N`, índices podem variar desde `0` para `N - 1` inclusivo.</span><span class="sxs-lookup"><span data-stu-id="03d86-111">The length of a dimension determines the valid range of indices for that dimension: For a dimension of length `N`, indices can range from `0` to `N - 1` inclusive.</span></span> <span data-ttu-id="03d86-112">O número total de elementos em uma matriz é o produto dos comprimentos de cada dimensão da matriz.</span><span class="sxs-lookup"><span data-stu-id="03d86-112">The total number of elements in an array is the product of the lengths of each dimension in the array.</span></span> <span data-ttu-id="03d86-113">Se um ou mais das dimensões de uma matriz tem um comprimento igual a zero, a matriz deve estar vazio.</span><span class="sxs-lookup"><span data-stu-id="03d86-113">If one or more of the dimensions of an array have a length of zero, the array is said to be empty.</span></span>

<span data-ttu-id="03d86-114">O tipo do elemento de uma matriz pode ser qualquer tipo, incluindo um tipo de matriz.</span><span class="sxs-lookup"><span data-stu-id="03d86-114">The element type of an array can be any type, including an array type.</span></span>

## <a name="array-types"></a><span data-ttu-id="03d86-115">Tipos de matriz</span><span class="sxs-lookup"><span data-stu-id="03d86-115">Array types</span></span>

<span data-ttu-id="03d86-116">Um tipo de matriz é escrito como um *non_array_type* seguido por um ou mais *rank_specifier*s:</span><span class="sxs-lookup"><span data-stu-id="03d86-116">An array type is written as a *non_array_type* followed by one or more *rank_specifier*s:</span></span>

```antlr
array_type
    : non_array_type rank_specifier+
    ;

non_array_type
    : type
    ;

rank_specifier
    : '[' dim_separator* ']'
    ;

dim_separator
    : ','
    ;
```

<span data-ttu-id="03d86-117">Um *non_array_type* é qualquer *tipo* que é não por uma *array_type*.</span><span class="sxs-lookup"><span data-stu-id="03d86-117">A *non_array_type* is any *type* that is not itself an *array_type*.</span></span>

<span data-ttu-id="03d86-118">A classificação de um tipo de matriz é determinada pela mais à esquerda *rank_specifier* na *array_type*: Um *rank_specifier* indica que a matriz é uma matriz com uma classificação de um mais o número de "`,`" tokens no *rank_specifier*.</span><span class="sxs-lookup"><span data-stu-id="03d86-118">The rank of an array type is given by the leftmost *rank_specifier* in the *array_type*: A *rank_specifier* indicates that the array is an array with a rank of one plus the number of "`,`" tokens in the *rank_specifier*.</span></span>

<span data-ttu-id="03d86-119">O tipo de elemento de um tipo de matriz é o tipo que é o resultado da exclusão de mais à esquerda *rank_specifier*:</span><span class="sxs-lookup"><span data-stu-id="03d86-119">The element type of an array type is the type that results from deleting the leftmost *rank_specifier*:</span></span>

*  <span data-ttu-id="03d86-120">Um tipo de matriz no formato `T[R]` é uma matriz com rank `R` e um tipo de elemento de não matriz `T`.</span><span class="sxs-lookup"><span data-stu-id="03d86-120">An array type of the form `T[R]` is an array with rank `R` and a non-array element type `T`.</span></span>
*  <span data-ttu-id="03d86-121">Um tipo de matriz no formato `T[R][R1]...[Rn]` é uma matriz com rank `R` e um tipo de elemento `T[R1]...[Rn]`.</span><span class="sxs-lookup"><span data-stu-id="03d86-121">An array type of the form `T[R][R1]...[Rn]` is an array with rank `R` and an element type `T[R1]...[Rn]`.</span></span>

<span data-ttu-id="03d86-122">Na verdade, o *rank_specifier*s são lidos da esquerda para a direita antes do tipo de elemento de não matriz final.</span><span class="sxs-lookup"><span data-stu-id="03d86-122">In effect, the *rank_specifier*s are read from left to right before the final non-array element type.</span></span> <span data-ttu-id="03d86-123">O tipo `int[][,,][,]` é uma matriz unidimensional de matrizes tridimensionais de matrizes bidimensionais de `int`.</span><span class="sxs-lookup"><span data-stu-id="03d86-123">The type `int[][,,][,]` is a single-dimensional array of three-dimensional arrays of two-dimensional arrays of `int`.</span></span>

<span data-ttu-id="03d86-124">Em tempo de execução, um valor de um tipo de matriz pode ser `null` ou uma referência a uma instância desse tipo de matriz.</span><span class="sxs-lookup"><span data-stu-id="03d86-124">At run-time, a value of an array type can be `null` or a reference to an instance of that array type.</span></span>

### <a name="the-systemarray-type"></a><span data-ttu-id="03d86-125">O tipo System. Array</span><span class="sxs-lookup"><span data-stu-id="03d86-125">The System.Array type</span></span>

<span data-ttu-id="03d86-126">O tipo `System.Array` é o tipo base abstrato de todos os tipos de matriz.</span><span class="sxs-lookup"><span data-stu-id="03d86-126">The type `System.Array` is the abstract base type of all array types.</span></span> <span data-ttu-id="03d86-127">Uma conversão implícita de referência ([conversões de referência implícita](conversions.md#implicit-reference-conversions)) existe a partir de qualquer tipo de matriz `System.Array`e uma conversão de referência explícita ([conversões de referência explícita](conversions.md#explicit-reference-conversions)) existe de `System.Array` para qualquer tipo de matriz.</span><span class="sxs-lookup"><span data-stu-id="03d86-127">An implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from any array type to `System.Array`, and an explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) exists from `System.Array` to any array type.</span></span> <span data-ttu-id="03d86-128">Observe que `System.Array` não é em si um *array_type*.</span><span class="sxs-lookup"><span data-stu-id="03d86-128">Note that `System.Array` is not itself an *array_type*.</span></span> <span data-ttu-id="03d86-129">Em vez disso, ele é um *class_type* da qual todas as *array_type*s são derivados.</span><span class="sxs-lookup"><span data-stu-id="03d86-129">Rather, it is a *class_type* from which all *array_type*s are derived.</span></span>

<span data-ttu-id="03d86-130">Em tempo de execução, um valor do tipo `System.Array` pode ser `null` ou uma referência a uma instância de qualquer tipo de matriz.</span><span class="sxs-lookup"><span data-stu-id="03d86-130">At run-time, a value of type `System.Array` can be `null` or a reference to an instance of any array type.</span></span>

### <a name="arrays-and-the-generic-ilist-interface"></a><span data-ttu-id="03d86-131">Matrizes e a interface IList genérica</span><span class="sxs-lookup"><span data-stu-id="03d86-131">Arrays and the generic IList interface</span></span>

<span data-ttu-id="03d86-132">Uma matriz unidimensional `T[]` implementa a interface `System.Collections.Generic.IList<T>` (`IList<T>` de forma abreviada) e suas interfaces base.</span><span class="sxs-lookup"><span data-stu-id="03d86-132">A one-dimensional array `T[]` implements the interface `System.Collections.Generic.IList<T>` (`IList<T>` for short) and its base interfaces.</span></span> <span data-ttu-id="03d86-133">Da mesma forma, há uma conversão implícita da `T[]` para `IList<T>` e suas interfaces base.</span><span class="sxs-lookup"><span data-stu-id="03d86-133">Accordingly, there is an implicit conversion from `T[]` to `IList<T>` and its base interfaces.</span></span> <span data-ttu-id="03d86-134">Além disso, se houver uma conversão de referência implícita da `S` para `T` , em seguida, `S[]` implementa `IList<T>` e não há uma conversão de referência implícita da `S[]` para `IList<T>` e sua base de interfaces ( [Conversões de referência implícita](conversions.md#implicit-reference-conversions)).</span><span class="sxs-lookup"><span data-stu-id="03d86-134">In addition, if there is an implicit reference conversion from `S` to `T` then `S[]` implements `IList<T>` and there is an implicit reference conversion from `S[]` to `IList<T>` and its base interfaces ([Implicit reference conversions](conversions.md#implicit-reference-conversions)).</span></span> <span data-ttu-id="03d86-135">Se não houver uma conversão de referência explícita de `S` para `T` e em seguida, há uma conversão de referência explícita de `S[]` à `IList<T>` e suas interfaces base ([conversões de referência explícita](conversions.md#explicit-reference-conversions)).</span><span class="sxs-lookup"><span data-stu-id="03d86-135">If there is an explicit reference conversion from `S` to `T` then there is an explicit reference conversion from `S[]` to `IList<T>` and its base interfaces ([Explicit reference conversions](conversions.md#explicit-reference-conversions)).</span></span> <span data-ttu-id="03d86-136">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="03d86-136">For example:</span></span>
```csharp
using System.Collections.Generic;

class Test
{
    static void Main() {
        string[] sa = new string[5];
        object[] oa1 = new object[5];
        object[] oa2 = sa;

        IList<string> lst1 = sa;                    // Ok
        IList<string> lst2 = oa1;                   // Error, cast needed
        IList<object> lst3 = sa;                    // Ok
        IList<object> lst4 = oa1;                   // Ok

        IList<string> lst5 = (IList<string>)oa1;    // Exception
        IList<string> lst6 = (IList<string>)oa2;    // Ok
    }
}
```

<span data-ttu-id="03d86-137">A atribuição `lst2 = oa1` gera um erro de tempo de compilação desde a conversão de `object[]` para `IList<string>` é uma conversão explícita, não é implícita.</span><span class="sxs-lookup"><span data-stu-id="03d86-137">The assignment `lst2 = oa1` generates a compile-time error since the conversion from `object[]` to `IList<string>` is an explicit conversion, not implicit.</span></span> <span data-ttu-id="03d86-138">A conversão `(IList<string>)oa1` fará com que uma exceção seja lançada em tempo de execução desde `oa1` referências um `object[]` e não um `string[]`.</span><span class="sxs-lookup"><span data-stu-id="03d86-138">The cast `(IList<string>)oa1` will cause an exception to be thrown at run-time since `oa1` references an `object[]` and not a `string[]`.</span></span> <span data-ttu-id="03d86-139">No entanto a conversão `(IList<string>)oa2` não causará uma exceção seja lançada desde `oa2` referências um `string[]`.</span><span class="sxs-lookup"><span data-stu-id="03d86-139">However the cast `(IList<string>)oa2` will not cause an exception to be thrown since `oa2` references a `string[]`.</span></span>

<span data-ttu-id="03d86-140">Sempre que houver uma conversão de referência implícita ou explícita de `S[]` ao `IList<T>`, também há uma conversão de referência explícita do `IList<T>` e sua base de interfaces para `S[]` ([referência explícita conversões](conversions.md#explicit-reference-conversions)).</span><span class="sxs-lookup"><span data-stu-id="03d86-140">Whenever there is an implicit or explicit reference conversion from `S[]` to `IList<T>`, there is also an explicit reference conversion from `IList<T>` and its base interfaces to `S[]` ([Explicit reference conversions](conversions.md#explicit-reference-conversions)).</span></span>

<span data-ttu-id="03d86-141">Quando um tipo de matriz `S[]` implementa `IList<T>`, alguns dos membros da interface implementada podem lançar exceções.</span><span class="sxs-lookup"><span data-stu-id="03d86-141">When an array type `S[]` implements `IList<T>`, some of the members of the implemented interface may throw exceptions.</span></span> <span data-ttu-id="03d86-142">O comportamento preciso da implementação da interface está além do escopo desta especificação.</span><span class="sxs-lookup"><span data-stu-id="03d86-142">The precise behavior of the implementation of the interface is beyond the scope of this specification.</span></span>

## <a name="array-creation"></a><span data-ttu-id="03d86-143">Criação de matriz</span><span class="sxs-lookup"><span data-stu-id="03d86-143">Array creation</span></span>

<span data-ttu-id="03d86-144">Instâncias de matriz são criadas pelo *array_creation_expression*s ([expressões de criação de matriz](expressions.md#array-creation-expressions)) ou por campo ou declarações de variável local que incluem um *array_initializer*([Inicializadores de matriz](arrays.md#array-initializers)).</span><span class="sxs-lookup"><span data-stu-id="03d86-144">Array instances are created by *array_creation_expression*s ([Array creation expressions](expressions.md#array-creation-expressions)) or by field or local variable declarations that include an *array_initializer* ([Array initializers](arrays.md#array-initializers)).</span></span>

<span data-ttu-id="03d86-145">Quando uma instância de matriz é criada, a classificação e o tamanho de cada dimensão são estabelecidas e permanecem constantes para o tempo de vida inteiro da instância.</span><span class="sxs-lookup"><span data-stu-id="03d86-145">When an array instance is created, the rank and length of each dimension are established and then remain constant for the entire lifetime of the instance.</span></span> <span data-ttu-id="03d86-146">Em outras palavras, não é possível alterar a classificação de uma instância de matriz existente, nem é possível redimensionar suas dimensões.</span><span class="sxs-lookup"><span data-stu-id="03d86-146">In other words, it is not possible to change the rank of an existing array instance, nor is it possible to resize its dimensions.</span></span>

<span data-ttu-id="03d86-147">Uma instância de matriz sempre é de um tipo de matriz.</span><span class="sxs-lookup"><span data-stu-id="03d86-147">An array instance is always of an array type.</span></span> <span data-ttu-id="03d86-148">O `System.Array` tipo é um tipo abstrato que não pode ser instanciado.</span><span class="sxs-lookup"><span data-stu-id="03d86-148">The `System.Array` type is an abstract type that cannot be instantiated.</span></span>

<span data-ttu-id="03d86-149">Elementos de matrizes criados pelo *array_creation_expression*s sempre são inicializados com seus valores padrão ([valores padrão](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="03d86-149">Elements of arrays created by *array_creation_expression*s are always initialized to their default value ([Default values](variables.md#default-values)).</span></span>

## <a name="array-element-access"></a><span data-ttu-id="03d86-150">Acesso de elemento de matriz</span><span class="sxs-lookup"><span data-stu-id="03d86-150">Array element access</span></span>

<span data-ttu-id="03d86-151">Elementos da matriz são acessados usando *element_access* expressões ([acesso de matriz](expressions.md#array-access)) do formulário `A[I1, I2, ..., In]`, onde `A` é uma expressão de um tipo de matriz e cada `Ix` é um expressão do tipo `int`, `uint`, `long`, `ulong`, ou pode ser convertido implicitamente em um ou mais desses tipos.</span><span class="sxs-lookup"><span data-stu-id="03d86-151">Array elements are accessed using *element_access* expressions ([Array access](expressions.md#array-access)) of the form `A[I1, I2, ..., In]`, where `A` is an expression of an array type and each `Ix` is an expression of type `int`, `uint`, `long`, `ulong`, or can be implicitly converted to one or more of these types.</span></span> <span data-ttu-id="03d86-152">O resultado de um acesso de elemento de matriz é uma variável, ou seja, o elemento da matriz selecionada pelos índices.</span><span class="sxs-lookup"><span data-stu-id="03d86-152">The result of an array element access is a variable, namely the array element selected by the indices.</span></span>

<span data-ttu-id="03d86-153">Os elementos de uma matriz podem ser enumerados usando um `foreach` instrução ([a instrução foreach](statements.md#the-foreach-statement)).</span><span class="sxs-lookup"><span data-stu-id="03d86-153">The elements of an array can be enumerated using a `foreach` statement ([The foreach statement](statements.md#the-foreach-statement)).</span></span>

## <a name="array-members"></a><span data-ttu-id="03d86-154">Membros da matriz</span><span class="sxs-lookup"><span data-stu-id="03d86-154">Array members</span></span>

<span data-ttu-id="03d86-155">Cada tipo de matriz herda os membros declarados pelo `System.Array` tipo.</span><span class="sxs-lookup"><span data-stu-id="03d86-155">Every array type inherits the members declared by the `System.Array` type.</span></span>

## <a name="array-covariance"></a><span data-ttu-id="03d86-156">Covariância de matriz</span><span class="sxs-lookup"><span data-stu-id="03d86-156">Array covariance</span></span>

<span data-ttu-id="03d86-157">Para quaisquer dois *reference_type*s `A` e `B`, se uma conversão implícita de referência ([conversões de referência implícita](conversions.md#implicit-reference-conversions)) ou conversão de referência explícita ([ Conversões de referência explícita](conversions.md#explicit-reference-conversions)) existe na `A` à `B`, em seguida, também existe a mesma conversão de referência do tipo de matriz `A[R]` para o tipo de matriz `B[R]`, onde `R` é qualquer determinada *rank_specifier* (mas o mesmo para ambos os tipos de matriz).</span><span class="sxs-lookup"><span data-stu-id="03d86-157">For any two *reference_type*s `A` and `B`, if an implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) or explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) exists from `A` to `B`, then the same reference conversion also exists from the array type `A[R]` to the array type `B[R]`, where `R` is any given *rank_specifier* (but the same for both array types).</span></span> <span data-ttu-id="03d86-158">Essa relação é conhecida como ***covariância de matriz***.</span><span class="sxs-lookup"><span data-stu-id="03d86-158">This relationship is known as ***array covariance***.</span></span> <span data-ttu-id="03d86-159">Covariância de matriz em particular significa que um valor de um tipo de matriz `A[R]` , na verdade, pode ser uma referência a uma instância de um tipo de matriz `B[R]`, desde que existe uma conversão de referência implícita da `B` para `A`.</span><span class="sxs-lookup"><span data-stu-id="03d86-159">Array covariance in particular means that a value of an array type `A[R]` may actually be a reference to an instance of an array type `B[R]`, provided an implicit reference conversion exists from `B` to `A`.</span></span>

<span data-ttu-id="03d86-160">Devido a covariância de matriz, atribuições aos elementos de matrizes de tipo de referência incluem uma verificação de tempo de execução que garante que o valor que está sendo atribuído ao elemento de matriz, na verdade, seja de um tipo permitido ([atribuição simples](expressions.md#simple-assignment)).</span><span class="sxs-lookup"><span data-stu-id="03d86-160">Because of array covariance, assignments to elements of reference type arrays include a run-time check which ensures that the value being assigned to the array element is actually of a permitted type ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="03d86-161">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="03d86-161">For example:</span></span>
```csharp
class Test
{
    static void Fill(object[] array, int index, int count, object value) {
        for (int i = index; i < index + count; i++) array[i] = value;
    }

    static void Main() {
        string[] strings = new string[100];
        Fill(strings, 0, 100, "Undefined");
        Fill(strings, 0, 10, null);
        Fill(strings, 90, 10, 0);
    }
}
```

<span data-ttu-id="03d86-162">A atribuição ao `array[i]` no `Fill` método implicitamente inclui uma verificação de tempo de execução que garante que o objeto referenciado pela `value` seja `null` ou uma instância que é compatível com o tipo de elemento real do `array`.</span><span class="sxs-lookup"><span data-stu-id="03d86-162">The assignment to `array[i]` in the `Fill` method implicitly includes a run-time check which ensures that the object referenced by `value` is either `null` or an instance that is compatible with the actual element type of `array`.</span></span> <span data-ttu-id="03d86-163">Na `Main`, as duas primeiras invocações de `Fill` bem-sucedida, mas as causas de invocação de terceiro uma `System.ArrayTypeMismatchException` seja lançada após executar a primeira atribuição para `array[i]`.</span><span class="sxs-lookup"><span data-stu-id="03d86-163">In `Main`, the first two invocations of `Fill` succeed, but the third invocation causes a `System.ArrayTypeMismatchException` to be thrown upon executing the first assignment to `array[i]`.</span></span> <span data-ttu-id="03d86-164">A exceção ocorre porque um demarcado `int` não podem ser armazenados em um `string` matriz.</span><span class="sxs-lookup"><span data-stu-id="03d86-164">The exception occurs because a boxed `int` cannot be stored in a `string` array.</span></span>

<span data-ttu-id="03d86-165">Covariância de matriz especificamente não se estende a matrizes de *value_type*s.</span><span class="sxs-lookup"><span data-stu-id="03d86-165">Array covariance specifically does not extend to arrays of *value_type*s.</span></span> <span data-ttu-id="03d86-166">Por exemplo, não existe conversão que permite que um `int[]` deve ser tratado como um `object[]`.</span><span class="sxs-lookup"><span data-stu-id="03d86-166">For example, no conversion exists that permits an `int[]` to be treated as an `object[]`.</span></span>

## <a name="array-initializers"></a><span data-ttu-id="03d86-167">Inicializadores de matriz</span><span class="sxs-lookup"><span data-stu-id="03d86-167">Array initializers</span></span>

<span data-ttu-id="03d86-168">Inicializadores de matriz podem ser especificados em declarações de campo ([campos](classes.md#fields)), declarações de variável local ([declarações de variável Local](statements.md#local-variable-declarations)) e expressões de criação de matriz ([criação de matriz expressões](expressions.md#array-creation-expressions)):</span><span class="sxs-lookup"><span data-stu-id="03d86-168">Array initializers may be specified in field declarations ([Fields](classes.md#fields)), local variable declarations ([Local variable declarations](statements.md#local-variable-declarations)), and array creation expressions ([Array creation expressions](expressions.md#array-creation-expressions)):</span></span>

```antlr
array_initializer
    : '{' variable_initializer_list? '}'
    | '{' variable_initializer_list ',' '}'
    ;

variable_initializer_list
    : variable_initializer (',' variable_initializer)*
    ;

variable_initializer
    : expression
    | array_initializer
    ;
```

<span data-ttu-id="03d86-169">Um inicializador de matriz consiste em uma sequência de inicializadores de variável, delimitados por "`{`"e"`}`"tokens e separados por"`,`" tokens.</span><span class="sxs-lookup"><span data-stu-id="03d86-169">An array initializer consists of a sequence of variable initializers, enclosed by "`{`" and "`}`" tokens and separated by "`,`" tokens.</span></span> <span data-ttu-id="03d86-170">Cada inicializador de variável é uma expressão ou, no caso de uma matriz multidimensional, um inicializador de matriz aninhados.</span><span class="sxs-lookup"><span data-stu-id="03d86-170">Each variable initializer is an expression or, in the case of a multi-dimensional array, a nested array initializer.</span></span>

<span data-ttu-id="03d86-171">O contexto no qual um inicializador de matriz é usado determina o tipo da matriz que está sendo inicializado.</span><span class="sxs-lookup"><span data-stu-id="03d86-171">The context in which an array initializer is used determines the type of the array being initialized.</span></span> <span data-ttu-id="03d86-172">Em uma expressão de criação de matriz, o tipo de matriz imediatamente precede o inicializador, ou é inferido das expressões no inicializador de matriz.</span><span class="sxs-lookup"><span data-stu-id="03d86-172">In an array creation expression, the array type immediately precedes the initializer, or is inferred from the expressions in the array initializer.</span></span> <span data-ttu-id="03d86-173">Em um campo ou uma declaração de variável, o tipo de matriz é o tipo do campo ou variável sendo declarada.</span><span class="sxs-lookup"><span data-stu-id="03d86-173">In a field or variable declaration, the array type is the type of the field or variable being declared.</span></span> <span data-ttu-id="03d86-174">Quando um inicializador de matriz é usado em um campo ou uma declaração de variável, como:</span><span class="sxs-lookup"><span data-stu-id="03d86-174">When an array initializer is used in a field or variable declaration, such as:</span></span>
```csharp
int[] a = {0, 2, 4, 6, 8};
```
<span data-ttu-id="03d86-175">ele é simplesmente uma abreviação para uma expressão de criação de matriz equivalente:</span><span class="sxs-lookup"><span data-stu-id="03d86-175">it is simply shorthand for an equivalent array creation expression:</span></span>
```csharp
int[] a = new int[] {0, 2, 4, 6, 8};
```

<span data-ttu-id="03d86-176">Para uma matriz unidimensional, o inicializador de matriz deve conter uma sequência de expressões de atribuição compatível com o tipo de elemento da matriz.</span><span class="sxs-lookup"><span data-stu-id="03d86-176">For a single-dimensional array, the array initializer must consist of a sequence of expressions that are assignment compatible with the element type of the array.</span></span> <span data-ttu-id="03d86-177">As expressões de inicializar os elementos da matriz em ordem crescente, começando com o elemento no índice zero.</span><span class="sxs-lookup"><span data-stu-id="03d86-177">The expressions initialize array elements in increasing order, starting with the element at index zero.</span></span> <span data-ttu-id="03d86-178">O número de expressões no inicializador de matriz determina o tamanho da instância de matriz que está sendo criado.</span><span class="sxs-lookup"><span data-stu-id="03d86-178">The number of expressions in the array initializer determines the length of the array instance being created.</span></span> <span data-ttu-id="03d86-179">Por exemplo, o inicializador de matriz acima cria um `int[]` instância de comprimento 5 e, em seguida, inicializa a instância com os seguintes valores:</span><span class="sxs-lookup"><span data-stu-id="03d86-179">For example, the array initializer above creates an `int[]` instance of length 5 and then initializes the instance with the following values:</span></span>
```csharp
a[0] = 0; a[1] = 2; a[2] = 4; a[3] = 6; a[4] = 8;
```

<span data-ttu-id="03d86-180">Para uma matriz multidimensional, o inicializador de matriz deve ter tantos níveis de aninhamento quanto há dimensões na matriz.</span><span class="sxs-lookup"><span data-stu-id="03d86-180">For a multi-dimensional array, the array initializer must have as many levels of nesting as there are dimensions in the array.</span></span> <span data-ttu-id="03d86-181">O nível de aninhamento mais externo corresponde à dimensão mais à esquerda e o nível de aninhamento mais interno corresponde à dimensão mais à direita.</span><span class="sxs-lookup"><span data-stu-id="03d86-181">The outermost nesting level corresponds to the leftmost dimension and the innermost nesting level corresponds to the rightmost dimension.</span></span> <span data-ttu-id="03d86-182">O comprimento de cada dimensão da matriz é determinado pelo número de elementos no nível de aninhamento correspondente no inicializador de matriz.</span><span class="sxs-lookup"><span data-stu-id="03d86-182">The length of each dimension of the array is determined by the number of elements at the corresponding nesting level in the array initializer.</span></span> <span data-ttu-id="03d86-183">Para cada inicializador de matriz aninhados, o número de elementos deve ser o mesmo que outros inicializadores de matriz no mesmo nível.</span><span class="sxs-lookup"><span data-stu-id="03d86-183">For each nested array initializer, the number of elements must be the same as the other array initializers at the same level.</span></span> <span data-ttu-id="03d86-184">O Exemplo:</span><span class="sxs-lookup"><span data-stu-id="03d86-184">The example:</span></span>
```csharp
int[,] b = {{0, 1}, {2, 3}, {4, 5}, {6, 7}, {8, 9}};
```
<span data-ttu-id="03d86-185">cria uma matriz bidimensional com um comprimento de cinco para a dimensão mais à esquerda e um comprimento de dois para a dimensão mais à direita:</span><span class="sxs-lookup"><span data-stu-id="03d86-185">creates a two-dimensional array with a length of five for the leftmost dimension and a length of two for the rightmost dimension:</span></span>
```csharp
int[,] b = new int[5, 2];
```
<span data-ttu-id="03d86-186">e, em seguida, inicializa a matriz da instância com os seguintes valores:</span><span class="sxs-lookup"><span data-stu-id="03d86-186">and then initializes the array instance with the following values:</span></span>
```csharp
b[0, 0] = 0; b[0, 1] = 1;
b[1, 0] = 2; b[1, 1] = 3;
b[2, 0] = 4; b[2, 1] = 5;
b[3, 0] = 6; b[3, 1] = 7;
b[4, 0] = 8; b[4, 1] = 9;
```

<span data-ttu-id="03d86-187">Se uma dimensão que não seja mais à direita é fornecida com comprimento zero, as dimensões subsequentes serão consideradas como também tem comprimento zero.</span><span class="sxs-lookup"><span data-stu-id="03d86-187">If a dimension other than the rightmost is given with length zero, the subsequent dimensions are assumed to also have length zero.</span></span> <span data-ttu-id="03d86-188">O Exemplo:</span><span class="sxs-lookup"><span data-stu-id="03d86-188">The example:</span></span>
```csharp
int[,] c = {};
```
<span data-ttu-id="03d86-189">cria uma matriz bidimensional com um comprimento de zero para o mais à esquerda e a dimensão mais à direita:</span><span class="sxs-lookup"><span data-stu-id="03d86-189">creates a two-dimensional array with a length of zero for both the leftmost and the rightmost dimension:</span></span>
```csharp
int[,] c = new int[0, 0];
```

<span data-ttu-id="03d86-190">Quando uma expressão de criação de matriz inclui tamanhos da dimensão explícita e um inicializador de matriz, os comprimentos devem ser expressões constantes e o número de elementos em cada nível de aninhamento deve corresponder o tamanho da dimensão correspondente.</span><span class="sxs-lookup"><span data-stu-id="03d86-190">When an array creation expression includes both explicit dimension lengths and an array initializer, the lengths must be constant expressions and the number of elements at each nesting level must match the corresponding dimension length.</span></span> <span data-ttu-id="03d86-191">Estes são alguns exemplos:</span><span class="sxs-lookup"><span data-stu-id="03d86-191">Here are some examples:</span></span>
```csharp
int i = 3;
int[] x = new int[3] {0, 1, 2};        // OK
int[] y = new int[i] {0, 1, 2};        // Error, i not a constant
int[] z = new int[3] {0, 1, 2, 3};     // Error, length/initializer mismatch
```

<span data-ttu-id="03d86-192">Aqui, o inicializador para `y` resulta em um erro de tempo de compilação, porque a expressão de comprimento da dimensão não é uma constante e o inicializador para `z` resulta em um erro de tempo de compilação porque o comprimento e o número de elementos no inicializador não concordar.</span><span class="sxs-lookup"><span data-stu-id="03d86-192">Here, the initializer for `y` results in a compile-time error because the dimension length expression is not a constant, and the initializer for `z` results in a compile-time error because the length and the number of elements in the initializer do not agree.</span></span>
