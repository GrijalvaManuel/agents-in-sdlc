---
title: "Exercício 1 - Instruções personalizadas (agente de nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Agente de nuvem do Copilot →][next-lesson]

O contexto é fundamental ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de uma maneira específica — ou se o Copilot precisa conhecer informações prévias — esse contexto deve estar acessível. Os [arquivos de instruções][instruction-files] fornecem essa orientação para que o Copilot entenda não só *o que* deve fazer, mas também *como*.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e arquivos de instruções com escopo definido por caminho;
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`.

> [!NOTE]
> Diferentemente dos percursos do VS Code e da CLI, aqui você não executará um prompt antes e depois da alteração. O agente de nuvem do Copilot trabalha de forma assíncrona em issues do GitHub, então é mais difícil demonstrar o impacto lado a lado em tempo real. Você verá a influência do arquivo de instruções mais adiante, ao revisar os pull requests produzidos pelo agente.

## Arquivos de instruções

### Cenário

Como toda boa empresa de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento. Entre eles:

- A camada de dados sempre precisa ter testes unitários.
- A interface do usuário deve usar o modo escuro e ter um visual moderno.
- A documentação do código deve ser adicionada na forma de comentários TSDoc.
- Um bloco de comentários no início de cada arquivo deve descrever sua finalidade.

Com os arquivos de instruções, você garantirá que o Copilot tenha as informações necessárias para executar tarefas de acordo com essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo de código e seus requisitos. Esse recurso ajuda a orientar as sugestões e os trechos de código. Você pode especificar convenções de código, bibliotecas e até os tipos de comentários que prefere. Crie instruções para todo o repositório ou para tipos específicos de arquivos e tarefas.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`, um único arquivo enviado ao Copilot em **todas** as solicitações para o repositório. Ele deve conter informações gerais do projeto — contexto relevante para a maioria das solicitações feitas ao Copilot Chat ou à CLI.
- Arquivos `.github/instructions/*.instructions.md`, que podem ser criados para tarefas ou tipos de arquivo específicos. Use-os para definir diretrizes para linguagens específicas, como TypeScript, ou tarefas, como criar um componente de interface ou testes unitários.

> [!NOTE]
> No IDE, os arquivos de instruções são usados para gerar código no Copilot Chat — não para conclusões de código nem sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam os arquivos de instruções do repositório e os arquivos `*.instructions.md` com frontmatter `applyTo` ao gerar código.
>
> O Copilot também [oferece suporte a arquivos de instruções em outros padrões][custom-instructions-support], como AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Uma conversa completa sobre como criar arquivos de instruções está além do escopo deste workshop. No entanto, os exemplos do projeto mostram uma abordagem representativa. Em linhas gerais:

- Mantenha o conteúdo de `copilot-instructions.md` focado em orientações gerais do projeto, como uma descrição do que está sendo criado, a estrutura do projeto e padrões de código globais.
- Use arquivos `*.instructions.md` para fornecer orientações específicas a tipos de arquivos, como testes unitários, componentes Astro e a camada de dados, ou para tarefas específicas.
- Use linguagem natural. Mantenha as orientações claras e inclua exemplos de como o código deve ou não deve ser escrito.

Não há uma única maneira certa de criar arquivos de instruções, assim como não há uma única maneira certa de usar IA. A experimentação ajudará você a descobrir o que funciona melhor para seu projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto robusto de arquivos de instruções. Ao explorar os arquivos deste projeto, você encontrará instruções para vários tipos de tarefa, incluindo [atualizações de interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada ambiente oferece esse recurso de forma diferente (por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI) — a lição do ambiente que você está usando indicará quando isso for relevante.
>
> Procura templates ou um ponto de partida? Explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explore os arquivos de instruções personalizadas do projeto

Reserve um momento para ler os arquivos de instruções incluídos neste repositório — há um arquivo principal `copilot-instructions.md` e vários arquivos `*.instructions.md` para diferentes tarefas. Abra-os no editor ou na interface web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas sobre **GitHub Actions Workflows**. Elas se aplicam a qualquer interação com o Copilot.
3. Abra a pasta `.github/instructions` e explore o conteúdo. Observe as instruções para arquivos Astro, a camada de dados Drizzle, testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início — ele define um glob relativo à raiz do repositório que determina a quais arquivos as instruções se aplicam. Neste caso, qualquer arquivo de teste TypeScript que corresponda, por exemplo, a `**/*.test.ts` será incluído.
5. Observe as instruções específicas para criar testes unitários neste projeto.
6. Por fim, abra `.github/instructions/drizzle.instructions.md` e role até o fim. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes no projeto. Isso permite dividir conjuntos maiores de instruções em arquivos menores e reutilizáveis e apontar o Copilot para exemplos. (Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.)

> [!NOTE]
> A seção **Code formatting requirements** de `copilot-instructions.md` documenta os padrões de código do projeto, mas ainda não exige documentação dentro do código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentários nos arquivos.

## Adicione um novo padrão para o repositório

Esta é a única edição que você fará neste exercício: adicionar uma regra geral ao projeto para que a documentação fique no código, em comentários TSDoc e em um cabeçalho de comentário no início dos arquivos. O agente de nuvem aplicará essa orientação aos issues que você atribuir a ele mais adiante.

Antes de editar, crie uma branch de trabalho. O agente de nuvem lê as instruções da branch de destino do issue, mas você fará o commit das alterações nesta branch:

1. No terminal do codespace, crie e alterne para uma nova branch:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Conforme explicado anteriormente, `.github/copilot-instructions.md` fornece informações gerais do projeto ao Copilot. Vamos documentar os padrões de código do repositório para melhorar as sugestões.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar próxima à linha 27. Observe que ela documenta os padrões de código do projeto, mas ainda não exige documentação no código, por isso o helper gerado não tinha comentários.
3. Adicione as linhas de Markdown a seguir logo depois dos padrões existentes para orientar o Copilot a criar cabeçalhos de comentários nos arquivos e comentários TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, os arquivos de instruções podem ser criados no nível do repositório (`.github/copilot-instructions.md`) para orientações globais ou como arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo do repositório é o lugar certo para padrões gerais, como a regra de comentários de documentação.

## Faça commit, push e merge da atualização das instruções

O agente de nuvem lê os arquivos de instruções da branch de destino do issue. Quando você atribuir um issue ao Copilot no próximo exercício, ele criará uma branch a partir de `main` — portanto, as alterações nas instruções precisam estar em `main` para que o agente de nuvem possa usá-las.

1. Prepare e faça commit:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Publique a branch:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. Abra um pull request de `custom-instructions-cloud` para `main` em github.com e faça o merge. Assim, o agente de nuvem poderá ler as instruções ao trabalhar nos issues atribuídos no próximo exercício.

> [!TIP]
> Se preferir trabalhar diretamente em `main` neste workshop, você pode ignorar as etapas da branch e fazer commit diretamente em `main`. A etapa da branch foi incluída para que o workshop reflita como você trabalharia em um projeto real.

## Resumo e próximos passos

Você explorou como o Copilot usa o contexto dos arquivos de instruções do projeto e adicionou um novo padrão para todo o repositório em `.github/copilot-instructions.md`. Você verá esse padrão aplicado aos pull requests produzidos pelo agente de nuvem ao longo deste percurso.

Em seguida, [atribua seu primeiro issue ao agente de nuvem do Copilot][next-lesson].

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot — uma coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
