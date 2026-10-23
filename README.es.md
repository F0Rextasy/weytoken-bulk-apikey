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

**Crea cuentas de WeyToken en masa con claves API autogeneradas — inferencia compatible con OpenAI + conector 9Router.**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-63%20served-412991?style=for-the-badge)](https://api.weytoken.com)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![OmniRoute](https://img.shields.io/badge/OmniRoute-connector-8B5CF6?style=for-the-badge)](#-connect-to-omniroute)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 Inicio rápido](#-quick-start) · [🎁 Nivel gratuito](#-free-tier--how-much-you-get) · [🤖 Modelos](#-models--all-63-of-them) · [🧪 Usa la API](#-how-to-use-the-api) · [✅ Verificado](#-verified-working--every-key-is-tested) · [🧭 Cómo funciona](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [🔌 OmniRoute](#-connect-to-omniroute) · [⚙️ Opciones](#️-cli-options) · [🛰️ Proxies](#️-behind-a-proxy) · [🐞 Problemas](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ Por qué existe esto

WeyToken (微元算力) es un panel de New API cuyo formulario de registro está **libre de captcha** (`turnstile_check: false`) — correo electrónico + código de 6 caracteres es toda la barrera, y la cuenta nueva llega con aproximadamente **1,5M de unidades de cuota** (saldo medido tras la primera llamada: 1.499.887; `quota_per_unit` 500.000). La hoja de precios contiene **63 filas**, incluidos modelos multimedia con ratio cero (`doubao-seedream-*`, `gpt-image-2-count`), GitHub OAuth se ofrece como puerta alternativa, y el predeterminado verificado por chat es `deepseek-v4.1-flash`.

## 🎁 Nivel gratuito — cuánto obtienes

| Qué | Valor |
| --- | --- |
| Cuota inicial | **~1.500.000 unidades** medidas (`quotaEnd` justo después del primer chat; `quota_per_unit` 500.000) |
| Registro | correo electrónico + código de 6 caracteres — **sin captcha** (`turnstile_check: false`) |
| Puerta alternativa | GitHub OAuth (`github_oauth: true`) |
| Catálogo | **63 filas de precios**; ratio cero: `doubao-seedream-5-0-pro-260628`, `doubao-seedream-5-0-260128`, `gpt-image-2-count` |
| Línea v4.1 | `deepseek-v4.1-flash` ratio 0.85, grupo `default` |
| Riel de correo | Las direcciones entrantes de Mailtrap funcionaron en el carril; los buzones de intermediarios también pasan |

> 💡 El nivel gratuito lo define WeyToken y puede cambiar. Consulta los números en vivo en [api.weytoken.com](https://api.weytoken.com) — y deja que la propia puerta de la herramienta decida:

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 Modelos — los 63

La hoja de precios contiene **63 filas**. El predeterminado verificado por chat:

| ID del modelo | Nota |
| --- | --- |
| `deepseek-v4.1-flash` | **200** con contenido real en `default`; delta de cuota ~113 por llamada pequeña |

Filas multimedia con ratio cero (gratis a ratio 0, imagen/vídeo): `doubao-seedream-5-0-pro-260628`, `doubao-seedream-5-0-260128`, `gpt-image-2-count`.

```bash
curl https://api.weytoken.com/v1/chat/completions \
  -H "Authorization: Bearer VOMujg…" -H "Content-Type: application/json" \
  -d '{"model":"deepseek-v4.1-flash","messages":[{"role":"user","content":"hi"}],"max_tokens":8}'
```

## 🧪 Cómo usar la API

URL base **`https://api.weytoken.com/v1`** — compatible con OpenAI.

```python
from openai import OpenAI
c = OpenAI(base_url="https://api.weytoken.com/v1", api_key="VOMujg…")
print(c.chat.completions.create(model="deepseek-v4.1-flash",
      messages=[{"role":"user","content":"hi"}]).choices[0].message.content)
```

## 🛠 Inicio rápido

```bash
cd weytoken-bulk-apikey
printf 'socks5://127.0.0.1:1080\n' > proxies.txt
node src/launcher.js

# or: node src/cli.js --count 30
```

`keys.txt` se escribe solo tras una finalización de chat en vivo; `accounts.json` conserva el nombre de usuario / correo electrónico / id de clave.

### 🖱️ El lanzador

El lanzador solicita la lista de proxies y la cantidad, las recuerda y ejecuta el CLI — acuñar, verificar, probar salidas, ajustes, ayuda, Conectar a 9Router.

## ✅ Verificado y funcionando — cada clave se prueba

| Comprobación | Resultado |
| --- | --- |
| `GET /api/status` | **200** — registro activado, verificación por correo activada, **captcha desactivado** |
| `GET /api/verification?email=…` | **200** `{"message":"","success":true}` |
| registro + inicio de sesión | **200** — sesión en mano |
| `POST /api/token/` (+ revelar) | **200** — clave escrita una vez |
| `POST /v1/chat/completions` (`deepseek-v4.1-flash`) — para **cada** clave antes de escribirse | **200** con contenido real; cuota 1.499.887 tras la primera llamada |
| una clave que falla su comprobación de chat | nunca se escribe — la cuenta permanece en `accounts.json` |

```bash
node src/cli.js --verify-only
node src/cli.js --screen
```

## 🧭 Cómo funciona

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: WeyToken page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://api.weytoken.com/v1/chat/completions → real content = the key is live
```

| Paso | Llamada | Notas |
| --- | --- | --- |
| estado | `GET /api/status` | puertas: registro activado, verificación por correo activada, captcha **desactivado** |
| enviar código | `GET /api/verification?email=…` | código de 6 caracteres desde el buzón |
| registro | `POST /api/user/register` | correo electrónico + código + contraseña |
| inicio de sesión | `POST /api/user/login` | token de sesión |
| clave | `POST /api/token/` (+ revelar) | escrita en disco una vez |
| prueba | `POST /v1/chat/completions` | chat-200 con contenido = la clave está viva |

## ⚙️ Opciones del CLI

```bash
node src/cli.js --count N        # mint N keys
node src/cli.js --verify-only    # re-verify every key in keys.txt
node src/cli.js --screen         # exit pool self-test
node src/cli.js --connect-9router
node src/cli.js --help
```

| Variable de entorno | Predeterminado | Significado |
| --- | --- | --- |
| `WEYTOKEN_PROXIES` | `proxies.txt` | grupo de salidas, una línea por proxy |
| `WEYTOKEN_MODEL` | `deepseek-v4.1-flash` | modelo para la verificación en vivo |

## 🛰️ Detrás de un proxy

**Opcional — recomendado a escala (límites por IP, bloqueos).** La herramienta también funciona sin proxy.

- **Con un grupo:** pasa `--pool <file>` (o define `WEYTOKEN_PROXIES`) y cada solicitud pasa por una salida de esa lista — una línea muerta hace fallar ese intento y la herramienta pasa a la siguiente (fail-closed mientras se proporciona un grupo; el grupo se vuelve a probar al usarse).
- **Sin un grupo:** el CLI imprime `running WITHOUT a proxy (direct)` y se conecta directamente. Nada del registro, check-in, acuñación de claves o verificación cambia — solo desde dónde salen los paquetes.
- Ejecuta `--screen` en cualquiera de los dos modos: prueba las salidas que tengas (o la ruta directa) contra el proveedor antes de una ejecución real.

## 📄 Archivos

| Ruta | Contenido |
| --- | --- |
| `keys.txt` | una clave verificada en vivo por línea — ignorada por git |
| `accounts.json` | nombre de usuario / correo electrónico / id de clave |
| `proxies.txt` | tus salidas — ignorado por git |
| `outputs/` | registros de ejecución — ignorados por git |

## 🔌 Conectar a 9Router

[9Router](https://9router.com) es un router autoalojado compatible con OpenAI:

```bash
node src/connect-9router.js --base http://localhost:20128 --password 123456
```

Predeterminados: `--prefix weytoken`, `--base-url https://api.weytoken.com/v1`, `--model deepseek-v4.1-flash`. Los modelos se sirven como `weytoken/<model id>`.

## 🔌 Conectar a OmniRoute

[OmniRoute](https://omniroute.online) es un router autoalojado compatible con OpenAI:

```bash
node src/connect-omniroute.js --base http://localhost:20128 --password 123456
```

Predeterminados: `--prefix weytoken`, `--base-url https://api.weytoken.com/v1`, `--model deepseek-v4.1-flash`. Los modelos se sirven como `weytoken/<model id>`.

## 🔑 Qué te da una clave

| | |
| --- | --- |
| 🏢 Proveedor | **WeyToken (微元算力)** — [api.weytoken.com](https://api.weytoken.com) |
| 🎁 Qué da el proveedor | ~1,5M de cuota inicial, registro sin captcha, catálogo de 63 filas |
| 🔑 Qué te da una clave | una clave en su propia cuenta: los ids del catálogo en `https://api.weytoken.com/v1` |
| 🌐 Forma de la API | Compatible con OpenAI: `POST https://api.weytoken.com/v1/chat/completions` |
| 🔒 Tus datos | solo se contacta con `api.weytoken.com` y tu backend de buzón |

## 🐞 Problemas

Si WeyToken cambia la forma del registro o su paso de cuota, la herramienta se detiene ahí y lo indica. **[Abre un issue](../../issues)** con el comando y la salida exacta.

## 📄 Licencia

MIT © 2026 — consulta [LICENSE](LICENSE).

<div align="center">

**[MIT](LICENSE)** © 2026 weytoken-bulk-apikey contributors

</div>