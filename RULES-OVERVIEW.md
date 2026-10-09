# GitHub 规则总览（跨仓库）

> [!IMPORTANT]
> 本文件回答四个问题：**有什么规则、谁来执行、违反后会怎样、去哪里看细节**。
> 规则冲突时，以各仓库自己的规则文件和可执行门禁为准；本文件只做总览和导航。

## 1. 规则分五层

| 层 | 解决什么问题 | 主要载体 | 执行力度 |
|---|---|---|---|
| **可迁移性门禁** | 内容不能绑定旧机器、不能泄露凭据、换机后能恢复 | `codebuddy-skills/PORTABILITY.md`、各仓库 `tools/portability_gate.ps1` | **硬拦截** |
| **AI 入口规则** | AI 打开仓库后先读什么、哪些事必须先问用户、何时不许宣称成功 | `AGENTS.md` / `CODEBUDDY.md` / `CLAUDE.md` / `.cursor/rules/` / `.github/copilot-instructions.md` | 自动加载；文件存在性被门禁警告级检查 |
| **算法库接口规则** | 一个算法只有一个外部接口、最终版可追踪、接口与阅读材料隔离 | `dsp28335-algorithm/ACCESS-RULES.md` | 接口检查为**警告级**；写调不一致属于人工红线 |
| **验证规则** | 什么状态才算真的通过，PC、编译、硬件分别证明什么 | `RESULT: OK` 约定、各仓库测试脚本、`IRREPLACEABLE.md` | 未出现 `RESULT: OK` 不得宣称通过 |
| **恢复与警示规则** | 不能从 GitHub 恢复的内容必须显式列出 | `RECOVER.md`、`IRREPLACEABLE.md` | 缺恢复入口或警示文件为**硬拦截** |

## 2. 可迁移性门禁

### 2.1 硬拦截项

- 硬编码凭据：GitHub / OpenAI / Slack / AWS / Google / GitLab / JWT / 私钥 / 明码密码。
- 敏感文件：`.env`、`*.pem`、`*.key`、`id_rsa*`、`*.pfx`、`gh_token.txt` 等。
- 代码或配置中的机器绑定路径：`C:\Users\<具体用户>`、固定盘符、旧绝对安装路径。
- 缺仓库根 `RECOVER.md`。
- 缺仓库根 `IRREPLACEABLE.md`，或其中没有醒目的 `[!WARNING]` / 不可恢复警示。

### 2.2 警告项

- 缺 AI 入口文件。
- 缺 `README.md`。
- 文档中的旧机器路径示例。
- 脚本文件名含非 ASCII。
- 单文件超过 50 MB。

### 2.3 豁免边界

- `.portability-allow` 只能按路径豁免“机器绑定路径”检查。
- `.portability-allow` **永远不能**豁免凭据扫描或敏感文件扫描。
- 文件内 `portability:ignore-path` 也只跳过路径检查。

### 2.4 fail-closed

- `pre-push` 在缺 PowerShell 7、缺门禁脚本、门禁崩溃或退出码不是 `0/2` 时拒绝推送。
- CI 同样只接受 `exit 0` 或 `exit 2`。
- 自动上传脚本在门禁缺失或异常时中止，不继续上传。

## 3. AI 规则算不算门禁？

**一半算，一半不算。**

- 算门禁的部分：仓库是否放了 AI 入口文件，`portability_gate.ps1` 会检查；缺文件会警告。
- 不算门禁的部分：AI 入口里的语义规则（先问用户、唯一接口、不许假成功）无法靠文件名机械验证，需要 AI 自觉执行并由测试与人工复核兜底。
- 因此 AI 入口是“自动上下文 + 红线索引”，真正的安全底线仍是门禁、CI、测试和 `IRREPLACEABLE.md`。

## 4. 算法库规则

完整细节见 [`dsp28335-algorithm/RULES-OVERVIEW.md`](https://github.com/sabeeeer/dsp28335-algorithm/blob/main/RULES-OVERVIEW.md)
和 [`ACCESS-RULES.md`](https://github.com/sabeeeer/dsp28335-algorithm/blob/main/ACCESS-RULES.md)。

核心只有四条：

1. **唯一接口原则**：一个算法只能有一个外部调用接口，而且必须是最终版。
2. **接口状态要写出来**：`唯一接口 = …（最终版）` / `暂无外部接口` / `非接口` 三态之一。
3. **接口区与非接口区隔离**：最终程序、流程图、原理说明、历代版本不能混放。
4. **写调一致**：README、接入入口表、实际目录和实际函数签名必须指向同一个接口。

## 5. 1-1 羊角波接口重点

- 算法编号：`1-1`。
- 唯一外部接口目录：`1-1Svpwm_YJB/1-1-1SVPWM_PODPWM/`。
- `1-1-2` 是流程图，`1-1-3` 是原理说明，二者都只供阅读，不是接入对象。
- 平台无关核心接口：`yjb_vs_calc(...)`、`yjb_vs_calc_phi(...)`。
- 调制输出桥接点：`gYJB_ModDuty[12]`；接 EPWM 时 `CMPA = duty × TBPRD`。
- 移植时拿算法本体，PWM / 定时 / 串口 / 时钟 / 引脚按目标平台重写；不要把整个 CCS 工程搬进去。

函数签名、12 路映射和调用顺序见算法库总览。

## 6. 每个仓库的入口

| 仓库 | 第一入口 | 规则/边界 |
|---|---|---|
| `codebuddy-skills` | `RECOVER.md` | `PORTABILITY.md`、`IRREPLACEABLE.md` |
| `pc-env-migration` | `00-START-HERE.md` / `RECOVER.md` | `IRREPLACEABLE.md` |
| `DSP-auto-debug` | `RECOVER.md` | `SKILL.md`、`IRREPLACEABLE.md` |
| `dsp28335-algorithm` | `dsp28335-algorithm/RULES-OVERVIEW.md` / `ACCESS-RULES.md` | `IRREPLACEABLE.md` |
| `git-autosnapshot-codebuddy` | `RECOVER.md` | `IRREPLACEABLE.md` |
| `c2000ware-ref` | `RECOVER.md` | `IRREPLACEABLE.md` |

## 7. 最后的不可消除边界

本地 `pre-push` 无法阻止 `git push --no-verify`、GitHub 网页直接编辑或另一台机器推送。
GitHub Actions 只能事后失败，不能撤销已经进入历史的提交。需要连这些路径也强制阻止时，
必须使用 GitHub rulesets / branch protection；这属于服务端权限设置，不在仓库文件本身。
