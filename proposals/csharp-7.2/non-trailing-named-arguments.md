---
ms.openlocfilehash: ac2b233eb703b5eea3bd2dfdbeeadd7494b0c695
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484667"
---
# <a name="non-trailing-named-arguments"></a>Argumentos nomeados que não estejam à direita

## <a name="summary"></a>Resumo
[summary]: #summary
Permitir que os argumentos nomeados sejam usados em posição não à direita, desde que eles sejam usados na posição correta. Por exemplo: `DoSomething(isEmployed:true, name, age);`.

## <a name="motivation"></a>Motivação
[motivation]: #motivation

A principal motivação é evitar digitar informações redundantes. É comum nomear um argumento que é um literal (como `null`, `true`) com a finalidade de esclarecer o código, em vez de passar argumentos fora de ordem.
Que atualmente não é permitido (`CS1738`), a menos que todos os argumentos a seguir também sejam nomeados.

```csharp
DoSomething(isEmployed:true, name, age); // currently disallowed, even though all arguments are in position
// CS1738 "Named argument specifications must appear after all fixed arguments have been specified"
```

Alguns exemplos adicionais:
```csharp
public void DoSomething(bool isEmployed, string personName, int personAge) { ... }

DoSomething(isEmployed:true, name, age); // currently CS1738, but would become legal
DoSomething(true, personName:name, age); // currently CS1738, but would become legal
DoSomething(name, isEmployed:true, age); // remains illegal
DoSomething(name, age, isEmployed:true); // remains illegal
DoSomething(true, personAge:age, personName:name); // already legal
```

Isso também funcionaria com params:
```csharp
public class Task
{
    public static Task When(TaskStatus all, TaskStatus any, params Task[] tasks);
}
Task.When(all: TaskStatus.RanToCompletion, any: TaskStatus.Faulted, task1, task2)
```

## <a name="detailed-design"></a>Design detalhado
[design]: #detailed-design

Em § 7.5.1 (listas de argumentos), a especificação atualmente diz:
> Um *argumento* com um *nome de argumento* é conhecido como um __argumento nomeado__, enquanto que um *argumento* sem um *nome de argumento* é um __argumento posicional__. É um erro para que um argumento posicional apareça após um argumento nomeado em uma *lista de argumentos*.

A proposta é remover esse erro e atualizar as regras para localizar o parâmetro correspondente para um argumento (§ 7.5.1.1):

Argumentos no argumento-lista de construtores de instância, métodos, indexadores e delegados:
- [regras existentes]
- Um argumento sem nome corresponde a nenhum parâmetro quando é depois de um argumento nomeado fora de posição ou um argumento chamado params.

Em particular, isso impede a invocação de `void M(bool a = true, bool b = true, bool c = true, );` com `M(c: false, valueB);`. O primeiro argumento é usado fora de posição (o argumento é usado na primeira posição, mas o parâmetro chamado "c" está na terceira posição), portanto, os argumentos a seguir devem ser nomeados.

Em outras palavras, argumentos nomeados não à direita são permitidos somente quando o nome e a posição resultam na localização do mesmo parâmetro correspondente.

## <a name="drawbacks"></a>Desvantagens
[drawbacks]: #drawbacks

Essa proposta exacerba as sutilezas existentes com argumentos nomeados na resolução de sobrecarga. Por exemplo:

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

Você pode obter essa situação hoje alternando os parâmetros:

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

Da mesma forma, se você tiver dois métodos `void M(int a, int b)` e `void M(int x, string y)`, a invocação errada `M(x: 1, 2)` produzirá um diagnóstico baseado na segunda sobrecarga ("não é possível converter de ' int ' em ' String '"). Esse problema já existe quando o argumento nomeado é usado em uma posição à direita.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

Há algumas alternativas a serem consideradas:

- O status quo
- Fornecer assistência IDE para preencher todos os nomes de argumentos à direita quando você digita um nome específico no meio.

Ambos sofrem com mais detalhes, pois eles introduzem vários argumentos nomeados, mesmo que você precise apenas de um nome de um literal no início da lista de argumentos.

## <a name="unresolved-questions"></a>Perguntas não resolvidas
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a>Criar reuniões
[ldm]: #ldm
O recurso foi brevemente discutido no LDM em 16 de maio de 2017, com aprovação no princípio (OK para mudar para a proposta/protótipo). Ele também foi brevemente discutido em 28 de junho de 2017.

Relacionado à discussão inicial https://github.com/dotnet/csharplang/issues/518 relacionado ao problema procedo https://github.com/dotnet/csharplang/issues/570
