# 外部工具连接器

让聊天里的 AI 使用外部网站提供的工具。

这是云吞的系统组件。每项外部工具仍由核心分别安装、确认和授权；连接器只负责按核心交来的地址和鉴权配置转发请求，不向用户开放任意地址代理。

## 支持范围

- 仅访问 HTTPS 公网服务，支持 Streamable HTTP 和旧式 HTTP+SSE。
- 支持无鉴权和核心提供的请求头鉴权；OAuth 请求明确返回不支持。
- 本组件没有独立网页、用户账号、Bot 或可直接启用的工具。工具的安装和使用由云吞负责。
- 主人配置出网代理时使用核心注入的标准代理变量。代理会独立解析目标域名，连接器侧的地址检查不能消除两次解析不一致的风险。

## 部署约束

镜像来自官方 `ghcr.io/wontonapp/wonton-remote-mcp`，按同时包含 `linux/amd64` 和 `linux/arm64` 的 OCI 索引摘要固定。

根文件系统只读，内存上限 64 MiB、PID 上限 64，删除全部 Linux capabilities 并启用 `no-new-privileges`。服务和健康检查各使用两个 Tokio 工作线程；只有一个命名卷挂在 `/data`，不发布宿主端口。

`remote_mcp` 能力指向容器的 8080 端口，不声明托管目录。健康检查调用官方二进制的 `healthcheck` 命令。

## 许可与图标

官方程序的源码工作区许可为 `LicenseRef-Proprietary`；清单按这个许可标注，不把程序重新授权为 MIT。本仓库的包装文件和原创线条图标使用 MIT，见 [LICENSE](LICENSE) 和 [NOTICE](NOTICE)。

开发与发布流程见[插件开发指南](https://github.com/wontonapp/wonton-docs/blob/main/develop.md)。

