---
title: "Exercício 3 - Agentes personalizados"
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

Você vai explorar como:

- os agentes personalizados são definidos;
- atribuir uma tarefa a um agente personalizado.

## Cenário

A Tailspin Toys está comprometida em tornar a plataforma de financiamento coletivo acessível a todas as pessoas, independentemente de suas capacidades ou preferências visuais. Comentários recentes de usuários indicaram que algumas pessoas têm dificuldade para ler o tema escuro atual devido ao contraste insuficiente entre as cores do texto e do plano de fundo. Para resolver essa questão de acessibilidade, a equipe de design solicitou a implementação de um modo de alto contraste que as pessoas possam ativar e desativar.

Como a acessibilidade é fundamental, você quer implementar isso o mais rápido possível. Para gerar a funcionalidade, usará um agente personalizado.

## Revise o agente personalizado de acessibilidade

Já existe um agente personalizado de acessibilidade. Vamos revisar o conteúdo para entender como ele orienta o Copilot.

Volte ao codespace e examine o arquivo do agente personalizado de acessibilidade:

1. Abra `.github/agents/accessibility.md`.
2. Observe os campos `name` e `description` no frontmatter YAML.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

3. Examine as seções seguintes, que destacam:
   - responsabilidades principais ao gerar código para um site acessível;
   - práticas recomendadas de acessibilidade;
   - exemplos de código HTML, CSS e JavaScript;
   - uma lista de erros e problemas comuns.

## Crie e atribua um issue

O Mission Control é o local central para trabalhar com todos os agentes do seu ambiente. Você pode atribuir tarefas ao agente de nuvem do Copilot, monitorá-las e até redirecionar uma sessão em andamento e fornecer orientações adicionais. Vamos atribuir ao Copilot a tarefa de criar o modo de alto contraste.

1. Acesse seu repositório.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de novo issue.
4. Selecione **Blank issue** para criar o issue.
5. No campo **Title**, insira `Add high contrast mode to website`.
6. No campo **Description**, insira:

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. Selecione **Create** para criar o issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.
9. Selecione **Accessibility agent** na lista de agentes personalizados.

    ![Captura de tela da atribuição do agente de nuvem, com o agente personalizado e Accessibility em destaque](../../../_images/ex5-select-custom-agent.png)

10. Selecione **Assign**.
11. O Copilot começará a trabalhar na tarefa em segundo plano.

## Resumo e próximos passos

Nesta lição, você conheceu os [agentes personalizados][custom-agents] do GitHub Copilot, assistentes de IA especializados para tarefas e domínios específicos. Com eles, você pode registrar os conhecimentos e padrões da equipe em agentes reutilizáveis que orientam o Copilot a executar determinados tipos de trabalho com mais eficiência.

Você explorou como:

- os agentes personalizados são definidos;
- atribuir uma tarefa a um agente personalizado.

Enquanto o Copilot trabalha na implementação do modo de alto contraste, vamos [acompanhar e orientar a sessão do agente][next-lesson] pelo Mission Control.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Preparar-se para usar agentes personalizados na organização][org-custom-agents]
- [Preparar-se para usar agentes personalizados na empresa][enterprise-custom-agents]

---

| [← Lição anterior: Agente de nuvem do GitHub Copilot][previous-lesson] | [Próxima lição: Gerenciar e monitorar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
