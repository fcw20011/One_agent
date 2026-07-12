# One_agent

基于 Anthropic Claude 的 AI Agent 框架，实现 **Plan → Act → Observe** 的智能体执行循环。

## 项目概述

One_agent 是一个功能完整的 AI Agent 框架，以 Anthropic Claude 为 LLM 后端，支持多 Agent 协作、子 Agent 派生、任务跟踪、会话持久化、权限审批、上下文压缩等能力。

## 核心特性

- **Plan → Act → Observe 循环** — 完整的 Agent 执行流程，支持多步骤推理
- **多 Agent 协作** — 内置 Planner / Executor / Reviewer 三种角色，通过 TOML 配置定义
- **子 Agent 派生** — 支持前台阻塞和后台并行两种模式，最大嵌套深度 2 层
- **工具系统** — 可扩展的工具注册表，内置 9 种工具（bash、文件操作、任务管理等）
- **上下文压缩** — 当对话超长时自动用 LLM 生成摘要，防止超出 context window
- **权限管理** — 6 层权限评估链，支持 allow/deny/ask 策略和用户交互审批
- **会话持久化** — 文件系统存储会话历史、笔记，支持跨 turn 记忆
- **MCP 协议** — 支持连接外部 MCP Server 扩展工具能力
- **事件驱动** — 25 种 Pydantic 事件类型覆盖完整生命周期
- **全链路追踪** — JSONL 格式记录 IPC 命令、LLM 调用的完整 I/O

## 目录结构

```
One_agent/
├── src/
│   └── one_agent/
│       ├── cli/                    # CLI 客户端
│       │   ├── commands/           # CLI 命令实现
│       │   ├── main.py            # CLI 入口
│       │   └── __main__.py        # 模块入口
│       ├── core/                   # 核心层
│       │   ├── agents/            # Agent 实现
│       │   ├── bus/               # 事件总线
│       │   ├── compact/           # 上下文压缩
│       │   ├── config.py          # 配置管理
│       │   ├── events/            # 事件系统
│       │   ├── llm/               # LLM 抽象层
│       │   ├── mcp/               # MCP 协议客户端
│       │   ├── memory/            # 上下文文件加载
│       │   ├── permissions/       # 权限与审批
│       │   ├── session/           # 会话管理
│       │   ├── skills/            # 技能系统
│       │   ├── subagent/          # 子 Agent 派生
│       │   ├── task/              # 任务编排
│       │   ├── tools/             # 工具系统
│       │   ├── trace/             # 全链路追踪
│       │   └── transport/         # TCP IPC 传输层
│       └── tui/                    # Textual TUI 前端
│           └── app.py             # TUI 应用
├── tests/                         # 测试代码
├── docs/                          # 文档
├── scripts/                       # 脚本工具
├── .env.example                   # 环境变量示例
├── AGENT.md                       # Agent 配置
├── CLAUDE.md                      # Claude 配置
├── Makefile                       # 构建脚本
├── pyproject.toml                 # 项目配置
├── README.md                      # 项目说明
├── RUNBOOK.md                     # 运维手册
└── WIRE_PROTOCOL.md               # 线协议文档
```

## 三大入口

| 命令 | 用途 |
|------|------|
| `one-agent chat` | 交互式对话模式 |
| `one-agent run "目标"` | 单次任务执行 |
| `one-agent tui` | Textual TUI 前端 |

## 快速开始

### 安装

```bash
# 克隆仓库
git clone git@github.com:fcw20011/One_agent.git
cd One_agent

# 安装依赖
pip install -e ".[dev]"

# 设置 API Key
export ANTHROPIC_API_KEY="your-api-key"

# 运行
one-agent run "你的任务目标"
```

### 配置

四层配置优先级：`内置默认值` → `~/.kama/config.toml` → `.env` → `环境变量`

关键环境变量：
- `ANTHROPIC_API_KEY` — Anthropic API 密钥（必需）
- `ONE_AGENT_MODEL` — 默认模型（默认 `claude-sonnet-4-6`）
- `ONE_AGENT_MAX_STEPS` — 最大执行步数（默认 30）

## 内置工具

| 工具 | 功能 |
|------|------|
| `bash` | 执行 Shell 命令 |
| `read_file` | 读取文件内容 |
| `write_file` | 写入文件 |
| `list_dir` | 列出目录结构 |
| `note_save` | 保存跨 turn 笔记 |
| `task_create` | 创建任务（支持依赖） |
| `task_update` | 更新任务状态 |
| `task_list` | 列出所有任务 |
| `task_get` | 查询任务详情 |
| `spawn_agent` | 派生子 Agent |
| `agent_result` | 查询子 Agent 结果 |

## 开发

```bash
# 运行测试
make test

# 代码检查
make lint

# 类型检查
make typecheck
```

## 技术栈

- **Python 3.12+** — 运行时
- **Anthropic SDK** — LLM 调用（流式 + prompt caching）
- **Pydantic v2** — 数据校验与序列化
- **Textual** — 终端 UI 框架
- **asyncio** — 异步编程
- **Hatchling** — 构建系统
- **Ruff** — 代码风格
- **Mypy** — 静态类型检查
- **pytest** — 测试框架

## 许可证

MIT License




