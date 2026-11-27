---
repo: microsoft/markitdown
url: https://github.com/microsoft/markitdown
lisans: MIT
surum: v0.1.9 (2026-10-21)
readme_tarihi: 2026-11-06
dogrulama: denenmedi
bolum: 2026-W48-3
ozet: PDF, Word, PowerPoint, Excel, görsel, ses ve daha birçok dosyayı LLM'lerin okuyacağı Markdown'a çeviren Python aracı.
---

# markitdown — kurulum ve ilk örnek

> **Bu belge, README'nin 6 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

MarkItDown, Microsoft'un hafif Python aracı: dosyaları LLM'lere ve metin analizi hatlarına verilecek Markdown'a çevirir. Başlıkları, listeleri, tabloları ve bağlantıları korumaya odaklanır. Desteklediği biçimler README'ye göre: PDF, PowerPoint, Word, Excel, görseller (EXIF bilgisi ve görsel açıklaması), ses (EXIF bilgisi ve konuşma dökümü), HTML, CSV/JSON/XML gibi metin biçimleri, ZIP (içindekileri tek tek gezer), YouTube adresleri, EPub ve daha fazlası.

Komut satırından, Python API'sinden ya da Docker ile kullanılır; üçüncü taraf eklentileri destekler (örneğin LLM Vision ile OCR ekleyen `markitdown-ocr`). README çıktının insanlar için yüksek sadakatli dönüşüm değil, metin analizi araçları için olduğunu söylüyor. Lisansı MIT.

## Gereksinimler

- Python 3.10 ile 3.14 arası; bağımlılık çakışmasın diye sanal ortam öneriliyor.
- Temel dönüşümler yerelde çalışır. README'ye göre görsel açıklaması ve konuşma dökümü web üzerindeki servislere dayanır; görsel açıklaması için bir LLM istemcisi (örneğin OpenAI) ve anahtarı gerekir.
- İsteğe bağlı: Azure Document Intelligence ya da Azure Content Understanding. İkisi de ücretli Azure API çağrısı yapar (README'deki maliyet notları).
- Docker yolu için Docker.

## Kurulum

Önce sanal ortam (README'nin standart Python yolu):

```bash
python -m venv .venv
source .venv/bin/activate
```

`uv` kullanıyorsan:

```bash
uv venv --python=3.12 .venv
source .venv/bin/activate
# NOTE: Be sure to use 'uv pip install' rather than just 'pip install' to install packages in this virtual environment
```

Anaconda kullanıyorsan:

```bash
conda create -n markitdown python=3.12
conda activate markitdown
```

Sonra pip ile, bütün isteğe bağlı bağımlılıklarla: `pip install 'markitdown[all]'`. Ya da kaynaktan:

```bash
git clone git@github.com:microsoft/markitdown.git
cd markitdown
pip install -e 'packages/markitdown[all]'
```

Yalnız gereken biçimleri kurmak için (örnek yalnız PDF, DOCX ve PPTX):

```bash
pip install 'markitdown[pdf, docx, pptx]'
```

README'deki seçenekler: `[all]`, `[pptx]`, `[docx]`, `[xlsx]`, `[xls]`, `[pdf]`, `[outlook]`, `[az-doc-intel]`, `[az-content-understanding]`, `[audio-transcription]`, `[youtube-transcription]`.

Docker ile (repo klasöründe):

```sh
docker build -t markitdown:latest .
docker run --rm -i markitdown:latest < ~/your-file.pdf > output.md
```

## İlk çalıştırma

Komut satırında bir PDF'i Markdown'a çevir:

```bash
markitdown path-to-file.pdf > document.md
```

Çıktı dosyasını `-o` ile vermek de olur:

```bash
markitdown path-to-file.pdf -o document.md
```

Ya da içeriği boru ile ver:

```bash
cat path-to-file.pdf | markitdown
```

Python'dan:

```python
from markitdown import MarkItDown

md = MarkItDown(enable_plugins=False) # Set to True to enable plugins
result = md.convert("test.xlsx")
print(result.markdown)
```

Eklentiler varsayılan olarak kapalıdır; kurulu eklentileri `markitdown --list-plugins` listeler, `markitdown --use-plugins path-to-file.pdf` eklentilerle çevirir.

Azure Document Intelligence ile (anahtar ve uç nokta senin Azure kaynağından; açılı ayraç içindekiler README'deki yer tutucular):

```bash
export AZURE_API_KEY="<document_intelligence_api_key>"
markitdown path-to-file.pdf -o document.md -d -e "<document_intelligence_endpoint>"
```

`AZURE_API_KEY` tanımlı değilse MarkItDown `DefaultAzureCredential` kullanır.

## Sık takılınan yerler

- Güvenlik: README'nin başındaki uyarıya göre MarkItDown, çalıştığı sürecin yetkileriyle dosya ve ağ erişimi yapar. Güvenilmeyen girdiyi doğrudan verme; sunucu tarafında yalnız gereken dar fonksiyonu çağır (`convert_local()`, `convert_stream()`, `convert_response()`), çünkü `convert()` yerel dosya, uzak adres ve byte akışını kabul eder.
- Yalnız yerelde çalışmak istiyorsan sadece ihtiyacın olan isteğe bağlı bağımlılıkları kur; görsel açıklaması, konuşma dökümü ve iki Azure dönüştürücüsü web servislerine gider.
- `markitdown-ocr` eklentisinde `llm_client` verilmezse eklenti yüklenir ama OCR sessizce atlanır, standart dönüştürücü kullanılır.
- Azure Content Understanding'e giden her `convert()` çağrısı ücretli; hangi biçimlerin gideceğini `cu_file_types` ile sınırla. Document Intelligence'ta `formulas`, `ocrHighResolution`, `styleFont` ve `queryFields` ek ücretli.
- `uv` ortamında `pip install` yerine `uv pip install` kullan (README'deki not).

## Bağlantılar

- Repo: https://github.com/microsoft/markitdown
- PyPI: https://pypi.org/project/markitdown/
- OCR eklentisi: packages/markitdown-ocr/README.md · örnek eklenti: packages/markitdown-sample-plugin · eklenti bulmak için GitHub'da `#markitdown-plugin`
- Azure Content Understanding: https://learn.microsoft.com/azure/ai-services/content-understanding/
- Sürümler: https://github.com/microsoft/markitdown/releases (son sürüm v0.1.9, 21 Ekim 2026)
- Issues: https://github.com/microsoft/markitdown/issues
- Video: Haftanın Popüler Repoları, 25–27 Kasım 2026 Cuma bölümü (listenin 3. sırası)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W48-3 bölümü](../bolumler/2026-W48-3.md) videosu için hazırlandı (video henüz yayında değil). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
