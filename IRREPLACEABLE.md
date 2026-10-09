# 不可自动迁移 / 不可从 GitHub 完整恢复的事项

> [!WARNING]
> 本主页只是入口。GitHub 不能恢复本机软件、硬件、账号登录态或明文凭据。

## 统一边界

- 系统运行时：PowerShell 7、Git、Python。
- 商业软件和许可证。
- 硬件、驱动、仿真器和目标板。
- GitHub token、SSH 私钥、平台账号登录态。
- 加密凭据包、本地未上传改动和个人数据。

详见各仓库根目录的 `IRREPLACEABLE.md` 与 `RECOVER.md`。

## 强制边界（不可由仓库代码消除）

> [!CAUTION]
> 本地 `pre-push` 只能约束正常 `git push`。`git push --no-verify`、GitHub 网页直接编辑、
> 其他机器或外部自动化可以绕过本地 hook。GitHub Actions 只能在事后失败，不能撤销已经进入
> Git 历史的提交。若确需使用绕过通道，必须先在本地运行门禁并确认退出码为 0 或 2。
