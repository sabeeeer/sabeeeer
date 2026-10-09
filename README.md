# AI 工作环境（唯一入口）

> **换电脑 / 换 AI 工具 / 换账号时，从这里开始。**
>
> 我的 **GitHub 账号是主要锚点**。代码、知识、规则和记忆尽量放进仓库；
> 系统软件、商业许可证、硬件驱动和明文凭据仍需新机手工/加密补齐。

---

## 核心仓库

| 仓库 | 内容 |
|---|---|
| **[`codebuddy-skills`](https://github.com/sabeeeer/codebuddy-skills)**（private）| **主仓库** —— 自建 skill + AI 长期记忆 + 恢复基础设施 + 可迁移性规矩 |

## 一步恢复（复制即可）

```powershell
git clone git@github.com:sabeeeer/codebuddy-skills.git "$HOME/codebuddy-skills"
pwsh -NoProfile -ExecutionPolicy Bypass -File "$HOME/codebuddy-skills/restore.ps1" -Apply
```

> **如果你是 AI**：请读 `CODEBUDDY.md`（或 `AGENTS.md` / `CLAUDE.md`），
> 按其中的「**第 0 步**」完成**自举 + 记忆恢复**，再按恢复文档补齐系统依赖和凭据。
> 所需规则和脚本在仓库里；运行时、GitHub 凭据和授权软件仍需按 `RECOVER.md` 补齐。

然后读这两个文件：

- **`RECOVER.md`** —— 恢复指南（换电脑 / 换 AI 平台 / 换账号三种场景）
- **`CODEBUDDY.md`** —— **AI 必读的硬性规矩 + 新机自举指令**（各平台入口：`AGENTS.md` / `CLAUDE.md` /
  `.cursor/rules/` / `.github/copilot-instructions.md`）

---

## 规矩（对所有 AI 工具生效）

**任何要上传到 GitHub 的内容，都必须"只凭登录 GitHub 即可重建"。**

- 规范全文：`codebuddy-skills` 的 **`PORTABILITY.md`**（十条条款 + 提交前自检 7 条 + 平台等价表）
- **门禁已随仓库自带**：`tools/portability_gate.ps1`（检查器）+ `.githooks/pre-push`（git 推送前硬拦截）
  —— clone 后执行一次 `git config core.hooksPath .githooks` 即生效（`restore.ps1` 会自动做）
- 拦截级问题：硬编码凭据 · 机器绑定路径 · 敏感文件 · 缺 `RECOVER.md`

**AI 长期记忆**（工具中立，任何平台可读）：`codebuddy-skills` 的 **`memory/ai-memory.md`**。
导入方式见 `memory/README.md`（Codex→`AGENTS.md`、Claude→`CLAUDE.md`、Cursor→`.cursor/rules/`、
Copilot→`.github/copilot-instructions.md`、通用→当系统提示贴入）。

---

## 其他仓库

| 仓库 | 用途 |
|---|---|
| `pc-env-migration`（private）| 整机环境迁移包（Phase A→K 还原 + 验收自检）|
| `git-autosnapshot-codebuddy` | 本地 git 快照 skill |
| `DSP-auto-debug` | TI C2000 / DSP2833x 全自动开发调试 skill |
| `dsp28335-algorithm`（private）| DSP28335 平台无关算法、TI 适配层与 PC 回归 |
| `c2000ware-ref` | C2000Ware SDK 离线快照 |
