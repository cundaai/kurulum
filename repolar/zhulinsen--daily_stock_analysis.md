---
repo: ZhuLinsen/daily_stock_analysis
url: https://github.com/ZhuLinsen/daily_stock_analysis
lisans: MIT
surum: v3.34.0 (2026-11-09)
readme_tarihi: 2026-11-13
dogrulama: denenmedi
bolum: trend-short/t2026-11-14-2
ozet: Takip listendeki hisseleri her gün LLM ile analiz edip sonucu Telegram, Slack ya da e-postaya gönderen açık kaynak sistem.
---

# daily_stock_analysis — kurulum ve ilk örnek

> **Bu belge, README'nin 13 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

> README'nin kendi uyarısı: proje yalnız öğrenme ve araştırma içindir, yatırım tavsiyesi değildir; kullanımdan doğan kayıplardan yazar sorumlu değildir. Bu belge de yatırım tavsiyesi değildir.

## Ne işe yarar

Daily Stock Analysis, LLM ile çalışan bir hisse senedi analiz sistemi. README'ye göre Çin A hisseleri, Hong Kong, ABD, Japonya, Kore ve Tayvan borsalarındaki takip listeni (ve ETF'leri) her gün otomatik analiz eder; sonucu "karar panosu" raporu olarak WeChat Work, Feishu, Telegram, Discord, Slack ya da e-postaya gönderir. Rapor özet sonuç, puan, trend, risk uyarıları ve kontrol listesi içerir. Web ve masaüstü çalışma alanı, geçmiş raporlar, backtest ve çok turlu soru soran bir agent modu da var. Varsayılan veri kaynakları AkShare, Baostock ve YFinance gibi ücretsiz kaynaklar; model olarak Gemini, Claude, OpenAI uyumlu servisler (DeepSeek, Qwen) ya da yerel Ollama kullanılabilir. Lisansı MIT. README Çince; İngilizce sürümü docs/README_EN.md.

## Gereksinimler

- En az bir LLM servisinin API anahtarı (örneğin `GEMINI_API_KEY`, `ANTHROPIC_API_KEY` ya da `OPENAI_API_KEY`). README, GitHub Actions için bulut API'sini, Ollama'yı ise yerel ya da Docker kurulumu için öneriyor.
- En az bir bildirim kanalı: Telegram (`TELEGRAM_BOT_TOKEN` + `TELEGRAM_CHAT_ID`), Discord (`DISCORD_WEBHOOK_URL`), Slack (`SLACK_BOT_TOKEN` + `SLACK_CHANNEL_ID`), e-posta (`EMAIL_SENDER` + `EMAIL_PASSWORD`), WeChat Work ya da Feishu webhook'u.
- Zorunlu takip listesi `STOCK_LIST` (örnek biçim README'de: `600519,hk00700,AAPL,7203.T,005930.KS,2330.TW`).
- İsteğe bağlı ama önerilen bir haber arama servisi (SerpAPI, Tavily, Brave, SearXNG ve diğerleri).
- Yerelde çalıştırmak için Python 3.10 ve üstü (README rozeti) ve git; GitHub Actions yolunda sunucu gerekmez.

## Kurulum

**Yol 1: GitHub Actions (README'nin önerdiği, sunucusuz).**

1. Repoyu sağ üstteki `Fork` düğmesiyle kendi hesabına fork'la.
2. Fork'ta `Settings` → `Secrets and variables` → `Actions` → `New repository secret` ile en az bir model anahtarı, en az bir bildirim kanalı ve `STOCK_LIST` secret'larını ekle. Değerleri kendi anahtarlarınla doldur (örneğin `GEMINI_API_KEY` = `<API_ANAHTARI>`).
3. `Actions` sekmesinde `I understand my workflows, go ahead and enable them` ile workflow'ları aç.

**Yol 2: yerelde çalıştırma (README'den olduğu gibi; Çince açıklama satırları: projeyi klonla, bağımlılıkları kur, ortam değişkenlerini ayarla, analizi çalıştır).**

```bash
# 克隆项目
git clone https://github.com/ZhuLinsen/daily_stock_analysis.git && cd daily_stock_analysis

# 安装依赖
pip install -r requirements.txt

# 配置环境变量
cp .env.example .env && vim .env

# 运行分析
python main.py
```

`.env` içine Actions yolundaki değişkenlerin aynısını yaz. Docker kurulumu, zamanlanmış görevler ve bulut sunucuya erişim için README docs/full-guide.md belgesine yönlendiriyor.

## İlk çalıştırma

GitHub Actions yolunda: `Actions` → `每日股票分析` (Günlük hisse analizi) → `Run workflow` → `Run workflow` ile elle bir deneme çalıştır. Sonra varsayılan olarak her iş günü Pekin saatiyle 18:00'de (Türkiye saatiyle 13:00) kendiliğinden çalışır; tatil ve işlem olmayan günlerde çalışmaz.

Yerelde README'deki sık kullanılan komutlar:

```bash
python main.py --debug
python main.py --dry-run
python main.py --stocks 600519,hk00700,AAPL,2330.TW
python main.py --market-review
python main.py --schedule
python main.py --serve-only
```

README bu bayrakların ne yaptığını tek tek anlatmıyor; ayrıntı için docs/full-guide.md.

## Sık takılınan yerler

- Ücretsiz veri kaynakları (AkShare, Baostock, YFinance) kaynak tarafındaki hız sınırlarından, arayüz değişikliklerinden ve ağ dalgalanmalarından etkilenir; README uzun süreli zamanlı ya da toplu analiz için token'lı bir veri kaynağı (Tushare, Longbridge) öneriyor. Günlükte "yapılandırılmadı" uyarısı görmek çalışmayı engellemez.
- Haber kaynağı tanımlamazsan rapordaki haber, duyuru ve olay kısmının kalitesi düşer; README en az bir arama servisi öneriyor.
- Ollama GitHub Actions'ta değil, yerel ya da Docker kurulumunda önerilir.

## Bağlantılar

- Repo: https://github.com/ZhuLinsen/daily_stock_analysis
- İngilizce README: https://github.com/ZhuLinsen/daily_stock_analysis/blob/main/docs/README_EN.md
- Tam kılavuz: https://github.com/ZhuLinsen/daily_stock_analysis/blob/main/docs/full-guide.md
- LLM yapılandırma kılavuzu: https://github.com/ZhuLinsen/daily_stock_analysis/blob/main/docs/LLM_CONFIG_GUIDE.md
- Site: https://dsa.zhulinsen.tech
- Sürümler: https://github.com/ZhuLinsen/daily_stock_analysis/releases (son sürüm v3.34.0, 9 Kasım 2026)
- Issues: https://github.com/ZhuLinsen/daily_stock_analysis/issues
- Video: Haftanın Popüler Repoları, 14 Kasım 2026 trend Short'u

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki trend Short t2026-11-14-2 videosu için hazırlandı: https://www.youtube.com/shorts/KgYp1bvutxc. Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
