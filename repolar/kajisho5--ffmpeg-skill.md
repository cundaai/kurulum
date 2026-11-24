---
repo: kajisho5/ffmpeg-skill
url: https://github.com/kajisho5/ffmpeg-skill
lisans: MIT
surum: v4.7.0 (2026-11-22)
readme_tarihi: 2026-11-22
dogrulama: denenmedi
bolum: 2026-W47
ozet: Coding agent'ına yerel FFmpeg ile video kurgulatan ve çıktıyı ölçüp denetleyen 42 araçlık skill ve MCP sunucusu.
---

# ffmpeg-skill — kurulum ve ilk örnek

> **Bu belge, README'nin 22 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

ffmpeg-skill, coding agent'ına bir video düzenleyicisi verir: Claude Code, Cursor, Codex ya da herhangi bir MCP istemcisi görüntünü bilgisayarındaki FFmpeg ile keser, altyazı ekler, dikey biçime çevirir, ses düzeyini ayarlar, platforma göre dışa aktarır ve sonra kendi çıktısını denetler. README'nin gerekçesi: FFmpeg "bilen" bir agent kare hızını, codec'i tahmin eder ve sonucu açmadan "bitti" der; bu skill'de her iş `probe.py` ile gerçek dosyanın ölçülmesiyle başlar, her işlem tipli argümanları olan bir betiktir ve sonuç hedef platformun kurallarına göre ölçülür. Bulut yok, API anahtarı yok, Python bağımlılığı yok (yalnız standart kütüphane). Lisansı MIT.

## Gereksinimler

- FFmpeg 6.1+ (daha eski sürüm reddedilir). Bazı araçlar `libx264`, `aac` ile `drawtext`, `subtitles` (libass), `loudnorm` gibi filtreleri ister; hangilerinin eksik olduğunu `doctor` söyler.
- Python 3.10+, yalnız standart kütüphane.
- Node 16+, yalnız `npx` kurucusu için.
- Skill çalıştıran bir agent: Claude Code, Cursor, Codex ya da Gemini CLI; shell'i olmayan istemciler için MCP sunucusu.
- İsteğe bağlı: konuşmadan altyazı (`caption.py --transcribe`) için ayrıca kurulan yerel bir konuşma tanıma motoru; README'ye göre en kolayı `pip install faster-whisper`.

## Kurulum

Tek komut (README: bulduğu her agent için skill'i kurar, `ffmpeg` çalışmıyorsa FFmpeg'i paket yöneticisiyle kurar ve `doctor` ile bitirir; Node ve Python'u kurmaz):

```bash
npx ffmpeg-skill setup
```

Aynı adımlar tek tek (README'den):

```bash
# 0. FFmpeg itself, once (Ubuntu/Debian shown; macOS and Windows are in the Install table below)
sudo apt install ffmpeg

# 1. install the skill for Claude Code (Cursor: --cursor, Codex: --codex, all three: --all)
npx ffmpeg-skill

# 2. check the machine: ffmpeg, ffprobe and every FFmpeg component the tools need
npx ffmpeg-skill doctor
```

FFmpeg'in kendisi: macOS'ta `brew install ffmpeg-full`, Ubuntu ve Debian'da `sudo apt install ffmpeg`, Windows'ta `winget install Gyan.FFmpeg`.

Hedefe göre kurulum seçenekleri:

```bash
npx ffmpeg-skill              # Claude Code   → ~/.claude/skills/ffmpeg-skill
npx ffmpeg-skill --cursor     # Cursor        → ~/.cursor/skills/ffmpeg-skill
npx ffmpeg-skill --codex      # Codex         → ~/.agents/skills/ffmpeg-skill (Cursor reads this location too)
npx ffmpeg-skill --all        # all three
npx ffmpeg-skill --project    # this project  → ./.claude/skills/ffmpeg-skill
```

Claude Code plugin'i olarak (Node gerekmez; Claude Code 2.1.275 ve sonrası):

```bash
/plugin install ffmpeg-skill --marketplace kajisho5/ffmpeg-skill      # in Claude Code 2.1.275 or later
```

Codex için `codex plugin marketplace add kajisho5/ffmpeg-skill --sparse .claude-plugin`, sonra `codex plugin add ffmpeg-skill@ffmpeg-skill`. Gemini CLI için `gemini extensions install https://github.com/kajisho5/ffmpeg-skill`.

MCP sunucusu (shell'i olmayan istemciler): `npx -y ffmpeg-skill mcp`. README'deki istemci ayarı örneği (yolu kendi kullanıcı klasörüne göre değiştir):

```json
{"mcpServers": {"ffmpeg-skill": {"command": "python3", "args": ["/Users/you/.claude/skills/ffmpeg-skill/mcp/server.py"]}}}
```

## İlk çalıştırma

1. Kurulumu denetle:

```bash
npx ffmpeg-skill doctor
```

2. Agent'ına ne istediğini yaz. README'nin örnek isteği:

> "Take `interview.mp4`, keep 0:45–1:20 and 5:00–5:25, and make it a 60-second Reel."

   README'ye göre agent sırayla `probe.py`, `cut.py --segments 0:45-1:20,5:00-5:25 --accurate`, `fit.py --aspect 9:16 --fit crop`, `export.py --preset reels`, `check.py --platform reels` ve `look.py` çalıştırır; sonra "final.mp4: 60.00 s, 1080×1920, 30 fps, AAC stereo" biçiminde ölçülmüş sonucu bildirir.

3. Araçlar agent olmadan da shell'den çalışır:

```bash
S=~/.claude/skills/ffmpeg-skill/scripts
python3 $S/probe.py input.mp4 --compact
python3 $S/fit.py input.mp4 --duration 60 --method trim --aspect 9:16 --dry-run    # plan a trim and reframe
python3 $S/export.py input.mp4 --preset reels --json                 # structured result with a probe of the output
```

Tek komutla bir platforma teslim (TikTok şablonu; `tiktok`, `reels`, `shorts`, `youtube`, `x`, `linkedin`, `facebook`, `podcast` ve `audiogram` şablonları var):

```bash
python3 $S/render.py talk.mp4 --template tiktok --cues cues.txt
```

README `cues.txt` dosyasının biçimini bu bölümde anlatmıyor; başka istek ve komut örnekleri `examples/README.md` dosyasında.

## Sık takılınan yerler

- macOS'ta düz `ffmpeg` formülünde subtitles, drawtext ve zscale filtreleri yok; README `ffmpeg-full` istiyor. `ffmpeg-full` keg-only olduğu için PATH'e kendiliğinden girmez: PATH'teki `ffmpeg` Homebrew'un düz formülüyse skill aynı Homebrew'daki `ffmpeg-full`'u kendisi kullanır; PATH'te hiç `ffmpeg` yoksa shell profiline `export PATH="$(brew --prefix ffmpeg-full)/bin:$PATH"` eklenir.
- `doctor`'ın genel `ok` değeri, tek bir aracın gerektirdiği bir filtre eksik olduğunda bile false olur; araçların çoğu yine çalışır. Araç başına yanıt için `doctor --json` çıktısındaki `tools` alanına bak.
- Hiçbir şey kendiliğinden güncellenmez: kurulu kopyayı yenilemek için `npx ffmpeg-skill@latest setup`, Claude Code plugin'i için `claude plugin update`.
- Windows'ta Git Bash içinde `python3` yalnız Python Microsoft Store'dan kurulduysa PATH'tedir; python.org kurulumunda ham betik örneklerinde `python3` yerine `python` yaz. MCP ayarındaki `"command"` için de aynısı geçerli.
- `--dry-run` yalnız çalışacak ffmpeg komutunu yazar, ffmpeg'i başlatmaz; filtre grafiği doğrulanmış olmaz.
- Konuşma tanıma motoru yoksa `--transcribe` kurulum satırını yazarak reddeder; geri kalan her şey çalışır.

## Bağlantılar

- Repo: https://github.com/kajisho5/ffmpeg-skill
- Site: https://kajisho5.github.io/ffmpeg-skill/ · npm: https://www.npmjs.com/package/ffmpeg-skill
- Örnek istekler ve arkasındaki komutlar: https://github.com/kajisho5/ffmpeg-skill/blob/main/examples/README.md
- Agent'ın okuduğu tanım: https://github.com/kajisho5/ffmpeg-skill/blob/main/SKILL.md · Bayrak başvurusu: https://github.com/kajisho5/ffmpeg-skill/blob/main/references/scripts.md
- Sürümler: https://github.com/kajisho5/ffmpeg-skill/releases (son sürüm v4.7.0, 22 Kasım 2026) · Değişiklikler: https://github.com/kajisho5/ffmpeg-skill/blob/main/CHANGELOG.md
- Issues: https://github.com/kajisho5/ffmpeg-skill/issues
- Video: Haftanın Popüler Repoları, 16–22 Kasım 2026 bölümü

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W47 bölümü](../bolumler/2026-W47.md) videosu için hazırlandı: https://www.youtube.com/watch?v=6jXWzYzxfKI. Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
