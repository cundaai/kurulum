---
repo: harry0703/MoneyPrinterTurbo
url: https://github.com/harry0703/MoneyPrinterTurbo
lisans: MIT
surum: v1.3.10 (2026-11-07)
readme_tarihi: 2026-11-16
dogrulama: denenmedi
bolum: 2026-W46
ozet: Bir konu ya da anahtar kelimeden senaryo, seslendirme, görüntü, altyazı ve müzikle hazır kısa video üreten açık kaynak araç.
---

# MoneyPrinterTurbo — kurulum ve ilk örnek

> **Bu belge, README'nin 16 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

MoneyPrinterTurbo'ya bir video konusu ya da anahtar kelime verirsin; senaryoyu LLM ile yazar, uygun stok görüntüleri bulur (Pexels, Pixabay ya da kendi dosyaların), seslendirmeyi (ör. API anahtarı istemeyen Edge TTS), altyazıyı ve arka plan müziğini ekleyip hazır bir kısa video çıkarır. Dikey 9:16, yatay 16:9 ve kare 1:1 çıktı verir. WebUI, API, CLI ve AI agent yollarıyla kullanılır; OpenAI, Anthropic Claude, Gemini, DeepSeek, Ollama gibi birçok model sağlayıcısını destekler. Ana README Çince; komutlar README'nin İngilizce sürümüyle (README-en.md) aynı. Lisansı MIT.

## Gereksinimler

- Windows 10+, macOS 11+ ya da yaygın bir Linux dağıtımı.
- Yerel kurulum için Python 3.11 ya da üstü (önerilen 3.11). Kaynaktan kurulumda önerilen araç `uv`.
- En az 4 çekirdek CPU ve 4 GB RAM (önerilen 6–8 çekirdek, 8 GB). GPU şart değil; yerel Whisper altyazısı ve toplu üretimi hızlandırır.
- Kullanacağın servislerin API anahtarları (bulut LLM, Pexels/Pixabay görüntü, bulut ses servisleri). Anahtarlar WebUI'nin temel ayarlarına girilir.
- Docker yolu için Docker Desktop (Windows'ta WSL ile).

## Kurulum

README birkaç yol veriyor. En kısası resmî `mpt` paketini pip ile kurmak (repoyu klonlamak gerekmez). Önce sanal ortam (macOS / Linux):

```shell
python3 -m venv .venv
source .venv/bin/activate
```

Windows PowerShell:

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
```

Sonra kur ve başlat:

```shell
python -m pip install mpt
mpt webui
```

README'nin uyarısı: resmî PyPI paketi `mpt`; PyPI'daki `MoneyPrinterTurbo` adlı paket üçüncü bir kişiye ait.

Kaynaktan kurulum (`uv` ile):

```shell
git clone https://github.com/harry0703/MoneyPrinterTurbo.git
cd MoneyPrinterTurbo
uv python install 3.11
uv sync --frozen
```

`uv` kullanmıyorsan `venv + pip`:

```shell
python3.11 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Docker: ilk başlatmadan önce `MoneyPrinterTurbo` klasöründe `config.example.toml` dosyasını `config.toml` olarak kopyala (varsa eskisini koru), sonra:

```shell
cd MoneyPrinterTurbo
docker compose -f docker-compose.release.yml up
```

Bu, GitHub Container Registry'deki hazır imajı (`ghcr.io/harry0703/moneyprinterturbo:latest`) çeker; imajı kendin derlemek istersen `docker compose up`.

Windows'ta en hızlı yol: GitHub Releases sayfasının **Assets** bölümündeki `.7z` tek tık paketini indirip aç, önce `update.bat`, sonra `start.bat` dosyasına çift tıkla. Kurulum istemeyenler için README bir Google Colab defteri de veriyor.

## İlk çalıştırma

1. WebUI'yi başlat. pip kurulumunda `mpt webui` (tarayıcı kendiliğinden açılır; tarayıcısız ve başka portta: `mpt webui --port 8503 --headless`). Kaynaktan kurulumda proje kök klasöründe macOS / Linux için:

```shell
sh webui.sh
```

   Windows'ta `.\webui.bat`. Docker'da tarayıcıdan http://127.0.0.1:8501 adresini aç.
2. WebUI'nin temel ayarlarına kullanacağın LLM ve görüntü servisinin API anahtarlarını gir. `config.toml` ilk açılışta kendiliğinden oluşur (pip kurulumunda veri klasörü macOS'ta `~/Library/Application Support/MoneyPrinterTurbo`).
3. Bir video konusu yaz ve üretimi başlat. Üretilen videolar veri klasöründe `storage/tasks` altına düşer.

Tarayıcısız, yalnız komut satırıyla ilk video (kaynaktan kurulumda, README'nin örneği):

```shell
uv run python cli.py --video-subject "How AI is changing everyday life"
```

Bütün seçenekler için `uv run python cli.py --help`; pip kurulumunda `mpt cli --help`. API servisi için `uv run python main.py`; Docker'da API belgeleri http://127.0.0.1:8080/docs adresinde.

AI agent yolu: Skill okuyup terminal kullanabilen bir agent'a README'deki şu istemi verirsin; agent kurar, ayarlar, videoyu üretir ve eksik API anahtarlarını sorar (README'ye göre şimdilik macOS ve Windows):

```text
Use this Skill: https://raw.githubusercontent.com/harry0703/MoneyPrinterTurbo/main/docs/skill/SKILL.md
Create a video with the topic "How AI is changing everyday life."
```

## Sık takılınan yerler

- `RuntimeError: No ffmpeg exe could be found`: ffmpeg kendiliğinden inemediyse elle indirip `config.toml` içinde `[app]` altındaki `ffmpeg_path` değerini kendi yoluna göre ayarla (README).
- `OSError: [Errno 24] Too many open files`: açık dosya sınırını `ulimit -n` ile kontrol et, düşükse örneğin `ulimit -n 10240` ile artır (README).
- Whisper modeli inmiyorsa (`LocalEntryNotFoundError`): `whisper-large-v3` modelini Hugging Face'ten elle indirip `MoneyPrinterTurbo/models/whisper-large-v3` klasörüne koy (README'nin altyazı bölümü).
- Windows tek tık paketinde Releases'teki `Source code (zip)` dosyası yalnız kaynak koddur; içinde `start.bat` ve `update.bat` yoktur, **Assets** altındaki `.7z` dosyasını indir. WebUI boş açılırsa Chrome ya da Edge kullan (README).
- Windows'ta proje yolunda Türkçe karakter, özel karakter ya da boşluk olmasın (README).

## Bağlantılar

- Repo: https://github.com/harry0703/MoneyPrinterTurbo
- İngilizce README: https://github.com/harry0703/MoneyPrinterTurbo/blob/main/README-en.md
- Sürümler ve Windows paketi: https://github.com/harry0703/MoneyPrinterTurbo/releases (son sürüm v1.3.10, 7 Kasım 2026)
- PyPI paketi: https://pypi.org/project/mpt/
- Issues: https://github.com/harry0703/MoneyPrinterTurbo/issues
- Video: Haftanın Popüler Repoları, 9–15 Kasım 2026 bölümü (listenin 8. sırası)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W46 bölümü](../bolumler/2026-W46.md) videosu için hazırlandı (video henüz yayında değil). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
