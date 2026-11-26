---
repo: obra/superpowers
url: https://github.com/obra/superpowers
lisans: MIT
surum: v7.0.2 (2026-11-03)
readme_tarihi: 2026-10-20
dogrulama: denenmedi
bolum: 2026-W48-2
ozet: Coding agent'a önce tasarım, sonra plan, sonra testle geliştirme yaptıran, kendiliğinden devreye giren skill'lerden oluşan yazılım geliştirme yöntemi.
---

# superpowers — kurulum ve ilk örnek

> **Bu belge, README'nin 20 Ekim 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

Superpowers, coding agent'lar için eksiksiz bir yazılım geliştirme yöntemidir: birbirine eklenebilen skill'lerden ve agent'ın bunları kullanmasını sağlayan başlangıç talimatlarından oluşur. README'ye göre agent bir şey yaptırdığını görünce hemen kod yazmaya girişmez; önce ne istediğini sorar, tasarımı okunabilir parçalar hâlinde gösterir, onayından sonra ayrıntılı bir uygulama planı çıkarır (gerçek red/green TDD, YAGNI, DRY) ve "go" dediğinde işleri subagent'larla tek tek yapıp inceler. Skill'ler kendiliğinden tetiklenir; ayrıca bir şey yapman gerekmez. İçindeki skill'ler arasında `brainstorming`, `writing-plans`, `test-driven-development`, `systematic-debugging`, `using-git-worktrees` ve `requesting-code-review` var. Lisansı MIT.

## Gereksinimler

- Desteklenen bir coding agent (README "harness" diyor): Claude Code, Antigravity, Codex App, Codex CLI, Cursor, Devin CLI, Factory Droid, Gemini CLI, GitHub Copilot CLI, Grok Build CLI, Kimi Code, OpenCode, Pi, Qwen Code, Hermes Agent ya da Muse.
- Kurulum her harness için ayrıdır; birden fazlasını kullanıyorsan Superpowers'ı her birine ayrı kurarsın (README: "install Superpowers separately for each one").
- README işletim sistemi, dil sürümü ya da başka bir ön koşul yazmıyor.

## Kurulum

Claude Code, Anthropic'in resmî plugin marketplace'inden (komut Claude Code oturumunun içinde yazılır):

```bash
/plugin install superpowers@claude-plugins-official
```

Claude Code için ikinci yol, Superpowers'ın kendi marketplace'i (Superpowers'ı ve ilgili birkaç plugin'i sunar). Önce marketplace'i kaydet, sonra plugin'i kur:

```bash
/plugin marketplace add obra/superpowers-marketplace
```

```bash
/plugin install superpowers@superpowers-marketplace
```

Öbür harness'ler için README'deki komutlar:

- Antigravity: `agy plugin install https://github.com/obra/superpowers` (güncellemek için aynı komutla yeniden kur).
- Codex App: kenar çubuğunda Plugins'e tıkla, Coding bölümündeki `Superpowers`'ın yanındaki `+` düğmesine bas ve adımları izle.
- Codex CLI: `/plugins` ile plugin aramasını aç, `superpowers` diye ara, `Install Plugin`'i seç.
- Cursor: Cursor Agent sohbetinde `/add-plugin superpowers` ya da plugin marketplace'inde "superpowers" diye ara.
- Devin CLI: `devin plugins install obra/superpowers`; güncelleme `devin plugins update superpowers`.
- Factory Droid: `droid plugin marketplace add https://github.com/obra/superpowers`, sonra `droid plugin install superpowers@superpowers`.
- Gemini CLI: `gemini extensions install https://github.com/obra/superpowers`; güncelleme `gemini extensions update superpowers`.
- GitHub Copilot CLI: `copilot plugin marketplace add obra/superpowers-marketplace`, sonra `copilot plugin install superpowers@superpowers-marketplace`.
- Grok Build CLI: `grok plugin install superpowers@xai-official --trust` ya da TUI'de `/marketplace` ile arayıp kur.
- Kimi Code: `/plugins` ile plugin yöneticisini aç, `Marketplace` > `Superpowers`; ya da doğrudan `/plugins install https://github.com/obra/superpowers`. Ayrıntı: `docs/README.kimi.md`.
- OpenCode: kurulum bir komutla değil, OpenCode'a README'deki kurulum cümlesini yazarak yapılır (agent `.opencode/INSTALL.md` dosyasındaki adımları izler). Cümle ve ayrıntı README'nin OpenCode bölümünde ve `docs/README.opencode.md` belgesinde.
- Pi: `pi install git:github.com/obra/superpowers`.
- Qwen Code: `qwen extensions install obra/superpowers` (sorulduğunda `superpowers`'ı seç); güncelleme `qwen extensions update superpowers`.
- Hermes Agent: `hermes plugins install obra/superpowers --enable`; kurduktan sonra açık Hermes oturumlarını yeniden başlat.
- Muse: aşağıdaki üç satır; güncelleme `muse plugins update superpowers`. Kurduktan sonra açık Muse oturumlarını yeniden başlat.

```bash
git clone https://github.com/obra/superpowers.git
muse plugins install ./superpowers
muse plugins approve superpowers
```

Güncelleme için README yalnız şunu söylüyor: agent'a göre değişir, çoğu zaman kendiliğinden olur.

## İlk çalıştırma

Ayrı bir başlatma komutu yok: skill'ler kendiliğinden devreye girer. README'nin doğrulama örneği (Muse bölümünde verilmiş): yeni bir oturum aç ve şunu yaz:

```text
Let's make a react todo list
```

Kurulum çalışıyorsa agent kod yazmadan önce `brainstorming` skill'ini kendiliğinden başlatır. README bu doğrulamayı yalnız Muse için yazıyor; öbür harness'lerde ayrı bir doğrulama adımı anlatmıyor.

Sonraki akış README'nin "The Basic Workflow" bölümündeki sırayla ilerler:

1. `brainstorming`: bir şey yapmadan önce sorularla ne istediğini çıkarır; küçük değişiklikler sohbette, projeler yazılı tasarım belgesiyle kararlaştırılır.
2. `using-git-worktrees`: tasarım onayından sonra yeni dalda ayrı bir çalışma alanı açar, proje kurulumunu çalıştırır, testlerin temiz olduğunu doğrular.
3. `writing-plans`: işi 2–5 dakikalık görevlere böler; her görevde dosya yolları, kod ve doğrulama adımları bulunur.
4. `subagent-driven-development` ya da `executing-plans`: her görev için yeni bir subagent ve ardından inceleme (en titiz yol), ya da bütün görevler aynı oturumda ve sonda tek inceleme (en ucuz yol).
5. `test-driven-development`: önce başarısız test, sonra en az kod, sonra commit; testten önce yazılan kodu siler.
6. `requesting-code-review`: görevler arasında plana göre inceler; kritik sorunlar ilerlemeyi durdurur.
7. `finishing-a-development-branch`: testleri doğrular, merge / PR / tut / at seçeneklerini sunar, worktree'yi temizler.

## Sık takılınan yerler

- Bir oturum ters giderse (skill gereksiz yere tetiklenir, tetiklenmesi gerekirken susar, agent planı yok sayar, işi yineler ya da beklenenden çok token harcar) agent'a şunu yaz: `figure out what went wrong with superpowers in this session`. Agent `diagnosing-superpowers` skill'ini çalıştırır, oturum dökümünü okuyup kanıtıyla raporlar ve istersen hata bildirimi için arındırılmış bir paket hazırlar. Eski bir oturum için adını ver: `figure out what went wrong with superpowers in session <id>`.
- Hermes'te compaction sonrası hook yok: çok uzun bir oturumda ilk tur sıkıştırılırsa başlangıç talimatı kaybolur; skill'ler tetiklenmeyi bırakırsa yeni oturum aç.
- Muse'ta skill'ler hemen etkin olur ama hook'lar ilk kurulumda onay ister (`muse plugins approve superpowers`) ve `SessionStart` hook'u için oturumun yeniden başlatılması gerekir.
- OpenCode kendi plugin kurulumunu kullanır: Superpowers başka bir harness'te kurulu olsa bile OpenCode'a ayrıca kurulur.
- Telemetri: `brainstorming`'in isteğe bağlı görsel yardımcısı Prime Radiant logosunu varsayılan olarak onların sitesinden yükler ve kullanılan Superpowers sürümünü iletir (README'ye göre proje, prompt ya da agent bilgisi iletmez). Kapatmak için `SUPERPOWERS_DISABLE_TELEMETRY` ortam değişkenine doğru bir değer ver; Claude Code'un `DISABLE_TELEMETRY` ve `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC` ayarları da geçerlidir.

## Bağlantılar

- Repo: https://github.com/obra/superpowers
- Claude plugin sayfası: https://claude.com/plugins/superpowers
- Superpowers marketplace: https://github.com/obra/superpowers-marketplace
- Belgeler: https://github.com/obra/superpowers/blob/main/docs/README.opencode.md · https://github.com/obra/superpowers/blob/main/docs/README.kimi.md
- Sürümler: https://github.com/obra/superpowers/releases (son sürüm v7.0.2, 3 Kasım 2026) · sürüm duyuruları: https://primeradiant.com/superpowers/
- Issues: https://github.com/obra/superpowers/issues · Discord: https://discord.gg/35wsABTejz
- İlk duyuru yazısı: https://blog.fsck.com/2025/10/09/superpowers/
- Video: Haftanın Popüler Repoları, 23–25 Kasım 2026 bölümü (listenin 3. sırası)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W48-2 bölümü](../bolumler/2026-W48-2.md) videosu için hazırlandı: https://www.youtube.com/watch?v=2wDhRoLOWh8. Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
