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

**自動生成された API キーで WeyToken アカウントを一括作成 — OpenAI 互換の推論 + 9Router コネクタ。**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-63%20served-412991?style=for-the-badge)](https://api.weytoken.com)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![OmniRoute](https://img.shields.io/badge/OmniRoute-connector-8B5CF6?style=for-the-badge)](#-connect-to-omniroute)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 クイックスタート](#-quick-start) · [🎁 無料枠](#-free-tier--how-much-you-get) · [🤖 モデル](#-models--all-63-of-them) · [🧪 API の使い方](#-how-to-use-the-api) · [✅ 検証済み](#-verified-working--every-key-is-tested) · [🧭 仕組み](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [🔌 OmniRoute](#-connect-to-omniroute) · [⚙️ オプション](#️-cli-options) · [🛰️ プロキシ](#️-behind-a-proxy) · [🐞 問題](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ これが存在する理由

WeyToken（微元算力）は、登録フォームが**キャプチャ不要**（`turnstile_check: false`）の New API パネルです — メールアドレス + 6文字のコードがすべての関門で、新しいアカウントには約 **150万クォータ単位**が付与されます（最初の呼び出し後の実測残高: 1,499,887、`quota_per_unit` 500,000）。料金表には**63行**が含まれ、ゼロレートのメディアモデル（`doubao-seedream-*`、`gpt-image-2-count`）も含まれます。GitHub OAuth も代替の入口として提供されており、チャットで検証済みのデフォルトは `deepseek-v4.1-flash` です。

## 🎁 無料枠 — どれだけ得られるか

| 項目 | 値 |
| --- | --- |
| 開始クォータ | **約1,500,000単位**（実測、最初のチャット直後の `quotaEnd`、`quota_per_unit` 500,000） |
| 登録 | メールアドレス + 6文字のコード — **キャプチャなし**（`turnstile_check: false`） |
| 代替の入口 | GitHub OAuth（`github_oauth: true`） |
| カタログ | **63の料金行**、ゼロレート: `doubao-seedream-5-0-pro-260628`、`doubao-seedream-5-0-260128`、`gpt-image-2-count` |
| v4.1 ライン | `deepseek-v4.1-flash` レート 0.85、グループ `default` |
| メール経路 | Mailtrap の受信アドレスがこのレーンで動作、ブローカーのボックスも通過 |

> 💡 無料枠は WeyToken によって設定され、変更される可能性があります。最新の数値は [api.weytoken.com](https://api.weytoken.com) で確認してください — そしてツール自身のゲートに判断を委ねてください:

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 モデル — 全63モデル

料金表には**63行**が含まれます。チャットで検証済みのデフォルト:

| モデル ID | 備考 |
| --- | --- |
| `deepseek-v4.1-flash` | `default` で実際のコンテンツとともに **200**、小さな呼び出しごとにクォータ差分は約113 |

ゼロレートのメディア行（レート0で無料、画像/動画）: `doubao-seedream-5-0-pro-260628`、`doubao-seedream-5-0-260128`、`gpt-image-2-count`。

```bash
curl https://api.weytoken.com/v1/chat/completions \
  -H "Authorization: Bearer VOMujg…" -H "Content-Type: application/json" \
  -d '{"model":"deepseek-v4.1-flash","messages":[{"role":"user","content":"hi"}],"max_tokens":8}'
```

## 🧪 API の使い方

ベース URL **`https://api.weytoken.com/v1`** — OpenAI 互換。

```python
from openai import OpenAI
c = OpenAI(base_url="https://api.weytoken.com/v1", api_key="VOMujg…")
print(c.chat.completions.create(model="deepseek-v4.1-flash",
      messages=[{"role":"user","content":"hi"}]).choices[0].message.content)
```

## 🛠 クイックスタート

```bash
cd weytoken-bulk-apikey
printf 'socks5://127.0.0.1:1080\n' > proxies.txt
node src/launcher.js

# or: node src/cli.js --count 30
```

`keys.txt` は実際のチャット補完が成功した後にのみ書き込まれます。`accounts.json` にはユーザー名 / メールアドレス / キー ID が保持されます。

### 🖱️ ランチャー

ランチャーはプロキシリストと件数を尋ね、それらを記憶して CLI を実行します — 発行、検証、出口のスクリーニング、設定、ヘルプ、9Router への接続。

## ✅ 動作検証済み — すべてのキーがテストされています

| チェック | 結果 |
| --- | --- |
| `GET /api/status` | **200** — 登録オン、メール認証オン、**キャプチャオフ** |
| `GET /api/verification?email=…` | **200** `{"message":"","success":true}` |
| 登録 + ログイン | **200** — セッション取得 |
| `POST /api/token/`（+ 表示） | **200** — キーは一度だけ書き込まれる |
| `POST /v1/chat/completions`（`deepseek-v4.1-flash`）— 書き込まれる前の**すべての**キーに対して | 実際のコンテンツとともに **200**、最初の呼び出し後のクォータ 1,499,887 |
| チャットチェックに失敗したキー | 書き込まれない — アカウントは `accounts.json` に残る |

```bash
node src/cli.js --verify-only
node src/cli.js --screen
```

## 🧭 仕組み

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: WeyToken page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://api.weytoken.com/v1/chat/completions → real content = the key is live
```

| ステップ | 呼び出し | 備考 |
| --- | --- | --- |
| status | `GET /api/status` | ゲート: 登録オン、メール認証オン、キャプチャ**オフ** |
| コード送信 | `GET /api/verification?email=…` | 受信トレイからの6文字コード |
| 登録 | `POST /api/user/register` | メールアドレス + コード + パスワード |
| ログイン | `POST /api/user/login` | セッショントークン |
| キー | `POST /api/token/`（+ 表示） | 一度だけディスクに書き込まれる |
| 証明 | `POST /v1/chat/completions` | コンテンツ付きのチャット200 = キーが有効 |

## ⚙️ CLI オプション

```bash
node src/cli.js --count N        # mint N keys
node src/cli.js --verify-only    # re-verify every key in keys.txt
node src/cli.js --screen         # exit pool self-test
node src/cli.js --connect-9router
node src/cli.js --help
```

| 環境変数 | デフォルト | 意味 |
| --- | --- | --- |
| `WEYTOKEN_PROXIES` | `proxies.txt` | 出口プール、1行につき1プロキシ |
| `WEYTOKEN_MODEL` | `deepseek-v4.1-flash` | ライブ検証に使用するモデル |

## 🛰️ プロキシの背後で

**オプション — 規模が大きい場合に推奨（IPごとの制限、BAN）。** プロキシなしでもツールは動作します。

- **プールあり:** `--pool <file>` を渡す（または `WEYTOKEN_PROXIES` を設定する）と、すべてのリクエストがそのリストの出口を経由します — 死んだ回線はその試行を失敗させ、ツールは次へ進みます（プールが提供されている間はフェイルクローズ、プールは使用時に再プローブされます）。
- **プールなし:** CLI は `running WITHOUT a proxy (direct)` と表示し、直接接続します。登録、チェックイン、キー発行、検証について何も変わりません — パケットが出ていく場所だけが変わります。
- どちらのモードでも `--screen` を実行してください: 実際の実行前に、利用可能な出口（または直接ルート）をプロバイダーに対してテストします。

## 📄 ファイル

| パス | 内容 |
| --- | --- |
| `keys.txt` | 1行につき1つのライブ検証済みキー — git 無視 |
| `accounts.json` | ユーザー名 / メールアドレス / キー ID |
| `proxies.txt` | あなたの出口 — git 無視 |
| `outputs/` | 実行ログ — git 無視 |

## 🔌 9Router への接続

[9Router](https://9router.com) はセルフホスト型の OpenAI 互換ルーターです:

```bash
node src/connect-9router.js --base http://localhost:20128 --password 123456
```

デフォルト: `--prefix weytoken`、`--base-url https://api.weytoken.com/v1`、`--model deepseek-v4.1-flash`。モデルは `weytoken/<model id>` として提供されます。

## 🔌 OmniRoute への接続

[OmniRoute](https://omniroute.online) はセルフホスト型の OpenAI 互換ルーターです:

```bash
node src/connect-omniroute.js --base http://localhost:20128 --password 123456
```

デフォルト: `--prefix weytoken`、`--base-url https://api.weytoken.com/v1`、`--model deepseek-v4.1-flash`。モデルは `weytoken/<model id>` として提供されます。

## 🔑 1つのキーで得られるもの

| | |
| --- | --- |
| 🏢 プロバイダー | **WeyToken（微元算力）** — [api.weytoken.com](https://api.weytoken.com) |
| 🎁 プロバイダーが提供するもの | 約150万の開始クォータ、キャプチャ不要の登録、63行のカタログ |
| 🔑 1つのキーで得られるもの | 独自のアカウント上の1つのキー: `https://api.weytoken.com/v1` のカタログ ID |
| 🌐 API の形式 | OpenAI 互換: `POST https://api.weytoken.com/v1/chat/completions` |
| 🔒 あなたのデータ | `api.weytoken.com` とあなたの受信トレイバックエンドのみが接触されます |

## 🐞 問題

WeyToken が登録の形式やクォータのステップを変更した場合、ツールはそこで停止し、その旨を報告します。コマンドと正確な出力を添えて **[issue を開いてください](../../issues)**。

## 📄 ライセンス

MIT © 2026 — [LICENSE](LICENSE) を参照。

<div align="center">

**[MIT](LICENSE)** © 2026 weytoken-bulk-apikey contributors

</div>