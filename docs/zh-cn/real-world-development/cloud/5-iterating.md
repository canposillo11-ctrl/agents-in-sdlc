---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本研讨会中，你让 GitHub Copilot 执行了多个改善用户体验和添加功能的任务：为代码添加文档、实现供设计团队迭代的相关游戏功能，以及实现高对比度和浅色模式切换。现在审查这些更改，必要时向 Copilot 提供反馈，改进结果。

### 场景

加入生成式人工智能 (AI) 并不会改变软件设计和开发运维 (DevOps) 的基本原则。仍然需要审查生成的代码并遵循常规开发运维流程。接下来，在为团队启用审查前，先检查 Copilot 提出的文档、相关游戏功能和无障碍功能更改。

## 安全性与 GitHub Copilot 云智能体

Copilot 云智能体会异步且自主地执行任务，因此有一些安全限制：

- Copilot 对仓库只有读取权限，对用于生成代码的分支才有写入权限。
- 云智能体在 GitHub Actions 中运行，并在独立的临时环境中工作。
- 运行任何 GitHub Actions 工作流前，都需要人工批准。
- 默认情况下，[外部资源访问受限][agent-firewall]，包括模型上下文协议 (MCP) 服务器。

## 审查生成的文档

先通过 GitHub.com 上的标准界面，查看 GitHub Copilot 云智能体创建的第一个拉取请求 (PR)，其中包含代码文档。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器。如果需要，可以[自定义或禁用 Copilot 云智能体防火墙][agent-firewall]。

1. 返回 github.com 上的仓库。
2. 选择 **Pull Requests** 打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会带有 **[WIP]** 标记。此时等待 Copilot 完成，可能需要几分钟。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查更改。

   ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新增的 TSDoc 文档注释和其他文档。具体更改可能不同。检查 TypeScript 规范和文件头注释；这些内容来自练习 2 中查看的自定义说明文件。
6. 确认更改符合要求后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准。
8. 如果有，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流会在 PR 的检查区域中排队运行。检查单个 Astro 应用的项目检查是否通过；这可能需要几分钟。

## 请求 GitHub Copilot 更改

你可以像在团队成员的 PR 中一样，在评论或代码行内评论中提及 Copilot。Copilot 会读取评论并启动新会话来处理请求。由于结果具有非确定性，无法提供必须照搬的提示。可以要求 Copilot：

- 在每个代码文件开头添加简短说明文件用途的注释。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用结构。

1. 在 PR 中添加评论并提及 **@copilot**，请求它修改生成的文档。可以采用上述建议，或提出其他代码文档要求。
2. 选择 **View Session**，观察 Copilot 如何启动新会话并进行更改。
3. 选择 **Back to pull request** 返回 PR。
4. Copilot 完成更改后，PR 中应出现新的提交。
5. 选择 **Files changed** 选项卡并审查更改。

可以继续迭代，直到满意为止。完成后，可将草稿 PR 转为可审查状态并合并到 main 分支。

![将 PR 转为可审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

接下来审查 Copilot 为“在游戏详情页显示相关游戏”议题创建的 PR。

1. 返回 GitHub.com 上的仓库。
2. 选择 **Pull Requests** 选项卡。
3. 打开标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 确认更改符合要求后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流等待批准。
7. 如果有，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

8. 检查 PR 的检查区域，确认单个 Astro 应用的项目检查通过。此过程可能需要几分钟。
9. **可选：**在 Codespace 中切换到该分支，手动测试相关游戏功能。打开终端，将 `<branch-name>` 替换为 Copilot 创建的分支名（例如 **copilot/fix-8**），然后运行：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot 已实现相关游戏功能。你可以像之前一样通过评论请求云智能体继续迭代，例如调整显示的相关游戏数量，或补充文件头注释和 TSDoc 文档注释。注意，此任务是在自定义说明更新之前分配的。可在 **Conversation** 选项卡中发表评论，Copilot 会启动新会话处理请求。

## 审查无障碍功能

最后，审查由无障碍自定义智能体实现的功能。该 PR 应同时包含练习 3 中要求的高对比度模式，以及练习 4 中通过任务控制中心追加的浅色模式。

1. 返回 GitHub.com 上的仓库。
2. 选择 **Pull Requests** 选项卡。
3. 打开标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会带有 **[WIP]** 标记。等待它完成，这可能需要几分钟。

4. 选择 **Files changed** 选项卡审查生成的代码。
5. 检查实现，尤其留意：
   - 用于切换模式的用户界面 (UI) 组件。
   - 用于保存用户偏好的本地存储。
   - 高对比度和浅色模式的层叠样式表 (CSS) 或样式更改。
   - 无障碍属性，例如无障碍富互联网应用 (ARIA) 标签和键盘导航。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 确认更改符合要求后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准。
8. 如果有，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 检查 PR 的检查区域，确认单个 Astro 应用的项目检查通过。此过程可能需要几分钟。
10. **可选：**在 Codespace 中切换到该分支，手动测试无障碍功能。打开终端，将 `<branch-name>` 替换为 Copilot 创建的分支名，然后运行：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   随后启动应用，在浏览器中测试高对比度和浅色模式切换，并确认设置在重新加载页面后仍然保留。

无障碍自定义智能体帮助 Copilot 按照无障碍最佳实践实现功能。如果发现问题或改进点，可以像处理前面的 PR 一样在评论中提及 **@copilot** 并请求修改。

## 可选练习 — 继续委派任务

云智能体适合处理真实待办事项，让你可以把注意力转移到其他工作。继续练习时，可在仓库中创建更多议题并分配给 Copilot，例如：

- 在游戏详情页创建支持者意向表单。
- 为游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体工作环境的练习。在这些课程中，你：

- **检查了仓库自带的自定义说明文件**，以便之后观察它们对云智能体输出的影响。
- **将议题分配给 Copilot 云智能体**，并观察它设置环境、制定计划和异步执行任务。
- **创建并使用自定义智能体**处理无障碍工作，实现高对比度和浅色模式切换。
- **使用 Copilot 智能体页面作为任务控制中心**，监控并指导进行中的无障碍会话。
- **审查并迭代了云智能体创建的拉取请求**，通过提及 `@copilot` 请求更改并批准工作流。

## 回顾和后续步骤

你已完成云智能体工作环境的练习。还可以继续探索其他工作环境：

- 🖥️ **[VS Code 工作环境](../../vscode/)** — 在集成开发环境 (IDE) 中体验 Copilot Chat 代理模式和 MCP 集成。
- 💻 **[CLI 工作环境](../../cli/)** — 在终端中交付两项经过审查的更改，体验 Plan 和 Autopilot 模式、智能体技能、质量保证 (QA) 自定义智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体工作流。

也可以在自己的仓库中尝试以下后续任务：

- 将重构或测试覆盖率议题分配给云智能体。
- 为其他领域创建自定义智能体。
- 为其他技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
