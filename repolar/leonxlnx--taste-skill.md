---
repo: Leonxlnx/taste-skill
url: https://github.com/Leonxlnx/taste-skill
lisans: MIT
surum: v2.5.1 (2026-11-07)
readme_tarihi: 2026-11-07
dogrulama: denenmedi
bolum: 2026-W46
ozet: Coding agent'ların sıradan landing page yerine tek tasarım sistemli, gerçek fotoğraflı ve tarayıcıda denetlenmiş profesyonel şirket siteleri kurmasını sağlayan Agent Skill.
---

# taste-skill — kurulum ve ilk örnek

> **Bu belge, README'nin 7 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

Taste Skill (tasteskill 2.5.1), coding agent'ların sıradan bir landing page toplamak yerine figma.com ve x.ai düzeyinde profesyonel şirket siteleri kurmasını hedefleyen bir Agent Skill'dir. README'ye göre her yapımda sade metin, tek tasarım sistemi, ekran başına bir bölüm, eşit sol ve sağ kenar boşlukları, gerçek fotoğraflar (müşterinin yoksa lisanslı yedekler), gerçek marka ikonları ve ürün arayüzü, ölçülü mikro etkileşimler kullanılır; shader efektleri ve kaydırma sahneleri yalnız teknoloji, oyun ve yazılım sayfalarına ayrılır. Sonuç gerçek bir tarayıcıda bir denetleyici ve bitiş kapısıyla kontrol edilir. Lisansı MIT.

## Gereksinimler

- Skill destekleyen bir coding agent: README Claude Code ve Codex için plugin yolunu ayrıca anlatıyor.
- Node 22 ve üstü: skill betiklerini Node ile çalıştırır.
- Kurulu Chrome, Edge ya da Chromium; standart olmayan bir yerdeyse `CHROME_PATH` ile göster.
- İsteğe bağlı: PATH'te ffmpeg (WebP çıktısı ve `photos.mjs` içinde hero fotoğrafının telefon kırpması için).
- API anahtarı gerekmez. Paketteki Wikimedia Commons fotoğrafları çevrimdışı çalışır; ek fotoğraf ya da ikon çekmek internet ister.

## Kurulum

`npx skills` CLI'ı ile:

```bash
npx skills add https://github.com/Leonxlnx/taste-skill --skill "design-taste-frontend"
```

Claude Code plugin'i olarak (Claude Code'un içinde çalıştır; skill `taste-skill:` adı altında gelir, öbür skill'lerinle çakışmaz):

```
/plugin marketplace add Leonxlnx/taste-skill
/plugin install taste-skill@taste-skill
```

Sonradan güncellemek için `/plugin update`.

Codex plugin'i olarak (ardından skill'in yüklenmesi için yeni bir Codex oturumu aç):

```bash
codex plugin marketplace add Leonxlnx/taste-skill
codex plugin add taste-skill@taste-skill
```

Kopyalamak yerine bağlamak istersen: repoyu klonla ve `./install.sh` çalıştır; skill Claude Code'a sembolik bağla eklenir (`--codex` Codex için, `--all-agents` ikisi için), böylece `git pull` onu yerinde günceller.

Skill zaten kuruluysa kurulum komutunu yeniden çalıştırman yeter; kurulum adı değişmedi.

## İlk çalıştırma

README'den ayıklanan kurulum bölümü adım adım bir ilk kullanım örneği ya da hazır bir istem vermiyor. Skill kurulduktan sonra agent'ına bir şirket sitesi brief'i verip sayfayı kurmasını istersin; README'nin anlattığı örnek sayfalar "tek bir brief"ten üretilmiş. Claude Code plugin'i ile kurduysan skill `taste-skill:` adı altında görünür. Skill'in fotoğraf, ikon ve tarayıcı denetimi betikleri Node 22+ ve kurulu bir Chrome/Edge/Chromium ister; bunlar eksikse önce onları kur.

## Bağlantılar

- Repo: https://github.com/Leonxlnx/taste-skill
- Site: https://tasteskill.dev
- `npx skills` CLI'ı: https://github.com/vercel-labs/agent-skills
- Sürümler: https://github.com/Leonxlnx/taste-skill/releases (son sürüm v2.5.1, 7 Kasım 2026)
- Issues: https://github.com/Leonxlnx/taste-skill/issues
- Video: Haftanın Popüler Repoları, 9–15 Kasım 2026 bölümü (listenin 7. sırası)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W46 bölümü](../bolumler/2026-W46.md) videosu için hazırlandı (video henüz yayında değil). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
