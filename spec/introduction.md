---
ms.openlocfilehash: db10046af5d635b430951679a448e23680b18b87
ms.sourcegitcommit: 4cc6d73a765ac9827ab00c48ad9f09204baf888f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59426806"
---
# <a name="introduction"></a>Introdução

O C# (pronuncia-se "C Sharp") é uma linguagem de programação simples, moderna, orientada a objeto e fortemente tipada. C# tem suas raízes na família de idiomas do C e será imediatamente familiar aos programadores de C, C++ e Java. C# é padronizado pelo ECMA International como o ***ECMA-334*** padrão e pela ISO/IEC como o ***ISO/IEC 23270*** padrão. Compilador c# da Microsoft para o .NET Framework é uma implementação em conformidade de ambos os padrões.

O C# é uma linguagem orientada a objeto, mas inclui ainda suporte para programação ***orientada a componentes***. O design de software atual depende cada vez mais dos componentes de software na forma de pacotes independentes e autodescritivos de funcionalidade. O principal é que esses componentes apresentam um modelo de programação com propriedades, métodos e eventos; eles têm atributos que fornecem informações declarativas sobre o componente; e incorporam sua própria documentação. O c# fornece construções de linguagem para dar suporte diretamente a esses conceitos, tornando c# uma linguagem muito natural para criar e usar componentes de software.

Vários recursos de C# auxiliam na construção de aplicativos robustos e duráveis: ***Coleta de lixo*** recupera automaticamente a memória ocupada por objetos não utilizados; ***tratamento de exceção*** fornece uma abordagem estruturada e extensível para detecção de erros e recuperação; e o ***fortemente tipado*** design da linguagem impossibilita a leitura de variáveis não inicializadas, para indexar matrizes além dos seus limites ou realizar desmarcada conversões de tipos.

C# tem um ***sistema de tipo unificado***. Todos os tipos do C#, incluindo tipos primitivos, como `int` e `double`, herdam de um único tipo de `object` raiz. Assim, todos os tipos compartilham um conjunto de operações comuns, e valores de qualquer tipo podem ser armazenados, transportados e operados de maneira consistente. Além disso, C# oferece suporte a tipos de referência e tipos de valor definidos pelo usuário, permitindo a alocação dinâmica de objetos, bem como o armazenamento em linha de estruturas leves.

Para garantir que as bibliotecas e programas em c# podem evoluir ao longo do tempo de uma maneira compatível, muita ênfase foi colocado no ***controle de versão*** no design do #. Muitas linguagens de programação prestam pouca atenção a esse problema e, como resultado, programas escritos nessas linguagens quebram com mais frequência do que o necessário quando versões mais recentes das bibliotecas dependentes são introduzidas. Aspectos do C#design que foram diretamente influenciados pelas considerações de controle de versão de inclusão separada `virtual` e `override` modificadores, as regras de resolução de sobrecarga de método e o suporte para declarações de membro de interface explícita.

O restante deste capítulo descreve os recursos essenciais da linguagem c#. Embora os capítulos descrevem regras e exceções de uma maneira orientada em detalhes e, às vezes, matemática, este capítulo se esforça para maior clareza e fins de brevidade às custas da integridade. A intenção é fornecer ao leitor uma introdução à linguagem que irá facilitar a escrita dos programas antecipadas e a leitura de capítulos.

## <a name="hello-world"></a>Hello world

O programa "Hello, World" é usado tradicionalmente para introduzir uma linguagem de programação. Este é para C#:

```csharp
using System;

class Hello
{
    static void Main() {
        Console.WriteLine("Hello, World");
    }
}
```

Os arquivos de origem em C# normalmente têm a extensão de arquivo `.cs`. Supondo que o programa "Hello, World" esteja armazenado no arquivo `hello.cs`, o programa pode ser compilado com o compilador Microsoft c# usando a linha de comando
```
csc hello.cs
```
que produz um assembly executável denominado `hello.exe`. É a saída produzida por este aplicativo quando ele é executado
```
Hello, World
```

O programa "Hello, World" começa com uma diretiva `using` que faz referência ao namespace `System`. Namespaces fornecem um meio hierárquico de organizar bibliotecas e programas em C#. Os namespaces contêm tipos e outros namespaces — por exemplo, o namespace `System` contém uma quantidade de tipos, como a classe `Console` referenciada no programa e diversos outros namespaces, como `IO` e `Collections`. A diretiva `using` que faz referência a um determinado namespace permite o uso não qualificado dos tipos que são membros desse namespace. Devido à diretiva `using`, o programa pode usar `Console.WriteLine` como um atalho para `System.Console.WriteLine`.

A classe `Hello` declarada pelo programa "Hello, World" tem um único membro, o método chamado `Main`. O `Main` método é declarado com o `static` modificador. Embora os métodos de instância possam fazer referência a uma determinada instância de objeto delimitador usando a palavra-chave `this`, métodos estáticos operam sem referência a um objeto específico. Por convenção, um método estático denominado `Main` serve como ponto de entrada de um programa.

A saída do programa é produzida pelo método `WriteLine` da classe `Console` no namespace `System`. Essa classe é fornecida pelas bibliotecas de classe do .NET Framework, que, por padrão, são referenciadas automaticamente pelo compilador Microsoft c#. Observe que c# em si não tem uma biblioteca de tempo de execução separado. Em vez disso, o .NET Framework é a biblioteca de tempo de execução da linguagem c#.

## <a name="program-structure"></a>Estrutura do programa

Os principais conceitos organizacionais em C# são ***programas***, ***namespaces***, ***tipos***, ***membros*** e ***assemblies***. Os programas C# consistem em um ou mais arquivos de origem. Os programas declaram tipos que contêm membros e podem ser organizados em namespaces. Classes e interfaces são exemplos de tipos. Campos, métodos, propriedades e eventos são exemplos de membros. Quando os programas em C# são compilados, eles são empacotados fisicamente em assemblies. Os assemblies normalmente têm a extensão de arquivo `.exe` ou `.dll`, dependendo se eles implementam ***aplicativos*** ou ***bibliotecas***.

O exemplo

```csharp
using System;

namespace Acme.Collections
{
    public class Stack
    {
        Entry top;

        public void Push(object data) {
            top = new Entry(top, data);
        }

        public object Pop() {
            if (top == null) throw new InvalidOperationException();
            object result = top.data;
            top = top.next;
            return result;
        }

        class Entry
        {
            public Entry next;
            public object data;
    
            public Entry(Entry next, object data) {
                this.next = next;
                this.data = data;
            }
        }
    }
}
```
declara uma classe chamada `Stack` em um namespace chamado `Acme.Collections`. O nome totalmente qualificado dessa classe é `Acme.Collections.Stack`. A classe contém vários membros: um campo chamado `top`, dois métodos chamados `Push` e `Pop` e uma classe aninhada chamada `Entry`. A classe `Entry` ainda contém três membros: um campo chamado `next`, um campo chamado `data`e um construtor. Supondo que o código-fonte do exemplo seja armazenado no arquivo `acme.cs`, a linha de comando

```
csc /t:library acme.cs
```
compila o exemplo como uma biblioteca (o código sem um ponto de entrada `Main`) e produz um assembly denominado `acme.dll`.

Assemblies contêm código executável na forma de ***Intermediate Language*** instruções de (IL) e informações simbólicas na forma de ***metadados***. Antes de ser executado, o código de IL em um assembly é automaticamente convertido em código específico do processador pelo compilador JIT (Just-In-Time) do .NET Common Language Runtime.

Como um assembly é uma unidade autodescritiva da funcionalidade que contém o código e os metadados, não é necessário de diretivas `#include` e arquivos de cabeçalho no C#. Os tipos públicos e os membros contidos em um assembly específico são disponibilizados em um programa C# simplesmente fazendo referência a esse assembly ao compilar o programa. Por exemplo, esse programa usa a classe `Acme.Collections.Stack` do assembly `acme.dll`:

```csharp
using System;
using Acme.Collections;

class Test
{
    static void Main() {
        Stack s = new Stack();
        s.Push(1);
        s.Push(10);
        s.Push(100);
        Console.WriteLine(s.Pop());
        Console.WriteLine(s.Pop());
        Console.WriteLine(s.Pop());
    }
}
```
Se o programa é armazenado no arquivo `test.cs`, quando `test.cs` é compilado, o `acme.dll` assembly pode ser referenciado usando o compilador `/r` opção:

```
csc /r:acme.dll test.cs
```
Isso cria um assembly executável denominado `test.exe`, que, quando executado, produz a saída:

```
100
10
1
```
O C# permite que o texto de origem de um programa seja armazenado em vários arquivos de origem. Quando um programa em C# com vários arquivo é compilado, todos os arquivos de origem são processados juntos e os arquivos de origem podem referenciar livremente uns aos outros. Conceitualmente, é como se todos os arquivos de origem fossem concatenados em um arquivo grande antes de serem processados. Declarações de encaminhamento nunca são necessárias em C#, porque, com poucas exceções, a ordem de declaração é insignificante. O C# não limita um arquivo de origem para declarar somente um tipo público nem requer o nome do arquivo de origem para corresponder a um tipo declarado no arquivo de origem.

## <a name="types-and-variables"></a>Tipos e variáveis

Há dois tipos em C#: ***tipos de referência*** e ***tipos de valor***. As variáveis de tipos de valor contêm diretamente seus dados enquanto variáveis de tipos de referência armazenam referências a seus dados, o último sendo conhecido como objetos. Com tipos de referência, é possível que duas variáveis referenciem o mesmo objeto e, portanto, é possível que operações em uma variável afetem o objeto referenciado por outra variável. Com tipos de valor, cada variável tem sua própria cópia dos dados e não é possível que operações em uma variável afetem a outra (exceto no caso de variáveis de parâmetros `ref` e `out`).

C#de tipos de valor são divididos em ***tipos simples***, ***tipos enum***, ***tipos struct***, e ***tipos anuláveis***, e C#do tipos de referência são divididos em ***tipos de classe***, ***tipos de interface***, ***tipos de matriz***, e ***tipos delegados***.

A tabela a seguir fornece uma visão geral do C#do sistema de tipos.

| __Categoria__    |                 | __Descrição__ |
|-----------------|-----------------|-----------------|
| Tipos de valor     | Tipos simples    | Integral com sinal: `sbyte`, `short`, `int`,`long` |
|                 |                 | Integral sem sinal: `byte`, `ushort`, `uint`,`ulong` |
|                 |                 | Caracteres Unicode: `char` |
|                 |                 | Ponto flutuante IEEE: `float`, `double` |
|                 |                 | Decimal de alta precisão:`decimal` |
|                 |                 | Booliano: `bool` |
|                 | Tipos enum      | Tipos definidos pelo usuário do formulário `enum E {...}` |
|                 | Tipos struct    | Tipos definidos pelo usuário do formulário `struct S {...}` |
|                 | Tipos que permitem valor nulo  | Extensões de todos os outros tipos de valor com um valor `null` |
| Tipos de referência | Tipos de classe     | Classe base definitiva de todos os outros tipos: `object` |
|                 |                 | Cadeia de caracteres Unicode: `string` |
|                 |                 | Tipos definidos pelo usuário do formulário `class C {...}` |
|                 | Tipos de interface | Tipos definidos pelo usuário do formulário `interface I {...}` |
|                 | Tipos de matriz     | Unidimensional e multidimensional, por exemplo, `int[]` e `int[,]` |
|                 | Tipos delegados  | Tipos definidos pelo usuário do formulário, por exemplo, `delegate int  D(...)` |

Os tipos integrais oito dão suporte a valores de 8 bits, 16 bits, 32 bits e 64 bits no formulário com ou sem sinal.

De tipos, os dois flutuante ponto `float` e `double`, são representados usando os formatos 32 bits de precisão simples e 64 bits de precisão dupla IEEE 754.

O tipo `decimal` é um tipo de dados de 128 bits adequado para cálculos financeiros e monetários.

C#do `bool` tipo é usado para representar valores boolianos — valores que são `true` ou `false`.

O processamento de cadeia de caracteres e caracteres em C# usa codificação Unicode. O tipo `char` representa uma unidade de código UTF-16 e o tipo `string` representa uma sequência de unidades de código UTF-16.

A tabela a seguir resume C#de tipos numéricos.


| __Categoria__      | __Bits__ | __Tipo__  | __Intervalo de precisão__ |
|-------------------|----------|-----------|---------------------|
| Integral assinado   | 8        | `sbyte`   | -128...127 |
|                   | 16       | `short`   | -32,768...32,767 |
|                   | 32       | `int`     | -2,147,483,648...2,147,483,647 |
|                   | 64       | `long`    | -9,223,372,036,854,775,808...9,223,372,036,854,775,807 |
| Integral sem sinal | 8        | `byte`    | 0...255 |
|                   | 16       | `ushort`  | 0...65,535 |
|                   | 32       | `uint`    | 0...4,294,967,295 |
|                   | 64       | `ulong`   | 0...18,446,744,073,709,551,615 |
| Ponto flutuante    | 32       | `float`   | 1,5 × 10 ^ − 45 a 3,4 × 10 ^ 38, precisão de 7 dígitos |
|                   | 64       | `double`  | 5,0 × 10 ^ −324 a 1,7 × 10 ^ 308, precisão de 15 dígitos |
| Decimal           | 128      | `decimal` | 1.0 × 10 ^ − 28 a 7,9 × 10 ^ 28, precisão de 28 dígitos |

Os programas em C# usam ***declarações de tipos*** para criar novos tipos. Uma declaração de tipo especifica o nome e os membros do novo tipo. Cinco das C#do categorias de tipos são definidos pelo usuário: classe tipos, tipos struct, tipos de interface, tipos enum e tipos de delegado.

Um tipo de classe define uma estrutura de dados que contém membros de dados (campos) e os membros da função (métodos, propriedades e outros). Os tipos de classe dão suporte à herança única e ao polimorfismo, mecanismos nos quais as classes derivadas podem estender e especializar as classes base.

Um tipo de estrutura é semelhante a um tipo de classe que representa uma estrutura com membros de dados e os membros da função. No entanto, diferentemente das classes, structs são tipos de valor e não precisam de alocação de heap. Os tipos de estrutura não dão suporte à herança especificada pelo usuário, e todos os tipos de structs são herdados implicitamente do tipo `object`.

Um tipo de interface define um contrato como um conjunto nomeado de membros da função pública. Uma classe ou struct que implementa uma interface deve fornecer implementações da interface membros da função. Uma interface pode herdar de várias interfaces base e uma classe ou struct pode implementar várias interfaces.

Um tipo de delegado representa referências aos métodos com uma lista de parâmetro específico e o tipo de retorno. Delegados possibilitam o tratamento de métodos como entidades que podem ser atribuídos a variáveis e passadas como parâmetros. Os delegados são parecidos com o conceito de ponteiros de função em outras linguagens, mas ao contrário dos ponteiros de função, os delegados são orientados a objetos e fortemente tipados.

Tipos dão suporte a genéricos, podem ser parametrizados com outros tipos de classe, struct, interface e delegado.

Um tipo de enumeração é um tipo distinto com constantes nomeadas. Cada tipo de enumeração tem um tipo subjacente, que deve ser um dos oito tipos integrais. O conjunto de valores de um tipo de enumeração é o mesmo que o conjunto de valores do tipo subjacente.

O C# dá suporte a matrizes uni e multidimensionais de qualquer tipo. Ao contrário dos tipos listados acima, os tipos de matriz não precisam ser declarados antes de serem usados. Em vez disso, os tipos de matriz são construídos seguindo um nome de tipo entre colchetes. Por exemplo, `int[]` é uma matriz unidimensional de `int`, `int[,]` é uma matriz bidimensional de `int`, e `int[][]` é uma matriz unidimensional de matrizes unidimensionais de `int`.

Tipos anuláveis também não precisa ser declarada antes que possam ser usados. Para cada tipo de valor não anulável `T` há um tipo que permite valor nulo correspondente `T?`, que pode conter um valor adicional `null`. Por exemplo, `int?` é um tipo que pode conter qualquer número inteiro de 32 bits ou o valor `null`.

C#do sistema de tipos é unificado, de modo que um valor de qualquer tipo pode ser tratado como um objeto. Cada tipo no C#, direta ou indiretamente, deriva do tipo de classe `object`, e `object` é a classe base definitiva de todos os tipos. Os valores de tipos de referência são tratados como objetos simplesmente exibindo os valores como tipo `object`. Valores de tipos de valor são tratados como objetos, executando ***conversão boxing*** e ***unboxing*** operações. No exemplo a seguir, um valor `int` é convertido em `object` e volta novamente ao `int`.

```csharp
using System;

class Test
{
    static void Main() {
        int i = 123;
        object o = i;          // Boxing
        int j = (int)o;        // Unboxing
    }
}
```
Quando um valor de um tipo de valor é convertido para o tipo `object`, uma instância do objeto, também chamada de "caixa", é alocada para armazenar o valor e o valor é copiado na caixa. Por outro lado, quando um `object` referência é convertida para um tipo de valor, é feita uma verificação que o objeto referenciado é uma caixa do tipo do valor correto e, se a verificação for bem-sucedida, o valor na caixa será copiado.

C#unificada do tipo de sistema significa que os tipos de valor podem se tornar objetos "sob"demanda. Devido à unificação, as bibliotecas de finalidade geral que usam o tipo `object` podem ser usadas com os tipos de referência e os tipos de valor.

Existem vários tipos de ***variáveis*** no C#, incluindo campos, elementos de matriz, variáveis locais e parâmetros. As variáveis representam os locais de armazenamento, e cada variável tem um tipo que determina quais valores podem ser armazenados na variável, conforme mostrado pela tabela a seguir.


| __Tipo de variável__    | __Conteúdo possível__ |
|-------------------------|-----------------------|
| Tipo de valor não nulo | Um valor de tipo exato |
| Tipos de valor anulável     | Um valor nulo ou um valor de tipo exato |
| `object`                | Uma referência nula, uma referência a um objeto de qualquer tipo de referência ou uma referência a um valor demarcado de qualquer tipo de valor |
| Tipo de classe              | Uma referência nula, uma referência a uma instância desse tipo de classe ou uma referência a uma instância de uma classe derivada de tipo de classe |
| Tipo de interface          | Uma referência nula, uma referência a uma instância de um tipo de classe que implementa esse tipo de interface ou uma referência a um valor demarcado de um tipo de valor que implementa esse tipo de interface |
| Tipo de matriz              | Uma referência nula, uma referência a uma instância desse tipo de matriz ou uma referência a uma instância de um tipo de matriz compatível |
| Tipo delegado           | Uma referência nula ou uma referência a uma instância do tipo delegado |

## <a name="expressions"></a>Expressões

***Expressões*** são construídas a partir de ***operandos*** e ***operadores***. Os operadores de uma expressão indicam quais operações devem ser aplicadas aos operandos. Exemplos de operadores incluem `+`, `-`, `*`, `/` e `new`. Exemplos de operandos incluem literais, campos, variáveis locais e expressões.

Quando uma expressão contiver vários operadores, a ***precedência*** dos operadores controla a ordem na qual os operadores individuais são avaliados. Por exemplo, a expressão `x + y * z` é avaliada como `x + (y * z)` porque o operador `*` tem precedência maior do que o operador `+`.

A maioria dos operadores pode ser ***sobrecarregada***. A sobrecarga de operador permite que implementações de operador definidas pelo usuário sejam especificadas para operações em que um ou ambos os operandos são de um tipo struct ou de classe definida pelo usuário.

A tabela a seguir resume C#do operadores, listando as categorias de operador em ordem decrescente de precedência mais alto ao mais baixo. Operadores na mesma categoria têm a mesma precedência.


| __Categoria__                     | __Expressão__    | __Descrição__ |
|----------------------------------|-------------------|-----------------|
| Primária                          | `x.m`             | Acesso de membros |
|                                  | `x(...)`          | Invocação de método e delegado |
|                                  | `x[...]`          | Acesso de matriz e indexador |
|                                  | `x++`             | Pós-incremento |
|                                  | `x--`             | Pós-decremento |
|                                  | `new T(...)`      | Criação de objeto e delegado |
|                                  | `new T(...){...}` | Criação de objeto com inicializador |
|                                  | `new {...}`       | Inicializador de objeto anônimo |
|                                  | `new T[...]`      | Criação de matriz |
|                                  | `typeof(T)`       | Obter objeto `System.Type` para `T` |
|                                  | `checked(x)`      | Avalia expressão no contexto selecionado |
|                                  | `unchecked(x)`    | Avalia expressão no contexto desmarcado |
|                                  | `default(T)`      | Obter valor padrão do tipo `T` |
|                                  | `delegate {...}`  | Função anônima (método anônimo) |
| Unário                            | `+x`              | Identidade |
|                                  | `-x`              | Negação |
|                                  | `!x`              | Negação lógica |
|                                  | `~x`              | Negação bit a bit |
|                                  | `++x`             | Pré-incremento |
|                                  | `--x`             | Pré-decremento |
|                                  | `(T)x`            | Converter explicitamente `x` no tipo `T` |
|                                  | `await x`         | Aguardar assincronamente `x` para concluir |
| Multiplicativo                   | `x * y`           | Multiplicação |
|                                  | `x / y`           | Divisão |
|                                  | `x % y`           | Restante |
| Aditivo                         | `x + y`           | Adição, concatenação de cadeia de caracteres, combinação de delegados |
|                                  | `x - y`           | Subtração, remoção de delegado |
| Shift                            | `x << y`          | Shift esquerdo |
|                                  | `x >> y`          | Shift direito |
| Teste de tipo e relacional      | `x < y`           | Menor que |
|                                  | `x > y`           | Maior que |
|                                  | `x <= y`          | Menor que ou igual a |
|                                  | `x >= y`          | Maior que ou igual a |
|                                  | `x is T`          | Retorna `true` se `x` for um `T`, caso contrário, `false` |
|                                  | `x as T`          | Retorna `x` digitado como `T` ou `null`, se `x` não for um `T` |
| Igualdade                         | `x == y`          | Igual      |
|                                  | `x != y`          | Não é igual a |
| AND lógico                      | `x & y`           | AND bit a bit inteiro, AND lógico booliano |
| XOR lógico                      | `x ^ y`           | XOR bit a bit inteiro, XOR lógico booliano |
| OR lógico                       | <code>x &#124; y</code> | OR bit a bit inteiro, OR lógico booliano |
| AND condicional                  | `x && y`          | Avalia `y` somente se `x` é `true` |
| OR condicional                   | <code>x &#124;&#124; y</code> | Avalia `y` somente se `x` é `false` |
| Coalescência nula                  | `x ?? y`          | É avaliada como `y` se `x` é `null`, para `x` caso contrário, |
| Condicional                      | `x ? y : z`       | Avalia `y` se `x` for `true`, `z` se `x` for `false` |
| Atribuição ou função anônima | `x = y`           | Atribuição |
|                                  | `x op= y`         | Atribuição composta; operadores com suporte são `*=` `/=` `%=` `+=` `-=` `<<=` `>>=` `&=` `^=` <code>&#124;=</code> |
|                                  | `(T x) => y`      | Função anônima (expressão lambda) |

## <a name="statements"></a>Instruções

As ações de um programa são expressas usando ***instruções***. O C# oferece suporte a vários tipos diferentes de instruções, algumas delas definidas em termos de instruções inseridas.

Um ***bloco*** permite a produção de várias instruções em contextos nos quais uma única instrução é permitida. Um bloco é composto por uma lista de instruções escritas entre os delimitadores `{` e `}`.

***Instruções de declaração*** são usadas para declarar constantes e variáveis locais.

***Instruções de expressão*** são usadas para avaliar expressões. Expressões que podem ser usadas como instruções incluem chamadas de método, alocações de objeto usando o `new` operador, atribuições usando `=` e os operadores de atribuição composta, operações de incremento e decremento usando o `++`e `--` operadores e expressões await.

***Instruções de seleção*** são usadas para selecionar uma dentre várias instruções possíveis para execução com base no valor de alguma expressão. Neste grupo estão as instruções `if` e `switch`.

***Instruções de iteração*** são usados para executar repetidamente uma instrução inserida. Neste grupo estão as instruções `while`, `do`, `for` e `foreach`.

***Instruções de salto*** são usadas para transferir o controle. Neste grupo estão as instruções `break`, `continue`, `goto`, `throw`, `return` e `yield`.

A instrução `try`... `catch` é usada para capturar exceções que ocorrem durante a execução de um bloco, e a instrução `try`... `finally` é usada para especificar o código de finalização que é executado sempre, se uma exceção ocorrer ou não.

O `checked` e `unchecked` instruções são usadas para controlar o contexto para operações aritméticas de tipo integral e conversões de verificação de estouro.

A instrução `lock` é usada para obter o bloqueio de exclusão mútua para um determinado objeto, executar uma instrução e, em seguida, liberar o bloqueio.

A instrução `using` é usada para obter um recurso, executar uma instrução e, em seguida, descartar esse recurso.

A seguir estão exemplos de cada tipo de instrução

__Declarações de variável local__

```csharp
static void Main() {
   int a;
   int b = 2, c = 3;
   a = 1;
   Console.WriteLine(a + b + c);
}
```


__Declaração de constante local__

```csharp
static void Main() {
    const float pi = 3.1415927f;
    const int r = 25;
    Console.WriteLine(pi * r * r);
}
```


__Instrução de expressão__

```csharp
static void Main() {
    int i;
    i = 123;                // Expression statement
    Console.WriteLine(i);   // Expression statement
    i++;                    // Expression statement
    Console.WriteLine(i);   // Expression statement
}
```

__`if` statement__

```csharp
static void Main(string[] args) {
    if (args.Length == 0) {
        Console.WriteLine("No arguments");
    }
    else {
        Console.WriteLine("One or more arguments");
    }
}
```


__`switch` statement__

```csharp
static void Main(string[] args) {
    int n = args.Length;
    switch (n) {
        case 0:
            Console.WriteLine("No arguments");
            break;
        case 1:
            Console.WriteLine("One argument");
            break;
        default:
            Console.WriteLine("{0} arguments", n);
            break;
    }
}
```

__`while` statement__

```csharp
static void Main(string[] args) {
    int i = 0;
    while (i < args.Length) {
        Console.WriteLine(args[i]);
        i++;
    }
}
```


__`do` statement__

```csharp
static void Main() {
    string s;
    do {
        s = Console.ReadLine();
        if (s != null) Console.WriteLine(s);
    } while (s != null);
}
```

__`for` statement__

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        Console.WriteLine(args[i]);
    }
}
```

__`foreach` statement__

```csharp
static void Main(string[] args) {
    foreach (string s in args) {
        Console.WriteLine(s);
    }
}
```

__`break` statement__

```csharp
static void Main() {
    while (true) {
        string s = Console.ReadLine();
        if (s == null) break;
        Console.WriteLine(s);
    }
}
```

__`continue` statement__

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        if (args[i].StartsWith("/")) continue;
        Console.WriteLine(args[i]);
    }
}
```

__`goto` statement__

```csharp
static void Main(string[] args) {
    int i = 0;
    goto check;
    loop:
    Console.WriteLine(args[i++]);
    check:
    if (i < args.Length) goto loop;
}
```

__`return` statement__

```csharp
static int Add(int a, int b) {
    return a + b;
}

static void Main() {
    Console.WriteLine(Add(1, 2));
    return;
}
```

__`yield` statement__

```csharp
static IEnumerable<int> Range(int from, int to) {
    for (int i = from; i < to; i++) {
        yield return i;
    }
    yield break;
}

static void Main() {
    foreach (int x in Range(-10,10)) {
        Console.WriteLine(x);
    }
}
```

__`throw` e `try` instruções__

```csharp
static double Divide(double x, double y) {
    if (y == 0) throw new DivideByZeroException();
    return x / y;
}

static void Main(string[] args) {
    try {
        if (args.Length != 2) {
            throw new Exception("Two numbers required");
        }
        double x = double.Parse(args[0]);
        double y = double.Parse(args[1]);
        Console.WriteLine(Divide(x, y));
    }
    catch (Exception e) {
        Console.WriteLine(e.Message);
    }
    finally {
        Console.WriteLine("Good bye!");
    }
}
```

__`checked` e `unchecked` instruções__

```csharp
static void Main() {
    int i = int.MaxValue;
    checked {
        Console.WriteLine(i + 1);        // Exception
    }
    unchecked {
        Console.WriteLine(i + 1);        // Overflow
    }
}
```

__`lock` statement__

```csharp
class Account
{
    decimal balance;
    public void Withdraw(decimal amount) {
        lock (this) {
            if (amount > balance) {
                throw new Exception("Insufficient funds");
            }
            balance -= amount;
        }
    }
}
```

__`using` statement__

```csharp
static void Main() {
    using (TextWriter w = File.CreateText("test.txt")) {
        w.WriteLine("Line one");
        w.WriteLine("Line two");
        w.WriteLine("Line three");
    }
}
```

## <a name="classes-and-objects"></a>Classes e objetos

As ***classes*** são os tipos do C# mais fundamentais. Uma classe é uma estrutura de dados que combina ações (métodos e outros membros da função) e estado (campos) em uma única unidade. Uma classe fornece uma definição para ***instâncias*** da classe criadas dinamicamente, também conhecidas como ***objetos***. As classes dão suporte à ***herança*** e ***polimorfismo***, mecanismos nos quais ***classes derivadas*** podem estender e especializar ***classes base***.

Novas classes são criadas usando declarações de classe. Uma declaração de classe inicia com um cabeçalho que especifica os atributos e modificadores de classe, o nome da classe, a classe base (se fornecida) e as interfaces implementadas pela classe. O cabeçalho é seguido pelo corpo da classe, que consiste em uma lista de declarações de membro escrita entre os delimitadores `{` e `}`.

A seguir está uma declaração de uma classe simples chamada `Point`:

```csharp
public class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
Instâncias de classes são criadas usando o operador `new`, que aloca memória para uma nova instância, chama um construtor para inicializar a instância e retorna uma referência à instância. As instruções a seguir criam dois `Point` objetos e armazenam referências a esses objetos em duas variáveis:

```
Point p1 = new Point(0, 0);
Point p2 = new Point(10, 20);
```
A memória ocupada por um objeto é recuperada automaticamente quando o objeto não está mais em uso. Não é necessário nem possível desalocar explicitamente os objetos em C#.

### <a name="members"></a>Membros

Os membros de uma classe são ***membros estáticos*** ou ***membros de instância***. Os membros estáticos pertencem às classes e os membros de instância pertencem aos objetos (instâncias de classes).

A tabela a seguir fornece uma visão geral dos tipos de membros de que uma classe pode conter.


| __Membro__   | __Descrição__ |
|------------  |-----------------|
| Constantes    | Valores constantes associados à classe |
| Campos       | Variáveis de classe |
| Métodos      | Os cálculos e as ações que podem ser executados pela classe |
| Propriedades   | Ações associadas à leitura e à gravação de propriedades nomeadas da classe |
| Indexadores     | Ações associadas a instâncias de indexação da classe como uma matriz |
| Eventos       | Notificações que podem ser geradas pela classe |
| Operadores    | Operadores de conversões e expressão com suporte da classe |
| Construtores | Ações necessárias para inicializar instâncias da classe ou a própria classe |
| Destruidores  | Ações a serem executadas antes de as instâncias da classe serem descartadas permanentemente |
| Tipos        | Tipos aninhados declarados pela classe |

### <a name="accessibility"></a>Acessibilidade

Cada membro de uma classe tem uma acessibilidade associada, que controla as regiões de texto do programa que são capazes de acessar o membro. Há cinco formas possíveis de acessibilidade. Esses métodos estão resumidos na tabela a seguir.


| __Acessibilidade__    | __Significado__ |
|----------------------|-----------------|
| `public`             | Acesso não limitado |
| `protected`          | Acesso limitado a essa classe ou classes derivadas dessa classe |
| `internal`           | Acesso limitado a este programa |
| `protected internal` | Acesso limitado a esse programa ou a classes derivadas dessa classe |
| `private`            | Acesso limitado a essa classe |

### <a name="type-parameters"></a>Parâmetros de tipo

Uma definição de classe pode especificar um conjunto de parâmetros de tipo seguindo o nome da classe com colchetes angulares com uma lista de nomes de parâmetro de tipo. Os parâmetros de tipo podem a ser usado no corpo das declarações de classe para definir os membros da classe. No exemplo a seguir, os parâmetros de tipo de `Pair` são `TFirst` e `TSecond`:

```csharp
public class Pair<TFirst,TSecond>
{
    public TFirst First;
    public TSecond Second;
}
```
Um tipo de classe é declarado para pegar parâmetros de tipo é chamado de um tipo de classe genérica. Tipos de struct, de interface e de delegado também podem ser genéricos.

Quando a classe genérica é usada, os argumentos de tipo devem ser fornecidos para cada um dos parâmetros de tipo:

```csharp
Pair<int,string> pair = new Pair<int,string> { First = 1, Second = "two" };
int i = pair.First;     // TFirst is int
string s = pair.Second; // TSecond is string
```
Um tipo genérico com argumentos de tipo fornecidos, como `Pair<int,string>
    ` acima, é chamado um tipo construído.

### <a name="base-classes"></a>Classes base

Uma declaração de classe pode especificar uma classe base, seguindo os parâmetros de nome da classe e tipo com dois-pontos e o nome da classe base. Omitir uma especificação de classe base é o mesmo que derivar do `object` de tipo. No exemplo a seguir, a classe base de `Point3D` é `Point` e a classe base de `Point` é `object`:

```csharp
public class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

public class Point3D: Point
{
    public int z;

    public Point3D(int x, int y, int z): base(x, y) {
        this.z = z;
    }
}
```
Uma classe herda os membros de sua classe base. Herança significa que uma classe contém implicitamente todos os membros de sua classe base, exceto para a instância e construtores estáticos e os destruidores da classe base. Uma classe derivada pode adicionar novos membros aos que ela herda, mas ela não pode remover a definição de um membro herdado. No exemplo anterior, `Point3D` herda os campos `x` e `y` de `Point` e cada instância `Point3D` contém três campos: `x`, `y` e `z`.

Existe uma conversão implícita de um tipo de classe para qualquer um de seus tipos de classe base. Portanto, uma variável de um tipo de classe pode referenciar uma instância dessa classe ou uma instância de qualquer classe derivada. Por exemplo, dadas as declarações de classe anteriores, uma variável do tipo `Point` podem referenciar um `Point` ou um `Point3D`:

```csharp
Point a = new Point(10, 20);
Point b = new Point3D(10, 20, 30);
```

### <a name="fields"></a>Campos

Um campo é uma variável que está associada a uma classe ou uma instância de uma classe.

Um campo declarado com o `static` modificador define uma ***campo estático***. Um campo estático identifica exatamente um local de armazenamento. Não importa quantas instâncias de uma classe são criadas, há sempre apenas uma cópia de um campo estático.

Um campo declarado sem o `static` modificador define uma ***campo de instância***. Cada instância de uma classe contém uma cópia separada de todos os campos de instância dessa classe.

No exemplo a seguir, cada instância da classe `Color` tem uma cópia separada dos campos de instância `r`, `g` e `b`, mas há apenas uma cópia dos campos estáticos `Black`, `White`, `Red`, `Green` e `Blue`:

```csharp
public class Color
{
    public static readonly Color Black = new Color(0, 0, 0);
    public static readonly Color White = new Color(255, 255, 255);
    public static readonly Color Red = new Color(255, 0, 0);
    public static readonly Color Green = new Color(0, 255, 0);
    public static readonly Color Blue = new Color(0, 0, 255);
    private byte r, g, b;

    public Color(byte r, byte g, byte b) {
        this.r = r;
        this.g = g;
        this.b = b;
    }
}
```
Conforme mostrado no exemplo anterior, os ***campos somente leitura*** podem ser declarados com um modificador `readonly`. Atribuição a um `readonly` campo só pode ocorrer como parte da declaração do campo ou em um construtor na mesma classe.

### <a name="methods"></a>Métodos

Um ***método*** é um membro que implementa um cálculo ou uma ação que pode ser executada por um objeto ou classe. Os ***métodos estáticos*** são acessados pela classe. Os ***métodos de instância*** são acessados pelas instâncias da classe.

Métodos têm uma lista (possivelmente vazia) de ***parâmetros***, que representam valores ou referências de variável passadas para o método e uma ***tipo de retorno***, que especifica o tipo do valor calculado e retornado pelo o método. Tipo de retorno do método é `void` se ele não retorna um valor.

Como os tipos, os métodos também podem ter um conjunto de parâmetros de tipo, para que os quais os argumentos de tipo devem ser especificados quando o método é chamado. Ao contrário dos tipos, os argumentos de tipo geralmente podem ser inferidos de argumentos de uma chamada de método e não precisam ser fornecidos explicitamente.

A ***assinatura*** de um método deve ser exclusiva na classe na qual o método é declarado. A assinatura de um método consiste no nome do método, número de parâmetros de tipo e número, modificadores e tipos de seus parâmetros. A assinatura de um método não inclui o tipo de retorno.

#### <a name="parameters"></a>Parâmetros

Os parâmetros são usados para passar valores ou referências de variável aos métodos. Os parâmetros de um método obtêm seus valores reais de ***argumentos*** que são especificados quando o método é invocado. Há quatro tipos de parâmetros: parâmetros de valor, parâmetros de referência, parâmetros de saída e matrizes de parâmetros.

Um ***parâmetro de valor*** é usado para passar o parâmetro de entrada. Um parâmetro de valor corresponde a uma variável local que obtém seu valor inicial do argumento passado para o parâmetro. As modificações em um parâmetro de valor não afetam o argumento passado para o parâmetro.

Os parâmetros de valor podem ser opcionais, especificando um valor padrão para que os argumentos correspondentes possam ser omitidos.

Um ***parâmetro de referência*** é usado para passar os parâmetros de entrada e saída. O argumento passado para um parâmetro de referência deve ser uma variável e, durante a execução do método, o parâmetro de referência representa o mesmo local de armazenamento como a variável de argumento. Um parâmetro de referência é declarado com o modificador `ref`. O exemplo a seguir mostra o uso de parâmetros `ref`.

```csharp
using System;

class Test
{
    static void Swap(ref int x, ref int y) {
        int temp = x;
        x = y;
        y = temp;
    }

    static void Main() {
        int i = 1, j = 2;
        Swap(ref i, ref j);
        Console.WriteLine("{0} {1}", i, j);            // Outputs "2 1"
    }
}
```
Um ***parâmetro de saída*** é usado para passar o parâmetro de entrada. Um parâmetro de saída é semelhante a um parâmetro de referência, exceto que o valor inicial do argumento fornecido não é importante. Um parâmetro de saída é declarado com o modificador `out`. O exemplo a seguir mostra o uso de parâmetros `out`.

```csharp
using System;

class Test
{
    static void Divide(int x, int y, out int result, out int remainder) {
        result = x / y;
        remainder = x % y;
    }

    static void Main() {
        int res, rem;
        Divide(10, 3, out res, out rem);
        Console.WriteLine("{0} {1}", res, rem);    // Outputs "3 1"
    }
}
```
Uma ***matriz de parâmetros*** permite que um número variável de argumentos sejam passados para um método. Uma matriz de parâmetro é declarada com o modificador `params`. Somente o último parâmetro de um método pode ser uma matriz de parâmetros e o tipo de uma matriz de parâmetros deve ser um tipo de matriz unidimensional. O `Write` e `WriteLine` métodos do `System.Console` classe são bons exemplos de uso da matriz de parâmetro. Eles são declarados como segue.

```csharp
public class Console
{
    public static void Write(string fmt, params object[] args) {...}
    public static void WriteLine(string fmt, params object[] args) {...}
    ...
}
```
Dentro de um método que usa uma matriz de parâmetros, a matriz de parâmetros se comporta exatamente como um parâmetro regular de um tipo de matriz. No entanto, em uma invocação de um método com uma matriz de parâmetros, é possível passar um único argumento do tipo da matriz de parâmetro ou qualquer número de argumentos do tipo de elemento da matriz de parâmetros. No último caso, uma instância de matriz é automaticamente criada e inicializada com os argumentos determinados. Esse exemplo

```csharp
Console.WriteLine("x={0} y={1} z={2}", x, y, z);
```
é equivalente ao escrito a seguir.

```csharp
string s = "x={0} y={1} z={2}";
object[] args = new object[3];
args[0] = x;
args[1] = y;
args[2] = z;
Console.WriteLine(s, args);
```

#### <a name="method-body-and-local-variables"></a>Corpo do método e variáveis locais

Um corpo do método Especifica as instruções para executar quando o método é invocado.

Um corpo de método pode declarar variáveis que são específicas para a invocação do método. Essas variáveis são chamadas de ***variáveis locais***. Uma declaração de variável local especifica um nome de tipo, um nome de variável e, possivelmente, um valor inicial. O exemplo a seguir declara uma variável local `i` com um valor inicial de zero e uma variável local `j` sem valor inicial.

```csharp
using System;

class Squares
{
    static void Main() {
        int i = 0;
        int j;
        while (i < 10) {
            j = i * i;
            Console.WriteLine("{0} x {0} = {1}", i, j);
            i = i + 1;
        }
    }
}
```
O C# requer que uma variável local seja ***atribuída definitivamente*** antes de seu valor poder ser obtido. Por exemplo, se a declaração do `i` anterior não incluísse um valor inicial, o compilador relataria um erro para usos subsequentes de `i` porque `i` não seria definitivamente atribuído a esses pontos do programa.

Um método pode usar instruções `return` para retornar o controle é pelo chamador. Em um método que retorna `void`, as instruções `return` não podem especificar uma expressão. Em um método de retorno não -`void`, `return` instruções devem incluir uma expressão que calcula o valor de retorno.

#### <a name="static-and-instance-methods"></a>Métodos estáticos e de instância

Um método declarado com um `static` modificador é uma ***método estático***. Um método estático não funciona em uma instância específica e pode acessar diretamente apenas membros estáticos.

Um método declarado sem um `static` modificador é um ***método de instância***. Um método de instância opera em uma instância específica e pode acessar membros estáticos e de instância. A instância em que um método de instância foi invocado pode ser explicitamente acessada como `this`. É um erro se referir a `this` em um método estático.

A seguinte classe `Entity` tem membros estáticos e de instância.

```csharp
class Entity
{
    static int nextSerialNo;
    int serialNo;

    public Entity() {
        serialNo = nextSerialNo++;
    }

    public int GetSerialNo() {
        return serialNo;
    }

    public static int GetNextSerialNo() {
        return nextSerialNo;
    }

    public static void SetNextSerialNo(int value) {
        nextSerialNo = value;
    }
}
```
Cada instância `Entity` contém um número de série (e, possivelmente, outras informações que não são mostradas aqui). O construtor `Entity` (que é como um método de instância) inicializa a nova instância com o próximo número de série disponível. Como o construtor é um membro de instância, ele tem permissão para acessar tanto o campo de instância `serialNo` e o campo estático `nextSerialNo`.

Os métodos estáticos `GetNextSerialNo` e `SetNextSerialNo` podem acessar o campo estático `nextSerialNo`, mas seria um erro para eles acessar diretamente o campo de instância `serialNo`.

O exemplo a seguir mostra o uso da `Entity` classe.

```csharp
using System;

class Test
{
    static void Main() {
        Entity.SetNextSerialNo(1000);
        Entity e1 = new Entity();
        Entity e2 = new Entity();
        Console.WriteLine(e1.GetSerialNo());           // Outputs "1000"
        Console.WriteLine(e2.GetSerialNo());           // Outputs "1001"
        Console.WriteLine(Entity.GetNextSerialNo());   // Outputs "1002"
    }
}
```
Observe que os métodos estáticos `SetNextSerialNo` e `GetNextSerialNo` são invocados na classe enquanto o método de instância `GetSerialNo` é chamado em instâncias da classe.

#### <a name="virtual-override-and-abstract-methods"></a>Métodos abstratos, virtuais e de substituição

Quando uma declaração de método de instância inclui um modificador `virtual`, o método deve ser um ***método virtual***. Quando nenhum `virtual` modificador estiver presente, o método deve ser um ***método não virtual***.

Quando um método virtual é invocado, o ***tipo de tempo de execução*** da instância para o qual essa invocação ocorre determina a implementação real do método para invocar. Em uma invocação de método não virtual, o ***tipo de tempo de compilação*** da instância é o fator determinante.

Um método virtual pode ser ***substituído*** em uma classe derivada. Quando uma declaração de método de instância inclui um `override` modificador, o método substitui um método virtual herdado com a mesma assinatura. Enquanto uma declaração de método virtual apresenta um novo método, uma declaração de método de substituição restringe um método virtual herdado existente fornecendo uma nova implementação do método.

Uma ***abstrata*** é um método virtual sem implementação. Um método abstrato é declarado com o `abstract` modificador e é permitido somente em uma classe que também é declarada `abstract`. Um método abstrato deve ser substituído em cada classe derivada não abstrata.

O exemplo a seguir declara uma classe abstrata, `Expression`, que representa um nó de árvore de expressão e três classes derivadas, `Constant`, `VariableReference` e `Operation`, que implementam nós de árvore de expressão para operações aritméticas, referências de variável e constantes. (Isso é semelhante, mas não deve para ser confundido com os tipos de árvore de expressão introduzido no [tipos de árvore de expressão](types.md#expression-tree-types)).

```csharp
using System;
using System.Collections;

public abstract class Expression
{
    public abstract double Evaluate(Hashtable vars);
}

public class Constant: Expression
{
    double value;

    public Constant(double value) {
        this.value = value;
    }

    public override double Evaluate(Hashtable vars) {
        return value;
    }
}

public class VariableReference: Expression
{
    string name;

    public VariableReference(string name) {
        this.name = name;
    }

    public override double Evaluate(Hashtable vars) {
        object value = vars[name];
        if (value == null) {
            throw new Exception("Unknown variable: " + name);
        }
        return Convert.ToDouble(value);
    }
}

public class Operation: Expression
{
    Expression left;
    char op;
    Expression right;

    public Operation(Expression left, char op, Expression right) {
        this.left = left;
        this.op = op;
        this.right = right;
    }

    public override double Evaluate(Hashtable vars) {
        double x = left.Evaluate(vars);
        double y = right.Evaluate(vars);
        switch (op) {
            case '+': return x + y;
            case '-': return x - y;
            case '*': return x * y;
            case '/': return x / y;
        }
        throw new Exception("Unknown operator");
    }
}
```
As quatro classes anteriores podem ser usadas para modelar expressões aritméticas. Por exemplo, usando instâncias dessas classes, a expressão `x + 3` pode ser representada da seguinte maneira.

```csharp
Expression e = new Operation(
    new VariableReference("x"),
    '+',
    new Constant(3));
```
O método `Evaluate` de uma instância `Expression` é chamado para avaliar a expressão especificada e produzir um valor `double`. O método recebe como argumento um `Hashtable` que contém nomes de variáveis (como chaves das entradas) e valores (como valores das entradas). O `Evaluate` é um método abstrato virtual, que significa que classes derivadas não abstratas devem o substituir para fornecer uma implementação real.

Uma implementação de `Evaluate` do `Constant` retorna apenas a constante armazenada. Um `VariableReference`retorna o valor resultante e da implementação procura o nome da variável da tabela de hash. Uma implementação de `Operation` primeiro avalia os operandos esquerdo e direito (chamando recursivamente seus métodos `Evaluate`) e, em seguida, executa a operação aritmética determinada.

O seguinte programa usa as classes `Expression` para avaliar a expressão `x * (y + 2)` para valores diferentes de `x` e `y`.

```csharp
using System;
using System.Collections;

class Test
{
    static void Main() {
        Expression e = new Operation(
            new VariableReference("x"),
            '*',
            new Operation(
                new VariableReference("y"),
                '+',
                new Constant(2)
            )
        );
        Hashtable vars = new Hashtable();
        vars["x"] = 3;
        vars["y"] = 5;
        Console.WriteLine(e.Evaluate(vars));        // Outputs "21"
        vars["x"] = 1.5;
        vars["y"] = 9;
        Console.WriteLine(e.Evaluate(vars));        // Outputs "16.5"
    }
}
```

#### <a name="method-overloading"></a>Sobrecarga de método

A ***sobrecarga*** de método permite que vários métodos na mesma classe tenham o mesmo nome, contanto que tenham assinaturas exclusivas. Ao compilar uma invocação de um método sobrecarregado, o compilador usa a ***resolução de sobrecarga*** para determinar o método específico para invocar. A resolução de sobrecarga localizará o método que melhor corresponde aos argumentos ou relatará um erro se nenhuma correspondência for encontrada. O exemplo a seguir mostra a resolução de sobrecarga em vigor. O comentário para cada invocação no método `Main` mostra qual método é realmente chamado.

```csharp
class Test
{
    static void F() {
        Console.WriteLine("F()");
    }

    static void F(object x) {
        Console.WriteLine("F(object)");
    }

    static void F(int x) {
        Console.WriteLine("F(int)");
    }

    static void F(double x) {
        Console.WriteLine("F(double)");
    }

    static void F<T>(T x) {
        Console.WriteLine("F<T>(T)");
    }

    static void F(double x, double y) {
        Console.WriteLine("F(double, double)");
    }

    static void Main() {
        F();                 // Invokes F()
        F(1);                // Invokes F(int)
        F(1.0);              // Invokes F(double)
        F("abc");            // Invokes F(object)
        F((double)1);        // Invokes F(double)
        F((object)1);        // Invokes F(object)
        F<int>(1);           // Invokes F<T>(T)
        F(1, 1);             // Invokes F(double, double)
    }
}
```
Conforme mostrado no exemplo, um determinado método sempre pode ser selecionado ao converter explicitamente os argumentos para os tipos de parâmetro exatos e/ou fornecendo explicitamente os argumentos de tipo.

### <a name="other-function-members"></a>Outros membros da função

Os membros que contêm código executável são conhecidos coletivamente como ***membros de função*** de uma classe. A seção anterior descreve os métodos, que são o tipo principal de membros da função. Esta seção descreve os outros tipos de membros da função comportados pelo c#: construtores, propriedades, indexadores, eventos, operadores e destruidores.

O código a seguir mostra uma classe genérica chamada `List<T>`, que implementa uma lista crescente de objetos. A classe contém vários exemplos dos tipos mais comuns de membros da função.


```csharp
public class List<T> {
    // Constant...
    const int defaultCapacity = 4;

    // Fields...
    T[] items;
    int count;

    // Constructors...
    public List(int capacity = defaultCapacity) {
        items = new T[capacity];
    }

    // Properties...
    public int Count {
        get { return count; }
    }
    public int Capacity {
        get {
            return items.Length;
        }
        set {
            if (value < count) value = count;
            if (value != items.Length) {
                T[] newItems = new T[value];
                Array.Copy(items, 0, newItems, 0, count);
                items = newItems;
            }
        }
    }

    // Indexer...
    public T this[int index] {
        get {
            return items[index];
        }
        set {
            items[index] = value;
            OnChanged();
        }
    }

    // Methods...
    public void Add(T item) {
        if (count == Capacity) Capacity = count * 2;
        items[count] = item;
        count++;
        OnChanged();
    }
    protected virtual void OnChanged() {
        if (Changed != null) Changed(this, EventArgs.Empty);
    }
    public override bool Equals(object other) {
        return Equals(this, other as List<T>);
    }
    static bool Equals(List<T> a, List<T> b) {
        if (a == null) return b == null;
        if (b == null || a.count != b.count) return false;
        for (int i = 0; i < a.count; i++) {
            if (!object.Equals(a.items[i], b.items[i])) {
                return false;
            }
        }
        return true;
    }

    // Event...
    public event EventHandler Changed;

    // Operators...
    public static bool operator ==(List<T> a, List<T> b) {
        return Equals(a, b);
    }
    public static bool operator !=(List<T> a, List<T> b) {
        return !Equals(a, b);
    }
}
```

#### <a name="constructors"></a>Construtores

O C# dá suporte aos construtores estáticos e de instância. Um ***construtor de instância*** é um membro que implementa as ações necessárias para inicializar uma instância de uma classe. Um ***construtor estático*** é um membro que implementa as ações necessárias para inicializar uma classe quando ele for carregado pela primeira vez.

Um construtor é declarado como um método sem nenhum tipo de retorno e o mesmo nome que a classe continente. Se uma declaração de construtor inclui um `static` modificador, ele declara um construtor estático. Caso contrário, ela declara um construtor de instância.

Construtores de instância podem ser sobrecarregados. Por exemplo, a classe `List<T>
` declara dois construtores de instância, um sem parâmetros e um que utiliza um parâmetro `int`. Os construtores de instância são invocados usando o operador `new`. As seguintes instruções alocam duas `List<string>
` instâncias usando cada um dos construtores do `List` classe.

```csharp
List<string> list1 = new List<string>();
List<string> list2 = new List<string>(10);
```
Diferentemente de outros membros, construtores de instância não são herdados e uma classe não tem nenhum construtor de instância que não os que são realmente declarados na classe. Se nenhum construtor de instância for fornecido para uma classe, então um construtor vazio sem parâmetros será fornecido automaticamente.

#### <a name="properties"></a>Propriedades

As ***propriedades*** são uma extensão natural dos campos. Elas são denominadas membros com tipos associados, e a sintaxe para acessar os campos e as propriedades é a mesma. No entanto, diferentemente dos campos, as propriedades não denotam locais de armazenamento. Em vez disso, as propriedades têm ***acessadores*** que especificam as instruções a serem executadas quando os valores forem lidos ou gravados.

Uma propriedade é declarada como um campo, exceto que a declaração termina com um `get` acessador e/ou um `set` escrito entre os delimitadores de acessador `{` e `}` em vez de terminar em um ponto e vírgula. Uma propriedade que tem ambos um `get` acessador e um `set` acessador é uma ***propriedade de leitura / gravação***, uma propriedade que tem apenas um `get` acessador é uma ***propriedade somente leitura***e um propriedade que tem apenas um `set` acessador é uma ***propriedade somente gravação***.

Um `get` acessador corresponde a um método sem parâmetros com um valor de retorno do tipo de propriedade. Exceto como o destino de uma atribuição, quando uma propriedade é referenciada em uma expressão, o `get` acessador da propriedade é invocado para calcular o valor da propriedade.

Um `set` acessador corresponde a um método com um parâmetro único chamado `value` e nenhum tipo de retorno. Quando uma propriedade é referenciada como o destino de uma atribuição ou como o operando da `++` ou `--`, o `set` acessador é invocado com um argumento que fornece o novo valor.

A classe `List<T>
` declara duas propriedades, `Count` e `Capacity`, que são somente leitura e leitura/gravação, respectivamente. A seguir está um exemplo de uso dessas propriedades.

```csharp
List<string> names = new List<string>();
names.Capacity = 100;            // Invokes set accessor
int i = names.Count;             // Invokes get accessor
int j = names.Capacity;          // Invokes get accessor
```
Como nos campos e métodos, o C# dá suporte a propriedades de instância e a propriedades estáticas. Propriedades estáticas são declaradas com o `static` modificador e as propriedades de instância são declaradas sem ele.

Os acessadores de uma propriedade podem ser virtuais. Quando uma declaração de propriedade inclui um modificador `virtual`, `abstract` ou `override`, ela se aplica aos acessadores da propriedade.

#### <a name="indexers"></a>Indexadores

Um ***indexador*** é um membro que permite que objetos sejam indexados da mesma forma que uma matriz. Um indexador é declarado como uma propriedade, exceto que é o nome do membro `this` seguido por uma lista de parâmetros escrita entre os delimitadores `[` e `]`. Os parâmetros estão disponíveis nos acessadores do indexador. Semelhante às propriedades, os indexadores podem ser de leitura-gravação, somente leitura e somente gravação, e os acessadores de um indexador pode ser virtuais.

A classe `List` declara um indexador único de leitura-gravação que usa um parâmetro `int`. O indexador possibilita indexar instâncias `List` com valores `int`. Por exemplo

```csharp
List<string> names = new List<string>();
names.Add("Liz");
names.Add("Martha");
names.Add("Beth");
for (int i = 0; i < names.Count; i++) {
    string s = names[i];
    names[i] = s.ToUpper();
}
```
Os indexadores podem ser sobrecarregados, o que significa que uma classe pode declarar vários indexadores, desde que o número ou os tipos de seus parâmetros sejam diferentes.

#### <a name="events"></a>Eventos

Um ***evento*** é um membro que permite que uma classe ou objeto forneça notificações. Um evento é declarado como um campo, exceto que a declaração inclui um `event` palavra-chave e o tipo devem ser um tipo de delegado.

Em uma classe que declara um membro de evento, o evento se comporta exatamente como um campo de um tipo delegado (desde que o evento não seja abstrato e não declare acessadores). O campo armazena uma referência a um delegado que representa os manipuladores de eventos que foram adicionados ao evento. Se nenhum identificador de evento estiver presente, o campo é `null`.

A classe `List<T>
` declara um membro único de evento chamado `Changed`, que indica que um novo item foi adicionado à lista. O `Changed` é gerado pela `OnChanged` método virtual, que primeiro verifica se o evento é `null` (o que significa que nenhum manipulador está presente). A noção de gerar um evento é precisamente equivalente a invocar o delegado representado pelo evento — assim, não há constructos de linguagem especial para gerar eventos.

Os clientes reagem a eventos por meio de ***manipuladores de eventos***. Os manipuladores de eventos são conectados usando o operador `+=` e removidos usando o operador `-=`. O exemplo a seguir anexa um manipulador de eventos para o evento `Changed` de um `List<string>
`.

```csharp
using System;

class Test
{
    static int changeCount;

    static void ListChanged(object sender, EventArgs e) {
        changeCount++;
    }

    static void Main() {
        List<string> names = new List<string>();
        names.Changed += new EventHandler(ListChanged);
        names.Add("Liz");
        names.Add("Martha");
        names.Add("Beth");
        Console.WriteLine(changeCount);        // Outputs "3"
    }
}
```
Para cenários avançados nos quais o controle do armazenamento subjacente de um evento é desejado, uma declaração de evento pode fornecer explicitamente acessadores `add` e `remove`, que são um pouco semelhantes ao acessador `set` de uma propriedade.

#### <a name="operators"></a>Operadores

Um ***operador*** é um membro que define o significado da aplicação de um operador de expressão específico para instâncias de uma classe. Três tipos de operadores podem ser definidos: operadores unários, operadores binários e operadores de conversão. Todos os operadores devem ser declarados como `public` e `static`.

A classe `List<T>
` declara dois operadores, `operator==` e `operator!=` e, portanto, dá um novo significado para as expressões que aplicam esses operadores a instâncias `List`. Especificamente, os operadores de definem a igualdade de duas `List<T>
` instâncias como comparar cada um dos objetos contidos usando seus `Equals` métodos. O exemplo a seguir usa o operador `==` para comparar duas instâncias `List<int>
`.

```csharp
using System;

class Test
{
    static void Main() {
        List<int> a = new List<int>();
        a.Add(1);
        a.Add(2);
        List<int> b = new List<int>();
        b.Add(1);
        b.Add(2);
        Console.WriteLine(a == b);        // Outputs "True"
        b.Add(3);
        Console.WriteLine(a == b);        // Outputs "False"
    }
}
```

O primeiro `Console.WriteLine` gera `True` porque as duas listas contêm o mesmo número de objetos com os mesmos valores na mesma ordem. Como `List<T>
` não definiu `operator==`, o primeiro `Console.WriteLine` geraria `False` porque `a` e `b` referenciam diferentes instâncias `List<int>
`.

#### <a name="destructors"></a>Destruidores

Um ***destruidor*** é um membro que implementa as ações necessárias para destruir uma instância de uma classe. Destruidores não podem ter parâmetros, eles não podem ter modificadores de acessibilidade e não pode ser invocados explicitamente. O destruidor para uma instância é invocado automaticamente durante a coleta de lixo.

O coletor de lixo tem latitude ampla ao decidir quando coletar objetos e executar os destruidores. Especificamente, o tempo de invocações de destruidor não é determinístico e destruidores podem ser executados em qualquer thread. Para esses e outros motivos, as classes devem implementar os destruidores somente quando não houver outras soluções viáveis.

A instrução `using` fornece uma abordagem melhor para a destruição de objetos.

## <a name="structs"></a>Structs

Como classes, os ***structs*** são estruturas de dados que podem conter membros de dados e os membros da função, mas, ao contrário das classes, as estruturas são tipos de valor e não precisam de alocação de heap. Uma variável de um tipo struct armazena diretamente os dados do struct, enquanto que uma variável de um tipo de classe armazena uma referência a um objeto alocado dinamicamente. Os tipos de estrutura não dão suporte à herança especificada pelo usuário, e todos os tipos de structs são herdados implicitamente do tipo `object`.

Os structs são particularmente úteis para estruturas de dados pequenas que têm semântica de valor. Números complexos, pontos em um sistema de coordenadas ou pares chave-valor em um dicionário são exemplos de structs. O uso de structs, em vez de classes para estruturas de dados pequenas, pode fazer uma grande diferença no número de alocações de memória que um aplicativo executa. Por exemplo, o programa a seguir cria e inicializa uma matriz de 100 pontos. Com `Point` implementado como uma classe, 101 objetos separados são instanciados — um para a matriz e um para os elementos de 100.

```csharp
class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

class Test
{
    static void Main() {
        Point[] points = new Point[100];
        for (int i = 0; i < 100; i++) points[i] = new Point(i, i);
    }
}
```
Uma alternativa é fazer `Point` um struct.

```csharp
struct Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
Agora, somente um objeto é instanciado — um para a matriz — e as instâncias `Point` são armazenadas em linha na matriz.

Construtores de struct são invocados com o operador `new`, mas isso não significa que a memória está sendo alocada. Em vez de alocar dinamicamente um objeto e retornar uma referência a ele, um construtor de struct simplesmente retorna o valor do struct (normalmente em um local temporário na pilha), e esse valor é, então, copiado conforme necessário.

Com classes, é possível que duas variáveis referenciem o mesmo objeto e, portanto, é possível que operações em uma variável afetem o objeto referenciado por outra variável. Com structs, as variáveis têm sua própria cópia dos dados e não é possível que as operações em um afetem o outro. Por exemplo, a saída produzida pelo seguinte fragmento de código depende de se `Point` é uma classe ou estrutura.

```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 20;
Console.WriteLine(b.x);
```
Se `Point` é uma classe, a saída é `20` porque `a` e `b` referenciam o mesmo objeto. Se `Point` é um struct, a saída é `10` porque a atribuição de `a` à `b` cria uma cópia do valor, e essa cópia não é afetada pela atribuição subsequente para `a.x`.

O exemplo anterior destaca duas das limitações dos structs. Primeiro, copiar um struct inteiro é, geralmente, menos eficiente do que copiar uma referência de objeto, então a passagem de atribuição e de valor do parâmetro pode ser mais custosa com structs que com tipos de referência. Segundo, com exceção para parâmetros `ref` e `out`, não é possível criar referências para structs, o que rege o uso em diversas situações.

## <a name="arrays"></a>Matrizes

Uma ***matriz*** é uma estrutura de dados que contém algumas variáveis acessadas por meio de índices calculados. As variáveis contidas em uma matriz, também chamadas de ***elementos*** da matriz, são todas do mesmo tipo, e esse tipo é chamado de ***tipo de elemento*** da matriz.

Os tipos de matriz são tipos de referência, e a declaração de uma variável de matriz simplesmente reserva espaço para uma referência a uma instância de matriz. Instâncias reais da matriz são criadas dinamicamente em tempo de execução usando o `new` operador. O `new` operação Especifica a ***comprimento*** da nova instância de matriz, que depois fica fixa para o tempo de vida da instância. Os índices dos elementos de uma matriz variam de `0` a `Length - 1`. O operador `new` inicializa automaticamente os elementos de uma matriz usando o valor padrão, que, por exemplo, é zero para todos os tipos numéricos e `null` para todos os tipos de referência.

O exemplo a seguir cria uma matriz de elementos `int`, inicializa a matriz e imprime o conteúdo da matriz.

```csharp
using System;

class Test
{
    static void Main() {
        int[] a = new int[10];
        for (int i = 0; i < a.Length; i++) {
            a[i] = i * i;
        }
        for (int i = 0; i < a.Length; i++) {
            Console.WriteLine("a[{0}] = {1}", i, a[i]);
        }
    }
}
```
Este exemplo cria e opera em uma ***matriz unidimensional***. O C# também oferece suporte a ***matrizes multidimensionais***. O número de dimensões de um tipo de matriz, também conhecido como ***classificação*** do tipo de matriz, é o número um mais o número de vírgulas escrito entre os colchetes do tipo de matriz. O exemplo a seguir aloca um unidimensional, bidimensional e uma matriz tridimensional.

```csharp
int[] a1 = new int[10];
int[,] a2 = new int[10, 5];
int[,,] a3 = new int[10, 5, 2];
```
A matriz `a1` contém 10 elementos, a matriz `a2` contém 50 (10 × 5) elementos e a matriz `a3` contém 100 (10 × 5 × 2) elementos.

O tipo do elemento de uma matriz pode ser qualquer tipo, incluindo um tipo de matriz. Uma matriz com elementos de um tipo de matriz é chamada às vezes de ***matriz denteada***, pois os tamanhos das matrizes do elemento nem sempre precisam ser iguais. O exemplo a seguir aloca uma matriz de matrizes de `int`:

```csharp
int[][] a = new int[3][];
a[0] = new int[10];
a[1] = new int[5];
a[2] = new int[20];
```
A primeira linha cria uma matriz com três elementos, cada um do tipo `int[]`, e cada um com um valor inicial de `null`. As linhas subsequentes inicializam os três elementos com referências às instâncias individuais da matriz de tamanhos variados.

O `new` operador permite que os valores iniciais dos elementos da matriz seja especificada com um ***inicializador de matriz***, que é uma lista de expressões escritas entre os delimitadores `{` e `}`. O exemplo a seguir aloca e inicializa um `int[]` com três elementos.

```csharp
int[] a = new int[] {1, 2, 3};
```
Observe que o comprimento da matriz é inferido do número de expressões entre `{` e `}`. A variável local e declarações de campo podem ser reduzidas ainda mais, de modo que o tipo de matriz não precise ser redefinido.

```csharp
int[] a = {1, 2, 3};
```
Os dois exemplos anteriores são equivalentes ao seguinte:

```csharp
int[] t = new int[3];
t[0] = 1;
t[1] = 2;
t[2] = 3;
int[] a = t;
```
## <a name="interfaces"></a>Interfaces

Uma ***interface*** define um contrato que pode ser implementado por classes e estruturas. Uma interface pode conter métodos, propriedades, eventos e indexadores. Uma interface não fornece implementações dos membros que define — ela simplesmente especifica os membros que devem ser fornecidos por classes ou estruturas que implementam a interface.

As interfaces podem empregar a ***herança múltipla***. No exemplo a seguir, a interface `IComboBox` herda de `ITextBox` e `IListBox`.

```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

interface IComboBox: ITextBox, IListBox {}
```
Classes e structs podem implementar várias interfaces. No exemplo a seguir, a classe `EditBox` implementa `IControl` e `IDataBound`.

```csharp
interface IDataBound
{
    void Bind(Binder b);
}

public class EditBox: IControl, IDataBound
{
    public void Paint() {...}
    public void Bind(Binder b) {...}
}
```
Quando uma classe ou struct implementa uma interface específica, as instâncias dessa classe ou struct podem ser convertidas implicitamente para esse tipo de interface. Por exemplo

```csharp
EditBox editBox = new EditBox();
IControl control = editBox;
IDataBound dataBound = editBox;
```
Em casos nos quais uma instância não é conhecida por ser estática para implementar uma interface específica, é possível usar conversões de tipo dinâmico. Por exemplo, as instruções a seguir usam conversões de tipo dinâmico para obter um objeto `IControl` e `IDataBound` implementações de interface. Porque o tipo real do objeto é `EditBox`, as conversões são bem-sucedidas.

```csharp
object obj = new EditBox();
IControl control = (IControl)obj;
IDataBound dataBound = (IDataBound)obj;
```
Anteriormente na `EditBox` classe, o `Paint` método da `IControl` interface e o `Bind` método da `IDataBound` interface são implementados usando `public` membros. C# também dá suporte a ***implementações de membros de interface explícita***, com o qual a classe ou struct pode evitar a tornar os membros `public`. Uma implementação de membro de interface explícita é escrita usando o nome do membro de interface totalmente qualificado. Por exemplo, a classe `EditBox` pode implementar os métodos `IControl.Paint` e `IDataBound.Bind` usando implementações de membros de interface explícita da seguinte maneira.

```csharp
public class EditBox: IControl, IDataBound
{
    void IControl.Paint() {...}
    void IDataBound.Bind(Binder b) {...}
}
```
Os membros de interface explícita só podem ser acessados por meio do tipo de interface. Por exemplo, a implementação de `IControl.Paint` fornecidos pelo anterior `EditBox` classe só pode ser chamada convertendo primeiro o `EditBox` de referência para o `IControl` tipo de interface.

```csharp
EditBox editBox = new EditBox();
editBox.Paint();                        // Error, no such method
IControl control = editBox;
control.Paint();                        // Ok
```

## <a name="enums"></a>Enums

Um ***tipo enum*** é um tipo de valor diferente com um conjunto de constantes nomeadas. O exemplo a seguir declara e usa um tipo de enumeração denominado `Color` com três valores de constantes `Red`, `Green`, e `Blue`.

```csharp
using System;

enum Color
{
    Red,
    Green,
    Blue
}

class Test
{
    static void PrintColor(Color color) {
        switch (color) {
            case Color.Red:
                Console.WriteLine("Red");
                break;
            case Color.Green:
                Console.WriteLine("Green");
                break;
            case Color.Blue:
                Console.WriteLine("Blue");
                break;
            default:
                Console.WriteLine("Unknown color");
                break;
        }
    }

    static void Main() {
        Color c = Color.Red;
        PrintColor(c);
        PrintColor(Color.Blue);
    }
}
```
Cada tipo de enumeração tem um tipo integral correspondente chamado de ***tipo subjacente*** o tipo de enumeração. Um tipo de enumeração que não declara explicitamente um tipo subjacente tem um tipo subjacente de `int`. O formato de armazenamento e intervalo de valores possíveis de um tipo de enumeração são determinados pelo seu tipo subjacente. O conjunto de valores que pode assumir um tipo de enumeração não é limitado por seus membros de enum. Em particular, qualquer valor do tipo subjacente de uma enumeração pode ser convertido para o tipo de enumeração e é um valor válido diferente daquele tipo enum.

O exemplo a seguir declara um tipo de enumeração denominado `Alignment` com um tipo subjacente de `sbyte`.

```csharp
enum Alignment: sbyte
{
    Left = -1,
    Center = 0,
    Right = 1
}
```
Conforme mostrado no exemplo anterior, uma declaração de membro de enumeração pode incluir uma expressão constante que especifica o valor do membro. O valor da constante para cada membro de enumeração deve ser no intervalo do tipo subjacente da enumeração. Quando uma declaração de membro de enumeração não especifica explicitamente um valor, o membro recebe o valor zero (se ele for o primeiro membro no tipo enum) ou o valor do membro enum textualmente precedente mais um.

Valores de enumeração podem ser convertidos em valores integrais e vice-versa usando conversões de tipo. Por exemplo

```csharp
int i = (int)Color.Blue;        // int i = 2;
Color c = (Color)2;             // Color c = Color.Blue;
```
O valor padrão de qualquer tipo de enumeração é o valor integral zero convertido para o tipo de enumeração. Em casos em que as variáveis são inicializadas automaticamente como um valor padrão, isso é o valor fornecido para variáveis de tipos enum. Para que o valor padrão de um tipo de enumeração fique facilmente disponível, o literal `0` converte implicitamente para qualquer tipo de enumeração. Dessa forma, o seguinte é permitido.

```csharp
Color c = 0;
```

## <a name="delegates"></a>Delegados

Um ***delegado*** é um tipo que representa referências aos métodos com uma lista de parâmetros e tipo de retorno específicos. Delegados possibilitam o tratamento de métodos como entidades que podem ser atribuídos a variáveis e passadas como parâmetros. Os delegados são parecidos com o conceito de ponteiros de função em outras linguagens, mas ao contrário dos ponteiros de função, os delegados são orientados a objetos e fortemente tipados.

O exemplo a seguir declara e usa um tipo delegado chamado `Function`.

```csharp
using System;

delegate double Function(double x);

class Multiplier
{
    double factor;

    public Multiplier(double factor) {
        this.factor = factor;
    }

    public double Multiply(double x) {
        return x * factor;
    }
}

class Test
{
    static double Square(double x) {
        return x * x;
    }

    static double[] Apply(double[] a, Function f) {
        double[] result = new double[a.Length];
        for (int i = 0; i < a.Length; i++) result[i] = f(a[i]);
        return result;
    }

    static void Main() {
        double[] a = {0.0, 0.5, 1.0};
        double[] squares = Apply(a, Square);
        double[] sines = Apply(a, Math.Sin);
        Multiplier m = new Multiplier(2.0);
        double[] doubles =  Apply(a, m.Multiply);
    }
}
```
Uma instância do tipo delegado `Function` pode fazer referência a qualquer método que usa um argumento `double` e retorna um valor `double`. O `Apply` método aplica-se um determinado `Function` para os elementos de uma `double[]`, retornando um `double[]` com os resultados. No método `Main`, `Apply` é usado para aplicar três funções diferentes para um `double[]`.

Um delegado pode referenciar um método estático (como `Square` ou `Math.Sin` no exemplo anterior) ou um método de instância (como `m.Multiply` no exemplo anterior). Um delegado que referencia um método de instância também referencia um objeto específico, e quando o método de instância é invocado por meio do delegado, esse objeto se torna `this` na invocação.

Os delegados podem ser criados usando funções anônimas, que são "métodos embutidos" criados dinamicamente. As funções anônimas podem ver as variáveis locais dos métodos ao redor. Portanto, o exemplo de multiplicador acima pode ser gravado mais facilmente sem usar um `Multiplier` classe:

```csharp
double[] doubles =  Apply(a, (double x) => x * 2.0);
```
Uma propriedade interessante e útil de um delegado é que ele não sabe ou se importa com a classe do método que referencia; o que importa é que o método referenciado tem os mesmos parâmetros e o tipo de retorno do delegado.

## <a name="attributes"></a>Atributos

Tipos, membros e outras entidades em um programa C# dão suporte a modificadores que controlam determinados aspectos de seu comportamento. Por exemplo, a acessibilidade de um método é controlada usando os modificadores `public`, `protected`, `internal` e `private`. O C# generaliza essa funcionalidade, de modo que os tipos definidos pelo usuário de informações declarativas podem ser anexados a entidades de programa e recuperados no tempo de execução. Os programas especificam essas informações declarativas adicionais, definindo e usando os ***atributos***.

O exemplo a seguir declara um atributo `HelpAttribute` que pode ser colocado em entidades de programa para fornecem links para a documentação associada.

```csharp
using System;

public class HelpAttribute: Attribute
{
    string url;
    string topic;

    public HelpAttribute(string url) {
        this.url = url;
    }

    public string Url {
        get { return url; }
    }

    public string Topic {
        get { return topic; }
        set { topic = value; }
    }
}
```
Todas as classes de atributo derivam o `System.Attribute` fornecido pelo .NET Framework de classe base. Os atributos podem ser aplicados, fornecendo seu nome, junto com quaisquer argumentos, dentro dos colchetes pouco antes da declaração associada. Se o nome de um atributo termina em `Attribute`, essa parte do nome pode ser omitida quando o atributo é referenciado. Por exemplo, o atributo `HelpAttribute` pode ser usado da seguinte maneira.

```csharp
[Help("http://msdn.microsoft.com/.../MyClass.htm")]
public class Widget
{
    [Help("http://msdn.microsoft.com/.../MyClass.htm", Topic = "Display")]
    public void Display(string text) {}
}
```
Este exemplo anexa um `HelpAttribute` para o `Widget` classe e outro `HelpAttribute` para o `Display` método na classe. Os construtores públicos de uma classe de atributo controlam as informações que devem ser fornecidas quando o atributo é anexado a uma entidade de programa. As informações adicionais podem ser fornecidas ao referenciar propriedades públicas de leitura-gravação da classe de atributo (como a referência anterior à propriedade `Topic`).

O exemplo a seguir mostra como as informações de atributo para uma entidade de determinado programa podem ser recuperadas em tempo de execução usando reflexão.

```csharp
using System;
using System.Reflection;

class Test
{
    static void ShowHelp(MemberInfo member) {
        HelpAttribute a = Attribute.GetCustomAttribute(member,
            typeof(HelpAttribute)) as HelpAttribute;
        if (a == null) {
            Console.WriteLine("No help for {0}", member);
        }
        else {
            Console.WriteLine("Help for {0}:", member);
            Console.WriteLine("  Url={0}, Topic={1}", a.Url, a.Topic);
        }
    }

    static void Main() {
        ShowHelp(typeof(Widget));
        ShowHelp(typeof(Widget).GetMethod("Display"));
    }
}
```
Quando um atributo específico for solicitado por meio de reflexão, o construtor para a classe de atributo será invocado com as informações fornecidas na origem do programa e a instância do atributo resultante será retornada. Se forem fornecidas informações adicionais por meio de propriedades, essas propriedades serão definidas para os valores fornecidos antes que a instância do atributo seja retornada.
