---
repo: yannicschuller/flowplan
url: https://github.com/yannicschuller/flowplan
lisans: AGPL-3.0
surum: v0.11.4 (2026-11-23)
readme_tarihi: 2026-11-22
dogrulama: denenmedi
bolum: trend-short/t2026-11-24-1
ozet: Doküman, veritabanı, whiteboard ve günlüğü tek sayfa ağacında toplayan, kendi sunucunda tek container ile çalışan açık kaynak ekip çalışma alanı.
---

# flowplan — kurulum ve ilk örnek

> **Bu belge, README'nin 22 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

Flowplan kendini Notion ve Confluence'a açık kaynak alternatif olarak tanıtıyor: ekipler için dokümanlar, veritabanları, whiteboard'lar ve günlük. README'ye göre her şey tek sayfa ağacında durur: dokümanlar, on bir görünümlü veritabanları, whiteboard'lar, el yazısı için defterler, günlük, ticket ve sprint'li projeler, anketler, workflow'lar ve flashcard'lar. Kendi sunucunda tek container ve SQLite ile çalışır; başka bir servis ya da dış arama servisi gerekmez. Büyük ekipler için isteğe bağlı PostgreSQL ve cluster kipi var. Tamamı AGPL-3.0 lisanslı; README "kapalı enterprise sürümü yok, lisans anahtarı arkasında özellik yok" diyor. Kendin kurmak istemezsen Almanya'da barındırılan hazır sürümü de var (https://app.flowplan.org). Kendi sunucunda çalıştırdığında kurulum, yedek ve bakım sende.

## Gereksinimler

README'nin "System requirements" tablosundan:

- Docker (image `linux/amd64` ve `linux/arm64` için yayımlanıyor) ya da Helm chart ile Kubernetes. Docker'sız çalıştırmak için Node.js 22.13 ya da yenisi.
- Bellek ve işlemci: küçük ekip için 256 MB RAM istenir, 1 GB'a kadar; bir işlemcinin küçük bir bölümü. Taranmış PDF'lerde metin tanıma ve büyük içe aktarmalar kısa süre daha çok işlemci ister.
- Kalıcı bir volume (blok ya da yerel disk; SQLite yüzünden NFS olmaz), isteğe bağlı S3 uyumlu depolama.
- Veritabanı: container içindeki SQLite; isteğe bağlı PostgreSQL 16 ya da yenisi.
- Ağ: reverse proxy arkasında HTTPS'li bir alan adı; kendi ağında deneme için `http://` çalışır.
- Tarayıcı: güncel Chrome, Edge, Firefox (111 ya da yenisi) ve Safari 16.4 ya da yenisi.
- Kişi sayısı: SQLite'lı tek container aynı anda çalışan yaklaşık 20 kişiyi, PostgreSQL ile yaklaşık 30 kişiyi taşır; ötesi için cluster kipi.

## Kurulum

Kendi bilgisayarında denemek için README'deki tek komut:

```sh
docker run -d --name flowplan -p 3000:3000 -v flowplan:/app/data -e APP_URL=http://localhost:3000 ghcr.io/yannicschuller/flowplan:latest
```

`APP_URL`, adresin tarayıcıya yazdığın hâliyle birebir aynı olmalı. Başka bir cihazdan ya da alan adıyla erişmek için değiştir (README örneği: `-e APP_URL=https://flowplan.example.com`) ve önüne HTTPS'li bir reverse proxy koy. Bütün veri `flowplan` volume'unda durur.

Sunucu için depodaki hazır `compose.yaml` ve örnek ayar dosyası:

```sh
curl -O https://raw.githubusercontent.com/yannicschuller/flowplan/main/compose.yaml
curl -o .env https://raw.githubusercontent.com/yannicschuller/flowplan/main/.env.example
# edit .env: at least APP_URL (optionally OIDC_* and SMTP_*)
docker compose up -d
```

Ubuntu 24.04 ya da Debian 12 kurulu bir sunucuda tek komutla kurulum (README'ye göre Docker, HTTPS'li Caddy, üretilmiş gizli anahtarlar ve firewall kurar; `flowplan.example.com` yerine kendi alan adını yaz):

```sh
curl -fsSL https://raw.githubusercontent.com/yannicschuller/flowplan/main/deploy/install.sh | sudo bash -s -- flowplan.example.com
```

README ayrıca Render ve DigitalOcean App Platform için tek tıkla kurulum düğmeleri veriyor. Render'da kalıcı disk ücretli bir instance ister. DigitalOcean App Platform'da volume olmadığı için veri PostgreSQL'e gider ve yüklenen dosyalar için bir Spaces bucket gerekir. Kubernetes için Helm chart `deploy/helm/flowplan` klasöründe.

İsteğe bağlı yerel AI: `docker compose --profile ai up -d` Flowplan'ın yanında Ollama'yı başlatır; `OLLAMA_URL=http://ollama:11434` ayarlanır ve model **Administration → AI** altından seçilir.

## İlk çalıştırma

1. Yukarıdaki `docker run` komutundan sonra tarayıcıda http://localhost:3000 adresini aç.
2. **Create account** seç. İlk açılan hesap instance'ın yöneticisi olur.
3. Sunucu kurulumunda `docker compose up -d` sonrası kendi alan adını aç ve aynı şekilde ilk hesabı oluştur.

README bunun ötesinde adım adım bir "ilk sayfa" örneği vermiyor; kurmadan bakmak için demo adresi var: https://flowplan.org/en/demo

## Sık takılınan yerler

- SQLite ile her veri klasörü için **tam olarak bir** container çalıştır; SQLite kopyalar arasında paylaşılmaz. Birden çok instance için PostgreSQL, Redis ve S3'lü cluster kipi gerekir.
- Volume NFS üzerinde olmamalı (SQLite).
- `APP_URL` tarayıcıya yazdığın adresle aynı değilse erişim sorun çıkarır; adres değişince bu değeri de değiştir.
- İnternete açık sunucuda README, ilk hesabın yalnız `https://flowplan.example.com/register?setup=<token>` adresinden açılması için `FLOWPLAN_SETUP_TOKEN` ayarlamayı öneriyor; böylece instance'ı senden önce başkası kuramaz.
- `:beta` etiketli image en son elle yapılan derlemedir; README "not for production" diyor.
- DigitalOcean App Platform'da Spaces bucket (`S3_*`) ayarlanmazsa dosyalar her deploy'da kaybolur; oradaki dev veritabanının yedeği yoktur.

## Bağlantılar

- Repo: https://github.com/yannicschuller/flowplan
- Site: https://flowplan.org · Demo: https://flowplan.org/en/demo · Hazır sürüm: https://app.flowplan.org
- Belgeler: https://docs.flowplan.org/en (kurulum: https://docs.flowplan.org/en/installation, ayarlar: https://docs.flowplan.org/en/configuration, tek tıkla kurulum: https://docs.flowplan.org/en/one-click-deploy)
- Sürümler: https://github.com/yannicschuller/flowplan/releases (son sürüm v0.11.4, 23 Kasım 2026)
- Issues: https://github.com/yannicschuller/flowplan/issues · Discord: https://discord.gg/bCEdB6MPrR
- Video: Haftanın Popüler Repoları, 24 Kasım 2026 trend Short'u

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki trend Short t2026-11-24-1 videosu için hazırlandı: https://www.youtube.com/shorts/dRrNLuDq7yQ. Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
