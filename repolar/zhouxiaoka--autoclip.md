---
repo: zhouxiaoka/autoclip
url: https://github.com/zhouxiaoka/autoclip
lisans: MIT
surum: v1.5.9 (2026-11-22)
readme_tarihi: 2026-11-23
dogrulama: denenmedi
bolum: 2026-W47
ozet: Podcast, röportaj ve ders gibi uzun videoları bağlantıdan alıp altyazılı, kapaklı ve paylaşım metinli kısa videolara çeviren açık kaynak masaüstü aracı.
---

# autoclip — kurulum ve ilk örnek

> **Bu belge, README'nin 23 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

AutoClip'e bir video bağlantısı yapıştırıp platformu seçersin; o da platformun kadraj ve süre kurallarına uygun kısa videoları, kapağı ve paylaşım metnini üretir. README'ye göre Douyin, Xiaohongshu, TikTok, Reels, YouTube Shorts, Bilibili ve YouTube için çıktı verir; her platformda en yüksek puanlı en çok 10 klibi kendiliğinden hazırlar, her klip kapak, başlık, açıklama, etiket ve ZIP paylaşım paketiyle gelir. Kesme, kadrajlama ve render senin bilgisayarında yapılır; çözümleme modelini sen seçersin (bulut API, Ollama ya da LM Studio). Uygulama ücretsiz ve MIT lisanslı; bulut modelleri kendi sağlayıcının tarifesiyle ücretlenir. Masaüstü uygulaması, CLI ve MCP sunucusu aynı üretim hattını paylaşır.

README'nin aslı Çince; İngilizcesi repodaki `README-EN.md` dosyasında. Komutlar o dosyadan alındı (iki kod bloğundaki Çince açıklama satırları README'de öyle).

## Gereksinimler

README'nin "Quick start" tablosuna göre yol seçersin:

- Masaüstü: macOS Apple Silicon ya da Windows 10 / 11 x64. Python ve FFmpeg kurulum dosyasının içinde gelir. Intel Mac için masaüstü sürümü yok.
- Docker (kendi sunucun, Linux, Intel Mac): Docker ve Compose v2.
- CLI / MCP: Python 3.10+ (3.11 önerilir), PATH'te FFmpeg ve FFprobe; yerel CLI Redis istemez.
- Model: bir bulut sağlayıcısının API anahtarı ya da yerelde çalışan Ollama / LM Studio. Örnek projeyi görmek için anahtar gerekmez.

## Kurulum

Masaüstü: https://github.com/zhouxiaoka/autoclip/releases/latest adresinden indir. macOS Apple Silicon için `.dmg`, Windows için `-setup.exe`. Kurulum dosyaları şimdilik imzasız; macOS 15 ve sonrasında engellenirse System Settings → Privacy & Security → Open Anyway, Windows SmartScreen çıkarsa More info → Run anyway (README).

Docker / Web:

```bash
git clone https://github.com/zhouxiaoka/autoclip.git
cd autoclip
cp env.example .env
mkdir -p data logs uploads
docker compose up -d --build
```

Sonra web arayüzü http://localhost:3000 adresinde açılır; API belgeleri http://localhost:8000/docs.

CLI / MCP: Releases sayfasından resmî CLI / MCP ZIP dosyasını indir, aç ve o klasörde çalıştır:

```bash
python3 -m venv venv
source venv/bin/activate
python -m pip install -r requirements.txt
python -m pip install --force-reinstall --no-deps autoclip_mcp-*-py3-none-any.whl
```

ZIP olmadan tek komutla kurulum (README'deki blok):

```bash
# 一行安装
uvx autoclip-mcp
npx skills add zhouxiaoka/autoclip
```

Bu komutlar macOS / Linux içindir. Windows'ta README'ye göre ortam `py -m venv venv` ile kurulur, `.\venv\Scripts\Activate.ps1` ile açılır; `python -m pip` komutları aynıdır.

## İlk çalıştırma

Masaüstünde README'nin adımları:

1. Model ayarla: sağlayıcıyı seç, API anahtarını gir (`<API_ANAHTARI>`), çözümleme modelini seç, bağlantıyı sına ve kaydet. Yerel model için önce Ollama ya da LM Studio'yu başlatıp bir model yükle.
2. Ana ekranda proje yokken "用示例项目看看效果" (örnek projeyi gör) düğmesine bas; örnek kurulum dosyasının içinde gelir, anahtar istemez.
3. Kendi röportaj ya da podcast videonu ver, dikey düzeni seç.
4. Altyazıyı, kadrajı ve içeriğin bütünlüğünü gözden geçir; paylaşım paketini kaydet ya da hesabını bağlayıp yayınla.

Komut satırında (önce model ayarı kayıtlı olmalı; `--srt` verirsen yazıya dökme adımı atlanır):

```bash
autoclip --version
autoclip produce talk.mp4 --srt talk.srt --platform douyin --portrait-style podcast --json
autoclip outputs PROJECT_ID --export-kits
```

`PROJECT_ID` yerine üretimin döndürdüğü proje kimliğini yaz. MCP sunucusunu istemci başlatır; tek başına denemek için ayrı bir terminalde `autoclip mcp`.

## Sık takılınan yerler

- Linux'ta Docker klasör bağlamaları izin hatası verirse README'nin komutu:

```bash
docker compose run --rm --no-deps --user root --entrypoint sh autoclip -c 'chown -R autoclip:autoclip /app/data /app/logs /app/uploads'
docker compose up -d
```

- Docker içinde `localhost` kapsayıcının kendisidir; Ollama / LM Studio için kapsayıcının erişebildiği bir adres ver. Yerel ağ adresi ya da özel alan adı kullanıyorsan arayüz adresini `.env` içindeki `AUTOCLIP_ALLOWED_ORIGINS` değerine ekle.
- Kaynakta altyazı yoksa ve yerel Whisper kurulmuyor ya da yavaşsa Settings → Default models → "Speech to subtitles" altından bulut yazıya dökmeye geç; ses buluta yüklenir. İlk yerel kurulum yaklaşık 220 MB indirir.
- Hiç klip çıkmadıysa README'nin denetim listesi: altyazı / yazıya dökme, model bağlantısı, FFmpeg, disk alanı ve platform uygunluğu. YouTube uzun biçimi en az 180 saniyelik tam klip ister; daha kısası için Shorts ya da Bilibili seç.
- Proje tek kişi tarafından sürdürülüyor; README canlı destek ya da bire bir kurulum yardımı olmadığını söylüyor.

## Bağlantılar

- Repo: https://github.com/zhouxiaoka/autoclip
- İngilizce README: https://github.com/zhouxiaoka/autoclip/blob/main/README-EN.md
- Site: https://zhouxiaoka.github.io/autoclip_intro/ · Örnek klipler: https://zhouxiaoka.github.io/autoclip_intro/cases/
- Belgeler: kurulum `docs/USER_INSTALLATION_GUIDE.en.md`, Docker `docs/DOCKER.en.md`, sorun giderme `docs/FAQ.en.md`, CLI / MCP `docs/CLI_AND_MCP.md` (Çince)
- Sürümler: https://github.com/zhouxiaoka/autoclip/releases (son sürüm v1.5.9, 22 Kasım 2026)
- Issues: https://github.com/zhouxiaoka/autoclip/issues · Bilinen sorunlar: https://github.com/zhouxiaoka/autoclip/issues/96 · Discussions: https://github.com/zhouxiaoka/autoclip/discussions
- Video: Haftanın Popüler Repoları, 16–22 Kasım 2026 bölümü

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W47 bölümü](../bolumler/2026-W47.md) videosu için hazırlandı (video henüz yayında değil). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
