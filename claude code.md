## 1. Agent (代理)

### 定义

子代理是具有自己上下文窗口、自定义系统提示和特定工具权限的专业 AI 助手

**子代理如何工作：** 每个子代理都有其自己的配置——你可以定义它做什么、如何处理问题以及它可以访问哪些工具

### 核心特征

- **决策者角色**：处理开放式、多步骤的复杂目标
- **独立上下文**：拥有独立的对话历史，避免与主对话干扰
- **高能动性**：可以自主思考和调整策略
- **工具使用者**：可以调用多个 Skills 和 Tools

### 适用场景

- 需要定期进行安全审计（扫描 → 分析 → 修复 → 验证）
- 复杂的代码重构任务
- 需要多轮推理和判断的工作流

## 2. Skill (技能)

### 定义

指令、脚本和资源的文件夹集合，Claude 通过动态加载来提升特定任务的执行能力。是**标准化、可重复的工作能力包**。

**何时使用技能：** 如果你发现自己在不同对话中反复输入相同的提示词，就该创建一个技能了。将重复的指令，如“使用 OWASP 标准审查此代码的安全性”或“用执行摘要、关键发现和建议格式化此分析”，转化为技能。这可以让你避免每次都重新解释流程，并确保一致的执行。

## 3. Agent vs Skill 对比

- **Agent** 负责"想"（决策）
- **Skill** 负责"怎么做"（方法）
- **Tool** 负责"去做"（执行）

| 维度 | Agent (代理) | Skill (技能) |
|------|-------------|-------------|
| **本质** | 决策者 (Decision Maker) | 执行单元 (Capability) |
| **隐喻** | 专职工人（有人格、记忆） | 操作手册 / 工具 |
| **复杂性** | 开放式、多步骤目标<br>*例：Python 项目重构为 Go* | 确定性、原子级任务<br>*例：查询数据库用户表* |
| **上下文** | 独立的上下文窗口 | 共享当前上下文 |
| **能动性** | 高（可自主调整策略） | 低（按步骤执行） |
| **关系** | 包含/使用 Skill | 被 Agent 调用 |

### 选择原则

- **用 Skill**：扩展 Claude 的"知识库"或"工具箱"（教它*怎么做*）
- **用 Agent**：扩展 Claude 的"团队"（找个帮手*负责*某领域）

### 协作示例

```
用户："提升测试覆盖率到 90%"
  ↓
Claude 主程序：委派给 QA-Agent
  ↓
QA-Agent：分析代码，决定运行测试
  ↓
QA-Agent：调用 Run-Test-Script (Skill)
  ↓
Skill：执行 Shell 命令，返回覆盖率数据
  ↓
QA-Agent：根据数据编写新测试用例，循环直到达标
```

### 场景决策指南

#### 场景 A：代码提交规范

- **选择**：Skill
- **原因**：标准化知识，不需要专门的"提交人员"
- **做法**：创建 `Commit-Style` Skill，定义提交信息格式规则

#### 场景 B：安全审计

- **选择**：Agent
- **原因**：复杂任务流，需要推理和判断
- **做法**：创建 `Security-Auditor` Agent，配备扫描工具

#### 场景 C：查询日志

- **选择**：Skill
- **原因**：工具行为，是动作而非角色
- **做法**：创建 `Fetch-Logs` Skill，封装查询命令

## 4. Command (命令)

### 定义

用于自动化常见开发任务的快捷操作，通过斜杠命令快速执行预定义流程。

### 核心价值

- 快速执行预定义流程，无需手动输入详细指令
- 提升工作流自动化能力
- 方便团队共享"任务模板"或"流程脚本"

## 5. Plugin (插件)

### 定义

标准化的扩展包，用于团队共享和复用工作流配置。

### 组成部分

- **Commands**：自定义斜杠命令
- **Agents**：特定任务的专门子代理
- **Skills**：Claude 自主使用的能力
- **Hooks**：事件响应处理器
- **MCP Servers**：外部工具集成

### 核心价值

- 标准化团队的智能体开发设置
- 所有人获得相同的工具、配置、快捷方式
- 跨项目共享和重用功能

### 目录结构

```
my-first-plugin/
├── .claude-plugin/
│   └── plugin.json          # 插件元数据
├── commands/                 # 自定义斜杠命令（可选）
│   └── hello.md
├── agents/                   # 自定义代理（可选）
│   └── helper.md
├── skills/                   # Agent Skills（可选）
│   └── my-skill/
│       └── SKILL.md
└── hooks/                    # 事件处理器（可选）
    └── hooks.json
```

## 6. Hook (钩子)

### 定义

在开发工作流程中特定事件发生时自动触发的脚本，提供对 Claude Code 行为的确定性控制。

### 特点

- 在后台自动运行
- 确保某些操作总是发生（不依赖 LLM 选择）

### 示例

- 每次文件编辑后对 `.ts` 文件运行 `prettier`
- 代码提交前自动运行 lint 检查

## 7. Setting (设置)

### 定义

用于调整 Claude Code 在项目中的行为方式，可控制性能、安全性、界面及工作流程偏好。

## 8. MCP (Model Context Protocol)

### 定义

通过模型上下文协议集成外部工具的机制。

### 示例

- [chrome-devtools-mcp](https://github.com/ChromeDevTools/chrome-devtools-mcp)

## 参考资源

### 官方资源

- [Claude Skills 官方博客](https://claude.com/blog/skills-explained)
- [Anthropic Skills 仓库](https://github.com/anthropics/skills)
- [Claude Code 插件仓库](https://github.com/anthropics/claude-code/tree/main/plugins)

### 社区资源

- [Claude Marketplaces](https://claudemarketplaces.com/)
- [Claudex Directory](https://claudex.directory/)
- [Rube App](https://rube.app/)
- [Awesome Claude Code](https://github.com/hesreallyhim/awesome-claude-code)
- [Claude Code Workflows](https://github.com/OneRedOak/claude-code-workflows)

### 其他工具

- [Metaso](https://metaso.cn/)
- [GitHub Spec Kit](https://github.com/github/spec-kit)
- [ZCF](https://github.com/UfoMiao/zcf)
- [Skill Seekers](https://github.com/yusufkaraaslan/Skill_Seekers)
