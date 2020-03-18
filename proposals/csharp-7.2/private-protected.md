---
ms.openlocfilehash: 6cf489595654236c18edee94c0af380e605c9571
ms.sourcegitcommit: f61a06970fa0562d2e40363fae3948eb168624ca
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/14/2020
ms.locfileid: "79485171"
---
# <a name="private-protected"></a>privado protegido

* [x] proposta
* [x] protótipo: [concluído](https://github.com/dotnet/roslyn/blob/master/docs/features/private-protected.md)
* [x] implementação: [concluída](https://github.com/dotnet/roslyn/blob/master/docs/features/private-protected.md)
* [x] especificação: [concluída](#detailed-design)

## <a name="summary"></a>Resumo
[summary]: #summary

Expor o nível de acessibilidade `protectedAndInternal` CLR C# no as `private protected`.

## <a name="motivation"></a>Motivação
[motivation]: #motivation

Há muitas circunstâncias em que uma API contém membros que se destinam apenas a serem implementados e usados por subclasses contidas no assembly que fornece o tipo. Embora o CLR forneça um nível de acessibilidade para essa finalidade, ele não está disponível C#no. Consequentemente, os proprietários da API são forçados a usar a proteção `internal` e autodisciplinar ou um analisador personalizado, ou para usar `protected` com documentação adicional explicando que, embora o membro apareça na documentação pública do tipo, ele não se destina a fazer parte da API pública.  Para obter exemplos do último, consulte membros de `CSharpCompilationOptions` do Roslyn cujos nomes começam com `Common`.

O fornecimento direto de suporte para esse nível C# de acesso no permite que essas circunstâncias sejam expressas naturalmente na linguagem.

## <a name="detailed-design"></a>Design detalhado
[design]: #detailed-design

### <a name="private-protected-access-modifier"></a>modificador de acesso `private protected`

Sugerimos a adição de uma nova combinação de modificadores de acesso `private protected` (que pode aparecer em qualquer ordem entre os modificadores). Isso é mapeado para a noção de protectedAndInternal do CLR e empresta a mesma sintaxe atualmente usada em [ C++/CLI](https://docs.microsoft.com/cpp/dotnet/how-to-define-and-consume-classes-and-structs-cpp-cli#BKMK_Member_visibility).

Um membro declarado `private protected` pode ser acessado dentro de uma subclasse de seu contêiner se essa subclasse estiver no mesmo assembly que o membro.

Modificamos a especificação da linguagem da seguinte maneira (adições em negrito). Os números de seção não são mostrados abaixo, pois podem variar dependendo de qual versão da especificação está integrada.

-----

> A acessibilidade declarada de um membro pode ser uma das seguintes:
- Público, que é selecionado incluindo um modificador público na declaração de membro. O significado intuitivo do público é "acesso não limitado".
- Protegido, que é selecionado com a inclusão de um modificador protegido na declaração de membro. O significado intuitivo de protegido é "acesso limitado à classe que a contém ou tipos derivados da classe que a contém".
- Interno, que é selecionado incluindo um modificador interno na declaração de membro. O significado intuitivo de interno é "acesso limitado a este assembly".
- Interno protegido, que é selecionado incluindo um modificador protegido e um interno na declaração de membro. O significado intuitivo de interno protegido é "acessível dentro desse assembly, bem como os tipos derivados da classe que a contém".
- **Privada protegida, que é selecionada com a inclusão de um modificador privado e protegido na declaração de membro. O significado intuitivo de particular protegido é "acessível nesse assembly por tipos derivados da classe que a contém".**

-----

> Dependendo do contexto no qual uma declaração de membro ocorre, somente determinados tipos de acessibilidade declarada são permitidos. Além disso, quando uma declaração de membro não inclui nenhum modificador de acesso, o contexto no qual a declaração ocorre determina a acessibilidade declarada padrão. 
- Os namespaces implicitamente têm acessibilidade declarada pública. Nenhum modificador de acesso é permitido em declarações de namespace.
- Tipos declarados diretamente em unidades de compilação ou namespaces (ao contrário de outros tipos) podem ter acessibilidade pública ou interna declarada e o padrão para a acessibilidade declarada interna.
- Membros de classe podem ter qualquer um dos cinco tipos de acessibilidade declarada e por padrão para a acessibilidade declarada privada. [Observação: um tipo declarado como membro de uma classe pode ter qualquer um dos cinco tipos de acessibilidade declarada, enquanto um tipo declarado como um membro de um namespace pode ter apenas acessibilidade pública ou interna declarada. Nota de término]
- Os membros de struct podem ter acessibilidade definida pública, interna ou privada e, por padrão, a acessibilidade declarada privada, pois as estruturas são seladas implicitamente. Membros de struct introduzidos em uma struct (ou seja, não herdados por essa struct) não podem ter acessibilidade protegida *,* ~~ou~~ interna protegida **, ou protegida privada** . [Observação: um tipo declarado como membro de uma struct pode ter acessibilidade pública, interna ou privada declarada, enquanto um tipo declarado como membro de um namespace pode ter apenas acessibilidade pública ou interna declarada. Nota de término]
- Os membros da interface implicitamente têm acessibilidade declarada pública. Nenhum modificador de acesso é permitido em declarações de membro de interface.
- Os membros de enumeração têm implicitamente a acessibilidade declarada. Nenhum modificador de acesso é permitido em declarações de membro de enumeração.

-----

> O domínio de acessibilidade de um membro aninhado M declarado em um tipo T dentro de um programa P, é definido da seguinte maneira (observando que a própria M pode ser um tipo):
- Se a acessibilidade declarada de M for pública, o domínio de acessibilidade de M é o domínio de acessibilidade de T.
- Se a acessibilidade declarada de M for protegida internamente, deixe D ser a União do texto do programa de P e o texto do programa de qualquer tipo derivado de T, que é declarado fora do P. O domínio de acessibilidade de M é a interseção do domínio de acessibilidade de T com D.
- **Se a acessibilidade declarada de M for privada protegida, deixe D ser a interseção do texto do programa de P e o texto do programa de qualquer tipo derivado de T. O domínio de acessibilidade de M é a interseção do domínio de acessibilidade de T com D.**
- Se a acessibilidade declarada de M estiver protegida, deixe D ser a União do texto do programa T e o texto do programa de qualquer tipo derivado de T. O domínio de acessibilidade de M é a interseção do domínio de acessibilidade de T com D.
- Se a acessibilidade declarada de M for interna, o domínio de acessibilidade de M é a interseção do domínio de acessibilidade de T com o texto do programa de P.
- Se a acessibilidade declarada de M for privada, o domínio de acessibilidade de M é o texto do programa de T.

-----

> Quando um membro de instância protegida protegida **ou privada** é acessado fora do texto do programa da classe na qual ele é declarado e quando um membro da instância interna protegida é acessado fora do texto do programa do programa no qual ele é declarado, o acesso deve ocorrer dentro de uma declaração de classe que deriva da classe na qual ela é declarada. Além disso, o acesso é necessário para ocorrer por meio de uma instância desse tipo de classe derivada ou um tipo de classe construído a partir dele. Essa restrição impede que uma classe derivada acesse membros protegidos de outras classes derivadas, mesmo quando os membros são herdados da mesma classe base.

-----

> Os modificadores de acesso permitidos e o acesso padrão para uma declaração de tipo dependem do contexto no qual a declaração ocorre (§ 9.5.2):
- Tipos declarados em unidades de compilação ou namespaces podem ter acesso público ou interno. O padrão é acesso interno.
- Os tipos declarados em classes podem ter acesso público, interno **protegido, protegido**, interno ou privado. O padrão é acesso privado.
- Os tipos declarados em structs podem ter acesso público, interno ou privado. O padrão é acesso privado.

-----

> Uma declaração de classe estática está sujeita às seguintes restrições:
- Uma classe estática não deve incluir um modificador lacrado ou abstrato. (No entanto, como uma classe estática não pode ser instanciada ou derivada de, ela se comporta como se fosse selada e abstrata.)
- Uma classe estática não deve incluir uma especificação de base de classe (§ 16.2.5) e não pode especificar explicitamente uma classe base ou uma lista de interfaces implementadas. Uma classe estática herda implicitamente do tipo Object.
- Uma classe estática deve conter apenas membros estáticos (§ 16.4.8). [Observação: todas as constantes e tipos aninhados são classificados como membros estáticos. Nota de término]
- Uma classe estática não deve ter membros com acessibilidade protegida **, privada protegida** ou protegida interna declarada.

> É um erro de tempo de compilação para violar qualquer uma dessas restrições. 

-----

> Uma declaração de membro de classe pode ter um dos ~~cinco~~**seis** tipos possíveis de acessibilidade declarada (§ 9.5.2): pública, **privada protegida**, protegida interna, protegida, interna ou privada. Exceto para**as combinações**protegidas interna **e privada** protegida, trata-se de um erro de tempo de compilação para especificar mais de um modificador de acesso. Quando uma declaração de membro de classe não inclui nenhum modificador de acesso, a particular é assumida.

-----

> Os tipos não aninhados podem ter acessibilidade declarada pública ou interna e ter uma acessibilidade declarada interna por padrão. Os tipos aninhados também podem ter esses formulários de acessibilidade declarados, além de uma ou mais formas adicionais de acessibilidade declarada, dependendo se o tipo recipiente é uma classe ou estrutura:
- Um tipo aninhado declarado em uma classe pode ter uma das ~~cinco~~**seis** formas de acessibilidade declaradas (pública, **privada protegida**, protegida interna, protegida, interna ou privada) e, como outros membros da classe, usa como padrão a acessibilidade declarada privada.
- Um tipo aninhado declarado em um struct pode ter qualquer uma das três formas de acessibilidade declarada (pública, interna ou privada) e, como outros membros de struct, usa como padrão a acessibilidade declarada privada.

-----

> O método substituído por uma declaração de substituição é conhecido como o método base substituído para um método de substituição M declarado em uma classe C, o método base substituído é determinado examinando cada tipo de classe base de C, começando com o tipo de classe base direta de C e Continuando com cada tipo de classe base direta sucessiva, até que em um determinado tipo de classe base, pelo menos um método acessível esteja localizado, que tenha a mesma assinatura que M após a substituição dos argumentos de tipo. Para a finalidade de localizar o método base substituído, um método é considerado acessível se for público, se estiver protegido, se estiver protegido, se for **um** banco de dados interno **ou interno ou privado** , e declarado no mesmo programa que C.

-----

> O uso de acessadores-modificadores é regido pelas seguintes restrições:
- Um modificador de acessador não deve ser usado em uma interface ou em uma implementação de membro de interface explícita.
- Para uma propriedade ou um indexador que não tem um modificador de substituição, um modificador de acessador é permitido somente se a propriedade ou o indexador tiver um acessador get e set e, em seguida, for permitido somente em um desses acessadores.
- Para uma propriedade ou um indexador que inclui um modificador de substituição, um acessador deve corresponder ao modificador de acessador, se houver, do acessador que está sendo substituído.
- O modificador de acessador deve declarar uma acessibilidade que seja estritamente mais restritiva do que a acessibilidade declarada da propriedade ou do indexador em si. Para ser preciso:
  - Se a propriedade ou o indexador tiver uma acessibilidade declarada de Public, o modificador de acessador poderá ser **particular protegido**,, interno protegido, interno, protegido ou privado.
  - Se a propriedade ou o indexador tiver uma acessibilidade declarada de interna protegida, o modificador de assessor poderá ser **particular protegido**, interno, protegido ou privado.
  - Se a propriedade ou o indexador tiver uma acessibilidade declarada interna ou protegida, o modificador de acessador deverá ser particular **ou protegido** .
  - **Se a propriedade ou o indexador tiver uma acessibilidade declarada de protegida privada, o modificador de acessador deverá ser privado.**
  - Se a propriedade ou o indexador tiver uma acessibilidade declarada de particular, nenhum assessor-modificador poderá ser usado.

-----

> Como a herança não tem suporte para structs, a acessibilidade declarada de um Membro struct não pode ser protegida, **privada protegida**ou interna protegida.

-----

## <a name="drawbacks"></a>Desvantagens
[drawbacks]: #drawbacks

Como com qualquer recurso de linguagem, devemos questionar se a complexidade adicional para o idioma é repagada na maior clareza oferecida ao corpo de C# programas que se beneficiariam do recurso.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

Uma alternativa seria o provisionamento de uma API que combina um atributo e um analisador. O atributo é colocado pelo programador em um membro `internal` para indicar que o membro deve ser usado somente em subclasses, e o analisador verifica se essas restrições estão em obedecer. 

## <a name="unresolved-questions"></a>Perguntas não resolvidas
[unresolved]: #unresolved-questions

A implementação está totalmente completa. O único item de trabalho aberto é o rascunho de uma especificação correspondente para o VB.

## <a name="design-meetings"></a>Criar reuniões

TBD
