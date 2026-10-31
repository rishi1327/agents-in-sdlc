---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前两项练习中，你让 Copilot 云智能体执行了三项任务，以改善用户体验并添加功能。云智能体能够异步自主工作，但监控这些任务仍然很重要。

GitHub 提供了多种工具来管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在此任务控制中心，你可以查看所有具有未关闭拉取请求 (PR) 的智能体任务，了解已执行的操作，还可以引导正在运行的会话。

本课将介绍如何：

- 使用智能体页面监控云智能体任务。
- 在会话运行期间引导智能体添加其他功能。

## 场景

将任务分配给智能体以实现高对比度模式后，团队意识到也可以顺便添加浅色模式。网站正在更新样式并添加切换功能，因此同时实现这项功能很合理。现在引导智能体，让它在添加高对比度模式的同时也实现浅色模式。

## 查看 Copilot 云智能体任务

查看分配给 Copilot 云智能体的所有任务的当前状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。列表中应包含你分配给 Copilot 的任务，例如：
   - 更新代码文档。
   - 生成修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码。
   - 创建 Copilot 工作环境。
   - 设置模型上下文协议 (MCP) 服务器。
   - 执行完成分配任务所需的其他步骤。

> [!NOTE]
> 显示的具体步骤会根据 Copilot 当前的工作状态和执行方式而变化。

4. 同时查看右侧显示的 PR 面板，以便查看 PR 和更改的文件并进一步监控任务。

## 引导云智能体

了解当前运行的任务后，要求 Copilot 在实现高对比度模式时也添加浅色模式。

1. 选择高对比度模式任务对应的会话。确切标题取决于 Copilot 使用的名称和当前任务状态。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 继续监视会话几分钟，直到会话显示已完成设置并开始执行任务。此时会出现类似下方的消息。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示：

    ```plaintext
    在实现高对比度模式的同时，也添加浅色模式。浅色模式也应有一个开关，让用户选择所需的显示模式。
    ```

    ![智能体页面上的云智能体任务，其中突出显示了引导正在运行会话的对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 如何确认收到提示并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务。完成你发送消息时正在执行的具体步骤后，它会将新请求加入工作流程。

这同样需要几分钟。等待期间，可以回顾和思考目前学到和探索的内容。

## 总结与后续步骤

本课介绍了 Copilot 智能体页面，这是监控和引导 GitHub Copilot 云智能体任务的中心位置。在此任务控制中心中，你可以跟踪所有正在进行和已完成的任务、审查已执行的工作，也可以重定向正在运行的任务以调整范围或提供额外指导。

你探索了以下概念：

- 使用任务控制中心和智能体页面监控云智能体任务。
- 重定向正在运行的会话以请求添加其他功能。

Copilot 正在完成无障碍功能。接下来[迭代改进 Copilot 创建的拉取请求][next-lesson]。

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
