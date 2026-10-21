---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que quase todas as organizações enfrentem dificuldades com a dívida técnica. Ela pode incluir problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de recursos que ficaram no backlog por falta de tempo para implementá-las. O agente de nuvem do GitHub Copilot foi criado para realizar tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina o trabalho, ele cria um pull request em rascunho para que uma pessoa desenvolvedora o revise. Isso permite delegar tarefas repetitivas, acelerar o desenvolvimento e liberar a equipe para se concentrar em questões mais amplas.

Neste exercício, você vai explorar como:

- personalizar o ambiente de geração de código;
- garantir a segurança das operações;
- criar issues com escopo claro;
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem dívidas técnicas que gostaria de resolver. As pessoas contratadas inicialmente para criar a primeira versão do site deixaram a documentação em um estado inadequado — na verdade, ela está totalmente ausente. Como primeiro passo, a equipe quer adicionar comentários TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design está pronta para melhorar a descoberta de jogos. Ela quer que a página de detalhes de cada jogo mostre jogos relacionados — outros títulos da mesma categoria — para que as pessoas possam continuar explorando. Ainda não é necessária uma implementação refinada; a equipe quer apenas algo que permita testar a experiência do usuário. Essa tarefa está bloqueando o trabalho, mas há outros issues com prioridade maior no momento.

Esses são exemplos de tarefas que podem ser despriorizadas facilmente e que são ótimas opções para atribuir ao agente de nuvem do Copilot. O agente trabalha nelas de forma assíncrona, permitindo que a pessoa desenvolvedora se concentre em outras tarefas e depois volte para revisar o trabalho do Copilot.

## Apresentação do agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) pode executar tarefas em segundo plano, de forma semelhante a uma pessoa desenvolvedora. Assim como no trabalho com uma pessoa desenvolvedora, há várias maneiras de fazer isso, incluindo [atribuir um issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois que você atribui o issue, o Copilot cria um pull request em rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode examinar a sessão do Copilot enquanto ela está em andamento ou depois de concluída. Quando a solução proposta estiver pronta para revisão, ele marcará você no pull request.

## A importância de instruções com escopo bem definido

Não há mágica no GitHub Copilot — você não pode deixar de pensar. Até operações aparentemente simples podem se tornar complexas quando examinadas em detalhe. Por isso, [defina com cuidado o escopo das tarefas para o agente de nuvem do Copilot][cloud-agent-best-practices]. Trate-o como um par de programação com IA: trabalhe em etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a chegada da IA generativa.

## Instruções personalizadas neste repositório

Nos exercícios anteriores, você conheceu as instruções personalizadas e como elas orientam o Copilot. Se já fez um exercício prático sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação; caso contrário, aproveite este momento para examiná-lo. Antes de atribuir trabalho ao agente de nuvem do Copilot, leia os arquivos de instruções incluídos neste repositório sem fazer alterações para reconhecer seus efeitos mais adiante.

Abra os arquivos a seguir na interface web do GitHub para o seu repositório ou em um codespace, caso já tenha um iniciado:

- `.github/copilot-instructions.md` — revise a seção **Code standards**, especialmente as expectativas sobre convenções TypeScript e comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md` — observe o frontmatter `applyTo`, que limita essas instruções a arquivos `**/*.test.ts`.

Quando você atribuir o issue `Code lacks documentation` ao agente de nuvem na próxima seção, observe os comentários TSDoc, as convenções TypeScript e os cabeçalhos de comentários no pull request resultante. Esses elementos vêm dos arquivos de instruções que você examinou. Voltaremos a esse tópico ao revisar os PRs em uma lição posterior.

## Configure o ambiente de desenvolvimento do agente de nuvem do Copilot

Normalmente, criar código exige um ambiente específico e a execução de scripts de configuração para garantir que tudo esteja pronto. Isso também vale ao atribuir tarefas ao Copilot, que trabalha de forma semelhante a uma pessoa engenheira de software.

O agente de nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizar esse ambiente criando um [fluxo de trabalho de configuração especial][setup-workflow], definido no arquivo `.github/workflows/copilot-setup-steps.yml`, para ser executado antes de o agente começar. Isso permite disponibilizar as ferramentas de desenvolvimento e dependências necessárias. Esse fluxo de trabalho já está configurado para facilitar o laboratório e a experiência de aprendizagem. Ele garante que o Copilot tenha acesso ao Node.js, às dependências do projeto, aos binários do navegador para testes de ponta a ponta e ao banco de dados SQLite migrado e populado do aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mínimas necessárias para *suas etapas*. O Copilot receberá um token próprio para as operações dele.
    permissions:
      # Para clonar o repositório durante as etapas de configuração, por exemplo, para instalar dependências, você precisará da permissão `contents: read`. Se não clonar o repositório nessas etapas, o Copilot fará isso automaticamente quando elas terminarem.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do front-end/aplicativo: Node.js (o aplicativo inteiro agora usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra e popula o banco de dados SQLite local para que compilações e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

Esse arquivo parece qualquer outro arquivo de fluxo de trabalho do GitHub, mas tem alguns pontos importantes:

- Ele contém um único job chamado `copilot-setup-steps`. Esse job é executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- Observe o gatilho `workflow_dispatch`, que permite executar o fluxo de trabalho manualmente na guia **Actions** do repositório. Isso é útil para testar se ele funciona corretamente, sem precisar esperar o Copilot executá-lo.

## Adicione a documentação

Embora todos reconheçam a importância da documentação, a maioria dos projetos tem informações desatualizadas ou não tem documentação. Esse tipo de dívida técnica costuma ficar sem solução, reduzindo a produtividade e dificultando a manutenção da base de código e a integração de novas pessoas à equipe. Felizmente, o Copilot é muito bom em criar documentação, e este é um issue ideal para atribuir ao agente de nuvem. Ele trabalhará em segundo plano para gerar a documentação necessária. Em um exercício futuro, você voltará para revisar o resultado.

1. Em uma nova guia do navegador, acesse seu repositório em github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de novo issue.
4. Selecione **Blank issue** para criar o issue.
5. No campo **Title**, insira `Code lacks documentation`.
6. No campo **Description**, insira:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecione **Create** para criar o issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuição de um issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

  ![Detalhes da atribuição ao Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecione a guia **Pull Requests**.
11. Abra o pull request (PR) recém-criado, cujo título será parecido com `[WIP]: Code lacks documentation`. Se o novo PR ainda não aparecer na lista, aguarde um ou dois minutos e atualize a página.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas do Copilot pode levar vários minutos para aparecer no PR. O Copilot está criando o ambiente (executando o fluxo de trabalho descrito anteriormente), analisando o projeto e determinando a melhor abordagem para resolver o problema.

13. Revise a lista e as tarefas que serão realizadas.
14. Role a linha do tempo do pull request. Você deverá ver uma atualização informando que o Copilot começou a trabalhar no issue.
15. Selecione o botão **View session**.

  ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Observe que você pode percorrer a sessão em tempo real e acompanhar como o Copilot resolve o problema. Ele explora o código e entende o estado atual, pausa para pensar e decidir a abordagem adequada e cria o código.

O trabalho provavelmente levará alguns minutos. Um dos principais objetivos do agente de nuvem do Copilot é permitir a execução assíncrona de tarefas, liberando você para se concentrar em outras atividades. Vamos aproveitar esse recurso: enquanto o Copilot trabalha em outro issue, você começará a desenvolver um recurso para o aplicativo.

## Adicione uma seção de jogos relacionados à página de detalhes

Uma das grandes vantagens do agente de nuvem do GitHub Copilot é poder dividir o trabalho, enquanto você se concentra em um conjunto de tarefas e ele em outro. Mesmo que a seção de jogos relacionados solicitada pela equipe de design não demore muito, esse tempo pode ser usado em outras atividades. Vamos atribuí-la ao agente de nuvem do Copilot.

1. Volte ao repositório em github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de novo issue.
4. Selecione **Blank issue** para usar o template em branco.
5. No campo **Title**, insira `Show related games on the game details page`.
6. No campo **Description**, insira:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecione **Create** para criar o issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuição de um issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você verá um emoji 👀 no primeiro comentário do issue, indicando que o Copilot está trabalhando.

![O Copilot usa o emoji de olhos para indicar que está trabalhando no issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot já está trabalhando no segundo pedido. O agente de nuvem trabalha de forma semelhante a uma pessoa engenheira de software: você não precisa acompanhá-lo ativamente e pode revisar o resultado quando ele terminar. Agora, vamos criar e usar agentes personalizados.

## Resumo e próximos passos

Nesta lição, você conheceu o [agente de nuvem do GitHub Copilot][copilot-agents]. Com ele, você pode atribuir issues ao Copilot para que sejam resolvidos de forma assíncrona. Use o Copilot para lidar com dívida técnica, criar recursos ou ajudar a migrar código de um framework para outro.

Você explorou como:

- personalizar o ambiente para gerar código;
- garantir a segurança das operações;
- criar issues com escopo claro;
- atribuir issues ao Copilot.

Enquanto o agente de nuvem trabalha em segundo plano, vamos passar à criação e ao uso de agentes personalizados. [O agente de nuvem do Copilot também pode usar servidores MCP][cloud-agent-mcp] e arquivos de instruções personalizadas, que você conheceu em módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de trabalho de configuração do agente de nuvem][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
