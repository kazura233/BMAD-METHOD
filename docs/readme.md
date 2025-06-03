# 扩展文档

如果您来到这里但还没有设置 Web Agent - 强烈建议您先设置 Web Agent 并与它对话，这比阅读这些令人昏昏欲睡的文档要容易得多。

## IDE 项目快速启动

将项目克隆到本地机器后，您可以将 `bmad-agent` 文件夹复制到项目根目录。这将放置模板、清单和其他资源，本地 Agent 将需要这些来使用 IDE 中的 Agent，而不是 Web Agent。要构建项目，您至少需要 sm.ide.md 和 dev.ide.md，这样您就可以逐步起草和构建项目。

这里是 [IDE、WEB 和任务设置的更多设置和使用说明](./instruction.md)。

从最新版本的 BMad Agent 开始使用 BMad 方法非常简单 - 您只需要将 `bmad-agent` 文件夹复制到您的项目中。之前版本中存在的专用 dev 和 sm 仍然可用，它们位于 `bmad-agent/personas` 文件夹中，扩展名为 .ide.md。将内容复制并粘贴到您特定 IDE 的配置自定义 Agent 模式的方法中。dev 和 sm 都配置为在 (project-root)/docs 中生成架构和 prd 工件，故事将在/从您的 (project-root)/docs/stories 中生成和开发。

对于所有其他 Agent 使用（包括 dev 和 sm），您可以设置 [ide orchestrator](../bmad-agent/ide-bmad-orchestrator.md) - 您可以要求 orchestrator bmad 成为您已[配置](../bmad-agent/ide-bmad-orchestrator.cfg.md)的任何 Agent。

[通用 IDE 自定义模式设置](../docs/ide-setup.md)。

## 推进 AI 驱动开发

欢迎使用最新且最先进但易于使用的 Web 和 IDE Agent 敏捷工作流！这个新版本，称为 BMad Agent V3 版本，代表了一个重要的演进，它建立在之前版本的基础上。

## 有什么新功能？

所有 IDE Agent 现在都优化为不超过 6K 字符，因此它们可以在 windsurf 的文件限制限制下工作。

该方法现在有一个超级 Orchestrator，称为 BMAD - 这个 Agent 将把您的 Web 或 IDE 使用提升到新的水平 - 这个 Agent 可以变形并成为您想要使用的特定 Agent！这使得 Web 使用变得超级简单易用。在 IDE 中 - 如果您不想，就不必设置这么多不同的 Agent！

文档和工件的生成有了巨大的改进，Agent 现在被编程为真正帮助您制定最佳计划。先进的 LLM 提示技术已被纳入并编程，以帮助您帮助 Agent 生成令人惊叹的准确工件，这是前所未有的。此外，Agent 现在可以在它们能做什么和不能做什么方面进行配置 - 所以您可以接受默认值，或设置哪些角色能够执行哪些任务。如果您认为 PO 应该是生成 PRD 的人，而 Scrum Master 应该是您的课程纠正者 - 现在这一切都成为可能！**用 BMad 方式定义敏捷 - 或者您的方式！**

虽然这非常强大 - 但您可以从这个仓库中的默认推荐设置开始，基本上按照预期使用 Agent，并将进行解释。详细配置和使用在[说明](./instruction.md)中概述。

## 什么是 BMad 方法？

BMad 方法是一种革命性的方法，它将"氛围编码"提升到高级项目规划，以确保您的开发 Agent 能够在非常明确的指导下开始和完成高级项目。它提供了一个结构化但灵活的框架，使用专门的 AI Agent 团队来规划、执行和管理软件项目。

这个方法和工具不仅仅是任务运行器 - 这是一个精炼的工具，它将帮助您发挥最佳想法，定义您真正要构建的内容，并执行它！从构思，到 PRD 创建，到技术决策 - 这将帮助您利用高级 LLM 指导的力量完成所有工作。

该方法在设计上对工具保持中立，Agent 指令和工作流可以适应各种 AI 平台和 IDE。

## 敏捷 Agent

Agent 要么直接自包含，可以直接放入 IDE 中的 Agent 配置中 - 或者它们可以配置为 orchestrating Agent 可以成为的可编程实体。

### Web Agent

Gemini 2.5 或 Open AI customGPTs 通过运行 node 构建脚本来生成输出到构建文件夹。这个输出是创建 orchestrator web agent 的完整包。

查看详细的 [Web Orchestration 设置和使用说明](./instruction.md#setting-up-web-agent-orchestrator)

### IDE Agent

有专门的自包含 Agent，它们可以独立运行，也有 IDE 版本的 orchestrator。对于独立的，有：

- [Dev IDE Agent](../bmad-agent/personas/dev.ide.md)
- [故事生成 SM Agent](../bmad-agent/personas/sm.ide.md)

如果您想使用其他 Agent，您可以使用该文件夹中的其他 Agent - 但有些会大于 Windsurf 允许的大小 - 而且有很多 Agent。所以建议要么使用一次性任务 - 或者更好 - 使用 IDE Orchestrator Agent。查看这些 [IDE Orchestrator 的设置和使用说明](./instruction.md#ide-agent-setup-and-usage)。

## 任务

位于 `bmad-agent/tasks/` 中，这些自包含的指令集允许 IDE Agent 或 orchestrators 配置的 Agent 执行特定工作。这些也可以作为一次性命令与 IDE 中的普通 Agent 一起使用，只需引用任务并要求 Agent 执行它。

**目的：**

- **减少 Agent 膨胀：** 避免将很少使用的指令添加到主要 Agent 中。
- **按需功能：** 通过提供任务文件内容，指示任何有能力的 IDE Agent 执行任务。
- **多功能性：** 处理特定功能，如运行清单、创建故事、分片文档、索引库等。

将任务视为可由您的主要 IDE Agent 调用的专业迷你 Agent。
