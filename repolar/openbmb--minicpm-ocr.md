---
repo: OpenBMB/MiniCPM-OCR
url: https://github.com/OpenBMB/MiniCPM-OCR
lisans: Apache-2.0
surum: sdk-v1.0.5 (2026-11-09)
readme_tarihi: 2026-11-16
dogrulama: denenmedi
bolum: trend-short/t2026-11-27-1
ozet: İngilizce ve Çince belgeleri, PDF'leri ve görselleri Markdown ya da JSON'a çeviren, sonucun güvenilmez bölgelerini ayrı bir modelle puanlayan açık kaynak OCR model ailesi.
---

# MiniCPM-OCR — kurulum ve ilk örnek

> **Bu belge, README'nin 16 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

MiniCPM-OCR iki modelden oluşur. **MiniCPM-OCR 1.0** (1,3 milyar parametre) İngilizce ve Çince belgeleri ve PDF'leri okuyup metin, tablo, formül, grafik ve mühürleri Markdown ya da JSON olarak çıkarır; README'ye göre taranmış belgelerde, fotoğraflarda, kıvrık sayfalarda ve ekrandan yeniden çekilmiş belgelerde de çalışır, belgeden anahtar bilgi çıkarmayı (KIE) destekler. **MiniCPM-OCR Jev** (2 milyar parametre) bir görseli ve onun OCR sonucunu alıp 0 ile 1 arası bir kalite puanı verir; böylece hangi bölgenin elle kontrol edilmesi gerektiği görünür. Jev yalnız MiniCPM-OCR'ın değil, başka OCR modellerinin sonuçlarını da puanlayabilir. Modeller vLLM, SGLang, llama.cpp ve Ollama ile kendi donanımında çalışır; README'ye göre yerel kurulumda belgeler buluta gönderilmez. Kod, belgeler ve model ağırlıkları Apache 2.0 lisanslı.

## Gereksinimler

Yerel kurulum öğreticisinin (docs/minicpm_ocr_tutorial_en.md) verdiği değerler:

- İşletim sistemi: Linux x86_64 (vLLM'i kaynaktan derlemek için gerekli); öğreticideki komutlar Ubuntu 22.04 içindir.
- NVIDIA GPU (öğretici komutları RTX 5090 32 GB üzerinde doğrulanmış) ve en az 580 sürümlü NVIDIA sürücüsü (CUDA 13); daha eski sürücüler için FAQ'daki CUDA uyumluluk kütüphanesi adımı.
- Python 3.12: `uv` kendisi indirir, önceden kurman gerekmez.
- Disk: en az 40 GB boş yer (SDK ortamı, vLLM ortamı, derleme dosyaları ve modeller).
- Docker ile kurmak istersen: Linux x86-64, NVIDIA GPU, CUDA 12.9 ile uyumlu sürücü ve NVIDIA Container Toolkit kurulu Docker (README).
- GPU'n yoksa: README'deki Free API ya da telefondaki demo uygulaması (iOS, Android, HarmonyOS) seçenekleri var.

## Kurulum

README kurulumu dört yoldan anlatıyor; komutların hepsi bağlantılı belgelerde.

**1. Coding agent ile (README'nin önerdiği yol).** Agent'ına şu talimatı ver; skill ortamı kurar, modelleri indirir ve ilk belgeyi ayrıştırır:

```text
Read and execute https://github.com/OpenBMB/MiniCPM-OCR/blob/main/docs/SKILL_en.md
```

Jev puanlama servisi için ayrı skill:

```text
Read and execute https://github.com/OpenBMB/MiniCPM-OCR/blob/main/docs/SKILL_jev_en.md
```

**2. Elle yerel kurulum (öğretici).** Komutlar reponun kök klasöründe çalışır. Önce SDK ortamı:

```bash
pip install uv
uv venv .minicpm-ocr --python 3.12 --seed
source .minicpm-ocr/bin/activate
python -m pip install --upgrade "minicpm-ocr[all]"
python -m pip install "paddlepaddle-gpu==3.3.0" \
  -i https://www.paddlepaddle.org.cn/packages/stable/cu129/
```

Sonra çıkarım (inference) ortamı; öğretici vLLM kullanır ve script vLLM'i kaynaktan derler (öğreticiye göre yaklaşık 40 dakika sürebilir):

```bash
apt-get update
apt-get install -y software-properties-common
apt-get update
add-apt-repository -y ppa:ubuntu-toolchain-r/test
apt-get install -y gcc-13 g++-13 libdw-dev libelf-dev
bash scripts/install.sh vllm
```

SGLang, llama.cpp, Ollama ya da FlagOS kullanacaksan öğreticinin "Other inference frameworks" tablosundaki ayrı rehberlere bak (docs/sglang_deploy.md, docs/llamacpp_deploy.md, docs/ollama_deploy.md, docs/flagos_deploy_en.md).

**3. Docker.** Resmî Docker imajı model ağırlıklarını, SDK'yı, vLLM'i ve belge ayrıştırma backend'ini birlikte getirir; başlatma adımları imajın Docker Hub sayfasında: https://hub.docker.com/repository/docker/openbmbofficial/minicpm-ocr/general

**4. Kurmadan.** Free API (https://platform.modelbest.ai/console/docs/api/document-parse) ya da telefon demo uygulaması (iOS için TestFlight, Android ve HarmonyOS için MiniCPM-V-Apps sürümleri).

## İlk çalıştırma

Öğreticinin vLLM yolu, iki terminalle:

1. Yeni bir terminalde, reponun kök klasöründe tanıma servisini başlat (DSpark hızlandırmasıyla):

```bash
source .venv-vllm/bin/activate
export CC=gcc-13 CXX=g++-13
export CUDA_HOME="$(python -c \
  'import sysconfig; print(sysconfig.get_path("purelib") + "/nvidia/cu13")')"
export PATH="$CUDA_HOME/bin:$PATH"
vllm serve openbmb/MiniCPM-OCR-1.0 \
  --served-model-name minicpm-ocr \
  --host 127.0.0.1 \
  --port 18499 \
  --max-model-len 40960 \
  --max-num-batched-tokens 40960 \
  --max-num-seqs 32 \
  --limit-mm-per-prompt '{"image":36,"video":0}' \
  --attention-backend FLASH_ATTN \
  --mm-encoder-attn-backend FLASH_ATTN \
  --mm-processor-kwargs '{"downsample_mode":"4x"}' \
  --gpu-memory-utilization 0.85 \
  --trust-request-mm-kwargs \
  --speculative-config '{
    "method": "dspark",
    "model": "openbmb/MiniCPM-OCR-1.0-DSpark",
    "num_speculative_tokens": 15,
    "enable_adaptive_verification": false
  }' \
  --default-chat-template-kwargs '{"enable_thinking":false}'
```

   Taslak model olmadan normal çözümleme istersen `--speculative-config` satırını ve JSON'unu tümüyle çıkar (öğretici).

2. Öbür terminalde bir PDF'i Markdown'a çevir (GPU ile yerleşim tespiti):

```bash
source .minicpm-ocr/bin/activate
minicpmvocr parse \
  --device cuda:0 \
  --server-url http://127.0.0.1:18499 \
  --model minicpm-ocr \
  --input doc.pdf \
  --output doc.md
```

   SDK ilk çalıştırmada `models/PP-DocLayoutV3_infer/` klasörüne bakar; yerleşim modeli yoksa kendisi indirir. Başka seçenekler için docs/minicpm_ocr_sdk_api_en.md.

## Sık takılınan yerler

Öğreticinin FAQ tablosundan:

- Üçüncü taraf kaynak hazırlığı başarısız olursa: GitHub erişimini ya da `sources/` altındaki mevcut kaynakları kontrol et, sonra `bash scripts/install.sh vllm` komutunu yeniden çalıştır.
- "Unsupported `minicpm_ocr` architecture" hatası: `bash scripts/install.sh vllm` komutunu yeniden çalıştırıp uyumlu Transformers kaynağını kur.
- vLLM derlemesi bellek yetmediği için kesilirse: `MAX_JOBS` değerini düşür ve derleme için yeterli bellek olduğunu kontrol et.
- GPU belleği yetmezse: `--max-model-len` değerini, gerekirse `--gpu-memory-utilization` değerini düşür.
- SDK istekleri başarısız olur ya da bekleyip durursa: önce `/v1/models` endpoint'ine bak, servis adresini doğrula ve vLLM başlatma komutunda `--trust-request-mm-kwargs` kalsın.
- 580'den eski NVIDIA sürücüsü: FAQ'daki CUDA uyumluluk kütüphanesi ortam değişkenlerini vLLM'i kuracağın ve başlatacağın her terminalde ayarla.

## Bağlantılar

- Repo: https://github.com/OpenBMB/MiniCPM-OCR
- Model rehberleri: docs/minicpm_ocr_1dot0_en.md · docs/minicpm_ocr_jev_en.md
- Yerel kurulum öğreticileri: docs/minicpm_ocr_tutorial_en.md · docs/minicpm_ocr_jev_tutorial_en.md · web demo: docs/webdemo_deploy.md
- Model ağırlıkları: https://huggingface.co/openbmb/MiniCPM-OCR-1.0 · https://huggingface.co/openbmb/MiniCPM-OCR-Jev
- pip SDK: https://pypi.org/project/minicpm-ocr/
- Docker imajı: https://hub.docker.com/repository/docker/openbmbofficial/minicpm-ocr/general
- Free API belgeleri: https://platform.modelbest.ai/console/docs/api/document-parse · https://platform.modelbest.ai/console/docs/api/ocr-jev
- Telefon demosu: https://github.com/OpenBMB/MiniCPM-V-Apps
- SSS: docs/faq_en.md · Issues: https://github.com/OpenBMB/MiniCPM-OCR/issues
- Sürümler: https://github.com/OpenBMB/MiniCPM-OCR/releases (son sürüm sdk-v1.0.5, 9 Kasım 2026)
- Video: Haftanın Popüler Repoları, 27 Kasım 2026 Short'u

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki trend Short t2026-11-27-1 videosu için hazırlandı (video henüz yayında değil). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
