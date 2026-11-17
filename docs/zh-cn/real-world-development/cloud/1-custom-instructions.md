---
title: "练习 1 - 自定义说明（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式人工智能 (AI) 时，上下文至关重要。如果任务必须以特定方式完成，或 Copilot 需要了解背景信息，就应确保它能访问这些内容。[说明文件][instruction-files]可提供此类指导，让 Copilot 明白不仅要做什么，也要如何完成。

本练习将：

- 了解项目上下文、编码指南和文档标准如何通过仓库自定义说明及按路径限定的说明文件传递给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加一条新的仓库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 工作环境不同，本练习不会使用前后对比提示。Copilot 云智能体会在 GitHub 议题上异步工作，因此较难实时并排展示说明的影响。后续审查云智能体生成的拉取请求时，可以看到说明文件发挥的作用。

## 说明文件

### 场景

Tailspin Toys 有一套开发指南和要求，包括：

- 数据层始终需要单元测试。
- 用户界面 (UI) 应采用深色模式并具有现代感。
- 代码应添加 TSDoc 文档注释。
- 每个文件开头都应添加说明文件用途的注释块。

通过说明文件，可以确保 Copilot 按照这些实践完成任务。

### 自定义说明

自定义说明可为 Copilot 提供上下文和偏好，帮助它理解编码风格和要求。你可以指定编码规范、库以及注释风格，也可以为整个仓库或特定文件类型创建说明。

说明文件有两种：

- `.github/copilot-instructions.md`：每次针对该仓库的请求都会发送此文件，因此应包含项目级信息，例如技术栈、项目概况、最佳实践和全局指南。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在集成开发环境 (IDE) 中，说明文件只用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用仓库级说明和包含 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 也支持其他标准格式的说明文件，例如 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

本研讨会不深入讨论如何创建说明文件，但示例项目展示了常见做法：

- 将 `copilot-instructions.md` 聚焦于项目级指南，例如项目说明、目录结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（例如单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，保持说明清楚，并提供正反示例。

说明文件没有唯一的正确写法。通过实践找到适合项目的方式。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一套完善的说明文件。你会看到示例项目包含许多任务的说明，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。不同界面的入口不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`；当前工作环境的相关课程会介绍对应操作。
>
> 如需模板或起点，请浏览 [awesome-copilot][awesome-copilot]，其中包含说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义说明文件

先阅读仓库自带的说明文件，包括核心的 `copilot-instructions.md` 和针对不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub 网站中打开。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。留意 **Code standards** 下的 **GitHub Actions Workflows** 指南。这些内容适用于与 Copilot 的各种交互。
3. 浏览 `.github/instructions` 文件夹，查看 Astro、Drizzle 数据层、测试等说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段；它定义相对于仓库根目录的 glob，确定说明适用范围。此处会匹配 TypeScript 测试文件，例如符合 `**/*.test.ts` 的文件。
5. 阅读项目对单元测试的具体要求。
6. 打开 `.github/instructions/drizzle.instructions.md` 并查看文件末尾，留意它指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的说明集拆分成可复用的小文件，并提供代码示例供 Copilot 参考。链接路径相对于说明文件所在目录，而不是仓库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但目前尚未要求在代码中添加文档注释。接下来会添加 TSDoc 注释和文件头注释要求。

## 添加新的仓库标准

此处只需编辑一处：添加项目级规则，要求在代码中使用 TSDoc 文档注释和文件级注释。云智能体稍后处理分配给它的议题时会读取此规则。

先创建分支。云智能体会读取议题目标分支中的说明文件，而独立提交更改也更便于管理。

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。接下来把编码标准写入其中，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（大约在第 27 行）。留意它记录了项目编码标准，但还没有代码文档要求。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件头注释和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 仓库级 `.github/copilot-instructions.md` 适合放置项目通用标准；`*.instructions.md` 则适合提供特定语言、文件类型或任务的说明。刚添加的文档注释规则属于项目级标准。

## 提交、推送并合并说明更新

云智能体会读取议题目标分支中的说明文件。下一练习中，Copilot 会从 `main` 分支创建分支，因此必须先将说明更改合并到 `main`。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求，并合并。之后，云智能体处理任务时就会读取这些说明。

> [!TIP]
> 如果希望直接在 `main` 上操作，可以跳过分支步骤并直接提交。本练习使用分支是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你探索了 Copilot 如何读取项目说明，并在 `.github/copilot-instructions.md` 中添加了仓库级标准。后续审查云智能体生成的拉取请求时，可以看到这条标准的作用。

接下来，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建有效自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
