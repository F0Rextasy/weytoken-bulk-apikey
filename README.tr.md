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

**Otomatik oluşturulan API anahtarlarıyla toplu WeyToken hesabı açın — OpenAI uyumlu çıkarım + 9Router bağlayıcısı.**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-63%20served-412991?style=for-the-badge)](https://api.weytoken.com)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 Hızlı başlangıç](#-quick-start) · [🎁 Ücretsiz katman](#-free-tier--how-much-you-get) · [🤖 Modeller](#-models--all-63-of-them) · [🧪 API'yi kullanma](#-how-to-use-the-api) · [✅ Doğrulandı](#-verified-working--every-key-is-tested) · [🧭 Nasıl çalışır](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [⚙️ Seçenekler](#️-cli-options) · [🛰️ Proxy'ler](#️-behind-a-proxy) · [🐞 Sorunlar](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ Bu neden var

WeyToken (微元算力), kayıt formu **captcha'sız** olan bir New API panelidir (`turnstile_check: false`) — e-posta + 6 karakterlik kod tüm kapıdır ve yeni hesap yaklaşık **1,5M kota birimi** ile açılır (ilk çağrıdan sonra ölçülen bakiye: 1.499.887; `quota_per_unit` 500.000). Fiyat listesi, sıfır oranlı medya modelleri (`doubao-seedream-*`, `gpt-image-2-count`) dahil **63 satır** içerir, GitHub OAuth alternatif bir kapı olarak sunulur ve sohbetle doğrulanmış varsayılan `deepseek-v4.1-flash`'tır.

## 🎁 Ücretsiz katman — ne kadar alırsınız

| Ne | Değer |
| --- | --- |
| Başlangıç kotası | **~1.500.000 birim** ölçüldü (ilk sohbetten hemen sonra `quotaEnd`; `quota_per_unit` 500.000) |
| Kayıt | e-posta + 6 karakterlik kod — **captcha yok** (`turnstile_check: false`) |
| Alternatif kapı | GitHub OAuth (`github_oauth: true`) |
| Katalog | **63 fiyat satırı**; sıfır oranlı: `doubao-seedream-5-0-pro-260628`, `doubao-seedream-5-0-260128`, `gpt-image-2-count` |
| v4.1 hattı | `deepseek-v4.1-flash` oran 0.85, grup `default` |
| Posta hattı | Mailtrap gelen adresleri bu hatta çalıştı; broker kutuları da geçer |

> 💡 Ücretsiz katman WeyToken tarafından belirlenir ve değişebilir. Güncel sayıları [api.weytoken.com](https://api.weytoken.com) adresinden kontrol edin — ve kararı aracın kendi kapısına bırakın:

```bash
node src/cli.js --verify-only     # keys.txt içindeki her anahtar gerçek bir sohbet çağrısıyla yeniden test edilir
```

## 🤖 Modeller — 63'ünün tamamı

Fiyat listesi **63 satır** içerir. Sohbetle doğrulanmış varsayılan:

| Model Kimliği | Not |
| --- | --- |
| `deepseek-v4.1-flash` | `default` üzerinde gerçek içerikle **200**; küçük bir çağrı başına kota farkı ~113 |

Sıfır oranlı medya satırları (oran 0'da ücretsiz, görsel/video): `doubao-seedream-5-0-pro-260628`, `doubao-seedream-5-0-260128`, `gpt-image-2-count`.

```bash
curl https://api.weytoken.com/v1/chat/completions \
  -H "Authorization: Bearer VOMujg…" -H "Content-Type: application/json" \
  -d '{"model":"deepseek-v4.1-flash","messages":[{"role":"user","content":"hi"}],"max_tokens":8}'
```

## 🧪 API'yi nasıl kullanırım

Temel URL **`https://api.weytoken.com/v1`** — OpenAI uyumlu.

```python
from openai import OpenAI
c = OpenAI(base_url="https://api.weytoken.com/v1", api_key="VOMujg…")
print(c.chat.completions.create(model="deepseek-v4.1-flash",
      messages=[{"role":"user","content":"hi"}]).choices[0].message.content)
```

## 🛠 Hızlı başlangıç

```bash
cd weytoken-bulk-apikey
printf 'socks5://127.0.0.1:1080\n' > proxies.txt
node src/launcher.js
# or: node src/cli.js --count 30
```

`keys.txt` yalnızca canlı bir sohbet tamamlamasından sonra yazılır; `accounts.json` kullanıcı adını / e-postayı / anahtar kimliğini saklar.

### 🖱️ Başlatıcı

Başlatıcı proxy listesini ve sayıyı sorar, bunları hatırlar ve CLI'yi çalıştırır — üret, doğrula, çıkışları tara, ayarlar, yardım, 9Router'a Bağlan.

## ✅ Çalıştığı doğrulandı — her anahtar test edilir

| Kontrol | Sonuç |
| --- | --- |
| `GET /api/status` | **200** — kayıt açık, e-posta doğrulama açık, **captcha kapalı** |
| `GET /api/verification?email=…` | **200** `{"message":"","success":true}` |
| kayıt + giriş | **200** — oturum elde edildi |
| `POST /api/token/` (+ göster) | **200** — anahtar bir kez yazıldı |
| `POST /v1/chat/completions` (`deepseek-v4.1-flash`) — yazılmadan önce **her** anahtar için | gerçek içerikle **200**; ilk çağrıdan sonra kota 1.499.887 |
| sohbet kontrolünde başarısız olan anahtar | asla yazılmaz — hesap `accounts.json` içinde kalır |

```bash
node src/cli.js --verify-only
node src/cli.js --screen
```

## 🧭 Nasıl çalışır

```
gelen kutusu (hesap başına bir adres)                            hesap başına yeni bir adres
   │
   ├─▶ kayıt: WeyToken sayfası → e-posta adresi → gelen kutusundan kod / bağlantı
   ├─▶ oturum: hesabın çerezleri veya erişim belirteci elde edildi
   ├─▶ anahtar üretimi: hesabın anahtar uç noktası → düz metin anahtar bir kez döndürülür (hemen diske yazılır)
   └─▶ canlı kanıt: POST https://api.weytoken.com/v1/chat/completions → gerçek içerik = anahtar canlı
```

| Adım | Çağrı | Notlar |
| --- | --- | --- |
| durum | `GET /api/status` | kapılar: kayıt açık, e-posta doğrulama açık, captcha **kapalı** |
| kod gönder | `GET /api/verification?email=…` | gelen kutusundan 6 karakterlik kod |
| kayıt | `POST /api/user/register` | e-posta + kod + parola |
| giriş | `POST /api/user/login` | oturum belirteci |
| anahtar | `POST /api/token/` (+ göster) | diske bir kez yazılır |
| kanıt | `POST /v1/chat/completions` | içerikli sohbet-200 = anahtar canlı |

## ⚙️ CLI seçenekleri

```bash
node src/cli.js --count N        # N anahtar üret
node src/cli.js --verify-only    # keys.txt içindeki her anahtarı yeniden doğrula
node src/cli.js --screen         # çıkış havuzu öz testi
node src/cli.js --connect-9router
node src/cli.js --help
```

| Ortam değişkeni | Varsayılan | Anlamı |
| --- | --- | --- |
| `WEYTOKEN_PROXIES` | `proxies.txt` | çıkış havuzu, proxy başına bir satır |
| `WEYTOKEN_MODEL` | `deepseek-v4.1-flash` | canlı doğrulama için model |

## 🛰️ Proxy arkasında

**İsteğe bağlı — ölçekte önerilir (IP başına limitler, yasaklar).** Araç proxy olmadan da çalışır.

- **Havuzla:** `--pool <file>` geçin (veya `WEYTOKEN_PROXIES` ayarlayın) ve her istek o listeden bir çıkıştan geçer — ölü bir hat o denemeyi başarısız kılar ve araç bir sonrakine geçer (havuz sağlandığında fail-closed; havuz kullanımda yeniden yoklanır).
- **Havuz olmadan:** CLI `running WITHOUT a proxy (direct)` yazdırır ve doğrudan bağlanır. Kayıt, giriş kontrolü, anahtar üretimi veya doğrulamayla ilgili hiçbir şey değişmez — yalnızca paketlerin nereden çıktığı değişir.
- Her iki moda karşı `--screen` çalıştırın: gerçek bir çalıştırmadan önce sahip olduğunuz çıkışları (veya doğrudan rotayı) sağlayıcıya karşı test eder.

## 📄 Dosyalar

| Yol | İçerik |
| --- | --- |
| `keys.txt` | satır başına bir canlı doğrulanmış anahtar — git tarafından yok sayılır |
| `accounts.json` | kullanıcı adı / e-posta / anahtar kimliği |
| `proxies.txt` | çıkışlarınız — git tarafından yok sayılır |
| `outputs/` | çalıştırma günlükleri — git tarafından yok sayılır |

## 🔌 9Router'a bağlan

[9Router](https://9router.com) kendi sunucunuzda barındırılan, OpenAI uyumlu bir yönlendiricidir:

```bash
node src/connect-9router.js --base http://localhost:20128 --password 123456
```

Varsayılanlar: `--prefix weytoken`, `--base-url https://api.weytoken.com/v1`, `--model deepseek-v4.1-flash`. Modeller `weytoken/<model id>` olarak sunulur.

## 🔑 Bir anahtar size ne kazandırır

| | |
| --- | --- |
| 🏢 Sağlayıcı | **WeyToken (微元算力)** — [api.weytoken.com](https://api.weytoken.com) |
| 🎁 Sağlayıcının verdikleri | ~1,5M başlangıç kotası, captcha'sız kayıt, 63 satırlık katalog |
| 🔑 Bir anahtarın size kazandırdıkları | kendi hesabında bir anahtar: `https://api.weytoken.com/v1` üzerindeki katalog kimlikleri |
| 🌐 API şekli | OpenAI uyumlu: `POST https://api.weytoken.com/v1/chat/completions` |
| 🔒 Verileriniz | yalnızca `api.weytoken.com` ve gelen kutusu arka ucunuzla iletişim kurulur |

## 🐞 Sorunlar

WeyToken kayıt şeklini veya kota adımını değiştirirse, araç orada durur ve bunu bildirir. Komut ve tam çıktıyla birlikte **[bir sorun açın](../../issues)**.

## 📄 Lisans

MIT © 2026 — bkz. [LICENSE](LICENSE).

<div align="center">

**[MIT](LICENSE)** © 2026 weytoken-bulk-apikey katkıda bulunanlar

</div>