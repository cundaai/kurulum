---
repo: google/elephant-goldfish-model
url: https://github.com/google/elephant-goldfish-model
lisans: Apache-2.0
surum: v0.1.1 (2026-11-06)
readme_tarihi: 2026-11-10
dogrulama: denenmedi
bolum: 2026-W46
ozet: Coding agent'larla yazılım tasarımı yapıp tasarım belgesini önceki konuşmayı bilmeyen taze gözlerle inceleten açık kaynak plugin.
---

# elephant-goldfish-model — kurulum ve ilk örnek

> **Bu belge, README'nin 10 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

EGM (Elephant Goldfish Model), AI asistanları için açık kaynak bir plugin. "Fil" (elephant) projenin bağlamını aklında tutar ve tasarım belgesini (spec) taslak olarak yazar; "japon balıkları" (goldfish) ise önceki konuşmayı bilmeden taslağı ve başvurulan kaynak kodu inceler, böylece söylenmemiş varsayımlar ve kör noktalar ortaya çıkar. Mimari kararları ve spec'in onayını sen verirsin, sonra uygulamaya geçilir. Hazır akış yazılım spec'leri için; başka belgeler için `/egm-review-loop` ile inceleyicileri ve standartları seçebilirsin. Lisansı Apache-2.0.

## Gereksinimler

- Desteklenen bir coding agent: Claude Code, Codex CLI, Antigravity CLI (`agy`), ChatGPT ya da Codex masaüstü uygulaması; Antigravity App, Antigravity for IDEs, Claude Cowork ve ChatGPT Work için repodaki kurulum belgesi (docs/harnesses.md).
- README, plugin'in tamamının kurulmasını istiyor; skill'ler inceleyici istemlerini ve belge standartlarını oradan okur.

## Kurulum

Claude Code:

```sh
claude plugin marketplace add google/elephant-goldfish-model
claude plugin install egm@egm
```

Codex CLI:

```sh
codex plugin marketplace add google/elephant-goldfish-model
codex plugin add egm@egm
```

Antigravity CLI:

```sh
agy plugin install https://github.com/google/elephant-goldfish-model
```

ChatGPT ve Codex masaüstü: yayımlanmış EGM plugin sayfasını masaüstü uygulamasında açıp kur (bu liste yalnız masaüstü için). Antigravity App ve Antigravity for IDEs için `/plugin` çalıştır ya da docs/harnesses.md'deki adımlara bak. Kurulumdan sonra yeni bir oturum başlat.

## İlk çalıştırma

README'nin "Quickstart" kısmı iki yol öneriyor:

1. **Var olan kod üstünde 60 saniyelik inceleme:** elindeki bir tasarım belgesini, README'yi ya da Markdown olarak kaydedilmiş bir PR planını canlı koda karşı inceletirsin; yeni spec yazman gerekmez. Taze inceleyiciler tasarımı, açıklığını, koda uyumunu ve uygulama planını kontrol eder. Bir klasördeki belgeleri güncel kaynak koda karşılaştırıp ayrıştıkları yerleri raporlayan bir "drift" denetimi de var; belgeyi mi kodu mu güncelleyeceğine sen karar verirsin.
2. **İlk tasarım:** projeni coding agent'ında aç ve tasarım kararı gerektiren bir özellik seç; fil taslağı yazar, japon balıkları inceler.

Antigravity (`agy`) ve Claude Code'da komutlar `/egm:` önekiyle çağrılır; Codex'te skill seçiciden ilgili EGM skill'ini seçersin. Tam komut adları bu belgenin hammaddesinde yer almadığı için buraya yazılmadı; README'nin Quickstart bölümüne bak.

## Bağlantılar

- Repo: https://github.com/google/elephant-goldfish-model
- Site: https://egm.dev
- Agent'a göre kurulum: https://github.com/google/elephant-goldfish-model/blob/main/docs/harnesses.md
- Sürümler: https://github.com/google/elephant-goldfish-model/releases (son sürüm v0.1.1, 6 Kasım 2026)
- Issues: https://github.com/google/elephant-goldfish-model/issues
- Video: Haftanın Popüler Repoları, 9–15 Kasım 2026 bölümü (listenin 2. sırası)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W46 bölümü](../bolumler/2026-W46.md) videosu için hazırlandı (video henüz yayında değil). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
