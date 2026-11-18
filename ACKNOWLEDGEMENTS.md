# 最初参考的仓库与借鉴范围

这份教程由作者与 AI 共同实践、核对和整理。我们从开源项目与社区方案出发，比较后整理适合 Windows 的网页到本机工作流。感谢以下项目的作者和贡献者。

MCPX 的原生软件和能力来自上游，本仓库发布 Windows 接入教程、示例配置、提示词与脱敏截图。作者自用的 `windows_*` 网关、独立任务执行器、会话隔离和监督程序属于额外增强，其源码未随首版公开；安装原生 MCPX 不会自动获得这些扩展。

核对日期：2026-11-18。下表保留研究采用的固定源码版本，并于本次发布前读取相应 LICENSE 核对许可。固定版本用于说明来源，不表示它现在仍是最新版本，也不等于对整个项目完成安全审计。

## 主要上游和直接相关参考

| 仓库与固定版本 | 我们具体使用或参考了什么 | 许可证 |
| --- | --- | --- |
| [opentokenz/mcpx](https://github.com/opentokenz/mcpx)，`v0.9.19` → [`1b0ec44`](https://github.com/opentokenz/mcpx/commit/1b0ec4481f9e314b7e2e807b1d27accf61d2f48e) | **最初直接采用的上游工具，也是本教程的主要依据。** Windows 二进制、工作区注册、OAuth、Streamable HTTP、会话、文件及命令工具的原始实现来自该项目。读者从上游官方 Release 下载程序；本仓库不重打包其源码或二进制。 | [Apache-2.0](https://github.com/opentokenz/mcpx/blob/1b0ec4481f9e314b7e2e807b1d27accf61d2f48e/LICENSE) |
| [openai/codex](https://github.com/openai/codex)，既有安装对应 [`4ebc485`](https://github.com/openai/codex/commit/4ebc485ca321243727539187169425fcf972f2c4)，后续研究快照 [`88c5856`](https://github.com/openai/codex/commit/88c58562580aa4abe2da6ce4d7676bf171682230) | 可选本机模型任务路线及 app-server 接口的官方依据。区分独立文件/命令操作与启动模型轮次，区分既有安装和新主线；本教程的直接工具路线不要求另行启动 Codex 模型。 | [Apache-2.0](https://github.com/openai/codex/blob/4ebc485ca321243727539187169425fcf972f2c4/LICENSE) |
| [yyjeqhc/webcodex](https://github.com/yyjeqhc/webcodex)，[`3c14196`](https://github.com/yyjeqhc/webcodex/commit/3c1419678e9c5f513e1f0fcc24d48ca1cd897f93) | 最初候选，也是后续并发、任务派发和 Runner 恢复的研究对象。参考其派发 fencing、实例和连接校验设计；未作为本教程运行依赖集成。 | [Apache-2.0](https://github.com/yyjeqhc/webcodex/blob/3c1419678e9c5f513e1f0fcc24d48ca1cd897f93/LICENSE) |
| [Waishnav/devspace](https://github.com/Waishnav/devspace)，[`5597812`](https://github.com/Waishnav/devspace/commit/559781284677e5b128aa44a2c95f8bdb0c72b7c0) | 最初候选，用来比较网页编码入口、工具执行和进程内记录的恢复边界；未集成。 | [MIT](https://github.com/Waishnav/devspace/blob/559781284677e5b128aa44a2c95f8bdb0c72b7c0/LICENSE) |
| [alexanderradahl/mac-developer-bridge](https://github.com/alexanderradahl/mac-developer-bridge)，[`fea70d1`](https://github.com/alexanderradahl/mac-developer-bridge/commit/fea70d1a3c5524164f2159f6063ba685fef91324) | 最初候选及“网页模型思考、本机直接执行”的设计参考。上游要求 macOS，本 Windows 教程没有安装其 macOS 能力。 | [MIT](https://github.com/alexanderradahl/mac-developer-bridge/blob/fea70d1a3c5524164f2159f6063ba685fef91324/LICENSE) |
| [openai/tunnel-client](https://github.com/openai/tunnel-client)，[`15e889b`](https://github.com/openai/tunnel-client/commit/15e889b5d911ab818dab8d09382079859a6d1f9b) | 后续网络研究中的官方 HTTPS 443 传输候选；不是最初选用的底座，本实例也没有迁移到该传输。 | [Apache-2.0](https://github.com/openai/tunnel-client/blob/15e889b5d911ab818dab8d09382079859a6d1f9b/LICENSE) |

MCPX 的 `v0.9.19` 是 annotated tag，上表链接到解引用后的源码 commit，避免把 tag 对象当作源码版本。

## 其他最初比较过的方案

- [XiaoDuoYa/codex-with-chatgpt](https://github.com/XiaoDuoYa/codex-with-chatgpt)：比较 ChatGPT 规划/审查、Codex 执行写入与测试的分工。未集成；它不能直接作为“Codex 无额度时仍可执行模型任务”的依据。
- [steipete/oracle](https://github.com/steipete/oracle)：比较将提示词和文件交给另一模型分析的流程；未集成。
- [LibreChat-AI/LibreChat](https://github.com/LibreChat-AI/LibreChat)：比较自建聊天平台方向；未采用。
- [volcengine/OpenViking](https://github.com/volcengine/OpenViking)：补充线索中的外部记忆方向；未接入本执行链路。

这四项仅保留比较关系与公开链接，没有可引用的当时固定源码快照，本次也未核对其许可证；不将它们列为已分发依赖。

## 本仓库新增的整理工作

- 面向普通用户解释 Windows、账号、HTTPS、OAuth、持续运行与用量条件。
- 整理从上游安装到网页连接、演示目录验收、项目修改和故障恢复的步骤。
- 编写安装与日常操作两套 SOP、示例配置、原理图和版本范围说明。
- 使用真实界面截图进行坐标遮挡与裁切，保留日期、来源和未覆盖的验收项。
- 明确直接工具与模型委派、原生 MCPX 与自用增强的区别，避免将调研方案写成已经上线的功能。

上游权利与原创许可见[第三方说明](THIRD_PARTY_NOTICES.md)；平台规则和本机实测范围见[来源与验证](docs/sources-and-validation.md)。
