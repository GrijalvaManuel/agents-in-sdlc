---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 Copilot 云端代理练习前，需要先完成准备工作。你将创建 Tailspin Toys 存储库的副本，并启动一个 [codespace][codespaces]，用于编辑指令文件和审查云端代理生成的成果。

## 设置实验存储库

要创建用于练习的代码副本，请从[模板存储库][template-repository]创建一个实例。新实例会包含实验所需的文件，后续练习都将在其中进行。

1. 在新的浏览器窗口中打开 [Tailspin Toys 模板][tailspin-template]。
2. 在实验存储库页面选择 **Use this template**，然后选择 **Create a new repository**，创建自己的存储库副本。

    ![Use this template 按钮](../../../_images/ex0-use-template.png)

3. 如果你正在参加由 GitHub 或 Microsoft 主办的活动，请遵循导师提供的说明。否则，可以在拥有 GitHub Copilot 访问权限的组织中创建存储库。

    ![输入存储库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下创建的存储库路径（**组织或用户名/存储库名称**），稍后会在实验中用到。

## 创建 codespace

接下来，你将使用 codespace 完成实验练习。

[GitHub Codespaces][codespaces] 是基于云的开发环境，让你可以直接在浏览器中编写、运行和调试代码。它提供功能完善的 IDE，支持多种编程语言、扩展和工具。

1. 打开刚创建的存储库。
2. 选择绿色的 **Code** 按钮。

    ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，然后选择 **+** 按钮创建新的 Codespace。

    ![创建新的 codespace](../../../_images/ex0-create-codespace.png)

创建 codespace 需要几分钟，不过仍比手动安装所有服务快得多。等待期间，可以探索 GitHub Copilot 的其他功能；接下来我们会开始介绍这些功能。

> [!CAUTION]
> 后续练习会再次使用此 codespace。现在先不要关闭它，请让它保持在浏览器选项卡中打开。

> [!NOTE]
> 本工作坊可在 codespace 或本地 [dev container][dev-containers] 中运行。两者都能确保环境中已安装顺利完成练习所需的先决条件。如果希望在本地运行，请在 VS Code 中打开克隆的存储库，并在提示时选择 **Reopen in Container**；VS Code 会构建与 codespace 相同的开发容器。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 总结

恭喜，你已创建实验存储库的副本，并开始创建 codespace。接下来，你将与 Copilot 云端代理一起使用这个环境。

## 下一步

接下来，为云端代理添加自定义指令。继续学习[练习 1：自定义指令][next-lesson]。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [从模板创建存储库][template-repository]
- [开始使用 Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
