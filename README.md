# 用 MCPX 让网页 AI 操作自己的 Windows

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

这份教程由作者与 AI 共同整理，面向希望在 ChatGPT 或 Claude 网页中读写自己电脑文件、运行明确命令的普通用户。核对日期：2026-11-18。

**v0.1.0 · 教程首版**：[博客阅读版](https://anagnoraxis.com/builds/mcpx-web-local-windows/) · [下载 Release](https://github.com/7yedeng/mcpx-web-local-guide/releases/tag/v0.1.0) · [来源与借鉴说明](ACKNOWLEDGEMENTS.md)。

电脑上运行 MCPX，网页通过自定义连接器调用它。网页模型负责理解你的要求，本机工具负责执行。直接工具路线无需再启动本机 Codex 模型；网页对话仍受对应平台的账号权限、用量和服务规则约束。

本仓库首先提供教程、示例配置、提示词与脱敏截图。作者自用的 `windows_*` 网关、独立任务执行器、会话隔离和登录后监督属于额外增强，不应把安装原生 MCPX 说成自动获得了这些扩展。

先读 [完整图文教程](GUIDE.zh-CN.md)，或在下载后打开 [HTML 阅读版](PREVIEW.html)。首版包含 15 张真实截图：9 张本轮网页补拍、6 张明确日期的留存素材。Claude 空白添加表单按作者选择提供文字字段说明，保留现有连接，不提供该步截图。干净 Windows 从零安装、手机端和当前 ChatGPT 账号的真实写入/命令验收尚未完成。

## 从哪里开始

1. 普通用户先读[前置条件](docs/prerequisites.md)，确认自己的账号有自定义远程连接器及所需工具权限。
2. 按 [Windows 安装步骤](docs/install-windows.md)准备 MCPX 与 HTTPS/OAuth，再选择 [Claude](docs/claude.md)或 [ChatGPT](docs/chatgpt.md)接入。
3. 想交给 Codex 配置，复制[安装 SOP](prompts/codex-install-sop.zh.md)；接通后用[日常直接工具 SOP](prompts/direct-tools-sop.zh.md)执行任务。
4. 第一次只在演示目录做真实读写验收，再授权真实项目。

安装 MCPX 的程序来自 [opentokenz/mcpx 官方 Release](https://github.com/opentokenz/mcpx/releases/tag/v0.9.19)。本仓库提供接入方法、示例和经验整理，最初上游及其他参考方案见[致谢与来源](ACKNOWLEDGEMENTS.md)。

## 先看你是否适合

- 有一台可以联网、持续运行的 Windows 电脑，并有权操作目标文件。
- 使用的网页账号允许添加自定义远程 MCP 连接器，并且允许调用需要的读、写或执行工具。
- 能提供网页服务可访问的 HTTPS MCP 地址和可用授权流程；只填 `localhost` 不会让云端访问到你的电脑。
- 愿意先用 `C:/MCPX/demo` 测试，而后再按明确任务授权真实项目。

完整要求见[前置条件](docs/prerequisites.md)，操作步骤见 [Windows 安装](docs/install-windows.md)、[ChatGPT 接入](docs/chatgpt.md)和 [Claude 接入](docs/claude.md)。

## 用量和账号条件

OpenAI 公告从 12 月开始 Plus、Pro、Business 的 ChatGPT 与 Codex 共享用量，Instant chat 不计入；本次证据未明确具体切换日与新额度数。请核对[官方用量说明](https://help.openai.com/en/articles/20001625-understanding-chatgpt-and-codex-usage-limits)及账号页面。直接调用本地工具不等于额外赠送额度，也不保证未来计费方式不变。

Claude Free 的自定义连接器条件、连接器数量和当前账号验证见 [Claude 接入](docs/claude.md)。不要由某个账号实测成功推断所有地区、所有账号永久可用。ChatGPT 能添加插件也不等于有任意写入或执行权限，具体入口和限制见 [ChatGPT 接入](docs/chatgpt.md)。

## 教程内容

| 内容 | 入口 |
| --- | --- |
| 需要准备什么，哪些条件可选 | [前置条件](docs/prerequisites.md) |
| Windows 安装和首次启动 | [安装步骤](docs/install-windows.md) |
| 两个平台的网页连接步骤 | [ChatGPT](docs/chatgpt.md)、[Claude](docs/claude.md) |
| 直接给网页 AI 的操作约定 | [直接工具 SOP](prompts/direct-tools-sop.zh.md) |
| 直接交给 Codex 的安装任务 | [安装 SOP 提示词](prompts/codex-install-sop.zh.md) |
| 数据和权限边界 | [安全与隐私](docs/security-and-privacy.md) |
| 连不上、工具旧版、任务结果不明 | [排查方法](docs/troubleshooting.md) |
| 截图来源、脱敏与覆盖范围 | [截图说明](docs/screenshots.md) |
| 上游与许可证说明 | [第三方说明](THIRD_PARTY_NOTICES.md) |
| 最初参考了哪些仓库、具体借鉴什么 | [致谢与来源](ACKNOWLEDGEMENTS.md) |
| 官方来源与验收覆盖范围 | [来源与验证](docs/sources-and-validation.md) |
| 如何反馈、后续维护与取消归档 | [维护说明](docs/maintenance.md) |

## 第一次只验收一个演示目录

先让网页读取 `C:/MCPX/demo/marker.txt`，再根据最新文件版本修改一行，最后在电脑上核对实际内容。若测试命令执行，只运行演示目录内事先明确的无害命令，并检查真实输出和退出码。

成功标准是本地文件和执行结果确实对应这次请求。网页显示“已完成”、连接器列表出现名称或健康检查返回成功，均不能单独替代这个验证。

## 会话和工具名不要混用

原生 MCPX 与作者自用增强网关是两套工具契约。原生方案按实际 `tools/list` 和输入 schema 使用，可能返回 `remote_session_id`；增强网关的 `windows_open_session` 返回 `session_id`。只在同一入口、同一授权和同一会话内续接，不能互换这些标识，也不要恢复绕过会话校验的旧写入口。

## 发布状态

教程的文字、配置示例、提示词和截图已整理完成。截图有明确来源和覆盖范围：本轮补拍覆盖 ChatGPT 的插件入口、添加菜单、示例表单和已有连接详情，以及 Claude 的插件入口、当前 Free 账号达到连接器数量限制的菜单、已有连接和工具权限。ChatGPT 示例表单未提交；Claude 空白添加步骤按作者选择采用文字说明，现有连接保留。截图不等于新授权或当前写入执行已验收。6 张旧图继续保留原日期，详见[截图说明](docs/screenshots.md)。

本仓库未复制作者私有环境、令牌、数据库或浏览器资料。公开文件和截图已完成本地复核；目录检查不替代新电脑的安装验收。

## 许可与维护

本仓库由作者提供的原创文档、提示词、示例和原理图按 [MIT License](LICENSE) 发布。第三方软件、界面截图中的产品内容与商标仍属于原权利人，不因本仓库的 MIT 许可被重新授权。上游 MCPX 的 Apache-2.0 及其他参考仓库的许可分别见[第三方说明](THIRD_PARTY_NOTICES.md)。

账号权限、用量与网页界面以核对日期为准。发现变化请附平台、日期、使用版本和脱敏错误，通过 Issue 提供可复现信息，勿上传口令、令牌、Cookie 或真实用户文件。

GitHub 归档可以取消，恢复后可继续维护。维护步骤及范围见[维护说明](docs/maintenance.md)；本首版按可维护仓库发布。
