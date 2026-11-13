---
repo: coreyhaines31/marketingskills
url: https://github.com/coreyhaines31/marketingskills
lisans: MIT
surum: v2.14.12 (2026-11-11)
readme_tarihi: 2026-11-11
dogrulama: denenmedi
bolum: gunun-reposu/g09
ozet: Claude Code, Codex ve Cursor gibi agent'lara CRO, copywriting, SEO ve analytics bilgisi katan açık kaynak pazarlama skill koleksiyonu.
---

# Marketing Skills — kurulum ve ilk örnek

> **Bu belge, README'nin 11 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

Corey Haines'in hazırladığı bu depo, kurulacak bir yazılım değil, Markdown dosyalarından oluşan bir skill koleksiyonudur. Her skill, agent'a belirli bir pazarlama işinde (landing page dönüşümü, sayfa metni, e-posta dizisi, SEO denetimi, fiyatlandırma, A/B testi…) hangi çerçeveyi ve adımları uygulayacağını öğretir. Claude Code, OpenAI Codex, Cursor, Windsurf ve Agent Skills spec'ini destekleyen her agent ile çalışır. Teknik pazarlamacılar ve kendi ürününü pazarlayan kurucular için.

## Gereksinimler

- Skill destekleyen bir agent: Claude Code, OpenAI Codex, Cursor, Windsurf ya da Agent Skills spec'ini okuyan başka bir araç.
- Önerilen yol olan `npx skills` için Node.js ve npm. README sürüm belirtmiyor; kullanılan `skills` CLI'ının kendi `package.json` dosyası (vercel-labs/skills) Node 22.20 ve üzerini istiyor.
- Klonlama ve submodule seçenekleri için git.
- Hesap ya da API anahtarı gerekmez; skill'ler düz metindir. Pazarlama hesaplarını (analytics, reklam) bağlamak isteğe bağlıdır ve ayrı bir kılavuzu var: [tools/SETUP.md](https://github.com/coreyhaines31/marketingskills/blob/main/tools/SETUP.md).

## Kurulum

### Seçenek 1: skills CLI (önerilen)

```bash
# Install all skills
npx skills add coreyhaines31/marketingskills

# Install specific skills
npx skills add coreyhaines31/marketingskills --skill cro copywriting

# List available skills
npx skills add coreyhaines31/marketingskills --list
```

CLI hangi agent'ların kurulu olduğunu bulur ve nereye kuracağını sorar. Claude Code için `.claude/skills/` altına, ortak agent'lar için `.agents/skills/` altına kurar. Komutu bir agent oturumunun içinden çalıştırıyorsan hedefi açıkça ver:

```bash
npx skills add coreyhaines31/marketingskills -a claude-code
```

### Seçenek 2: Claude Code plugin

Etkileşimli Claude Code oturumunda:

```bash
# Add the marketplace
/plugin marketplace add coreyhaines31/marketingskills

# Install all marketing skills
/plugin install marketing-skills
```

`/plugin` komutunun olmadığı ortamlarda (Claude Code web, GitHub Actions) terminal karşılıkları:

```bash
claude plugin marketplace add coreyhaines31/marketingskills
claude plugin install marketing-skills@marketingskills
```

### Seçenek 3: OpenAI Codex plugin

```bash
# Add the marketplace
codex plugin marketplace add coreyhaines31/marketingskills

# Then browse and install from inside a Codex session
/plugins
```

Listeden **marketing-skills** seç. Yeni sürüm için `codex plugin marketplace upgrade`.

### Seçenek 4: klonla ve kopyala

```bash
git clone https://github.com/coreyhaines31/marketingskills.git
cp -r marketingskills/skills/* .agents/skills/
```

README'de ayrıca git submodule, fork, SkillKit, Claude web için ZIP ve Perplexity Computer için ZIP seçenekleri var (Seçenek 5–9).

## İlk çalıştırma

Kurulumdan sonra ek ayar yok; agent'a pazarlama işi verirsin, uygun skill kendiliğinden devreye girer. README'deki örnekler:

```
"Help me optimize this landing page for conversions"
→ Uses cro skill

"Write homepage copy for my SaaS"
→ Uses copywriting skill
```

Skill'i doğrudan da çağırabilirsin:

```
/cro
/emails
/seo-audit
```

Mantıklı ilk adım `product-marketing` skill'idir: öteki bütün skill'ler işe başlamadan önce ürünü, hedef kitleyi ve konumlandırmayı anlatan `.agents/product-marketing.md` dosyasına bakar. Önce bu dosyayı oluşturup sonra `/cro` ile bir sayfanı değerlendirmek, kurulumun çalıştığını görmenin en kısa yolu.

## Sık takılınan yerler

- `npx skills add` komutunu agent oturumunun içinden (örneğin Claude Code'a "kur" diyerek) çalıştırınca CLI etkileşimsiz çalışır ve yalnız `.agents/skills/` altına kurabilir; Claude Code bu dizini okumaz. `-a claude-code` bayrağını ver ([README → Option 1](https://github.com/coreyhaines31/marketingskills#option-1-cli-install-recommended)).
- `/plugin` yalnız etkileşimli Claude Code CLI oturumunda var; web, GitHub Actions ve diğer uzak ortamlarda `/plugin isn't available in this environment` görürsün. `npx skills` ya da `claude plugin ...` komutlarını kullan ([README → Option 2](https://github.com/coreyhaines31/marketingskills#option-2-claude-code-plugin)).
- v1.x'ten v2.0'a geçişte 17 skill'in adı değişti ve `page-cro` ile `form-cro` tek `cro` skill'inde birleşti; eski klasörler yenilerinin yanında kalır. README'deki `rm -rf` listesiyle temizleyip yeniden kur ([README → Upgrading](https://github.com/coreyhaines31/marketingskills#upgrading-from-v1x-to-v20)).
- Bağlam dosyası `.claude/product-marketing-context.md` yerine artık `.agents/product-marketing.md`; eski dosya yedek olarak okunmaya devam eder ama taşımak daha temiz ([README → Migrate](https://github.com/coreyhaines31/marketingskills#migrate-the-product-marketing-context-file)).

## Bağlantılar

- Repo: https://github.com/coreyhaines31/marketingskills
- Belgeler: https://marketing-skills.com, [tools/SETUP.md](https://github.com/coreyhaines31/marketingskills/blob/main/tools/SETUP.md) (hesap bağlama), [CONTRIBUTING.md](https://github.com/coreyhaines31/marketingskills/blob/main/CONTRIBUTING.md)
- Sürümler: https://github.com/coreyhaines31/marketingskills/releases
- Lisans: https://github.com/coreyhaines31/marketingskills/blob/main/LICENSE (MIT)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki Günün Reposu g09 videosu için hazırlandı (yakında — 19 Kas 12:30). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
