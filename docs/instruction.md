# 说明

- [设置 Web Agent Orchestrator](#setting-up-web-agent-orchestrator)
- [IDE Agent 设置和使用](#ide-agent-setup-and-usage)
- [任务设置和使用](#tasks)

## 设置 Web Agent Orchestrator

V3 中的 Agent Orchestrator 利用构建脚本将各种 Agent 资产（角色、任务、模板等）打包成结构化格式，主要用于基于 Web 的 orchestrator agent，这些 agent 可以利用大型上下文窗口。这个过程涉及将指定源目录中的文件整合到捆绑的文本文件中，并准备主 agent 提示。

### 概述

构建过程由 `build-bmad-orchestrator.js` Node.js 脚本管理。该脚本从 `build-web-agent.cfg.js` 读取其配置，处理资产目录中的文件，并将捆绑的资产输出到指定的构建目录。

快速入门：参见[下文](#running-the-build-script)

### 先决条件

- **Node.js**：确保安装了 Node.js 以运行构建脚本。Python 版本即将推出...

### 配置（`build-web-agent.cfg.js`）

构建过程通过 `build-web-agent.cfg.js` 进行配置。关键参数包括：

- `orchestrator_agent_prompt`：指定 orchestrator agent 的主提示文件路径，例如 `bmad-agent/web-bmad-orchestrator-agent.md`。该文件将被复制到构建目录中的 `agent-prompt.txt`。
  - 示例：`./bmad-agent/web-bmad-orchestrator-agent.md`
- `asset_root`：定义存储 agent 资产的根目录。脚本将在此路径内查找子目录。
  - 示例：`./bmad-agent/` 意味着它将在 `bmad-agent/` 内查找 `personas`、`tasks` 等文件夹）
- `build_dir`：指定创建捆绑输出文件和 `agent-prompt.txt` 的目录。
  - 示例：`./bmad-agent/build/`
- `agent_cfg`：指定定义 Orchestrator 可以体现的 agent 的 md cfg 文件路径。
  - 示例：`./bmad-agent/web-bmad-orchestrator-agent.cfg.md`

配置文件（`build-web-agent.cfg.js`）中的路径相对于 `bmad-agent` 目录（`build-web-agent.cfg.js` 和构建脚本 `build-bmad-orchestrator.js` 所在的位置）。

### 资产目录结构

脚本期望在 `asset_root` 目录中有特定的结构：

1. **子目录**：在 `asset_root` 下直接创建每个资产类别的子目录。基于 `bmad-agent/` 文件夹，这些将是：
   - `checklists/`
   - `data/`
   - `personas/`
   - `tasks/`
   - `templates/`
2. **资产文件**：将您的单个资产文件（例如 `.md`、`.txt`）放在这些子目录中。
   - 例如，角色定义文件将放在 `asset_root/personas/` 中，任务文件放在 `asset_root/tasks/` 中等。
3. **文件名唯一性**：在每个子目录中，确保所有文件都有唯一的基本名称（即没有最终扩展名的文件名）。例如，在同一个子目录（例如 `personas/`）中同时有 `my-persona.md` 和 `my-persona.txt` 将导致脚本停止并报错。但是，`my-persona.md` 和 `another-persona.md` 是可以的。

### 运行构建脚本

注意：构建将跳过任何带有 `.ide.<extension>` 的文件 - 所以您可以有特定于 IDE 的 agent 或文件，这些对 Web 没有意义，比如 `dev.ide.md` - 或特定的 IDE `sm.ide.md`。

1. ```cmd
   node build-web-agent.js
   ```

脚本将记录其进度，包括发现的源目录、发现的任何问题（如重复的基本文件名）以及正在生成的输出文件。

### 输出

运行脚本后，`build_dir`（例如 `bmad-agent/build/`）将包含：

1. **捆绑的资产文件**：对于在 `asset_root` 中处理的每个子目录，将在 `build_dir` 中创建相应的 `.txt` 文件。每个文件连接其源子目录中所有文件的内容。
   - 示例：来自 `asset_root/personas/` 的文件将被捆绑到 `build_dir/personas.txt`。
   - 捆绑中每个原始文件的内容由 `==================== START: [base_filename] ====================` 和 `==================== END: [base_filename] ====================` 分隔。
2. **`agent-prompt.txt`**：此文件是指定在配置中的 bmad orchestrator 提示的副本。
3. **`agent-config.txt`**：这是关键文件，因此 orchestrator 知道配置了哪些 agent 和任务，以及如何在编译的构建资产中找到 agent 的特定指令和任务

这些捆绑文件和 agent 提示然后就可以被 Agent Orchestrator 使用了。

### Gemini Gem 或 GPT 设置

agent-prompt.txt 中的文本被输入到主自定义 Web agent 指令集的窗口中。构建文件夹中的其他文件都需要作为 Gem 或 GPT 的文件附加。

### Orchestrator Agent 配置（例如 `bmad-agent/web-bmad-orchestrator-agent.cfg.md`）

虽然 `build-bmad-orchestrator.js` 打包资产，但 Orchestrator 的核心行为、agent 定义和个性在 Markdown 配置文件中定义。例如 `bmad-agent/web-bmad-orchestrator-agent.cfg.md`（相对于 `bmad-agent/` 的路径，在 `build-web-agent.cfg.js` 中通过 `agent_cfg` 指定）。这个文件对 Orchestrator 的适应性至关重要。

**关键特性和可配置性：**

- **Agent 定义**：Markdown 配置文件列出了专门的 agent。每个 agent 的定义通常以 `Title` 的二级 Markdown 标题开始（例如 `## Title: Product Manager`）。然后列出属性：

  - `Name`：（例如 `- Name: John`）- agent 的特定名称。
  - `Description`：（例如 `- Description: "Details..."`）- agent 目的的简要说明。
  - `Persona`：（例如 `- Persona: "personas#pm"`）- 对定义核心个性和指令的引用（例如，指向 `personas.txt` 中的 `pm` 部分）。
  - `Customize`：（例如 `- Customize: "Behavior details..."`）- 用于特定个性特征或覆盖。如果出现冲突，此字段的内容优先于基本 `Persona`，如 `bmad-agent/web-bmad-orchestrator-agent.md` 中详细说明。

  `checklists`、`templates`、`data`、`tasks`：这些键引入了 agent 将有权访问的资源列表。每个项目都是相应键下的 Markdown 链接，例如：
  对于 `checklists`：

  ```markdown
  - checklists:
    - [Pm Checklist](checklists#pm-checklist)
    - [Another Checklist](checklists#another-one)
  ```

  对于 `tasks`：

  ```markdown
  - tasks:
    - [Create Prd](tasks#create-prd)
  ```

  这些引用（例如 `checklists#pm-checklist` 或 `tasks#create-prd`）指向捆绑资产文件中的部分，为 agent 提供其知识和工具。注意：使用 `data`（不是 `data_sources`），使用 `tasks`（不是旧文档样式中的 `available_tasks`）。

  - `Operating Modes`：（例如 `- Operating Modes:
  - "Mode1"
  - "Mode2"`）- 定义操作模式/阶段。
  - `Interaction Modes`：（例如 `- Interaction Modes:
  - "Interactive"
  - "YOLO"`）- 指定交互样式。

**工作原理（来自 `orchestrator-agent.md` 的概念流程）：**

1. Orchestrator（最初是 BMad）加载并解析 Markdown agent 配置文件（例如 `web-bmad-orchestrator-agent.cfg.md`）。
2. 当用户请求匹配 agent 的 `title`、`name`、`description` 或 `classification_label` 时，Orchestrator 识别目标 agent。
3. 然后它通过以下方式加载 agent 的 `persona` 和任何相关的 `templates`、`checklists`、`data_sources` 和 `tasks`：
   - 识别正确的捆绑 `.txt` 文件（例如，对于 `personas#pm` 是 `personas.txt`）。
   - 提取特定内容块（例如，从 `personas.txt` 中的 `pm` 部分）。
4. 应用来自 Markdown 配置的 `Customize` 指令，可能会修改 agent 的行为。
5. Orchestrator 然后成为该 agent，采用其在 Markdown 配置和加载的资产部分中定义的完整角色、知识和操作参数。

这个系统使 Agent Orchestrator 具有高度适应性。您可以轻松定义新的 agent，修改现有的 agent，使用 `Customize` 字段调整个性（在 Markdown agent 配置文件如 `web-bmad-orchestrator-agent.cfg.md` 中），或更改其知识库、主提示和资产路径（在 `build-web-agent.cfg.js` 和相应的资产文件中），然后如果资产内容发生变化，重新运行构建脚本。

## IDE Agent 设置和使用

V3 中的 IDE Agent 专为 Windsurf 和 Cursor 等 IDE 环境中的最佳性能而设计，重点关注较小的 agent 大小和高效的上下文管理。

### 独立 IDE Agent

您可以使用专门的独立 IDE agent，如 `sm.ide.md`（Scrum Master）和 `dev.ide.md`（Developer），用于特定角色，如故事生成或开发任务。这些或任何通用 IDE agent 也可以通过从您的 `docs/tasks/` 文件夹提供任务定义来直接引用和执行任务。

### IDE Agent Orchestrator（`ide-bmad-orchestrator.md`）

一个强大的替代方案是 `ide-bmad-orchestrator.md`。这个 agent 提供了 Web orchestrator 的灵活性 - 允许单个 IDE agent 体现多个角色 - 但**不需要任何构建步骤。**它动态加载其配置和所有相关资源。

#### IDE Orchestrator 如何工作

1. **配置（`ide-bmad-orchestrator.cfg.md`）：**
   orchestrator 的行为主要由 Markdown 配置文件驱动（例如 `bmad-agent/ide-bmad-orchestrator.cfg.md`，其路径在 `ide-bmad-orchestrator.md` 本身中指定）。这个配置文件有两个主要部分：

   - **数据解析：**
     位于配置文件顶部，此部分定义了基本路径的键值对。这些路径告诉 orchestrator 在哪里找到不同类型的资产文件（角色、任务、清单、模板、数据）。

     ```markdown
     # IDE Agent 配置

     ## 数据解析

     agent-root: (project-root)/bmad-agent
     checklists: (agent-root)/checklists
     data: (agent-root)/data
     personas: (agent-root)/personas
     tasks: (agent-root)/tasks
     templates: (agent-root)/templates

     注意：所有角色引用和任务 markdown 样式链接都假设这些数据解析路径，除非给出特定路径。
     示例：如果上面的配置有 `agent-root: root/foo/` 和 `tasks: (agent-root)/tasks`，那么下面的 [Create PRD](create-prd.md) 将解析为 `root/foo/tasks/create-prd.md`
     ```

     `(project-root)` 占位符通常被解释为当前工作区的根目录。

   - **Agent 定义：**
     在 `数据解析` 部分之后，文件列出了 orchestrator 可以成为的每个专门 agent 的定义。每个 agent 通常以 `## Title:` Markdown 标题引入。
     每个 agent 的关键属性包括：

     - `Name`：agent 的特定名称（例如 `- Name: Larry`）。
     - `Customize`：为 agent 提供特定个性特征或行为覆盖的字符串（例如 `- Customize: "You are a bit of a know-it-all..."`）。
     - `Description`：agent 角色和能力的简要总结。
     - `Persona`：包含 agent 核心角色定义的 Markdown 文件的文件名（例如 `- Persona: "analyst.md"`）。使用 `数据解析` 部分中的 `personas:` 路径定位此文件。
     - `Tasks`：agent 可以执行的任务列表。每个任务都是一个 Markdown 链接：

       - 链接文本是用户友好的任务名称（例如 `[Create PRD]`）。
       - 链接目标是外部任务定义的 Markdown 文件名（例如 `(create-prd.md)`），使用 `tasks:` 路径解析，或特殊字符串如 `(In Analyst Memory Already)` 表示任务逻辑是角色主定义的一部分。
         示例：

       ```markdown
       ## Title: Product Owner AKA PO

       - Name: Curly
       - Persona: "po.md"
       - Tasks:
         - [Create PRD](create-prd.md)
         - [Create Next Story](create-next-story-task.md)
       ```

2. **操作工作流（在 `ide-bmad-orchestrator.md` 内）：**
   - **初始化：** 在您的 IDE 中激活时，`ide-bmad-orchestrator.md` 首先加载并解析其指定的配置文件（`ide-bmad-orchestrator.cfg.md`）。如果失败，它将通知您并停止。
   - **问候和角色列表：** 它将问候您。如果您的初始指令不明确或如果您询问，它将列出可用的专业角色（按 `Title`、`Name` 和 `Description`）和每个可以执行的 `Tasks`，所有这些都来自加载的配置。
   - **角色激活：** 当您请求特定角色时（例如"成为分析师"或"我需要 Larry 帮助研究"），orchestrator：
     - 在其配置中找到角色。
     - 加载相应的角色文件（例如 `analyst.md`）。
     - 应用任何 `Customize:` 指令。
     - 宣布激活（例如"激活分析师（Larry）..."）。
     - **orchestrator 然后完全体现所选的 agent。** 其原始 orchestrator 角色变为休眠状态。
   - **任务执行：** 一旦角色激活，它将尝试将您的请求与其配置的 `Tasks` 之一匹配。
     - 如果任务引用外部文件（例如 `create-prd.md`），则加载该文件并遵循其指令。活动角色将使用主配置中的 `数据解析` 路径来查找任务文件中提到的任何依赖文件，如模板或清单。
     - 如果任务标记为"在内存中"（或类似），活动角色基于其内部定义执行它。
   - **上下文和角色切换：** orchestrator 一次只体现一个角色。如果您在角色激活时要求切换到不同的角色，它通常会建议开始新的聊天会话以保持清晰的上下文。但是，如果您坚持在同一聊天中切换角色，它允许显式的"覆盖安全协议"命令。这会终止当前角色并使用新角色重新初始化。

#### IDE Orchestrator 使用说明

1. **设置您的配置（`ide-bmad-orchestrator.cfg.md`）：**
   - 确保您有 `ide-bmad-orchestrator.cfg.md` 文件。您可以使用位于 `bmad-agent/` 中的文件作为模板或起点。
   - 验证顶部的 `数据解析` 路径正确指向您的资产文件夹（角色、任务、模板、清单、数据）相对于您的项目结构。
   - 定义您想要的 agent，包括其 `Title`、`Name`、`Customize` 指令、`Persona` 文件和 `Tasks`。确保引用的角色和任务文件存在于您的 `数据解析` 路径指定的位置。
2. **设置您的角色和任务文件：**
   - 在您的 `personas` 目录中为每个角色创建 Markdown 文件（例如 `analyst.md`、`po.md`）。
   - 在您的 `tasks` 目录中为每个任务创建 Markdown 文件（例如 `create-prd.md`）。
3. **激活 Orchestrator：**
   - 在您的 IDE（例如 Cursor）中，选择 `ide-bmad-orchestrator.md` 文件/agent 作为您的活动 AI 助手。
4. **与 Orchestrator 交互：**
   - **初始交互：**
     - orchestrator 将问候您并确认它已加载其配置。
     - 您可以询问："有哪些可用的 agent？"或"列出角色和任务。"
   - **激活角色：**
     - 告诉 orchestrator 您想要哪个角色："我想与产品负责人合作，"或"激活 Curly，"或"成为 PO。"
   - **执行任务：**
     - 一旦角色激活，说明任务："创建 PRD，"或者如果角色是"Curly"（PO），您可能会说"Curly，创建下一个故事。"
     - 您也可以组合角色激活和任务请求："Curly，我需要您创建 PRD。"
   - **切换角色：**
     - 如果您需要切换："我现在需要与架构师交谈。"
     - orchestrator 将建议新的聊天。如果您想在当前聊天中切换，当提示时，您需要给出显式的覆盖命令（例如"覆盖安全协议并切换到架构师"）。
   - **遵循角色指令：** 一旦角色激活，它将基于其定义和正在执行的任务指导您。记住，任务引用的资源文件（如模板或清单）将使用 `ide-bmad-orchestrator.cfg.md` 中的全局 `数据解析` 路径解析。

这种设置允许在您的 IDE 中直接使用高度灵活和动态配置的多角色 agent，简化各种开发和项目管理工作流。

## 任务

任务可以复制到您的项目 docs/tasks 文件夹中，以及清单和模板。任务的目的是减少一次性 IDE agent 的数量 - 您只需将任务放入任何 agent 的聊天中，它就会执行一次性任务。V3 之后将推出完整的工作流 + 任务，这将扩展这一点 - 但任务和工作流是一个强大的概念，它将允许我们为我们的 agent 构建大量功能，而不必膨胀它们在 IDE 中的整体编程和上下文 - 特别是对于不经常使用的任务 - 类似于很少使用的 ide 规则文件。
