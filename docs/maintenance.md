# 后续维护与 GitHub 归档

核对日期：2026-11-18。作者与 AI 共同整理。

## 如何反馈

通过仓库 Issue 提供平台、Windows 版本、MCPX 版本、发生日期、期望行为、实际行为和脱敏后的最小复现步骤。报告连接问题时，区分 HTTPS/OAuth 连接、工具目录、会话、文件操作和模型委派；不要为判断是否执行过而重复运行写入或部署。

账号套餐、连接器入口、工具 schema 或上游版本变化后，应更新对应正文与[来源说明](sources-and-validation.md)，明确验证日期。历史实测保留原日期，不能直接改成新版本已通过。

此仓库维护公开教程与配置示例。作者的自用网关、授权数据和运行环境未随此首版分发。

## 归档后还能继续吗

可以。GitHub 归档使仓库只读；拥有仓库管理权限的人可以取消归档，再继续提交和处理 Issue、PR。取消归档沿用原仓库，无需为继续维护另建同名仓库。

操作入口：仓库首页 → **Settings** → **General** → **Danger Zone** → **Unarchive this repository** → 按页面要求输入仓库名并确认。

只能管理自己的仓库或已有管理权限的仓库。对于别人的归档仓库，应联系维护者；需要独立维护时，先核对其许可证，再按许可 fork 或迁移并保留来源。

如果只想固定一个可下载版本、以后还会继续更新，发布 tag/Release 即可。本教程首版使用 `v0.1.0`，保持仓库可维护。

依据：[GitHub 官方归档与取消归档说明](https://docs.github.com/en/repositories/archiving-a-github-repository/archiving-repositories)。
