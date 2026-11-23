---
repo: mvanhorn/last30days-skill
url: https://github.com/mvanhorn/last30days-skill
lisans: MIT
surum: v4.6.0 (2026-11-22)
readme_tarihi: 2026-11-22
dogrulama: denenmedi
bolum: 2026-W47
ozet: Bir konuyu son 30 günde Reddit, X, YouTube, Hacker News, Polymarket ve web'de arayıp tek özet çıkaran agent skill'i.
---

# last30days-skill — kurulum ve ilk örnek

> **Bu belge, README'nin 22 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

`/last30days`, verdiğin konuyu son 30 gün içinde Reddit, X, YouTube, TikTok, Hacker News, Polymarket, GitHub ve web'de aynı anda arar; sonuçları editör seçimine göre değil, insanların gerçekten ilgi gösterdiği şeye (upvote, beğeni, izlenme, gerçek parayla oluşan oranlar) göre puanlar ve bir agent bunları tek bir özete çevirir. README'nin örnekleri: toplantıdan önce bir kişiyi ya da şirketi tanımak, araçları karşılaştırmak, yükselen konuları bulmak. Konu bir şirketse patent başvurularına ve kariyer sayfasına da bakar. README'ye göre varsayılan kullanımda her şey kendi bilgisayarında çalışır; veri yalnız çağırdığın arama API'lerine gider. Lisansı MIT.

## Gereksinimler

- Skill çalıştırabilen bir agent: Claude Code (önerilen), Grok Build CLI, Codex, Cursor, Copilot, Gemini CLI ya da Agent Skills destekleyen başka bir araç; ayrıca claude.ai (web), Claude Desktop ve OpenClaw.
- `npx skills` yolu için Node.js (`npx`).
- Claude Desktop paketi için PATH'te Python 3.12+ (README: paket motorun kaynağını taşır ama senin Python'unu kullanır). README "Built with Python 3.12+, yt-dlp, Node.js" diyor.
- Anahtar gerekmeyen kaynaklar: Reddit (yorumlarla), Hacker News, Polymarket, GitHub, StockTwits, şirket patentleri ve kariyer sayfaları. arXiv, Techmeme ve Yahoo Finance için ücretsiz CLI'ları ilk çalıştırmadaki kurulum yükler.
- İsteğe bağlı kaynaklar: YouTube için `brew install yt-dlp`; X için `X_BEARER_TOKEN` ya da tarayıcıda x.com oturumu; TikTok, Instagram, Threads, Pinterest, LinkedIn için ScrapeCreators anahtarı; web araması için Brave Search anahtarı. Tam liste README'nin "Bring your own keys" tablosunda.

## Kurulum

Claude Code (README'nin önerdiği yol; marketplace güncellemeyi kendisi yapar):

```
/plugin marketplace add mvanhorn/last30days-skill
/plugin install last30days
```

Codex, Cursor, Copilot, Gemini CLI ve öbür Agent Skills araçları:

```bash
npx skills add mvanhorn/last30days-skill -g
```

`-g` skill'i kullanıcı klasörüne kurar, bütün projelerde kullanılır; `-g` olmadan yalnız o projeye kurulur. Belirli bir aracı hedeflemek için:

```bash
npx skills add mvanhorn/last30days-skill -g -a codex
npx skills add mvanhorn/last30days-skill -g -a cursor
npx skills add mvanhorn/last30days-skill -g -a gemini-cli
npx skills add mvanhorn/last30days-skill -g -a codex -a cursor
```

Grok (xAI Build CLI):

```bash
grok plugin marketplace add mvanhorn/last30days-skill
grok plugin install last30days
```

OpenClaw:

```bash
clawhub install last30days-official
```

claude.ai (web): son sürümden `last30days.skill` dosyasını indir, claude.ai > Customize > Skills > `+` > `Create skill` > `Upload a skill` ile yükle. Önce Capabilities altında "Code execution and file creation" açık olmalı.

Claude Desktop: son sürümden platformuna uygun `.mcpb` dosyasını indir (örneğin macOS Apple Silicon için `last30days-pp-mcp-darwin-arm64.mcpb`), Settings > Extensions'a sürükle, istenen anahtarları gir (hepsi isteğe bağlı) ve Claude Desktop'ı yeniden başlat. Burada skill bir MCP sunucusu olarak kurulur.

Elle (geliştirici) kurulum:

```bash
git clone https://github.com/mvanhorn/last30days-skill.git
ln -s "$(pwd)/last30days-skill/skills/last30days" ~/.claude/skills/last30days
```

Güncelleme: Claude Code'da `claude plugin update last30days@last30days-skill`, `npx skills` yolunda `npx skills update last30days -g`.

## İlk çalıştırma

1. Agent'ında komutu bir konuyla çalıştır (README'nin örneği):

```
/last30days Peter Steinberger
```

2. README'ye göre ayar gerekmez: Reddit, Hacker News, Polymarket ve GitHub hemen çalışır. İlk çalıştırmada açılan kurulum sihirbazı X, YouTube, TikTok, arXiv, Techmeme gibi kaynakları yaklaşık 30 saniyede açar.
3. Başka kullanım biçimleri (README'den): karşılaştırma için `/last30days OpenClaw vs Hermes vs Paperclip`, yükselen konuları bulmak için `/last30days what's exploding in AI agents?`.

Araştırma dosyaları varsayılan olarak `~/Documents/Last30Days/` klasörüne yazılır (`LAST30DAYS_MEMORY_DIR` ile ya da çalıştırma başına `--save-dir <path>` ile değişir). Bir çalıştırmanın neyi okuyup yazacağını araştırma başlatmadan görmek için kaynak klasöründe:

```bash
python3 skills/last30days/scripts/last30days.py --preflight
```

README bir çalıştırmanın ne kadar sürdüğünü ve özetin tam çıktısını örnekle göstermiyor.

## Sık takılınan yerler

- Claude Code'da marketplace plugin'i ile `npx skills` kopyası birlikte etkinse `/last30days` iki kez görünür; README makine başına tek kurulum yolu öneriyor.
- claude.ai'de "Code execution and file creation" kapalıysa skill çalışmaz.
- Claude Desktop ile Claude Code anahtarları ayrı saklar: `~/.config/last30days/.env` dosyasını ayarladıysan aynı anahtarları Claude Desktop'ta bir kez daha girersin. Claude Desktop paketinde Windows desteği ertelenmiş.
- X için resmî API yaklaşık son bir haftanın gönderilerini verir (X geliştirici projende tam arşiv erişimi yoksa).
- Bir akıl yürütme sağlayıcısının anahtarını (`OPENAI_API_KEY`, `XAI_API_KEY`, `OPENROUTER_API_KEY`, `GOOGLE_API_KEY` gibi) ayarlarsan konu ve bulunan içerik parçaları puanlama için o sağlayıcıya gönderilir; anahtar yoksa yerel puanlama kullanılır.
- README güncel kaynak ağacını anlatır; kurulu sürümün farklı olabilir. Komut ve kurulum davranışı için esas kaynak `skills/last30days/SKILL.md`.

## Bağlantılar

- Repo: https://github.com/mvanhorn/last30days-skill
- Türkçe README: https://github.com/mvanhorn/last30days-skill/blob/main/README.tr.md
- Ayarlar ve kaynak başına anahtar tablosu: https://github.com/mvanhorn/last30days-skill/blob/main/CONFIGURATION.md
- Skill tanımı: https://github.com/mvanhorn/last30days-skill/blob/main/skills/last30days/SKILL.md
- Sürümler: https://github.com/mvanhorn/last30days-skill/releases (son sürüm v4.6.0, 22 Kasım 2026)
- Issues: https://github.com/mvanhorn/last30days-skill/issues · Agent Skills: https://agentskills.io
- Video: Haftanın Popüler Repoları, 16–22 Kasım 2026 bölümü

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W47 bölümü](../bolumler/2026-W47.md) videosu için hazırlandı (yakında — 23 Kas 12:30). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
