# Windows 安装与联网：先跑通原生 MCPX

本文与 AI 共同整理，核对日期 2026-11-18。示例均为占位地址，不能直接照抄为已部署服务。

## 1. 先准备这四件事

1. 一台可以保持唤醒、联网的 Windows 电脑；本文二进制路线针对 x64。
2. 可以使用自定义远程 MCP 的网页账号；Claude Free 当前限一个自定义连接器，ChatGPT 需先核对套餐与写入权限。
3. 一条能从公网访问本机服务的可信 HTTPS 通道，包含 MCP 和 OAuth 端点；不是只让自己的浏览器能打开。
4. 一个专门的空目录，例如 `C:/MCPX/demo`，用于首次验收。

下载二进制的基础路线不要求安装 Codex、Python、Git、Go 或模型 API Key。要构建项目，才另装项目所需的 Node、Python、Git 等工具。源码构建另需上游 `go.mod` 要求的 Go 版本；本次核对为 Go 1.27.2。

## 2. 下载并核对版本

从 [MCPX v0.9.19 官方 Release](https://github.com/opentokenz/mcpx/releases/tag/v0.9.19) 下载 `mcpx_0.9.19_windows_amd64.zip`，解压到 `C:/MCPX/bin`。不要使用来源不明的重打包版本。

本次 GitHub Release 资产记录的 ZIP SHA-256：

```text
7b8c8b619d9596d77ae92969fa6abf898abc5a372df9e5aa40aa8e34422fa678
```

用 PowerShell 检查你下载的 ZIP；路径换成自己的下载位置：

```powershell
Get-FileHash -Algorithm SHA256 -LiteralPath "$env:USERPROFILE/Downloads/mcpx_0.9.19_windows_amd64.zip"
& 'C:/MCPX/bin/mcpx.exe' -version
& 'C:/MCPX/bin/mcpx.exe' -h
```

升级后重新按对应 Release 校验，不把上面的哈希用于其他版本。

## 3. 独立运行目录与测试目录

在专用 PowerShell 窗口设置进程环境变量，不覆盖已经运行的服务：

```powershell
$env:MCPX_HOME = 'C:/MCPX/runtime'
New-Item -ItemType Directory -Path 'C:/MCPX/runtime','C:/MCPX/demo' -Force
Set-Content -LiteralPath 'C:/MCPX/demo/marker.txt' -Value 'MCPX_READ_OK' -Encoding UTF8
```

把本仓库的 [配置模板](../configs/config.oauth.example.yaml) 复制到 `C:/MCPX/runtime/config.yaml`，在本地编辑：

- `password` 换成唯一的强口令，不放进聊天、截图或 Git。
- `server_url` 换成自己的公网 HTTPS origin，结尾不带 `/mcp`。
- `workspaces` 先只保留 demo；客户端填写的连接地址才是 `https://mcp.example.com/mcp`。

模板不是可直接上线的配置。公网代理、域名和口令未准备好时不要启动公网接入。

模板只允许一条完整匹配的 `echo MCPX_CONNECTED_OK` 命令，其他命令拒绝；关闭了本地 Skill 和上游 MCP 的自动发现。文件操作仍需客户端建立 Remote Session。原生 MCPX 还可能公开截图等支持工具，工具是否出现以实际 `tools/list` 为准；不要把“只注册 demo”说成系统级沙箱。

本教程模板保留 `disable_localhost_protection: false` 与 `trust_proxy_headers: false`，配套的 Tunnel 模板把本地 origin Host 固定为 `127.0.0.1:9090`；公开 OAuth origin 则由 `server_url` 明确设置。不要直接开放本机端口。上游 FRP/Caddy 另一种反代示例会使用不同的 Host 配置，迁移时要成套核对，不能为了消除报错一律关闭 Host、TLS 或 OAuth 校验。

如果已经由配置模板注册 demo，不需要重复注册。另加一个目录的原生命令是：

```powershell
& 'C:/MCPX/bin/mcpx.exe' workspace register 'C:/MCPX/demo'
```

该命令只注册目录，不启动服务。不要把其他版本的 `--workspace` 示例与此命令混用。

## 4. 建立 HTTPS 通道

常见选择有两种，不需要两种都装：

| 路线 | 需要准备 | 更适合谁 |
| --- | --- | --- |
| Cloudflare Tunnel | 自己控制的域名、相应 Cloudflare 配置、cloudflared、本机到边缘的可用网络 | 已在使用 Cloudflare 的人 |
| FRP + Caddy | VPS、域名、服务端 Caddy/frps、本机 frpc | 已有 VPS、希望自管入口的人 |

教程不把 VPS、Cloudflare、代理三项都写成必需。代理只在实际网络需要时加入。公网 HTTPS 是需求，具体供应商是选择。

完整的 FRP/Caddy 配置使用 [上游原生部署教程](https://github.com/opentokenz/mcpx/blob/v0.9.19/docs/frp-caddy-native.md)。本仓库没有一键创建 DNS、证书或公网隧道的脚本，也未在一台全新 Windows 上重装这条路线。

### 选择 Cloudflare 时的顺序

先从 [官方渠道安装 cloudflared](https://developers.cloudflare.com/tunnel/get-started/)，确认你有该域名与 Tunnel 的管理权限。以下采用本地管理隧道的官方命令，不是立即执行的脚本：

```powershell
& 'C:/MCPX/bin/cloudflared.exe' tunnel login
& 'C:/MCPX/bin/cloudflared.exe' tunnel create mcpx-demo
```

第一条会让你在浏览器登录并授权；第二条会创建 Tunnel 和本地凭据。不要把凭据内容发进聊天。根据输出的 UUID 与凭据路径，填写本地私有副本的 [Tunnel 配置模板](../configs/cloudflared.example.yml)，另存为 `C:/MCPX/private/cloudflared.yml`。模板中的 `credentials-file` 必须指向真正的文件；不要求把原凭据复制到教程仓库。

在你自己的域名准备好后，以下命令会创建对应 DNS 路由、随后运行隧道；先确认目标域名没有与现有业务冲突：

```powershell
& 'C:/MCPX/bin/cloudflared.exe' tunnel route dns mcpx-demo mcp.example.com
& 'C:/MCPX/bin/cloudflared.exe' tunnel --config 'C:/MCPX/private/cloudflared.yml' run mcpx-demo
```

把示例域名换成自己的域名。不要只转发 `/mcp` 而漏掉认证发现路径。此路线要求能向 Cloudflare 的 7844 端口建立连接；QUIC 用 UDP，HTTP/2 用 TCP。切 HTTP/2 并不会把端口变成 443。如果两者均不可达，先解决网络条件或选择其他传输，不复制作者环境中的隐藏参数补丁。[创建隧道的官方步骤](https://developers.cloudflare.com/tunnel/features/locally-managed-tunnels/create-local-tunnel/)、[配置与 Host 参数](https://developers.cloudflare.com/tunnel/configuration/)

通道应转发整个认证服务需要的路径，包括 `/.well-known/*`、`/mcp` 和 `/mcp/oauth/*`。在 MCP 前再加交互式网页登录墙可能阻止云端客户端访问；需要通过客户端支持的 OAuth 流程完成认证。

仅填写 `http://127.0.0.1:9090/mcp` 无法让 ChatGPT/Claude 云端访问你的电脑；云端的 127.0.0.1 不是你的 Windows。

## 5. 前台启动，再连接网页

```powershell
$env:MCPX_HOME = 'C:/MCPX/runtime'
& 'C:/MCPX/bin/mcpx.exe'
```

先在前台检查错误。默认本机 MCP 地址为 `http://127.0.0.1:9090/mcp`，网页连接器用公网的 HTTPS 地址。不要仅凭裸 `GET /mcp` 的状态码判断成功，要核对 MCP 握手、实际工具清单、建立会话和测试文件读写。

本教程记录的成功案例使用了已有增强网关。上面的上游原生路线基于固定版本文档核对；两者不能合并成“一台新电脑已全程实测”的结论。

## 6. 登录后自动运行

先让前台链路完成读、改、回读和命令验收，再配后台启动。原生 `mcpx -d` 是后台运行，不等于已经注册登录启动任务：

```powershell
$env:MCPX_HOME = 'C:/MCPX/runtime'
& 'C:/MCPX/bin/mcpx.exe' -d
# 需要暂停本教程实例时，在相同 MCPX_HOME 下执行：
& 'C:/MCPX/bin/mcpx.exe' stop
```

`-d` 会处理同一运行目录里的既有后台进程，不能拿生产实例的运行目录来重复试验。

Windows 任务计划程序中，使用当前用户、登录触发、普通权限和明确的程序路径；同时安排网络通道启动。独立脚本必须先设置该实例的 `MCPX_HOME`。任务计划程序的启动成功只代表程序被启动，仍要验收连接器真实调用。

我们当前实例是 Interactive/AtLogOn：**用户登录后运行，可以锁屏或关闭显示器；注销、睡眠、休眠、关机会中断服务。** 它不是无人登录的系统服务，也没有自动提权。文件操作可在锁屏时继续；桌面截图、鼠标键盘和交互式程序另有登录会话要求。

## 7. 首次验收

先让模型查看实际工具 schema。原生 MCPX 正常流程为：

```text
workspace → session → read → edit → read/observe → execute
```

为 demo 创建本对话自己的 `remote_session_id`；先读 marker.txt 取得 SHA，再更新为 `MCPX_WRITE_OK`，回读核对。然后只执行模板允许的 `echo MCPX_CONNECTED_OK`，核对 stdout 和退出码。保留 Task ID 查询长任务，断线后不要重新提交来试探是否执行过。

增强网关的 `windows_*` 工具使用另一套 `session_id`，请用 [对应 SOP](../prompts/direct-tools-sop.zh.md)，不能把字段混用。
