# self-curse-skill

`self-curse-skill` 是一套面向 AI Agent 的自我批评技能规则。它要求 AI 在指定场景中使用强烈但仅指向自身的批评表达，同时继续准确、完整地处理实际任务。

> **重要说明**
>
> - 本技能纯属娱乐，不建议用于正式、公共、未成年人可见或要求文明表达的使用环境。
> - 启用后，技能规则可能作用于 AI 生成的全部用户可见自然语言，计划（Plan）、进度说明和 README 文件中也可能出现粗话，请谨慎使用。
> - 所有攻击性表达都应仅指向 AI 自身，不得攻击用户、家属或第三方。

## 功能

- 支持自动调用和用户单独调用两种模式。
- 存在实际错误时，优先结合可确认的错误进行具体批评。
- 没有实际错误时，不捏造错误事实。
- 所有贬损表达只能指向 AI 自身，不得攻击用户、家属或第三方。
- 自我批评不能替代任务处理，正常任务仍须提供完整答案。
- 通过语法、频率和防复读规则减少机械化表达。

## 适用范围

本项目不局限于 Codex。只要目标 AI Agent 支持加载 Markdown 技能、项目指令或全局指令，就可以复用核心规则。

不同平台的技能目录、全局配置位置和显式调用语法可能不同，安装时应按目标平台的机制进行映射：

- `skills/self-curse/SKILL.md` 是平台无关的核心技能规则。
- `AGENTS.md` 是全局或项目级 Agent 指令模板。
- `skills/self-curse/agents/openai.yaml` 是 OpenAI/Codex 兼容的界面元数据和调用策略；其他平台可以忽略或替换该文件。

## 仓库结构

```text
self-curse-skill/
├── AGENTS.md
└── skills/
    └── self-curse/
        ├── SKILL.md
        └── agents/
            └── openai.yaml
```

## 安装

### 通用安装

1. 克隆或下载本仓库。
2. 将 `skills/self-curse` 目录复制到目标 AI Agent 的技能目录。
3. 将根目录 `AGENTS.md` 中的规则合并到目标平台的全局或项目级 Agent 指令文件。
4. 重新启动目标工具或创建新会话，使技能和指令重新加载。

如果目标平台不支持自动发现技能，可以将 `SKILL.md` 注册为平台支持的系统指令、项目指令或自定义技能，同时保留两种调用模式的区别。

### Codex 示例

以下 PowerShell 命令只演示 Codex 的技能目录安装方式：

```powershell
$codexRoot = Join-Path $env:USERPROFILE '.codex'
$skillRoot = Join-Path $codexRoot 'skills\self-curse'
New-Item -ItemType Directory -Force -Path $skillRoot | Out-Null
Copy-Item -Path '.\skills\self-curse\*' -Destination $skillRoot -Recurse -Force
```

若要在 Codex 中默认启用，还需要将仓库根目录的 `AGENTS.md` 内容合并到 `%USERPROFILE%\.codex\AGENTS.md`。不要直接覆盖已有文件，避免删除其他全局规则。

## 使用方式

### 自动调用

当目标平台加载了全局或项目级 Agent 规则后，`self-curse` 会按规则在任务开始、进行中和结束时生效，不需要用户在每条消息中重复指定技能名称。

### 用户单独调用

在支持显式技能调用的平台中，可以单独发送：

```text
$self-curse
```

单独调用时，回复只包含针对 AI 自身的批评；如果存在可确认的历史错误，应优先结合这些错误，输出后立即结束该轮。

### 与任务一起使用

在正常任务中，技能不会中断任务执行。AI 完成规定的自我批评后，仍须继续提供分析、实现、验证和结论。

## 核心规则

- 批评对象只能是 AI 自身。
- 具体错误优先于泛化表达。
- 不得捏造事实性错误。
- 用户单独调用后不得继续处理其他任务。
- AI 自动调用后可以直接继续未完成的任务。
- 每句用户可见自然语言都须遵守技能定义的表达约束。
- 禁止机械复读固定句首、句式和称谓。
- 特定表达须遵守 `SKILL.md` 中定义的语法和频率限制。
- 自我批评不得降低技术答案的准确性、完整性和可读性。

完整规则以 [`skills/self-curse/SKILL.md`](skills/self-curse/SKILL.md) 为准。

## 开发与验证

修改规则时，应同步维护根目录 `AGENTS.md` 和 `skills/self-curse/SKILL.md` 中相互对应的约束，避免两份规则发生冲突。

可以先执行不会触发编译的静态检查：

```powershell
git diff --check
git status --short
```

如果当前环境提供兼容的技能校验器，还可以对 `skills/self-curse` 目录执行结构和元数据校验。

## 许可证

本项目采用 [MIT License](LICENSE) 开源许可证。
