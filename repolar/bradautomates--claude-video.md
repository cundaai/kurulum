---
repo: bradautomates/claude-video
url: https://github.com/bradautomates/claude-video
lisans: MIT
surum: v0.3.2 (2026-09-25)
readme_tarihi: 2026-09-25
dogrulama: denenmedi
bolum: gunun-reposu/g05
ozet: Claude Code ve Codex'e video izletir: URL ya da dosyadan zaman damgalı kareler ve transkript çıkaran /watch skill'i.
---

# claude-video — kurulum ve ilk örnek

> **Bu belge, README'nin 25 Eylül 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

claude-video, kodlama ajanına "video kanıtı" veren **Watch** (`/watch`) skill'ini içerir: bir URL ya da yerel dosya, zaman damgalı karelere ve transkripte dönüşüp ajana verilir. Önce videonun kendi altyazıları okunur; altyazı yoksa yerel WhisperX ya da Groq/OpenAI ile konuşma yazıya dökülür; isteğe bağlı Gemini motoru videonun tamamını görüntü ve sesiyle Google'a gönderip zaman damgalı cevap alır. Claude Code (Claude Desktop'taki Code oturumu, VS Code eklentisi ya da terminal), Codex ve diğer Agent Skills host'larında çalışır; Claude Chat ve Cowork'te çalışmaz. Bir YouTube anlatımından ya da ekran kaydından bilgi çıkarmak isteyen geliştiriciler için.

## Gereksinimler

- Ajan: Claude Code, Codex (masaüstü, CLI ya da IDE eklentisi) ya da Cursor/Copilot gibi bir [Agent Skills](https://agentskills.io) host'u.
- Python 3.10+, FFmpeg/ffprobe ve güncel yt-dlp; YouTube için ayrıca bir JavaScript çalışma zamanı (Deno). Skill bu programları kurmaz, yalnız ajana kontrol betiği ve yönerge verir.
- Anahtar zorunlu değil: yerel yol anahtarsız çalışır. En hızlı yol için ücretsiz [Google AI Studio](https://aistudio.google.com/apikey) anahtarı (`GEMINI_API_KEY`); konuşma transkripti için anahtarsız WhisperX (3 GB disk, 8 GB RAM; yalnız Apple Silicon macOS'ta doğrulanmış) ya da `GROQ_API_KEY` / `OPENAI_API_KEY`.
- Node.js yalnız `npx skills` ile kurulum yolunda gerekir.

## Kurulum

Tek bir kurulum yöntemi seç. README'nin ana yolları:

### Claude Code (terminal)

`claude` ile oturumu aç ve oturumun **içinde** sırayla gir:

```text
/plugin marketplace add bradautomates/claude-video
/plugin install watch@claude-video
```

Sıradan kabuk isteminden kuracaksan:

```bash
claude plugin marketplace add bradautomates/claude-video
claude plugin install watch@claude-video
```

Sonra yeni bir Claude Code oturumu aç; `/watch` yazıp otomatik tamamlamadan **`/watch:watch`** seç.

### Claude Desktop — Code

**Customize → Plugins → Add → Add marketplace → Add from a repository**; URL alanına yapıştır ve **Sync**:

```text
https://github.com/bradautomates/claude-video
```

Eklenen marketplace'te **Watch**'ı bul ve kur (marketplace'i eklemek tek başına kurmaz). Sonra bir **Code** oturumu başlat.

### Claude Code (VS Code)

Mesaj kutusuna **`/plugins`** yaz → **Marketplaces**'e `bradautomates/claude-video` ekle → **Plugins**'te **watch** → **Install for you**.

### Codex

Codex'in mesaj kutusuna yapıştır:

```text
Use $skill-installer to install the watch skill from:
https://github.com/bradautomates/claude-video/tree/main/skills/watch
```

Codex CLI kuruluysa terminalden de olur:

```bash
codex plugin marketplace add bradautomates/claude-video
codex plugin add watch@claude-video
```

### Diğer ajanlar (Cursor, Copilot…)

```bash
npx skills add bradautomates/claude-video -g --skill watch
```

### Medya araçları

README'nin tablosu: macOS'ta [Homebrew](https://brew.sh) ile `brew install python ffmpeg yt-dlp`; Ubuntu/Debian'da `sudo apt install python3 ffmpeg pipx`, sonra `pipx install "yt-dlp[default,curl-cffi]"` ve `pipx ensurepath`, YouTube için [Deno](https://docs.deno.com/runtime/getting_started/installation/); Windows'ta [Python 3.10+](https://www.python.org/downloads/windows/), sonra `winget install --id Gyan.FFmpeg --exact`, `winget install --id yt-dlp.yt-dlp --exact`, `winget install --id DenoLand.Deno --exact`. Kurulumdan sonra terminali ve ajanı yeniden aç.

## İlk çalıştırma

Ajanın eriştiği bir klasöre kısa bir video koy (ör. `example.mp4`), o klasörü proje olarak aç ve mesaj kutusuna README'deki anahtarsız örneği yapıştır:

```text
Use the watch skill on example.mp4 in the folder I shared.
For setup, choose balanced detail and captions only (none).
Summarize what is visible, with timestamps.
```

Beklenen sonuç: zaman damgalı bir görsel özet; transkript anahtarı ya da yerel konuşma modeli gerekmez. Ajan eksik program bildirirse önce bağımlılık kontrolünü istet:

```text
Use the watch skill's bundled setup.py to check dependencies in this session.
Tell me which tools are missing and help me install them here.
```

Sonraki adım herkese açık bir video URL'si verip **transcript detail** istemek. Odaklı aralıklar daha isabetli; Claude Code'da `/watch:watch` seçip README'deki kısaltmalarla:

```text
/watch video.mp4 --start 2:15 --end 2:45
/watch video.mp4 --detail efficient --max-frames 30
```

Gemini motoru için `~/.config/watch/.env` dosyasına `GEMINI_API_KEY=<API_ANAHTARI>` satırını kendin yaz (sohbete yapıştırma, commit'leme); anahtar varsa `auto` modu Gemini'yi seçer, `--engine local` yerel yola zorlar.

## Sık takılınan yerler

- **Watch görünmüyor ya da `/plugin` tanınmıyor.** Marketplace eklenmiş ama eklenti kurulmamış olabilir; kurulumu kontrol et, yeni oturum aç, skill'i adıyla iste. İki watch görünüyorsa tek yöntemde kal. VS Code eklentisinde komut `/plugins`, Desktop'ta Customize; Claude Code on the web'de etkileşimli `/plugin` yok ([README, Updating and troubleshooting](https://github.com/bradautomates/claude-video#updating-and-troubleshooting)).
- **403 ya da giriş istiyor.** Önce yt-dlp'yi en son sürüme güncelle (`brew upgrade yt-dlp`, `pipx upgrade yt-dlp`, `winget upgrade --id yt-dlp.yt-dlp --exact`); Watch yalnız en son yt-dlp'yi destekler ([README](https://github.com/bradautomates/claude-video#updating-and-troubleshooting)).
- **Hiç kare çıkmıyor: `Unrecognized option 'vsync'`.** Yeni FFmpeg `-vsync`'i kaldırdı; v0.2 sürümlerinde iki kare yolu da bu yüzden çöküyordu. README v0.3.2'de `-fps_mode` denendiğini söylüyor; hâlâ görüyorsan FFmpeg yolunu kontrol et ([#99](https://github.com/bradautomates/claude-video/issues/99)).
- **Windows'ta her seferinde `.env` izin uyarısı.** `chmod 600` önerisi NTFS'te karşılıksız; uyarı zararsız ama kapatılamıyor ([#107](https://github.com/bradautomates/claude-video/issues/107)).
- **Dublajlı YouTube videosunda yanlış dilde altyazı.** Birden çok `-orig` parçası olan videolarda ilk parça (ör. `bn-orig`) seçiliyor; `--sub-lang en-orig` ile doğru parçayı iste ([#247](https://github.com/bradautomates/claude-video/issues/247)).

## Bağlantılar

- Repo: https://github.com/bradautomates/claude-video
- Belgeler: README ve [AGENTS.md](https://github.com/bradautomates/claude-video/blob/main/AGENTS.md) (ayrı belge sitesi yok)
- Sürümler: https://github.com/bradautomates/claude-video/releases
- Lisans: MIT — https://github.com/bradautomates/claude-video/blob/main/LICENSE

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [Günün Reposu g05](../bolumler/gunun-reposu-g05.md) videosu için hazırlandı (yakında — 15 Kas 12:30). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
