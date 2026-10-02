# 代码解释 Agent（Code Explanation Agent）

一个基于 **DeepSeek 大模型** 的简单代码助手 Agent。用户输入代码、文件路径或代码相关问题，
Agent 会自动读取代码并给出逻辑解释、逐段注释与潜在问题分析。

本项目是「Homework 1：代码助手 Agent」的实现，重点演示 Agent 开发的三项基础能力：
**LLM 调用、Prompt 设计、工具集成**。

## 仓库地址

- GitHub：<https://github.com/HenryCreel-25/code-agent>

## 功能特性

- ✅ 基本的 Agent 循环：`输入 → 推理 → 工具调用 → 输出`
- ✅ 至少一种工具：`read_file`（读取代码文件）、`list_files`（列出目录）
- ✅ 命令行交互（CLI）
- ✅ 上下文记忆（多轮对话）
- ✅ 错误处理与重试机制（LLM 调用失败自动重试、工具失败回传模型）
- ✅ 无需 Key 的 `--demo` 演示模式，方便快速查看流程

## 技术栈

- 语言：Python 3.10+
- LLM：DeepSeek（`deepseek-chat`，OpenAI 兼容接口）
- 依赖：`openai`（SDK）、`python-dotenv`（读取 `.env`）

## 环境要求

- Python 3.10 及以上
- 一个可用的 [DeepSeek API Key](https://platform.deepseek.com)

## 安装

```bash
# 1. 克隆/进入项目目录
cd code-agent

# 2.（推荐）创建虚拟环境
python -m venv .venv
# Windows
.venv\Scripts\activate
# macOS / Linux
source .venv/bin/activate

# 3. 安装依赖
pip install -r requirements.txt
```

## 配置 API Key

1. 复制示例配置：

   ```bash
   # Windows
   copy .env.example .env
   # macOS / Linux
   cp .env.example .env
   ```

2. 编辑 `.env`，填入你的 Key：

   ```dotenv
   DEEPSEEK_API_KEY=sk-你的真实Key
   ```

   > `.env` 已被 `.gitignore` 忽略，**不会**被提交到 Git，请放心使用。
   > 也可以不建 `.env`，直接设置环境变量 `DEEPSEEK_API_KEY`。

可选环境变量：

| 变量 | 默认值 | 说明 |
|------|--------|------|
| `DEEPSEEK_API_KEY` | 无 | 必填，你的 API Key |
| `DEEPSEEK_MODEL` | `deepseek-chat` | 模型名（函数调用请使用 `deepseek-chat`） |
| `DEEPSEEK_BASE_URL` | `https://api.deepseek.com` | 接口地址 |

## 使用方法

### 1. 交互式对话（真实模式）

```bash
python main.py
```

示例：

```
你 > 请解释 examples/sample.py
Agent 思考中…
Agent > 这段代码定义了一个 calculate_total 函数……
```

可用命令：

| 命令 | 作用 |
|------|------|
| `/help` | 查看帮助 |
| `/clear` | 清空对话上下文 |
| `/exit` | 退出 |

### 2. 单次问答

```bash
python main.py --once "请解释 examples/sample.py"
```

### 3. 演示模式（无需 Key）

```bash
python main.py --demo
# 或
python main.py --demo --once "请解释 examples/sample.py"
```

演示模式会模拟「先读文件、再回答」的完整循环，方便在未配置 Key 时查看流程。

### 可以这样提问

- 指向文件：`请解释 examples/sample.py`
- 直接贴代码：`下面这段代码是什么意思？`（随后粘贴代码）
- 代码问题：`calculate_total 为什么要检查 discount 的范围？`

## 项目结构

```
code-agent/
├── main.py                 # 命令行入口
├── requirements.txt        # 依赖清单
├── .env.example            # 配置模板（复制为 .env 使用）
├── agent/
│   ├── config.py           # 全局配置
│   ├── llm.py              # LLM 调用层（DeepSeek / Demo 客户端）
│   ├── prompts.py          # Prompt 设计
│   ├── tools.py            # 工具定义与执行
│   ├── memory.py           # 上下文记忆
│   └── agent.py            # Agent 核心循环
├── examples/
│   └── sample.py           # 示例代码
└── tests/                  # 单元测试
```

## 测试

```bash
python -m unittest discover -s tests -t .
```

## 架构说明

更详细的架构、Agent 循环流程图、Prompt 与工具设计，请参见 [Design.md](Design.md)。
