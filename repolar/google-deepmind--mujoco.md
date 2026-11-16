---
repo: google-deepmind/mujoco
url: https://github.com/google-deepmind/mujoco
lisans: Apache-2.0
surum: 4.0.1 (2026-11-11)
readme_tarihi: 2026-11-07
dogrulama: denenmedi
bolum: 2026-W46
ozet: Robotik, biyomekanik ve makine öğrenmesi için eklemli yapıların temaslı hareketini hızlı ve doğru simüle eden genel amaçlı fizik motoru.
---

# mujoco — kurulum ve ilk örnek

> **Bu belge, README'nin 7 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

MuJoCo (Multi-Joint dynamics with Contact), robotik, biyomekanik, grafik ve animasyon, makine öğrenmesi gibi alanlarda eklemli yapıların çevreleriyle etkileşimini hızlı ve doğru simüle etmek için yapılmış genel amaçlı bir fizik motoru. Bakımını Google DeepMind yapıyor. C API'si var ve araştırmacılar ile geliştiriciler için tasarlanmış; modeller yerleşik XML derleyicisiyle hazırlanır. Masaüstü pencerelerinde ve tarayıcıda etkileşimli görselleştirme, Python bağlamaları ve Unity plugin'i de var. Lisansı Apache-2.0.

## Gereksinimler

- Hazır dosyalar: Linux (x86-64 ve AArch64), Windows (yalnız x86-64), macOS (universal).
- Python bağlamaları için Python 3.11 ya da üstü; hazır Linux wheel'leri `manylinux2014` hedefler.
- Hiçbir şey kurmadan denemek için yalnız bir tarayıcı.

## Kurulum

README'nin önerdiği yol, GitHub releases sayfasındaki sürümlü hazır dosyaları (precompiled binaries) indirmek: https://github.com/google-deepmind/mujoco/releases

Python kullanıyorsan, içinde MuJoCo'nun bir kopyası gelen Python bağlamaları PyPI'dan kurulur:

```bash
pip install mujoco
```

Kaynaktan derlemek isteyenler belgelerdeki "build from source" bölümüne bakmalı; README, `main` dalının en son commit'inin kararsız olabileceğini söylüyor.

## İlk çalıştırma

README'nin "Getting Started" adımları:

1. **Tarayıcıda dene:** https://mujoco.org adresinde hiçbir şey kurmadan masaüstünde ya da telefonda MuJoCo'yu keşfet; https://live.mujoco.org adresinde MuJoCo Studio'nun WebAssembly sürümünde kendi modellerini sürükleyip bırakarak incele (yerel C sürümünden yavaştır).
2. **Kendi bilgisayarında görüntüleyici çalıştır:** belgelerdeki Getting Started kılavuzuna göre hazır bir sürümü indirip MuJoCo Studio uygulamasını ya da klasik `simulate` görüntüleyicisini aç.
3. **Python notebook'ları:** Google Colab'da çalışan eğitim notebook'larıyla başla; giriş eğitimi MuJoCo'nun temellerini anlatır (python/tutorial.ipynb). Model düzenleme, `rollout`, LQR, en küçük kareler, MJX ve türevlenebilir fizik eğitimleri de var.

README, `pip install mujoco` sonrası için ayrı bir komut satırı örneği vermiyor; ilk örnek olarak giriş notebook'u öneriliyor.

## Bağlantılar

- Repo: https://github.com/google-deepmind/mujoco
- Site: https://mujoco.org · Tarayıcıda Studio: https://live.mujoco.org
- Belgeler: https://mujoco.readthedocs.io
- Giriş notebook'u (Colab): https://colab.research.google.com/github/google-deepmind/mujoco/blob/main/python/tutorial.ipynb
- Sürümler: https://github.com/google-deepmind/mujoco/releases (son sürüm 4.0.1, 11 Kasım 2026)
- Issues: https://github.com/google-deepmind/mujoco/issues
- Video: Haftanın Popüler Repoları, 9–15 Kasım 2026 bölümü (listenin 4. sırası)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W46 bölümü](../bolumler/2026-W46.md) videosu için hazırlandı (video henüz yayında değil). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
