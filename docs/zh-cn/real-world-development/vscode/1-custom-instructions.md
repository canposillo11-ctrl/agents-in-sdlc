---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式人工智能 (AI) 时，上下文至关重要。说明文件可以向 Copilot 提供特定项目上下文，指导它如何生成代码。

本练习将：

- 了解项目上下文、编码指南和文档标准如何通过仓库自定义说明及按路径限定的说明文件传递给 Copilot。
- 在现有说明下使用 Copilot 生成筛选功能的数据层基础（发行商辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加仓库级标准，再次运行相同提示，观察生成代码如何应用新标准。
- 将说明更新和筛选功能基础提交到 `main`，供下一练习使用。

> [!CAUTION]
> 生成的代码可能不完全遵循设置的标准，因为 Copilot 具有非确定性。本练习旨在观察更新说明后的行为趋势，而不是要求输出逐字一致。

## 说明文件

### 场景

Tailspin Toys 有一套开发指南和要求，包括：

- 数据层始终需要单元测试。
- 用户界面应采用深色模式并具有现代感。
- 代码应添加 TSDoc 文档注释。
- 每个文件开头都应添加说明文件用途的注释块。

通过说明文件，可以确保 Copilot 按这些实践执行任务。

### 自定义说明

自定义说明可为 Copilot 提供上下文和偏好，帮助它理解编码风格和要求。说明文件可以指定编码规范、库以及注释风格，也可以为整个仓库或特定文件类型创建说明。

- `.github/copilot-instructions.md`：每次针对该仓库的请求都会发送此文件，适合存放项目级信息，例如技术栈、项目概况、最佳实践和全局指南。
- `.github/instructions/*.instructions.md`：为特定任务或文件类型提供指南，例如 TypeScript、Astro、用户界面 (UI) 组件或单元测试。

> [!NOTE]
> 在集成开发环境 (IDE) 中，说明文件只用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用仓库级说明和包含 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 也支持其他标准格式的说明文件，例如 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

- 将 `copilot-instructions.md` 聚焦于项目级指南，例如项目说明、目录结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（例如单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，保持说明清楚，并提供正反示例。

说明文件没有唯一正确的写法。通过实践找到适合项目的方式。

> [!TIP]
> Tailspin Toys 项目包含多种任务的说明，例如[用户界面更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。不同界面的入口不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。
>
> 如需模板或起点，请浏览包含说明文件、自定义智能体和其他资源的 [awesome-copilot][awesome-copilot]。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义说明文件

先阅读仓库自带的说明文件，包括核心的 `copilot-instructions.md` 和针对不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub 网站中打开。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。留意 **Code standards** 下的 **GitHub Actions Workflows** 指南。
3. 浏览 `.github/instructions` 文件夹，查看 Astro、Drizzle 数据层、测试等说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段；它定义相对于仓库根目录的 glob，此处会匹配 TypeScript 测试文件，例如 `**/*.test.ts`。
5. 阅读项目对单元测试的具体要求。
6. 打开 `.github/instructions/drizzle.instructions.md` 并查看文件末尾，留意它指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。路径相对于说明文件所在目录。这样可以将较大的说明集拆分成可复用的小文件，并提供代码示例供 Copilot 参考。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但目前尚未要求在代码中添加文档。接下来会添加 TSDoc 注释和文件头注释要求。

## 创建更改分支

为这些更改创建分支。

1. 返回上一练习中的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型选择因练习而异；需要时会在相应步骤中说明。

先在当前说明下发送提示，之后更新说明并再次发送相同提示，比较结果。

1. 关闭之前练习留下的编辑器选项卡，避免 Copilot 获取无关上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，以便 Copilot 修改文件。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并修改代码，通常包括辅助函数和测试。
6. 留意辅助函数是否为类型化函数，以 `db` 客户端为第一个参数，并返回类型化的发行商数组。这些约定来自适用于 `src/lib/*.ts` 的 `.github/instructions/drizzle.instructions.md`。
7. 留意生成的代码是否缺少 TSDoc 文档注释和文件级注释。

> [!CAUTION]
> Copilot 具有非确定性，因此即使尚未添加说明，它也可能生成文档注释。这没有问题；本练习重点是观察更新说明后行为的一致性改进。

## 添加新的仓库标准

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。接下来把编码标准写入其中，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（大约在第 27 行），并留意其中还没有代码文档要求。
3. 在现有标准下方添加以下内容，要求 Copilot 添加文件头注释和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 仓库级 `.github/copilot-instructions.md` 适合放置项目通用标准；`*.instructions.md` 则适合提供特定语言、文件类型或任务的说明。刚添加的文档注释规则属于项目级标准。

## 重新运行提示并观察变化

使用相同提示，让 Copilot 根据更新后的说明再次生成建议。

1. 返回 Copilot Chat 并选择 **New Chat**，清空对话上下文。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦正确文件。
3. 发送与之前**完全相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 查看生成的文件是否以类似以下注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 查看函数是否包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你已引导 Copilot 遵循新的项目标准，并将其应用到真实代码中；下一练习会继续使用这段代码。

## 提交说明并推送分支

说明文件与仓库中的其他资源一样，也需要通过源代码管理。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开终端。
2. 运行以下命令，确认说明更新和辅助函数更改均已存在：

   ```bash
   git status
   ```

3. 暂存说明更新及 Copilot 为辅助函数基础所做的所有更改，然后提交：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到仓库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，创建拉取请求 (PR) 并关联待办事项中的文档标准议题。Copilot 可以通过已连接的 GitHub 工具查找议题并创建 PR。

> [!NOTE]
> 这是首次让 Copilot 代表你操作 GitHub。项目模板已配置 GitHub 工具，无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按提示授权即可。后续练习会介绍这种连接所用的模型上下文协议 (MCP)。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认智能体选择器中已选择 **Agent**。
3. 要求 Copilot 找到与说明文件相关的议题，并从当前分支创建 PR，说明其中既有说明更新，也有新的发行商辅助函数基础：

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会查找议题并创建 PR。
5. Copilot 请求代表你执行 GitHub 操作时，检查命令并酌情选择 **Allow**。
6. PR 创建后，使用以下提示要求 Copilot 合并 PR 并将本地分支切回 main：

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot 请求执行 GitHub 操作或 shell 命令时，检查命令并酌情选择 **Allow**。

你已借助 GitHub Copilot 创建并合并拉取请求。

## 总结和后续步骤

你了解了 Copilot 如何读取项目说明，并在 VS Code 的 Copilot Chat 中：

- 发送代码生成提示，观察 Copilot 在现有说明下的输出。
- 在 `.github/copilot-instructions.md` 中添加仓库级标准。
- 再次发送相同提示，观察生成代码如何应用新标准。
- 将说明更新和辅助函数基础提交到 `main`，供下一练习使用。

接下来，在[代理模式][next-lesson]中应用这些说明，让 Copilot 跨代码库添加功能。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建有效自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot：说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
