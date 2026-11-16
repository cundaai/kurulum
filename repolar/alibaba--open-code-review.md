---
repo: alibaba/open-code-review
url: https://github.com/alibaba/open-code-review
lisans: Apache-2.0
surum: v1.13.10 (2026-11-15)
readme_tarihi: 2026-11-09
dogrulama: denenmedi
bolum: 2026-W46
ozet: Git diff'lerini LLM agent'ıyla inceleyip satır satır kod incelemesi yorumu üreten, Alibaba'nın açık kaynak code review CLI aracı.
---

# open-code-review — kurulum ve ilk örnek

> **Bu belge, README'nin 9 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

Open Code Review (komutu `ocr`), Alibaba'nın iç kod inceleme asistanından açık kaynağa çıkarılmış bir CLI aracı. Git diff'ini okur, değişen dosyaları tool kullanabilen bir agent üzerinden seçtiğin LLM'e gönderir ve satır düzeyinde yapılandırılmış inceleme yorumları üretir. Agent dosyanın tamamını okuyabilir, kod tabanında arama yapabilir; `ocr scan` ile diff olmadan bütün dosyaları da tarayabilir. Kurallı (deterministik) adımlarla agent'ı birleştirir; README'ye göre aynı modelle genel amaçlı agent'lardan daha isabetli ve çok daha az token harcar. OpenAI ve Anthropic uyumlu sağlayıcılarla çalışır; Claude Code, Codex, Cursor gibi agent'lar için plugin'leri ve CI/CD entegrasyonu var. Lisansı Apache-2.0.

## Gereksinimler

- Git 2.41 ya da üstü (diff, kod arama ve repo işlemleri için).
- npm (README'deki kurulum yolu `npm install -g`).
- Bir LLM sağlayıcısı ve onun API anahtarı. İstisna: Delegation Mode'da incelemeyi senin coding agent'ın kendi modeliyle yapar, OCR için ayrı anahtar gerekmez.

## Kurulum

```bash
npm install -g @alibaba-group/open-code-review
```

Kurulumdan sonra `ocr` komutu her yerden çalışır. Kurulum betiği, GitHub Release'teki hazır dosya ya da kaynaktan derleme gibi başka yollar için README sitenin Installation sayfasını gösteriyor: https://open-codereview.ai/docs/installation

Claude Code, Codex, Cursor, Kimi Code ve OpenCode plugin'lerinin kurulumu repodaki `plugins/open-code-review/README.md` dosyasında.

## İlk çalıştırma

1. LLM'i ayarla (Delegation Mode kullanmıyorsan zorunlu). Etkileşimli ekran sağlayıcıyı seçtirir, API anahtarını ister, modeli seçtirir ve bağlantıyı kendiliğinden test eder:

```bash
ocr config provider          # Select a built-in provider or add a custom one
ocr config model             # Pick a model for the active provider
```

2. Bir projede incelemeyi başlat. README'deki örnekler:

```bash
cd your-project

# Workspace mode — review all staged, unstaged, and untracked changes
ocr review

# Branch range — reviews feature-branch's changes since it diverged from main (merge-base mode)
ocr review --from main --to feature-branch

# Single commit
ocr review --commit abc123
```

3. Diff yerine dosyaların tamamını taramak için `ocr scan` (bütün repo) ya da `ocr scan --path internal/agent` (bir klasör). Sonucu dosyaya yazmak için (README AI agent'lar için bunu öneriyor):

```bash
ocr review --format json --output result.json
```

Yarıda kalan inceleme `ocr session list` ile bulunur, `--resume <session-id>` ile sürdürülür. Delegation Mode'da (`ocr delegate preview`, `ocr delegate rule <dosyalar>`) dosya seçimini ve kuralları OCR yapar, incelemeyi senin coding agent'ın yapar; LLM ayarı gerekmez.

## Sık takılınan yerler

- `ocr review`, `/open-code-review:review` komutu ve `open-code-review` skill'i OCR'ın kendi sağlayıcı ayarını kullanır, Claude Code Team/Pro aboneliğindeki oturumunu kullanmaz. Anahtar girmeden çalıştırmak istiyorsan `/open-code-review:delegate-review` komutu ya da `open-code-review-delegate` skill'i ile Delegation Mode'u kullan (README).
- README'ye göre Recall (yakalanan hata oranı) genel amaçlı agent'lardan düşük; bu, gürültüyü azaltmak için bilerek seçilmiş bir denge. Her hatayı yakalamasını bekleme.
- Diğer sorular için sitenin SSS sayfası: https://open-codereview.ai/docs/faq

## Bağlantılar

- Repo: https://github.com/alibaba/open-code-review
- Site ve belgeler: https://open-codereview.ai (kurulum: /docs/installation, ayarlar: /docs/configuration, Delegation Mode: /docs/delegate, CI/CD: /docs/cicd, MCP: /docs/mcp)
- Sürümler: https://github.com/alibaba/open-code-review/releases (son sürüm v1.13.10, 15 Kasım 2026)
- Issues: https://github.com/alibaba/open-code-review/issues
- Video: Haftanın Popüler Repoları, 9–15 Kasım 2026 bölümü (listenin 9. sırası)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W46 bölümü](../bolumler/2026-W46.md) videosu için hazırlandı (video henüz yayında değil). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
