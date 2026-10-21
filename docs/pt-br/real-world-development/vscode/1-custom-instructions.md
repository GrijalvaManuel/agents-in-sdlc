---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo de agente →][next-lesson]

O contexto é fundamental ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de uma maneira específica — ou se o Copilot precisa conhecer informações prévias — esse contexto deve estar acessível. Há várias maneiras de compartilhar contexto específico com o Copilot. Uma das principais são os [arquivos de instruções][instruction-files], usados para fornecer orientações sobre a geração de código.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e arquivos de instruções com escopo definido por caminho;
- gerar a primeira fatia de dados para a filtragem (um helper de editoras) com as instruções *atuais*;
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`;
- enviar o mesmo prompt novamente e observar o código gerado adotar o novo padrão;
- fazer commit das instruções e da fatia de filtragem na branch `main` para que o Copilot possa usar as orientações atualizadas no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir alguns dos padrões definidos. O Copilot é não determinístico. O objetivo deste exercício é observar a *tendência* de mudança no comportamento depois de atualizar as instruções, não obter exatamente o mesmo código do exemplo.

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

- `.github/copilot-instructions.md`, um único arquivo enviado ao Copilot em **todas** as solicitações para o repositório. Ele deve conter informações gerais do projeto — contexto relevante para a maioria das solicitações feitas ao Copilot Chat ou à CLI. Isso pode incluir a stack, uma visão geral do que está sendo criado, práticas recomendadas e outras orientações globais.
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

## Explore os arquivos de instruções personalizadas deste projeto

Reserve um momento para ler os arquivos de instruções incluídos neste repositório — há um arquivo principal `copilot-instructions.md` e vários arquivos `*.instructions.md` para diferentes tarefas. Abra-os no editor ou na interface web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas sobre **GitHub Actions Workflows**. Elas se aplicam a qualquer interação com o Copilot.
3. Abra a pasta `.github/instructions` e explore o conteúdo. Observe as instruções para arquivos Astro, a camada de dados Drizzle, testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início — ele define um glob relativo à raiz do repositório que determina a quais arquivos as instruções se aplicam. Neste caso, qualquer arquivo de teste TypeScript que corresponda, por exemplo, a `**/*.test.ts` será incluído.
5. Observe as instruções específicas para criar testes unitários neste projeto.
6. Por fim, abra `.github/instructions/drizzle.instructions.md` e role até o fim. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes no projeto. Isso permite dividir conjuntos maiores de instruções em arquivos menores e reutilizáveis e apontar o Copilot para exemplos. (Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.)

> [!NOTE]
> A seção **Code formatting requirements** de `copilot-instructions.md` documenta os padrões de código do projeto, mas ainda não exige documentação dentro do código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentários nos arquivos.

## Crie uma branch para as alterações

Vamos seguir algumas práticas recomendadas e criar uma branch para as alterações.

1. Volte ao codespace do exercício anterior.
2. Abra um novo terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie uma nova branch e alterne para ela:

   ```bash
   git checkout -b custom-instructions
   ```

## Use o Copilot Chat *antes* de atualizar as instruções

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa nova. O modo e a seleção do modelo variam em cada exercício — cada etapa indicará as opções pertinentes.

Para observar o impacto das instruções personalizadas, comece enviando um prompt com o arquivo de instruções atual. Depois, você o atualizará e enviará o mesmo prompt novamente para ver a diferença.

1. Feche todas as guias do editor abertas nos exercícios anteriores para que o Copilot use somente o contexto desejado.
2. Abra `src/lib/publishers.ts` para indicar ao Copilot onde o helper deve ficar.
3. Selecione **Agent** na lista de agentes da visualização Chat para permitir que o Copilot aplique alterações aos arquivos.

   ![Captura de tela da lista de agentes na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envie este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. O Copilot explorará o projeto e aplicará as atualizações de código, muitas vezes abrangendo o arquivo helper e seus testes.
6. Observe que o helper proposto é uma função tipada que recebe um cliente `db` como primeiro argumento e retorna um array tipado de editoras — isso vem das convenções da camada de dados em `.github/instructions/drizzle.instructions.md` (aplicável a `src/lib/*.ts`).
7. Observe que o código proposto **não tem** comentários TSDoc nem um cabeçalho de comentários no início do arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, ele talvez adicione comentários de documentação mesmo sem ser solicitado. Não há problema se isso acontecer — o objetivo é observar a melhora na *consistência* após a atualização das instruções.

## Adicione um novo padrão ao repositório

Como destacado anteriormente, `.github/copilot-instructions.md` fornece informações gerais do projeto ao Copilot. Vamos documentar os padrões de código do repositório para melhorar as sugestões.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar próxima à linha 27. Observe como ela documenta os padrões de código do projeto, mas ainda não tem uma regra para documentação no código — por isso o helper gerado não tinha comentários.
3. Adicione as linhas de Markdown a seguir logo depois dos padrões existentes para orientar o Copilot a criar cabeçalhos de comentários nos arquivos e comentários TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, os arquivos de instruções podem ser criados no nível do repositório (`.github/copilot-instructions.md`) para orientações globais ou como arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo do repositório é o lugar certo para padrões gerais, como a regra de comentários de documentação.

## Envie o prompt novamente e observe a mudança

1. Volte ao Copilot Chat e selecione **New Chat** para limpar o histórico.
2. Selecione `src/lib/publishers.ts` para que o Copilot se concentre no arquivo correto.
3. Envie **o mesmo prompt** de antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observe que o arquivo proposto agora começa com um bloco de comentários semelhante a:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observe que a função proposta agora inclui um comentário TSDoc semelhante a:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Você acabou de orientar o Copilot a seguir um novo padrão do projeto e aplicá-lo a código real, que servirá de base para o próximo exercício.

## Faça commit das instruções e publique a branch

Os arquivos de instruções são recursos do repositório, assim como qualquer outro arquivo, e são gerenciados pelo mesmo processo de controle do código-fonte. Vamos fazer commit das alterações e enviar a branch ao repositório.

1. Abra uma nova janela do terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. No terminal, confirme se as atualizações das instruções e do helper estão presentes executando:

   ```bash
   git status
   ```

3. Prepare e faça commit da atualização das instruções e de todos os arquivos alterados pelo Copilot para o helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Publique a branch no repositório:

   ```bash
   git push -u origin custom-instructions
   ```

## Crie e faça merge de um pull request

Com a branch publicada, crie um pull request e associe-o ao issue do backlog sobre os padrões de documentação. Você poderia fazer isso manualmente, mas o Copilot pode ajudar usando as ferramentas do GitHub já conectadas ao projeto. Vamos pedir que ele encontre o issue, crie um PR que o feche e faça o merge.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot executa ações no GitHub. O template do projeto já configura as ferramentas do GitHub no seu espaço de trabalho, então não é preciso configurar nada. Na primeira vez que o Copilot usar uma delas, o VS Code talvez peça para você entrar no GitHub. Siga as instruções para permitir o acesso. Em um exercício posterior, você aprenderá como funciona essa conexão (Model Context Protocol).

1. Abra o Copilot Chat no codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a visualização Copilot Chat e confirme que **Agent** está selecionado na lista de agentes.
3. Peça ao Copilot que encontre o issue sobre a atualização dos arquivos de instruções e crie um PR a partir da branch atual, descrevendo as atualizações das instruções e a nova base do helper de editoras:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. O Copilot começará a procurar o issue e criar o PR.
5. Quando solicitado a permitir que o Copilot execute ações do GitHub, revise o comando e selecione **Allow** conforme apropriado.
6. Depois da criação do PR, peça ao Copilot que faça o merge e retorne sua branch para `main` usando este prompt:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Quando solicitado a permitir que o Copilot execute ações do GitHub e comandos de shell, revise o comando e selecione **Allow** conforme apropriado.

Agora você criou e integrou um pull request com a ajuda do GitHub Copilot.

## Resumo e próximos passos

Você explorou como o Copilot usa o contexto dos arquivos de instruções do projeto e usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar o resultado com as instruções *existentes*;
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`;
- enviar o mesmo prompt novamente e observar o código proposto adotar o novo padrão;
- fazer commit das instruções e da base do helper na branch `main` para que a próxima lição possa usá-las.

Agora, vamos aplicar essas instruções ao [modo de agente][next-lesson], enquanto o Copilot adiciona um novo recurso à base de código.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções personalizadas pessoais para o GitHub Copilot][personal-instructions]
- [Awesome Copilot — uma coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
