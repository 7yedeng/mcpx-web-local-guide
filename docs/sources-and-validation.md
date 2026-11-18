# 来源与验证范围

核对日期：2026-11-18。作者与 AI 共同整理。

| 来源 | 支持的结论 |
| --- | --- |
| [MCPX v0.9.19 README](https://github.com/opentokenz/mcpx/blob/v0.9.19/README.md) | Windows 原生二进制、workspace register、OAuth/Streamable HTTP、工具与会话、命令策略 |
| [MCPX v0.9.19 Release](https://github.com/opentokenz/mcpx/releases/tag/v0.9.19) | Windows amd64 ZIP 与 SHA-256；查询时 latest 仍为此版本 |
| [MCPX 配置结构](https://github.com/opentokenz/mcpx/blob/v0.9.19/internal/config/config.go) | 模板字段名称；配置存在不等于已经部署验收 |
| [MCPX Apache-2.0 LICENSE](https://github.com/opentokenz/mcpx/blob/v0.9.19/LICENSE) | 上游许可证；本仓库不重打包其程序或源码 |
| [MCPX FRP/Caddy 部署](https://github.com/opentokenz/mcpx/blob/v0.9.19/docs/frp-caddy-native.md) | 可选公网传输路线和 OAuth 代理边界 |
| [ChatGPT 自定义 MCP](https://help.openai.com/en/articles/12584461-connecting-custom-mcp-servers-in-chatgpt) | 当前 Plugins 入口、Pro read/fetch、Business/Enterprise/Edu 完整 MCP、工作区条件 |
| [ChatGPT/Codex 用量说明](https://help.openai.com/en/articles/20001625-understanding-chatgpt-and-codex-usage-limits) | 从 12 月起 Plus/Pro/Business 共享用量，Instant chat 例外；本次证据未明确具体切换日或新额度数 |
| [Claude 自定义远程 MCP](https://support.claude.com/en/articles/11175166-get-started-with-custom-connectors-using-remote-mcp) | Free 可用且限一个自定义连接器，云端请求与本机 localhost 区别 |
| [Claude 连接器使用](https://support.claude.com/en/articles/11176164-use-connectors-to-extend-claude-s-capabilities) | 目录连接器与自定义连接器、界面流程；中文以实例所示为准 |

帮助页为滚动更新文档。本次搜索显示的“Updated … days ago/this week”不能直接当作首次发布日期。套餐和 UI 变化后应重新核对。

## 本机证据

- Claude：2026-11-18 Free 网页账号，已有增强网关的 9 个直接工具；读取、新建、哈希更新与命令完成。对应文件为两行标记、59 字节，SHA-256 `88aa291c0bc6cec05666e9d36f74122d04ece054436b3f61c4b3414c141d0f8a`。本轮重新读取文件与计算哈希，结果一致。截图含 Free 标识。
- ChatGPT：本轮补拍当前插件入口、添加菜单、示例表单与已有增强连接详情。表单填示例地址，没有勾选风险确认或提交，也没有创建新授权。另保留 2026-10-20 历史隔离插件和读改验收截图；历史成功不证明 2026-11-18 当前账号的写入能力。
- Claude 当前网页：本轮补拍插件入口、添加菜单、已有连接详情和工具权限，入口图实际收起侧栏。当前 Free 已有一个自定义连接器，添加入口禁用；作者选择保留现有连接，空白添加表单以文字字段说明，不提供该步截图。工具目录展示增强网关的 5 个只读和 4 个写入类入口，不等于本轮重新执行了这些操作。
- 本机服务：本轮只读检查 supervisor/MCPX/gateway/tunnel 均 ready；当前是用户登录触发、普通权限的后台任务。没有为写教程重启服务。
- 截图：15 张真实截图，由 9 张本轮网页补拍和 6 张明确日期的留存图组成；按用户要求本地坐标遮挡/裁切，逐图视觉检查信息见图片 manifest。原始素材不改写，不使用 OCR 或界面重绘，旧图不修改为本轮拍摄日期。
- 配置：按上游固定版本文档及配置结构核对。没有把模板填入真实口令，也没有用它在新电脑完成公网安装。

## 尚未验证

- 当前 ChatGPT 账号的新建授权及真实写入/命令全流程；本轮界面截图只覆盖入口、菜单、未提交示例表单与已有连接。
- Claude 重新授权全流程：本轮保留现有连接，没有删除、重建或重新授权；空白添加表单按作者选择仅作文字说明，不列为教程待补截图。
- Windows 从零安装各步的新截图。
- 一台干净 Windows 按文档从零安装，手机浏览器，注销后无人值守服务。
- 示例配置的真实公网/证书/网络故障注入验收。模板不是已经完成安全审计的生产方案。

教程首版为 [v0.1.0](https://github.com/7yedeng/mcpx-web-local-guide/releases/tag/v0.1.0)，原创文档、提示词、示例和原理图采用 [MIT License](../LICENSE)，第三方权利见[第三方说明](../THIRD_PARTY_NOTICES.md)。最初参考仓库与固定版本见[致谢与来源](../ACKNOWLEDGEMENTS.md)。

公开发布只说明教程文件可取得；全新环境等未执行验收仍不作为已验证结论，也不把它写成“一键安装已验收”。
