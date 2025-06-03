# IDE Agent 配置说明

Uber Orchestrating BMad Agent 主要推荐在 Gemini Web 中使用，特别是用于处理简报、PRD、高级史诗、故事、Web 设计和提示输出。但是 - 如果需要，所有内容也可以在 IDE 中完成，请参见下面的 BMad Agent 设置部分。

## 单个 Agent

按照文档创建自定义模式，并从 personas 文件夹中粘贴任何以 .ide.md 结尾的 agent。

## 任务

由于 cursor 目前限制了允许的自定义模式总数 - 您可以使用任务来处理您可能希望 agent 执行的一次性操作。只需将任务拖到任何 agent 聊天窗口中，并要求 agent 完成任务。

## BMad Agent

BMad Agent 需要完整的 bmad agent 文件夹位于项目根目录。设置 orchestrator 只需要以与单个 Agent 相同的方式复制 ide-bmad-orchestrator.md 的 markdown 内容。

## 在 Cursor 中设置自定义模式

要使用自定义 agent 模式 - 请查看此处的文档：https://docs.cursor.com/chat/custom-modes。

- 具体来说，您需要在以下位置启用自定义模式：设置 → 功能 → 聊天 → 自定义模式
- 可以通过 GUI 界面创建和配置具有特定工具、模型和自定义提示的自定义 Agent
- Cursor 允许通过 GUI 界面创建自定义 Agent

来自 Cursor 的注意："我们正在考虑添加 .cursor/modes.json 文件到您的项目中，以使创建和共享自定义模式更容易。"

## Windsurf

### 在 Windsurf 中设置自定义模式

1. **访问 Agent 配置**：

   - 点击右下角的"Windsurf - 设置"按钮
   - 通过设置面板中的按钮或右上角个人资料下拉菜单访问高级设置

2. **配置自定义规则**：

   - 为 Cascade（Windsurf 的 agentic 聊天机器人）定义自定义 AI 规则
   - 指定 agent 应该以某些方式响应，使用特定框架，或遵循特定 API

3. **使用流程**：

   - 流程将 Agent 和 Copilot 组合在一起，形成全面的工作流
   - Windsurf 编辑器专为可以独立处理复杂任务的 AI agent 设计
   - 使用模型上下文协议（MCP）扩展 agent 功能

4. **BMAD 方法实现**：
   - 为 BMAD 工作流中的每个角色创建自定义 agent
   - 为每个 agent 配置适当的权限和能力
   - 利用 Windsurf 的 agentic 功能保持工作流连续性

## RooCode

### 在 RooCode 中设置自定义 Agent

1. **自定义模式配置**：

   - 通过配置文件创建定制的 AI 行为
   - 每个自定义模式可以有特定的提示、文件限制和自动批准设置

2. **创建 BMAD 方法 Agent**：

   - 为每个 BMAD 角色（分析师、PM、架构师、设计架构师、PO、SM、Dev 等）创建不同的模式
   - 为每个模式定制特定于其角色的提示
   - 为每个角色配置适当的文件限制（例如，架构师和 PM 模式可能编辑 markdown 文件）
   - 设置直接模式切换，以便 agent 在需要时可以请求切换到其他模式

3. **模型配置**：

   - 为每个模式配置不同的模型（例如，架构使用高级模型，日常编码任务使用更便宜的模型）
   - RooCode 支持多个 API 提供商，包括 OpenRouter、Anthropic、OpenAI、Google Gemini、AWS Bedrock、Azure 和本地模型

4. **使用跟踪**：
   - 监控每个会话的令牌和成本使用情况
   - 根据任务复杂性优化模型选择

## Cline

### 在 Cline 中设置自定义 Agent

1. **自定义指令**：

   - 通过 Cline > 设置 > 自定义指令访问
   - 为您的 agent 提供行为指南

2. **自定义工具集成**：

   - Cline 可以通过模型上下文协议（MCP）扩展功能
   - 要求 Cline"添加工具"，它将创建一个针对您特定工作流的新 MCP 服务器
   - 自定义工具保存在 ~/Documents/Cline/MCP 本地，便于与团队共享

3. **BMAD 方法实现**：

   - 为 BMAD 工作流中的每个角色创建自定义工具
   - 配置特定于每个角色的行为指南
   - 利用 Cline 的自主能力处理整个工作流

4. **模型选择**：
   - 根据角色和任务复杂性配置 Cline 使用不同的模型

## GitHub Copilot

### 自定义 Agent 配置（即将推出）

https://github.com/microsoft/vscode-copilot-release/issues/9452

GitHub Copilot 目前正在开发其 Copilot Extensions 系统，这将允许创建自定义 agent/模式：

1. **Copilot Extensions**：

   - 将 GitHub App 与 Copilot agent 结合，创建自定义功能
   - 允许开发人员构建自定义功能并直接集成到 Copilot Chat 中

2. **构建自定义 Agent**：

   - 需要创建 GitHub App 并将其与 Copilot agent 集成
   - 自定义 agent 可以部署到可通过 HTTP 请求访问的服务器

3. **自定义指令**：
   - 目前支持用于指导一般行为的基本自定义指令
   - 完整的 agent 自定义支持正在开发中

_注意：GitHub Copilot 中的完整自定义模式配置仍在开发中。查看 GitHub 的文档以获取最新更新。_
