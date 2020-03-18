---
ms.openlocfilehash: 39fb0aab5e8bb0d422f25fd2e92ab3d8256d3f59
ms.sourcegitcommit: b8f1103eb686c5d82e294837c9386d9b667da292
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2019
ms.locfileid: "79485108"
---
# <a name="readonly-references"></a>Referências somente leitura

* [x] proposta
* [x] protótipo
* [x] implementação: iniciada
* [] Especificação: não iniciada

## <a name="summary"></a>Resumo
[summary]: #summary

O recurso "referências somente leitura" é, na verdade, um grupo de recursos que aproveitam a eficiência da passagem de variáveis por referência, mas sem expor os dados às modificações:  
- `in` parâmetros
- Retornos de `ref readonly`
- `readonly` structs
- `ref`/métodos de extensão `in`
- locais `ref readonly`
- `ref` expressões condicionais

## <a name="passing-arguments-as-readonly-references"></a>Passando argumentos como referências somente leitura.

Há uma proposta existente que toca neste tópico https://github.com/dotnet/roslyn/issues/115 como um caso especial de parâmetros ReadOnly sem entrar em muitos detalhes.
Aqui, quero apenas reconhecer que a ideia propriamente dita não é muito nova.

### <a name="motivation"></a>Motivação

Antes desse recurso C# não havia uma maneira eficiente de expressar um desejo de passar variáveis struct em chamadas de método para fins somente leitura sem intenção de modificar. O argumento regular por valor passando implica em copiar, o que adiciona custos desnecessários.  Isso orienta os usuários a usar o argumento-ref passando e a contar com comentários/documentação para indicar que os dados não devem ser modificados pelo receptor. Não é uma boa solução por muitos motivos.  
Os exemplos são muitos operadores matemáticos/matrizes matemáticas em bibliotecas gráficas, como o [XNA](https://msdn.microsoft.com/library/bb194944.aspx) é conhecido por ter operandos de referência puramente devido a considerações de desempenho. Há código no próprio compilador Roslyn que usa structs para evitar alocações e as passa por referência para evitar a cópia de custos.

### <a name="solution-in-parameters"></a>Solução (parâmetros de`in`)

Da mesma forma que os parâmetros de `out`, `in` parâmetros são passados como referências gerenciadas com garantias adicionais do receptor.  
Ao contrário dos parâmetros de `out` que _devem_ ser atribuídos pelo receptor antes de qualquer outro uso, `in` parâmetros não podem ser atribuídos pelo receptor.

Como resultado `in` parâmetros permitem a eficácia da passagem de argumentos indiretos sem expor argumentos a mutações pelo receptor.

### <a name="declaring-in-parameters"></a>Declarando parâmetros `in`

parâmetros de `in` são declarados usando a palavra-chave `in` como um modificador na assinatura de parâmetro.

Para todas as finalidades, o parâmetro `in` é tratado como uma variável `readonly`. A maioria das restrições sobre o uso de `in` parâmetros dentro do método são as mesmas que com `readonly` campos.

> Na verdade, um parâmetro `in` pode representar um campo de `readonly`. A similaridade de restrições não é um coincidência.

Por exemplo, os campos de um parâmetro de `in` que tem um tipo struct são todos classificados recursivamente como variáveis `readonly`.

```csharp
static Vector3 Add (in Vector3 v1, in Vector3 v2)
{
    // not OK!!
    v1 = default(Vector3);

    // not OK!!
    v1.X = 0;

    // not OK!!
    foo(ref v1.X);

    // OK
    return new Vector3(v1.X + v2.X, v1.Y + v2.Y, v1.Z + v2.Z);
}
```

- `in` parâmetros são permitidos em qualquer lugar em que parâmetros de ByVal comuns são permitidos. Isso inclui indexadores, operadores (incluindo conversões), delegados, lambdas, funções locais.

> ```csharp
>  (in int x) => x                                                     // lambda expression  
>  TValue this[in TKey index];                                         // indexer
>  public static Vector3 operator +(in Vector3 x, in Vector3 y) => ... // operator
>  ```

- `in` não é permitido em combinação com `out` ou com qualquer coisa que `out` não combina com.

- Não é permitido sobrecarregar `ref`/`out`/diferenças de `in`.

- É permitido sobrecarregar em diferenças comuns de ByVal e `in`.

- Com a finalidade de OHI (sobrecarregar, ocultar, implementar), `in` se comporta de forma semelhante a um parâmetro `out`.
Todas as mesmas regras se aplicam.
Por exemplo, o método de substituição terá que Coincidir `in` parâmetros com `in` parâmetros de um tipo conversível de identidade.

- Para a finalidade das conversões de grupo delegate/lambda/Method, `in` se comporta de forma semelhante a um parâmetro `out`.
As lambdas e os candidatos à conversão do grupo de métodos aplicáveis terão que corresponder `in` parâmetros do delegado de destino com parâmetros `in` de um tipo de conversível de identidade.

- Para fins de variância genérica, `in` parâmetros são nonvariant.

> Observação: não há avisos sobre `in` parâmetros que têm tipos de referência ou primitivos.
Pode ser inútil em geral, mas, em alguns casos, o usuário deve/deseja passar primitivos como `in`. Exemplos – substituindo um método genérico como `Method(in T param)` quando `T` foi substituído para ser `int`ou ao ter métodos como `Volatile.Read(in int location)`
>
> É concebível ter um analisador que avisa em casos de uso ineficiente de parâmetros de `in`, mas as regras para essa análise seriam muito difusas para fazer parte de uma especificação de linguagem.

### <a name="use-of-in-at-call-sites-in-arguments"></a>Uso de `in` em sites de chamada. (argumentos de`in`)

Há duas maneiras de passar argumentos para `in` parâmetros.

#### <a name="in-arguments-can-match-in-parameters"></a>`in` argumentos podem corresponder `in` parâmetros:

Um argumento com um modificador de `in` no site de chamada pode corresponder `in` parâmetros.

```csharp
int x = 1;

void M1<T>(in T x)
{
  // . . .
}

var x = M1(in x);  // in argument to a method

class D
{
    public string this[in Guid index];
}

D dictionary = . . . ;
var y = dictionary[in Guid.Empty]; // in argument to an indexer
```

- `in` argumento deve ser um LValue _legível_ (*).
Exemplo: `M1(in 42)` é inválido

> (*) A noção de [LValue/rvalue](https://en.wikipedia.org/wiki/Value_(computer_science)#lrvalue) varia entre os idiomas.  
Aqui, por LValue, quero dizer uma expressão que representa um local que pode ser referenciado diretamente.
E RValue significa uma expressão que produz um resultado temporário que não persiste por conta própria.  

- Em particular, é válido passar `readonly` campos, `in` parâmetros ou outras variáveis de `readonly` formalmente como argumentos de `in`.
Exemplo: `dictionary[in Guid.Empty]` é legal. `Guid.Empty` é um campo somente leitura estático.

- `in` argumento deve ter o tipo _Identity-conversível_ para o tipo do parâmetro.
Exemplo: `M1<object>(in Guid.Empty)` é inválido. `Guid.Empty` não é _conversível em identidade_ para `object`

A motivação para as regras acima é que `in` argumentos garantem a _alias_ da variável de argumento. O receptor sempre recebe uma referência direta para o mesmo local, conforme representado pelo argumento.

- em raras situações em que os argumentos de `in` devem ser despejados em pilha devido a `await` expressões usadas como operandos da mesma chamada, o comportamento é o mesmo que os argumentos `out` e `ref`-se a variável não puder ser despejada de maneira referencialmente transparente, um erro será relatado.

Exemplos:
1. `M1(in staticField, await SomethingAsync())` é válido.
`staticField` é um campo estático que pode ser acessado mais de uma vez sem efeitos colaterais observáveis. Portanto, a ordem dos efeitos colaterais e os requisitos de alias podem ser fornecidos.
2. `M1(in RefReturningMethod(), await SomethingAsync())` produzirá um erro.
`RefReturningMethod()` é um método `ref` retornando. Uma chamada de método pode ter efeitos colaterais observáveis, portanto, ela deve ser avaliada antes do operando de `SomethingAsync()`. No entanto, o resultado da invocação é uma referência que não pode ser preservada no ponto de suspensão `await` que torna impossível o requisito de referência direta.

> Observação: os erros de despejo de pilha são considerados limitações específicas da implementação. Portanto, eles não têm efeito sobre a resolução de sobrecarga ou a inferência de lambda.

#### <a name="ordinary-byval-arguments-can-match-in-parameters"></a>Argumentos de ByVal comuns podem corresponder a parâmetros `in`:

Argumentos regulares sem modificadores podem corresponder `in` parâmetros. Nesse caso, os argumentos têm as mesmas restrições relaxadas que os argumentos comuns de ByVal teriam.

A motivação para esse cenário é que `in` parâmetros nas APIs podem resultar em inconveniências para o usuário quando argumentos não podem ser passados como referência direta-ex: literais, resultados computados ou `await`-Ed ou argumentos que ocorrem para ter tipos mais específicos.  
Todos esses casos têm uma solução trivial de armazenar o valor do argumento em um local temporário do tipo apropriado e passar esse local como um argumento `in`.  
Para reduzir a necessidade de tal compilador de código clichê pode executar a mesma transformação, se necessário, quando `in` modificador não está presente no site de chamada.  

Além disso, em alguns casos, como invocação de operadores ou `in` métodos de extensão, não há nenhuma maneira sintática de especificar `in`. Isso sozinho requer a especificação do comportamento de argumentos de ByVal comuns quando eles correspondem `in` parâmetros.

Em particular:

- é válido passar RValues.
Uma referência a um temporário é passada nesse caso.
Exemplo:
```csharp
Print("hello");      // not an error.

void Print<T>(in T x)
{
  //. . .
}
```

- conversões implícitas são permitidas.

> Isso é, na verdade, um caso especial para passar um RValue  

Uma referência a um valor temporário em retenção é transmitida nesse caso.
Exemplo:
```csharp
Print<int>(Short.MaxValue)     // not an error.
```

- em um caso de um receptor de um método de extensão `in` (em oposição aos métodos de extensão `ref`), os RValues ou as _conversões implícitas desse argumento_ são permitidas.
Uma referência a um valor temporário em retenção é transmitida nesse caso.
Exemplo:
```csharp
public static IEnumerable<T> Concat<T>(in this (IEnumerable<T>, IEnumerable<T>) arg)  => . . .;

("aa", "bb").Concat<char>()    // not an error.
```
Mais informações sobre `ref`/métodos de extensão `in` são fornecidas mais adiante neste documento.

- o despejo do argumento devido a `await` operandos pode despejar "por valor", se necessário.
Em cenários em que não é possível fornecer uma referência direta ao argumento devido ao intervir `await` uma cópia do valor do argumento é despejada.  
Exemplo:
```csharp
M1(RefReturningMethod(), await SomethingAsync())   // not an error.
```
Como o resultado de uma invocação de efeito colateral é uma referência que não pode ser preservada em `await` suspensão, um temporário contendo o valor real será preservado (como faria em um caso de parâmetro ByVal comum).

#### <a name="omitted-optional-arguments"></a>Argumentos opcionais omitidos

É permitido que um parâmetro `in` especifique um valor padrão. Isso torna o argumento correspondente opcional.

Omitir o argumento opcional no site de chamada resulta na passagem do valor padrão por meio de um temporário.

```csharp
Print("hello");      // not an error, same as
Print("hello", c: Color.Black);

void Print(string s, in Color c = Color.Black)
{
    // . . .
}
```

### <a name="aliasing-behavior-in-general"></a>Comportamento de alias em geral

Assim como as variáveis `ref` e `out`, `in` variáveis são referências/aliases para os locais existentes.

Embora o receptor não tenha permissão para gravar neles, a leitura de um parâmetro `in` pode observar valores diferentes como um efeito colateral de outras avaliações.

Exemplo:

```C#
static Vector3 v = Vector3.UnitY;

static void Main()
{
    Test(v);
}

static void Test(in Vector3 v1)
{
    Debug.Assert(v1 == Vector3.UnitY);
    // changes v1 deterministically (no races required)
    ChangeV();
    Debug.Assert(v1 == Vector3.UnitX);
}

static void ChangeV()
{
    v = Vector3.UnitX;
}
```

### <a name="in-parameters-and-capturing-of-local-variables"></a>`in` parâmetros e a captura de variáveis locais.  
Para a finalidade da captura de lambda/Async `in` parâmetros se comportam da mesma forma que os parâmetros `out` e `ref`.

- `in` parâmetros não podem ser capturados em um fechamento
- parâmetros de `in` não são permitidos em métodos iteradores
- parâmetros de `in` não são permitidos em métodos assíncronos

### <a name="temporary-variables"></a>Variáveis temporárias.  
Alguns usos de `in` passagem de parâmetro podem exigir uso indireto de uma variável local temporária:  
- `in` argumentos são sempre passados como aliases diretos quando Call-site usa `in`. O temporário nunca é usado nesse caso.
- os argumentos de `in` não precisam ser aliases diretos quando o site de chamada não usa `in`. Quando o argumento não é um LValue, um temporário pode ser usado.
- `in` parâmetro pode ter um valor padrão. Quando o argumento correspondente é omitido no site de chamada, o valor padrão é passado por meio de um temporário.
- `in` argumentos podem ter conversões implícitas, incluindo aquelas que não preservam a identidade. Um temporário é usado nesses casos.
- receptores de chamadas struct comuns não podem ser LValue graváveis (**caso existente!** ). Um temporário é usado nesses casos.

O tempo de vida do argumento temporaries corresponde ao escopo de abrangência mais próximo do site de chamada.

O tempo de vida formal das variáveis temporárias é semanticamente significativo em cenários que envolvem a análise de escape de variáveis retornadas por referência.

### <a name="metadata-representation-of-in-parameters"></a>Representação de metadados de parâmetros de `in`.
Quando `System.Runtime.CompilerServices.IsReadOnlyAttribute` é aplicado a um parâmetro ByRef, isso significa que o parâmetro é um parâmetro `in`.

Além disso, se o método for *abstract* ou *virtual*, a assinatura desses parâmetros (e apenas esses parâmetros) deverá ter `modreq[System.Runtime.InteropServices.InAttribute]`.

**Motivação**: isso é feito para garantir que, em um caso do método que substitui/implemente os parâmetros de `in` coincidam.

Os mesmos requisitos se aplicam aos métodos `Invoke` em delegados.

**Motivação**: isso é para garantir que os compiladores existentes não possam simplesmente ignorar `readonly` ao criar ou atribuir delegados.

## <a name="returning-by-readonly-reference"></a>Retornando por referência ReadOnly.

### <a name="motivation"></a>Motivação
A motivação para esse subrecurso é aproximadamente simétrica para os motivos para os parâmetros de `in` – evitando a cópia, mas no lado de retorno. Antes desse recurso, um método ou um indexador tinha duas opções: 1) retornar por referência e ser exposto a possíveis mutações ou 2) retornar por valor, o que resulta em cópia.

### <a name="solution-ref-readonly-returns"></a>Solução (`ref readonly` retorna)  
O recurso permite que um membro retorne variáveis por referência sem expô-las a mutações.

### <a name="declaring-ref-readonly-returning-members"></a>Declarando `ref readonly` retornando Membros

Uma combinação de modificadores `ref readonly` na assinatura de retorno é usada para indicar que o membro retorna uma referência somente leitura.

Para todas as finalidades, um membro de `ref readonly` é tratado como uma variável de `readonly` semelhante a campos de `readonly` e `in` parâmetros.

Por exemplo, os campos de `ref readonly` membro que tem um tipo de struct são todos classificados recursivamente como variáveis de `readonly`. -É permitido passá-los como `in` argumentos, mas não como `ref` ou `out` argumentos.

```csharp
ref readonly Guid Method1()
{
}

Method2(in Method1()); // valid. Can pass as `in` argument.

Method3(ref Method1()); // not valid. Cannot pass as `ref` argument
```

- `ref readonly` retornos são permitidos nos mesmos locais `ref` retornos são permitidos.
Isso inclui indexadores, delegados, lambdas, funções locais.

- Não é permitido sobrecarregar `ref`/`ref readonly`/diferenças.

- É permitido sobrecarregar em diferenças comuns de retorno de ByVal e `ref readonly`.

- Com a finalidade de OHI (sobrecarregar, ocultar, implementar), `ref readonly` é semelhante, mas diferente de `ref`.
Por exemplo, o método que substitui `ref readonly` um, deve ser `ref readonly`do e ter tipo de identidade conversível.

- Para a finalidade das conversões de grupo delegate/lambda/Method, `ref readonly` é semelhante, mas diferente de `ref`.
As lambdas e os candidatos à conversão do grupo de métodos aplicáveis precisam corresponder `ref readonly` retorno do delegado de destino com `ref readonly` retorno do tipo que tem a identidade conversível.

- Para fins de variância genérica, `ref readonly` retornos são nonvariant.

> Observação: não há avisos sobre `ref readonly` retornos que têm tipos de referência ou primitivos.
Pode ser inútil em geral, mas, em alguns casos, o usuário deve/deseja passar primitivos como `in`. Exemplos – substituindo um método genérico como `ref readonly T Method()` quando `T` foi substituído para ser `int`.
>
>É concebível ter um analisador que avisa em casos de uso ineficiente de `ref readonly` Devoluções, mas as regras para essa análise seriam muito difusas para fazer parte de uma especificação de linguagem.

### <a name="returning-from-ref-readonly-members"></a>Retornando de membros de `ref readonly`
Dentro do corpo do método, a sintaxe é a mesma que com retornos de referência regulares. O `readonly` será inferido do método que o contém.

A motivação é que `return ref readonly <expression>` é desnecessária e só permite incompatibilidades na parte `readonly` que sempre resultaria em erros.
No entanto, o `ref` é necessário para a consistência com outros cenários em que algo é passado por meio de alias estrito vs. por valor.

> Ao contrário do caso com `in` parâmetros, `ref readonly` retorna nunca retornar por meio de uma cópia local. Considerar que a cópia deixaria de existir imediatamente ao retornar tal prática seria inútil e perigosa. Portanto `ref readonly` retorna são sempre referências diretas.

Exemplo:

```csharp
struct ImmutableArray<T>
{
    private readonly T[] array;

    public ref readonly T ItemRef(int i)
    {
        // returning a readonly reference to an array element
        return ref this.array[i];
    }
}

```

- Um argumento de `return ref` deve ser um LValue (**regra existente**)
- Um argumento de `return ref` deve ser "Safe para retornar" (**regra existente**)
- Em um membro `ref readonly`, um argumento de `return ref` _não precisa ser gravável_ .
Por exemplo, esse membro pode ser ref-retornar um campo ReadOnly ou um de seus parâmetros `in`.

### <a name="safe-to-return-rules"></a>Seguro para retornar regras.
A segurança normal para retornar regras para referências também será aplicada a referências ReadOnly.

Observe que um `ref readonly` pode ser obtido de um `ref` local/parâmetro/retorno regular, mas não o contrário. Caso contrário, a segurança de retornos de `ref readonly` será inferida da mesma maneira que para retornos de `ref` regulares.

Considerando que os RValues podem ser passados como `in` parâmetro e retornados como `ref readonly` precisamos de mais uma regra- **rvalues não são seguros para retornar por referência**.

> Considere a situação em que um RValue é passado para um parâmetro `in` por meio de uma cópia e retornada de volta em uma forma de um `ref readonly`. No contexto do chamador, o resultado dessa invocação é uma referência a dados locais e, como tal, não é seguro retornar.
> Depois que RValues não são seguros de retornar, a regra existente `#6` já lida com esse caso.

Exemplo:
```csharp
ref readonly Vector3 Test1()
{
    // can pass an RValue as "in" (via a temp copy)
    // but the result is not safe to return
    // because the RValue argument was not safe to return by reference
    return ref Test2(default(Vector3));
}

ref readonly Vector3 Test2(in Vector3 r)
{
    // this is ok, r is returnable
    return ref r;
}
```

Regras de `safe to return` atualizadas:

1.  **refs para variáveis no heap é seguro para retornar**
2.  os **parâmetros REF/in são seguros para retornar**
parâmetros de `in` naturalmente só podem ser retornados como ReadOnly.
3.  os **parâmetros de saída são seguros para retornar** (mas devem ser definitivamente atribuídos, como já é o caso hoje)
4.  **os campos de struct da instância são seguros para retornar, contanto que o receptor seja seguro para retornar**
5.  **' this ' não é seguro para retornar de membros de struct**
6.  **uma ref, retornada de outro método, é segura para retornar se todos os refs/esgotamentos passados para esse método como parâmetros formais eram seguros para retornar.** 
*especificamente é irrelevante se o destinatário é seguro de retornar, independentemente de o destinatário ser uma struct, uma classe ou um parâmetro de tipo genérico.*
7.  Os **rvalues não são seguros para retornar por referência.** 
 *, especificamente, os rvalues são seguros como em parâmetros.*

> Observação: há regras adicionais sobre a segurança de retornos que entram em jogo quando tipos de referência e reatribuições de referência estão envolvidos.
> As regras se aplicam igualmente aos membros `ref` e `ref readonly` e, portanto, não são mencionadas aqui.

### <a name="aliasing-behavior"></a>Comportamento de alias.
`ref readonly` Membros fornecem o mesmo comportamento de alias que os membros comuns de `ref` (exceto para serem ReadOnly).
Portanto, para a finalidade de capturar em lambdas, Async, iteradores, despejo de pilha, etc... as mesmas restrições se aplicam. ,. devido à incapacidade de capturar as referências reais e devido à natureza do efeito colateral da avaliação do membro, esses cenários não são permitidos.

> É permitido e necessário fazer uma cópia quando `ref readonly` retorno é um destinatário de métodos struct regulares, que usam `this` como uma referência gravável comum. Historicamente, em todos os casos em que essas invocações são aplicadas à variável ReadOnly, é feita uma cópia local.

### <a name="metadata-representation"></a>Representação de metadados.
Quando `System.Runtime.CompilerServices.IsReadOnlyAttribute` é aplicado ao retorno de um método de retorno ByRef, isso significa que o método retorna uma referência ReadOnly.

Além disso, a assinatura de resultado de tais métodos (e apenas esses métodos) deve ter `modreq[System.Runtime.CompilerServices.IsReadOnlyAttribute]`.

**Motivação**: isso é para garantir que os compiladores existentes não possam simplesmente ignorar `readonly` ao invocar métodos com `ref readonly` retorna

## <a name="readonly-structs"></a>Structs ReadOnly
Em um recurso curto que faz `this` parâmetro de todos os membros de instância de um struct, exceto para construtores, um parâmetro `in`.

### <a name="motivation"></a>Motivação
O compilador deve assumir que qualquer chamada de método em uma instância de struct pode modificar a instância. Na verdade, uma referência gravável é passada para o método como `this` parâmetro e habilita totalmente esse comportamento. Para permitir essas invocações em variáveis de `readonly`, as invocações são aplicadas a cópias temporárias. Isso pode ser não intuitivo e, às vezes, força as pessoas a abandonar `readonly` por motivos de desempenho.  
Exemplo: https://codeblog.jonskeet.uk/2014/07/16/micro-optimization-the-surprising-inefficiency-of-readonly-fields/

Depois de adicionar suporte para `in` parâmetros e `ref readonly` retorna o problema de cópia defensiva será pior, pois as variáveis ReadOnly se tornarão mais comuns.

### <a name="solution"></a>Solução
Permitir `readonly` modificador em declarações de struct que resultaria na `this` tratada como parâmetro `in` em todos os métodos de instância de struct, exceto para construtores.

```csharp
static void Test(in Vector3 v1)
{
    // no need to make a copy of v1 since Vector3 is a readonly struct
    System.Console.WriteLine(v1.ToString());
}

readonly struct Vector3
{
    . . .

    public override string ToString()
    {
        // not OK!!  `this` is an `in` parameter
        foo(ref this.X);

        // OK
        return $"X: {X}, Y: {Y}, Z: {Z}";
    }
}
```

### <a name="restrictions-on-members-of-readonly-struct"></a>Restrições em membros de struct ReadOnly
- Os campos de instância de uma struct ReadOnly devem ser ReadOnly.  
**Motivação:** só pode ser gravada externamente, mas não por meio de membros.
- As propriedades autopropriedade de uma struct ReadOnly devem ser somente obtenção.  
**Motivação:** conseqüência de restrição em campos de instância.
- Struct ReadOnly não pode declarar eventos do tipo campo.  
**Motivação:** conseqüência de restrição em campos de instância.

### <a name="metadata-representation"></a>Representação de metadados.
Quando `System.Runtime.CompilerServices.IsReadOnlyAttribute` é aplicado a um tipo de valor, isso significa que o tipo é um `readonly struct`.

Em particular:
-  A identidade do tipo de `IsReadOnlyAttribute` não é importante. Na verdade, ele pode ser inserido pelo compilador no assembly que o contém, se necessário.

## <a name="refin-extension-methods"></a>`ref`/métodos de extensão `in`
Na verdade, há uma proposta existente (https://github.com/dotnet/roslyn/issues/165) e a PR (https://github.com/dotnet/roslyn/pull/15650)de protótipo correspondente.
Quero apenas reconhecer que essa ideia não é totalmente nova. No entanto, é relevante aqui, já que `ref readonly` remove elegantemente o problema mais contenciosos sobre tais métodos, o que fazer com os receptores de RValue.

A ideia geral é permitir que os métodos de extensão adotem o parâmetro `this` por referência, desde que o tipo seja conhecido como um tipo struct.

```csharp
public static void Extension(ref this Guid self)
{
    // do something
}
```

Os motivos para escrever esses métodos de extensão são principalmente:  
1.  Evite copiar quando o destinatário é um struct grande
2.  Permitir a mutação de métodos de extensão em structs

Os motivos pelos quais não queremos permitir isso em classes  
1.  Seria uma finalidade muito limitada.
2.  Ela iria parar muito em constante distância que uma chamada de método não pode transformar o receptor não`null` para se tornar `null` após a invocação.
> Na verdade, atualmente uma variável não`null` não pode se tornar `null`, a menos que seja _explicitamente_ atribuída ou passada por `ref` ou `out`.
> Isso ajuda muito a legibilidade ou outras formas da análise "pode ser uma nula aqui".
3.  Seria difícil reconciliar com a semântica "avaliar uma vez" de acessos condicionais nulos.
Exemplo: `obj.stringField?.RefExtension(...)`-é necessário capturar uma cópia de `stringField` para tornar a verificação nula significativa, mas as atribuições para `this` dentro de RefExtension não seriam refletidas de volta para o campo.

Uma capacidade de declarar métodos de extensão em **structs** que usam o primeiro argumento por referência era uma solicitação de longa duração. Uma das considerações de bloqueio foi "o que acontece se o destinatário não for um LValue?".

- Há um precedente de que qualquer método de extensão também poderia ser chamado como um método estático (às vezes, é a única maneira de resolver a ambiguidade). Isso ditaria que os receptores de RValue não devem ser permitidos.
- Por outro lado, há uma prática de fazer a invocação em uma cópia em situações semelhantes quando os métodos de instância de struct estão envolvidos.

O motivo pelo qual a "cópia implícita" existe é porque a maioria dos métodos struct não modifica realmente a estrutura, embora não seja capaz de indicar isso. Portanto, a solução mais prática era simplesmente fazer a invocação em uma cópia, mas essa prática é conhecida por prejudicar o desempenho e causar bugs.

Agora, com a disponibilidade de parâmetros de `in`, é possível que uma extensão sinalize a intenção. Portanto, o enigma pode ser resolvido exigindo-se que as extensões de `ref` sejam chamadas com receptores graváveis, enquanto as extensões de `in` permitem a cópia implícita, se necessário.

```csharp
// this can be called on either RValue or an LValue
public static void Reader(in this Guid self)
{
    // do something nonmutating.
    WriteLine(self == default(Guid));
}

// this can be called only on an LValue
public static void Mutator(ref this Guid self)
{
    // can mutate self
    self = new Guid();
}
```

### <a name="in-extensions-and-generics"></a>extensões de `in` e genéricos.
A finalidade dos métodos de extensão de `ref` é mutar o receptor diretamente ou invocar os membros mutantes. Portanto, `ref this T` extensões são permitidas desde que `T` seja restrito a ser uma struct.

Por outro lado `in` métodos de extensão existem especificamente para reduzir a cópia implícita. No entanto, qualquer uso de um parâmetro de `in T` precisará ser feito por meio de um membro de interface. Como todos os membros da interface são considerados mutação, qualquer uso exigiria uma cópia. -Em vez de reduzir a cópia, o efeito seria o oposto. Portanto `in this T` não é permitido quando `T` é um parâmetro de tipo genérico, independentemente das restrições.

### <a name="valid-kinds-of-extension-methods-recap"></a>Tipos válidos de métodos de extensão (recapitulação):
Agora, são permitidas as seguintes formas de declaração de `this` em um método de extensão:
1) `this T arg`-extensão de ByVal regular. (**caso existente**)
- T pode ser qualquer tipo, incluindo tipos de referência ou parâmetros de tipo.
A instância será a mesma variável após a chamada.
Permite conversões implícitas desse tipo de _conversão de argumento_ .
Pode ser chamado em RValues.

-  - `in this T self``in` extensão.
T deve ser um tipo struct real.
A instância será a mesma variável após a chamada.
Permite conversões implícitas desse tipo de _conversão de argumento_ .
Pode ser chamado em RValues (pode ser invocado em um temp, se necessário).

-  - `ref this T self``ref` extensão.
T deve ser um tipo struct ou um parâmetro de tipo genérico restrito a ser um struct.
A instância pode ser gravada pela invocação.
Permite apenas conversões de identidade.
Deve ser chamado em LValue gravável. (nunca é invocado por meio de uma Temp).

## <a name="readonly-ref-locals"></a>Locais de referência ReadOnly.

### <a name="motivation"></a>Motivação.
Uma vez que `ref readonly` Membros foram introduzidos, ele estava claro do uso de que precisam ser emparelhados com o tipo apropriado de local. A avaliação de um membro pode produzir ou observar efeitos colaterais, portanto, se o resultado precisar ser usado mais de uma vez, ele precisará ser armazenado. Os locais `ref` comuns não ajudam aqui, pois não é possível atribuir uma referência `readonly`.   

### <a name="solution"></a>Soluções.
Permitir a declaração de locais de `ref readonly`. Esse é um novo tipo de `ref` locais que não são graváveis. Como resultado `ref readonly` locais podem aceitar referências a variáveis ReadOnly sem expor essas variáveis a gravações.

### <a name="declaring-and-using-ref-readonly-locals"></a>Declarando e usando `ref readonly` locais.

A sintaxe de tais locais usa `ref readonly` modificadores no site da declaração (nessa ordem específica). Da mesma forma que os locais `ref` comuns, `ref readonly` locais devem ser inicializados como REF na declaração. Ao contrário de locais de `ref` regulares, `ref readonly` locais podem se referir a `readonly` LValues como parâmetros `in`, campos de `readonly`, métodos de `ref readonly`.

Para todas as finalidades, uma `ref readonly` local é tratada como uma variável `readonly`. A maioria das restrições sobre o uso é a mesma de `readonly` campos ou parâmetros de `in`.

Por exemplo, os campos de um parâmetro de `in` que tem um tipo struct são todos classificados recursivamente como variáveis `readonly`.   

```csharp
static readonly ref Vector3 M1() => . . .

static readonly ref Vector3 M1_Trace()
{
    // OK
    ref readonly var r1 = ref M1();

    // Not valid. Need an LValue
    ref readonly Vector3 r2 = ref default(Vector3);

    // Not valid. r1 is readonly.
    Mutate(ref r1);

    // OK.
    Print(in r1);

    // OK.
    return ref r1;
}
```

### <a name="restrictions-on-use-of-ref-readonly-locals"></a>Restrições no uso de locais de `ref readonly`
Exceto por sua natureza `readonly`, `ref readonly` locais se comportam como locais de `ref` comuns e estão sujeitos a exatamente as mesmas restrições.  
Por exemplo, as restrições relacionadas à captura em fechamentos, declarando em `async` métodos ou na análise de `safe-to-return` se aplicam igualmente a `ref readonly` locais.

## <a name="ternary-ref-expressions-aka-conditional-lvalues"></a>Expressões ternários `ref`. (também conhecido como "LValues condicionais")

### <a name="motivation"></a>Motivação
O uso de `ref` e `ref readonly` locais expôs a necessidade de uma inicialização de referência desses locais com uma ou outra variável de destino com base em uma condição.

Uma solução alternativa típica é introduzir um método como:

```csharp
ref T Choice(bool condition, ref T consequence, ref T alternative)
{
    if (condition)
    {
         return ref consequence;
    }
    else
    {
         return ref alternative;
    }
}
```

Observe que `Choice` não é uma substituição exata de um ternário, já que _todos os_ argumentos devem ser avaliados no site de chamada, o que estava levando a um comportamento e a bugs não intuitivos.

O seguinte não funcionará conforme o esperado:

```csharp
    // will crash with NRE because 'arr[0]' will be executed unconditionally
    ref var r = ref Choice(arr != null, ref arr[0], ref otherArr[0]);
```

### <a name="solution"></a>Solução
Permitir tipo especial de expressão condicional que é avaliada como uma referência a um dos argumentos LValue com base em uma condição.

### <a name="using-ref-ternary-expression"></a>Usando `ref` expressão Ternário.

A sintaxe para o `ref` tipo de uma expressão condicional é `<condition> ? ref <consequence> : ref <alternative>;`

Assim como com a expressão condicional comum somente `<consequence>` ou `<alternative>` é avaliada dependendo do resultado da expressão de condição booliana.

Diferentemente da expressão condicional comum, `ref` expressão condicional:
- requer que `<consequence>` e `<alternative>` sejam LValues.
- `ref` expressão condicional em si é um LValue e
- `ref` expressão condicional é gravável se `<consequence>` e `<alternative>` são LValue graváveis

Exemplos:  
`ref` ternário é um LValue e, como tal, pode ser passado/atribuído/retornado por referência;
```csharp
     // pass by reference
     foo(ref (arr != null ? ref arr[0]: ref otherArr[0]));

     // return by reference
     return ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

Sendo um LValue, ele também pode ser atribuído a.
```csharp
     // assign to
     (arr != null ? ref arr[0]: ref otherArr[0]) = 1;

     // error. readOnlyField is readonly and thus conditional expression is readonly
     (arr != null ? ref arr[0]: ref obj.readOnlyField) = 1;
```

Pode ser usado como um receptor de uma chamada de método e ignorar a cópia, se necessário.
```csharp
     // no copies
     (arr != null ? ref arr[0]: ref otherArr[0]).StructMethod();

     // invoked on a copy.
     // The receiver is `readonly` because readOnlyField is readonly.
     (arr != null ? ref arr[0]: ref obj.readOnlyField).StructMethod();

     // no copies. `ReadonlyStructMethod` is a method on a `readonly` struct
     // and can be invoked directly on a readonly receiver
     (arr != null ? ref arr[0]: ref obj.readOnlyField).ReadonlyStructMethod();
```

`ref` ternário também pode ser usado em um contexto regular (não ref).
```csharp
     // only an example
     // a regular ternary could work here just the same
     int x = (arr != null ? ref arr[0]: ref otherArr[0]);
```

### <a name="drawbacks"></a>Desvantagens
[drawbacks]: #drawbacks

Posso ver dois argumentos principais em relação ao suporte aprimorado para referências e referências ReadOnly:

1) Os problemas que são resolvidos aqui são muito antigos. Por que repentinamente solucioná-los agora, especialmente porque não ajudariam a usar o código existente?

Como encontramos C# e .net usados em novos domínios, alguns problemas se tornam mais proeminentes.  
Como exemplos de ambientes que são mais críticos do que a média de sobrecargas de computação, posso listar

* cenários de nuvem/datacenter em que a computação é cobrada e a capacidade de resposta é uma vantagem competitiva.
* Jogos/VR/AR com requisitos de tempo real em latências     

Esse recurso não sacrifica nenhum dos pontos fortes existentes, como a segurança de tipo, ao mesmo tempo que permite reduzir sobrecargas em alguns cenários comuns.

2) Podemos, razoavelmente, garantir que o receptor seja tocado pelas regras quando ele se deparar com contratos de `readonly`?

Temos confiança semelhante ao usar `out`. A implementação incorreta de `out` pode causar comportamento não especificado, mas, na realidade, raramente acontece.  

Fazer as regras de verificação formal familiarizadas com o `ref readonly` atenuaria ainda mais o problema de confiança.

### <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

O design principal da concorrência é realmente "não fazer nada".

### <a name="unresolved-questions"></a>Perguntas não resolvidas
[unresolved]: #unresolved-questions

### <a name="design-meetings"></a>Criar reuniões

https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-02-22.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-01.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-08-28.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-25.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-27.md
