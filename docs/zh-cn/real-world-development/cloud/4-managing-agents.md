---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几项练习中，您让 Copilot 云智能体处理了三个不同任务，以改进用户体验并添加功能。云智能体虽以异步、自主方式运行，但监控这些任务仍然很重要。

您可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心，可以查看所有创建了未关闭拉取请求 (PR) 的智能体任务，探索执行过的操作，甚至可以指导正在进行的会话。

本课将学习如何：

- 使用智能体页面监控云智能体任务。
- 指导正在进行的会话，要求添加其他功能。

## 场景

团队要求添加高对比度模式后，意识到也可以同时添加浅色模式。由于网站样式和切换功能都在更新，因此一并实现浅色模式合乎情理。您希望指导智能体，确保它同时添加浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的所有任务的状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。列表中应包含分配给 Copilot 的任务，例如：
   - 更新代码文档。
   - 生成修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 执行的操作，例如：
   - 从存储库检出代码。
   - 创建 Copilot 的工作环境。
   - 设置 MCP 服务器。
   - 执行完成分配任务所需的各种步骤。

> [!NOTE]
> 显示的具体步骤会根据 Copilot 的工作状态和采用的方法而变化。

4. 还可以查看右侧显示的 PR 窗格，以便进一步监控 PR 和更改的文件。

## 指导云智能体

现在您已了解当前任务的状态，接下来要求 Copilot 在实现高对比度模式时也添加浅色模式切换开关。

1. 选择与添加高对比度模式相关的会话。确切标题会根据 Copilot 使用的名称和当前工作状态而异。

   ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 监控会话几分钟，直到会话显示已完成环境设置并开始工作。看到类似下方的消息时，就表示已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中，添加以下提示：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![智能体页面上的云智能体任务截图，其中突出显示了“Steer active session while Copilot is working”对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 会确认收到提示，并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务。它会在完成发送消息时正在执行的步骤后，将新请求加入工作流程。

这同样需要几分钟，您可以借此机会回顾目前学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，这是监控和指导 GitHub Copilot 云智能体任务的中心位置。在此任务控制中心，可以跟踪所有活动和已完成的任务、审查正在执行的工作，甚至可以重定向正在运行的任务以调整范围或提供其他指导。

本课介绍了以下概念：

- 使用任务控制中心和智能体页面监控云智能体任务。
- 重定向正在进行的会话，要求添加其他功能。

Copilot 正在完成无障碍功能，接下来[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
