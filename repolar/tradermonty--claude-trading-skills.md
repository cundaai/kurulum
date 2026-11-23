---
repo: tradermonty/claude-trading-skills
url: https://github.com/tradermonty/claude-trading-skills
lisans: MIT
surum: sürüm etiketi yok
readme_tarihi: 2026-11-15
dogrulama: denenmedi
bolum: 2026-W47
ozet: Bireysel yatırımcının piyasa incelemesi, risk yönetimi, işlem planı ve günlük tutma sürecini düzenleyen Claude skill koleksiyonu; yatırım tavsiyesi değildir.
---

# claude-trading-skills — kurulum ve ilk örnek

> **Bu belge, README'nin 15 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

Claude Trading Skills, zamanı kısıtlı bireysel yatırımcılar için Claude Skills üzerine kurulu bir iş akışı araç takımıdır. README'ye göre çekirdeği uzun vadeli yatırım, ETF ve temettü hisseleri olan, piyasa uygun olduğunda disiplinli swing trading'i ek strateji olarak kullanan yatırımcılar için tasarlandı. Amaç al/sat kararını yapay zekâya bırakmak değil; piyasa incelemesini, risk yönetimini, işlem planını, günlük tutmayı ve sürekli iyileştirmeyi düzene sokmak. Skill'ler piyasa rejimi, portföy, temettü, swing fırsatları, işlem planı, işlem hafızası ve strateji araştırması alanlarına ayrılır.

**Uyarı (README'nin "Disclaimer" bölümü): bu repo yalnız eğitim, araştırma ve süreç iyileştirme amaçlıdır. Yatırım tavsiyesi, yatırım danışmanlığı, vergi ya da hukuk tavsiyesi, sinyal servisi ya da emir ileten bir aracı kurum platformu değildir.** Alım satım ve yatırım, anaparanın kaybı dahil risk içerir. Geçmiş performans, backtest'ler, taramalar, raporlar ve yapay zekânın ürettiği analizler gelecekteki sonuçları garanti etmez. Bütün işlem kararları, pozisyon büyüklüğü, vergi ve mevzuat uyumu ile aracı kurum kullanımı kullanıcının sorumluluğundadır. README ayrıca bunun bir kârlılık vaadi olmadığını; tam otomatik işlem, sinyal dışarıdan alma ya da kısa vadeli scalping için tasarlanmadığını yazar. Proje MIT lisansıyla, olduğu gibi ve garantisiz sunulur.

## Gereksinimler

- Claude web uygulaması ya da Claude Code. README'ye göre web'deki Skills özelliği Free, Pro, Max, Team ve Enterprise hesaplarda var (erişim değişebilir); Claude Code'un hesap koşulları ayrıdır ve Claude.ai Free planı onu içermez.
- Web uygulamasında bireysel hesap için **Settings > Capabilities** altında **Code execution and file creation** açık olmalı; Team ve Enterprise'da bunu kuruluş sahibinin açması gerekebilir.
- Python: ortak proje ortamı Linux, Windows ve macOS'ta 3.11–3.13 destekler; paketlenmiş tek skill'lerin alt sınırı 3.9.
- API anahtarları isteğe bağlıdır: FMP (ücretsiz katman günde 250 istek), FINVIZ Elite (ücretli abonelik) ve Alpaca (ücretsiz paper trading hesabı var; Alpaca MCP Server ayarı ister) yalnız belirli iş akışları için gerekir. Beş skill'lik başlangıç yolu ücretli veri API'ı istemez.

## Kurulum

Claude Code (masaüstü ya da CLI), README'deki adımlar:

1. Repoyu klonla ya da indir.
2. İstediğin skill klasörünü (örneğin `backtest-expert`) kişisel kullanım için `~/.claude/skills/` altına, ya da bir projenin içindeki `.claude/skills/` altına kopyala. Skill klasörleri repoda `skills/<skill-name>/` altındadır.
3. Claude Code var olan bir skills klasöründeki değişikliği kendisi görür; üst düzey skills klasörünü oturum başladıktan sonra oluşturduysan yeniden başlat.

README bu adımlar için `git clone` ya da `cp` komutu vermiyor; yalnız adımları anlatıyor.

Claude web uygulaması:

1. Son GitHub Release'inden istediğin skill'in `.skill` dosyasını ve `SHA256SUMS` dosyasını indir; hash'i release policy belgesine göre doğrula. Releases sayfası boşsa klasör kopyalama yolunu izle ya da `scripts/package_skills.py` ile yerelde paket üret.
2. **Customize > Skills** bölümünü aç, ZIP'i yükle, listede göründüğünü doğrula ve gerekirse etkinleştir.

Bir skill'i özelleştirdiysen web uygulamasına yüklemeden önce yeniden paketle:

```bash
python3 scripts/package_skills.py --skill <skill-name>
```

API kullanan skill'ler için anahtarlar ortam değişkeniyle verilir (README'deki örnek değerler yerine kendi anahtarını yaz):

```bash
export FMP_API_KEY=<API_ANAHTARI>
export FINVIZ_API_KEY=<API_ANAHTARI>
```

## İlk çalıştırma

README tek bir "ilk komut" vermiyor; yeni başlayanlara hazır iş akışlarından biriyle başlamayı öneriyor ve ayrıntı için `docs/en/your-first-week.md` ile `docs/en/offline-quickstart.md` belgelerine yönlendiriyor. API anahtarın yoksa README'nin "No API Key Starter Path" listesi:

1. `market-breadth-analyzer`: herkese açık CSV ile piyasa genişliği puanı.
2. `uptrend-analyzer`: herkese açık CSV ile yükseliş trendine katılım.
3. `position-sizer`: yalnız hesaplama.
4. `trader-memory-core`: yerel YAML ile işlem günlüğü.
5. `signal-postmortem`: sonuç inceleme çerçevesi.

Bu beş skill'i kurup elle sırayla çalıştırırsın. İlk hedef olarak README 15 dakikalık günlük piyasa kontrolünü gösterir: `market-regime-daily` iş akışı (`workflows/market-regime-daily.yaml`; `market-breadth-analyzer`, `uptrend-analyzer`, `exposure-coach`). Bu akışın çıktısı emir değil, insanın gözden geçireceği bir duruş özetidir. Hangi akışın sana uyduğunu `trading-skills-navigator` skill'i önerir.

## Sık takılınan yerler

- "No API" dış veri gerekmediği anlamına gelmez: bu skill'ler yine de herkese açık CSV, grafik ekran görüntüsü ya da yerel dosya ister. Kesin girdi listesi `skills-index.yaml` içindeki `integrations:` alanındadır.
- README ile belgeler çelişirse `skills-index.yaml` doğrudur; çoklu skill akışlarında `workflows/*.yaml` esastır.
- `core-portfolio-weekly` akışı Alpaca ister; elle CSV daha zayıf bir yedek yoldur.
- `ALPACA_PAPER` değişkeni `"true"` iken paper trading, `"false"` iken gerçek hesap kullanılır; README portföy akışında hangi işlemin yapılacağına elle karar verilmesini söyler.
- README'deki örnek çıktılar kurgusal veriyle hazırlanmıştır; canlı piyasa okuması ya da işlem talimatı değildir.
- Kurulu bir skill'i güncellemeden önce README, Changelog'a ve release compatibility belgesine bakmayı ister.

## Bağlantılar

- Repo: https://github.com/tradermonty/claude-trading-skills
- Belge sitesi: https://tradermonty.github.io/claude-trading-skills/
- İlk hafta rehberi: https://github.com/tradermonty/claude-trading-skills/blob/main/docs/en/your-first-week.md · SSS: https://github.com/tradermonty/claude-trading-skills/blob/main/docs/en/faq.md
- İş akışları: https://github.com/tradermonty/claude-trading-skills/blob/main/workflows/README.md
- Sürümler: https://github.com/tradermonty/claude-trading-skills/releases/latest (GitHub API bu belge yazılırken sürüm etiketi döndürmedi)
- Issues: https://github.com/tradermonty/claude-trading-skills/issues
- Video: Haftanın Popüler Repoları, 16–22 Kasım 2026 bölümü

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W47 bölümü](../bolumler/2026-W47.md) videosu için hazırlandı (yakında — 23 Kas 12:30). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
