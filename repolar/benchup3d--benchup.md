---
repo: BenchUp3D/benchup
url: https://github.com/BenchUp3D/benchup
lisans: MIT
surum: v0.7.3 (2026-11-15)
readme_tarihi: 2026-11-15
dogrulama: denenmedi
bolum: trend-short/t2026-11-17-1
ozet: Ahşap işleri ve mobilya tasarımı için Rust ile yazılmış, masaüstünde ve tarayıcıda çalışan ücretsiz, açık kaynak 3D modelleme uygulaması.
---

# BenchUp — kurulum ve ilk örnek

> **Bu belge, README'nin 15 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

BenchUp, ahşap işleri ve mobilya tasarımı için 3D modelleme uygulaması. Tek tek tahtaları, ahşap birleşimlerini ve tüm montajı atölyeye girmeden önce kesin ölçüleriyle planlarsın. Aynı editör Windows, macOS ve Linux'ta yerel uygulama olarak, masaüstü tarayıcıda da WebAssembly ile çalışır. Araçlar arasında Select, Line, Rectangle, Circle, Polygon, Freehand, Push/Pull, Scale, Offset, Move, Rotate, Paint, Tape Measure, Protractor, Dimensions, Router, Erase ve Orbit var; otomatik geometri snapping, gruplar ve component'ler, undo/redo, yerel autosave ve taşınabilir `.bup` proje dosyaları da var. File → Import/Export menüsü SketchUp `.skp` dosyası içe aktarır ve Wavefront `.obj` dışa aktarır. GitHub açıklamasında kendini SketchUp alternatifi olarak tanıtıyor. Lisansı MIT.

## Gereksinimler

Hazır sürümü kullanacaksan (masaüstü kurulum dosyası ya da tarayıcıdan deneme) README ek gereksinim yazmıyor; GPU ve sürücünün wgpu tarafından desteklenmesi gerekir (tarayıcı WebGPU'yu tercih eder, yoksa WebGL2'ye düşer).

Kaynaktan derleyeceksen README'nin "Prerequisites" bölümü:

- rustup ile kurulu Rust (`rust-toolchain.toml` geliştirme için Rust 1.97.1'i sabitler; en düşük desteklenen sürüm 1.95).
- Yerel SketchUp okuyucusu için C/C++17 derleyicisi (Linux'ta GCC/Clang, macOS'ta Xcode Clang, Windows'ta MSVC). Tarayıcı derlemesi bunu istemez.
- Linux'ta X11/Wayland, xkbcommon ve libudev geliştirme kütüphaneleri. Debian/Ubuntu için:

```sh
sudo apt install build-essential pkg-config libx11-dev libxcb1-dev libxkbcommon-dev libwayland-dev libegl1-mesa-dev libudev-dev
```

- Windows'ta MSVC Rust toolchain ve **Desktop development with C++** iş yükü ile Windows SDK içeren Visual Studio Build Tools.
- macOS'ta Xcode Command Line Tools:

```sh
xcode-select --install
```

## Kurulum

**Hazır sürüm:** README'nin başındaki indirme bağlantıları son sürümün dosyalarına gider (Releases sayfası: https://github.com/BenchUp3D/benchup/releases/latest):

- Windows x64: `*-win-x64-Setup.exe` (kullanıcıya özel kurulum)
- macOS Apple Silicon: `*-osx-arm64-Setup.pkg` ya da taşınabilir `.app` zip
- macOS Intel: `*-osx-x64-Setup.pkg` ya da taşınabilir `.app` zip
- Linux x64: `*-linux-x64.AppImage`
- Tarayıcı: `benchup-vX.Y.Z-wasm.zip` dosyasını açıp bir statik web sunucusuyla sun.

Kurmadan denemek için README'deki tarayıcı bağlantısı: https://benchup3d.github.io/benchup/play/

**Kaynaktan, masaüstü:** repoyu klonladıktan sonra kök klasörde:

```sh
cargo run -p benchup-app --profile dev-desktop
```

Çalıştırılabilir dosya `target/dev-desktop/benchup-app` (Windows'ta `benchup-app.exe`) olur. Linux'ta derledikten sonra uygulama başlatıcısını ve simgeleri bir kez kaydetmek için:

```sh
python3 scripts/install_linux_desktop.py target/dev-desktop/benchup-app
```

**Kaynaktan, tarayıcı:** tarayıcı hedefini ve Trunk'ı bir kez kur:

```sh
rustup target add wasm32-unknown-unknown
cargo install --locked trunk
```

Sonra repo kökünde:

```sh
trunk serve
```

## İlk çalıştırma

1. Uygulamayı aç (kurulum dosyası, tarayıcı bağlantısı ya da `trunk serve` sonrası http://127.0.0.1:8080). README'ye göre uygulama boş bir modelle başlar.
2. Line ya da Rectangle ile bir yüzey çiz, Push/Pull ile kalınlık ver; Tape Measure ve Dimensions ile ölçüleri kontrol et.
3. Var olan bir SketchUp çizimin varsa File → Import/Export ile `.skp` dosyasını içe aktar; işin bitince `.bup` proje dosyası olarak kaydet ya da `.obj` olarak dışa aktar.

README adım adım bir "ilk model" örneği vermiyor; bu akış README'deki araç listesinden ve menü açıklamasından çıkarıldı. Tarayıcıda aynı kaydedilmiş modele dönmek için uygulamayı aynı adres ve port üzerinde açman gerekir (README).

## Sık takılınan yerler

- Linux'ta derleme `Unable to find libudev` ya da `Package 'libudev' not found` hatasıyla durursa `libudev-dev` paketini kurup yeniden dene (README). SpaceMouse bağlı olmasa da bu paket gerekir.
- Tarayıcıda çalıştırmak için Node.js, JavaScript framework'ü ya da backend gerekmez; Trunk gerekli JavaScript'i kendisi üretir (README).
- `dev-desktop` profili yalnız yerel masaüstü geliştirme içindir; `trunk serve` standart `dev` profilini kullanır (README).

## Bağlantılar

- Repo: https://github.com/BenchUp3D/benchup
- Site: https://benchup.io/
- Tarayıcıda dene: https://benchup3d.github.io/benchup/play/
- Sürümler: https://github.com/BenchUp3D/benchup/releases (son sürüm v0.7.3, 15 Kasım 2026)
- Issues: https://github.com/BenchUp3D/benchup/issues
- Video: Haftanın Popüler Repoları, "SketchUp'a boşuna para ödeme: benchup" Short'u

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki trend Short t2026-11-17-1 videosu için hazırlandı (video henüz yayında değil). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
