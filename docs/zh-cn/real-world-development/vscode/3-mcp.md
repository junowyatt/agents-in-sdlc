---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：代理模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

您刚刚使用代理模式构建了筛选功能。创建拉取请求前，应先在浏览器中确认功能确实正常。无需自己逐一操作应用，可以连接 **Playwright MCP 服务器**，让 Copilot 操作真实浏览器为您测试功能，然后发布分支并创建 PR。

本练习将学习如何：

- 了解模型上下文协议 (MCP) 以及 MCP 服务器如何使用新工具扩展 Copilot。
- 向工作区添加 Playwright MCP 服务器。
- 让 Copilot 使用该服务器在浏览器中手动测试筛选功能。
- 发布分支并为筛选工作创建拉取请求。

## 什么是模型上下文协议 (MCP)？

当代理模式可以访问编辑器以外的工具时，其能力会大幅增强。Copilot 使用模型上下文协议 (MCP) 与这些工具交互，这是智能体连接外部工具和服务的一种标准方式。

![示意图显示代理模式如何与上下文、LLM 和工具交互，包括由 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议 (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)为 AI 智能体与外部工具和服务通信提供了一种方式。借助 MCP，AI 智能体可以实时连接外部工具和服务，通过资源获取最新信息，并使用工具代表您执行操作。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁，并负责管理二者之间的通信，例如连接现有 API 或 NPM 包等本地工具。每个 MCP 服务器代表一组智能体可以访问的工具和资源。

以下是几个常见的 MCP 服务器：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 存储库的 API，允许 AI 智能体创建存储库、更新现有存储库以及管理议题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化能力，允许 AI 智能体访问网页、填写表单和选择按钮。

还有许多其他 MCP 服务器，可用于访问不同的工具和资源。GitHub 提供 [MCP 注册表](https://github.com/mcp)，帮助用户发现并为生态系统做出贡献。

> [!CAUTION]
> 出于安全考虑，请像对待项目中的其他依赖项一样对待 MCP 服务器。使用前，请仔细审查其源代码、验证发布者并考虑安全影响。只使用信任的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置此 VS Code 工作区可用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 其中应已配置 `github` 服务器：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

项目模板预先配置了 `github` 项，因此上一练习中 Copilot 可以读取议题待办事项。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需本地安装；VS Code 会通过 HTTP 连接，并在 Copilot 首次使用其工具时要求您向 GitHub 进行身份验证。

## 添加 Playwright MCP 服务器

现在添加第二台服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控的浏览器，正好可以用于测试功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 项，与 `github` 并列。文件内容如下：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会通过运行 `npx @playwright/mcp@latest` 在您的计算机上启动它。`--headless` 标记要求 Playwright 在后台运行浏览器，不显示窗口。这是 codespace 的必要设置，因为其中没有桌面环境可显示窗口。

> [!NOTE]
> Tailspin Toys 项目已在端到端测试中使用 Playwright，通常已经安装了所需浏览器。如果 Copilot 报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器：Copilot 首次需要服务器工具时，VS Code 会启动服务器并询问您是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 项上方会显示 **Start** 操作。选择该操作以立即启动服务器并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请检查配置并选择信任，使服务器可以启动。

手动启动是可选的。如果跳过此步骤，下一步中 Copilot 首次需要服务器工具时，VS Code 会启动服务器并询问您是否信任它。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供真实浏览器。无需亲自点击应用检查工作，智能体可以打开页面、导航、应用筛选并读取结果，然后向您总结观察结果。这样无需离开对话即可快速确认功能是否符合预期。

Playwright MCP 服务器会根据页面的[无障碍树][playwright-mcp-server]工作，而不是根据屏幕截图。智能体会分析结构化且带标签的元素（按钮、链接和列表项），与辅助技术使用的方式相同，因此快速功能检查也能顺便验证一些基础无障碍情况。

连接服务器并启动应用后，发送以下提示，让 Copilot 测试刚刚构建的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器、逐步完成检查，并报告结果。根据议题中的验收标准审查报告。如果发现问题，可以继续询问，或要求 Copilot 在创建 PR 前修复代码。

> [!NOTE]
> 测试时应用必须运行在 `http://localhost:4321`。如果开发服务器已停止，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 服务器时可能需要下载浏览器；如果报告缺少浏览器，请让它运行 `npx playwright install chromium` 后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，可以创建拉取请求 (PR)，让团队审查。先发布 `filtering-vscode` 分支。

1. 在 codespace 中打开 **Source Control** 面板，审查 Copilot 进行的更改。
2. 选择 **+** 图标暂存更改。
3. 选择 **Sparkle** 按钮生成提交消息。

   ![显示 Copilot 所做更改的 Source Control 面板截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

可以通过 github.com 或 GitHub 命令行界面 (CLI) 等方式创建拉取请求。也可以让 Copilot 创建 PR，因为它能够找到相关议题并将 PR 与议题关联。

1. 打开 Copilot Chat 面板并选择 **New Chat**，开始新会话。
2. 在智能体下拉列表中确认选择 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 使用以下提示让 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 按需选择 **Continue**，允许 Copilot 获取信息并执行操作。首次使用 GitHub 工具时，可能会提示您向 GitHub 进行身份验证；按照提示授权即可。
5. 查看 Copilot 如何搜索议题、找到正确议题并创建 PR。
6. 选择 Copilot 生成的链接审查 PR，但**暂时不要合并**。

## 总结和后续步骤

恭喜！本练习中，您：

- 了解了模型上下文协议 (MCP) 以及 MCP 服务器如何使用新工具扩展 Copilot。
- 向工作区添加了 Playwright MCP 服务器。
- 在发布前使用它在浏览器中手动测试筛选功能。
- 发布分支并为筛选工作创建拉取请求。

您使用 MCP 服务器测试了一项功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义智能体][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
