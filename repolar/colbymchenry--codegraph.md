---
repo: colbymchenry/codegraph
url: https://github.com/colbymchenry/codegraph
lisans: MIT
surum: v1.6.4 (2026-11-20)
readme_tarihi: 2026-11-20
dogrulama: denenmedi
bolum: 2026-W48-3
ozet: Kod tabanının sembol, çağrı ve bağımlılık grafiğini önceden çıkarıp MCP üzerinden coding agent'lara tek çağrıda veren, tamamen yerel çalışan araç.
---

# codegraph — kurulum ve ilk örnek

> **Bu belge, README'nin 20 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

CodeGraph, projendeki her sembolü, çağrıyı ve bağımlılığı önceden bir bilgi grafiğine (yerel SQLite veritabanı, `.codegraph/codegraph.db`) çıkarır ve bunu MCP sunucusu olarak agent'ına verir. Agent dosyaları tek tek grep'leyip okumak yerine tek soruyla ilgili kaynak kodu, semboller arası çağrı yollarını ve bir değişikliğin etki alanını alır. README'deki 2026-08 ölçümüne göre yedi benchmark reposunda ortalama %44 daha düşük maliyet ve %62 daha az token. Ayrıştırma Rust çekirdeğinde ve tree-sitter ile yapılır; 20'den fazla dil (TypeScript, Python, Go, Rust, Java, Swift, Kotlin, PHP…) desteklenir. Dosya izleyici sayesinde grafik her dosya değişikliğinde kendiliğinden güncellenir. README: "100% Local" — veri makineden çıkmaz, API anahtarı yok. Claude Code, Cursor, Codex CLI, opencode, Gemini CLI, Kiro, GitHub Copilot, Windsurf dahil 18 agent'ı tanır. Lisansı MIT.

## Gereksinimler

- Windows, macOS ya da Linux; x64 ve arm64. README'ye göre Node.js gerekmez: her sürüm kendi Node runtime'ını içeren hazır dosyayla gelir. Node varsa npm ile de kurulabilir.
- Kurulu bir coding agent (Claude Code, Cursor, Codex CLI vb.).
- Ücretli hesap ya da API anahtarı gerekmez.

## Kurulum

1. Komut satırı aracını kur:

```bash
# macOS / Linux
curl -fsSL https://raw.githubusercontent.com/colbymchenry/codegraph/main/install.sh | sh

# Windows (PowerShell)
irm https://raw.githubusercontent.com/colbymchenry/codegraph/main/install.ps1 | iex
```

   Node zaten kuruluysa npm ile:

```bash
npm i -g @colbymchenry/codegraph
```

   Kurulum `codegraph` komutunu PATH'e koyar ama açık terminali değiştirmez; sonraki adım için yeni bir terminal aç (README).

2. Yeni terminalde agent'larını bağla:

```bash
codegraph install
```

   Kurulu agent'ları bulur, hangilerini ayarlayacağını sorar, her birine CodeGraph MCP sunucusunu yazar. README'ye göre agent'ın talimat dosyasına (`CLAUDE.md` / `AGENTS.md` / `GEMINI.md` / `QWEN.md`) işaretli küçük bir CodeGraph bölümü de ekler ve Claude Code ya da Qwen Code hedefse otomatik izin listesi kurar (`--no-permissions` bunu atlar). Kısayol: `npx @colbymchenry/codegraph` indirip bu adımı tek seferde çalıştırır. Bu adım kod indekslemez.

   Soru sormadan (betik ya da CI için) README'deki örnekler:

```bash
codegraph install --yes                              # auto-detect agents, install global
codegraph install --target=cursor,claude --yes       # explicit target list
codegraph install --print-config codex               # print snippet, no file writes
```

## İlk çalıştırma

1. Agent'ını yeniden başlat ki MCP sunucusu yüklensin.
2. Her projede bir kez grafiği kur:

```bash
cd your-project
codegraph init
```

   `codegraph init` yerel `.codegraph/` klasörünü açar ve tam grafiği aynı adımda çıkarır. Sonrası kendiliğinden: README "No more syncing!" diyor, dosya izleyici her değişiklikte grafiği günceller. `.codegraph/` klasörü olan projede agent CodeGraph araçlarını kendisi kullanır.
3. Agent'a yapısal bir soru sor ("bir istek veritabanına nasıl ulaşıyor?" gibi); agent `codegraph_explore` aracıyla tek çağrıda cevaplar. Aynı şeyi terminalden de deneyebilirsin:

```bash
codegraph explore <query>         # Relevant symbols' source + call paths in one shot (same output as the codegraph_explore MCP tool)
codegraph status [path]           # Show statistics (--json, --strict)
codegraph impact <symbol>         # Analyze what code is affected by changing a symbol (--depth, --json)
```

Kaldırmak için `codegraph uninstall` (agent ayarlarını ve komutu siler; yalnız ayarlar için `--keep-cli`); bir projenin indeksini silmek için `codegraph uninit`.

## Sık takılınan yerler

- "CodeGraph not initialized": proje klasöründe önce `codegraph init` çalıştır (README Troubleshooting).
- Güncellemeden sonra cevaplar eski gibi görünüyorsa her projede `codegraph index` ile yeniden indeksle; `codegraph status` indeksin eski sürümle kurulduğunu söyler (README).
- MCP sunucusu bağlanmıyorsa: sunucuyu agent kendisi başlatır, elle başlatma. `codegraph status` ile projenin indekslendiğini kontrol et; olmazsa `codegraph install`'ı yeniden çalıştırıp ayarı yeniden yazdır (README).
- WSL2'de proje `/mnt/c` gibi bir Windows sürücüsündeyse `database is locked` ya da `Transport closed` görülebilir; projeyi Linux dosya sistemine (`~/` altına) taşı ya da MCP ortamında `CODEGRAPH_NO_DAEMON=1` ver (README).
- Uzun oturumlarda dikkat: README'ye göre CodeGraph toplam işlenen token'ı düşürür ama oturum sonunda context penceresinde kalan bilgi yaklaşık %80 daha fazladır (VS Code reposunda 67k'ya 18k token). Küçük context penceresiyle uzun oturum açıyorsan bunu hesaba kat.
- Telemetri: anonim kullanım istatistiği toplar (hangi komut ve diller; kod, yol, sembol adı ya da IP yok). Kurulum baştan sorar; kapatmak için `codegraph telemetry off` ya da `CODEGRAPH_TELEMETRY=0` / `DO_NOT_TRACK=1` (README).
- CodeGraph `/`, `/etc`, `~/.ssh`, `~/.aws`, `~/.config` gibi sistem klasörlerini proje olarak kabul etmez; agent'ı projenin kendi klasöründe başlat (README).

## Bağlantılar

- Repo: https://github.com/colbymchenry/codegraph
- Belgeler ve site: https://colbymchenry.github.io/codegraph/
- npm paketi: https://www.npmjs.com/package/@colbymchenry/codegraph
- Telemetri ayrıntısı: https://github.com/colbymchenry/codegraph/blob/main/TELEMETRY.md
- Sürümler: https://github.com/colbymchenry/codegraph/releases (son sürüm v1.6.4, 20 Kasım 2026)
- Issues: https://github.com/colbymchenry/codegraph/issues
- Video: Haftanın Popüler Repoları, 25–27 Kasım 2026 Cuma bölümü (listenin 7. sırası)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W48-3 bölümü](../bolumler/2026-W48-3.md) videosu için hazırlandı (video henüz yayında değil). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
