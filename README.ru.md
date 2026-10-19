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

**Массовое создание аккаунтов WeyToken с автоматически сгенерированными API-ключами — вывод, совместимый с OpenAI, + коннектор 9Router.**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-63%20served-412991?style=for-the-badge)](https://api.weytoken.com)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 Быстрый старт](#-quick-start) · [🎁 Бесплатный тариф](#-free-tier--how-much-you-get) · [🤖 Модели](#-models--all-63-of-them) · [🧪 Использование API](#-how-to-use-the-api) · [✅ Проверено](#-verified-working--every-key-is-tested) · [🧭 Как это работает](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [⚙️ Опции](#️-cli-options) · [🛰️ Прокси](#️-behind-a-proxy) · [🐞 Проблемы](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ Зачем это нужно

WeyToken (微元算力) — это панель New API, форма регистрации которой **не требует капчи** (`turnstile_check: false`) — e-mail + 6-символьный код это весь барьер, и свежий аккаунт получает примерно **1,5 млн единиц квоты** (измеренный баланс после первого вызова: 1 499 887; `quota_per_unit` 500 000). Прайс-лист содержит **63 строки**, включая медиамодели с нулевым коэффициентом (`doubao-seedream-*`, `gpt-image-2-count`), GitHub OAuth предлагается как альтернативный вход, а проверенная в чате модель по умолчанию — `deepseek-v4.1-flash`.

## 🎁 Бесплатный тариф — сколько вы получаете

| Что | Значение |
| --- | --- |
| Стартовая квота | **~1 500 000 единиц** измерено (`quotaEnd` сразу после первого чата; `quota_per_unit` 500 000) |
| Регистрация | e-mail + 6-символьный код — **без капчи** (`turnstile_check: false`) |
| Альтернативный вход | GitHub OAuth (`github_oauth: true`) |
| Каталог | **63 строки прайса**; нулевой коэффициент: `doubao-seedream-5-0-pro-260628`, `doubao-seedream-5-0-260128`, `gpt-image-2-count` |
| Линейка v4.1 | `deepseek-v4.1-flash` коэффициент 0.85, группа `default` |
| Почтовый канал | Входящие адреса Mailtrap работали в этом канале; брокерские ящики также проходят |

> 💡 Бесплатный тариф устанавливается WeyToken и может измениться. Проверяйте актуальные цифры на [api.weytoken.com](https://api.weytoken.com) — и пусть собственный контроль инструмента решает:

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 Модели — все 63

Прайс-лист содержит **63 строки**. Проверенная в чате модель по умолчанию:

| ID модели | Примечание |
| --- | --- |
| `deepseek-v4.1-flash` | **200** с реальным содержимым на `default`; изменение квоты ~113 за крошечный вызов |

Медиастроки с нулевым коэффициентом (бесплатно при коэффициенте 0, изображение/видео): `doubao-seedream-5-0-pro-260628`, `doubao-seedream-5-0-260128`, `gpt-image-2-count`.

```bash
curl https://api.weytoken.com/v1/chat/completions \
  -H "Authorization: Bearer VOMujg…" -H "Content-Type: application/json" \
  -d '{"model":"deepseek-v4.1-flash","messages":[{"role":"user","content":"hi"}],"max_tokens":8}'
```

## 🧪 Как использовать API

Базовый URL **`https://api.weytoken.com/v1`** — совместим с OpenAI.

```python
from openai import OpenAI
c = OpenAI(base_url="https://api.weytoken.com/v1", api_key="VOMujg…")
print(c.chat.completions.create(model="deepseek-v4.1-flash",
      messages=[{"role":"user","content":"hi"}]).choices[0].message.content)
```

## 🛠 Быстрый старт

```bash
cd weytoken-bulk-apikey
printf 'socks5://127.0.0.1:1080\n' > proxies.txt
node src/launcher.js
# or: node src/cli.js --count 30
```

`keys.txt` записывается только после живого завершения чата; `accounts.json` хранит имя пользователя / e-mail / id ключа.

### 🖱️ Лаунчер

Лаунчер запрашивает список прокси и количество, запоминает их и запускает CLI — создание, проверка, проверка выходов, настройки, справка, подключение к 9Router.

## ✅ Проверено в работе — каждый ключ тестируется

| Проверка | Результат |
| --- | --- |
| `GET /api/status` | **200** — регистрация включена, подтверждение e-mail включено, **капча выключена** |
| `GET /api/verification?email=…` | **200** `{"message":"","success":true}` |
| регистрация + вход | **200** — сессия получена |
| `POST /api/token/` (+ раскрытие) | **200** — ключ записан один раз |
| `POST /v1/chat/completions` (`deepseek-v4.1-flash`) — для **каждого** ключа перед его записью | **200** с реальным содержимым; квота 1 499 887 после первого вызова |
| ключ, не прошедший проверку чата | никогда не записывается — аккаунт остаётся в `accounts.json` |

```bash
node src/cli.js --verify-only
node src/cli.js --screen
```

## 🧭 Как это работает

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: WeyToken page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://api.weytoken.com/v1/chat/completions → real content = the key is live
```

| Шаг | Вызов | Примечания |
| --- | --- | --- |
| статус | `GET /api/status` | условия: регистрация включена, подтверждение e-mail включено, капча **выключена** |
| отправка кода | `GET /api/verification?email=…` | 6-символьный код из входящих |
| регистрация | `POST /api/user/register` | e-mail + код + пароль |
| вход | `POST /api/user/login` | токен сессии |
| ключ | `POST /api/token/` (+ раскрытие) | записывается на диск один раз |
| доказательство | `POST /v1/chat/completions` | чат-200 с содержимым = ключ работает |

## ⚙️ Опции CLI

```bash
node src/cli.js --count N        # mint N keys
node src/cli.js --verify-only    # re-verify every key in keys.txt
node src/cli.js --screen         # exit pool self-test
node src/cli.js --connect-9router
node src/cli.js --help
```

| Переменная | По умолчанию | Значение |
| --- | --- | --- |
| `WEYTOKEN_PROXIES` | `proxies.txt` | пул выходов, одна строка на прокси |
| `WEYTOKEN_MODEL` | `deepseek-v4.1-flash` | модель для живой проверки |

## 🛰️ За прокси

**Обязательно — без запасного варианта прямого подключения.** Каждый запрос уходит через выход; в этом канале измерен таймаут `/api/status` на уставших бесплатных выходах (пул из 15 строк, `TIMEOUT status after 90s`), поэтому `--screen` перед большим запуском.

## 📄 Файлы

| Путь | Содержимое |
| --- | --- |
| `keys.txt` | один проверенный в работе ключ на строку — игнорируется git |
| `accounts.json` | имя пользователя / e-mail / id ключа |
| `proxies.txt` | ваши выходы — игнорируется git |
| `outputs/` | логи запусков — игнорируется git |

## 🔌 Подключение к 9Router

[9Router](https://9router.com) — это самостоятельно размещаемый роутер, совместимый с OpenAI:

```bash
node src/connect-9router.js --base http://localhost:20128 --password 123456
```

По умолчанию: `--prefix weytoken`, `--base-url https://api.weytoken.com/v1`, `--model deepseek-v4.1-flash`. Модели подаются как `weytoken/<model id>`.

## 🔑 Что даёт один ключ

| | |
| --- | --- |
| 🏢 Провайдер | **WeyToken (微元算力)** — [api.weytoken.com](https://api.weytoken.com) |
| 🎁 Что даёт провайдер | ~1,5 млн стартовой квоты, регистрация без капчи, каталог из 63 строк |
| 🔑 Что даёт один ключ | один ключ на собственном аккаунте: id каталога на `https://api.weytoken.com/v1` |
| 🌐 Форма API | Совместим с OpenAI: `POST https://api.weytoken.com/v1/chat/completions` |
| 🔒 Ваши данные | связь только с `api.weytoken.com` и вашим почтовым бэкендом |

## 🐞 Проблемы

Если WeyToken изменит форму регистрации или шаг квоты, инструмент остановится на этом месте и сообщит об этом. **[Откройте issue](../../issues)** с командой и точным выводом.

## 📄 Лицензия

MIT © 2026 — см. [LICENSE](LICENSE).

<div align="center">

**[MIT](LICENSE)** © 2026 weytoken-bulk-apikey contributors

</div>