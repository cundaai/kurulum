---
repo: cathrynlavery/diagram-design
url: https://github.com/cathrynlavery/diagram-design
lisans: MIT
surum: sürüm etiketi yok
readme_tarihi: 2026-11-23
dogrulama: denenmedi
bolum: trend-short/t2026-11-23-2
ozet: Coding agent'ına, markanın renk ve yazı tipleriyle tek dosyalık HTML + SVG diyagramlar çizdiren agent skill.
---

# diagram-design — kurulum ve ilk örnek

> **Bu belge, README'nin 23 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

Diagram Design, agent'ına diyagram çizdiren bir agent skill. README'ye göre diyagramı kendi kendine yeten tek bir HTML + SVG dosyası olarak, senin markanın görünümüyle üretir: agent'a düz cümleyle ne istediğini söylersin, agent diyagram türünü seçer, planını söyler ve çift tıklayınca açılan tek bir `.html` dosyası yazar. README 52 diyagram türü diyor (GitHub açıklamasında 45 yazıyor). Lisansı MIT.

## Gereksinimler

- Agent Skills okuyan bir host. README'nin saydıkları: Cursor, Claude Code, Codex, Qoder, GitHub Copilot, Gemini CLI, Cline, Windsurf ve Amp; ayrıca Factory Droid, Pi, Kiro ve OpenCode için ayrı adımlar var.
- `npx skills add` yolu için Node.js (`npx`).
- README'nin notu: resmî sürümler yalnız bu repodan gelir; skill'in ağ üzerinden ne gönderdiği `PRIVACY.md` dosyasında yazıyor.

## Kurulum

Agent Skills okuyan her host için:

```bash
npx skills add cathrynlavery/diagram-design
```

Claude Code'da plugin marketplace yolu (README'ye göre güncellemeleri ve slash komutlarını bu yol getirir):

```text
/plugin marketplace add cathrynlavery/diagram-design
/plugin install diagram-design@diagram-design
```

Codex:

```bash
codex plugin marketplace add cathrynlavery/diagram-design
codex plugin add diagram-design@diagram-design
```

GitHub Copilot:

```bash
copilot plugin marketplace add cathrynlavery/diagram-design
copilot plugin install diagram-design@diagram-design
```

Factory Droid:

```bash
droid plugin marketplace add https://github.com/cathrynlavery/diagram-design
droid plugin install diagram-design@diagram-design --scope user
```

Pi:

```bash
pi install https://github.com/cathrynlavery/diagram-design
```

Qoder, Kiro, OpenCode, Claude Cowork ve style guide'ı klonda düzenlemek için "editable install" adımları README'nin Install bölümünde.

## İlk çalıştırma

Kurduktan sonra agent'ına düz cümleyle diyagram iste. README'nin örnek istekleri:

```text
Make me an architecture diagram of my app: frontend, backend, database, Redis cache.
```

```text
I need a quadrant showing Q2 projects by impact vs effort.
```

```text
Onboard diagram-design to https://yoursite.com
```

Son istek, README'ye göre sitenin renk paletini ve yazı tiplerini alır; sonraki her diyagram sitene uyar.

Repoyu klonladıysan bütün diyagramları görmek için galeriyi aç (README'nin Quickstart bölümü):

```bash
# From a cloned checkout, open the gallery to see every diagram
open skills/diagram-design/assets/index.html       # macOS
xdg-open skills/diagram-design/assets/index.html  # Linux
```

Doğrudan bir şablondan başlamak da mümkün:

```bash
cp skills/diagram-design/assets/template.html my-diagram.html        # minimal light
cp skills/diagram-design/assets/template-full.html my-diagram.html   # editorial with summary cards
cp skills/diagram-design/assets/template-motion.html my-diagram.html # optional accessible motion
```

## Sık takılınan yerler

- İlk kullanımda skill durup soru sorar: yeni bir projede `style-guide.md` hâlâ varsayılandaysa onboarding çalıştırmak, token'ları elle yapıştırmak ya da varsayılanla devam etmek arasında seçim ister (README, "First-run gate").
- `npx skills add` ile kurulan kopya marketplace güncellemelerini kendiliğinden izlemez; güncelleme için `npx skills update diagram-design`. Bu yol yalnız skill'i kurar; `/export-diagram`, `/import-mermaid`, `/import-plantuml`, `/profile` ve `/doctor` komutları marketplace paketlerinde kalır (README).
- Claude Code üçüncü taraf marketplace'lerde auto-update'i varsayılan olarak kapalı tutar: `/plugin` → **Marketplaces** → **diagram-design** → **Enable auto-update** (README).
- Önceden `npx skills add` ile kurduysan Codex marketplace'e geçmeden önce o kopyayı kaldırman gerekir (README, "One-time migration").
- Paket güncellemeleri `references/style-guide.md` dosyasındaki değişiklikleri ezebilir; `~/.diagram-design/profiles/` altındaki kayıtlı profiller güncellemede kalır (README, "Editable install").

## Bağlantılar

- Repo: https://github.com/cathrynlavery/diagram-design
- Site: https://diagramdesign.dev
- Tarifler (ilk diyagram, marka ayarı, import, export, Windows): https://github.com/cathrynlavery/diagram-design/blob/main/docs/cookbook.md
- Gizlilik: https://github.com/cathrynlavery/diagram-design/blob/main/PRIVACY.md
- `skills` CLI: https://skills.sh
- Issues: https://github.com/cathrynlavery/diagram-design/issues
- Video: Haftanın Popüler Repoları, 23 Kasım 2026 Short'u

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki trend Short t2026-11-23-2 videosu için hazırlandı (video henüz yayında değil). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
