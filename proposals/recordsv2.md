---
ms.openlocfilehash: 5636157ba54e93587847d6f2f7aac2dc675f3112
ms.sourcegitcommit: af27912886f22cda9b98b7769447d85cd9732736
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/09/2019
ms.locfileid: "79484996"
---

# <a name="records-v2"></a><span data-ttu-id="2da7d-101">Registros v2</span><span class="sxs-lookup"><span data-stu-id="2da7d-101">Records v2</span></span>

<span data-ttu-id="2da7d-102">No passado, pensamos sobre os registros como um recurso para habilitar o trabalho com dados.</span><span class="sxs-lookup"><span data-stu-id="2da7d-102">In the past we've thought about records as a feature to enable working with data.</span></span>

<span data-ttu-id="2da7d-103">"Trabalhar com dados" é um grande grupo com várias facetas, portanto, pode ser interessante examinar cada um deles isoladamente.</span><span class="sxs-lookup"><span data-stu-id="2da7d-103">"Working with data" is a big group with a number of facets, so it may be interesting to look at each in isolation.</span></span> <span data-ttu-id="2da7d-104">Vamos começar examinando um exemplo de registros hoje e algumas das suas desvantagens.</span><span class="sxs-lookup"><span data-stu-id="2da7d-104">Let's start by looking at an example of records today and some of its drawbacks.</span></span>

<span data-ttu-id="2da7d-105">Por exemplo, um registro simples seria definido hoje da seguinte maneira</span><span class="sxs-lookup"><span data-stu-id="2da7d-105">For instance, a simple record would be defined today as follows</span></span>

```C#
public class UserInfo
{
    public string Username { get; set; }
    public string Email { get; set; }
    public bool IsAdmin  { get; set; } = false;
}
```

<span data-ttu-id="2da7d-106">e o uso seria lido</span><span class="sxs-lookup"><span data-stu-id="2da7d-106">and the usage would read</span></span>

```C#
void M()
{
    var userInfo = new UserInfo() 
    {
        Username = "andy",
        Email = "angocke@microsoft.com",
        IsAdmin = true
    };
}
```

<span data-ttu-id="2da7d-107">Há vantagens significativas neste código:</span><span class="sxs-lookup"><span data-stu-id="2da7d-107">There are significant advantages in this code:</span></span>

1. <span data-ttu-id="2da7d-108">A definição tem a versão resiliente, as propriedades podem ser facilmente adicionadas ou movidas</span><span class="sxs-lookup"><span data-stu-id="2da7d-108">The definition is version resilient, properties can easily be added or moved</span></span>
2. <span data-ttu-id="2da7d-109">As propriedades podem ser definidas em qualquer ordem, e os nomes na inicialização sempre correspondem aos acessadores</span><span class="sxs-lookup"><span data-stu-id="2da7d-109">Properties can be set in any order, and the names in the initialization always match the accessors</span></span>
3. <span data-ttu-id="2da7d-110">Propriedades com valores padrão podem simplesmente ser ignoradas</span><span class="sxs-lookup"><span data-stu-id="2da7d-110">Properties with default values can simply be skipped</span></span>

<span data-ttu-id="2da7d-111">A primeira falha é que as propriedades agora devem ser mutáveis.</span><span class="sxs-lookup"><span data-stu-id="2da7d-111">The first flaw is that the properties must now be mutable.</span></span>

# <a name="mutability"></a><span data-ttu-id="2da7d-112">Imutabilidade</span><span class="sxs-lookup"><span data-stu-id="2da7d-112">Mutability</span></span>

<span data-ttu-id="2da7d-113">O que gostaríamos é para C# fornecer uma maneira de definir um membro de `readonly` em inicializadores de objeto.</span><span class="sxs-lookup"><span data-stu-id="2da7d-113">What we'd like is for C# to provide a way to set a `readonly` member in object initializers.</span></span>
<span data-ttu-id="2da7d-114">Como alguns tipos podem não ter sido projetados com essa inicialização em mente, também gostaríamos de ser aceitos.</span><span class="sxs-lookup"><span data-stu-id="2da7d-114">Since some types may not have been designed with this initialization in mind, we'd also like it to be opt-in.</span></span>

<span data-ttu-id="2da7d-115">A solução proposta é um novo modificador, `initonly`, que pode ser aplicado a propriedades e campos:</span><span class="sxs-lookup"><span data-stu-id="2da7d-115">The proposed solution is a new modifier, `initonly`, that can be applied to properties and fields:</span></span>

```C#
public class UserInfo
{
    public initonly string Username { get; }
    public initonly string Email { get; }
    public initonly bool IsAdmin { get; } = false;
}
```

<span data-ttu-id="2da7d-116">O CodeGen para isso é surpreendentemente simples: apenas definimos o campo ReadOnly.</span><span class="sxs-lookup"><span data-stu-id="2da7d-116">The codegen for this is surprisingly straight forward: we just set the readonly field.</span></span>
<span data-ttu-id="2da7d-117">Especificamente, as propriedades reduzidas teriam a seguinte aparência:</span><span class="sxs-lookup"><span data-stu-id="2da7d-117">Specifically, the lowered properties would look like:</span></span>

```C#
public class UserInfo
{
    private readonly string <Backing>_username;
    public string get_Username() => <Backing>_username;
    [return: modreq(initonly)]
    public void set_Username(string value) { <Backing>_username = value; }
    ...
}
```

<span data-ttu-id="2da7d-118">O CLR considera a definição de campos somente leitura como não verificável, mas não é seguro.</span><span class="sxs-lookup"><span data-stu-id="2da7d-118">The CLR considers setting readonly fields to be unverifiable, but not unsafe.</span></span> <span data-ttu-id="2da7d-119">Para dar suporte a um verificador mais avançado, a seguinte regra é proposta: um campo somente leitura pode ser modificado somente dentro de `initonly` métodos ou em um novo objeto que está na pilha CLR e não foi publicado por meio de uma chamada de método ou loja.</span><span class="sxs-lookup"><span data-stu-id="2da7d-119">To support a more advanced verifier, the following rule is proposed: a readonly field can be modified only inside `initonly` methods, or on a new object that is on the CLR stack and has not been published via a store or method call.</span></span>

<span data-ttu-id="2da7d-120">Isso deve resolver muitos dos problemas com a imutabilidade no exemplo de `UserInfo`, embora não precise de estratégias de emissão complicadas ou frágeis.</span><span class="sxs-lookup"><span data-stu-id="2da7d-120">This should solve many of the problems with mutability in the `UserInfo` example, while not requiring complicated or brittle emit strategies.</span></span> <span data-ttu-id="2da7d-121">No entanto, a imutabilidade apresenta um novo problema: criar facilmente um objeto com alterações.</span><span class="sxs-lookup"><span data-stu-id="2da7d-121">However, immutability does present a new problem: easily constructing an object with changes.</span></span>

# <a name="with-ing"></a><span data-ttu-id="2da7d-122">Com-ing</span><span class="sxs-lookup"><span data-stu-id="2da7d-122">With-ing</span></span>

<span data-ttu-id="2da7d-123">Ao programar com imutabilidade, fazer alterações em um objeto é feito construindo uma cópia com alterações em vez de fazer as alterações diretamente no objeto.</span><span class="sxs-lookup"><span data-stu-id="2da7d-123">When programming with immutability, making changes to an object is done by constructing a copy with changes instead of making the changes directly on the object.</span></span> <span data-ttu-id="2da7d-124">Infelizmente, não há uma maneira conveniente de fazer isso no C#, mesmo com registros de estilo atual.</span><span class="sxs-lookup"><span data-stu-id="2da7d-124">Unfortunately, there's no convenient way to do this in C#, even with current-style records.</span></span> <span data-ttu-id="2da7d-125">Anteriormente, foi proposto que algum tipo de método "with" gerado automaticamente seja fornecido para registros que implementam essa funcionalidade.</span><span class="sxs-lookup"><span data-stu-id="2da7d-125">It's been previously proposed that some kind of autogenerated "With" method be provided for records that implements that functionality.</span></span> <span data-ttu-id="2da7d-126">Se tivermos um mecanismo desse tipo, é importante que ele funcione com membros `initonly`.</span><span class="sxs-lookup"><span data-stu-id="2da7d-126">If we have such a mechanism, it's important that it work with `initonly` members.</span></span> <span data-ttu-id="2da7d-127">Para conseguir isso, é proposto que adicionamos uma expressão `with`, análoga a um inicializador de objeto.</span><span class="sxs-lookup"><span data-stu-id="2da7d-127">To achieve this, it's proposed that we add a `with` expression, analogous to an object initializer.</span></span> <span data-ttu-id="2da7d-128">O uso de exemplo seria o seguinte:</span><span class="sxs-lookup"><span data-stu-id="2da7d-128">Sample usage would be as follows:</span></span>

```C#
var userInfo = new UserInfo() 
{
    Username = "andy",
    Email = "angocke@microsoft.com",
    IsAdmin = true
};
var newUserName = userInfo with { Username = "angocke" };
```

<span data-ttu-id="2da7d-129">O objeto `newUserName` resultante seria uma cópia de `userInfo`, com `Username` definido como "angocke".</span><span class="sxs-lookup"><span data-stu-id="2da7d-129">The resulting `newUserName` object would be a copy of `userInfo`, with `Username` set to "angocke".</span></span>
<span data-ttu-id="2da7d-130">O CodeGen na expressão de `with` também seria semelhante ao inicializador de objeto: um novo objeto é construído e, em seguida, o `Username` setter de `initonly` seria chamado no corpo do método.</span><span class="sxs-lookup"><span data-stu-id="2da7d-130">The codegen on the `with` expression would also be similar to the object initializer: a new object is constructed, and then the `initonly` `Username` setter would be called in the method body.</span></span>

<span data-ttu-id="2da7d-131">É claro que a diferença aqui é que o novo objeto que está sendo construído não é uma nova criação de objeto simples, é uma duplicata do objeto original.</span><span class="sxs-lookup"><span data-stu-id="2da7d-131">Of course, the difference here is that the new object being constructed is not a simple new object creation, it is a duplicate of the original object.</span></span> <span data-ttu-id="2da7d-132">Para fornecer essa funcionalidade, exigimos que o objeto forneça um "com Construtor" que forneça um objeto duplicado.</span><span class="sxs-lookup"><span data-stu-id="2da7d-132">To provide this functionality, we require that the object provide a "With constructor" that provides a duplicate object.</span></span> <span data-ttu-id="2da7d-133">Um construtor de `With` de exemplo teria A seguinte aparência:</span><span class="sxs-lookup"><span data-stu-id="2da7d-133">A sample `With` constructor would look like:</span></span>

```C#
class UserInfo
{
    ...
    [WithConstructor] // placeholder syntax, up for debate
    public UserInfo With()
    {
        return new UserInfo() { Username = this.Username, Email = this.Email, IsAdmin = this.IsAdmin };
    }
}
```

<span data-ttu-id="2da7d-134">Notavelmente, a expressão `with` definirá Membros `initonly`, assim como o inicializador de objeto, portanto, para dar suporte à verificação, devemos garantir que o objeto não possa ter sido publicado antes que os membros `initonly` sejam definidos.</span><span class="sxs-lookup"><span data-stu-id="2da7d-134">Notably, the `with` expression will set `initonly` members, just like the object initializer, so to support verification we must ensure that the object cannot have been published before the `initonly` members are set.</span></span> <span data-ttu-id="2da7d-135">Para impor isso, o atributo `WithConstructor` (ou sintaxe equivalente) irá impor uma nova regra para o método: todas as instruções de retorno devem conter diretamente uma expressão de criação de objeto, possivelmente com um inicializador de objeto.</span><span class="sxs-lookup"><span data-stu-id="2da7d-135">To enforce this, the `WithConstructor` attribute (or equivalent syntax) will enforce a new rule for the method: all return statements must directly contain an object creation expression, possibly with an object initializer.</span></span>

<span data-ttu-id="2da7d-136">Se o construtor de `With` exigir validação, o usuário poderá introduzir um construtor para fazer essa validação, por exemplo,</span><span class="sxs-lookup"><span data-stu-id="2da7d-136">If the `With` constructor requires validation, the user may introduce a constructor to do that validation, e.g.</span></span>

```C#
class UserInfo
{
    ...
    private UserInfo(UserInfo original)
    {
        // validation code
    }
    [WithConstructor]
    public UserInfo With() => new UserInfo(this);
}
```

<span data-ttu-id="2da7d-137">A última parte da complexidade associada ao `With` é a herança.</span><span class="sxs-lookup"><span data-stu-id="2da7d-137">The last piece of complexity associated with `With` is inheritance.</span></span> <span data-ttu-id="2da7d-138">Se o registro for extensível, será necessário fornecer um novo `With` para a subclasse.</span><span class="sxs-lookup"><span data-stu-id="2da7d-138">If your record is extensible, you will need to provide a new `With` for the subclass.</span></span> <span data-ttu-id="2da7d-139">Isso pode ser feito da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="2da7d-139">This can be achieved as follows:</span></span>

```C#
class Base
{
    ...
    protected Base(Base original)
    {
        // validation
    }
    [WithConstructor]
    public virtual Base With() => new Base(this);
}
class Derived : Base
{
    ...
    protected Derived(Derived original)
    : base(original)
    {
        // validation
    }
    [WithConstructor]
    public override Derived With() => new Derived(this);
}
```

<span data-ttu-id="2da7d-140">Observe uma parte adicional da complexidade aqui: para substituir o construtor de `With` pelo tipo derivado, o idioma também precisará oferecer suporte a tipos de retorno covariantes em substituições.</span><span class="sxs-lookup"><span data-stu-id="2da7d-140">Note one additional piece of complexity here: in order to override the `With` constructor with the derived type the language will also need to support covariant return types in overrides.</span></span>
<span data-ttu-id="2da7d-141">Já existe uma proposta separada para esse recurso [aqui](https://github.com/dotnet/csharplang/blob/725763343ad44a9251b03814e6897d87fe553769/proposals/covariant-returns.md).</span><span class="sxs-lookup"><span data-stu-id="2da7d-141">There is already a separate proposal for this feature [here](https://github.com/dotnet/csharplang/blob/725763343ad44a9251b03814e6897d87fe553769/proposals/covariant-returns.md).</span></span>

<span data-ttu-id="2da7d-142">**Desvantagens**</span><span class="sxs-lookup"><span data-stu-id="2da7d-142">**Drawbacks**</span></span>

- <span data-ttu-id="2da7d-143">Fazer todas as instruções de retorno em `WithConstructor`s contém novas expressões de objeto é restritivo.</span><span class="sxs-lookup"><span data-stu-id="2da7d-143">Making all return statements in `WithConstructor`s contain new object expressions is restrictive.</span></span>
  <span data-ttu-id="2da7d-144">Isso pode ser possivelmente mitigado pela análise de fluxo que garante que o novo objeto não escape o método</span><span class="sxs-lookup"><span data-stu-id="2da7d-144">This could be possibly be mitigated by flow analysis that ensures the new object doesn't escape the method</span></span>
- <span data-ttu-id="2da7d-145">O suporte à variação em substituições por meio de truques do compilador exigirá métodos stub, o que aumentará de forma quadrática com a profundidade da herança.</span><span class="sxs-lookup"><span data-stu-id="2da7d-145">Supporting variance in overrides through compiler tricks will require stub methods, which will grow quadratically with the inheritance depth.</span></span> <span data-ttu-id="2da7d-146">A necessidade de um método stub é devido a um requisito de tempo de execução que substituisse as assinaturas sejam exatamente iguais.</span><span class="sxs-lookup"><span data-stu-id="2da7d-146">The need for a stub method is due to a runtime requirement that override signatures match exactly.</span></span> <span data-ttu-id="2da7d-147">Se o requisito de tempo de execução fosse diminuído, os métodos de stub não seriam necessários.</span><span class="sxs-lookup"><span data-stu-id="2da7d-147">If the runtime requirement were loosened, the stub methods would not be required at all.</span></span>
- <span data-ttu-id="2da7d-148">O uso de construtores encadeados do formulário `Type(Type original)` reserva efetivamente esse construtor para o uso do padrão.</span><span class="sxs-lookup"><span data-stu-id="2da7d-148">Using chained constructors of the form `Type(Type original)` effectively reserves that constructor for the use of the pattern.</span></span> <span data-ttu-id="2da7d-149">Como os construtores têm semântica exclusiva e não podem ser renomeados, isso pode ser limitado.</span><span class="sxs-lookup"><span data-stu-id="2da7d-149">Since constructors have unique semantics and cannot be re-named this could be limiting.</span></span>


## <a name="wrapping-it-all-up-records"></a><span data-ttu-id="2da7d-150">Encapsulando tudo: registros</span><span class="sxs-lookup"><span data-stu-id="2da7d-150">Wrapping it all up: Records</span></span>

<span data-ttu-id="2da7d-151">Os recursos acima permitem um estilo de programação que era muito difícil antes.</span><span class="sxs-lookup"><span data-stu-id="2da7d-151">The above features enable a style of programming that was very difficult before.</span></span> <span data-ttu-id="2da7d-152">Mas, mesmo com os novos recursos, pode ser bastante detalhado e propenso a erros para anotar tudo por conta própria.</span><span class="sxs-lookup"><span data-stu-id="2da7d-152">But even with the new features it could be quite verbose and error prone to annotate everything yourself.</span></span> <span data-ttu-id="2da7d-153">Há também alguns itens, como Equals e GetHashCode, que já podem ser escritos hoje, é apenas uma questão de trabalhos.</span><span class="sxs-lookup"><span data-stu-id="2da7d-153">There are also a few items, like Equals and GetHashCode, which can already be written today, it's just laborious.</span></span>
<span data-ttu-id="2da7d-154">Além disso, uma falha significativa na implementação de igualdade sobre esses novos primitivos é que a igualdade estrutural é algo que deve ser alterado com o tipo de dados à medida que novos dados são adicionados, mas ao tratá-lo manualmente, é provável que essas coisas possam ficar fora de sincronia.</span><span class="sxs-lookup"><span data-stu-id="2da7d-154">Moreover, a significant flaw in implementing equality on top of these new primitives is that structural equality is something that should change with your data type as new data is added, but when handling it manually it is likely that these things can get out of sync.</span></span>

<span data-ttu-id="2da7d-155">Portanto, é proposto que C# ofereça suporte à nova sintaxe para registros, não para fornecer novos recursos, mas para definir padrões e gerar código projetado para uso em registros.</span><span class="sxs-lookup"><span data-stu-id="2da7d-155">Therefore, it is proposed that C# support new syntax for records, not for providing new features, but for setting defaults and generating code designed for use in records.</span></span> <span data-ttu-id="2da7d-156">A sintaxe de exemplo ficaria parecida com</span><span class="sxs-lookup"><span data-stu-id="2da7d-156">Example syntax would look like</span></span>

```C#
data class UserInfo
{
    public string Username { get; }
    public string Email { get; }
    public bool IsAdmin { get; } = false;
}
```

<span data-ttu-id="2da7d-157">O código gerado para essa classe considera todos os campos públicos e as propriedades automáticas como membros estruturais do registro.</span><span class="sxs-lookup"><span data-stu-id="2da7d-157">The generated code for this class would regard all public fields and auto-properties as structural members of the record.</span></span> <span data-ttu-id="2da7d-158">Os membros do registro podem ser personalizados usando um novo atributo `RecordMember(bool)` que poderia ser usado para incluir ou excluir Membros.</span><span class="sxs-lookup"><span data-stu-id="2da7d-158">Record members could be customized using a new `RecordMember(bool)` attribute that could be used to either include or exclude members.</span></span> <span data-ttu-id="2da7d-159">Os membros do registro seriam `initonly` por padrão e a igualdade seria gerada automaticamente para a classe com base nos membros do registro.</span><span class="sxs-lookup"><span data-stu-id="2da7d-159">Record members would be `initonly` by default and equality would be autogenerated for the class based on the record members.</span></span> <span data-ttu-id="2da7d-160">A qualquer momento, o comportamento desses membros poderia ser personalizado simplesmente declarando-os na origem.</span><span class="sxs-lookup"><span data-stu-id="2da7d-160">At any point the behavior of these members could be customized simply by declaring them in source.</span></span> <span data-ttu-id="2da7d-161">A implementação escrita pelo usuário substituiria a implementação padrão em todos os padrões de uso.</span><span class="sxs-lookup"><span data-stu-id="2da7d-161">The user-written implementation would replace the default implementation in all pattern usage.</span></span>

<span data-ttu-id="2da7d-162">Observe que a igualdade na aparência da herança é complexa, mas parece ter sido resolvida adequadamente na proposta de [outros registros](records.md).</span><span class="sxs-lookup"><span data-stu-id="2da7d-162">Note that equality in the face of inheritance is complex, but seems to have been adequately solved in the [other records proposal](records.md).</span></span>

## <a name="primary-constructors"></a><span data-ttu-id="2da7d-163">Construtores primários</span><span class="sxs-lookup"><span data-stu-id="2da7d-163">Primary constructors</span></span>

<span data-ttu-id="2da7d-164">A proposta de registro anterior também incluiu uma nova sintaxe para uma lista de parâmetros no próprio tipo, por exemplo,</span><span class="sxs-lookup"><span data-stu-id="2da7d-164">Previous record proposal have also included a new syntax for a parameter list on the type itself, e.g.</span></span>

```C#
class Point(int X, int Y);
```

<span data-ttu-id="2da7d-165">No novo design, a lista de parâmetros seria um recurso ortogonal C# , que poderia ser perfeitamente integrado com registros.</span><span class="sxs-lookup"><span data-stu-id="2da7d-165">In the new design, the parameter list would be an orthogonal C# feature, which could be cleanly integrated with records.</span></span> <span data-ttu-id="2da7d-166">Se um construtor principal for incluído em um registro, ele teria novos padrões, assim como campos públicos e propriedades automáticas: os parâmetros no construtor primário seriam usados para gerar Propriedades de membro de registro público com o mesmo nome.</span><span class="sxs-lookup"><span data-stu-id="2da7d-166">If a primary constructor is included in a record, it would have new defaults, just like public fields and auto-properties: the parameters in the primary constructor would be used to generate public record-member properties with the same name.</span></span> <span data-ttu-id="2da7d-167">Além disso, o construtor primário agora poderia ser usado para gerar um desconstrutor automaticamente.</span><span class="sxs-lookup"><span data-stu-id="2da7d-167">In addition, the primary constructor could now be used to auto-generate a deconstructor.</span></span>

<span data-ttu-id="2da7d-168">Por exemplo, o seguinte registro com um construtor primário</span><span class="sxs-lookup"><span data-stu-id="2da7d-168">For example, the following record with a primary constructor</span></span>

```C#
data class Point(int X, int Y);
```

<span data-ttu-id="2da7d-169">seria equivalente a</span><span class="sxs-lookup"><span data-stu-id="2da7d-169">would be equivalent to</span></span>

```C#
data class Point
{
    public int X { get; }
    public int Y { get; }

    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }

    public void Deconstruct(out int X, out int Y)
    {
        X = this.X;
        Y = this.Y;
    }
}
```

<span data-ttu-id="2da7d-170">e a geração final do acima seria</span><span class="sxs-lookup"><span data-stu-id="2da7d-170">and the final generation of the above would be</span></span>

```C#
class Point
{
    public initonly int X { get; }
    public initonly int Y { get; }

    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }

    protected Point(Point other)
    : this(other.X, other.Y)
    { }

    [WithConstructor]
    public virtual Point With() => new Point(this);

    public void Deconstruct(out int X, out int Y)
    {
        X = this.X;
        Y = this.Y;
    }

    // Generated equality
}
```

<span data-ttu-id="2da7d-171">Observe que fizemos uma outra informação em consideração para uma classe de dados com um construtor principal: em vez de definir os campos primários dentro do construtor protegido gerado, delegamos para o Construtor principal.</span><span class="sxs-lookup"><span data-stu-id="2da7d-171">Note that we've taken one other piece of information into account for a data class with a primary constructor: instead of setting the primary fields inside the generated protected constructor, we delegate to the primary constructor.</span></span> <span data-ttu-id="2da7d-172">Se a classe Point tivesse outro membro de registro não primário, por exemplo,</span><span class="sxs-lookup"><span data-stu-id="2da7d-172">If the Point class had another non-primary record member, e.g.</span></span>

```C#
data class Point(int X, int Y)
{
    public int Z { get; }
}
```

<span data-ttu-id="2da7d-173">em seguida, isso alteraria o construtor protegido gerado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="2da7d-173">then that would change the generated protected constructor as follows:</span></span>

```C#
class Point
{
    // ...
    protected Point(Point other)
    : this(other.X, other.Y)
    {
        Z = other.Z;
    }
    // ...
}
```

<span data-ttu-id="2da7d-174">Notavelmente, isso não responde ao que fazer sobre a herança de registros com construtores primários.</span><span class="sxs-lookup"><span data-stu-id="2da7d-174">Notably, this doesn't answer what to do about inheritance of records with primary constructors.</span></span> <span data-ttu-id="2da7d-175">Por exemplo,</span><span class="sxs-lookup"><span data-stu-id="2da7d-175">For instance,</span></span>

```C#
data class A(int X, int Y);
data class B(int X, int Y, int Z) : A;
```

<span data-ttu-id="2da7d-176">Em vez de resolver de maneira arbitrária, uma abordagem mais explícita pode exigir que uma lista de parâmetros seja fornecida com a lista de base, por exemplo,</span><span class="sxs-lookup"><span data-stu-id="2da7d-176">Rather than resolving in an arbitrary manner, a more explicit approach could require that a parameter list be provided with the base list, e.g.</span></span>

```C#
data class A(int X, int Y);
data class B(int X, int Y, int Z) : A(X, Y);
```

<span data-ttu-id="2da7d-177">A lista de parâmetros na lista de base seria aplicada a uma chamada de `base` no construtor primário gerado:</span><span class="sxs-lookup"><span data-stu-id="2da7d-177">The parameter list in the base list would then be applied to a `base` call in the generated primary constructor:</span></span>

```C#
class B
{
    // ..
    public B(int x, int y, int z)
    : base(x, y)
    // ..
}
```

<span data-ttu-id="2da7d-178">Quanto ao que um construtor principal poderia significar fora de um registro, que ainda está aberto para uma proposta adicional.</span><span class="sxs-lookup"><span data-stu-id="2da7d-178">As for what a primary constructor could mean outside of a record, that is still open to further proposal.</span></span>
