---
repo: lightpanda-io/browser
url: https://github.com/lightpanda-io/browser
lisans: AGPL-3.0
surum: 1.0.3 (2026-11-05)
readme_tarihi: 2026-11-12
dogrulama: denenmedi
bolum: gunun-reposu/g08
ozet: Zig ile sıfırdan yazılmış, Chromium'dan çok daha hafif, AI agent'ları ve web otomasyonu için tasarlanmış headless browser.
---

# Lightpanda — kurulum ve ilk örnek

> **Bu belge, README'nin 12 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

Lightpanda, Chromium ya da WebKit çatalı olmayan, Zig ile sıfırdan yazılmış bir headless browser'dır. Grafik çizim motoru yoktur; JavaScript'i V8 ile çalıştırır, DOM'u kurar, CDP ve WebDriver BiDi sunar. README'deki ölçüme göre Chromium'dan 16 kat az bellek kullanıp 9 kat hızlı çalışıyor. Yüzlerce sayfayı aynı anda gezen crawler, scraper ve AI agent yazan geliştiriciler için; Puppeteer ya da Playwright script'in `browserWSEndpoint` değişikliğiyle aynen çalışır.

## Gereksinimler

- Linux x86_64 ve aarch64 (glibc tabanlı dağıtım; release binary'leri x86_64'te glibc 2.35, arm64'te 2.38 ister), macOS aarch64 ve x86_64.
- Windows: native binary yok; WSL2 içinde Linux adımları uygulanır. Android/Termux: desteklenmiyor.
- Homebrew, yay (Arch), Nix ya da Docker: isteğe bağlı, her biri ayrı kurulum yolu.
- Puppeteer örneği için Node.js ve `puppeteer-core`; README Node sürümü belirtmiyor.
- Agent modu için bir LLM sağlayıcı anahtarı (Anthropic, OpenAI, Gemini, Vertex, OpenRouter, Ollama ile yerel model…) ortam değişkeninde; `--no-llm` ile anahtarsız REPL de var.
- Kaynaktan derleme için Zig 0.17.0, Rust, Debian/Ubuntu'da `clang make curl git`, macOS'ta `cmake`.

## Kurulum

README'nin ana yolu nightly build'lerdir; paket yöneticileri de nightly kurar. Sürüm etiketi olarak son release 1.0.3.

### Paket yöneticisi

```console
brew install lightpanda-io/browser/lightpanda
```

Arch Linux (AUR):

```console
yay -S lightpanda-nightly-bin
```

### Nightly binary indirme

Linux x86_64:

```console
curl -L -o lightpanda https://github.com/lightpanda-io/browser/releases/download/nightly/lightpanda-x86_64-linux && \
chmod a+x ./lightpanda
```

macOS (Apple Silicon):

```console
curl -L -o lightpanda https://github.com/lightpanda-io/browser/releases/download/nightly/lightpanda-aarch64-macos && \
chmod a+x ./lightpanda
```

Linux aarch64 ve macOS x86_64 binary'leri de [nightly sayfasında](https://github.com/lightpanda-io/browser/releases/tag/nightly). Çalıştırmadan önce doğrula:

```console
./lightpanda version
```

### Windows + WSL2

Yönetici shell'inde `wsl --install`, yeniden başlat, `wsl` aç ve Linux adımlarını uygula. Puppeteer/Playwright WSL içinde ya da Windows tarafında çalışabilir; WSL `localhost:9222` portunu kendiliğinden iletir.

### Docker

Linux amd64 ve arm64 için resmi image var. Şu komut CDP sunucusunu 9222 portunda açar:

```console
docker run -d --name lightpanda -p 127.0.0.1:9222:9222 lightpanda/browser:latest
```

### Nix

```console
nix run github:lightpanda-io/browser -- --help
```

### Kaynaktan

Zig 0.17.0 ve Rust kurulu olmalı. Bir kez `make download-v8` ile hazır V8 arşivini indir (yoksa V8 kaynaktan derlenir, dakikalar sürer), sonra `make build` ya da `zig build run`. Ayrıntı README'nin "Build from sources" bölümünde ve [CONTRIBUTING.md](https://github.com/lightpanda-io/browser/blob/main/CONTRIBUTING.md) içinde.

## İlk çalıştırma

En küçük örnek, bir sayfayı indirip HTML'ini basmak:

```console
./lightpanda fetch --obey-robots --dump html --log-format pretty  --log-level info https://demo-browser.lightpanda.io/campfire-commerce/
```

Terminale sayfanın JavaScript çalıştırılmış HTML'i düşer. `--dump markdown` Markdown verir, `--dump png > page.png` ve `--dump pdf > page.pdf` metin odaklı görüntü üretir, `--dump structured_data` JSON-LD ve meta etiketlerini JSON olarak basar.

Puppeteer ile kullanmak için CDP sunucusunu başlat:

```console
./lightpanda serve --obey-robots --log-format pretty  --log-level info --host 127.0.0.1 --port 9222
```

Script'inde yalnız bağlantı satırı değişir; README'deki örneğin özü:

```js
import puppeteer from 'puppeteer-core';

// use browserWSEndpoint to pass the Lightpanda's CDP server address.
const browser = await puppeteer.connect({
  browserWSEndpoint: "ws://127.0.0.1:9222",
});

// The rest of your script remains the same.
const context = await browser.createBrowserContext();
const frame = await context.newPage();
```

MCP olarak kullanmak için agent'ının MCP ayarına şunu ekle:

```json
{
  "mcpServers": {
    "lightpanda": {
      "command": "/path/to/lightpanda",
      "args": ["mcp"]
    }
  }
}
```

Agent modu, anahtarı ortam değişkeninden kendi bulur (`<SAGLAYICI_API_ANAHTARI>` değerini export ettikten sonra): `./lightpanda agent --task "top story on news.ycombinator.com?"`. Oturumun çıktısı bir PandaScript dosyasıdır; `/save` ile kaydedip `./lightpanda run session.js` ile modelsiz tekrar çalıştırırsın.

## Sık takılınan yerler

- Alpine gibi musl tabanlı Linux'ta binary `cannot execute: required file not found` ile çıkar; glibc tabanlı image kullan. arm64'te `debian:bookworm-slim` (glibc 2.36) yetmez, `ubuntu:24.04` ya da `debian:trixie-slim` gerekir ([README notu](https://github.com/lightpanda-io/browser#install)).
- Puppeteer'da birden çok context ya da sayfa açınca `createBrowserContext` hata veriyor; çoklu context henüz desteklenmiyor ([#882](https://github.com/lightpanda-io/browser/issues/882), [#1096](https://github.com/lightpanda-io/browser/issues/1096)).
- Synology NAS üzerinde Docker container'ı açılışta `fatal ... err=Unexpected` ile düşüyor ([#1904](https://github.com/lightpanda-io/browser/issues/1904)).
- 64 KB sayfa boyutlu çekirdek kullanan arm64 sistemlerde binary fatal ile çıkıyor ([#1370](https://github.com/lightpanda-io/browser/issues/1370)).
- Telemetri varsayılan olarak açık; kapatmak için `LIGHTPANDA_DISABLE_TELEMETRY=true` ([README → Telemetry](https://github.com/lightpanda-io/browser#telemetry)).

## Bağlantılar

- Repo: https://github.com/lightpanda-io/browser
- Belgeler: https://lightpanda.io/docs (agent, PandaScript ve MCP kılavuzları)
- Docker image: https://hub.docker.com/r/lightpanda/browser
- Sürümler: https://github.com/lightpanda-io/browser/releases
- Lisans: https://github.com/lightpanda-io/browser/blob/main/LICENSE (AGPL-3.0)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki Günün Reposu g08 videosu için hazırlandı (yakında — 18 Kas 12:30). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
