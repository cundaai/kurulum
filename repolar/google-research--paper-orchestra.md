---
repo: google-research/paper-orchestra
url: https://github.com/google-research/paper-orchestra
lisans: Apache-2.0
surum: sürüm etiketi yok
readme_tarihi: 2026-05-17
dogrulama: denenmedi
bolum: 2026-W48-3
ozet: Fikir notlarını ve deney kayıtlarını, literatür taraması ve grafikleriyle gönderime hazır LaTeX makalesine çeviren çok agent'lı araştırma çerçevesi.
---

# paper-orchestra — kurulum ve ilk örnek

> **Bu belge, README'nin 17 Mayıs 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

PaperOrchestra, Google Cloud AI Research ekibinin AI araştırma makalesi yazmayı otomatikleştiren çok agent'lı çerçevesi. Serbest biçimli ön malzemeyi (fikir ve deney kayıtları) alır, gönderime hazır bir LaTeX makalesine çevirir. İşi uzmanlaşmış agent'lardan oluşan bir ekip gibi böler: taslak (outline), literatür taraması, bölüm yazımı, içerik iyileştirme ve grafik çizimi. İlgili literatürü kendisi arar, önceliklendirir ve metne yerleştirir; hedef konferansın LaTeX şablonuna uyar (örneğin `templates/cvpr2025`).

Bir araştırma kodu: README'nin sonunda "resmî olarak desteklenen bir Google ürünü değildir" notu var, makalesi arXiv'de (2604.05018). Lisansı Apache-2.0.

## Gereksinimler

- Conda ve Python 3.11 (README `paper_orchestra` adlı bir Conda ortamı kuruyor).
- Kullanacağın model için API anahtarı: Gemini modelleri için ya `VERTEX_AI_PROJECT` ve `VERTEX_AI_LOCATION` ya da `GEMINI_API_KEY`; OpenAI modelleri için `OPENAI_API_KEY`. Bunlar ücretli model çağrılarıdır.
- README `.env` örneğinde bir Semantic Scholar API anahtarı da geçiyor (`SEMANTIC_SCHOLAR_API_KEY`).
- Kendi ham malzemen (fikir ve deney kaydı) ve bir LaTeX şablonu. README'ye göre makale üretiminde kullanılan veri seti bu depoda yok, daha sonra ayrıca yayımlanacak.

## Kurulum

README repoyu klonlama adımını yazmıyor; komutlar repo klasöründe çalıştırılıyor (repo: https://github.com/google-research/paper-orchestra).

1. Ortamı kur:

```bash
conda create -n paper_orchestra python=3.11
conda activate paper_orchestra
pip install -r requirements.txt
```

2. Proje kökünde bir `.env` dosyası oluştur ve anahtarlarını ekle. README'deki alan adları aşağıda; değerler yer tutucu, elinde olmayanları boş bırak:

```env
SEMANTIC_SCHOLAR_API_KEY=<API_ANAHTARI>
OPENAI_API_KEY=<API_ANAHTARI>
VERTEX_AI_PROJECT=<VERTEX_PROJE_KIMLIGI>
VERTEX_AI_LOCATION=<VERTEX_BOLGESI>
GEMINI_API_KEY=<API_ANAHTARI>
```

README'ye göre demo sonunda kendine e-posta göndermek istersen `SMTP_EMAIL` ve `SMTP_PASSWORD` alanları da gerekir; bunlar isteğe bağlı.

## İlk çalıştırma

Hazır bash betiğiyle:

```bash
./paper_writing_cli.sh
```

Ya da doğrudan Python CLI ile:

```bash
python paper_writing_cli.py \
  --raw_materials_dir /path_to_your_raw_materials/ \
  --latex_template_dir /path_to_your_latex_template/ \
  --output_dir /path_to_your_output_folder/ \
  --use_plotting true
```

Argümanlar (README):

- `--raw_materials_dir`: (zorunlu) ham malzemenin (fikir, deney kaydı) olduğu klasör.
- `--latex_template_dir`: (zorunlu) LaTeX şablonunun klasörü, örneğin `templates/cvpr2025`.
- `--output_dir`: çıktı klasörü; verilmezse zaman damgalı bir klasör açılır.
- `--use_plotting`: koddan grafik üreten agent akışını açar (varsayılan `False`).
- `--writer_model_name`: yazar ve literatür agent'larının LLM'i; `--reflection_model_name`: değerlendirme (reflection) agent'larının LLM'i.

Etkileşimli bir Streamlit demosu da var; nasıl çalıştırılacağı `frontend/README.md` dosyasında.

## Sık takılınan yerler

- Makale üretiminde kullanılan veri seti depoda yok; yayımlandığında indirilip `datasets/` klasörüne konacak (README). O zamana kadar kendi ham malzemenle çalışırsın.
- `.env` dosyasında yalnız kullanacağın modelin anahtarları gerekli; Gemini için Vertex AI ikilisi ya da `GEMINI_API_KEY`'den biri yeterli.
- Kendi konferans şablonunu eklemek için `templates/` altında yeni bir klasör aç (README'deki proje yapısı).

## Bağlantılar

- Repo: https://github.com/google-research/paper-orchestra
- Proje sayfası: https://yiwen-song.github.io/paper_orchestra/
- Makale: arXiv 2604.05018 (https://arxiv.org/abs/2604.05018)
- Streamlit demosu: frontend/README.md
- Sürüm etiketi yok; issues: https://github.com/google-research/paper-orchestra/issues
- Video: Haftanın Popüler Repoları, 25–27 Kasım 2026 Cuma bölümü (listenin 5. sırası)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W48-3 bölümü](../bolumler/2026-W48-3.md) videosu için hazırlandı (yakında — 27 Kas 12:30). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
