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

**WeyToken-Konten im Bulk anlegen mit automatisch generierten API-Schlüsseln — OpenAI-kompatible Inferenz + 9Router-Connector.**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-63%20served-412991?style=for-the-badge)](https://api.weytoken.com)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 Schnellstart](#-quick-start) · [🎁 Free-Tier](#-free-tier--how-much-you-get) · [🤖 Modelle](#-models--all-63-of-them) · [🧪 Die API nutzen](#-how-to-use-the-api) · [✅ Verifiziert](#-verified-working--every-key-is-tested) · [🧭 So funktioniert es](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [⚙️ Optionen](#️-cli-options) · [🛰️ Proxies](#️-behind-a-proxy) · [🐞 Issues](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ Warum es das gibt

WeyToken (微元算力) ist ein New-API-Panel, dessen Registrierungsformular **captcha-frei** ist (`turnstile_check: false`) — E-Mail + 6-stelliger Code sind die gesamte Hürde, und das frische Konto landet bei ungefähr **1,5 Mio. Quota-Einheiten** (gemessenes Guthaben nach dem ersten Aufruf: 1.499.887; `quota_per_unit` 500.000). Die Preisliste umfasst **63 Zeilen**, darunter Zero-Ratio-Medienmodelle (`doubao-seedream-*`, `gpt-image-2-count`), GitHub OAuth wird als alternative Tür angeboten, und der chat-verifizierte Standard ist `deepseek-v4.1-flash`.

## 🎁 Free-Tier — wie viel du bekommst

| Was | Wert |
| --- | --- |
| Startguthaben | **~1.500.000 Einheiten** gemessen (`quotaEnd` direkt nach dem ersten Chat; `quota_per_unit` 500.000) |
| Registrierung | E-Mail + 6-stelliger Code — **kein Captcha** (`turnstile_check: false`) |
| Alternative Tür | GitHub OAuth (`github_oauth: true`) |
| Katalog | **63 Preiszeilen**; Zero-Ratio: `doubao-seedream-5-0-pro-260628`, `doubao-seedream-5-0-260128`, `gpt-image-2-count` |
| v4.1-Linie | `deepseek-v4.1-flash` Ratio 0,85, Gruppe `default` |
| Mail-Rail | Mailtrap-Inbound-Adressen funktionierten in der Lane; Broker-Boxen gehen ebenfalls durch |

> 💡 Der Free-Tier wird von WeyToken festgelegt und kann sich ändern. Prüfe die Live-Zahlen unter [api.weytoken.com](https://api.weytoken.com) — und lass das Gate des Tools selbst entscheiden:

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 Modelle — alle 63

Die Preisliste umfasst **63 Zeilen**. Der chat-verifizierte Standard:

| Modell-ID | Hinweis |
| --- | --- |
| `deepseek-v4.1-flash` | **200** mit echtem Inhalt auf `default`; Quota-Delta ~113 pro Mini-Aufruf |

Zero-Ratio-Medienzeilen (kostenlos bei Ratio 0, Bild/Video): `doubao-seedream-5-0-pro-260628`, `doubao-seedream-5-0-260128`, `gpt-image-2-count`.

```bash
curl https://api.weytoken.com/v1/chat/completions \
  -H "Authorization: Bearer VOMujg…" -H "Content-Type: application/json" \
  -d '{"model":"deepseek-v4.1-flash","messages":[{"role":"user","content":"hi"}],"max_tokens":8}'
```

## 🧪 So nutzt du die API

Base-URL **`https://api.weytoken.com/v1`** — OpenAI-kompatibel.

```python
from openai import OpenAI
c = OpenAI(base_url="https://api.weytoken.com/v1", api_key="VOMujg…")
print(c.chat.completions.create(model="deepseek-v4.1-flash",
      messages=[{"role":"user","content":"hi"}]).choices[0].message.content)
```

## 🛠 Schnellstart

```bash
cd weytoken-bulk-apikey
printf 'socks5://127.0.0.1:1080\n' > proxies.txt
node src/launcher.js
# or: node src/cli.js --count 30
```

`keys.txt` wird erst nach einer Live-Chat-Completion geschrieben; `accounts.json` behält den Benutzernamen / die E-Mail / die Key-ID.

### 🖱️ Der Launcher

Der Launcher fragt nach der Proxy-Liste und der Anzahl, merkt sie sich und führt die CLI aus — Mint, Verify, Screen-Exits, Einstellungen, Hilfe, Connect to 9Router.

## ✅ Verifiziert funktionierend — jeder Schlüssel wird getestet

| Prüfung | Ergebnis |
| --- | --- |
| `GET /api/status` | **200** — Registrierung an, E-Mail-Verifizierung an, **Captcha aus** |
| `GET /api/verification?email=…` | **200** `{"message":"","success":true}` |
| register + login | **200** — Session in der Hand |
| `POST /api/token/` (+ reveal) | **200** — Schlüssel einmal geschrieben |
| `POST /v1/chat/completions` (`deepseek-v4.1-flash`) — für **jeden** Schlüssel, bevor er geschrieben wird | **200** mit echtem Inhalt; Quota 1.499.887 nach dem ersten Aufruf |
| ein Schlüssel, der seine Chat-Prüfung nicht besteht | wird nie geschrieben — das Konto bleibt in `accounts.json` |

```bash
node src/cli.js --verify-only
node src/cli.js --screen
```

## 🧭 So funktioniert es

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: WeyToken page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://api.weytoken.com/v1/chat/completions → real content = the key is live
```

| Schritt | Aufruf | Hinweise |
| --- | --- | --- |
| status | `GET /api/status` | Gates: Registrierung an, E-Mail-Verifizierung an, Captcha **aus** |
| Code senden | `GET /api/verification?email=…` | 6-stelliger Code aus dem Postfach |
| registrieren | `POST /api/user/register` | E-Mail + Code + Passwort |
| login | `POST /api/user/login` | Session-Token |
| key | `POST /api/token/` (+ reveal) | einmal auf die Festplatte geschrieben |
| proof | `POST /v1/chat/completions` | Chat-200 mit Inhalt = der Schlüssel ist live |

## ⚙️ CLI-Optionen

```bash
node src/cli.js --count N        # mint N keys
node src/cli.js --verify-only    # re-verify every key in keys.txt
node src/cli.js --screen         # exit pool self-test
node src/cli.js --connect-9router
node src/cli.js --help
```

| Env | Standard | Bedeutung |
| --- | --- | --- |
| `WEYTOKEN_PROXIES` | `proxies.txt` | Exit-Pool, eine Zeile pro Proxy |
| `WEYTOKEN_MODEL` | `deepseek-v4.1-flash` | Modell für die Live-Verifizierung |

## 🛰️ Hinter einem Proxy

**Optional — bei Skalierung empfohlen (Limits pro IP, Bans).** Das Tool funktioniert auch ohne Proxy.

- **Mit einem Pool:** übergib `--pool <file>` (oder setze `WEYTOKEN_PROXIES`), und jede Anfrage geht durch einen Exit aus dieser Liste — eine tote Leitung lässt diesen Versuch fehlschlagen, und das Tool geht zum nächsten über (fail-closed, solange ein Pool angegeben ist; der Pool wird bei der Nutzung erneut geprüft).
- **Ohne Pool:** die CLI gibt `running WITHOUT a proxy (direct)` aus und verbindet direkt. An Registrierung, Check-in, Key-Minting oder Verifizierung ändert sich nichts — nur daran, wo die Pakete rausgehen.
- Führe `--screen` in beiden Modi aus: es testet die Exits, die du hast (oder die direkte Route), gegen den Anbieter, bevor ein echter Lauf startet.

## 📄 Dateien

| Pfad | Inhalt |
| --- | --- |
| `keys.txt` | ein live-verifizierter Schlüssel pro Zeile — von Git ignoriert |
| `accounts.json` | Benutzername / E-Mail / Key-ID |
| `proxies.txt` | deine Exits — von Git ignoriert |
| `outputs/` | Run-Logs — von Git ignoriert |

## 🔌 Mit 9Router verbinden

[9Router](https://9router.com) ist ein selbst gehosteter, OpenAI-kompatibler Router:

```bash
node src/connect-9router.js --base http://localhost:20128 --password 123456
```

Standardwerte: `--prefix weytoken`, `--base-url https://api.weytoken.com/v1`, `--model deepseek-v4.1-flash`. Modelle werden als `weytoken/<model id>` bereitgestellt.

## 🔑 Was ein Schlüssel dir bringt

| | |
| --- | --- |
| 🏢 Anbieter | **WeyToken (微元算力)** — [api.weytoken.com](https://api.weytoken.com) |
| 🎁 Was der Anbieter gibt | ~1,5 Mio. Startguthaben, captcha-freie Registrierung, 63-zeiliger Katalog |
| 🔑 Was ein Schlüssel dir bringt | ein Schlüssel auf seinem eigenen Konto: die Katalog-IDs auf `https://api.weytoken.com/v1` |
| 🌐 API-Form | OpenAI-kompatibel: `POST https://api.weytoken.com/v1/chat/completions` |
| 🔒 Deine Daten | nur `api.weytoken.com` und dein Postfach-Backend werden kontaktiert |

## 🐞 Issues

Wenn WeyToken die Registrierungsform oder seinen Quota-Schritt ändert, stoppt das Tool dort und sagt es dir. **[Öffne ein Issue](../../issues)** mit dem Befehl und der exakten Ausgabe.

## 📄 Lizenz

MIT © 2026 — siehe [LICENSE](LICENSE).

<div align="center">

**[MIT](LICENSE)** © 2026 weytoken-bulk-apikey contributors

</div>