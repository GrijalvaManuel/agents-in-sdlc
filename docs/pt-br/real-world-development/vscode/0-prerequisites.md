---
title: "Exercício 0: Pré-requisitos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de começar os exercícios do VS Code, você precisa preparar tudo. Crie sua própria cópia do repositório Tailspin Toys, inicie um [codespace][codespaces] para trabalhar e confirme se o GitHub Copilot Chat está funcionando no editor.

## Configurar o repositório do laboratório

Para criar uma cópia do repositório que conterá o código que você desenvolverá, crie uma instância a partir do [template][template-repository]. A nova instância terá todos os arquivos necessários para o laboratório e será usada durante os exercícios.

1. Em uma nova janela do navegador, acesse o [template Tailspin Toys][tailspin-template].
2. Crie sua própria cópia do repositório selecionando o botão **Use this template** na página do repositório do laboratório. Em seguida, selecione **Create a new repository**.

    ![Botão Use this template](../../../_images/ex0-use-template.png)

3. Se você estiver participando de um evento conduzido pelo GitHub ou pela Microsoft, siga as instruções das pessoas mentoras. Caso contrário, você poderá criar o novo repositório em uma organização na qual tenha acesso ao GitHub Copilot.

    ![Configurações do template do repositório](../../../_images/ex0-repository-settings.png)

4. Anote o caminho do repositório que você criou (**organization-or-user-name/repository-name**), pois ele será necessário mais adiante no laboratório.

> [!NOTE]
> **Seu backlog está pronto**
>
> Quando você cria o repositório a partir do template, um backlog de issues do GitHub é criado automaticamente. Você usará esses issues ao longo do workshop — não precisa criar nenhum.

## Criar um codespace

Em seguida, você usará um codespace para concluir os exercícios do laboratório.

[GitHub Codespaces][codespaces] é um ambiente de desenvolvimento na nuvem que permite escrever, executar e depurar código diretamente no navegador. Ele oferece um IDE completo com suporte a várias linguagens de programação, extensões e ferramentas.

1. Acesse o repositório que você acabou de criar.
2. Selecione o botão verde **Code**.

    ![Selecione o botão Code](../../../_images/ex0-code-button.png)

3. Selecione a guia **Codespaces** e o botão **+** para criar um novo codespace.

    ![Criar um novo codespace](../../../_images/ex0-create-codespace.png)

A criação do codespace levará alguns minutos, mas ainda será muito mais rápida do que instalar todos os serviços manualmente. Enquanto isso, você pode explorar outros recursos do GitHub Copilot, que serão apresentados em seguida.

> [!CAUTION]
> Você voltará ao codespace em um exercício futuro. Por enquanto, deixe-o aberto em uma guia do navegador.

> [!NOTE]
> Este workshop foi criado para ser executado em um codespace ou em um [contêiner de desenvolvimento][dev-containers] local. Ambos incluem todos os pré-requisitos necessários para que a experiência seja tranquila. Se preferir trabalhar localmente, abra o repositório clonado no VS Code e selecione **Reopen in Container** quando solicitado — o VS Code criará o mesmo contêiner de desenvolvimento usado pelo codespace.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Usar o GitHub Copilot Chat e o modo de agente

Para acessar o modo de agente do GitHub Copilot Chat, instale a extensão GitHub Copilot Chat no IDE. Ela já deve estar instalada se você estiver usando o GitHub Codespaces.

> [!TIP]
> Se a extensão GitHub Copilot Chat não estiver instalada, você poderá [instalá-la no Visual Studio Code Marketplace][copilot-chat-extension]. Outra opção é abrir a visualização Extensions no Visual Studio Code, pesquisar **GitHub Copilot Chat** e selecionar **Install**.

Depois de instalar a extensão, talvez seja necessário autenticar com sua conta do GitHub para habilitá-la.

1. Volte ao codespace.
2. Se o Copilot Chat ainda não estiver visível à direita do editor, selecione o ícone **Copilot Chat** na parte superior do codespace.
3. Digite uma mensagem, como "Hello world", na janela do Copilot Chat e pressione Enter. Isso deverá ativá-lo.
4. Se você ainda não estiver autenticado, uma solicitação de entrada na conta do GitHub aparecerá. Siga as instruções para autenticar.

    ![Exemplo de solicitação de autenticação do Copilot Chat](../../../_images/ex1-copilot-authentication.png)

5. Após a autenticação, a janela do Copilot Chat deverá aparecer.

## Resumo

Parabéns! Você criou uma cópia do repositório do laboratório e começou a criar seu codespace, que será usado quando você começar a escrever código.

## Próxima etapa

Vamos colocar o Copilot para trabalhar. Continue para o [Exercício 1: Instruções personalizadas][next-lesson], no qual você ensinará ao Copilot as convenções do projeto.

## Recursos

- [Visão geral do GitHub Codespaces][codespaces]
- [Criar um repositório a partir de um template][template-repository]
- [Introdução ao Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
