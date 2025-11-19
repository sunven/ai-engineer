Agent

代理是专门的人工智能助手，能够理解特定领域并提供专家级指导。每个代理都经过其专业领域最佳实践的培训

你可以把普通的 Claude 聊天模型理解为一个**通才（general AI）**，  
而 Agent 是在它的基础上，**赋予角色、任务、记忆和工具访问能力**的“专用人格”。

以用户说：

> “请每周帮我生成一份销售报告并发给我邮箱。”

Claude Agent 会：

1. **理解意图**：知道你要每周自动报告
2. **规划任务**：决定需要做哪些步骤
3. **调用 Skill**：使用 `data_analysis.skill`、`report_format.skill`
4. **调用 Tool**：从数据库提取数据 → 调用邮件 API 发送
5. **记忆更新**：保存报告历史、发送状态
6. **执行完成**：返回你结果，并等待下次调度

## Skills

Claude 自动发现的能力,适合需要多个文件或验证步骤的复杂工作流程

人掌握的技能

Agent 决定“做什么”，Skill 决定“怎么做”。  
多个 Skills 可以被同一个 Agent 调用，构成一个完整的工作流

**Agent** 负责“想”；**Skill** 负责“怎么做”；**Tool** 负责“去做”。
Agent 是核心大脑，Skills 是大脑学过的专业技能，Tools 是执行这些技能的工具。

技能是指令、脚本和资源的文件夹，克劳德通过动态加载这些内容来提升特定任务的执行能力。技能教会克劳德如何以可重复的方式完成具体任务，无论是按照公司品牌规范创建文档、运用组织特定工作流程分析数据，还是自动化个人事务

专业化、可重复的工作能力，使其在特定任务上表现得更像领域专家，而不是仅靠一次性提示完成所有工作

**门任务能力包**（模块化工作流程插件/扩展）

- 在组织/团队内部，设置标准化流程：比如“根据公司的品牌指南生成 PPT”“处理 Excel 报表并加入公式”“生成填充 PDF 文档”。 [anthropic.com+1](https://www.anthropic.com/news/skills?utm_source=chatgpt.com)
- 减少每次都需要写大量 Prompt 的重复劳动：当 Skill 被触发，Claude 会自动识别并加载相应资源。 [Tom's Guide+1](https://www.tomsguide.com/ai/claude-skills-are-here-and-they-might-be-the-smartest-ai-feature-youre-not-using-yet?utm_source=chatgpt.com)
- 提升工作效率 & 精准度：因为 Skill 内含“脚本＋模板＋规则”，Claude 在执行时更加贴合预期

## 使用注意事项

- 安全性：因为 Skills 可以包含脚本代码、外部调用、资源依赖，若来源不可信可能引入风险。 [anthropic.com](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills?utm_source=chatgpt.com)
- 触发机制：要让 Claude “知道”何时使用某个 Skill，Skill 的说明／元数据写得好很关键，否则 Claude 可能不会自动调用或调用不准确。
- 版本管理 &维护：Skill 一旦用在组织中，可能随着流程演化需要更新。团队需有机制去管理 Skill 包

## Command

用于自动化常见的开发任务

- 它让开发者 **快速执行预定义流程**，而不是每次都手动输入详细指令。
- 它提升了工作流自动化能力 — 你只需输入一个命令，就可以调用 Claude 阅读代码、修改文件、运行 shell 命令等。 [Claude 文档+1](https://docs.anthropic.com/en/docs/claude-code/overview?utm_source=chatgpt.com)
- Slash commands 还方便团队共享“任务模板”或“流程脚本”，保证一致性

## Plugin

明确调用的提示,适合重复使用的简单提示

## Plugin 的组成部分

Plugin 可以包含以下组件[2](https://docs.claude.com/en/api/agent-sdk/plugins):

- **Commands**(命令):自定义斜杠命令
- **Agents**(代理):用于特定任务的专门子代理
- **Skills**(技能):Claude 自主使用的模型调用能力
- **Hooks**(钩子):响应工具使用和其他事件的事件处理器
- **MCP servers**(MCP 服务器):通过模型上下文协议的外部工具集成

## Plugin 的作用

他们通过让团队标准化其智能体开发设置，解决了“如何为我的配置建立相同的智能体工作流”的问题。

工程负责人可以创建标准化的设置供所有人使用。每个人都能获得相同的工具、相同的配置、相同的快捷方式，无需额外设置。

Plugin 让您可以[1](https://docs.claude.com/en/docs/claude-code/plugins#develop-more-complex-plugins):

- **安装预构建的扩展**:从市场安装插件以添加预构建的命令、代理、钩子、技能和 MCP 服务器
- **创建自定义功能**:创建自己的插件来自动化工作流程
- **跨项目共享**:在不同项目间重用功能
- **团队协作**:与团队成员共享标准化工具

## Plugin 的结构

```
my-first-plugin/
├── .claude-plugin/
│   └── plugin.json          # Plugin metadata
├── commands/                 # Custom slash commands (optional)
│   └── hello.md
├── agents/                   # Custom agents (optional)
│   └── helper.md
├── skills/                   # Agent Skills (optional)
│   └── my-skill/
│       └── SKILL.md
└── hooks/                    # Event handlers (optional)
    └── hooks.json
```

https://github.com/anthropics/claude-code/tree/main/plugins

https://claudemarketplaces.com/

https://claudex.directory/

https://rube.app/

## Hooks

钩子是自动化脚本，在开发工作流程中特定事件发生时触发操作。它们会在后台自动运行。

用户定义的 shell 命令，在 Claude Code 生命周期的各个点执行。Hooks 提供对 Claude Code 行为的确定性控制，确保某些操作总是发生，而不是依赖 LLM 选择运行它们

>在每次文件编辑后对 .ts 文件运行 `prettier`

## Setting

设置项用于调整 Claude Code 在项目中的行为方式，可控制性能、安全性、界面及工作流程偏好

## MCP

|                                                                              |     |
| ---------------------------------------------------------------------------- | --- |
| [chrome-devtools-mcp](https://github.com/ChromeDevTools/chrome-devtools-mcp) |     |
|                                                                              |     |

https://metaso.cn/

https://github.com/anthropics/skills

https://github.com/github/spec-kit

https://github.com/UfoMiao/zcf

https://github.com/yusufkaraaslan/Skill_Seekers

https://github.com/hesreallyhim/awesome-claude-code

https://github.com/OneRedOak/claude-code-workflows

这是一个非常好的问题。乍一看，**Agent (代理)** 和 **Skill (技能)** 确实很容易混淆，因为它们都是用来“教”Claude 做它原本不会做的事情。

**直接回答你的问题：**
**它们不重复。** 它们处于解决问题的不同层级。

简单的一句话区别：**Agent 是“谁”（Who），而 Skill 是“怎么做”（How）。**

我们可以通过以下几个维度来深度拆解它们的区别：

### 1. 核心隐喻：工人 vs. 工具书

*   **Agent (代理) = 专职工人**
    *   想象你雇佣了一个 **“专业的前端测试员”**。
    *   他有**独立的人格**（系统提示词），比如“你是一个严格的测试员，只关注 Bug”。
    *   他有**独立的记忆**（上下文），专门处理测试任务，不会被之前聊的午饭话题干扰。
    *   他**可以使用**各种工具（包括 Skills）。
*   **Skill (技能) = 操作手册 / 工具**
    *   想象一本 **《公司内部 API 调用手册》** 或者一个 **“自动生成日报的脚本”**。
    *   它是一段**死的知识**或**固定的流程**。
    *   无论是“测试员 Agent”还是“项目经理 Agent”，甚至是主 Claude，**谁都可以拿来用**。

### 2. 详细对比表

| 维度 | Agent (代理) | Skill (技能) |
| :--- | :--- | :--- |
| **本质** | **决策者 (Decision Maker)** | **执行单元 (Capability)** |
| **复杂性** | 处理**开放式、多步骤**的目标。 <br> *(例：“帮我把这个 Python 项目重构为 Go”)* | 处理**确定性、原子级**的任务。 <br> *(例：“查询数据库中的用户表”)* |
| **上下文** | 拥有**独立的上下文窗口**。 <br> 它的对话历史与主对话隔离，避免干扰。 | **共享当前上下文**。 <br> 它是被嵌入到当前的对话流中执行的。 |
| **能动性** | **高**。Agent 可以自己思考：“第一步失败了，我该试第二步还是报错？” | **低**。Skill 通常是：“照着这几步做，做完告诉我结果。” |
| **关系** | Agent **包含/使用** Skill。 <br> 一个 Agent 可以装备多个 Skills。 | Skill 是被调用的对象。 |

### 3. 场景举例：如何决定用哪个？

为了更直观地理解，我们来看几个具体的开发场景：

#### 场景 A：公司内部有一套复杂的代码提交规范（Git Commit Message）

*   **你应该用 Skill。**
*   **为什么？** 因为这是一个**标准化的知识**。你不需要一个专门的“提交人员”，你需要的是让现在的 Claude 在写提交信息时，参考这份规范。
*   **做法：** 创建一个 `Commit-Style` Skill，里面写上：“提交信息必须包含 Ticket ID，必须是动宾结构...”。

#### 场景 B：你需要定期对项目进行安全审计

*   **你应该用 Agent。**
*   **为什么？** 因为这是一个**复杂的任务流**。它涉及：扫描代码 -> 分析报告 -> 排除误报 -> 尝试修复 -> 验证修复。这中间需要大量的推理和判断。
*   **做法：** 创建一个 `Security-Auditor` Agent。给它配备扫描工具（Skill），并设定 Persona：“你是一个安全专家，对漏洞零容忍”。

#### 场景 C：查询生产环境的日志

*   **你应该用 Skill。**
*   **为什么？** 这是一个**工具行为**。这是一个动作，而不是一个角色。
*   **做法：** 创建一个 `Fetch-Logs` Skill，封装 SSH 命令或 API 调用。

### 4. 进阶理解：它们的协作关系

在 Claude Code 的高级用法中，它们通常是配合使用的：

1.  **用户指令**：“Claude，帮我把这个模块的测试覆盖率提升到 90%。”
2.  **Claude (主程序)**：分析任务，发现比较复杂，决定委派给 **QA-Agent**（因为这个 Agent 擅长写测试）。
3.  **QA-Agent (代理)**：接管任务。它分析代码，发现需要运行测试。
4.  **QA-Agent**：决定调用 **Run-Test-Script (Skill)** 来查看当前覆盖率。
5.  **Skill**：执行具体的 Shell 命令，返回覆盖率数据。
6.  **QA-Agent**：根据 Skill 返回的数据，编写新的测试用例，再次循环，直到达成目标。

### 总结

*   如果你想扩展 Claude 的 **“知识库”** 或 **“工具箱”**（让它学会*怎么做*某件具体的事），请使用 **Skill**。
*   如果你想扩展 Claude 的 **“团队”**（给它找个帮手去*负责*某一类复杂的领域），请使用 **Agent**。

https://claude.com/blog/skills-explained
