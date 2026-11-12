---
repo: D4Vinci/Scrapling
url: https://github.com/D4Vinci/Scrapling
lisans: BSD-3-Clause
surum: v0.5.1 (2026-11-04)
readme_tarihi: 2026-11-11
dogrulama: denenmedi
bolum: 2026-W45
ozet: Hızlı HTTP, stealth tarayıcı, güçlü parser ve eşzamanlı spider'ları tek pakette toplayan Python web scraping çatısı.
---

# Scrapling — kurulum ve ilk örnek

> **Bu belge, README'nin 11 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

Python'da web scraping'in tüm akışını tek kütüphanede toplar: tarayıcı TLS parmak izini taklit eden hızlı HTTP istekleri (`Fetcher`), JavaScript çalıştıran ve Cloudflare Turnstile/Interstitial geçen stealth tarayıcı (`StealthyFetcher`), CSS/XPath/metin/regex ile seçim yapan ve site değişince elemanı yeniden bulan parser, Scrapy benzeri eşzamanlı spider'lar (proxy rotasyonu, yeniden deneme, duraklat/sürdür, JSON/CSV dışa aktarma). Terminalden `scrapling extract` ile kod yazmadan sayfa çekebilir, AI agent'ına MCP server ve Agent Skill ile bağlayabilirsin. Scrapy ya da BeautifulSoup bilen Python geliştiricisine tanıdık gelir.

## Gereksinimler

- Python 3.10 veya üstü ve `pip`.
- Tarayıcı tabanlı fetcher'lar (`StealthyFetcher`, `StealthySession`) için `scrapling install` adımının indirdiği tarayıcılar ve sistem bağımlılıkları; düz HTTP istekleri için tarayıcı gerekmez.
- İşletim sistemi: README belirtmiyor. Tüm ekler ve tarayıcılarla hazır bir Docker imajı var.
- Hesap/anahtar: gerekmiyor. Proxy sağlayıcısı isteğe bağlı.

## Kurulum

Temel paket yalnız parser motorunu kurar:

```bash
pip install scrapling
```

README'nin uyarısı: bu kurulumla `scrapling.fetchers` ya da `scrapling.spiders` import edilemez (`ModuleNotFoundError`). Fetcher ya da spider kullanacaksan fetcher bağımlılıklarını ve tarayıcıları kur:

```bash
pip install "scrapling[fetchers]"

scrapling install           # normal install
scrapling install  --force  # force reinstall
```

`scrapling install` tüm tarayıcıları, sistem bağımlılıklarını ve parmak izi değiştirme bağımlılıklarını indirir. Aynı işi koddan da yapabilirsin:

```python
from scrapling.cli import install

install([], standalone_mode=False)          # normal install
install(["--force"], standalone_mode=False) # force reinstall
```

Ek özellikler isteğe bağlı. MCP server:

```bash
pip install "scrapling[ai]"
```

RAG sistemleri kurmak için bağımlılıklar:

```bash
pip install "scrapling[rag]"
```

Etkileşimli shell ve `extract` komutu:

```bash
pip install "scrapling[shell]"
```

Hepsi birden:

```bash
pip install "scrapling[all]"
```

### Docker

Tüm ekler ve tarayıcılarla hazır imaj (ana daldan GitHub Actions ile üretiliyor):

```bash
docker pull pyd4vinci/scrapling
```

```bash
docker pull ghcr.io/d4vinci/scrapling:latest
```

## İlk çalıştırma

En küçük örnek, tek sayfa (`scrapling[fetchers]` kurulu olmalı; tarayıcı gerekmez):

```python
from scrapling.fetchers import Fetcher

page = Fetcher.get('https://quotes.toscrape.com/')
quotes = page.css('.quote .text::text').getall()
print(quotes)
```

Beklenen çıktı: sayfadaki alıntı metinlerinin listesi. Aynı işi spider ile yapıp dosyaya yazmak için:

```python
from scrapling.spiders import Spider, Response

class QuotesSpider(Spider):
    name = "quotes"
    start_urls = ["https://quotes.toscrape.com/"]

    async def parse(self, response: Response):
        for quote in response.css('.quote'):
            yield {
                "text": quote.css('.text::text').get(),
                "author": quote.css('.author::text').get(),
            }

result = QuotesSpider().start()
print(len(result.items))
result.items.to_json("quotes.json")
```

README'ye göre bu `10` yazdırır ve alıntıları `quotes.json` dosyasına kaydeder. Kod yazmadan, terminalden:

```bash
pip install "scrapling[shell]"
scrapling shell
```

```bash
scrapling extract get 'https://quotes.toscrape.com/' quotes.md
```

## Sık takılınan yerler

- **`ModuleNotFoundError` (`scrapling.fetchers`, `scrapling.spiders`).** `pip install scrapling` yalnız parser'ı kurar; `pip install "scrapling[fetchers]"` gerekir. Kaynak: README, "Installation" uyarısı.
- **Tarayıcı fetcher'ı açılmıyor.** `StealthyFetcher` ve oturum sınıfları için `scrapling install` çalıştırılmış olmalı; `Fetcher` ile düz HTTP için gerekmez. Kaynak: README, "Optional Dependencies".
- **Eski sürümden geliyorsan API değişti.** v0.5 geçiş notları repoda: [docs/migrating-to-v0.5.md](https://github.com/D4Vinci/Scrapling/blob/main/docs/migrating-to-v0.5.md).

## Bağlantılar

- Repo: https://github.com/D4Vinci/Scrapling
- Belgeler: https://scrapling.readthedocs.io/en/latest/ (fetcher seçimi, spider'lar, CLI, MCP server, Agent Skill)
- Sürümler: https://github.com/D4Vinci/Scrapling/releases
- Lisans: https://github.com/D4Vinci/Scrapling/blob/main/LICENSE (BSD-3-Clause)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W45 bölümü](../bolumler/2026-W45.md) videosu için hazırlandı: https://www.youtube.com/watch?v=AkiRwaGZDZA. Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
