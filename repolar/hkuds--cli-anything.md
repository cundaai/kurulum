---
repo: HKUDS/CLI-Anything
url: https://github.com/HKUDS/CLI-Anything
lisans: Apache-2.0
surum: v0.4.0 (2026-06-25)
readme_tarihi: 2026-09-22
dogrulama: denenmedi
bolum: 2026-W48-2
ozet: Herhangi bir yazılımı agent'ların kullanabileceği bir CLI'a çeviren plugin ve hazır CLI'ları kuran CLI-Hub paket yöneticisi.
---

# CLI-Anything — kurulum ve ilk örnek

> **Bu belge, README'nin 22 Eylül 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

CLI-Anything, insanlar için yazılmış yazılımları (GIMP, Blender, LibreOffice, FreeCAD, Draw.io…) AI agent'ların komut satırından kullanabileceği hâle getirir. README iki başlangıç yolu sayıyor: hazır CLI'ları hemen kullanmak için `cli-anything-hub` paket yöneticisi (CLI-Hub: listele, ara, kur, güncelle, çalıştır), ya da henüz CLI'ı olmayan bir yazılım için coding agent'a kurulan plugin/skill ile 7 aşamalı üretim hattı (analiz, tasarım, uygulama, test planı, testler, belgeleme, yayımlama). Üretilen CLI'lar REPL kipi, agent'lar için `--json` çıktısı ve bir `SKILL.md` dosyasıyla gelir. Lisansı Apache-2.0.

## Gereksinimler

- Hazır CLI'lar için: `pip` (README, CLI-Hub yolu için Python sürümü yazmıyor).
- Yeni CLI üretmek için (README "Prerequisites"): Python 3.10+, hedef yazılımın ya da kaynak reposunun yerelde ya da çevrimiçi erişilebilir olması ve desteklenen bir coding agent: Claude Code, Cursor, Pi, OpenClaw, OpenCode, Codex, Hermes, Reasonix, Qodercli ya da GitHub Copilot CLI.
- Bazı CLI'lar gerçek masaüstü ya da sunucu yazılımını sarar: seçtiğin CLI GIMP, Blender, LibreOffice gibi bir uygulamaya gerek duyuyorsa o uygulamayı da kurman gerekir.
- Agent'lara CLI-Hub skill'ini kurmak için `npx` (Node.js).
- Windows'ta Claude Code ile: Git for Windows (`bash` ve `cygpath` ile gelir) ya da WSL.
- README'nin "Limitations" bölümü: güvenilir üretim için güçlü, en üst sınıf bir model gerekir; hat kaynak koddan çalışır, yalnız derlenmiş dosyası olan yazılımlarda nitelik belirgin biçimde düşer.

## Kurulum

Hazır CLI'ları kullanmak için CLI-Hub paket yöneticisi:

```bash
pip install cli-anything-hub
```

Agent'ların doğru CLI'ı kendilerinin bulup kurması için CLI-Hub meta-skill'i (README'ye göre OpenClaw, Nanobot, Claude Code, Codex, Reasonix, Antigravity ve SKILL uyumlu öbür agent'larla çalışır):

```bash
npx skills add HKUDS/CLI-Anything --skill cli-hub-meta-skill -g -y
```

Yeni CLI üretmek için Claude Code plugin'i (komutlar Claude Code oturumunun içinde yazılır):

```bash
# Add the CLI-Anything marketplace
/plugin marketplace add HKUDS/CLI-Anything
```

```bash
# Install the cli-anything plugin from the marketplace
/plugin install cli-anything
```

Marketplace kullanmak istemezsen README'deki elle kurulum:

```bash
# Clone the repo
git clone https://github.com/HKUDS/CLI-Anything.git

# Copy plugin to Claude Code plugins directory
cp -r CLI-Anything/cli-anything-plugin ~/.claude/plugins/cli-anything

# Reload plugins
/reload-plugins
```

Cursor, Pi, OpenClaw, OpenCode, Codex, Hermes, Reasonix, Qodercli ve GitHub Copilot CLI için kurulum adımları README'nin "Pick Your Agent Platform" bölümünde ayrı ayrı anlatılıyor; bu belgeye alınmadı.

## İlk çalıştırma

Hazır bir CLI'ı denemek (README'nin örneği GIMP):

```bash
# Browse what is available
cli-hub list
cli-hub search image

# Install and try a CLI immediately
cli-hub install gimp
cli-hub info gimp
cli-hub launch gimp
```

Öbür komutlar: `cli-hub update <name>` (güncelle), `cli-hub uninstall <name>` (kaldır), `cli-hub launch <name> [args...]` (kurulu CLI'ı çalıştır).

Meta-skill'i kurduysan agent'a README'deki şu cümleyle iş verirsin (üç noktanın yerine görevi yaz):

```text
Find appropriate CLI software in CLI-Hub and complete the task: ...
```

Yeni bir CLI üretmek (Claude Code; 7 aşamanın tamamını çalıştırır):

```bash
# /cli-anything <software-path-or-repo>
# Generate a complete CLI for GIMP (all 7 phases)
/cli-anything ./gimp
```

Yerel klasör yerine GitHub adresi de verilebilir (README örneği: `/cli-anything https://github.com/blender/blender`). Kapsamı genişletmek için isteğe bağlı adım:

```bash
# Broad refinement — agent analyzes gaps across all capabilities
/cli-anything:refine ./gimp

# Focused refinement — target a specific functionality area
/cli-anything:refine ./gimp "I want more CLIs on image batch processing and filters"
```

Üretilen CLI'ı kurup kullanmak (hangi agent ile üretildiğinden bağımsız):

```bash
# Install to PATH
cd gimp/agent-harness && pip install -e .

# Use from anywhere
cli-anything-gimp --help
cli-anything-gimp project new --width 1920 --height 1080 -o poster.json
cli-anything-gimp --json layer add -n "Background" --type solid --color "#1a1a2e"

# Enter interactive REPL
cli-anything-gimp
```

Öbür plugin komutları: `/cli-anything:test <software-path-or-repo>` (testleri çalıştırır, TEST.md'yi günceller) ve `/cli-anything:validate <software-path-or-repo>` (HARNESS.md ölçütlerine göre denetler).

## Sık takılınan yerler

- `Unknown skill: cli-anything` görürsen komutun yazılışını değiştirmek işe yaramaz; önce plugin'in kurulu ve yüklü olduğuna bak (README): `/reload-plugins` ile komutları yeniden yükle, `/help cli-anything` ile yüklendiğini doğrula, gerekirse marketplace'ten yeniden kur (`/plugin marketplace add HKUDS/CLI-Anything`, `/plugin install cli-anything`), sonra `/cli-anything ./gimp` komutunu yeniden dene.
- Eski Claude Code sürümlerinde, plugin'in yüklü olduğu doğrulandığı hâlde `/cli-anything` tanınmıyorsa eski biçimi dene: `/cli-anything:cli-anything ./gimp`. Yardımcı komutlar her zaman `:subcommand` biçimindedir (örn. `/cli-anything:refine`).
- Windows'ta `cygpath: command not found` hatası: Git for Windows kur ya da WSL kullan.
- `cli-hub install` yalnız CLI'ı kurar; sardığı uygulama (GIMP, Blender, LibreOffice…) kurulu değilse CLI işini yapamaz.
- Tek bir `/cli-anything` çalıştırması yazılımın bütün yeteneklerini kapsamayabilir; README kapsamı ve niteliği artırmak için `/cli-anything:refine` komutunun bir ya da birkaç kez çalıştırılmasını öneriyor (her çalıştırma eklemelidir, var olanı bozmaz).
- Zayıf ya da küçük modeller eksik ya da hatalı CLI üretebilir (README "Limitations").

## Bağlantılar

- Repo: https://github.com/HKUDS/CLI-Anything
- CLI-Hub (hazır CLI kataloğu): https://clianything.cc/
- Belgeler: https://github.com/HKUDS/CLI-Anything/blob/main/cli-anything-plugin/QUICKSTART.md (5 dakikalık başlangıç) · https://github.com/HKUDS/CLI-Anything/blob/main/cli-anything-plugin/README.md (plugin komutları, seçenekler, aşamalar) · https://github.com/HKUDS/CLI-Anything/blob/main/cli-anything-plugin/HARNESS.md (yöntem) · https://github.com/HKUDS/CLI-Anything/blob/main/cli-anything-plugin/PUBLISHING.md (yayımlama)
- Sürümler: https://github.com/HKUDS/CLI-Anything/releases (son sürüm v0.4.0, 25 Haziran 2026)
- Issues: https://github.com/HKUDS/CLI-Anything/issues
- Video: Haftanın Popüler Repoları, 23–25 Kasım 2026 bölümü (listenin 4. sırası)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W48-2 bölümü](../bolumler/2026-W48-2.md) videosu için hazırlandı: https://www.youtube.com/watch?v=2wDhRoLOWh8. Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
