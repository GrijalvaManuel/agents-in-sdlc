---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de IA especializados para tarefas ou domínios específicos do seu fluxo de desenvolvimento. Ao definir agentes em arquivos Markdown na pasta `.github/agents` do repositório, você pode fornecer ao Copilot instruções direcionadas, práticas recomendadas, padrões de código e conhecimento específico do domínio para que ele execute determinados tipos de trabalho com mais eficiência. As equipes podem registrar seus conhecimentos em agentes reutilizáveis — por exemplo, um agente de acessibilidade que aplica a conformidade com [WCAG][wcag], um agente de segurança que segue práticas de código seguro ou um agente de testes que mantém padrões consistentes.

Os agentes personalizados são definidos por arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com, no mínimo, os campos `name` e `description`, seguidos de um prompt em Markdown que define o comportamento, os conhecimentos especializados e as instruções do agente.

### Agentes personalizados e skills de agente

Há certa sobreposição entre agentes personalizados e [skills de agente][agent-skills-concept]. Ambos são definidos principalmente em arquivos Markdown e orientam a IA sobre como executar operações. A maneira mais simples de distingui-los é: o **agente personalizado** é quem trabalha; as **skills** são as ferramentas.

Os agentes personalizados têm sua própria janela de contexto e são criados para orquestrar skills (e até outros agentes) durante o trabalho. Neste laboratório, o agente personalizado de acessibilidade revisa e atualiza o site de acordo com as diretrizes de acessibilidade; como parte do trabalho, ele poderia chamar skills, por exemplo, para executar um fluxo de trabalho de pull request ou executar e gerenciar testes.

> [!NOTE]
> Não existe uma única maneira certa de criar um agente personalizado. Como acontece com qualquer recurso de IA, teste e itere até encontrar o que funciona nos seus ambientes e cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys está comprometida em tornar a plataforma de financiamento coletivo acessível a todas as pessoas, independentemente de suas capacidades ou preferências visuais. Comentários recentes de usuários indicaram que algumas pessoas têm dificuldade para ler o tema escuro atual devido ao contraste insuficiente entre as cores do texto e do plano de fundo. Para resolver essa questão de acessibilidade, a equipe de design solicitou a implementação de um modo de alto contraste que as pessoas possam ativar e desativar.

Como a acessibilidade é fundamental, você quer implementar isso o mais rápido possível. Para gerar a funcionalidade, usará um agente personalizado.

Nesta lição, você vai aprender a:

- revisar um agente personalizado de acessibilidade existente;
- usar o agente de acessibilidade no Copilot Chat para implementar o modo de alto contraste.

## Revise o agente personalizado de acessibilidade

Já existe um agente personalizado de acessibilidade. Vamos revisar o conteúdo para entender como ele orienta o Copilot.

Volte ao codespace e abra um terminal. Alterne para uma nova branch baseada em `main` para trabalhar na acessibilidade (o PR de filtragem do Exercício 3 permanecerá separado):

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abra `.github/agents/accessibility.md`.
2. Observe os campos `name` e `description` no frontmatter YAML.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

3. Examine as seções seguintes, que destacam:
   - responsabilidades principais ao gerar código para um site acessível;
   - práticas recomendadas de acessibilidade;
   - exemplos de código HTML, CSS e JavaScript;
   - uma lista de erros e problemas comuns.

## Use o agente personalizado no Copilot Chat

O VS Code exibe todos os agentes personalizados definidos em `.github/agents` na lista de agentes na parte inferior da visualização Copilot Chat. Você pode selecionar um agente personalizado para limitar a sessão de chat às instruções e ferramentas desse agente.

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa nova. O modo e a seleção do modelo variam em cada exercício — cada etapa indicará as opções pertinentes.

1. Selecione **Agent** na lista de agentes da visualização Chat, caso ainda não esteja selecionado.

   ![Captura de tela da lista de agentes na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione a lista de agentes na parte inferior da visualização Chat (ela mostra o agente ativo, que por padrão é **default**).
3. Selecione **Accessibility agent** na lista de agentes disponíveis.
4. Envie este prompt ao agente de acessibilidade:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. O Copilot Chat começará a trabalhar — explorará a base de código, proporá alterações e aplicará edições aos arquivos do projeto. Cada edição aparecerá na conversa com o caminho do arquivo e um diff para você revisar.
6. À medida que as edições forem feitas, o indicador **Files changed** será atualizado para mostrar os arquivos alterados pelo agente.

> [!NOTE]
> Esse processo provavelmente levará alguns minutos. O Copilot está fazendo alterações reais no repositório — editará arquivos existentes, como componentes Astro, CSS e testes relacionados.

Você revisará e orientará esse trabalho em andamento no próximo exercício.

## Resumo e próximos passos

Nesta lição, você conheceu os [agentes personalizados][custom-agents], assistentes de IA especializados para tarefas e domínios específicos. Com eles, você pode registrar os conhecimentos e padrões da equipe em agentes reutilizáveis que orientam o Copilot a executar determinados tipos de trabalho com mais eficiência.

Você explorou:

- como os agentes personalizados são definidos;
- como usar um agente personalizado no modo de agente do Copilot Chat.

Em seguida, você vai [monitorar e orientar o trabalho do agente][next-lesson], revisando as alterações conforme acontecem e adicionando um controle de modo claro à mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: Testar o recurso com o servidor MCP do Playwright][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
