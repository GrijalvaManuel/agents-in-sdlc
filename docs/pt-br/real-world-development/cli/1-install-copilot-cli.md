---
title: "Lição 1 - Instalar o GitHub Copilot CLI"
description: "Instale e autentique o Copilot CLI no Codespace, conheça os recursos e encontre o issue de filtragem predefinido."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O [GitHub Copilot CLI][about-copilot-cli] é um poderoso assistente de programação agêntico executado no terminal. Ele permite explorar bases de código, gerar código, executar comandos e interagir com ferramentas externas — tudo pela linha de comando. Com ele, você pode delegar tarefas, solicitar alterações e manter o foco. O primeiro passo, como você pode imaginar, é instalar a ferramenta. Felizmente, isso pode ser feito com ferramentas que você já conhece.

Nesta lição, você vai:

- instalar o GitHub Copilot CLI usando npm;
- autenticar-se com sua conta do GitHub;
- confiar no repositório do workshop e iniciar uma conversa rápida;
- encontrar o issue de filtragem pelo servidor MCP integrado do GitHub.

## Cenário

Sua equipe está começando a usar agentes de IA para trabalhar em um backlog crescente. O Copilot CLI leva esse recurso ao terminal, onde muitas pessoas desenvolvedoras já trabalham. Nesta lição, você instalará e autenticará a ferramenta e se preparará para usá-la no restante do workshop.

## Instale o Copilot CLI

Você pode instalar o Copilot CLI por meio do [npm][install-cli], WinGet ou Homebrew. Como o GitHub Codespaces já inclui o Node.js, você usará o npm.

1. Volte ao Codespace e abra um terminal.
2. Confirme que o Node.js está instalado e atende ao requisito de versão:

   ```bash
   node --version
   ```

   A versão deve ser 24 ou posterior.

3. Instale o Copilot CLI globalmente:

   ```bash
   npm install -g @github/copilot
   ```

4. Confirme a instalação:

   ```bash
   copilot --version
   ```

   O número da versão deverá aparecer.

## Autentique-se com o GitHub

Na primeira inicialização, o Copilot CLI solicitará a autenticação com sua conta do GitHub.

1. Inicie o Copilot CLI:

   ```bash
   copilot
   ```

2. Se solicitado, siga as instruções de autenticação com código de dispositivo e autorize o Copilot CLI.
3. O Copilot CLI exibirá este prompt:

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. Confirme que o caminho corresponde ao repositório Tailspin Toys e selecione **Yes, and remember this folder for future sessions**.

> [!NOTE]
> Em um Codespace, talvez você já esteja autenticado pela sessão do GitHub. Se o Copilot CLI iniciar sem solicitar autenticação, está tudo certo.

## Conheça os recursos

Os comandos no prompt normal do shell são executados diretamente no Codespace. Depois que o Copilot CLI for iniciado, o texto em linguagem natural será enviado ao agente, e os comandos de barra controlarão a conversa.

1. Digite `/model`, use as teclas de seta para selecionar **Auto**, pressione <kbd>Enter</kbd> e, em seguida, pressione <kbd>Enter</kbd> novamente para confirmar.
2. Digite `/help` para ver os comandos disponíveis na versão instalada e pressione <kbd>Esc</kbd> para fechar a tela de ajuda.
3. Faça uma pergunta simples para confirmar que tudo está funcionando:

   ```plaintext
   What are the key files in this project?
   ```

4. Leia a resposta e observe como o Copilot explora o repositório antes de responder.
5. Digite `/mcp list` e confirme que o servidor MCP integrado do GitHub está disponível.
6. Peça ao Copilot que encontre o issue de filtragem:

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. Abra a URL e leia o issue. Você usará esse issue depois de fazer uma primeira alteração rápida.

> [!TIP]
> Uma sessão normal do Copilot CLI trabalha na branch selecionada no terminal; ela não cria uma worktree automaticamente. Você criará uma branch de recurso antes de cada alteração.

## Use o atalho do workshop

Normalmente, o Copilot CLI pede autorização antes de usar ferramentas fora das permissões estabelecidas. Neste workshop, você o reiniciará com `--yolo`, um atalho aprovado pelo usuário que remove essas solicitações de aprovação dentro do Codespace para que você possa se concentrar nos exercícios.

> [!CAUTION]
> `--yolo` habilita permissões automáticas completas (`--allow-all-tools`, `--allow-all-paths` e `--allow-all-urls`). Use-o somente em um ambiente isolado, como um Codespace ou uma VM, e nunca o defina como alias padrão para o desenvolvimento diário. Consulte [Allowing and denying tool use][allow-all-warning] para obter detalhes.

Neste workshop, `--enable-all-github-mcp-tools` habilita as ferramentas MCP de leitura e gravação do GitHub usadas nas lições posteriores para trabalhar com issues e pull requests. O Codespace limita o acesso ao computador local, mas os recursos autenticados do GitHub continuam sendo reais. Revise as alterações antes de publicá-las ou integrá-las.

1. Saia do Copilot CLI com `/exit`.
2. Reinicie-o na raiz do repositório:

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. Faça outra pergunta rápida sobre o projeto para confirmar que a conversa está funcionando e saia com `/exit`.

O Copilot salva as conversas automaticamente. Mais adiante, depois de alterar as instruções ou adicionar um agente, use `copilot --resume` para voltar à mesma conversa e branch do recurso.

## Resumo e próximos passos

Parabéns! Nesta lição, você:

- instalou o GitHub Copilot CLI usando npm;
- autenticou-se com sua conta do GitHub;
- confiou no repositório do workshop e iniciou uma conversa rápida;
- encontrou o issue de filtragem pelo servidor MCP integrado do GitHub.

Em seguida, você [fará sua primeira alteração específica][next-lesson] e usará o Copilot CLI para exibir uma avaliação por estrelas nos cartões dos jogos.

## Recursos

- [Instalar o GitHub Copilot CLI][install-cli]
- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
