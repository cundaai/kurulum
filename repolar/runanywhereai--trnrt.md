---
repo: RunanywhereAI/trnrt
url: https://github.com/RunanywhereAI/trnrt
lisans: Apache-2.0
surum: v0.1.0 (2026-11-08)
readme_tarihi: 2026-11-07
dogrulama: denenmedi
bolum: trend-short/t2026-11-23-1
ozet: Büyük dil modellerini AWS Trainium üzerinde çalıştırıp OpenAI uyumlu API ile sunan açık kaynak inference motoru.
---

# trnrt — kurulum ve ilk örnek

> **Bu belge, README'nin 7 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

TrnRT, büyük dil modellerini AWS Trainium üzerinde çalıştıran açık bir inference motoru. README'ye göre tek bir `trn2.48xlarge` makineden OpenAI uyumlu bir API sunar, decode adımını NKI megakernel'leri olarak çalıştırır ve cihaz ile host belleği arasında bir prefix cache tutar. Desteklenen ilk model GLM-5.3-Flash; README başka açık modellerin yolda olduğunu söylüyor. Lisansı Apache-2.0.

Bu araç kendi bilgisayarında denenecek türden değil: sunmak için AWS'de bir Trainium makinesi gerekir, kurulum ve bakım sende.

## Gereksinimler

- Docker.
- Sunmak için bir `trn2.48xlarge` (AWS Trainium2) makine.
- Derleme için herhangi bir x86_64 host (README: NeuronCore gerekmez).
- İmaj Neuron SDK 2.32 (neuronx-cc 2.27) ve Python 3.13 ile kurulur (README'nin 1. adımındaki not).
- Derleme komutları `HF_TOKEN` ortam değişkenini container'a geçirir; modeli indirmek için `hf` komutu kullanılır.
- Trainium'a yeniysen README önce `docs/AWS.md` belgesini öneriyor.

## Kurulum

1. İmajı kur:

```bash
git clone https://github.com/RunanywhereAI/trnrt.git && cd trnrt
docker build -f docker/Dockerfile \
  --build-arg TRNRT_VERSION=$(python3 -c 'import trnrt; print(trnrt.__version__)') \
  --build-arg TRNRT_REVISION=$(git rev-parse HEAD) -t trnrt .
```

2. Derleme cache'ini üret (herhangi bir x86_64 host üzerinde; README'ye göre saatler sürer):

```bash
OUT=/data/trnrt REV=eb9eb208eb0d988989d07a6a12d0fdeb5f52574a
mkdir -p $OUT/nki $OUT/work
trnrt() { docker run --rm -e HF_TOKEN -v $OUT:$OUT \
  -v $OUT/nki:/var/tmp/nki-intermediate-cache trnrt "$@"; }
trnrt compile shapes zai-org/GLM-5.3-Flash --revision $REV --out $OUT
trnrt compile decisions --model zai-org/GLM-5.3-Flash --revision $REV --out $OUT
trnrt compile capture --out $OUT -- --model $OUT/shapes
trnrt compile compile --out $OUT --work-root $OUT/work
trnrt compile strip --out $OUT
```

## İlk çalıştırma

3. Sun (`trn2.48xlarge` üzerinde; `$OUT/cache` ve `$OUT/nki` klasörleri bu makineye kopyalanmış olmalı):

```bash
hf download zai-org/GLM-5.3-Flash --revision eb9eb208eb0d988989d07a6a12d0fdeb5f52574a \
  --local-dir /models/GLM-5.3-Flash
docker run --rm --privileged --network host --ipc=host \
  -v /data/trnrt/cache:/cache \
  -v /data/trnrt/nki:/var/tmp/nki-intermediate-cache \
  -v /models/GLM-5.3-Flash:/model \
  -e NEURON_LIBTORCH_CACHE_ROOT=/cache \
  trnrt serve --model /model --port 8000
```

4. Herhangi bir OpenAI istemcisiyle çağır (README'nin örneği):

```python
from openai import OpenAI

client = OpenAI(base_url="http://localhost:8000/v1", api_key="unused")
reply = client.chat.completions.create(
    model="glm",
    messages=[{"role": "user", "content": "Write a haiku about Trainium."}],
    extra_body={"session_id": "my-session"},  # keeps a conversation on one engine's prefix cache
)
print(reply.choices[0].message.content)
```

README'ye göre her adımın ne gerektirdiği, başlangıç denetimleri, batch 4, 8 ve 12 ayarları ve bütün flag'ler tam kılavuzda: `docs/DEPLOY.md`, `docs/API.md`, `docs/CONFIGURATION.md`.

## Sık takılınan yerler

- Derleme adımı README'ye göre saatler sürer; sunma adımı ise yalnız `trn2.48xlarge` üzerinde çalışır. İki adım farklı makinelerde yapılıyorsa `$OUT/cache` ve `$OUT/nki` klasörlerinin sunan makineye kopyalanmış olması gerekir (README'nin 3. adımı).

## Bağlantılar

- Repo: https://github.com/RunanywhereAI/trnrt
- Yazı (nasıl kuruldu, AgentX sonuçları): https://runanywhere.ai/blog/glm-5-3-flash-trainium2-nki-megakernels
- Belgeler: https://github.com/RunanywhereAI/trnrt/tree/development/docs (AWS.md, DEPLOY.md, API.md, CONFIGURATION.md, ARCHITECTURE.md)
- Sürümler: https://github.com/RunanywhereAI/trnrt/releases (son sürüm v0.1.0, 8 Kasım 2026)
- Issues: https://github.com/RunanywhereAI/trnrt/issues
- Video: Haftanın Popüler Repoları, 23 Kasım 2026 Short'u

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki trend Short t2026-11-23-1 videosu için hazırlandı: https://www.youtube.com/shorts/dHdzS5edkoc. Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
