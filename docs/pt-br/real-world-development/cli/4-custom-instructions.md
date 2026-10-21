---
title: "Lição 4 - Orientar o Copilot com instruções personalizadas"
description: "Explore as instruções do repositório, adicione um padrão de documentação e aplique-o ao código de filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O contexto é fundamental ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de uma maneira específica, essa orientação deve estar acessível ao Copilot. Os [arquivos de instruções][instruction-files] descrevem não só *qual* código você quer, mas também *como* ele deve ser estruturado. Agora que você desenvolveu a filtragem, vai explorar as instruções usadas pelo Copilot, adicionar um padrão de documentação e aplicá-lo ao seu código.

Nesta lição, você vai:

- explorar como as instruções do repositório e os arquivos de instruções com escopo definido por caminho chegam ao agente;
- atualizar o arquivo de instruções para garantir que os padrões de código sejam seguidos;
- observar o impacto dos arquivos de instruções no código.

## Cenário

Como toda boa empresa de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento. Entre eles:

- Os comentários devem explicar a intenção e decisões não óbvias, em vez de repetir o código.
- As funções exportadas em `db/` e `src/lib/` devem documentar a finalidade, os parâmetros e os valores de retorno usando TSDoc/JSDoc, incluindo um argumento `db` injetável quando aplicável.
- Os componentes Astro reutilizáveis devem documentar os contratos `Props`, e os comentários devem continuar corretos quando o código relacionado for alterado.
- As orientações existentes sobre formatação e lint devem ser preservadas.

Com os arquivos de instruções, você garantirá que o Copilot tenha as informações necessárias para executar tarefas de acordo com essas práticas.

## Arquivos de instruções

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo de código e seus requisitos. Esse recurso ajuda a orientar as sugestões e os trechos de código. Você pode especificar convenções de código, bibliotecas e até os tipos de comentários que prefere. Crie instruções para todo o repositório ou para tipos específicos de arquivos e tarefas.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`, um único arquivo enviado ao Copilot em **todas** as solicitações para o repositório. Ele deve conter informações gerais do projeto que sejam relevantes para a maioria das solicitações.
- Arquivos `.github/instructions/*.instructions.md`, que fornecem orientações para linguagens, tipos de arquivo ou tarefas específicas.

> [!NOTE]
> O suporte a outros formatos de instrução varia de acordo com o ambiente. Consulte a [referência de suporte a instruções personalizadas][custom-instructions-support] antes de depender de um formato específico.

## Explore os arquivos de instruções personalizadas deste projeto

O projeto inicial já inclui um conjunto de arquivos de instruções. Vamos explorá-los antes de fazer alterações para observar o impacto.

1. Volte ao Codespace.
2. No editor do Codespaces (não no terminal), abra `.github/copilot-instructions.md`.
3. Explore o arquivo e observe a breve descrição do projeto e as orientações de código. Essas instruções se aplicam a todas as interações com o Copilot neste repositório.
4. Abra a pasta `.github/instructions` e explore os arquivos. Observe as instruções para arquivos Astro, a camada de dados Drizzle, testes e outros tópicos.
5. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início — ele define um glob que determina a quais arquivos as instruções se aplicam.
6. Abra `.github/instructions/drizzle.instructions.md` e observe os links para outros arquivos de instruções e para arquivos existentes no projeto. Isso permite dividir conjuntos maiores de instruções em partes menores e reutilizáveis e apontar para exemplos.

## Atualize os arquivos de instruções para refletir as diretrizes da equipe

Os arquivos já são um bom começo, mas ainda há uma lacuna. Vamos modificar o arquivo principal `copilot-instructions.md` para garantir que comentários TSDoc sejam adicionados ao TypeScript gerado.

1. Em `.github/copilot-instructions.md`, localize a seção **Code formatting guidance**, que deve estar próxima à linha 35.
2. Adicione o seguinte item ao fim da seção:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

O arquivo será salvo automaticamente.

## Use as orientações atualizadas

O Copilot CLI carrega as instruções do repositório quando uma conversa é iniciada. Depois de salvar as instruções atualizadas, saia e retome a conversa de filtragem para que o Copilot carregue as novas orientações sem perder o contexto do recurso.

1. Se a conversa de filtragem ainda estiver aberta, use `/exit` para fechá-la. O Copilot CLI salva as conversas automaticamente.
2. Na raiz do repositório, na branch `game-filters-cli`, retome a conversa salva:

   ```bash
   copilot --resume
   ```

   Se solicitado, selecione a conversa de filtragem e confirme que a branch selecionada é `game-filters-cli`.

3. Peça ao Copilot que aplique as orientações atualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Digite `/diff` e revise os arquivos TypeScript alterados. Observe os comentários TSDoc recém-gerados e confirme se descrevem o código corretamente.

## Resumo e próximos passos

Você explorou como o Copilot CLI usa os arquivos de instruções e aplicou um novo padrão ao seu recurso. Você:

- explorou como as instruções do repositório e os arquivos de instruções com escopo definido por caminho chegam ao agente;
- atualizou o arquivo de instruções para que os padrões de código sejam seguidos;
- observou o impacto dos arquivos de instruções no código.

Em seguida, vamos [personalizar e executar a skill reutilizável `quality-checks`][next-lesson] para garantir que o lint e os testes sejam executados de maneira consistente.

## Recursos

- [Adicionar instruções personalizadas ao Copilot CLI][instruction-files]
- [Suporte a instruções personalizadas][custom-instructions-support]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Awesome Copilot — uma coleção de arquivos de instruções e outros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
