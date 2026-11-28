---
repo: rtk-ai/rtk
url: https://github.com/rtk-ai/rtk
lisans: Apache-2.0
surum: v1.2.1 (2026-11-19)
readme_tarihi: 2026-11-22
dogrulama: denenmedi
bolum: 2026-W48-3
ozet: Coding agent'ların okuduğu komut çıktısını (test, diff, log, build) sıkıştırıp token harcamasını düşüren, tek Rust dosyası olan komut satırı aracı.
---

# rtk — kurulum ve ilk örnek

> **Bu belge, README'nin 22 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

RTK, coding agent'ın çalıştırdığı komutların çıktısını agent okumadan önce sıkıştırır. README'ye göre agent `git status` çalıştırdığında RTK araya girip `rtk git status` çalıştırır ve agent 40 satır yerine 3 satırlık bir özet okur; komut çıktısında %90'a varan azalma, 100'den fazla komut, komut başına 25 ms'nin altında. Testlerde yalnız başarısız olanlar ve assertion'ları, `git diff`'te daraltılmış bağlam, lint çıktısında kurala ve dosyaya göre gruplama gelir. Ne kadar kazandığını `rtk gain` gösterir. README'nin kendi uyarısı: komut çıktısındaki %90'lık düşüş faturada %90'lık düşüş demek değildir, çünkü komut çıktısı input token'larının yalnız bir parçasıdır. Daha ileri gitmek isteyenler için "pipeline" dosyaları (büyük dosya okumalarını daha ucuz bir modele vermek gibi) ve isteğe bağlı `rtk-proxy` var. Claude Code, GitHub Copilot, Cursor, Gemini CLI, Codex, OpenCode, Windsurf dahil 18 aracı destekler.

## Gereksinimler

- macOS, Linux ya da Windows (WSL veya Git Bash ile de olur). Kurulum için Homebrew, winget ya da `curl` yeter; README'ye göre başka bağımlılık yok (tek Rust dosyası).
- Kurulu bir coding agent (örneğin Claude Code). RTK, `rtk init` ile agent'ın hook'una, plugin'ine ya da rules dosyasına bağlanır.
- Ücretli hesap gerekmez. "Hand big file reads to a cheaper model" pipeline'ı Claude Code'da kendi API anahtarınla çalışır (`<API_ANAHTARI>`); temel sıkıştırma anahtar istemez.

## Kurulum

macOS, ve Homebrew olan Linux:

```bash
brew install rtk
```

Windows, winget ile:

```bash
winget install rtk-ai.rtk
```

Linux, macOS, WSL ya da Windows'ta Git Bash, kurulum betiğiyle:

```bash
curl -fsSL https://raw.githubusercontent.com/rtk-ai/rtk/refs/heads/master/install.sh | sh
```

Sonra agent'ına bağla (bu satır Claude Code için; öbür agent'lar aşağıda):

```bash
rtk init -g                      # Claude Code; every other agent: see "Works in your agent"
```

README'deki "Works in your agent" tablosundan bazı örnekler: GitHub Copilot `rtk init -g --copilot`, Cursor `rtk init -g --agent cursor`, Gemini CLI `rtk init -g --gemini`, Codex `rtk init -g --codex`, OpenCode `rtk init -g --opencode`, Windsurf `rtk init -g --agent windsurf`. Cargo, hazır dosyalar, yerel Windows, WSL ve kaldırma adımları README'de değil, sitedeki Installation guide'da.

Yalnız Apache-2.0 lisanslı sürümü isteyenler için (pipeline motoru olmadan) README'nin "Editions and licence" bölümü:

```bash
brew install rtk-ai/tap/rtk-oss
```

## İlk çalıştırma

1. `rtk init -g` sonrası agent'ını yeniden başlat. README: "Restart your agent. That's it" — öğrenecek ya da çağıracak bir şey yok, agent'ın komutları kendiliğinden RTK'den geçer.
2. Agent'a birkaç iş yaptır (test çalıştırmak, `git status`, `git diff` gibi), sonra kazancı gör:

```bash
rtk gain                        # Summary stats
rtk gain --graph                # ASCII graph (last 30 days)
rtk gain --history              # Recent command history
rtk discover                    # Find missed savings opportunities
```

3. Bir komutun kırpılmamış çıktısını görmek istersen `rtk bypass` ile çalıştır (yine `rtk gain`'de sayılır):

```bash
rtk bypass git log --oneline -20        # full git log, no truncation
```

Agent'a göre kurulum ayrıntıları ve `--dry-run` için sitedeki Quick Start sayfasına bak.

## Sık takılınan yerler

- crates.io'da "rtk" adlı başka bir proje var. Doğrusunu kurduğunu anlamak için `rtk gain` tasarruf panosunu göstermeli, "command not found" dememeli (README).
- Hook yalnız shell komutlarını yeniden yazar. Claude Code'un `Read`, `Grep`, `Glob` gibi kendi araçları varsayılan olarak sıkıştırılmaz; bunlar için pipeline gerekir (README).
- Prompt cache bozulur mu? README'ye göre hayır: çıktı konuşmaya girmeden önce bir kez süzülür.
- Bir hafta veri biriktikten sonra `rtk gain` çıktısının sonunda günde en çok bir kez RTK Pro satırı çıkar; `config.toml` içinde `[display]` altında `pro_hint = false` bunu kapatır (README).
- Lisans iki katmanlı: `rtk` komutu Apache-2.0, ama varsayılan indirmedeki pipeline motoru (`engine/`) ve ayrı `rtk-proxy` BUSL-1.1. README'ye göre üretimde kullanmak ücretsiz; RTK'yi ya da türevini başkalarına ticari ürün/hizmet olarak sunmak bu izne girmez. İki `rtk` sürümü aynı adlı dosyayı kurar, birlikte kurulamaz; birini kaldırıp öbürünü kur.
- Telemetri varsayılan olarak kapalı; açık rıza vermeden (`rtk telemetry enable`) hiçbir şey gönderilmez, komut ve dosya içeriği hiç toplanmaz (README).
- Pipeline'lar komutları yeniden yazabilir ve içerik gönderebilir; README, paylaşılan bir pipeline'ı kurmadan önce dosyasını okumanı söylüyor.

## Bağlantılar

- Repo: https://github.com/rtk-ai/rtk
- Site ve belgeler: https://www.rtk-ai.app · https://www.rtk-ai.app/docs/
- Kurulum: https://www.rtk-ai.app/docs/getting-started/installation/ · Quick Start: https://www.rtk-ai.app/docs/getting-started/quick-start/
- Desteklenen agent'lar: https://www.rtk-ai.app/docs/getting-started/supported-agents/
- Pipeline'lar: https://www.rtk-ai.app/docs/pipelines/overview/ · Sorun giderme: https://www.rtk-ai.app/docs/resources/troubleshooting/
- Sürümler ve lisans: https://github.com/rtk-ai/rtk/releases (son sürüm v1.2.1, 19 Kasım 2026) · https://www.rtk-ai.app/docs/resources/editions/
- Issues: https://github.com/rtk-ai/rtk/issues · Discord: https://discord.gg/RySmvNF5kF
- Video: Haftanın Popüler Repoları, 25–27 Kasım 2026 Cuma bölümü (listenin 6. sırası)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W48-3 bölümü](../bolumler/2026-W48-3.md) videosu için hazırlandı: https://www.youtube.com/watch?v=AYiNLSGngiY. Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
