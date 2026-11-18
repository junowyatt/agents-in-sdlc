---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解背景信息，就应确保它能访问这些上下文。[指令文件][instruction-files]用于提供此类指导，让 Copilot 不仅了解要做什么，也了解应如何完成。

本练习将学习如何：

- 通过存储库自定义指令和按路径限定的指令文件，向 Copilot 提供项目上下文、编码指南和文档规范。
- 在 `.github/copilot-instructions.md` 中添加新的全局存储库规范。

> [!NOTE]
> 与 VS Code 和 CLI 学习路径不同，本练习不会比较更新指令前后的提示效果。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示指令的影响。稍后审查云智能体创建的拉取请求时，可以看到指令文件发挥的作用。

## 指令文件

### 场景

Tailspin Toys 和其他规范的开发团队一样，制定了一系列开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应使用深色模式并具有现代感。
- 代码应包含 TSDoc 文档注释。
- 每个文件开头都应添加说明文件用途的注释块。

通过使用指令文件，可以确保 Copilot 获取正确的信息，并按照上述实践完成任务。

### 自定义指令

自定义指令可以向 Copilot 提供上下文和偏好，帮助它了解您的编码风格和要求。您可以指定编码规范、库以及偏好的注释风格，也可以为整个存储库或特定类型的文件创建指令。

指令文件有两种类型：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个指令文件。内容应包括适用于大多数请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建的文件，例如为 TypeScript 或 Astro 编写指南，或为创建 UI 组件和单元测试提供指导。

> [!NOTE]
> 在 IDE 中，指令文件只用于 Copilot Chat 的代码生成，不用于代码补全或下一个编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级指令文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

完整讨论如何创建指令文件超出了本研讨会的范围，但示例项目展示了一种有代表性的方法：

- 将 `copilot-instructions.md` 中的内容限定为项目级指导，例如项目说明、结构和全局编码规范。
- 使用 `*.instructions.md` 为文件类型（如单元测试、Astro 组件和数据层）或特定任务提供专门指令。
- 使用自然语言，保持指导清晰，并提供代码应如何编写或不应如何编写的示例。

创建指令文件没有唯一方法，使用 AI 也是如此。通过实践可以找到适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一套完善的指令文件。浏览本项目的文件时，可能会发现其中有适用于[更新 UI][ui-instructions]和 [Astro][astro-instructions] 等任务的指令。
>
> Copilot 也可以帮助生成指令文件。不同产品界面的操作方式各不相同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。相关学习路径会在适用时介绍具体操作。
>
> 若要查找模板或起点，请浏览 [awesome-copilot][awesome-copilot]，其中包含大量指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义指令文件

花些时间阅读此存储库自带的指令文件：一个核心 `copilot-instructions.md` 文件，以及一组适用于不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下的 **GitHub Actions Workflows** 指导。这些内容适用于 Copilot 在此存储库中的各种交互。
3. 浏览 `.github/instructions` 文件夹，查看其中针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段；它设置相对于存储库根目录的 glob，用于确定适用文件。此处，匹配 `**/*.test.ts` 的 TypeScript 测试文件都会应用这些指令。
5. 查看此项目专门用于创建单元测试的指令。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。查看其中指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成较小、可复用的文件，并为 Copilot 提供参考示例。文件中的路径相对于指令文件所在位置，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目编码标准，但目前没有要求在代码中添加文档。接下来将添加 TSDoc 文档注释和文件注释标题的规则。

## 添加新的存储库规范

接下来只需编辑一处：添加全局项目规则，要求代码中包含 TSDoc 文档注释和文件级注释标题。后续练习中，云智能体处理分配给它的议题时会遵循此规则。

编辑前，先创建一个分支（云智能体会读取议题目标分支中的指令，但仍建议干净地提交这些编辑）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在记录存储库编码标准，改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到约在第 27 行的 **Code formatting requirements** 部分。注意其中记录了项目编码标准，但尚未规定代码文档要求。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释标题和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级文件 `.github/copilot-instructions.md` 中编写全局指导，也可以使用 `*.instructions.md` 为特定语言、文件类型或任务提供指令。项目级文件适合放置刚添加的文档注释等全局规范。

## 提交并推送指令更新

指令文件和存储库中的其他资源一样，也由源代码管理。现在提交并推送分支。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并将其合并。下一练习中，云智能体会在处理议题时读取这些指令。

> [!TIP]
> 如果想在本研讨会中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。此处创建分支是为了让研讨会模拟真实项目的处理方式。

## 总结和后续步骤

您已了解 Copilot 如何从项目指令文件获取上下文，并在 `.github/copilot-instructions.md` 中添加了新的全局存储库规范。后续练习中，可以在云智能体创建的拉取请求中看到该规范的作用。

接下来，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
