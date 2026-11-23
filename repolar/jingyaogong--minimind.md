---
repo: jingyaogong/minimind
url: https://github.com/jingyaogong/minimind
lisans: Apache-2.0
surum: v2 (2025-10-21)
readme_tarihi: 2026-11-23
dogrulama: denenmedi
bolum: 2026-W47
ozet: Yaklaşık 64 milyon parametrelik küçük bir dil modelini sıfırdan, tek ekran kartında iki saatte eğitmeyi öğreten açık kaynak proje.
---

# minimind — kurulum ve ilk örnek

> **Bu belge, README'nin 23 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

MiniMind, yaklaşık 64 milyon parametrelik çok küçük bir dil modelini tamamen sıfırdan eğitmeyi gösterir. README'ye göre en küçük sürüm GPT-3'ün yaklaşık 2700'de biri boyutunda; "2 saat", SFT aşamasının tek bir NVIDIA 3090'da bir epoch sürmesi, "3 yuan" da o sürenin ekran kartı kira bedeli. Repo yalnız modeli değil bütün eğitim hattını açar: veri temizleme, ön eğitim (pretrain), SFT, LoRA, DPO, PPO / GRPO / CISPO, Tool Use, Agentic RL ve model damıtma. Çekirdek algoritmalar hazır kütüphane soyutlamalarına yaslanmadan doğrudan PyTorch ile yazılmış; proje aynı zamanda LLM'lerin nasıl kurulduğunu öğreten uygulamalı bir ders. Lisansı Apache 2.0.

README'nin aslı Çince; İngilizcesi repodaki `README_en.md` dosyasında. Aşağıdaki komutlar o dosyadan alındı (Çince README'deki komutlarla aynı, yalnız açıklama satırları İngilizce).

## Gereksinimler

README ayrı bir gereksinim listesi vermiyor; yazarın "referans için" paylaştığı kendi ortamı şu: Ubuntu 20.04, Python 3.10.16, CUDA 12.2, NVIDIA GeForce RTX 3090 (24 GB) × 8, 128 GB RAM. README'den çıkanlar:

- Python ve `pip`; bağımlılıklar `requirements.txt` dosyasında. WebUI için README "python>=3.10 gerekebilir" diyor.
- Repoyu ve modeli indirmek için Git.
- Eğitim için CUDA destekli ekran kartı önerilir. CUDA yoksa CPU ya da MPS ile de çalışır, ama README hız ve uyumluluğun belirgin biçimde değişeceğini söylüyor.
- Hazır modeli indirmek için `modelscope` komutu ya da Hugging Face'ten `git clone`.

## Kurulum

Repoyu indir ve bağımlılıkları kur (README'nin "Step 0" adımı; komut paketleri Aliyun PyPI aynasından çeker):

```bash
# Clone repository and install dependencies
git clone --depth 1 https://github.com/jingyaogong/minimind
cd minimind && pip install -r requirements.txt -i https://mirrors.aliyun.com/pypi/simple
```

Hazır modeli proje kök klasörüne indir (iki yoldan biri):

```bash
# Method 1
modelscope download --model gongjy/minimind-3 --local_dir ./minimind-3
# Method 2
git clone https://huggingface.co/jingyaogong/minimind-3
```

Eğitimden önce PyTorch'un ekran kartını gördüğünü doğrulamak için README'nin önerdiği denetim:

```python
import torch
print(torch.cuda.is_available())
```

## İlk çalıştırma

En kısa yol, indirdiğin hazır modelle komut satırında konuşmak:

```bash
# Method 1: Using Transformers format model
python eval_llm.py --load_from ./minimind-3
# Method 2: Based on PyTorch model (ensure corresponding weights are in the ./out directory)
python eval_llm.py --load_from ./model --weight full_sft
```

İsteğe bağlı WebUI (Streamlit). README'ye göre önce model klasörünü `./scripts/` altına kopyalaman gerekir (README'nin örneği: `cp -r minimind-3 ./scripts/minimind-3`) ve `pip install streamlit` gerekebilir:

```bash
cd scripts && streamlit run web_demo.py
```

Ollama ile denemek için: `ollama run jingyaogong/minimind-3`.

Kendi modelini sıfırdan eğitmek için:

1. Veri dosyalarını https://www.modelscope.cn/datasets/gongjy/minimind_dataset/files adresinden indirip `./dataset` klasörüne koy. README'ye göre hızlı deneme için yalnız `pretrain_t2t_mini.jsonl` ve `sft_t2t_mini.jsonl` yeter.
2. Ön eğitim (zorunlu); çıktı `out/pretrain_*.pth`:

```bash
cd trainer && python train_pretrain.py
```

3. SFT, yani talimatla ince ayar (zorunlu); çıktı `out/full_sft_*.pth`:

```bash
cd trainer && python train_full_sft.py
```

4. Eğittiğin modeli dene:

```bash
python eval_llm.py --weight full_sft
```

## Sık takılınan yerler

- WebUI, `./scripts/` altında ağırlık dosyası içeren bir model klasörü bulamazsa hata verir; önce model klasörünü oraya kopyala.
- Eğitim yarıda kesilirse baştan başlama: bütün eğitim betikleri `--from_resume 1` ile kaldığı yerden sürer (örnek: `python train_pretrain.py --from_resume 1`); kayıtlar `./checkpoints/` klasöründe tutulur.
- Birden çok ekran kartı varsa README'nin komutu `torchrun --nproc_per_node N train_xxx.py` (N kart sayısı).
- README'ye göre Haziran 2025'ten sonra Çin'deki ağlardan WandB'ye doğrudan bağlanılamıyor; proje eğitim grafikleri için varsayılan olarak SwanLab kullanıyor.
- README'nin güncelleme notu: 26 Nisan 2025'ten önceki eski modeller artık "doğrudan" yüklenemiyor ve `minimind-v1` serisinin bakımı bırakıldı.
- Kurulum komutu paketleri Aliyun aynasından çeker; README başka bir kaynak için ayrı komut vermiyor.

## Bağlantılar

- Repo: https://github.com/jingyaogong/minimind
- İngilizce README: https://github.com/jingyaogong/minimind/blob/master/README_en.md
- Proje sitesi: https://jingyaogong.github.io/minimind
- Çevrim içi deneme: https://www.modelscope.cn/studios/gongjy/MiniMind
- Modeller: https://huggingface.co/collections/jingyaogong/minimind-66caf8d999f5c7fa64f399e5 · Veri: https://www.modelscope.cn/datasets/gongjy/minimind_dataset/files
- Sürümler: https://github.com/jingyaogong/minimind/releases (son sürüm etiketi v2, 21 Ekim 2025)
- Issues: https://github.com/jingyaogong/minimind/issues · Discussions: https://github.com/jingyaogong/minimind/discussions
- Video: Haftanın Popüler Repoları, 16–22 Kasım 2026 bölümü

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W47 bölümü](../bolumler/2026-W47.md) videosu için hazırlandı (video henüz yayında değil). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
