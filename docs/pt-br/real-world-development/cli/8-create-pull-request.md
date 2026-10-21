---
title: "Lição 8 - Criar e integrar o PR do recurso"
description: "Revise a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes; depois, crie um PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

A implementação da filtragem, as atualizações das instruções e da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma branch. Agora, revise tudo em conjunto e abra um pull request. Você integrou o pull request (PR) de avaliação por estrelas manualmente; desta vez, deixará o **Agent Merge** gerenciar o processo.

> [!NOTE]
> Em geral, dividiríamos o recurso, as atualizações das instruções, a atualização da skill e o agente de QA em vários PRs. Para simplificar o workshop, mantivemos todo o fluxo de filtragem e qualidade em uma conversa e branch, e todo esse trabalho será incluído neste PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de merge;
- inspecionar a alteração completa do recurso e as evidências de verificação;
- criar o PR de filtragem;
- habilitar o Agent Merge somente depois da revisão e confirmar que o PR foi integrado.

## Cenário

Ao longo do fluxo de trabalho de filtragem, você usou o Copilot para planejar, implementar e verificar um recurso. Agora, a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo sob controle da pessoa desenvolvedora a autorização do merge.

## Conheça o Agent Merge

O **Agent Merge** automatiza o trabalho restante para integrar um pull request. Quando habilitado, o Copilot trata o que estiver impedindo o merge — corrige falhas nas verificações de integração contínua (CI), responde a comentários de revisão e faz rebase quando necessário — e, em seguida, habilita o merge automático do GitHub quando o repositório permite.

Até agora, você selecionou **Merge pull request** manualmente. O Agent Merge pode assumir essa responsabilidade. Revise o trabalho e confirme que está pronto antes de habilitar o Agent Merge.

## Use o Agent Merge para gerenciar o PR

Com o código pronto, vamos revisá-lo, criar o PR e deixar que o Agent Merge gerencie o restante do processo.

1. Volte ao Codespace.
2. Abra a caixa de diálogo de agentes digitando `/agent`.
3. Selecione **Default** na lista de opções e pressione <kbd>Enter</kbd>.
4. Crie um novo PR usando o comando `/pr create`.
5. Ative o Agent Merge com `/pr agentmerge`.
6. O Copilot acompanhará o processo de integração contínua no PR. Quando todas as verificações forem aprovadas, ele fará o merge.
7. Confirme que aparece uma mensagem do Copilot semelhante a "PR #14 was squash-merged successfully."

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteção de branches, filas de merge, configurações do repositório ou permissões ausentes. Se houver algum bloqueio, leia o motivo informado e faça manualmente um merge revisado quando o repositório permitir.

## Resumo e próximos passos

Você automatizou várias partes do processo de desenvolvimento, incluindo a geração, o teste e a validação de código e, agora, o processo do pull request. Você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de merge;
- inspecionou a alteração completa do recurso e as evidências de verificação;
- criou o PR de filtragem;
- habilitou o Agent Merge somente depois da revisão e confirmou que o PR foi integrado.

Em seguida, você [explorará outros comandos de barra úteis do Copilot CLI][next-lesson] para gerenciar contexto, modelos e compartilhamento e, opcionalmente, delegar tarefas ao agente de nuvem.

## Recursos

- [Gerenciar pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
