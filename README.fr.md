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

**Création en masse de comptes WeyToken avec clés API générées automatiquement — inférence compatible OpenAI + connecteur 9Router.**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-63%20served-412991?style=for-the-badge)](https://api.weytoken.com)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 Démarrage rapide](#-quick-start) · [🎁 Offre gratuite](#-free-tier--how-much-you-get) · [🤖 Modèles](#-models--all-63-of-them) · [🧪 Utiliser l'API](#-how-to-use-the-api) · [✅ Vérifié](#-verified-working--every-key-is-tested) · [🧭 Comment ça marche](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [⚙️ Options](#️-cli-options) · [🛰️ Proxies](#️-behind-a-proxy) · [🐞 Problèmes](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ Pourquoi ce projet existe

WeyToken (微元算力) est un panneau New API dont le formulaire d'inscription est **sans captcha** (`turnstile_check: false`) — e-mail + code à 6 caractères constituent toute la barrière, et le nouveau compte arrive avec environ **1,5 million d'unités de quota** (solde mesuré après le premier appel : 1 499 887 ; `quota_per_unit` 500 000). La grille tarifaire comporte **63 lignes**, y compris des modèles média à ratio zéro (`doubao-seedream-*`, `gpt-image-2-count`), GitHub OAuth est proposé comme porte alternative, et le modèle par défaut vérifié par chat est `deepseek-v4.1-flash`.

## 🎁 Offre gratuite — ce que vous obtenez

| Élément | Valeur |
| --- | --- |
| Quota de départ | **~1 500 000 unités** mesurées (`quotaEnd` juste après le premier chat ; `quota_per_unit` 500 000) |
| Inscription | e-mail + code à 6 caractères — **sans captcha** (`turnstile_check: false`) |
| Porte alternative | GitHub OAuth (`github_oauth: true`) |
| Catalogue | **63 lignes tarifaires** ; ratio zéro : `doubao-seedream-5-0-pro-260628`, `doubao-seedream-5-0-260128`, `gpt-image-2-count` |
| Ligne v4.1 | `deepseek-v4.1-flash` ratio 0.85, groupe `default` |
| Rail mail | Les adresses entrantes Mailtrap ont fonctionné dans la voie ; les boîtes broker passent aussi |

> 💡 L'offre gratuite est définie par WeyToken et peut changer. Consultez les chiffres en direct sur [api.weytoken.com](https://api.weytoken.com) — et laissez la propre barrière de l'outil décider :

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 Modèles — les 63

La grille tarifaire comporte **63 lignes**. Le modèle par défaut vérifié par chat :

| ID du modèle | Remarque |
| --- | --- |
| `deepseek-v4.1-flash` | **200** avec du contenu réel sur `default` ; delta de quota ~113 par petit appel |

Lignes média à ratio zéro (gratuites à ratio 0, image/vidéo) : `doubao-seedream-5-0-pro-260628`, `doubao-seedream-5-0-260128`, `gpt-image-2-count`.

```bash
curl https://api.weytoken.com/v1/chat/completions \
  -H "Authorization: Bearer VOMujg…" -H "Content-Type: application/json" \
  -d '{"model":"deepseek-v4.1-flash","messages":[{"role":"user","content":"hi"}],"max_tokens":8}'
```

## 🧪 Comment utiliser l'API

URL de base **`https://api.weytoken.com/v1`** — compatible OpenAI.

```python
from openai import OpenAI
c = OpenAI(base_url="https://api.weytoken.com/v1", api_key="VOMujg…")
print(c.chat.completions.create(model="deepseek-v4.1-flash",
      messages=[{"role":"user","content":"hi"}]).choices[0].message.content)
```

## 🛠 Démarrage rapide

```bash
cd weytoken-bulk-apikey
printf 'socks5://127.0.0.1:1080\n' > proxies.txt
node src/launcher.js

# or: node src/cli.js --count 30
```

`keys.txt` n'est écrit qu'après une complétion de chat en direct ; `accounts.json` conserve le nom d'utilisateur / l'e-mail / l'id de clé.

### 🖱️ Le lanceur

Le lanceur demande la liste de proxies et le nombre, les mémorise, et exécute le CLI — création, vérification, test des sorties, paramètres, aide, Connect to 9Router.

## ✅ Vérifié fonctionnel — chaque clé est testée

| Vérification | Résultat |
| --- | --- |
| `GET /api/status` | **200** — inscription activée, vérification e-mail activée, **captcha désactivé** |
| `GET /api/verification?email=…` | **200** `{"message":"","success":true}` |
| inscription + connexion | **200** — session en main |
| `POST /api/token/` (+ révélation) | **200** — clé écrite une fois |
| `POST /v1/chat/completions` (`deepseek-v4.1-flash`) — pour **chaque** clé avant son écriture | **200** avec du contenu réel ; quota 1 499 887 après le premier appel |
| une clé qui échoue à sa vérification de chat | jamais écrite — le compte reste dans `accounts.json` |

```bash
node src/cli.js --verify-only
node src/cli.js --screen
```

## 🧭 Comment ça marche

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: WeyToken page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://api.weytoken.com/v1/chat/completions → real content = the key is live
```

| Étape | Appel | Remarques |
| --- | --- | --- |
| statut | `GET /api/status` | barrières : inscription activée, vérification e-mail activée, captcha **désactivé** |
| envoi du code | `GET /api/verification?email=…` | code à 6 caractères depuis la boîte de réception |
| inscription | `POST /api/user/register` | e-mail + code + mot de passe |
| connexion | `POST /api/user/login` | jeton de session |
| clé | `POST /api/token/` (+ révélation) | écrite sur disque une fois |
| preuve | `POST /v1/chat/completions` | chat-200 avec contenu = la clé est active |

## ⚙️ Options du CLI

```bash
node src/cli.js --count N        # mint N keys
node src/cli.js --verify-only    # re-verify every key in keys.txt
node src/cli.js --screen         # exit pool self-test
node src/cli.js --connect-9router
node src/cli.js --help
```

| Env | Défaut | Signification |
| --- | --- | --- |
| `WEYTOKEN_PROXIES` | `proxies.txt` | pool de sorties, une ligne par proxy |
| `WEYTOKEN_MODEL` | `deepseek-v4.1-flash` | modèle pour la vérification en direct |

## 🛰️ Derrière un proxy

**Optionnel — recommandé à grande échelle (limites par IP, bannissements).** L'outil fonctionne aussi sans proxy.

- **Avec un pool :** passez `--pool <file>` (ou définissez `WEYTOKEN_PROXIES`) et chaque requête passe par une sortie de cette liste — une ligne morte fait échouer cette tentative et l'outil passe à la suivante (fail-closed tant qu'un pool est fourni ; le pool est re-testé à l'usage).
- **Sans pool :** le CLI affiche `running WITHOUT a proxy (direct)` et se connecte directement. Rien ne change pour l'inscription, le check-in, la création de clé ou la vérification — seul le point de départ des paquets change.
- Lancez `--screen` dans l'un ou l'autre mode : il teste les sorties dont vous disposez (ou la route directe) contre le fournisseur avant une exécution réelle.

## 📄 Fichiers

| Chemin | Contenu |
| --- | --- |
| `keys.txt` | une clé vérifiée en direct par ligne — ignoré par git |
| `accounts.json` | nom d'utilisateur / e-mail / id de clé |
| `proxies.txt` | vos sorties — ignoré par git |
| `outputs/` | journaux d'exécution — ignorés par git |

## 🔌 Connexion à 9Router

[9Router](https://9router.com) est un routeur auto-hébergé compatible OpenAI :

```bash
node src/connect-9router.js --base http://localhost:20128 --password 123456
```

Défauts : `--prefix weytoken`, `--base-url https://api.weytoken.com/v1`, `--model deepseek-v4.1-flash`. Les modèles sont servis sous `weytoken/<model id>`.

## 🔑 Ce qu'une clé vous donne

| | |
| --- | --- |
| 🏢 Fournisseur | **WeyToken (微元算力)** — [api.weytoken.com](https://api.weytoken.com) |
| 🎁 Ce que le fournisseur offre | ~1,5 M de quota de départ, inscription sans captcha, catalogue de 63 lignes |
| 🔑 Ce qu'une clé vous donne | une clé sur son propre compte : les ids du catalogue sur `https://api.weytoken.com/v1` |
| 🌐 Forme de l'API | Compatible OpenAI : `POST https://api.weytoken.com/v1/chat/completions` |
| 🔒 Vos données | seuls `api.weytoken.com` et votre backend de boîte de réception sont contactés |

## 🐞 Problèmes

Si WeyToken modifie la forme de l'inscription ou son étape de quota, l'outil s'arrête là et le signale. **[Ouvrez un ticket](../../issues)** avec la commande et la sortie exacte.

## 📄 Licence

MIT © 2026 — voir [LICENSE](LICENSE).

<div align="center">

**[MIT](LICENSE)** © 2026 weytoken-bulk-apikey contributors

</div>