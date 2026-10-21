---
title: "Lição 9 - Comandos de barra no GitHub Copilot CLI"
description: "Explore comandos de barra para gerenciar contexto, selecionar modelos, compartilhar sessões e delegar tarefas à nuvem, se quiser."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como toda boa ferramenta de CLI, o GitHub Copilot CLI inclui vários comandos de barra para interação. Esses comandos disponibilizam funcionalidades avançadas, informações de bastidores e opções adicionais de configuração. Você já usou comandos como `/diff`, `/mcp`, `/skills`, `/agent` e `/pr`. Vamos explorar alguns outros que podem ser úteis.

Nesta lição, você vai:

- usar `/context` e `/compact` para explorar como o Copilot gerencia o contexto da conversa;
- usar `/model` para explorar os modelos disponíveis;
- aprender como `/share` pode exportar ou compartilhar uma sessão;
- explorar comandos opcionais para trabalho paralelo, worktrees e delegação ao agente de nuvem.

## Cenário

Você concluiu o fluxo central da CLI. Agora, vamos explorar alguns recursos adicionais — gerenciar contexto, alternar modelos, compartilhar sessões e, opcionalmente, delegar trabalho ao [agente de nuvem do Copilot][about-cloud-agent].

## Explore o contexto do Copilot CLI

Ao trabalhar em tarefas maiores ou mais complexas, talvez você chegue ao limite da janela de contexto do modelo. O Copilot CLI compacta a conversa automaticamente quando isso é necessário. Você também pode inspecionar ou compactar o contexto manualmente com os comandos de barra.

1. Volte ao Codespace e inicie o Copilot CLI na raiz do repositório, caso ainda não esteja aberto.
2. Digite:

   ```plaintext
   /context
   ```

3. Observe o modelo, o uso atual de tokens e a divisão do contexto entre as instruções do sistema, as ferramentas, as mensagens e o espaço livre.
4. Compacte a conversa:

   ```plaintext
   /compact
   ```

5. Digite `/context` novamente e compare o resultado. Talvez não haja uma grande diferença se a conversa ainda for curta.

> [!NOTE]
> O Copilot CLI compacta o contexto automaticamente conforme a janela fica cheia. Use `/compact` para escolher o momento. Use `/clear` ou `/new` ao mudar para uma tarefa não relacionada e começar uma conversa nova.

## Escolha seu modelo

Modelos diferentes têm pontos fortes diferentes, e pessoas desenvolvedoras diferentes têm preferências diferentes. O Copilot CLI permite listar e selecionar o modelo que você quer usar.

1. Digite:

   ```plaintext
   /model
   ```

2. Explore os modelos disponíveis e as informações de uso.
3. Mantenha o modelo atual, selecione outro ou pressione <kbd>Esc</kbd> para fechar a lista.

## Compartilhe uma sessão

Trabalhar em equipe e compartilhar aprendizados ajuda todas as pessoas a melhorar no uso de ferramentas de IA. O comando `/share` pode exportar uma sessão para um arquivo Markdown ou HTML, criar um link compartilhável ou publicar uma GitHub gist.

1. Digite `/help` e revise as opções `/share` disponíveis na versão instalada.
2. Se quiser compartilhar a sessão, escolha um destino adequado às suas necessidades, como `/share file` para exportá-la localmente em Markdown.
3. Revise o conteúdo exportado antes de enviá-lo a alguém ou publicá-lo. As exportações de sessões podem incluir prompts, respostas e detalhes do projeto.

A publicação de um link ou gist é opcional. Não publique conteúdo do repositório ou da conversa que a equipe não pretende compartilhar.

## Opcional: ampliar o trabalho ou delegar

O workshop principal foi concluído. O Copilot CLI também oferece comandos para tarefas maiores:

- `/fleet` pode dividir subtarefas independentes entre subagentes e executá-las em paralelo;
- `/worktree` pode criar uma worktree isolada para uma tarefa separada;
- `/delegate` pode enviar uma tarefa ao agente de nuvem do Copilot, que trabalhará de forma assíncrona e talvez abra um pull request.

Esses comandos são opcionais porque podem criar worktrees adicionais ou iniciar trabalho remoto. Comece uma tarefa nova e bem delimitada antes de experimentá-los e revise o resultado pelo fluxo de trabalho habitual. Para conhecer melhor o trabalho assíncrono com agentes, continue no [workshop do agente de nuvem][cloud-workshop].

## Resumo e próximos passos

Os comandos de barra do Copilot CLI permitem configurá-lo, compartilhar sessões e entender o que acontece nos bastidores. Nesta lição, você:

- usou `/context` e `/compact` para explorar como o Copilot gerencia o contexto da conversa;
- usou `/model` para explorar os modelos disponíveis;
- aprendeu como `/share` pode exportar ou compartilhar uma sessão;
- explorou comandos opcionais para trabalho paralelo, worktrees e delegação ao agente de nuvem.

Há mais comandos de barra e outros recursos para explorar no Copilot CLI. Vamos concluir esta jornada [revisando o que aprendemos][next-lesson] e os próximos passos para continuar aprendendo.

## Recursos

- [Referência de comandos do Copilot CLI][cli-reference]
- [Gerenciamento de contexto no Copilot CLI][context-management]
- [Sobre o agente de nuvem do Copilot][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
