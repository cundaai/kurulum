---
repo: firecrawl/firecrawl
url: https://github.com/firecrawl/firecrawl
lisans: AGPL-3.0
surum: v2.11.0 (2026-06-19)
readme_tarihi: 2026-11-19
dogrulama: denenmedi
bolum: 2026-W48-2
ozet: Web sitelerini AI agent'lar için temiz markdown'a çeviren; kendi bilgisayarında tek dosya olarak, bulutta ya da kendi sunucunda çalışan scrape, crawl ve arama aracı.
---

# firecrawl — kurulum ve ilk örnek

> **Bu belge, README'nin 19 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

Firecrawl, AI agent'lara web'den bilgi taşıyan altyapıdır: sayfayı scrape eder, siteyi crawl eder, sitedeki adresleri listeler (map) ve sonucu LLM'e hazır temiz markdown olarak verir. README üç kullanım yolu sayıyor:

- **Firecrawl Local**: kendi bilgisayarında tek binary. Veritabanı, kuyruk ya da container gerekmez; AI agent'ına MCP server olarak bağlanır ve Firecrawl API'sini localhost'ta sunar.
- **Firecrawl Cloud**: firecrawl.dev'deki barındırılan hizmet. Local'ın yaptıklarına ek olarak arama, yapılandırılmış JSON, Interact, Agent ve 550'den fazla veri sağlayıcılı Alexandria.
- **Self-hosting**: `apps/api` altındaki tam sunucuyu kendi kuyrukları, worker'ları ve veritabanlarıyla kendi altyapında çalıştırmak.

Bu repo Firecrawl Local'ın ve self-hosted sunucunun kodunu, bir de üçüyle çalışan SDK'ları barındırır. Lisans ağırlıklı olarak AGPL-3.0; SDK'lar ve bazı arayüz bileşenleri MIT.

## Gereksinimler

Firecrawl Local için (README):

- Rust ve bir C derleyicisi. macOS'ta `xcode-select --install`, Debian ya da Ubuntu'da `build-essential`.
- İsteğe bağlı: bilgisayarında kurulu Chrome ya da Chromium. JavaScript gerektiren sayfalar onunla işlenir; tarayıcı yoksa statik sayfalar yine çalışır, tarayıcı gereken sayfa uyarıyla döner.
- İsteğe bağlı: Node.js 22 ya da üstü. MCP server bununla çalışır.

Rust kurmak istemeyenler için: Node.js (`npx`) ile Firecrawl Cloud CLI; README'ye göre kayıt gerekmez.

Firecrawl Cloud için: Firecrawl Keyless ile API anahtarı ve kayıt olmadan ayda 1.000 ücretsiz kredi (MCP, CLI ve API'de); API anahtarı için firecrawl.dev'de hesap.

Self-hosting için: Docker Compose; sunucu Redis, PostgreSQL ve bir tarayıcı servisiyle birlikte çalışır.

## Kurulum

Firecrawl Local, crates.io'dan kurulur:

```bash
cargo install firecrawl-local
```

Rust yoksa, README'nin önerdiği derlemesiz yol Firecrawl Cloud'un CLI'ıdır (aynı scrape'i bulutta çalıştırır):

```bash
npx -y firecrawl-cli@latest scrape https://firecrawl.dev
```

AI agent'ına ekleme: `init`, bilgisayarındaki Claude Code, Claude Desktop, Cursor, Windsurf, Gemini CLI ve Codex'i bulur, her birine Firecrawl Local'ı MCP server olarak ekler ve önce her ayar dosyasının yedeğini alır:

```bash
firecrawl-local init
```

Ardından AI aracını yeniden başlat. Scrape, crawl, map ve batch scrape araçları kendi bilgisayarında çalışır; öbür Firecrawl araçları Firecrawl Cloud'a gider.

Bulut araçlarının tamamını agent'a bağlamak için (Claude Code, Antigravity, OpenCode ve başkaları; kurulumdan sonra agent yeniden başlatılır):

```bash
npx -y firecrawl-cli@latest init --all --browser
```

Herhangi bir MCP client için README'deki ayar (anahtarın yerine kendi anahtarını yaz):

```json
{
  "mcpServers": {
    "firecrawl-mcp": {
      "command": "npx",
      "args": ["-y", "firecrawl-mcp"],
      "env": {
        "FIRECRAWL_API_KEY": "<API_ANAHTARI>"
      }
    }
  }
}
```

SDK'lar: Python için `pip install firecrawl`, Node.js için `npm install firecrawl`. README'de Go, Java, Rust, Ruby, .NET, PHP ve Elixir için de kurulum satırları var.

Self-hosting: README bu yol için komut vermiyor; "sunucu Docker Compose ile çalışır" deyip self-hosting kılavuzuna ve repodaki `SELF_HOST.md` dosyasına yönlendiriyor. README ayrıca kendi başına çalıştırmanın en kolay yolunun Firecrawl Local olduğunu söylüyor.

## İlk çalıştırma

Bir sayfayı scrape et ve markdown'ını yazdır:

```bash
firecrawl-local scrape https://firecrawl.dev --markdown
```

Bir siteyi crawl et, her sayfayı ayrı markdown dosyası olarak kaydet:

```bash
firecrawl-local crawl https://docs.firecrawl.dev --limit 50 --out ./pages
```

Bir sitedeki bütün adresleri listele:

```bash
firecrawl-local map https://firecrawl.dev
```

Firecrawl API'sini localhost'ta sun: `serve`, Firecrawl v2 API'sine 3002 portunda yanıt verir; Firecrawl için yazılmış kod değişmeden çalışır ve yerel route'lar API anahtarı istemez.

```bash
firecrawl-local serve
```

Herhangi bir Firecrawl SDK'sını API adresini vererek buna yönlendir:

```python
from firecrawl import Firecrawl

app = Firecrawl(api_url="http://localhost:3002")
print(app.scrape("https://firecrawl.dev").markdown)
```

Bulutta aynı işler için README Python, Node.js, cURL ve CLI örnekleri veriyor (Search, Scrape, Interact, Agent, Crawl, Map, Batch Scrape); CLI örnekleri `firecrawl search "firecrawl" --limit 5` ve `firecrawl scrape https://firecrawl.dev` biçiminde. SDK örneklerinde API anahtarı `Firecrawl(api_key="<API_ANAHTARI>")` ile verilir.

## Sık takılınan yerler

- Varsayılan ayarda, kendi bilgisayarında yüklenemeyen tek sayfalık bir scrape Firecrawl Cloud üzerinden alınır ve sonuçta `"source": "firecrawl-cloud"` diye işaretlenir. Hiçbir şeyin buluta gitmemesi için `FIRECRAWL_CLOUD=off` ayarla (README, "What runs where").
- Bu bulut çağrıları hesapsız, sınırlı hızda çalışır; `firecrawl-local login` onları kendi Firecrawl hesabına bağlar. Alexandria, Local'da yalnız bu girişten sonra kullanılır.
- Arama (search) Local'da da Firecrawl Cloud üzerinden çalışır; Interact, Agent ve Monitor yalnız Cloud'da var. Local'da scrape markdown, HTML, raw HTML ve bağlantı verir; yapılandırılmış JSON, özet, ekran görüntüsü ve tarayıcı eylemleri Cloud'da.
- Tarayıcı kurulu değilse JavaScript gerektiren sayfa uyarıyla döner; statik sayfalar çalışır.
- MCP server Node.js 22 ya da üstünü ister; `init` sonrası AI aracını yeniden başlatmak gerekir.
- Self-hosted sunucuda Alexandria yok; yalnız Firecrawl Cloud'da çalışır.
- README'nin notu: scrape ederken sitelerin kurallarına uymak kullanıcının sorumluluğundadır; Firecrawl varsayılan olarak robots.txt'ye uyar.

## Bağlantılar

- Repo: https://github.com/firecrawl/firecrawl
- Site: https://firecrawl.dev · Playground: https://firecrawl.dev/playground
- Belgeler: https://docs.firecrawl.dev · API Reference: https://docs.firecrawl.dev/api-reference/introduction
- Firecrawl Local README (bütün komutlar, `serve` güvenlik ayarları, Docker): https://github.com/firecrawl/firecrawl/tree/main/apps/local
- Self-hosting: https://docs.firecrawl.dev/contributing/self-host · https://github.com/firecrawl/firecrawl/blob/main/SELF_HOST.md
- CLI ve skill belgeleri: https://docs.firecrawl.dev/sdks/cli · MCP: https://github.com/firecrawl/firecrawl-mcp-server
- Sürümler: https://github.com/firecrawl/firecrawl/releases (son sürüm v2.11.0, 19 Haziran 2026)
- Issues: https://github.com/firecrawl/firecrawl/issues · Discord: https://discord.gg/firecrawl
- Video: Haftanın Popüler Repoları, 23–25 Kasım 2026 bölümü (listenin 2. sırası)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W48-2 bölümü](../bolumler/2026-W48-2.md) videosu için hazırlandı (video henüz yayında değil). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
