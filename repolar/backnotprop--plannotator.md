---
repo: backnotprop/plannotator
url: https://github.com/backnotprop/plannotator
lisans: Apache-2.0
surum: v0.28.66 (2026-11-16)
readme_tarihi: 2026-11-16
dogrulama: denenmedi
bolum: 2026-W46
ozet: Coding agent'ların planlarını, dosyalarını ve kod değişikliklerini tarayıcıda işaretleyip yorumlarını tek tıkla agent'a geri gönderen yerel inceleme aracı.
---

# plannotator — kurulum ve ilk örnek

> **Bu belge, README'nin 16 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

Plannotator, coding agent'lar için bilgisayarında çalışan, tarayıcıda açılan bir inceleme ekranıdır. Agent bir plan, karar ya da HTML sayfası önerdiğinde veya işini bitirdiğinde ekran tarayıcıda açılır; metni işaretler, yorum yazar ve geri bildirimi doğrudan agent'a gönderirsin. Yerel değişiklikleri ya da uzak PR'ları diff üzerinde yorumlayarak inceleyebilirsin (Git, GitButler, Jujutsu, Perforce, GitHub, GitLab). README'ye göre Claude Code, Codex, Copilot CLI, Gemini CLI, OpenCode, Kiro, Droid, Amp, DeepSeek Harness ve Pi ile çalışır; agent'a plugin, hook ve komutlarla bağlanır. Yeni "Plannotator Inbox" ise agent'ların çalışırken sana soru, dosya ve inceleme bıraktığı tek bir yerel penceredir. Lisansı Apache-2.0.

## Gereksinimler

- macOS, Linux, WSL ya da Windows (PowerShell): kurulum betiği `plannotator` dosyasını indirir.
- Kurulu en az bir desteklenen agent: Claude Code, Codex, Copilot CLI, Gemini CLI (0.36.0 ve üstü), OpenCode, Kiro CLI, Droid, Amp, DeepSeek Harness, Pi ya da T3 Code.
- Kurulum sırasında internet: README'ye göre betik GitHub Releases'ten indirir; tam kurulumda isteğe bağlı `sem` yardımcısı ve bazı agent dosyaları için GitHub ve npm'e de bağlanabilir.
- Kaynaktan derlemek istersen Bun (README'nin Build bölümü `bun run build` komutlarını kullanır).

## Kurulum

Tek kurulum betiği `plannotator` komutunu kurar, bilgisayardaki agent'ları kendisi bulur ve her biri için hook, skill ve slash komutlarını ayarlar.

macOS, Linux ya da WSL:

```bash
# macOS / Linux / WSL
curl -fsSL https://plannotator.ai/install.sh | bash
```

Windows PowerShell:

```powershell
# Windows PowerShell
irm https://plannotator.ai/install.ps1 | iex
```

Yalnız `plannotator` dosyasını `~/.local/bin` altına kurup hiçbir skill, hook ve agent ayarına dokunmamak için `--minimal` (ya da `PLANNOTATOR_MINIMAL=1`):

```bash
curl -fsSL https://plannotator.ai/install.sh | bash -s -- --minimal
```

Sonra kullandığın agent için kalan adımı yap (README'deki tablodan):

- **Claude Code:** `/plugin marketplace add backnotprop/plannotator`, ardından `/plugin install plannotator@plannotator`; sonra Claude Code'u yeniden başlat.
- **Codex:** ek adım yok; plan incelemesi Codex'in deneysel `Stop` hook'u ile kendiliğinden açılır. `$plannotator-review`, `$plannotator-annotate` ve `$plannotator-last` skill'leri gelir.
- **Copilot CLI:** `/plugin marketplace add backnotprop/plannotator`, ardından `/plugin install plannotator-copilot@plannotator`; yeniden başlat. Plan incelemesi plan kipinde (`Shift+Tab`) çalışır.
- **Gemini CLI** ve **Kiro CLI:** ek adım yok (Kiro için README'nin önerisi `kiro-cli chat --agent plannotator`).
- **OpenCode:** `opencode.json` dosyasına `"plugin": ["@plannotator/opencode@latest"]` ekle, OpenCode'u yeniden başlat.
- **Pi:** kurulum betiğini atla; yalnız `pi install npm:@plannotator/pi-extension`.
- **Droid:** `droid plugin marketplace add https://github.com/backnotprop/plannotator`, ardından `droid plugin install plannotator@plannotator`.
- **Amp**, **DeepSeek Harness** ve **T3 Code** için adımlar README'deki tabloda ve her birinin kendi README'sinde.

Güncellemek için kurulum betiğini yeniden çalıştır. Kaldırmak için `plannotator uninstall` (yerel planlar, geçmiş ve ayarlar kalır); yerel verileri de silmek için `plannotator uninstall --purge`.

## İlk çalıştırma

README'nin "Try it" bölümüne göre en hızlı deneme, agent'ının içinden şu komutlardan birini çalıştırmak:

```
/plannotator-last                   # annotate the agent's last reply
/plannotator-review                 # review your current diff, PR-style
/plannotator-annotate report.html   # annotate any file, folder, or URL
```

1. `/plannotator-last`: agent'ın son cevabını tarayıcıda açar, üzerine not düşersin.
2. `/plannotator-review`: o anki diff'i PR gibi inceletir, yorumların agent'a döner.
3. `/plannotator-annotate report.html`: herhangi bir dosyayı, klasörü ya da adresi işaretlemek için açar.

Codex'te bunlar `$plannotator-*` skill'leri, Amp'te komut paletindedir. Plan incelemesi için komut gerekmez: agent bir sonraki plan önerisinde ekran tarayıcıda kendiliğinden açılır.

## Sık takılınan yerler

- Claude Code plugin'i güncellenmiyorsa: kurulum betiği plugin'i yalnız `claude` komutu PATH'te iken günceller; değilse `claude plugin marketplace update plannotator`, ardından `claude plugin update plannotator@plannotator` çalıştırıp Claude Code'u yeniden başlat (README: yalnız marketplace'i tazelemek plugin'i güncellemez).
- Claude Code 2.1.287 ve üstünde inceleme sırasında Claude beklemez, kararın sonradan mesaj olarak gelir; eski akışa dönmek için `PLANNOTATOR_CLAUDE_MOD=0`.
- Pi'de yalnız `pi update` Pi'yi günceller; eklenti için `pi update --extensions` gerekir.
- Ayarlar tarayıcı çerezlerinde tutulur, çünkü her hook çağrısı rastgele bir portta açılır; sabit port için `PLANNOTATOR_PORT`, tarayıcıyı hiç açmamak için `PLANNOTATOR_BROWSER=none` (README'nin Configuration tablosu).
- Plannotator dosyaları varsayılan olarak `~/.plannotator` altında; yeri `PLANNOTATOR_DATA_DIR` ile değişir, `plannotator data-dir` kullanılan klasörü yazar.

## Bağlantılar

- Repo: https://github.com/backnotprop/plannotator
- Site: https://plannotator.ai
- Kurulum belgeleri: https://docs.plannotator.ai/open-source/start/installation
- Gizlilik ve ağ bağlantıları: https://plannotator.ai/privacy
- Sürümler: https://github.com/backnotprop/plannotator/releases (son sürüm v0.28.66, 16 Kasım 2026)
- Issues: https://github.com/backnotprop/plannotator/issues
- Video: Haftanın Popüler Repoları, 9–15 Kasım 2026 bölümü (listenin 5. sırası)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W46 bölümü](../bolumler/2026-W46.md) videosu için hazırlandı: https://www.youtube.com/watch?v=UADSMY2Z4eY. Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
