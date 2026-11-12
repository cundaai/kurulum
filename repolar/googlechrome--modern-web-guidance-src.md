---
repo: GoogleChrome/modern-web-guidance-src
url: https://github.com/GoogleChrome/modern-web-guidance-src
lisans: Apache-2.0
surum: sürüm etiketi yok
readme_tarihi: 2026-11-12
dogrulama: denenmedi
bolum: trend-short/t2026-11-12-3
ozet: Chrome, Edge, Firefox ve MDN ekiplerinin coding agent'lara güncel web platformu pratiklerini öğreten skill setinin kaynak deposu.
---

# Modern Web Guidance — kurulum ve ilk örnek

> **Bu belge, README'nin 12 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

Coding agent'lar eğitim verisindeki eski kod yüzünden dialog, popover, view transition, container query gibi tarayıcının artık yerleşik sunduğu şeyler için ağır JavaScript üretir. Modern Web Guidance, 250 modern web özelliğini ve 203 gerçek geliştirici görevini kapsayan, uzmanların yazdığı kısa kılavuzları agent'ın context'ine çağıran bir skill'dir; ağ ve API anahtarı olmadan yerel arama yapar. Bu depo onun **kaynak** deposudur: kılavuzların yazıldığı, ölçüldüğü ve derlendiği yer. Kurulum, haftalık derlenip yayımlanan `modern-web-guidance` npm paketi ve [GoogleChrome/modern-web-guidance](https://github.com/GoogleChrome/modern-web-guidance) dağıtım deposu üzerinden yapılır; issue ve PR'lar buraya açılır. Web arayüzü yazan ve agent kullanan geliştiriciler için.

## Gereksinimler

- Skill'i kullanmak için Node.js ve `npx` (npm paketi `modern-web-guidance`; README kullanıcı tarafı için Node sürümü belirtmiyor) ve bir coding agent: Claude Code, OpenAI Codex, Gemini CLI/Antigravity, GitHub Copilot CLI, Kimi Code, Grok Build ya da `npx skills` destekleyen başka biri.
- API anahtarı gerekmez; arama çevrimdışı, yerel ve bağımlılıksız çalışır.
- Kaynak depoda çalışmak (kılavuz yazmak, eval koşmak) için Node.js 22.18.0 ve üzeri ile pnpm (`CONTRIBUTING.md` ve `package.json`); PR için imzalı Google CLA.

## Kurulum

### Etkileşimli sihirbaz (önerilen)

```shell
npx modern-web-guidance@latest install
```

Komut hangi agent'ların kurulu olduğunu bulur ve skill'i uygun yere yazar. Yalnız istediğin paketleri seçmek için `npx modern-web-guidance@latest install --choose`: `modern-web-guidance` (modern tarayıcı API'leri, layout, performans) ve `chrome-extensions` (Manifest V3, uzantı API'leri) paketleri var. Güncelleme: `npx modern-web-guidance@latest update`.

### Agent'a özel yollar

Claude Code:

```shell
/plugin marketplace add GoogleChrome/modern-web-guidance
/plugin install modern-web-guidance@googlechrome
/plugin  # Select GoogleChrome marketplace, press enter, enable AutoUpdate
/reload-plugins
```

OpenAI Codex:

```shell
codex plugin marketplace add GoogleChrome/modern-web-guidance
codex plugin add modern-web-guidance@googlechrome
```

skills CLI ve GitHub CLI:

```shell
npx skills add GoogleChrome/modern-web-guidance
```

```shell
gh skill install GoogleChrome/modern-web-guidance
```

Antigravity (`agy plugin install https://github.com/GoogleChrome/modern-web-guidance`), Copilot CLI, Kimi Code ve Grok Build komutları README'nin "Alternative Installation Methods" bölümünde.

### Kaynak depoyu kurmak (katkı için)

```bash
# Clone and install dependencies:
git clone https://github.com/GoogleChrome/modern-web-guidance-src.git
cd modern-web-guidance-src
pnpm install

# Link the unified CLI globally:
pnpm link --global && gd setup-completion

# Fast static check:
pnpm typecheck && pnpm lint

# Full preflight gate (builds distributions, typechecks, lints, and runs all unit tests):
pnpm preflight
```

Kılavuz yazımı için ayrı bir [guides/CONTRIBUTING.md](https://github.com/GoogleChrome/modern-web-guidance-src/blob/main/guides/CONTRIBUTING.md) var; sıfırdan yeni kılavuz yazmak onboard edilmiş "Peer" rolüne ayrılmış, var olanı iyileştiren PR'ı herkes açabilir.

## İlk çalıştırma

Kurmadan da deneyebilirsin:

```shell
# Search for relevant guides
npx modern-web-guidance@latest search "animate a dialog modal backdrop"

# Retrieve a guide by ID
npx modern-web-guidance@latest retrieve "animate-to-from-top-layer"

# Scan a codebase for legacy patterns to modernize
npx modern-web-guidance@latest legacy-scan .
```

İlk komut sorguya en yakın kılavuzları listeler, ikincisi kılavuzun tamamını (kod kalıbı, tuzaklar, fallback) basar, üçüncüsü projedeki eski kalıpları tarar. Kurulumdan sonra agent'a "modal dialog ekle, açılırken animasyonlu olsun" gibi bir web görevi ver; skill devreye girer, agent arkada `search` ve `retrieve` çalıştırıp kılavuzu okuyarak kodu yazar. README'nin eval tablosuna göre kılavuzla görev başarı oranı Claude Code'da %57'den %88'e çıkıyor.

## Sık takılınan yerler

- İki depo var: kurulum `GoogleChrome/modern-web-guidance` (salt okunur dağıtım) ve npm paketi üzerinden, kaynak ve issue'lar `-src` deposunda. Kaynak depoyu klonlamak skill'i agent'a kurmaz ([CONTRIBUTING → Repository Architecture](https://github.com/GoogleChrome/modern-web-guidance-src/blob/main/CONTRIBUTING.md#repository-architecture)).
- Origin Trial ve "volatile" (tek motorda yeni çıkmış) özelliklerin kılavuzları `search` ile görünür ama `retrieve` ile kilitlidir; proje köküne `.mwgrc` dosyasıyla `"allowOriginTrials": true` ya da `"allowVolatileFeatures": true` eklemen (ya da `set-allow-origin-trials` komutu) gerekir ([README → Project Configuration](https://github.com/GoogleChrome/modern-web-guidance-src#project-configuration-mwgrc)).
- Google anonim kullanım verisi toplar (kurulum sayısı, agent adı, kılavuz kimlikleri, agent'ın ürettiği arama sorguları); kapatmak için shell profiline `export DISABLE_TELEMETRY=1` ([README → Telemetry & Privacy](https://github.com/GoogleChrome/modern-web-guidance-src#-telemetry--privacy)).
- Claude Code plugin kurulumunda `/plugin` menüsünden GoogleChrome marketplace'i seçip AutoUpdate'i açman ve `/reload-plugins` demen gerekiyor; yoksa skill eski kalır ([README → Claude Code Plugin](https://github.com/GoogleChrome/modern-web-guidance-src#-alternative-installation-methods)).
- Kaynak depo Node 22.18.0 altında kurulmaz (`engines`), paket yöneticisi pnpm'e sabitlenmiş; npm ya da yarn ile `pnpm preflight` adımları çalışmaz ([CONTRIBUTING → Development Setup](https://github.com/GoogleChrome/modern-web-guidance-src/blob/main/CONTRIBUTING.md#development-setup--quality-gate)).

## Bağlantılar

- Repo (kaynak): https://github.com/GoogleChrome/modern-web-guidance-src
- Dağıtım deposu: https://github.com/GoogleChrome/modern-web-guidance
- Belgeler: https://developer.chrome.com/docs/modern-web-guidance, [Eval Dashboard](https://googlechrome.github.io/modern-web-guidance-src/), [CONTRIBUTING.md](https://github.com/GoogleChrome/modern-web-guidance-src/blob/main/CONTRIBUTING.md)
- npm paketi: https://www.npmjs.com/package/modern-web-guidance
- Sürümler: https://github.com/GoogleChrome/modern-web-guidance-src/releases (kaynak depoda sürüm etiketi yok; haftalık yayın npm paketine gidiyor)
- Lisans: https://github.com/GoogleChrome/modern-web-guidance-src/blob/main/LICENSE (kod Apache-2.0; `guides/` altındaki kılavuz içeriği CC-BY 4.0)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [trend Short t2026-11-12-3](../bolumler/trend-short-t2026-11-12-3.md) videosu için hazırlandı (yakında — 13 Kas 16:30). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
