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

**자동 생성된 API 키로 WeyToken 계정을 대량 생성 — OpenAI 호환 추론 + 9Router 커넥터.**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-63%20served-412991?style=for-the-badge)](https://api.weytoken.com)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 빠른 시작](#-quick-start) · [🎁 무료 티어](#-free-tier--how-much-you-get) · [🤖 모델](#-models--all-63-of-them) · [🧪 API 사용하기](#-how-to-use-the-api) · [✅ 검증됨](#-verified-working--every-key-is-tested) · [🧭 작동 방식](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [⚙️ 옵션](#️-cli-options) · [🛰️ 프록시](#️-behind-a-proxy) · [🐞 이슈](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ 이 도구가 존재하는 이유

WeyToken (微元算力)은 가입 양식에 **캡차가 없는** (`turnstile_check: false`) New API 패널입니다 — 이메일 + 6자리 코드만 있으면 통과되며, 새 계정에는 약 **150만 할당량 단위**가 들어옵니다 (첫 호출 후 측정된 잔액: 1,499,887; `quota_per_unit` 500,000). 가격표에는 비율이 0인 미디어 모델(`doubao-seedream-*`, `gpt-image-2-count`)을 포함해 **63개 행**이 있으며, GitHub OAuth가 대안 경로로 제공되고, 채팅으로 검증된 기본 모델은 `deepseek-v4.1-flash`입니다.

## 🎁 무료 티어 — 얼마나 받을 수 있나

| 항목 | 값 |
| --- | --- |
| 시작 할당량 | **약 1,500,000 단위** 측정됨 (첫 채팅 직후 `quotaEnd`; `quota_per_unit` 500,000) |
| 가입 | 이메일 + 6자리 코드 — **캡차 없음** (`turnstile_check: false`) |
| 대안 경로 | GitHub OAuth (`github_oauth: true`) |
| 카탈로그 | **63개 가격 행**; 비율 0: `doubao-seedream-5-0-pro-260628`, `doubao-seedream-5-0-260128`, `gpt-image-2-count` |
| v4.1 라인 | `deepseek-v4.1-flash` 비율 0.85, 그룹 `default` |
| 메일 경로 | Mailtrap 인바운드 주소가 이 경로에서 작동함; 브로커 박스도 통과 |

> 💡 무료 티어는 WeyToken이 설정하며 변경될 수 있습니다. 실시간 수치는 [api.weytoken.com](https://api.weytoken.com)에서 확인하세요 — 그리고 도구 자체의 게이트가 판단하게 하세요:

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 모델 — 63개 전부

가격표에는 **63개 행**이 있습니다. 채팅으로 검증된 기본 모델:

| 모델 ID | 비고 |
| --- | --- |
| `deepseek-v4.1-flash` | `default`에서 실제 콘텐츠와 함께 **200**; 작은 호출당 할당량 변화량 약 113 |

비율 0 미디어 행 (비율 0에서 무료, 이미지/비디오): `doubao-seedream-5-0-pro-260628`, `doubao-seedream-5-0-260128`, `gpt-image-2-count`.

```bash
curl https://api.weytoken.com/v1/chat/completions \
  -H "Authorization: Bearer VOMujg…" -H "Content-Type: application/json" \
  -d '{"model":"deepseek-v4.1-flash","messages":[{"role":"user","content":"hi"}],"max_tokens":8}'
```

## 🧪 API 사용 방법

Base URL **`https://api.weytoken.com/v1`** — OpenAI 호환.

```python
from openai import OpenAI
c = OpenAI(base_url="https://api.weytoken.com/v1", api_key="VOMujg…")
print(c.chat.completions.create(model="deepseek-v4.1-flash",
      messages=[{"role":"user","content":"hi"}]).choices[0].message.content)
```

## 🛠 빠른 시작

```bash
cd weytoken-bulk-apikey
printf 'socks5://127.0.0.1:1080\n' > proxies.txt
node src/launcher.js

# or: node src/cli.js --count 30
```

`keys.txt`는 실제 채팅 완료 후에만 기록됩니다; `accounts.json`은 사용자 이름 / 이메일 / 키 id를 보관합니다.

### 🖱️ 런처

런처는 프록시 목록과 개수를 묻고, 이를 기억한 뒤 CLI를 실행합니다 — 발급, 검증, 종료, 설정, 도움말, 9Router 연결.

## ✅ 검증됨 — 모든 키가 테스트됩니다

| 확인 | 결과 |
| --- | --- |
| `GET /api/status` | **200** — 가입 켜짐, 이메일 인증 켜짐, **캡차 꺼짐** |
| `GET /api/verification?email=…` | **200** `{"message":"","success":true}` |
| 가입 + 로그인 | **200** — 세션 확보 |
| `POST /api/token/` (+ 공개) | **200** — 키가 한 번 기록됨 |
| `POST /v1/chat/completions` (`deepseek-v4.1-flash`) — 기록되기 전 **모든** 키에 대해 | 실제 콘텐츠와 함께 **200**; 첫 호출 후 할당량 1,499,887 |
| 채팅 확인에 실패한 키 | 기록되지 않음 — 계정은 `accounts.json`에 남음 |

```bash
node src/cli.js --verify-only
node src/cli.js --screen
```

## 🧭 작동 방식

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: WeyToken page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://api.weytoken.com/v1/chat/completions → real content = the key is live
```

| 단계 | 호출 | 비고 |
| --- | --- | --- |
| 상태 | `GET /api/status` | 게이트: 가입 켜짐, 이메일 인증 켜짐, 캡차 **꺼짐** |
| 코드 전송 | `GET /api/verification?email=…` | 받은편지함에서 온 6자리 코드 |
| 가입 | `POST /api/user/register` | 이메일 + 코드 + 비밀번호 |
| 로그인 | `POST /api/user/login` | 세션 토큰 |
| 키 | `POST /api/token/` (+ 공개) | 한 번 디스크에 기록됨 |
| 증명 | `POST /v1/chat/completions` | 콘텐츠와 함께 채팅 200 = 키가 살아 있음 |

## ⚙️ CLI 옵션

```bash
node src/cli.js --count N        # mint N keys
node src/cli.js --verify-only    # re-verify every key in keys.txt
node src/cli.js --screen         # exit pool self-test
node src/cli.js --connect-9router
node src/cli.js --help
```

| 환경 변수 | 기본값 | 의미 |
| --- | --- | --- |
| `WEYTOKEN_PROXIES` | `proxies.txt` | 종료 풀, 프록시당 한 줄 |
| `WEYTOKEN_MODEL` | `deepseek-v4.1-flash` | 실시간 검증에 사용할 모델 |

## 🛰️ 프록시 뒤에서

**선택 사항 — 대규모에서는 권장됨 (IP당 제한, 차단).** 이 도구는 프록시 없이도 작동합니다.

- **풀 사용 시:** `--pool <file>`을 전달하거나 (또는 `WEYTOKEN_PROXIES` 설정) 모든 요청이 해당 목록의 종료 지점을 통해 나갑니다 — 죽은 회선은 해당 시도를 실패시키고 도구는 다음으로 넘어갑니다 (풀이 제공되면 fail-closed; 풀은 사용 시점에 다시 검사됨).
- **풀 없이:** CLI는 `running WITHOUT a proxy (direct)`를 출력하고 직접 연결합니다. 가입, 체크인, 키 발급 또는 검증에 대해 달라지는 것은 없습니다 — 패킷이 나가는 위치만 다릅니다.
- 어느 모드에서든 `--screen`을 실행하세요: 실제 실행 전에 보유한 종료 지점 (또는 직접 경로)을 제공자에 대해 테스트합니다.

## 📄 파일

| 경로 | 내용 |
| --- | --- |
| `keys.txt` | 실시간 검증된 키가 한 줄에 하나씩 — git 무시됨 |
| `accounts.json` | 사용자 이름 / 이메일 / 키 id |
| `proxies.txt` | 종료 지점 — git 무시됨 |
| `outputs/` | 실행 로그 — git 무시됨 |

## 🔌 9Router에 연결

[9Router](https://9router.com)는 자체 호스팅, OpenAI 호환 라우터입니다:

```bash
node src/connect-9router.js --base http://localhost:20128 --password 123456
```

기본값: `--prefix weytoken`, `--base-url https://api.weytoken.com/v1`, `--model deepseek-v4.1-flash`. 모델은 `weytoken/<model id>`로 제공됩니다.

## 🔑 키 하나로 얻는 것

| | |
| --- | --- |
| 🏢 제공자 | **WeyToken (微元算力)** — [api.weytoken.com](https://api.weytoken.com) |
| 🎁 제공자가 주는 것 | 약 150만 시작 할당량, 캡차 없는 가입, 63행 카탈로그 |
| 🔑 키 하나로 얻는 것 | 자체 계정의 키 하나: `https://api.weytoken.com/v1`의 카탈로그 id |
| 🌐 API 형태 | OpenAI 호환: `POST https://api.weytoken.com/v1/chat/completions` |
| 🔒 데이터 | `api.weytoken.com`과 받은편지함 백엔드만 접촉됨 |

## 🐞 이슈

WeyToken이 가입 형태나 할당량 단계를 변경하면, 도구는 그 지점에서 멈추고 그렇게 알립니다. 명령과 정확한 출력을 포함해 **[이슈를 열어주세요](../../issues)**.

## 📄 라이선스

MIT © 2026 — [LICENSE](LICENSE) 참조.

<div align="center">

**[MIT](LICENSE)** © 2026 weytoken-bulk-apikey contributors

</div>