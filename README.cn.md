<div align="center">

```
██╗    ██╗███████╗██╗   ██╗████████╗ ██████╗ ██╗  ██╗███████╗███╗   ██╗
██║    ██║██╔════╝╚██╗ ██╔╝╚══██╔══╝██╔═══██╗██║ ██╔╝██╔════╝████╗  ██║
██║ █╗ ██║█████╗   ╚████╔╝    ██║   ██║   ██║█████╔╝ █████╗  ██╔██╗ ██║
██║███╗██║██╔══╝    ╚██╔╝     ██║   ██║   ██║██╔═██╗ ██╔══╝  ██║╚██╗██║
╚███╔███╔╝███████╗   ██║      ██║   ╚██████╔╝██║  ██╗███████╗██║ ╚████║
 ╚══╝╚══╝ ╚══════╝   ╚═╝      ╚═╝    ╚═════╝ ╚═╝  ╚═╝╚══════╝╚═╝  ╚═══╝
```

# weytoken-bulk-apikey

**批量创建 WeyToken 账户并自动生成 API 密钥 — 兼容 OpenAI 的推理 + 9Router 连接器。**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-63%20served-412991?style=for-the-badge)](https://api.weytoken.com)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 快速开始](#-quick-start) · [🎁 免费额度](#-free-tier--how-much-you-get) · [🤖 模型](#-models--all-63-of-them) · [🧪 使用 API](#-how-to-use-the-api) · [✅ 已验证](#-verified-working--every-key-is-tested) · [🧭 工作原理](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [⚙️ 选项](#️-cli-options) · [🛰️ 代理](#️-behind-a-proxy) · [🐞 问题](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ 为什么会有这个项目

WeyToken（微元算力）是一个 New API 面板，其注册表单**无需验证码**（`turnstile_check: false`）——邮箱 + 6 位字符验证码就是全部门槛，新账户注册后大约有 **150 万配额单位**（首次调用后实测余额：1,499,887；`quota_per_unit` 为 500,000）。价格表包含 **63 行**，其中包括零倍率媒体模型（`doubao-seedream-*`、`gpt-image-2-count`），GitHub OAuth 作为备选入口，聊天验证的默认模型是 `deepseek-v4.1-flash`。

## 🎁 免费额度 — 你能获得多少

| 项目 | 值 |
| --- | --- |
| 初始配额 | **约 1,500,000 单位**（实测，首次聊天后 `quotaEnd`；`quota_per_unit` 为 500,000） |
| 注册 | 邮箱 + 6 位字符验证码 — **无需验证码**（`turnstile_check: false`） |
| 备选入口 | GitHub OAuth（`github_oauth: true`） |
| 目录 | **63 行价格表**；零倍率：`doubao-seedream-5-0-pro-260628`、`doubao-seedream-5-0-260128`、`gpt-image-2-count` |
| v4.1 系列 | `deepseek-v4.1-flash` 倍率 0.85，分组 `default` |
| 邮件通道 | Mailtrap 收件地址在该通道中可用；代理邮箱同样通过 |

> 💡 免费额度由 WeyToken 设定，可能发生变化。请到 [api.weytoken.com](https://api.weytoken.com) 查看实时数据 — 并让工具自身的校验来决定：

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 模型 — 全部 63 个

价格表包含 **63 行**。聊天验证的默认模型：

| 模型 ID | 备注 |
| --- | --- |
| `deepseek-v4.1-flash` | 在 `default` 分组下返回 **200** 且有真实内容；每次小调用配额消耗约 113 |

零倍率媒体行（倍率为 0 时免费，图像/视频）：`doubao-seedream-5-0-pro-260628`、`doubao-seedream-5-0-260128`、`gpt-image-2-count`。

```bash
curl https://api.weytoken.com/v1/chat/completions \
  -H "Authorization: Bearer VOMujg…" -H "Content-Type: application/json" \
  -d '{"model":"deepseek-v4.1-flash","messages":[{"role":"user","content":"hi"}],"max_tokens":8}'
```

## 🧪 如何使用 API

基础 URL **`https://api.weytoken.com/v1`** — 兼容 OpenAI。

```python
from openai import OpenAI
c = OpenAI(base_url="https://api.weytoken.com/v1", api_key="VOMujg…")
print(c.chat.completions.create(model="deepseek-v4.1-flash",
      messages=[{"role":"user","content":"hi"}]).choices[0].message.content)
```

## 🛠 快速开始

```bash
cd weytoken-bulk-apikey
printf 'socks5://127.0.0.1:1080\n' > proxies.txt
node src/launcher.js
# or: node src/cli.js --count 30
```

`keys.txt` 仅在实时聊天补全成功后才写入；`accounts.json` 保存用户名 / 邮箱 / 密钥 ID。

### 🖱️ 启动器

启动器会询问代理列表和数量，记住它们，并运行 CLI — 生成、验证、出口自检、设置、帮助、连接到 9Router。

## ✅ 已验证可用 — 每个密钥都经过测试

| 检查 | 结果 |
| --- | --- |
| `GET /api/status` | **200** — 注册开启，邮箱验证开启，**验证码关闭** |
| `GET /api/verification?email=…` | **200** `{"message":"","success":true}` |
| 注册 + 登录 | **200** — 已获得会话 |
| `POST /api/token/`（+ 显示） | **200** — 密钥仅写入一次 |
| `POST /v1/chat/completions`（`deepseek-v4.1-flash`）— 对**每个**密钥在写入前执行 | **200** 且有真实内容；首次调用后配额为 1,499,887 |
| 聊天检查失败的密钥 | 永不写入 — 账户保留在 `accounts.json` 中 |

```bash
node src/cli.js --verify-only
node src/cli.js --screen
```

## 🧭 工作原理

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: WeyToken page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://api.weytoken.com/v1/chat/completions → real content = the key is live
```

| 步骤 | 调用 | 备注 |
| --- | --- | --- |
| 状态 | `GET /api/status` | 门槛：注册开启，邮箱验证开启，验证码**关闭** |
| 发送验证码 | `GET /api/verification?email=…` | 从收件箱获取 6 位字符验证码 |
| 注册 | `POST /api/user/register` | 邮箱 + 验证码 + 密码 |
| 登录 | `POST /api/user/login` | 会话令牌 |
| 密钥 | `POST /api/token/`（+ 显示） | 仅写入磁盘一次 |
| 验证 | `POST /v1/chat/completions` | 聊天返回 200 且有内容 = 密钥可用 |

## ⚙️ CLI 选项

```bash
node src/cli.js --count N        # mint N keys
node src/cli.js --verify-only    # re-verify every key in keys.txt
node src/cli.js --screen         # exit pool self-test
node src/cli.js --connect-9router
node src/cli.js --help
```

| 环境变量 | 默认值 | 含义 |
| --- | --- | --- |
| `WEYTOKEN_PROXIES` | `proxies.txt` | 出口池，每行一个代理 |
| `WEYTOKEN_MODEL` | `deepseek-v4.1-flash` | 用于实时验证的模型 |

## 🛰️ 代理之后

**必需 — 无直连回退。** 每个请求都通过出口发出；该通道实测到疲惫的免费出口导致 `/api/status` 超时（15 行代理池，`TIMEOUT status after 90s`），因此在大规模运行前请先执行 `--screen`。

## 📄 文件

| 路径 | 内容 |
| --- | --- |
| `keys.txt` | 每行一个经过实时验证的密钥 — 已被 git 忽略 |
| `accounts.json` | 用户名 / 邮箱 / 密钥 ID |
| `proxies.txt` | 你的出口 — 已被 git 忽略 |
| `outputs/` | 运行日志 — 已被 git 忽略 |

## 🔌 连接到 9Router

[9Router](https://9router.com) 是一个自托管、兼容 OpenAI 的路由器：

```bash
node src/connect-9router.js --base http://localhost:20128 --password 123456
```

默认值：`--prefix weytoken`、`--base-url https://api.weytoken.com/v1`、`--model deepseek-v4.1-flash`。模型以 `weytoken/<model id>` 形式提供。

## 🔑 一个密钥能给你什么

| | |
| --- | --- |
| 🏢 提供商 | **WeyToken（微元算力）** — [api.weytoken.com](https://api.weytoken.com) |
| 🎁 提供商提供什么 | 约 150 万初始配额，免验证码注册，63 行目录 |
| 🔑 一个密钥能给你什么 | 一个独立账户上的一个密钥：`https://api.weytoken.com/v1` 上的目录 ID |
| 🌐 API 形态 | 兼容 OpenAI：`POST https://api.weytoken.com/v1/chat/completions` |
| 🔒 你的数据 | 仅联系 `api.weytoken.com` 和你的收件箱后端 |

## 🐞 问题

如果 WeyToken 更改了注册形态或其配额步骤，工具会在此处停止并说明原因。请**[提交 issue](../../issues)**，附上命令和确切的输出。

## 📄 许可证

MIT © 2026 — 见 [LICENSE](LICENSE)。

<div align="center">

**[MIT](LICENSE)** © 2026 weytoken-bulk-apikey contributors

</div>