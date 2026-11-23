---
repo: elementalsouls/Claude-BugHunter
url: https://github.com/elementalsouls/Claude-BugHunter
lisans: MIT
surum: v2.1 (2026-06-05)
readme_tarihi: 2026-11-18
dogrulama: denenmedi
bolum: trend-short/t2026-11-22-1
ozet: Claude Code'a bug bounty ve yetkili red team çalışmaları için 83 skill ve 15 slash command ekleyen açık kaynak skill paketi.
---

# Claude-BugHunter — kurulum ve ilk örnek

> **Bu belge, README'nin 18 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

Claude-BugHunter, Claude Code'un skill sistemine eklenen hazır bir skill paketi. README'ye göre 83 skill ve 15 slash command içeriyor; kalıplar 24 temel zafiyet sınıfında, açıklanmış 681 rapordan derlenmiş. Dört katmanı var: yöntem ve düşünme biçimi, web uygulaması avı, kurumsal kimlik ve altyapı zincirleri, bulguyu raporlama. Burp MCP bağlantısı ve çalışma klasörü kuran `hunt` aracı da pakette. README, paketin yetkili red team ve bug bounty çalışmalarında ve herkese açık eğitim ortamlarında (DVWA, OWASP Juice Shop, Hacker101) denendiğini söylüyor. Yalnız iznin olan, kapsam içindeki hedeflerde kullan. Lisansı MIT.

## Gereksinimler

- Claude Code (skill'ler onun skill sistemine yüklenir). README'ye göre aynı skill'ler OpenCode, OpenAI Codex CLI, Hermes Agent ve Google AntiGravity'de de çalışır; slash command'lar ve `/hunt` yalnız Claude Code'da.
- Kopyalayarak kurulum (B yolu) için `git`; macOS ve Linux'ta `bash`, Windows'ta PowerShell (`pwsh`).
- `cbh` komut satırı aracını plugin yoluyla ayrıca kurmak istersen `pipx`.
- HackerOne hesabı isteğe bağlı: kurulum bir HackerOne API kullanıcı adı ve token sorar; README'ye göre bunlar yalnız özel programlarını ve kendi raporlarını okumak için, gerekmiyorsa ikisini de boş bırak.

## Kurulum

A yolu, Claude Code plugin'i olarak (README'nin önerdiği). Claude Code'un içinde:

```text
/plugin marketplace add elementalsouls/Claude-BugHunter
/plugin install claude-bughunter@elementalsouls
```

Skill'ler ve command'lar `claude-bughunter:` ad alanıyla yüklenir; `~/.claude/` altına dosya kopyalanmaz.

B yolu, kopyalayarak kurulum (plugin sistemi yoksa ya da belirli bir kopyaya sabitlemek için):

```bash
git clone https://github.com/elementalsouls/Claude-BugHunter.git
cd Claude-BugHunter
```

```bash
# macOS / Linux
bash scripts/install.sh

# Windows (PowerShell)
pwsh ./scripts/install.ps1
```

İkisi de skill'leri ve command'ları `~/.claude/` (macOS, Linux) ya da `%USERPROFILE%\.claude\` (Windows) altına kopyalar ve `hunt` aracını bağlar.

Hangi yol ne verir (README'deki tablo): skill'ler ve slash command'lar iki yolda da gelir. `cbh` aracı B yolunda kopyadan gelir, A yolunda ayrıca kurulur. `hunt` aracı yalnız B yolunda var.

Plugin kullanıp `cbh` aracını da isteyenler için README'deki komut:

```bash
pipx install git+https://github.com/elementalsouls/Claude-BugHunter
```

Skill'leri kurulu bütün agent araçlarına birden kopyalamak ve Burp MCP sunucusunu bağlamak için:

```bash
# macOS / Linux
bash scripts/install.sh --all --burp-mcp

# Windows (PowerShell)
pwsh ./scripts/install.ps1 -All -BurpMcp
```

## İlk çalıştırma

README'ye göre kurulumdan sonra Claude Code'u açıp neyi test ettiğini düz İngilizceyle yazman yeterli; doğru skill adını yazmadan kendiliğinden yüklenir. README'deki örnek (README bunun gerçek bir kayıt değil, gösterim amaçlı bir döküm olduğunu belirtiyor):

```text
> Testing acme.com — an in-scope HackerOne target. Run recon and rank the surface.
```

Örnekte Claude Code ilgili skill'leri yükler, alt alan adlarını ve canlı sunucuları çıkarır, yüzeyi sıralar ve nereden başlanacağını önerir. `acme.com` bir örnek addır; kendi çalışmanda yalnız programın kapsamına giren, test etme iznin olan hedefi yaz.

## Bağlantılar

- Repo: https://github.com/elementalsouls/Claude-BugHunter
- Proje sayfası: https://elementalsouls.github.io/Claude-BugHunter
- Kurulum kılavuzu: https://github.com/elementalsouls/Claude-BugHunter/blob/main/INSTALL.md
- Kullanım kılavuzu: https://github.com/elementalsouls/Claude-BugHunter/blob/main/USAGE.md
- Skill kataloğu: https://github.com/elementalsouls/Claude-BugHunter/blob/main/docs/skills.md
- `cbh` aracı: https://github.com/elementalsouls/Claude-BugHunter/blob/main/docs/cbh-cli.md
- Birden çok agent aracı: https://github.com/elementalsouls/Claude-BugHunter/blob/main/docs/multi-harness.md
- Issues: https://github.com/elementalsouls/Claude-BugHunter/issues

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki trend Short t2026-11-22-1 videosu için hazırlandı (yakında — 23 Kas 14:00). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
