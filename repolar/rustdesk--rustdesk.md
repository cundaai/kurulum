---
repo: rustdesk/rustdesk
url: https://github.com/rustdesk/rustdesk
lisans: AGPL-3.0
surum: 1.5.3 (2026-11-09)
readme_tarihi: 2026-11-23
dogrulama: denenmedi
bolum: 2026-W48-2
ozet: Kendi sunucunda barındırabildiğin, Rust ile yazılmış açık kaynak uzak masaüstü uygulaması; TeamViewer alternatifi.
---

# rustdesk — kurulum ve ilk örnek

> **Bu belge, README'nin 23 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

RustDesk, Rust ile yazılmış bir uzak masaüstü uygulamasıdır: başka bir bilgisayarın ekranını görür, klavye ve fareyle yönetirsin. README'ye göre hiçbir ayar yapmadan çalışır ve verinin denetimi sende kalır: projenin rendezvous/relay sunucusunu kullanabilir, kendi sunucunu kurabilir ya da kendi rendezvous/relay sunucunu yazabilirsin. README'deki ekran görüntüleri bağlantı yöneticisini, dosya aktarımını ve TCP tünellemeyi gösteriyor. Lisansı AGPL-3.0.

README'nin kötüye kullanım uyarısı: yazılım yalnız izinli erişim için kullanılmalıdır; izinsiz erişim, denetim ve mahremiyet ihlali projenin kurallarına aykırıdır ve geliştiriciler kötüye kullanımdan sorumlu değildir.

## Gereksinimler

- Hazır kurulum dosyasıyla kullanmak için README bir gereksinim listesi vermiyor; desteklenen işletim sistemi sürümlerini de yazmıyor. İndirme yerleri: GitHub Releases, F-Droid (Android), Flathub (Linux).
- Kaynaktan derlemek için: Rust geliştirme ortamı ve C++ derleme ortamı, `vcpkg` ve doğru ayarlanmış `VCPKG_ROOT` ortam değişkeni.
- Kaynaktan derlemede Sciter dinamik kütüphanesi (README: "Please download Sciter dynamic library yourself"); Windows, Linux ve macOS için bağlantılar README'nin "Dependencies" bölümünde.
- Linux'ta derleme için dağıtıma göre paketler (aşağıdaki komutlar).
- Docker ile derlemek için Docker ve `git`.

## Kurulum

**Hazır kurulum dosyası (çoğu kullanıcı için):** README adım adım kurulum anlatmıyor, yalnız indirme bağlantılarını veriyor:

- Sürümler (BINARY DOWNLOAD): https://github.com/rustdesk/rustdesk/releases
- Gecelik derleme (NIGHTLY BUILD): https://github.com/rustdesk/rustdesk/releases/tag/nightly
- Android, F-Droid: https://f-droid.org/en/packages/com.carriez.flutter_hbb
- Linux, Flathub: https://flathub.org/apps/com.rustdesk.RustDesk

Hangi dosyanın hangi sisteme kurulacağını ve kurulum adımlarını README yazmıyor; bunun için sürüm sayfasına ve https://rustdesk.com/docs adresindeki belgelere bak.

**Kaynaktan derleme (README'nin asıl anlattığı yol).** README'deki kısa adımlar: Rust ve C++ derleme ortamını hazırla, vcpkg'yi kur ve `VCPKG_ROOT` değişkenini ayarla, sonra bağımlılıkları kur ve `cargo run` çalıştır. vcpkg satırları README'de şöyle:

```
Windows: vcpkg install libvpx:x64-windows-static libyuv:x64-windows-static opus:x64-windows-static aom:x64-windows-static
Linux/macOS: vcpkg install libvpx libyuv opus aom
```

Linux'ta önce dağıtımının paketlerini kur. Ubuntu 18 (Debian 10):

```sh
sudo apt install -y zip g++ gcc git curl wget nasm yasm libgtk-3-dev clang libxcb-randr0-dev libxdo-dev \
        libxfixes-dev libxcb-shape0-dev libxcb-xfixes0-dev libasound2-dev libpulse-dev cmake make \
        libclang-dev ninja-build libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev
```

openSUSE Tumbleweed:

```sh
sudo zypper install gcc-c++ git curl wget nasm yasm gcc gtk3-devel clang libxcb-devel libXfixes-devel cmake alsa-lib-devel gstreamer-devel gstreamer-plugins-base-devel xdotool-devel
```

Fedora 28 (CentOS 8):

```sh
sudo yum -y install gcc-c++ git curl wget nasm yasm gcc gtk3-devel clang libxcb-devel libxdo-devel libXfixes-devel pulseaudio-libs-devel cmake alsa-lib-devel gstreamer1-devel gstreamer1-plugins-base-devel
```

Arch (Manjaro):

```sh
sudo pacman -Syu --needed unzip git cmake gcc curl wget yasm nasm zip make pkg-config clang gtk3 xdotool libxcb libxfixes alsa-lib pipewire
```

vcpkg kurulumu:

```sh
git clone https://github.com/microsoft/vcpkg
cd vcpkg
git checkout 2023.04.15
cd ..
vcpkg/bootstrap-vcpkg.sh
export VCPKG_ROOT=$HOME/vcpkg
vcpkg/vcpkg install libvpx libyuv opus aom
```

Derleme (Rust'ı kurar, repoyu submodule'leriyle indirir, Sciter kütüphanesini `target/debug` altına koyar ve uygulamayı derleyip çalıştırır):

```sh
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source $HOME/.cargo/env
git clone --recurse-submodules https://github.com/rustdesk/rustdesk
cd rustdesk
mkdir -p target/debug
wget https://raw.githubusercontent.com/c-smile/sciter-sdk/master/bin.lnx/x64/libsciter-gtk.so
mv libsciter-gtk.so target/debug
VCPKG_ROOT=$HOME/vcpkg cargo run
```

**Docker ile derleme.** Önce repoyu indir ve container imajını oluştur:

```sh
git clone https://github.com/rustdesk/rustdesk
cd rustdesk
git submodule update --init --recursive
docker build -t "rustdesk-builder" .
```

Sonra uygulamayı her derlemek istediğinde şu komutu çalıştır:

```sh
docker run --rm -it -v $PWD:/home/user/rustdesk -v rustdesk-git-cache:/home/user/.cargo/git -v rustdesk-registry-cache:/home/user/.cargo/registry -e PUID="$(id -u)" -e PGID="$(id -g)" rustdesk-builder
```

Optimize edilmiş release sürümü için komutun sonuna `--release` eklenir (README).

## İlk çalıştırma

Hazır kurulum dosyasıyla kurduysan uygulamayı açman yeter; README "Works out of the box with no configuration required" diyor. İki bilgisayarın nasıl eşleştirileceğini (kimlik, parola, bağlantı adımları) README anlatmıyor; bunun için https://rustdesk.com/docs adresindeki belgelere bak. Yalnız kendi cihazlarına ya da sahibinin açıkça izin verdiği cihazlara bağlan.

Kaynaktan derlediysen yukarıdaki `cargo run` uygulamayı derleyip açar. Docker ile derlediysen çıkan dosyayı reponun kök klasöründen çalıştır:

```sh
target/debug/rustdesk
```

Release derlemesi için:

```sh
target/release/rustdesk
```

Kendi sunucunu kullanmak istersen README https://rustdesk.com/server adresine yönlendiriyor; sunucu kurulumunu README anlatmıyor.

## Sık takılınan yerler

- README'deki derleme anlatımı yalnız Sciter arayüzü içindir ve Sciter "deprecated" olarak işaretli. Güncel Flutter sürümünü derlemek için README yalnız CI dosyasına (`.github/workflows/flutter-build.yml`) yönlendiriyor, adım yazmıyor.
- Fedora'da libvpx için README ayrı bir düzeltme veriyor ("Fix libvpx (For Fedora)"):

```sh
cd vcpkg/buildtrees/libvpx/src
cd *
./configure
sed -i 's/CFLAGS+=-I/CFLAGS+=-fPIC -I/g' Makefile
sed -i 's/CXXFLAGS+=-I/CXXFLAGS+=-fPIC -I/g' Makefile
make
cp libvpx.a $HOME/vcpkg/installed/x64-linux/lib/
cd
```

- Docker'da ilk derleme, bağımlılıklar önbelleğe alınana kadar uzun sürer; sonrakiler daha hızlıdır.
- Docker komutlarını RustDesk reposunun kök klasöründen çalıştır, yoksa uygulama gereken kaynakları bulamayabilir.
- Docker yöntemiyle `install` ya da `run` gibi öbür cargo alt komutları desteklenmiyor: programı host yerine container içinde kurar ya da çalıştırırlar.
- README'deki dağıtım adları eski sürümlerdir (Ubuntu 18, Fedora 28) ve vcpkg `2023.04.15` etiketine sabitlenmiş; yeni dağıtımlarda paket adları farklı olabilir. Güncel derleme belgesi: https://rustdesk.com/docs/en/dev/build/

## Bağlantılar

- Repo: https://github.com/rustdesk/rustdesk
- Site: https://rustdesk.com · kendi sunucun: https://rustdesk.com/server
- Sürümler: https://github.com/rustdesk/rustdesk/releases (son sürüm 1.5.3, 9 Kasım 2026) · gecelik derleme: https://github.com/rustdesk/rustdesk/releases/tag/nightly
- Derleme belgesi: https://rustdesk.com/docs/en/dev/build/ · SSS: https://github.com/rustdesk/rustdesk/wiki/FAQ
- Türkçe README: https://github.com/rustdesk/rustdesk/blob/master/docs/README-TR.md
- Issues: https://github.com/rustdesk/rustdesk/issues · Discord: https://discord.gg/nDceKgxnkV · Reddit: https://www.reddit.com/r/rustdesk
- Video: Haftanın Popüler Repoları, 23–25 Kasım 2026 bölümü (listenin 10. sırası)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W48-2 bölümü](../bolumler/2026-W48-2.md) videosu için hazırlandı (yakında — 25 Kas 12:30). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
