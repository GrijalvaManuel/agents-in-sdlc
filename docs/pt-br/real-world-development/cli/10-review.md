---
title: "Lição 10 - Recapitulação e próximos passos"
description: "Recapitule o fluxo de trabalho do Copilot CLI, os dois pull requests, as personalizações reutilizáveis e outros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Você usou o GitHub Copilot CLI ao longo de um fluxo contínuo da Tailspin Toys. Você:

- preparou um Codespace, instalou o Copilot CLI, explorou o projeto e encontrou o issue de filtragem predefinido;
- adicionou avaliações por estrelas, revisou o resultado no navegador usando o encaminhamento de porta do Codespaces e integrou manualmente seu primeiro pull request (PR);
- começou pelo issue de filtragem, definiu a abordagem no modo Plan, implementou no modo Autopilot e revisou no modo Interactive;
- orientou o agente com instruções personalizadas, personalizou a skill existente `quality-checks` e usou-a para executar testes unitários, lint e verificações de tipos;
- adicionou o servidor Model Context Protocol (MCP) do Playwright e usou-o para explorar a filtragem em um navegador real;
- criou e selecionou um agente personalizado de garantia de qualidade (QA) para avaliar requisitos, cobertura, resultados da skill e evidências do navegador;
- revisou a alteração completa de filtragem e autorizou o Agent Merge para o PR de filtragem;
- explorou comandos de barra para gerenciar contexto, modelos e compartilhamento e delegar tarefas à nuvem, se quiser.

## O que você entregou

O workshop tem dois marcos de PR, cada um em sua própria branch baseada na `main` atualizada:

1. **Avaliações por estrelas:** exibição de `starRating` existente e de uma mensagem explícita quando não houver avaliação nos cartões de jogos.
2. **Filtragem e fluxo de qualidade:** implementação da filtragem, atualização das instruções e aplicação delas ao recurso, personalização do relatório `quality-checks`, criação de um perfil de QA e inclusão dos testes relacionados.

Do planejamento da filtragem à abertura do PR, você usou a mesma conversa e branch. Combinamos o trabalho em um único PR para simplificar o workshop.

## Diferentes tipos de verificação

Você verificou o código de várias maneiras: testes automatizados, inspeção própria no navegador e exploração do navegador pelo Copilot via MCP. A skill `quality-checks` executou testes unitários, lint e verificações de tipos e informou os resultados no novo formato. O QA reuniu esses resultados e os combinou com uma revisão dos requisitos e da cobertura de testes antes do PR.

Os testes adicionados devem corrigir lacunas reais; uma execução de QA que não exija novos testes também pode estar correta. Revise o código e as evidências antes de autorizar o merge e atualize as evidências afetadas quando fizer alterações.

## Práticas recomendadas

O contexto e as ferramentas fornecidos ao Copilot influenciam o trabalho realizado. Neste workshop, você atualizou instruções, personalizou uma skill, criou um perfil de QA e configurou um servidor MCP. Reutilize essas personalizações em outras conversas e adapte-as às necessidades da equipe. Instruções definem padrões; skills descrevem tarefas repetíveis; agentes personalizados definem funções especializadas; e servidores MCP conectam ferramentas externas. Revise as alterações e os resultados reais das ferramentas, não apenas o resumo do agente.

Escolha o **modo e o modelo** de acordo com a tarefa. Use o modo **Plan** para pensar na abordagem antes de começar, o modo **Interactive** para acompanhar alterações específicas e o modo **Autopilot** para tarefas bem delimitadas. Escolha um modelo mais rápido para edições rotineiras e um mais avançado para trabalhos complexos.

O contexto continua tão importante quanto a infraestrutura. Descrever claramente *o que* deve ser criado, *por que* e *como* altera significativamente o resultado.

## Mais coisas para explorar

Você concluiu o fluxo de trabalho principal. Há alguns outros recursos da CLI que vale a pena conhecer:

- `/review` para pedir ao agente de revisão de código que analise as alterações;
- `/rubber-duck` para conversar sobre um problema e obter outra perspectiva;
- `/fleet` para orquestrar subtarefas independentes em paralelo;
- `/worktree` para isolar uma tarefa separada;
- `/delegate` para enviar uma tarefa ao agente de nuvem do Copilot.

## Próximos passos

A melhor maneira de melhorar com qualquer ferramenta é continuar usando-a. Use-a em código de produção, em projetos pessoais ou naquele pequeno aplicativo que você pensa em criar há anos. Compartilhe o que aprendeu com a equipe e aprenda com outras pessoas. E, como sempre, explore a documentação.

Se quiser ampliar a Tailspin Toys pelo terminal, continue com a série opcional [Foundry Backer Concierge][foundry]. Para comparar outros ambientes, explore o [workshop do VS Code][vscode], o [workshop do aplicativo GitHub Copilot][app] ou o [workshop do agente de nuvem do Copilot][cloud].

## Recursos

- [Sobre o GitHub Copilot CLI][about-cli]
- [Referência de comandos do Copilot CLI][cli-reference]
- [Personalizar o Copilot CLI][customize-cli]
- [Gerenciar pull requests com o Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
