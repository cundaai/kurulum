---
repo: ranxianglei/billion-context
url: https://github.com/ranxianglei/billion-context
lisans: MIT (ek atıf koşulu; GitHub spdx: NOASSERTION)
surum: v0.1.213 (2026-11-07)
readme_tarihi: 2026-11-12
dogrulama: denenmedi
bolum: gunun-reposu/g07
ozet: Coding agent ile model API'si arasına giren, konuşma geçmişini özetleyerek sıkıştıran ve günlerce süren tek oturuma izin veren proxy.
---

# billion-context — kurulum ve ilk örnek

> **Bu belge, README'nin 12 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

billion-context (`bili` komutu), Claude Code, Codex, OpenCode, pi, Gemini CLI gibi coding agent'larla model API'si arasına giren bir proxy'dir. Uzun oturumlarda biriken konuşmayı katmanlı özetlere sıkıştırır; ne zaman ve neyi sıkıştıracağına model kendisi karar verir, gerekince özet geri açılır. Böylece 100K'lık bir context window ile günlerce süren tek bir oturum sürdürülebilir, token masrafı düşer ve prefix cache bozulmaz. Uzun süren agent oturumlarında "context doldu" sorununu yaşayan geliştiriciler için.

## Gereksinimler

- İşletim sistemi: Linux, macOS ve Windows (README üçünü de anlatıyor); Docker ile de çalışır.
- Node.js 20 ve üzeri ile npm (`package.json` içindeki `engines.node` değeri `>=20`). README'nin Docker notuna göre kendi indirdiği SQLite motoru Node 22 ister ve Node 20'de çöker; güvenli taraf Node 22+.
- Desteklenen bir agent: Claude Code, Codex, OpenCode, pi, omp, dsh, Kimi, Hermes, Gemini CLI, Qwen Code, aider, Copilot CLI ve diğerleri (README'deki "Which do I need?" tablosu).
- Kullandığın agent'ın kendi hesabı ya da API anahtarı. bili'nin kendisi anahtar istemez; anahtar proxy'den olduğu gibi geçer.

## Kurulum

### npm ile (önerilen)

Linux ve macOS'ta kullanıcı seviyesinde kur; `sudo` ve npm ayarı gerekmez, kendini güncellerken izin hatası almaz:

```bash
npm install -g billion-context --prefix=~/.local
```

`bili` komutu `~/.local/bin` altına iner. PATH'te değilse `~/.zshrc` ya da `~/.bashrc` dosyasına şu satırı ekle: `export PATH="$HOME/.local/bin:$PATH"`. nvm ya da Homebrew ile kurulmuş Node'da ve Windows'ta prefix zaten kullanıcıya ait; düz `npm install -g billion-context` yeter.

### Docker ile

`Dockerfile` repo kökünde; önce repoyu klonlayıp içinde çalıştır. Image yayımlanmış npm paketini kurar:

```bash
docker build -t billion-context .
docker run -d --name bili -p 8787:8787 \
  -v bili-config:/root/.config/billion-context \
  -v bili-data:/root/.local/share/billion-context \
  -v bili-cache:/root/.cache/billion-context \
  -v bili-state:/root/.local/state/billion-context \
  billion-context
```

### Kaynaktan (geliştirme)

README uyarıyor: `master` geliştirme dalıdır, her an bozuk olabilir. Kod üzerinde çalışmıyorsan npm ile release kur.

```bash
git clone https://github.com/ranxianglei/billion-context.git
cd billion-context
npm ci
npm run build
npm link
bili --version
```

## İlk çalıştırma

README üç yol veriyor; en kolayı launcher. Tek komut hem proxy'yi hem agent'ı başlatır, hiçbir ayar dosyasına dokunmaz:

```bash
bili claude                           # launch claude through the proxy
```

`bili codex`, `bili pi`, `bili opencode`, `bili gemini` aynı biçimde çalışır. Agent'ı her zamanki gibi kullan; proxy arkada çalışır.

İkinci yol, native plugin: `bili plugin install opencode` (pi, omp, dsh, kimi, hermes, zcode ve claude için de var). Kurulduktan sonra agent'ı normal başlatırsın. Claude Code için marketplace yolu da var: `/plugin marketplace add ranxianglei/billion-context`, ardından `/plugin install billion-context@billion-context` ve `/billion-context:bili-setup`.

Üçüncü yol, URL değişikliği. Proxy'yi tek başına başlat:

```bash
bili
```

Sonra agent'ın baseURL'inin önüne `http://localhost:8787/bili/` ekle:

```
client baseURL before:  https://api.openai.com/v1
client baseURL after:   http://localhost:8787/bili/https://api.openai.com/v1
```

Çalıştığını doğrula:

```bash
# Health check (proxy up + where it forwards)
curl -s http://localhost:8787/__bili/health
# → {"ok":true,"upstream":"https://api.anthropic.com"}
```

Agent'tan bir mesaj gönder ve `~/.local/state/billion-context/bili.log` dosyasına bak: her istekte bir `processTurn` satırı, konuşma büyüdükçe `[acp-usage] round N input=X cached=Y (cache hit Z%)` satırı ve bir `compress` olayı görmelisin. Web paneli `http://localhost:8787` adresinde; native plugin olan agent'larda `/acp` durumu, `/acp-cache` cache raporunu gösterir. Sağlıklı oturumda cache isabeti %95–97 olmalı.

## Sık takılınan yerler

- Eski, root'a ait npm prefix'inde `EACCES` hatası: `sudo` kullanma, `--prefix=~/.local` ile yeniden kur ve sonraki her npm kurulumunda bayrağı tekrar ver ([README → Install](https://github.com/ranxianglei/billion-context#install)).
- Native plugin ile bağımsız eklentiler (`billion-context-pi`, `opencode-acp`) aynı anda açık olursa çift sıkıştırma olur; kurucu eski girdiyi değiştirir ve yedeğini `.bili-bak` olarak alır, proje içi kurulumu elle kaldırman gerekir ([README → Native mode](https://github.com/ranxianglei/billion-context#quickstart)).
- OpenCode 2.x'te plugin listesine çıplak paket adı yazmak çalışmaz; `bili plugin install opencode` kullan ya da README'deki tek satırlık shim dizinini ekle ve `"compaction": { "auto": false }` ayarla ([#3976](https://github.com/ranxianglei/billion-context/issues/3976)).
- Claude Desktop uygulamasının Code sekmesi kendi `ANTHROPIC_BASE_URL` değerini dayatır; masaüstü oturumları proxy'yi tamamen atlar, terminal oturumları etkilenmez ([#2290](https://github.com/ranxianglei/billion-context/issues/2290)). Ayrıca bili yönlendirmesi açıkken Claude Code hesaba bağlı özellikleri (Remote Control) kapatır ([#4693](https://github.com/ranxianglei/billion-context/issues/4693)).
- Docker image'ını Node 20 tabanına çevirme; SQLite motoru Node 22 ister ve Node 20'de çöker ([#3341](https://github.com/ranxianglei/billion-context/issues/3341)). Container `0.0.0.0:8787` dinler ve kimlik doğrulaması yoktur; portu yalnız güvendiğin ağa aç.

## Bağlantılar

- Repo: https://github.com/ranxianglei/billion-context
- Belgeler: [docs/clients.md](https://github.com/ranxianglei/billion-context/blob/master/docs/clients.md) (agent'a göre ayrıntılar), [docs/configuration.md](https://github.com/ranxianglei/billion-context/blob/master/docs/configuration.md), [docs/technical-notes.md](https://github.com/ranxianglei/billion-context/blob/master/docs/technical-notes.md)
- npm paketi: https://www.npmjs.com/package/billion-context
- Sürümler: https://github.com/ranxianglei/billion-context/releases
- Lisans: https://github.com/ranxianglei/billion-context/blob/master/LICENSE — MIT artı bir ek koşul: yazılımı kullanan, kullanıcıya görünen her ürün ana sayfasında, belgelerinde ya da "About" sayfasında billion-context'e bağlantıyla atıf vermek zorunda. GitHub bu yüzden lisansı sınıflandıramıyor (NOASSERTION).

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki Günün Reposu g07 videosu için hazırlandı: https://www.youtube.com/watch?v=ORr4Kmvx4dQ. Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
