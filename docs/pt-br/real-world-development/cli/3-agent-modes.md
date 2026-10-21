---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Use o Plan para definir uma abordagem, o Autopilot para criar a filtragem a partir de um issue e o modo Interactive para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Começamos adicionando um pequeno recurso ao projeto, mas alterações maiores exigem um processo mais robusto. Felizmente, o GitHub Copilot CLI foi criado para trabalhar com o fluxo existente da organização e ajudar a criar os recursos certos da maneira certa. Esta é a primeira de várias lições em que você seguirá um processo comum de desenvolvimento com agentes: começará por um issue para gerar um recurso, confirmará que o código é válido e que o recurso funciona como esperado e, por fim, fará o merge no projeto.

> [!NOTE]
> Você usará a mesma conversa e branch durante o fluxo de trabalho do recurso. Em geral, seriam usadas branches ou PRs diferentes para tipos de arquivo distintos, mas neste workshop faremos um atalho para nos concentrarmos nos conceitos principais.

Nesta lição, você vai:

- iniciar uma nova conversa no Copilot a partir de um issue do GitHub;
- definir requisitos no modo Plan;
- implementar o novo recurso no modo Autopilot;
- revisar o código;
- validar manualmente o recurso no navegador usando o encaminhamento de porta do Codespaces.

Ao longo desse trabalho, você atualizará as instruções do repositório, personalizará a skill existente `quality-checks`, adicionará validação por MCP, criará um agente de QA e abrirá o PR do recurso.

## Cenário

O catálogo da Tailspin Toys está crescendo, e visitantes precisam filtrar jogos por categoria e editora. O issue do backlog descreve o recurso, mas alguns detalhes — como a combinação de categorias — precisam ser acordados antes de começar a programar. Você usará o modo Plan para resolver essas decisões e, depois, autorizará uma implementação com escopo definido usando o Autopilot.

## Contexto

A introdução de agentes de programação com IA ao fluxo de desenvolvimento não muda os fundamentos. Na verdade, eles ficam ainda mais importantes. A maioria das pessoas desenvolvedoras segue um fluxo semelhante a este:

1. Começa por um issue registrado que descreve o que precisa ser feito.
2. Cria um plano do que será desenvolvido.
3. Implementa e revisa o código.
4. Executa os testes para validar o código.
5. Valida manualmente a nova funcionalidade.
6. Cria um pull request (PR).
7. Depois que o código é revisado e a integração contínua é aprovada, faz o merge.

> [!NOTE]
> Os detalhes exatos variam de acordo com a equipe e a organização, mas a maioria segue alguma versão desse processo.

Seguir essa abordagem padrão ajuda a garantir que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de revisão aplicado ao código escrito manualmente.

## Modos de conversa

O **modo de conversa** controla o nível de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre os modos:

- **Interactive**: você e o agente trabalham juntos. O agente sugere alterações e aguarda sua participação antes de prosseguir.
- **Plan**: o agente cria um plano primeiro e não pode editar arquivos do projeto.
- **Autopilot**: o agente trabalha de forma autônoma — escreve código, executa testes e itera até concluir a tarefa.

Comece no modo Plan, revise o plano e, em seguida, use o Autopilot para implementá-lo.

## Comece pelo issue

Antes de iniciar o trabalho de filtragem, volte ao Codespace e confirme se o repositório e o terminal estão prontos.

1. Volte ao Codespace. Se estiver parado, reinicie-o antes de continuar.
2. Confirme que o PR das avaliações por estrelas foi integrado.
3. Se o terminal ainda não estiver aberto, pressione <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
4. Atualize `main` e crie uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicie o Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pressione <kbd>Tab</kbd> duas vezes para abrir a guia **Issues**.
7. Pressione <kbd>A</kbd> para mostrar todos os issues.
8. Use as teclas de seta para destacar o issue **Allow users to filter games by category and publisher**.
9. Pressione <kbd>C</kbd> para adicionar o issue ao prompt e voltar à guia **Session**.

Observe que o prompt agora começa com `#7` (ou um número semelhante). O caractere `#` permite adicionar um issue ou pull request (PR) do GitHub ao contexto.

## Planeje o recurso de filtragem

O planejamento permite definir como um recurso será implementado antes de delegar o trabalho ao Copilot. É sempre recomendável reservar algum tempo para planejar tarefas complexas. Vamos alternar para o modo Plan e pedir ao Copilot que crie o plano.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar para o modo Plan. Confirme que o indicador abaixo do prompt mostra **Plan**.
2. Depois da referência ao issue adicionada na etapa anterior, insira o prompt:

   ```plaintext
   Create a plan for implementing this feature.
   ```

O Copilot começará a criar o plano. Primeiro, explorará o projeto e, em seguida, definirá a abordagem mais adequada.

3. Durante o processo, o Copilot poderá perguntar como a filtragem deve funcionar. Responda de acordo com suas preferências. Não há respostas erradas.
4. Quando o plano estiver concluído, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Percorra o plano e revise-o.
6. Peça ao Copilot que ajuste qualquer parte que não corresponda às decisões tomadas.

## Aprove o Autopilot

Com o plano criado e revisado, chegou a hora de implementá-lo. Vamos deixar o Copilot trabalhar com o Autopilot.

O Autopilot permite que o Copilot itere sobre o problema até considerar que concluiu a tarefa.

1. Selecione **Accept plan and build on autopilot (recommended)** ou uma opção com nome semelhante na versão instalada.
2. Confirme que o indicador abaixo do prompt mostra **Autopilot**.
3. Observe o Copilot seguir o plano, gerar código e executar testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, então revise o plano antes. Se o Copilot informar que há dependências ausentes ou conflito de porta, resolva o problema de configuração antes de considerar as verificações concluídas.

## Revise e verifique a implementação

Depois que o código for gerado, revise-o antes do merge, como faria com qualquer outro código. Vamos examinar a implementação e iniciar o site para confirmar que tudo funciona.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar no modo Interactive. Confirme que o indicador não mostra mais **Plan** nem **Autopilot**.
2. Digite `/diff` e inspecione a implementação da filtragem e os testes.
3. Compare o resultado com o issue e com as decisões tomadas durante o planejamento.
4. Depois de revisar o código, pressione <kbd>Esc</kbd> para sair da visualização de diff.
5. Revise os resultados das verificações do projeto e peça ao Copilot que resolva eventuais falhas.

## Explore a nova funcionalidade

O código parece correto, mas será que funciona? Vamos iniciar o aplicativo como antes e abrir o site pela porta encaminhada do Codespaces.

1. Peça ao Copilot que inicie o aplicativo:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Teste a filtragem por categoria, a filtragem por editora e as combinações definidas no plano.
4. Confirme que a redefinição e o comportamento sem resultados correspondem ao issue e às decisões tomadas.
5. Volte ao Codespace e peça ao Copilot que pare o servidor de desenvolvimento iniciado.

## Resumo e próximos passos

Você usou diferentes modos de conversa para desenvolver e revisar um recurso. Nesta lição, você:

- iniciou uma nova conversa do Copilot a partir de um issue do GitHub;
- definiu requisitos no modo Plan;
- implementou o recurso no modo Autopilot;
- revisou o código;
- validou o recurso manualmente no navegador usando o encaminhamento de porta do Codespaces.

Em seguida, vamos explorar como o código é gerado e garantir que siga as práticas documentadas, [usando instruções personalizadas][next-lesson].

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
