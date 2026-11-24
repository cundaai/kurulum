---
repo: google-labs-code/stitch-skills
url: https://github.com/google-labs-code/stitch-skills
lisans: Apache-2.0
surum: v1.0 (2026-05-18)
readme_tarihi: 2026-11-17
dogrulama: denenmedi
bolum: 2026-W47
ozet: Google Stitch MCP sunucusuyla çalışan, tasarım üretme ve tasarımı koda çevirme işlerini coding agent'lara öğreten skill ve plugin koleksiyonu.
---

# stitch-skills — kurulum ve ilk örnek

> **Bu belge, README'nin 17 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

Stitch Design Skills, Google Stitch için hazırlanmış skill ve plugin koleksiyonudur; Agent Skills açık standardını izler. README'ye göre Codex, Antigravity, Gemini CLI, Claude Code ve Cursor ile uyumludur; OpenCode'da elle kurulur. Üç plugin var: `stitch-design` (metinden ya da görselden ekran üretme, kodu Stitch tasarımına çevirme, DESIGN.md çıkarma ve yükleme), `stitch-build` (Stitch ekranlarını React, React Native ve React + Vite koduna çevirme, Remotion ile tanıtım videosu, shadcn/ui rehberi) ve `stitch-utilities` (prompt iyileştirme, DESIGN.md ve SITE.md üretme, tek prompt'tan çok sayfalı site). README'nin notu: bu, Google'ın resmî olarak desteklediği bir ürün değildir. Lisansı Apache-2.0.

## Gereksinimler

- Agent'ının ortamında kurulu ve çalışan **Stitch MCP** sunucusu (README: "These skills require the Stitch MCP server to be configured and running"). Kayıt, ortam değişkenleri ve kimlik bilgileri için: https://stitch.withgoogle.com/docs/mcp/setup/
- Desteklenen bir agent: Codex, Antigravity, Gemini CLI, Claude Code, Cursor ya da OpenCode.
- `npx plugins` ve `npx skills` komutları için `npx` (Node.js ile gelir). README bir Node.js sürümü belirtmiyor.

## Kurulum

Önerilen yol plugin'leri kurmak. Claude Code (içinde bulunduğun projeye kurar):

```bash
# Claude Code — installs into the current project
npx plugins add google-labs-code/stitch-skills --scope project --target claude-code
```

Cursor (açık workspace'e kurar):

```bash
# Cursor — installs into the current workspace
npx plugins add google-labs-code/stitch-skills --scope workspace --target cursor
```

Codex: önce marketplace'i ekle, sonra `stitch-design`, `stitch-build`, `stitch-utilities` plugin'lerinden istediklerini kur. `--sparse` bayrakları isteğe bağlıdır; yalnız listelenen yolları indirir.

```bash
codex plugin marketplace add google-labs-code/stitch-skills --ref main \
  --sparse .agents/plugins \
  --sparse plugins/stitch-design \
  --sparse plugins/stitch-build \
  --sparse plugins/stitch-utilities
```

Codex arayüzünden: **Settings → Plugin Marketplaces → Add**; Source `https://github.com/google-labs-code/stitch-skills`, Git ref `main`.

Yalnız seçtiğin skill'leri kurmak için (README uyarıyor: skill'ler birbirine bağımlı olabilir, bağımlılıkları da kur):

```bash
npx skills add google-labs-code/stitch-skills
```

OpenCode `npx plugins` / `npx skills` yolunu kullanmaz; skill klasörleri elle kopyalanır:

```bash
# Project-local (recommended)
mkdir -p .opencode/skills
cp -R plugins/stitch-design/skills/generate-design .opencode/skills/generate-design
# repeat for other skills under plugins/*/skills/
```

Ardından Stitch MCP, proje `opencode.json` dosyasına ya da `~/.config/opencode/opencode.json` dosyasına eklenir; `YOUR_STITCH_MCP_URL` yerine Stitch MCP kurulum akışının verdiği endpoint yazılır:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "stitch": {
      "type": "remote",
      "url": "YOUR_STITCH_MCP_URL",
      "enabled": true
    }
  }
}
```

Antigravity ve Gemini CLI için README ayrı bir kurulum komutu vermiyor.

## İlk çalıştırma

README adım adım bir "ilk çalıştırma" bölümü vermiyor; bunun yerine her skill için örnek prompt'lar listeliyor. Stitch MCP kurulu ve plugin'ler yüklüyken agent'ına bunlardan birini yazarsın. README'deki örneklerden üçü:

```text
Make a browse tab for a mobile app for romance and date night ideas.
```

```text
Enhance this prompt: 'make a settings page'.
```

```text
Convert all screens in Stitch project `projects/123` to React components.
```

İlki `stitch::generate-design`, ikincisi `enhance-prompt`, üçüncüsü `stitch::react-components` skill'inin örneği (`projects/123` yerine kendi Stitch proje kimliğini yaz). Komutların yardım metni için:

```bash
npx plugins --help

npx skills --help
```

## Sık takılınan yerler

- Skill'ler Stitch MCP olmadan çalışmaz; önce Stitch MCP kurulum belgesindeki adımlar tamamlanmalı (README, Prerequisites).
- Skill'leri tek tek kuruyorsan bağımlı olduklarını da kur (README'nin IMPORTANT notu).
- OpenCode, skill `name` alanının küçük harfli kebab-case olmasını ve klasör adıyla eşleşmesini ister; `stitch::…` adlı skill'lerde `name` alanı ve klasör yeniden adlandırılmadan yüklenmez. `allowed-tools` gibi ek frontmatter yok sayılır.
- OpenCode'da araç adları farklı olabilir (`web_fetch` / `run_command` yerine `webfetch` / `bash`); skill'i uyarlarken OpenCode adlarını kullan.
- stitch.withgoogle.com/docs üzerindeki bir bağlantı yanlış yönlenirse hedef yolu aynı adres altında elle aç (README'nin "Docs navigation" notu).

## Bağlantılar

- Repo: https://github.com/google-labs-code/stitch-skills
- Stitch MCP kurulumu: https://stitch.withgoogle.com/docs/mcp/setup/
- Stitch belgeleri: https://stitch.withgoogle.com/docs/
- OpenCode skill belgeleri: https://opencode.ai/docs/skills/
- Sürümler: https://github.com/google-labs-code/stitch-skills/releases (son sürüm v1.0, 18 Mayıs 2026)
- Issues: https://github.com/google-labs-code/stitch-skills/issues
- Video: Haftanın Popüler Repoları, 16–22 Kasım 2026 bölümü

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W47 bölümü](../bolumler/2026-W47.md) videosu için hazırlandı: https://www.youtube.com/watch?v=6jXWzYzxfKI. Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
