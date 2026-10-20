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

**Bulk-create WeyToken accounts with auto-generated API keys — OpenAI-compatible inference + 9Router connector.**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-63%20served-412991?style=for-the-badge)](https://api.weytoken.com)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 Quick start](#-quick-start) · [🎁 Free tier](#-free-tier--how-much-you-get) · [🤖 Models](#-models--all-63-of-them) · [🧪 Use the API](#-how-to-use-the-api) · [✅ Verified](#-verified-working--every-key-is-tested) · [🧭 How it works](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [⚙️ Options](#️-cli-options) · [🛰️ Proxies](#️-behind-a-proxy) · [🐞 Issues](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ Why this exists

WeyToken (微元算力) is a New API panel whose register form is **captcha-free** (`turnstile_check: false`) — e-mail + 6-char code is the whole gate, and the fresh account lands with roughly **1.5M quota units** (measured balance after the first call: 1,499,887; `quota_per_unit` 500,000). The pricing sheet carries **63 rows** including zero-ratio media models (`doubao-seedream-*`, `gpt-image-2-count`), GitHub OAuth is offered as an alternative door, and the chat-verified default is `deepseek-v4.1-flash`.

## 🎁 Free tier — how much you get

| What | Value |
| --- | --- |
| Starting quota | **~1,500,000 units** measured (`quotaEnd` right after the first chat; `quota_per_unit` 500,000) |
| Signup | e-mail + 6-char code — **no captcha** (`turnstile_check: false`) |
| Alternative door | GitHub OAuth (`github_oauth: true`) |
| Catalogue | **63 pricing rows**; zero-ratio: `doubao-seedream-5-0-pro-260628`, `doubao-seedream-5-0-260128`, `gpt-image-2-count` |
| v4.1 line | `deepseek-v4.1-flash` ratio 0.85, group `default` |
| Mail rail | Mailtrap inbound addresses worked in the lane; broker boxes also pass |

> 💡 The free tier is set by WeyToken and can change. Check the live numbers at [api.weytoken.com](https://api.weytoken.com) — and let the tool's own gate decide:

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 Models — all 63 of them

Pricing sheet carries **63 rows**. The chat-verified default:

| Model ID | Note |
| --- | --- |
| `deepseek-v4.1-flash` | **200** with real content on `default`; quota delta ~113 per tiny call |

Zero-ratio media rows (free at ratio 0, image/video): `doubao-seedream-5-0-pro-260628`, `doubao-seedream-5-0-260128`, `gpt-image-2-count`.

```bash
curl https://api.weytoken.com/v1/chat/completions \
  -H "Authorization: Bearer VOMujg…" -H "Content-Type: application/json" \
  -d '{"model":"deepseek-v4.1-flash","messages":[{"role":"user","content":"hi"}],"max_tokens":8}'
```

## 🧪 How to use the API

Base URL **`https://api.weytoken.com/v1`** — OpenAI-compatible.

```python
from openai import OpenAI
c = OpenAI(base_url="https://api.weytoken.com/v1", api_key="VOMujg…")
print(c.chat.completions.create(model="deepseek-v4.1-flash",
      messages=[{"role":"user","content":"hi"}]).choices[0].message.content)
```

## 🛠 Quick start

```bash
cd weytoken-bulk-apikey
printf 'socks5://127.0.0.1:1080\n' > proxies.txt
node src/launcher.js
# or: node src/cli.js --count 30
```

`keys.txt` is written only after a live chat completion; `accounts.json` keeps the username / e-mail / key id.

### 🖱️ The launcher

The launcher asks for the proxy list and count, remembers them, and runs the CLI — mint, verify, screen exits, settings, help, Connect to 9Router.

## ✅ Verified working — every key is tested

| Check | Result |
| --- | --- |
| `GET /api/status` | **200** — register on, e-mail verification on, **captcha off** |
| `GET /api/verification?email=…` | **200** `{"message":"","success":true}` |
| register + login | **200** — session in hand |
| `POST /api/token/` (+ reveal) | **200** — key written once |
| `POST /v1/chat/completions` (`deepseek-v4.1-flash`) — for **every** key before it is written | **200** with real content; quota 1,499,887 after the first call |
| a key that fails its chat check | never written — account stays in `accounts.json` |

```bash
node src/cli.js --verify-only
node src/cli.js --screen
```

## 🧭 How it works

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: WeyToken page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://api.weytoken.com/v1/chat/completions → real content = the key is live
```

| Step | Call | Notes |
| --- | --- | --- |
| status | `GET /api/status` | gates: register on, e-mail verification on, captcha **off** |
| send code | `GET /api/verification?email=…` | 6-char code from the inbox |
| register | `POST /api/user/register` | e-mail + code + password |
| login | `POST /api/user/login` | session token |
| key | `POST /api/token/` (+ reveal) | written to disk once |
| proof | `POST /v1/chat/completions` | chat-200 with content = the key is live |

## ⚙️ CLI options

```bash
node src/cli.js --count N        # mint N keys
node src/cli.js --verify-only    # re-verify every key in keys.txt
node src/cli.js --screen         # exit pool self-test
node src/cli.js --connect-9router
node src/cli.js --help
```

| Env | Default | Meaning |
| --- | --- | --- |
| `WEYTOKEN_PROXIES` | `proxies.txt` | exit pool, one line per proxy |
| `WEYTOKEN_MODEL` | `deepseek-v4.1-flash` | model for the live verification |

## 🛰️ Behind a proxy

**Optional — recommended at scale (per-IP limits, bans).** The tool works without a proxy too.

- **With a pool:** pass `--pool <file>` (or set `WEYTOKEN_PROXIES`) and every request goes through an exit from that list — a dead line fails that attempt and the tool moves to the next one (fail-closed while a pool is provided; the pool is re-probed at use).
- **Without a pool:** the CLI prints `running WITHOUT a proxy (direct)` and connects directly. Nothing about sign-up, check-in, key minting or verification changes — only where the packets leave from.
- Run `--screen` against either mode: it tests the exits you have (or the direct route) against the provider before a real run.

## 📄 Files

| Path | Contents |
| --- | --- |
| `keys.txt` | one live-verified key per line — git-ignored |
| `accounts.json` | username / e-mail / key id |
| `proxies.txt` | your exits — git-ignored |
| `outputs/` | run logs — git-ignored |

## 🔌 Connect to 9Router

[9Router](https://9router.com) is a self-hosted, OpenAI-compatible router:

```bash
node src/connect-9router.js --base http://localhost:20128 --password 123456
```

Defaults: `--prefix weytoken`, `--base-url https://api.weytoken.com/v1`, `--model deepseek-v4.1-flash`. Models are served as `weytoken/<model id>`.

## 🔑 What one key gets you

| | |
| --- | --- |
| 🏢 Provider | **WeyToken (微元算力)** — [api.weytoken.com](https://api.weytoken.com) |
| 🎁 What the provider gives | ~1.5M starting quota, captcha-free signup, 63-row catalogue |
| 🔑 What one key gets you | one key on its own account: the catalogue ids on `https://api.weytoken.com/v1` |
| 🌐 API shape | OpenAI-compatible: `POST https://api.weytoken.com/v1/chat/completions` |
| 🔒 Your data | only `api.weytoken.com` and your inbox backend are contacted |

## 🐞 Issues

If WeyToken changes the register shape or its quota step, the tool stops there and says so. **[Open an issue](../../issues)** with the command and the exact output.

## 📄 License

MIT © 2026 — see [LICENSE](LICENSE).

<div align="center">

**[MIT](LICENSE)** © 2026 weytoken-bulk-apikey contributors

</div>
