---
title: "第 4 课 - 使用自定义说明引导 Copilot"
description: "探索仓库说明、添加文档标准，并将其应用于筛选功能代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，应确保 Copilot 能获取相应指导。[说明文件][instruction-files]不仅描述想要什么代码，也说明代码应如何组织。完成筛选功能后，现在可以了解 Copilot 使用的说明、添加文档标准，并将其应用于代码。

在本课中，你将：

- 了解仓库说明和按路径限定的说明文件如何传递给智能体。
- 更新说明文件，确保遵循编码标准。
- 观察说明文件对代码的影响。

## 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值；如果有可注入的 `db` 参数，也要记录。
- 可复用 Astro 组件应说明其 `Props` 契约；相关代码更改时应同步更新注释。
- 保留现有格式和 lint 指南。

通过说明文件，可以让 Copilot 按这些实践执行任务。

## 说明文件

自定义说明可为 Copilot 提供上下文和偏好，帮助它理解编码风格和要求。说明文件可以指定编码规范、库以及注释风格。你可以为整个仓库创建说明，也可以针对特定文件类型创建说明。

说明文件有两种：

- `.github/copilot-instructions.md`：每次针对该仓库的请求都会发送此单一说明文件，其中应包含适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指南。

> [!NOTE]
> 其他说明格式及其支持情况因环境而异。依赖特定格式前，请查阅[自定义说明支持参考][custom-instructions-support]。

## 探索项目中的自定义说明文件

入门项目已包含一组说明文件。先检查现有内容，再修改文件，以了解其影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而不是终端）中打开 `.github/copilot-instructions.md`。
3. 查看文件中的项目简介和编码指南。这些说明适用于 Copilot 在此仓库中的每次交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件，包括 Astro、Drizzle 数据层和测试等说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段；该 glob 用于确定说明适用的文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看其中指向其他说明文件和项目现有文件的链接。这样可以将较大的说明集拆分成可复用的小文件，并提供可参考的示例。

## 更新说明文件以匹配团队指南

现有文件已经提供了良好起点，但还缺少一项要求。修改核心 `copilot-instructions.md`，确保新生成的 TypeScript 遵循文档标准。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分（大约在第 35 行）。
2. 在该部分的最后添加以下条目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 应用更新后的指南

Copilot CLI 会在对话启动时加载仓库说明。保存更改后，退出并恢复筛选功能对话，让 Copilot 在保留功能上下文的同时加载新指南。

1. 如果筛选功能对话仍处于打开状态，输入 `/exit` 退出。Copilot CLI 会自动保存对话。
2. 在仓库根目录的 `game-filters-cli` 分支上恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，选择筛选功能对话，并确认检出的是 `game-filters-cli` 分支。
3. 要求 Copilot 应用更新后的指南：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，检查更改的 TypeScript 文件。确认新生成的 TSDoc 注释准确说明代码。

## 总结和后续步骤

你探索了 Copilot CLI 如何从说明文件获取上下文，并将新的标准应用于功能代码。具体来说，你：

- 探索了仓库说明和按路径限定的说明文件如何传递给智能体。
- 更新了说明文件，确保遵循编码标准。
- 了解了说明文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保以一致的方式运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建有效自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot：说明文件及其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
