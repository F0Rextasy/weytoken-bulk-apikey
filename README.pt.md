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

**Crie contas WeyToken em massa com chaves de API geradas automaticamente — inferência compatível com OpenAI + conector 9Router.**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-63%20served-412991?style=for-the-badge)](https://api.weytoken.com)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![OmniRoute](https://img.shields.io/badge/OmniRoute-connector-8B5CF6?style=for-the-badge)](#-connect-to-omniroute)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 Início rápido](#-quick-start) · [🎁 Nível gratuito](#-free-tier--how-much-you-get) · [🤖 Modelos](#-models--all-63-of-them) · [🧪 Use a API](#-how-to-use-the-api) · [✅ Verificado](#-verified-working--every-key-is-tested) · [🧭 Como funciona](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [⚙️ Opções](#️-cli-options) · [🛰️ Proxies](#️-behind-a-proxy) · [🐞 Problemas](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ Por que isto existe

WeyToken (微元算力) é um painel New API cujo formulário de registro é **livre de captcha** (`turnstile_check: false`) — e-mail + código de 6 caracteres é todo o portão, e a conta nova chega com aproximadamente **1,5M de unidades de cota** (saldo medido após a primeira chamada: 1.499.887; `quota_per_unit` 500.000). A tabela de preços contém **63 linhas**, incluindo modelos de mídia com proporção zero (`doubao-seedream-*`, `gpt-image-2-count`), o GitHub OAuth é oferecido como porta alternativa, e o padrão verificado por chat é `deepseek-v4.1-flash`.

## 🎁 Nível gratuito — quanto você recebe

| O quê | Valor |
| --- | --- |
| Cota inicial | **~1.500.000 unidades** medidas (`quotaEnd` logo após o primeiro chat; `quota_per_unit` 500.000) |
| Registro | e-mail + código de 6 caracteres — **sem captcha** (`turnstile_check: false`) |
| Porta alternativa | GitHub OAuth (`github_oauth: true`) |
| Catálogo | **63 linhas de preços**; proporção zero: `doubao-seedream-5-0-pro-260628`, `doubao-seedream-5-0-260128`, `gpt-image-2-count` |
| Linha v4.1 | `deepseek-v4.1-flash` proporção 0.85, grupo `default` |
| Trilho de e-mail | Endereços de entrada Mailtrap funcionaram na via; caixas de corretor também passam |

> 💡 O nível gratuito é definido pela WeyToken e pode mudar. Confira os números ao vivo em [api.weytoken.com](https://api.weytoken.com) — e deixe o próprio portão da ferramenta decidir:

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 Modelos — todos os 63

A tabela de preços contém **63 linhas**. O padrão verificado por chat:

| ID do modelo | Nota |
| --- | --- |
| `deepseek-v4.1-flash` | **200** com conteúdo real em `default`; delta de cota ~113 por chamada pequena |

Linhas de mídia com proporção zero (gratuitas na proporção 0, imagem/vídeo): `doubao-seedream-5-0-pro-260628`, `doubao-seedream-5-0-260128`, `gpt-image-2-count`.

```bash
curl https://api.weytoken.com/v1/chat/completions \
  -H "Authorization: Bearer VOMujg…" -H "Content-Type: application/json" \
  -d '{"model":"deepseek-v4.1-flash","messages":[{"role":"user","content":"hi"}],"max_tokens":8}'
```

## 🧪 Como usar a API

URL base **`https://api.weytoken.com/v1`** — compatível com OpenAI.

```python
from openai import OpenAI
c = OpenAI(base_url="https://api.weytoken.com/v1", api_key="VOMujg…")
print(c.chat.completions.create(model="deepseek-v4.1-flash",
      messages=[{"role":"user","content":"hi"}]).choices[0].message.content)
```

## 🛠 Início rápido

```bash
cd weytoken-bulk-apikey
printf 'socks5://127.0.0.1:1080\n' > proxies.txt
node src/launcher.js

# or: node src/cli.js --count 30
```

`keys.txt` só é escrito após uma conclusão de chat ao vivo; `accounts.json` mantém o nome de usuário / e-mail / id da chave.

### 🖱️ O launcher

O launcher pede a lista de proxies e a quantidade, lembra delas e executa a CLI — criar, verificar, testar saídas, configurações, ajuda, Conectar ao 9Router.

## ✅ Verificado funcionando — toda chave é testada

| Verificação | Resultado |
| --- | --- |
| `GET /api/status` | **200** — registro ativo, verificação por e-mail ativa, **captcha desativado** |
| `GET /api/verification?email=…` | **200** `{"message":"","success":true}` |
| registro + login | **200** — sessão em mãos |
| `POST /api/token/` (+ revelar) | **200** — chave escrita uma vez |
| `POST /v1/chat/completions` (`deepseek-v4.1-flash`) — para **toda** chave antes de ser escrita | **200** com conteúdo real; cota 1.499.887 após a primeira chamada |
| uma chave que falha na verificação de chat | nunca escrita — a conta permanece em `accounts.json` |

```bash
node src/cli.js --verify-only
node src/cli.js --screen
```

## 🧭 Como funciona

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: WeyToken page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://api.weytoken.com/v1/chat/completions → real content = the key is live
```

| Etapa | Chamada | Notas |
| --- | --- | --- |
| status | `GET /api/status` | portões: registro ativo, verificação por e-mail ativa, captcha **desativado** |
| enviar código | `GET /api/verification?email=…` | código de 6 caracteres da caixa de entrada |
| registrar | `POST /api/user/register` | e-mail + código + senha |
| login | `POST /api/user/login` | token de sessão |
| chave | `POST /api/token/` (+ revelar) | escrita em disco uma vez |
| prova | `POST /v1/chat/completions` | chat-200 com conteúdo = a chave está ativa |

## ⚙️ Opções da CLI

```bash
node src/cli.js --count N        # mint N keys
node src/cli.js --verify-only    # re-verify every key in keys.txt
node src/cli.js --screen         # exit pool self-test
node src/cli.js --connect-9router
node src/cli.js --help
```

| Env | Padrão | Significado |
| --- | --- | --- |
| `WEYTOKEN_PROXIES` | `proxies.txt` | pool de saída, uma linha por proxy |
| `WEYTOKEN_MODEL` | `deepseek-v4.1-flash` | modelo para a verificação ao vivo |

## 🛰️ Atrás de um proxy

**Opcional — recomendado em escala (limites por IP, banimentos).** A ferramenta funciona sem proxy também.

- **Com um pool:** passe `--pool <file>` (ou defina `WEYTOKEN_PROXIES`) e toda requisição passa por uma saída dessa lista — uma linha morta falha naquela tentativa e a ferramenta passa para a próxima (fail-closed enquanto um pool é fornecido; o pool é re-testado no uso).
- **Sem um pool:** a CLI imprime `running WITHOUT a proxy (direct)` e conecta diretamente. Nada sobre registro, check-in, criação de chaves ou verificação muda — apenas de onde os pacotes saem.
- Execute `--screen` em qualquer um dos modos: ele testa as saídas que você tem (ou a rota direta) contra o provedor antes de uma execução real.

## 📄 Arquivos

| Caminho | Conteúdo |
| --- | --- |
| `keys.txt` | uma chave verificada ao vivo por linha — ignorada pelo git |
| `accounts.json` | nome de usuário / e-mail / id da chave |
| `proxies.txt` | suas saídas — ignorado pelo git |
| `outputs/` | logs de execução — ignorados pelo git |

## 🔌 Conectar ao 9Router

[9Router](https://9router.com) é um roteador auto-hospedado e compatível com OpenAI:

```bash
node src/connect-9router.js --base http://localhost:20128 --password 123456
```

Padrões: `--prefix weytoken`, `--base-url https://api.weytoken.com/v1`, `--model deepseek-v4.1-flash`. Os modelos são servidos como `weytoken/<model id>`.

## 🔌 Conectar ao OmniRoute

[OmniRoute](https://omniroute.online) é um roteador auto-hospedado e compatível com OpenAI:

```bash
node src/connect-omniroute.js --base http://localhost:20128 --password 123456
```

Padrões: `--prefix weytoken`, `--base-url https://api.weytoken.com/v1`, `--model deepseek-v4.1-flash`. Os modelos são servidos como `weytoken/<model id>`.
## 🔑 O que uma chave te dá

| | |
| --- | --- |
| 🏢 Provedor | **WeyToken (微元算力)** — [api.weytoken.com](https://api.weytoken.com) |
| 🎁 O que o provedor dá | ~1,5M de cota inicial, registro sem captcha, catálogo de 63 linhas |
| 🔑 O que uma chave te dá | uma chave em sua própria conta: os ids do catálogo em `https://api.weytoken.com/v1` |
| 🌐 Formato da API | Compatível com OpenAI: `POST https://api.weytoken.com/v1/chat/completions` |
| 🔒 Seus dados | apenas `api.weytoken.com` e seu backend de caixa de entrada são contatados |

## 🐞 Problemas

Se a WeyToken mudar o formato de registro ou seu passo de cota, a ferramenta para ali e avisa. **[Abra uma issue](../../issues)** com o comando e a saída exata.

## 📄 Licença

MIT © 2026 — veja [LICENSE](LICENSE).

<div align="center">

**[MIT](LICENSE)** © 2026 weytoken-bulk-apikey contributors

</div>