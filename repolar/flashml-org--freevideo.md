---
repo: FlashML-org/FreeVideo
url: https://github.com/FlashML-org/FreeVideo
lisans: Apache-2.0
surum: v0.6.11 (2026-11-25)
readme_tarihi: 2026-11-23
dogrulama: denenmedi
bolum: 2026-W48-2
ozet: MiniMax H3 video modelini 8 GB VRAM'li ekran kartında, kendi bilgisayarında çalıştıran ComfyUI plugin'i ve tek tıkla kurulum aracı.
---

# FreeVideo — kurulum ve ilk örnek

> **Bu belge, README'nin 23 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

FreeVideo, MiniMax H3 video modelini kendi bilgisayarında çalıştıran yerel bir çıkarım motorudur. README'ye göre OpenVDN'in 8 adımlı VDN-H3 modeli üzerine kuruludur; VRAM, sistem belleği ve diski birlikte yönetir, ağırlık biçimini (GeForce RTX 30, 40, 50 serisinde int8; yeni mimarili iş istasyonu ve veri merkezi kartlarında FP8) ekran kartına göre kendisi seçer. ComfyUI plugin'i olarak çalışır; Windows, macOS ve Linux için ComfyUI'ı, çalışma ortamını ve modelleri kuran başlatıcıları vardır. Girdi olarak metin, ilk ve son kare, görsel, video ve ses referansı alır; topluluk LoRA'larını destekler. README'nin haber bölümüne göre yeni sürümlerde 16 MP'ye kadar görsel üretimi, bir dakikadan uzun videolar, zaman çizelgesinde düzenleme ve Claude Code ya da Codex gibi bir AI agent'ın kendi başına video üretmesi de var. Kodun lisansı Apache-2.0; model ağırlıkları ayrı bir lisansa bağlı (aşağıda).

## Gereksinimler

- En az 8 GB VRAM ve 16 GB RAM (README: "as little as 8 GB of VRAM and 16 GB of RAM").
- Compute capability 8.0 ya da üstü NVIDIA ekran kartı: RTX 30 serisi ve yenileri, profesyonel kartlar dahil.
- Apple silicon Mac'lerde önizleme sürümü çalışır; README'ye göre 24 GB birleşik bellekli bir M5 Mac'te denenmiş.
- Windows, macOS (Apple silicon) ya da Linux (x86_64). Başlatıcı ComfyUI'ı kendisi kurar ya da var olan ComfyUI klasörünü kullanır.
- Var olan ComfyUI'a ve Linux terminaline kurulum için `git`.
- Modeller ilk kurulumda indirilir. README disk alanı ve indirme boyutu için bir sayı vermiyor.

## Kurulum

**Windows:** [FreeVideo.exe](https://github.com/FlashML-org/FreeVideo/releases/latest/download/FreeVideo.exe) dosyasını indirip çalıştır. Var olan bir ComfyUI klasörünü seç ya da yenisini kur; var olan model klasörleri yeniden kullanılmak üzere eklenebilir, eksik modeller kendiliğinden indirilir. Sonra **Install & launch** düğmesine bas.

**macOS (Apple silicon, önizleme):** [FreeVideo-Mac-arm64.dmg](https://github.com/FlashML-org/FreeVideo/releases/latest/download/FreeVideo-Mac-arm64.dmg) dosyasını indir, aç ve **FreeVideo.app**'i **Applications** klasörüne sürükle. FreeVideo'yu aç, ComfyUI klasörünü seç ya da yenisini kur, **Install & launch** düğmesine bas.

**Var olan ComfyUI'a custom node olarak:**

```bash
cd ComfyUI/custom_nodes
git clone https://github.com/FlashML-org/FreeVideo.git
```

Ardından ComfyUI'ı yeniden başlat. Güncellemek için `custom_nodes/FreeVideo` klasöründe `git pull` çalıştır (ya da node'u ComfyUI-Manager'dan güncelle) ve ComfyUI'ı yeniden başlat.

**Linux, masaüstü:** [FreeVideo-Linux-x86_64.AppImage](https://github.com/FlashML-org/FreeVideo/releases/latest/download/FreeVideo-Linux-x86_64.AppImage) dosyasını indir, çalıştırılabilir yap ve aç; sonrası Windows'taki adımlarla aynı. README dosyayı çalıştırılabilir yapma komutunu yazmıyor.

**Linux, terminal ya da sunucu:**

```bash
git clone https://github.com/FlashML-org/FreeVideo.git && cd FreeVideo
./setup.sh
```

README'ye göre bu kurulum FreeVideo'yu, ComfyUI'ı ve modelleri kurar, sonra FreeVideo'yu tarayıcıda açar.

**İnternetsiz kurulum (Windows başlatıcısı):** paketler README'deki Quark bağlantısından indirilir ve ZIP dosyaları açılmadan başlatıcıya sürüklenir. Tam internetsiz kurulum için dört model paketi (video modeli, text encoder, video ve ses decoder'ı, sampling cache'leri), yeni bir ComfyUI kurulumu için ayrıca ortam paketi gerekir.

## İlk çalıştırma

1. Başlatıcıyla kurduysan **Install & launch** sonrasında ComfyUI, FreeVideo çalışma alanıyla tarayıcıda açılır.
2. Var olan ComfyUI'a kurduysan ComfyUI'ı yeniden başlat, **Workflow → Browse Templates → FreeVideo → FreeVideo-All-in-One** şablonunu aç ve kurulumu FreeVideo **Settings** bölümünde tamamla.
3. Linux terminal kurulumundan sonra yeniden açmak için:

```bash
freevideo
```

   Sunucuda çalıştırıyorsan kendi bilgisayarından SSH ile bağlan ya da yerel ağdan erişim bağlantısı için `freevideo server --listen 0.0.0.0` çalıştır (ayrıntı: Linux kılavuzu).

README ilk videonun adım adım nasıl üretileceğini (prompt yazma, kalite seçimi, süre) anlatmıyor; yalnız çalışma alanının açıldığını söylüyor. Haber bölümünden çıkan bilgiler: her video için Light, Medium, High ya da Max kalite düzeyi seçilir (yüksek düzey daha kaliteli ama daha yavaş), önce yarı çözünürlüklü bir önizleme görüp yalnız beğendiğin videoyu tamamlayabilirsin. Daha ince ayar için node görünümüne geçip LoRA eklenebilir.

## Sık takılınan yerler

- Güvenlik yazılımı FreeVideo.exe'yi engeller ya da FreeVideo klasöründeki dosyaları karantinaya alırsa: README'ye göre dosya kod imzalıdır; FreeVideo.exe'nin SHA-256 değerini sürüm sayfasındaki değerle karşılaştır, sonra dosyaları geri yükle ve FreeVideo klasörünü yazılımın dışlama listesine ekle.
- Mac önizlemesi henüz Apple tarafından notarize edilmedi, bu yüzden macOS ilk açılışta engeller. README dosyanın yalnız Releases sayfasından indirilmesini, sonra Mac kılavuzundaki "first open" adımlarıyla denetlenip onaylanmasını söylüyor; bu onay yalnız FreeVideo için geçerlidir.
- İnternetsiz kurulumda yalnız ortam paketini içe aktardıktan sonra modeller için **Automatic download** seçilebilir (v0.3.12 ve sonrası). Ses referansı cache'i isteğe bağlıdır, yalnız referansta ses varsa kullanılır.
- Lisans: kod Apache-2.0, ama model ağırlıkları MiniMax H3 Community License'a bağlıdır; README bu lisansın bölgesel ve kullanım amacına ilişkin kısıtlar içerdiğini yazıyor. Ticari kullanımdan önce lisans metnini oku.

## Bağlantılar

- Repo: https://github.com/FlashML-org/FreeVideo
- Sürümler: https://github.com/FlashML-org/FreeVideo/releases (son sürüm v0.6.11, 25 Kasım 2026)
- Kılavuzlar: Mac https://github.com/FlashML-org/FreeVideo/blob/main/docs/Mac.md · Linux https://github.com/FlashML-org/FreeVideo/blob/main/docs/Linux.md · LoRA örnekleri https://github.com/FlashML-org/FreeVideo/blob/main/docs/LoRA.md
- Örnek videolar: https://freevideo-community.pages.dev/#gallery · topluluk sonuçları https://github.com/FlashML-org/FreeVideo/blob/main/docs/community-results.md
- Model ağırlıklarının lisansı: https://huggingface.co/OpenVDN/vdn-minimax-h3-edge/blob/main/LICENSE
- Issues: https://github.com/FlashML-org/FreeVideo/issues · Discord: https://discord.gg/MsA277cJzZ
- Video: Haftanın Popüler Repoları, 23–25 Kasım 2026 bölümü (listenin 9. sırası)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W48-2 bölümü](../bolumler/2026-W48-2.md) videosu için hazırlandı: https://www.youtube.com/watch?v=2wDhRoLOWh8. Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
