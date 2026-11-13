---
repo: localsend/localsend
url: https://github.com/localsend/localsend
lisans: Apache-2.0
surum: v1.18.2 (2026-08-21)
readme_tarihi: 2026-11-11
dogrulama: denenmedi
bolum: gunun-reposu/g02
ozet: Aynı yerel ağdaki cihazlar arasında internet ve sunucu olmadan dosya ve mesaj paylaşan açık kaynak AirDrop alternatifi.
---

# LocalSend — kurulum ve ilk örnek

> **Bu belge, README'nin 11 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

LocalSend, aynı yerel ağdaki cihazlar arasında dosya ve kısa mesaj göndermek için ücretsiz, açık kaynak bir uygulama; internet bağlantısı ve dış sunucu gerektirmez. Cihazlar birbirini ağda bulur, veri her cihazda anında üretilen TLS sertifikasıyla HTTPS üzerinden gider. Telefon, bilgisayar ve tablet arasında platform fark etmeden (Windows, macOS, Linux, Android, iOS) AirDrop benzeri paylaşım isteyen herkes için; terminalden gönderim için ayrı bir CLI'ı da var.

## Gereksinimler

- En düşük sürümler (README, Compatibility): Android 7.0, iOS 15.0, macOS 12 Monterey, Windows 10. Linux'ta Gnome için `xdg-desktop-portal` + `xdg-desktop-portal-gtk`, KDE için `xdg-desktop-portal` + `xdg-desktop-portal-kde`.
- Cihazlar aynı yerel ağda olmalı; güvenlik duvarında 53317 portu (TCP ve UDP, gelen) açık, yönlendiricide AP isolation kapalı.
- Hesap, giriş, sunucu yok.
- CLI için macOS/Linux'ta Homebrew.
- Kaynaktan derlemek için Flutter (`.fvmrc` dosyasındaki sürüm; `fvm` önerilir) ve Rust.

## Kurulum

README, uygulamanın kendini güncellemediğini söyleyip mağaza ya da paket yöneticisini öneriyor. Paket yöneticisi komutları README'de yok; her bağlantı ilgili mağaza ya da paket sayfasına gider.

### Kullanıcı kurulumu: mağaza ya da paket yöneticisi (önerilen)

| Platform | Seçenekler |
|---|---|
| Windows | [Winget](https://github.com/microsoft/winget-pkgs/tree/master/manifests/l/LocalSend/LocalSend), [Scoop](https://scoop.sh/#/apps?s=0&d=1&o=true&q=localsend&id=fb88113be361ca32c0dcac423cb4afdeda0b0c66), [Chocolatey](https://community.chocolatey.org/packages/localsend), EXE kurucu ve taşınabilir ZIP ([releases](https://github.com/localsend/localsend/releases/latest)) |
| macOS | [App Store](https://apps.apple.com/us/app/localsend/id1661733229), [Homebrew](https://formulae.brew.sh/cask/localsend), DMG ([releases](https://github.com/localsend/localsend/releases/latest)) |
| Linux | [Flathub](https://flathub.org/apps/details/org.localsend.localsend_app), [Nixpkgs](https://search.nixos.org/packages?show=localsend), [Snap](https://snapcraft.io/localsend), [AUR](https://aur.archlinux.org/packages/localsend-bin), TAR / DEB / AppImage ([releases](https://github.com/localsend/localsend/releases/latest)) |
| Android | [Play Store](https://play.google.com/store/apps/details?id=org.localsend.localsend_app), [F-Droid](https://f-droid.org/packages/org.localsend.localsend_app), APK ([releases](https://github.com/localsend/localsend/releases/latest)) |
| iOS | [App Store](https://apps.apple.com/us/app/localsend/id1661733229) |
| Fire OS | [Amazon](https://www.amazon.com/dp/B0BW6MP732) |

Windows ikilileri imzalı ([Code signing policy](https://github.com/localsend/localsend/blob/main/CODE_SIGNING.md)). Kurulum istemeyen bir tarayıcı sürümü de var: https://web.localsend.org/ (localsend.org).

### CLI (macOS ve Linux, Homebrew)

```shell
brew update
brew install localsend-cli
```

Güncellemek için:

```shell
brew update
brew upgrade localsend-cli
```

### Kaynaktan derleme (geliştiriciler için)

README'deki adımlar: Flutter'ı ([doğrudan](https://flutter.dev) ya da [fvm](https://fvm.app) ile, `.fvmrc`'deki sürüm) ve [Rust](https://www.rust-lang.org/tools/install)'ı kur, depoyu klonla, sonra:

```
cd app
flutter pub get
flutter run
```

LocalSend eski bir Flutter sürümü istiyor; `fvm` kurduysan `flutter` yerine `fvm flutter` yaz.

## İlk çalıştırma

1. Uygulamayı iki cihazda aç; ikisi de aynı Wi-Fi/ağda olsun. README'ye göre çoğu durumda LocalSend kutudan çıktığı gibi çalışır.
2. Bir cihazda gönderme (Send) ekranından dosya ya da metin seç; listede görünen diğer cihaza dokun. Alıcı tarafta isteği kabul et; dosya alıcının indirme klasörüne iner.
3. Cihazlar birbirini görmüyorsa güvenlik duvarı kuralını ekle (README, Setup):

| Trafik | Protokol | Port | İşlem |
|---|---|---|---|
| Gelen | TCP, UDP | 53317 | İzin ver |
| Giden | TCP, UDP | Herhangi | İzin ver |

Linux'ta `ufw` ile: `sudo ufw allow 53317`. `firewalld` ile: `sudo firewall-cmd --permanent --add-port=53317/tcp`, `sudo firewall-cmd --permanent --add-port=53317/udp`, sonra `sudo firewall-cmd --reload`.

CLI ile ilk gönderim (bulunan cihazların listesi açılır, hedefi seçip Enter'a basarsın):

```shell
localsend-cli send report.pdf photo.jpg ./project-backup
```

Listeyi atlayıp takma ad ya da IP ile hedef vermek, dosya yerine metin göndermek için:

```shell
localsend-cli send --to "Cute Tomato" report.pdf
localsend-cli send --to 192.168.27.26 report.pdf
localsend-cli send --to "Cute Tomato" --text "https://localsend.org"
```

`localsend-cli --help` tüm seçenekleri gösterir. Masaüstünde taşınabilir mod için çalıştırılabilir dosyanın yanına boş bir `settings.json` koy (v1.13.0+); yalnız sistem tepsisine açılmak için `--hidden` bayrağı (`localsend_app.exe --hidden`).

## Sık takılınan yerler

- **Cihaz görünmüyor.** Yönlendiricide AP isolation'ı kapat (misafir ağlarında sık açık); Windows'ta ağı "özel" yap; macOS/iOS'ta Gizlilik altındaki "Yerel Ağ" iznini kapatıp aç; VPN açıksa yerel trafiğe izin ver ya da VPN'i geçici kapat. Hiçbiri olmazsa alıcının IP'sini elle gir, çalışırsa cihazı favorilere ekle ([README, Troubleshooting](https://github.com/localsend/localsend#troubleshooting)).
- **Windows'ta `SocketException ... errno = 10013 ... port = 53317`.** Uygulama sunucuyu başlatamıyor; 53317 portu başka bir süreç (bazen önceki bir LocalSend) tarafından tutuluyor. `netstat -ano | findstr ":53317"` ile bak, ayarlardan portu değiştir ([#2746](https://github.com/localsend/localsend/issues/2746); daha eski kayıt [#125](https://github.com/localsend/localsend/issues/125)).
- **Aktarım yavaş.** 5 GHz bant kullan, iki cihazda da şifrelemeyi kapat; Android alıcıda bilinen bir yavaşlık var ([saf_stream#4](https://github.com/flutter-cavalry/saf_stream/issues/4)) ([README, Troubleshooting](https://github.com/localsend/localsend#troubleshooting)).
- **Uygulama güncellenmiyor.** Otomatik güncelleme yok; mağaza ya da paket yöneticisinden kurduysan oradan güncellenir ([README, Download](https://github.com/localsend/localsend#download)).
- **Kaynaktan derleme hatası.** Sistemdeki Flutter sürümü `.fvmrc`'dekinden farklıysa derleme bozulur; `fvm` kurup `fvm flutter` kullan ([README, Getting Started](https://github.com/localsend/localsend#getting-started)).

## Bağlantılar

- Repo: https://github.com/localsend/localsend (ayna: https://codeberg.org/localsend/localsend)
- Belgeler: https://localsend.org — protokol: https://github.com/localsend/protocol
- Sürümler: https://github.com/localsend/localsend/releases
- Lisans: Apache-2.0 — https://github.com/localsend/localsend/blob/main/LICENSE

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki Günün Reposu g02 videosu için hazırlandı: https://www.youtube.com/watch?v=1IylBcEQ17w. Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
