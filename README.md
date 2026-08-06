# Capture to Obsidian：一键记录到 Obsidian

把语音转写、普通文字、链接和可读取的 ID 自动整理成结构化 Markdown
随记并写入 Obsidian。它直接写入本地文件系统，不依赖 Obsidian 插件、API
或外部服务。

首个版本稳定支持 Codex。Skill 遵循开放的 Agent Skills 格式，但暂不承诺
其他 Agent 已经适配；如果你需要其他 Agent 支持，请提交 Issue，并附上 Agent
名称、版本、安装位置和实际表现。

[English](README.en.md)

## 快速开始

1. 安装 Obsidian 和 Codex App，新建一个专用项目，最好使用工作区模式，并允许 Codex 访问 Obsidian 目录。
2. 把下面这句话发给 Codex：

```text
请从 GitHub 全局安装 `moyi-dong/speech-fix` 和 `moyi-dong/capture-to-obsidian`，并把当前项目配置成自动记录到 Obsidian 的入口；找不到目录时用中文询问我，配置完成后告诉我。
```

以后直接发送语音、文字、链接、Codex 对话链接或 ID 即可。它会尽量保留原话，并自动添加标题、摘要和必要的小标题；语音转写错误会通过 `speech-fix` 结合上下文纠正。

## 内置 speech-fix 纠错

为了防止语音转写里的错别字、同音词、漏字和中英混输被原样误记，
`capture-to-obsidian` 已内置 `speech-fix` 的核心理解规则：结合完整上下文，
只静默修正高置信度的转写错误，同时保留用户原本的观点和路径、链接、ID、
数字等精确信息。如果另外安装了
[`speech-fix`](https://github.com/moyi-dong/speech-fix)，Codex 会先调用它完成
更完整的语义纠错，再由 `capture-to-obsidian` 写入 Obsidian。

## 随记格式

每篇随记包含：

1. 精确到分钟的本地时间；
2. 一段简短总结；
3. 原则上完整保留的用户原话，只轻度整理明显语音识别和格式问题；
4. 长内容按语义自然添加的小标题；
5. 适用时保留准确的来源链接或 ID。

文件名根据内容自动生成，永不覆盖已有随记。保存后，Codex 只返回新文件链接。

## 安全边界

- 用户明确说“不要记录”时绝不保存。
- 无法读取或含义不清的 ID 会先询问，不把无意义编号直接写成正文。
- 从 Codex 对话中提取内容时，默认只保存用户消息，不混入助手回复。
- 不覆盖已有 `AGENTS.md` 内容或随记文件。
- 网页内容只做总结并保留链接，不复制大段第三方原文。

## 兼容性

- **Codex：** 已支持并测试。
- **其他 Agent Skills 客户端：** 核心格式可能可用，但首次设置和自动触发
  尚未逐一验证。请使用[兼容性 Issue 模板](https://github.com/moyi-dong/capture-to-obsidian/issues/new?template=agent-compatibility.yml)反馈。

## 许可证

MIT
