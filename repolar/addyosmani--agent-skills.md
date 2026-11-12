---
repo: addyosmani/agent-skills
url: https://github.com/addyosmani/agent-skills
lisans: MIT
surum: 0.6.15 (2026-10-31)
readme_tarihi: 2026-11-01
dogrulama: denenmedi
bolum: trend-short/t2026-11-12-2
ozet: Addy Osmani'nin coding agent'lara spec, plan, test, review ve ship disiplini kazandıran 25 skill ve 9 slash komutluk paketi.
---

# Agent Skills — kurulum ve ilk örnek

> **Bu belge, README'nin 1 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

Agent Skills, kurulacak bir program değil, Markdown ile yazılmış 25 iş akışından oluşan bir pakettir. Her skill, kıdemli mühendislerin yazılım geliştirirken izlediği adımları (önce spec, küçük görevler, test önce kod, beş eksenli review, güvenli yayın) agent'ın atlayamayacağı kontrol noktaları ve "bahane–karşılık" tablolarıyla kodlar. Claude Code'da `/spec`, `/plan`, `/build`, `/test`, `/review`, `/ship` gibi 9 slash komutu olarak kurulur; Cursor, Codex, Gemini CLI, Copilot ve diğer agent'larda da çalışır. Agent'ının spec ve test atlamadan üretim kalitesinde kod yazmasını isteyen geliştiriciler için.

## Gereksinimler

- Bir AI coding agent: Claude Code (README'nin önerdiği), Cursor, OpenAI Codex (CLI v0.122+), Gemini CLI, Antigravity CLI, GitHub Copilot, OpenCode, Windsurf, Kiro ya da Command Code.
- `npx skills` yolu için Node.js ve npm. README sürüm belirtmiyor; kullanılan `skills` CLI'ının (vercel-labs/skills) `package.json` dosyası Node 22.20 ve üzerini istiyor.
- Klonlama için git. Claude Code marketplace'i depoyu SSH ile klonlar; GitHub'da SSH anahtarın yoksa HTTPS yolu var (aşağıda).
- Hesap ya da API anahtarı gerekmez.

## Kurulum

### En kısa yol: skills CLI (70'ten fazla agent)

```bash
npx skills add addyosmani/agent-skills            # install all 25 skills
npx skills add addyosmani/agent-skills --list     # browse before installing
```

Tek skill kurmak için:

```bash
npx skills add addyosmani/agent-skills --skill code-review-and-quality   # five-axis review before merge
npx skills add addyosmani/agent-skills --skill interview-me              # requirements interrogation, one question at a time
npx skills add addyosmani/agent-skills --skill test-driven-development   # red-green-refactor, enforced
```

### Claude Code (önerilen)

Marketplace ile:

```
/plugin marketplace add addyosmani/agent-skills
/plugin install agent-skills@addy-agent-skills
```

SSH anahtarın yoksa HTTPS adresle ekle:

```bash
/plugin marketplace add https://github.com/addyosmani/agent-skills.git
/plugin install agent-skills@addy-agent-skills
```

Yerel klondan ya da geliştirme için:

```bash
git clone https://github.com/addyosmani/agent-skills.git
claude --plugin-dir /path/to/agent-skills
```

### Codex

```bash
codex plugin marketplace add addyosmani/agent-skills
codex plugin add agent-skills@agent-skills
```

Kurulduktan sonra sohbette `@spec-driven-development` gibi `@` ile çağrılır.

### Gemini CLI

```bash
gemini skills install https://github.com/addyosmani/agent-skills.git --path skills
```

Cursor (`.cursor/skills/`), Antigravity (`agy plugin install`), Windsurf, OpenCode, Copilot, Kiro ve Command Code için adımlar README'deki açılır bölümlerde ve `docs/` altındaki kurulum kılavuzlarında.

## İlk çalıştırma

Claude Code'da bir projede şu sırayı dene: `/spec` ile ne yapacağını anlat; skill bir PRD çıkarır. `/plan` bunu küçük, doğrulanabilir görevlere böler. `/build` görevleri tek tek, test yazarak uygular; `/test` kanıt ister; `/review` beş eksenli inceleme yapar; `/ship` yayın öncesi listeyi geçirir. Spec hazırsa `/build auto` planı üretip her görevi tek onayla sırayla uygular; test ve commit görev başına yine ayrı yapılır, hata ya da riskli adımda durur.

Komut vermeden de çalışır: API tasarlarken `api-and-interface-design`, arayüz yazarken `frontend-ui-engineering` kendiliğinden devreye girer. İstek belirsizse "interview me" demen `interview-me` skill'ini başlatır; tek tek soru sorarak ne istediğini netleştirir.

## Sık takılınan yerler

- Tek skill'i `npx` ile kurunca yalnız `skills/<ad>/` kopyalanır, depo kökündeki `references/` kontrol listeleri gelmez; skill çalışır ama ortak listelere giden yollar boş kalır. Tüm paketi kur, repoyu klonla ya da gereken listeyi skill içine `references/` olarak kopyala ([README notu](https://github.com/addyosmani/agent-skills#quick-start), [#361](https://github.com/addyosmani/agent-skills/issues/361)).
- Marketplace kurulumu `git@github.com: Permission denied (publickey)` verirse HTTPS adresle ekle ya da git'e SSH adreslerini HTTPS'e çevirmesini söyle: `git config --global url."https://github.com/".insteadOf git@github.com:` ([README → Claude Code](https://github.com/addyosmani/agent-skills#quick-start)).
- Claude Code'un kendi `/review` komutu aynı adı taşır; paketin review'ü için `/agent-skills:review` yaz ([README → Commands](https://github.com/addyosmani/agent-skills#commands)).
- Antigravity CLI'ın bazı sürümlerinde komut TOML'ları dönüştürüldü görünür ama sarmalayıcı komutlar listede çıkmaz; skill'leri doğrudan ad alanlı adıyla çağır ([docs/antigravity-setup.md](https://github.com/addyosmani/agent-skills/blob/main/docs/antigravity-setup.md#lifecycle-workflows-and-command-compatibility)).
- GitHub Copilot'ta `/spec`, `/plan` gibi komutlar yoktur, bunlar Claude Code komutudur; skill'i adıyla (`/spec-driven-development`) çağır ([docs/copilot-setup.md](https://github.com/addyosmani/agent-skills/blob/main/docs/copilot-setup.md)).

## Bağlantılar

- Repo: https://github.com/addyosmani/agent-skills
- Belgeler: https://skills.addy.ie, [docs/getting-started.md](https://github.com/addyosmani/agent-skills/blob/main/docs/getting-started.md), [docs/adoption-guide.md](https://github.com/addyosmani/agent-skills/blob/main/docs/adoption-guide.md) (var olan kod tabanına kademeli geçiş), [docs/comparison.md](https://github.com/addyosmani/agent-skills/blob/main/docs/comparison.md) (Superpowers ve Matt Pocock skill'leriyle karşılaştırma)
- Sürümler: https://github.com/addyosmani/agent-skills/releases
- Lisans: https://github.com/addyosmani/agent-skills/blob/main/LICENSE (MIT)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [trend Short t2026-11-12-2](../bolumler/trend-short-t2026-11-12-2.md) videosu için hazırlandı (yakında — 13 Kas 14:00). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
