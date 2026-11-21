---
repo: debpalash/VoiceStudio
url: https://github.com/debpalash/VoiceStudio
lisans: AGPL-3.0
surum: v0.5.8 (2026-11-12)
readme_tarihi: 2026-11-20
dogrulama: denenmedi
bolum: trend-short/t2026-11-21-1
ozet: Ses klonlama, ses tasarımı, video dublajı, dikte ve transkripsiyonu kendi cihazında yerel modellerle yapan açık kaynak ses stüdyosu.
---

# VoiceStudio — kurulum ve ilk örnek

> **Bu belge, README'nin 20 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

VoiceStudio, kendi cihazında çalışan bir yapay zekâ ses stüdyosu; GitHub açıklamasında kendini "open-source, fully-local ElevenLabs alternative" diye tanıtıyor. README'ye göre yapabildikleri: temiz bir referans kayıttan ses klonlama, videoyu çevirip özgün zamanlamaya göre dublaj, sesi sözle tarif edip üretme (voice design), yüzen bir dikte penceresi ve yerel transkripsiyon, hikâye ve sesli kitap anlatımı, sıraya alınmış işler; ayrıca yerel API, MCP, yeniden kullanılabilir tarifler ve izlenen klasörlerle otomasyon. Varsayılan motor VoiceStudio (k2-fsa/OmniVoice tabanlı); Model Manager'dan başka motorlar seçilir ve yalnız ihtiyacın olan model indirilir. README: yukarıdakilerin hepsi ücretsiz, kendi donanımında çalışır ve hesap istemez; Pro ve Cloud isteğe bağlıdır (Cloud henüz açık değil).

Lisans: uygulama AGPL-3.0. README'ye göre model ağırlıkları, sesler ve üretilen çıktı ayrı lisanslıdır; varsayılan OmniVoice ağırlıkları ticari kullanıma kapalıdır (CC-BY-NC). Ücretli iş için her modelin lisansına Model Manager'da bak. Bir sesi yalnız sahibinin izniyle klonla.

## Gereksinimler

- macOS 13.3 ya da üstü, Apple Silicon (yerel motorlar Metal ile). Intel Mac'te yalnız arayüz çalışır; başka bir bilgisayardaki VoiceStudio backend'ine bağlanır.
- Windows 10/11 x64: NVIDIA CUDA, AMD ROCm (isteğe bağlı, deneysel) ya da CPU. Windows on ARM deneysel.
- Linux x64: NVIDIA CUDA ya da CPU; desteklenen GGUF motorlarında Vulkan.
- Docker: CPU, NVIDIA CUDA ve AMD ROCm imajları var.
- Android 10+ ve iOS / iPadOS 17+ için önizleme sürümleri (model indirildikten sonra çevrimdışı çalışır).
- Ekran kartı şart değil. README: "No GPU? The app is fully usable on the CPU, just slower."
- Kaynaktan çalıştırmak için `git` ve `bun`; Nix yolunda `nix`.

README disk alanı ve bellek için bir alt sınır vermiyor; modeller ayrıca indirilir ve uygulama indirmeden önce sorar.

## Kurulum

README'de "Install" başlıklı ayrı bir bölüm yok; kurulum yolları "Platforms" başlığı altında işletim sistemine göre açılır kutularda duruyor. Aşağıdaki komutlar oradan olduğu gibi alındı.

macOS (Apple Silicon), Homebrew:

```sh
brew tap debpalash/tap
brew trust --cask debpalash/tap/voicestudio
brew install --cask debpalash/tap/voicestudio
```

Homebrew istemiyorsan Releases sayfasından DMG'yi indir, aç ve VoiceStudio'yu Applications klasörüne sürükle.

macOS ve Linux, tek komutluk kurulum betiği:

```sh
curl -fsSL https://voicestudio.sh/install | sh
```

Windows (x64), PowerShell:

```powershell
irm https://voicestudio.sh/install.ps1 | iex
```

Arch, Manjaro, EndeavourOS (AUR):

```sh
yay -S voicestudio-bin
```

Ubuntu, Debian, Linux Mint: Releases sayfasından `.deb` dosyasını bulunduğun klasöre indir, sonra:

```sh
sudo apt install ./VoiceStudio-*-linux-x64.deb
```

Snap (snapd kurulu olmalı):

```sh
sudo snap install voicestudio
```

Snap'te mikrofon erişimi için:

```sh
sudo snap connect voicestudio:audio-record
```

Herhangi bir Linux dağıtımı, AppImage (dosyayı bulunduğun klasöre indirdikten sonra):

```sh
chmod +x VoiceStudio-*-linux-x64.AppImage
./VoiceStudio-*-linux-x64.AppImage
```

Docker, CPU üzerinde yerel çalıştırma (README'nin Bash örneği; API anahtarını komut kendisi üretir ve sonunda ekrana yazar):

```sh
export OMNIVOICE_API_KEY="$(openssl rand -hex 32)"
docker run -d --name omnivoice \
  -p 127.0.0.1:3900:3900 \
  -e OMNIVOICE_API_KEY \
  -e OMNIVOICE_DATA_DIR=/app/omnivoice_data \
  -v omnivoice-data:/app/omnivoice_data \
  ghcr.io/debpalash/voicestudio:stable
printf 'API key: %s\n' "$OMNIVOICE_API_KEY"
```

NVIDIA, AMD ROCm ve Compose için README `docs/install/docker.md` belgesine yönlendiriyor.

Kaynaktan:

```bash
git clone https://github.com/debpalash/VoiceStudio.git
cd VoiceStudio
bun run dev
```

README'ye göre `bun run dev` JavaScript bağımlılıklarını, desteklenen yerel backend kullanılıyorsa Python bağımlılıklarını da kurar ve Electron'u başlatır. Yalnız Python ortamını yeniden kurmak için `bun run setup:api`.

Telefon: Android APK'sı ve iOS IPA'sı Releases sayfasında; iOS'ta imzalama ve yandan yükleme adımları `docs/mobile.md` belgesinde.

## İlk çalıştırma

README'nin "Get started" bölümü tek paragraf:

1. Uygulamayı aç ve **Voice cloning** ekranına gir.
2. Bir ses seç ya da temiz bir referans kayıt ekle.
3. Metnini yaz ve üret. Uygulama herhangi bir modeli indirmeden önce sorar.

Docker yolunda tarayıcıda http://localhost:3900 adresini aç ve komutun ekrana yazdığı API anahtarını kullan.

README bundan öte adım adım bir örnek vermiyor; dublaj, ses tasarımı ve otomasyon için belgeler sayfasına yönlendiriyor.

## Sık takılınan yerler

- macOS'ta "VoiceStudio is damaged and can't be opened" uyarısı indirmenin bozuk olduğu anlamına gelmez; Gatekeeper uygulamayı engellemiştir. Çözüm `docs/install/macos.md` belgesinin "Gatekeeper quarantine" bölümünde.
- `voicestudio.sh` adresine ulaşılamıyorsa README kurulum betiğini doğrudan depodan çalıştırmayı öneriyor: `curl -fsSL https://raw.githubusercontent.com/debpalash/VoiceStudio/main/scripts/install.sh | sh`.
- 0.5.3 ya da daha eski sürümden (Tauri uygulaması) geliyorsan yeni uygulama ayrı kurulur (`docs/electron-migration.md`).
- Uygulamayı kaldırıp verini saklamak için: `curl -fsSL https://voicestudio.sh/install | sh -s -- --uninstall`.
- AMD ROCm desteği deneysel ve yalnız seçili RX 6000 kartlarında; RX 6500 XT kapsam dışı (`docs/install/windows-rocm.md`).
- Kurulum sorunları için README `docs/install/troubleshooting.md`, model indirme için `docs/downloading-models.md` belgesini gösteriyor.

## Bağlantılar

- Repo: https://github.com/debpalash/VoiceStudio
- Site: https://voicestudio.sh
- Belgeler: https://github.com/debpalash/VoiceStudio/blob/main/docs/README.md (kurulum sorunları: docs/install/troubleshooting.md, modeller: docs/downloading-models.md, yerel API: docs/speech-platform.md, MCP: docs/mcp.md)
- Sürümler: https://github.com/debpalash/VoiceStudio/releases (son sürüm v0.5.8, 12 Kasım 2026)
- Issues: https://github.com/debpalash/VoiceStudio/issues · Discord: https://discord.gg/bzQavDfVV9
- Lisans notu: https://github.com/debpalash/VoiceStudio/blob/main/LICENSE-NOTICE.md
- Video: Haftanın Popüler Repoları, 21 Kasım 2026 Short'u

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki trend Short t2026-11-21-1 videosu için hazırlandı (video henüz yayında değil). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
