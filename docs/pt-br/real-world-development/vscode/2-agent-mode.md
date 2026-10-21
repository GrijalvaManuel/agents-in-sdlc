---
title: "Exercício 2 - Adicionar funcionalidade com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até mesmo atualizações simples de um aplicativo costumam exigir alterações em vários arquivos e operações como executar testes. O fluxo de trabalho de uma pessoa desenvolvedora normalmente envolve localizar todos os arquivos necessários, fazer as alterações, executar os testes, depurar, descobrir qual arquivo ficou faltando, fazer outra atualização... e assim por diante.

É nesse ponto que entra o modo de agente do Copilot.

O modo de agente do Copilot foi criado para trabalhar de forma mais autônoma no IDE. Ele se comporta de maneira semelhante a uma pessoa desenvolvedora: começa explorando a estrutura do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Vamos explorar como usar o modo de agente para adicionar funcionalidades ao site.

> [!NOTE]
> Embora os nomes sejam parecidos, o modo de agente e o agente de nuvem oferecem experiências diferentes. O modo de agente executa tarefas no IDE, permitindo ciclos rápidos de feedback e interação. O agente de nuvem foi criado para atuar como um par de programação que trabalha de forma assíncrona, como integrante da equipe, e interage com você por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o modo de agente do Copilot explora o projeto, identifica arquivos relevantes e faz alterações coordenadas;
- o modo de agente do GitHub Copilot implementa recursos na interface e na camada de dados;
- revisar alterações e testes gerados pelo modo de agente antes de integrá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que as pessoas filtrem por categoria e editora. No exercício anterior, você já adicionou um helper de editoras. Agora, concluirá com o modo de agente do Copilot o trabalho restante na camada de dados, na interface do usuário e nos testes.

## Executar o site da Tailspin Toys

Antes de fazer qualquer alteração, vamos explorar o site da Tailspin Toys para conhecer sua funcionalidade atual.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com temática de desenvolvimento. Ele permite listar jogos e exibir detalhes. O site é um único aplicativo Astro que renderiza as páginas como HTML estático durante a compilação. As páginas consultam diretamente um banco de dados SQLite local no frontmatter, usando o Drizzle ORM — não há uma API de back-end separada nem framework de interface executado no cliente. Os helpers reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é adicionada com um `<script>` Astro pequeno e com escopo definido, usando as APIs padrão do DOM.

### Iniciar o site

Para facilitar a execução do site, foi disponibilizada uma tarefa `npm run dev`, que inicia o servidor de desenvolvimento Astro. Execute-a no GitHub Codespace seguindo estas etapas:

1. Volte ao codespace. Você deverá estar na branch `main` após o exercício anterior.
2. Abra um novo terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie e alterne para uma nova branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Execute o seguinte comando para iniciar o site:

   ```bash
   npm run dev
   ```

   Quando o servidor de desenvolvimento Astro estiver pronto, você verá uma mensagem com a URL, semelhante a esta:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se aparecer uma caixa de diálogo perguntando se deseja abrir uma janela do navegador para `http://localhost:4321`, feche-a selecionando **x**.

5. Abra o site mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> Em um codespace, selecionar um link para a URL localhost no terminal do Codespace redirecionará automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para o codespace, que agora está hospedando o servidor web.

### Explore o site

Com o site em execução, explore sua funcionalidade. Os principais recursos incluem:

- **Página inicial**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você acessa uma página de detalhes com mais informações, incluindo título, descrição, editora e categoria.

## Explore o backlog com o Copilot

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa nova. O modo e a seleção do modelo variam em cada exercício — cada etapa indicará as opções pertinentes.

A implementação inicial do site funciona, mas queremos aprimorá-la com novos recursos. Vamos começar revisando o backlog. Ao configurar o projeto a partir do template, um backlog de issues foi criado automaticamente — peça ao GitHub Copilot que mostre os itens.

1. Selecione **Agent** na lista de agentes da visualização Chat. O agente **Agent** planeja e implementa alterações de forma autônoma em vários arquivos, executa comandos de terminal e chama ferramentas.

   ![Captura de tela da lista de agentes na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> Os autores deste workshop não estão indicando preferência por um modelo. Ao preparar o workshop, usamos o Claude Sonnet 4.5 e o incluímos nas instruções na esperança de que as sugestões de código sejam relativamente consistentes e proporcionem uma boa experiência. No entanto, como os LLMs são probabilísticos, as sugestões recebidas podem ser diferentes das indicadas no workshop. Isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot pode usar um comando MCP diferente e ainda assim concluir a tarefa.

3. Envie este prompt ao Copilot para perguntar sobre o backlog de issues:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para executar o comando que lista todos os issues.
5. Revise a lista de issues gerada.

Observe que o Copilot também priorizou os itens de acordo com o que considera mais útil para os usuários.

## Revise os arquivos de instruções

Antes de pedir ao agente que gere código, revise o arquivo de instruções que fornecerá contexto ao Copilot. Você usará o arquivo de instruções de [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica como abordar a adição de funcionalidades ao site.

1. No codespace, abra `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais sobre como adicionar funcionalidades. Elas incluem:
   - uma visão geral da arquitetura;
   - princípios de projeto de componentes, capacidade de teste e acessibilidade;
   - links para arquivos de instruções específicos de tipos de arquivo, incluindo:
     - Astro;
     - Tailwind CSS;
     - Drizzle.

> [!TIP]
> Os arquivos de instruções podem fazer referência a outros arquivos de instruções e a arquivos do projeto. Os caminhos são relativos ao arquivo de instruções. Assim, é possível reutilizar conteúdo, dividir conjuntos maiores de instruções em partes menores e mais fáceis de gerenciar e apontar para exemplos e templates.

## Implemente a funcionalidade de filtragem

Para concluir a filtragem, serão necessárias pelo menos três atualizações separadas no aplicativo:

- adicionar ou aprimorar a lógica de filtragem restante na camada de dados (`src/lib/`);
- adicionar ou atualizar testes para o comportamento da filtragem;
- atualizar a página de listagem de jogos para incluir a interface de filtragem.

Além disso, os testes precisam ser executados e aprovados antes do merge na base de código. O modo de agente do Copilot pode realizar essas tarefas. Vamos adicionar a funcionalidade.

1. Continue na conversa atual com o Copilot ou selecione **New Chat** para iniciar outra.
2. Selecione **Add Context**, **Instructions** e **ui** como arquivo de instruções.

   ![Captura de tela que mostra um exemplo de seleção do arquivo de instruções da interface.](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** continua selecionado na lista de agentes da visualização Chat.

   ![Captura de tela da lista de agentes na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirme que **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente a funcionalidade com base no issue relacionado do backlog usando este prompt:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Acompanhe o Copilot enquanto ele explora o projeto e localiza os arquivos relacionados à funcionalidade. Você deverá vê-lo encontrar os helpers da camada de dados, a interface e os testes. Em seguida, ele começará a modificar os arquivos e executar os testes.

   ![Captura de tela do Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, modificar arquivos e executar testes. O processo pode levar alguns minutos, dependendo da complexidade da tarefa e da base de código. Enquanto ele trabalha, talvez apareçam os botões **Keep** e **Undo** no editor. Quando o Copilot terminar, você verá os botões **Keep** ou **Undo** para todas as alterações. Portanto, não é necessário selecioná-los enquanto o trabalho estiver em andamento.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Captura de tela de uma caixa de diálogo no painel Copilot Chat solicitando confirmação para executar os testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Talvez haja pausas e alguns testes falhem durante o processo. Tudo bem! O Copilot alterna entre a geração de código e a execução de testes até concluir a tarefa sem detectar erros.

   ![Captura de tela de uma sessão completa do Copilot Chat no modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado para procurar possíveis problemas.

> [!CAUTION]
> É sempre importante revisar o código gerado pelo Copilot ou por qualquer outra ferramenta de IA.

10. Volte ao navegador com o site em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Parabéns! Neste exercício, exploramos como usar o modo de agente do GitHub Copilot para adicionar recursos ao site da Tailspin Toys. Aprendemos como:

- o modo de agente do GitHub Copilot implementa recursos na interface e na camada de dados;
- o modo de agente do Copilot explora o projeto, identifica arquivos relevantes e faz alterações coordenadas;
- revisar alterações e testes gerados pelo modo de agente antes de integrá-los à base de código.

Agora vamos [testar o recurso com o servidor MCP do Playwright][next-lesson] e abrir um pull request.

### Exercício extra — Implementar paginação

À medida que a lista de jogos cresce, será necessário habilitar a paginação. Com as habilidades aprendidas neste exercício, peça ao Copilot que atualize o site para implementar a paginação. Considere estes pontos:

- siga as práticas recomendadas existentes, inclusive o uso dos arquivos de instruções;
- decida como a paginação funcionará: se a pessoa usuária poderá selecionar o tamanho da página ou se ele será fixo;
- ao criar o prompt, forneça ao Copilot as orientações necessárias para que a implementação atenda às suas expectativas;
- talvez seja preciso iterar com o GitHub Copilot, pedir alterações e fornecer contexto. Esse é o fluxo normal de trabalho com o Copilot.

## Recursos

- [Copilot Ask, Edit e Agent: o que fazem e quando usar cada modo][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
