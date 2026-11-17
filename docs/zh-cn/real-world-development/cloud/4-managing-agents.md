---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前两项练习中，你让 Copilot 云智能体处理了三项改善用户体验和增加功能的任务。云智能体虽然会异步自主工作，但监控任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心，可以查看所有带有未关闭拉取请求 (PR) 的智能体任务、检查已执行的操作，并在会话进行中提供指导。

本练习将：

- 探索智能体页面，监控云智能体任务。
- 在会话进行中提出要求，增加功能。

## 场景

将高对比度模式任务分配给智能体后，团队决定同时添加浅色模式。由于智能体已经在更新网站样式并添加切换功能，可以将浅色模式一并纳入。你将指导智能体，让它同时实现浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的任务状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主区域和左侧区域中的任务列表，其中应包括：
   - 为代码库补充文档。
   - 生成修改游戏的应用程序编程接口 (API)。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从仓库检出代码。
   - 创建 Copilot 工作环境。
   - 设置模型上下文协议 (MCP) 服务器。
   - 执行完成任务所需的其他步骤。

> [!NOTE]
> 显示的具体步骤取决于 Copilot 的工作进度和采用的方案。

4. 查看右侧显示的 PR 面板，以监控拉取请求和更改的文件。

## 指导云智能体

了解当前任务后，在智能体处理高对比度模式期间，要求它同时添加浅色模式。

1. 选择负责添加高对比度模式的会话。会话标题取决于 Copilot 使用的名称和当前工作状态。

   ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到设置完成并开始执行任务。此时会出现类似下方的消息。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![智能体页面中突出显示 Steer active session while Copilot is working 对话框的任务视图](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 观察 Copilot 确认收到提示并将其纳入工作流程。

## 让 Copilot 继续工作

Copilot 会继续处理更新后的任务。完成发送提示时正在执行的步骤后，它会将新要求纳入工作流程。

这同样需要几分钟。等待期间，可以回顾目前学到的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，这是监控和指导 GitHub Copilot 云智能体任务的中心位置。你可以跟踪正在进行和已完成的任务、审查工作内容，并重定向进行中的任务以调整范围或补充说明。

本课中，你：

- 使用任务控制中心和智能体页面监控了云智能体任务。
- 重定向了进行中的会话，要求添加功能。

Copilot 正在完成无障碍功能。接下来[迭代 Copilot 创建的拉取请求][next-lesson]。

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
