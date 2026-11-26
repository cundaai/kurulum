---
repo: openvinotoolkit/openvino
url: https://github.com/openvinotoolkit/openvino
lisans: Apache-2.0
surum: 2026.4.1 (2026-10-01)
readme_tarihi: 2026-08-26
dogrulama: denenmedi
bolum: 2026-W48-2
ozet: Derin öğrenme modellerini optimize edip CPU, GPU ve NPU üzerinde hızlı çalıştıran açık kaynak inference araç takımı.
---

# openvino — kurulum ve ilk örnek

> **Bu belge, README'nin 26 Ağustos 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

OpenVINO, derin öğrenme modellerini optimize etmek ve yayına almak için açık kaynak bir araç takımı. README'ye göre bilgisayarlı görü, konuşma tanıma, üretken yapay zekâ ve büyük ya da küçük dil modelleriyle doğal dil işleme gibi işlerde inference başarımını artırır. PyTorch, TensorFlow, ONNX, Keras, PaddlePaddle ve JAX/Flax ile eğitilmiş modelleri kullanır; Hugging Face Hub'daki transformers ve diffusers modelleri Optimum Intel ile doğrudan bağlanır; modeller özgün framework olmadan dönüştürülüp çalıştırılabilir. CPU (x86, ARM), GPU (Intel tümleşik ve ayrık GPU) ve yapay zekâ hızlandırıcılarında (Intel NPU) çalışır. API'leri C++, Python, C ve NodeJS için var; üretken modeller için ayrıca GenAI API sunar. Lisansı Apache 2.0.

## Gereksinimler

- Hızlı kurulum `pip` ile yapılır; yani Python ve `pip` gerekir. README hangi Python sürümlerinin desteklendiğini yazmıyor; sistem gereksinimleri ve desteklenen cihazlar için belgelerdeki sayfalara yönlendiriyor (bağlantılar aşağıda).
- Donanım: CPU (x86, ARM); isteğe bağlı olarak Intel tümleşik ya da ayrık GPU ve Intel NPU.
- README'deki örnekler için ilgili framework ayrıca kurulu olmalı: PyTorch örneği `torch` ve `torchvision`, TensorFlow örneği `tensorflow` ve `numpy` içe aktarıyor. README bu paketlerin kurulum komutunu vermiyor.

## Kurulum

README'deki hızlı kurulum komutu:

```sh
pip install -U openvino
```

Başka dağıtımlar (README'nin rozetlerinde PyPI, npm `openvino-node`, conda-forge ve Homebrew görünüyor) için README komut yazmıyor; "Get your preferred distribution of OpenVINO" bağlantısıyla belgelerdeki kurulum sayfasına yönlendiriyor: https://docs.openvino.ai/2026/get-started/install-openvino.html

Kurulumu doğrulamak için README'deki Python parçası:

```python
import openvino as ov
print(ov.__version__)
```

Üretken yapay zekâ için OpenVINO GenAI ayrı kurulur; README komutunu vermiyor, belgelerdeki GenAI kurulum sayfasına bağlıyor. Kaynaktan derleme README'de anlatılmıyor; depodaki `docs/dev/build.md` belgesinde.

## İlk çalıştırma

README, PyTorch ve TensorFlow modelleri için iki kısa örnek veriyor. PyTorch modeli: model belleğe yüklenir, OpenVINO modeline dönüştürülür, CPU için derlenir ve rastgele veriyle çalıştırılır.

```python
import openvino as ov
import torch
import torchvision

# load PyTorch model into memory
model = torch.hub.load("pytorch/vision", "shufflenet_v2_x1_0", weights="DEFAULT")

# convert the model into OpenVINO model
example = torch.randn(1, 3, 224, 224)
ov_model = ov.convert_model(model, example_input=(example,))

# compile the model for CPU device
core = ov.Core()
compiled_model = core.compile_model(ov_model, 'CPU')

# infer the model on random data
output = compiled_model({0: example.numpy()})
```

TensorFlow modeli için aynı akış:

```python
import numpy as np
import openvino as ov
import tensorflow as tf

# load TensorFlow model into memory
model = tf.keras.applications.MobileNetV2(weights='imagenet')

# convert the model into OpenVINO model
ov_model = ov.convert_model(model)

# compile the model for CPU device
core = ov.Core()
compiled_model = core.compile_model(ov_model, 'CPU')

# infer the model on random data
data = np.random.rand(1, 224, 224, 3)
output = compiled_model({0: data})
```

İki örnek de modelin ağırlıklarını ilk çalıştırmada internetten indirir ve sonucu ekrana yazdırmaz; `output` değişkeninde kalır. Gerçek bir modeli adım adım yayına almak için README "OpenVINO Quickstart example" sayfasına, hazır örnekler için OpenVINO Notebooks reposuna yönlendiriyor (LLM sohbet botu, YOLOv11 optimizasyonu, metinden görüntü üretme, LLaVa ile çok kipli asistan, Whisper ile konuşma tanıma).

## Sık takılınan yerler

- README bir sorun giderme bölümü içermiyor; destek için GitHub Issues, Intel DevHub Discord sunucusu ve Stack Overflow'daki `openvino` etiketi gösteriliyor.
- Telemetri: README'ye göre OpenVINO, araçlarını geliştirmek için yazılım başarımı ve kullanım verisi toplar (doğrudan ya da Google Analytics 4 ile). İstediğin an çıkmak için README'deki komut: `opt_in_out --opt_out`.
- README'deki örnekler modeli `'CPU'` cihazı için derler; GPU ya da NPU kullanımı README'de komutla gösterilmiyor, belgelerdeki cihazlar sayfasına bağlanıyor.

## Bağlantılar

- Repo: https://github.com/openvinotoolkit/openvino
- Belgeler: https://docs.openvino.ai · kurulum: https://docs.openvino.ai/2026/get-started/install-openvino.html · ilk örnek: https://docs.openvino.ai/2026/get-started.html
- Sistem gereksinimleri: https://docs.openvino.ai/2026/about-openvino/release-notes-openvino/system-requirements.html · desteklenen cihazlar: https://docs.openvino.ai/2026/documentation/compatibility-and-support/supported-devices.html
- Örnekler: https://github.com/openvinotoolkit/openvino_notebooks · GenAI: https://github.com/openvinotoolkit/openvino.genai
- Sürümler: https://github.com/openvinotoolkit/openvino/releases (son sürüm 2026.4.1, 1 Ekim 2026) · sürüm notları: https://docs.openvino.ai/2026/about-openvino/release-notes-openvino.html
- Issues: https://github.com/openvinotoolkit/openvino/issues · Discord: https://discord.gg/7pVRxUwdWG
- Video: Haftanın Popüler Repoları, 23–25 Kasım 2026 bölümü (listenin 6. sırası)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W48-2 bölümü](../bolumler/2026-W48-2.md) videosu için hazırlandı: https://www.youtube.com/watch?v=2wDhRoLOWh8. Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
