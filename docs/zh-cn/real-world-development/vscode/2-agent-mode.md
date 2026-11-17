---
title: "练习 2 - 使用 Copilot 代理模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是简单的应用更新，通常也需要修改多个文件并运行测试。开发者需要找到所有相关文件、进行修改、运行测试、排查问题，再检查是否遗漏文件。Copilot 代理模式可以在集成开发环境 (IDE) 中更自主地处理这类工作：先探索项目结构、执行必要更改、运行测试，并尝试自动修复问题。

> [!NOTE]
> 代理模式和云智能体虽然名称相似，但适用于不同场景。代理模式在 IDE 中执行任务，适合快速反馈和交互；云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将介绍如何：

- 使用 Copilot 代理模式探索项目、查找相关文件并协调更改。
- 使用 Copilot 代理模式跨用户界面 (UI) 和数据层实现新功能。
- 合并前审查 Copilot 代理模式生成的更改和测试。

## 场景

游戏列表不断扩大，你希望允许用户按类别和发行商筛选。上一练习已添加发行商辅助函数，现在将使用 Copilot 代理模式完成剩余的数据层、用户界面和测试工作。

## 运行 Tailspin Toys 网站

先浏览 Tailspin Toys 网站，了解现有功能。

该网站是一个以开发者为主题的桌游众筹平台，可查看游戏列表和详情。它是一个 Astro 应用，构建时会将页面渲染为静态超文本标记语言 (HTML)。页面通过 frontmatter 和 Drizzle ORM 直接查询本地 SQLite 数据库，没有独立后端应用程序编程接口 (API) 或客户端用户界面框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能通过少量使用文档对象模型 (DOM) API 的 Astro `<script>` 实现。

### 启动网站

项目提供 `npm run dev` 任务，用于启动 Astro 开发服务器。

1. 返回 Codespace。上一练习结束后应已切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开终端。
3. 更新 `main` 并创建筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，应显示类似以下内容的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出提示，询问是否打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），同时选择 Astro 服务器地址 `http://localhost:4321`。

> [!NOTE]
> 在 Codespace 中选择终端里的 localhost 链接时，系统会自动将其转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是连接到 Codespace 网站服务器的专用通道。

### 浏览网站

网站启动后，可以浏览现有功能：

- **主页**：显示桌游列表、标题、图片和描述。
- **游戏详情页**：选择游戏后查看标题、描述、发行商和类别等详情。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型选择因练习而异；需要时会在相应步骤中说明。

项目已有初始实现，现在查看从模板自动创建的待办议题。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。Agent 可自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写研讨会时使用的是 Claude Sonnet 4.5，因此将其列入步骤，以尽量保持建议的一致性。不过，大语言模型具有概率性，收到不同建议属于正常现象。

> [!NOTE]
> 由于大语言模型 (LLM) 具有概率性，Copilot 可能会调用不同的模型上下文协议 (MCP) 命令，但仍应能完成任务。

3. 使用以下提示，让 Copilot 查看议题积压并协助排序：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 以运行列出所有议题的命令。
5. 审查生成的议题列表。

Copilot 还会根据它对用户价值的判断为议题排序。

## 审查说明文件

生成代码前，先查看将用于提供项目上下文的说明文件。`.github/instructions/ui.instructions.md` 包含网站功能开发指南。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看整体指南，包括：
   - 项目架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向 Astro、Tailwind CSS 和 Drizzle 等特定说明文件的链接。

> [!TIP]
> 说明文件可以引用其他说明文件和项目文件。路径相对于说明文件所在位置，因此可以复用内容、拆分复杂说明，并提供示例和模板。

## 实现筛选功能

完成筛选至少需要以下三项更改：

- 在数据层 (`src/lib/`) 添加或完善筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页面，加入筛选 UI。

合并前还必须运行并通过测试。Copilot 代理模式可以完成这些工作。

1. 继续当前 Copilot 对话，或选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，再选择 `ui` 说明文件。

   ![选择 UI 说明文件的示例](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表中仍选择 **Agent**。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5**。
5. 使用以下提示，让 Copilot 根据待办议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 探索项目、找到数据层、UI 和测试文件，然后修改文件并运行测试。

   ![Copilot 探索项目文件](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会探索项目、修改文件并运行测试。根据任务和代码库复杂度，这可能需要几分钟。工作期间，编辑器中可能会出现 **Keep** 和 **Undo**；完成后，每项更改都会提供 **Keep** 或 **Undo**，无需在处理中途选择。

7. Copilot 提示运行测试时，选择 **Continue**。

   ![Copilot Chat 提示确认运行测试](../../../_images/ex3-agent-mode-run-tests.png)

8. 处理中可能会暂停，也可能看到测试暂时失败。Copilot 会在生成代码和运行测试之间迭代，直到完成任务且不再发现错误。

   ![完整的 Copilot 代理模式会话](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在问题。

> [!CAUTION]
> 始终要审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行的网站，在浏览器中体验新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

本练习中，你使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加了功能。你了解了如何：

- 使用 Copilot 代理模式跨 UI 和数据层实现新功能。
- 让 Copilot 代理模式探索项目、查找相关文件并协调更改。
- 合并前审查 Copilot 代理模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]并创建拉取请求。

### 奖励探索练习 — 实现分页

游戏列表不断扩大后，需要实现分页。使用本练习学到的方法，让 Copilot 更新网站以支持分页。规划提示时，考虑分页方式、是否允许用户选择每页条目数，以及如何提供足够指导。根据需要与 Copilot 迭代，这是使用 Copilot 的正常流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
