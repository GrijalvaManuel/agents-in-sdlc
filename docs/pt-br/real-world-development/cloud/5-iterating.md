---
title: "Exercício 5: Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revise o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Você pediu ao Copilot que documentasse o código, criasse um recurso de jogos relacionados para a equipe de design avaliar e implementasse recursos de acessibilidade, incluindo controles de modo de alto contraste e modo claro. Vamos explorar as alterações no código e, se necessário, dar feedback ao Copilot para melhorar o resultado.

### Cenário

Como já destacamos várias vezes, os fundamentos de design de software e DevOps não mudam com a chegada da IA generativa. Sempre devemos revisar o código gerado e seguir nosso processo normal de DevOps. Com isso em mente, vamos revisar as sugestões do GitHub Copilot para a documentação, o recurso de jogos relacionados e os recursos de acessibilidade antes de habilitar a revisão para o restante da equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem do Copilot executa tarefas de forma assíncrona e sem supervisão, há restrições de segurança para manter tudo protegido. Entre elas:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à branch que usará para o código.
- O agente de nuvem é executado no GitHub Actions, em um ambiente separado e efêmero criado para o trabalho.
- Qualquer fluxo de trabalho do GitHub Actions precisa de aprovação de uma pessoa antes de ser executado.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), inclusive a servidores MCP.

## Revise a documentação gerada

Vamos começar examinando o primeiro pull request (PR) criado pelo agente de nuvem do GitHub Copilot: o que adiciona documentação ao código. Você fará isso pela interface padrão de PRs do GitHub.com.

> [!NOTE]
> Ao explorar o PR, talvez você veja um aviso de que o GitHub Copilot foi bloqueado por um firewall. Isso é **esperado**, pois o acesso do Copilot a recursos externos, incluindo chamadas a servidores MCP externos, é limitado por padrão. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem do Copilot][agent-firewall].

1. Volte ao seu repositório em github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra o pull request com um título parecido com **Add missing documentation** ou outro título mais descritivo.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request terá a indicação **[WIP]**. Nesse caso, espere o Copilot concluir o trabalho. Isso pode levar alguns minutos; aproveite o tempo para descansar ou refletir sobre o que aprendeu até aqui.

4. Quando o pull request estiver pronto, selecione a guia **Files changed** e revise as alterações.

    ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Explore o código atualizado, que inclui os novos comentários TSDoc e outras documentações. As alterações exatas variam.

    Ao revisar as alterações, procure comentários TSDoc, convenções TypeScript e cabeçalhos de comentários. Eles vêm dos arquivos de instruções personalizados que você examinou no início do Exercício 2.

6. Depois de revisar as atualizações e confirmar que parecem corretas, volte à guia **Conversation** e role a página para baixo.
7. Você deverá ver um indicador de que alguns fluxos de trabalho estão aguardando aprovação.
8. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

9. Os fluxos de trabalho deverão entrar na fila da seção de verificações do pull request. Se tudo der certo, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.

## Peça alterações ao GitHub Copilot

Trabalhar com o Copilot em um pull request não é uma via de mão única. Você também pode mencioná-lo em comentários — como faria com outras pessoas da equipe — no PR ou em comentários de linha no código. O Copilot verá esses comentários e iniciará outra sessão para tratar deles. Como os resultados são não determinísticos, não podemos fornecer um texto de prompt obrigatório. Algumas ideias do que pedir ao Copilot:

- adicionar cabeçalhos de comentários ao início de cada arquivo de código com uma breve descrição da finalidade;
- adicionar comentários TSDoc a arquivos TypeScript e Astro;
- criar um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário pedindo uma alteração na documentação gerada e mencione **@copilot**, como faria com qualquer pessoa. Use uma das sugestões acima ou peça outra alteração na documentação do código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe que ele inicia uma nova sessão para fazer as atualizações.
3. Você pode selecionar **Back to pull request** para voltar ao pull request.

    ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot terminar as alterações, um novo commit aparecerá no pull request.
5. Selecione a guia **Files changed** para revisar as alterações.

Continue iterando até ficar satisfeito. Depois, você poderá marcar o PR como pronto para revisão e fazer o merge na branch principal.

![Marcar PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revise o recurso de jogos relacionados

Agora, vamos voltar ao PR criado pelo Copilot para resolver o issue sobre a exibição de jogos relacionados na página de detalhes.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título parecido com **Show related games on the game details page** ou outro título mais descritivo.
4. Selecione a guia **Files changed** para revisar o código gerado.
5. Depois de revisar as atualizações e confirmar que parecem corretas, volte à guia **Conversation** e role para baixo.
6. Você deverá ver um indicador de que alguns fluxos de trabalho estão aguardando aprovação.
7. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

8. Os fluxos de trabalho deverão entrar na fila da seção de verificações do pull request. Se tudo der certo, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** você pode alternar seu Codespace para esta branch e testar manualmente o recurso de jogos relacionados. Acesse o Codespace, abra o terminal e execute estes comandos (substitua `<branch-name>` pelo nome da branch criada pelo Copilot, por exemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

O Copilot criou o recurso de jogos relacionados. Como antes, você pode iterar com o agente de nuvem do Copilot e solicitar atualizações. Por exemplo, peça para alterar a quantidade de jogos relacionados exibidos ou para adicionar cabeçalhos de comentários e comentários TSDoc. Lembre-se de que esse trabalho foi atribuído **antes** da atualização das instruções personalizadas. Como antes, faça essas solicitações com um novo comentário na guia **Conversation**; o Copilot verá o comentário e iniciará uma nova sessão.

## Revise os recursos de acessibilidade

Por fim, vamos revisar os recursos de acessibilidade implementados com o agente personalizado de acessibilidade. Este PR deve incluir tanto o modo de alto contraste atribuído no Exercício 3 quanto o modo claro solicitado no Mission Control no Exercício 4.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título parecido com **Add high contrast mode to website** ou outro título mais descritivo.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request terá a indicação **[WIP]**. Nesse caso, espere o Copilot concluir o trabalho. Isso pode levar alguns minutos.

4. Selecione a guia **Files changed** para revisar o código gerado.
5. Revise a implementação, prestando atenção especial a:
   - componentes de interface para alternar entre os modos;
   - uso do armazenamento local (local storage) para persistir as preferências;
   - alterações de CSS ou estilo para os modos de alto contraste e claro;
   - atributos de acessibilidade (rótulos ARIA, navegação por teclado etc.);
   - código JavaScript/TypeScript responsável por alternar os modos.

6. Depois de revisar as atualizações e confirmar que parecem corretas, volte à guia **Conversation** e role para baixo.
7. Você deverá ver um indicador de que alguns fluxos de trabalho estão aguardando aprovação.
8. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

9. Os fluxos de trabalho deverão entrar na fila da seção de verificações do pull request. Se tudo der certo, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** você pode alternar seu Codespace para esta branch e testar manualmente os recursos de acessibilidade. Acesse o Codespace, abra o terminal e execute estes comandos (substitua `<branch-name>` pelo nome da branch criada pelo Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Em seguida, inicie o aplicativo e teste no navegador os controles de modo de alto contraste e modo claro. Confirme que funcionam e que as preferências persistem depois de recarregar as páginas.

Observe como o agente personalizado de acessibilidade ajudou a orientar o Copilot na implementação desses recursos de acordo com as práticas recomendadas de acessibilidade. Se notar algum problema ou oportunidade de melhoria, mencione **@copilot** em um comentário para pedir atualizações, como fez com os outros PRs.

## Exercício opcional — continue delegando

O agente de nuvem funciona melhor quando você pode atribuir a ele itens reais do backlog e cuidar de outras tarefas. Para criar esse hábito, registre mais alguns issues no repositório e atribua-os ao Copilot. Algumas ideias:

- criar um formulário de interesse de apoiadores na página de detalhes dos jogos;
- implementar paginação na lista de jogos;
- adicionar validação de entrada e tratamento de erros aos helpers de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem. Ao longo destas lições, você:

- **Examinou os arquivos de instruções personalizados deste repositório** para conhecer sua influência no resultado do agente de nuvem.
- **Atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** para acessibilidade, adicionando controles de modo de alto contraste e modo claro.
- **Usou a página de agentes do Copilot como Mission Control** para monitorar e orientar a sessão de acessibilidade.
- **Revisou e iterou sobre os pull requests do agente de nuvem**, mencionando `@copilot` para solicitar alterações e aprovando fluxos de trabalho.

## Revisão e próximos passos

Você concluiu o percurso do agente de nuvem. Se quiser continuar aprendendo, os outros percursos complementam o que praticou:

- 🖥️ **[Percurso do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração MCP diretamente no IDE.
- 💻 **[Percurso da CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal usando os modos Plan e Autopilot, skills de agente, um agente de QA personalizado, Playwright MCP, Agent Merge e uma ponte opcional `/delegate` para o agente de nuvem.

No seu próprio repositório, experimente estas ideias:

- atribua ao agente de nuvem um issue de refatoração ou cobertura de testes;
- crie um agente personalizado para outro domínio;
- configure `copilot-setup-steps.yml` para outra stack.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de trabalho de configuração do agente de nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem do Copilot][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
