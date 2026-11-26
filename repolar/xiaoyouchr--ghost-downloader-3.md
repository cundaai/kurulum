---
repo: XiaoYouChR/Ghost-Downloader-3
url: https://github.com/XiaoYouChR/Ghost-Downloader-3
lisans: GPL-3.0
surum: v4.5.11 (2026-11-20)
readme_tarihi: 2026-11-23
dogrulama: denenmedi
bolum: 2026-W48-2
ozet: HTTP, torrent, FTP, M3U8, YouTube ve Bilibili indirmelerini tek uygulamada toplayan, Windows, macOS, Linux ve Android'de çalışan açık kaynak indirme yöneticisi.
---

# Ghost-Downloader-3 — kurulum ve ilk örnek

> **Bu belge, README'nin 23 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

Ghost Downloader, README'nin deyişiyle "ihtiyacın olan tek indirici": beş ayrı program yerine tek uygulama. `HTTP`, `Magnet/BT`, `FTP`, `M3U8`, `MPEG-DASH` ve `eD2k` indirir; `YouTube` ve `Bilibili` videolarını kesit seçimi, istenen kalite, altyazı, oynatma listesi ve çok parçalı bölümlerle alır; `GitHub` ve `HuggingFace` indirmelerini mirror ile hızlandırır. README'ye göre kötü bağlantıda da hızlı kalmak için dosyayı birleştirme gerektirmeyen akıllı parçalara böler, bant genişliğini kendisi algılar ve gerçek tarayıcı TLS parmak izi kullanır. Tarayıcı eklentisi sayfadaki videonun yanına indirme düğmesi koyar ve sayfadaki bütün medyayı listeler; indirmeyi masaüstü uygulaması devralır, oturum bilgisi de taşınır. Üçüncü taraf araçlar için `aria2` uyumlu RPC API'si vardır; indirme sürerken URL, header ya da proxy değiştirilebilir. Android'de canlı M3U8 yayınlarını kaydeder ve arka planda indirmeyi sürdürür. Lisansı GPL v3.0.

## Gereksinimler

README'deki platform tablosu:

- Windows `10+` (`x86_64` / `arm64`)
- macOS `13.0+` (`x86_64` / `arm64`)
- Linux `glibc 2.35+` (`x86_64` / `arm64`)
- Android `9.0+` (`arm64-v8a`)
- README uyarısı: Qt `6.6+`, `AVX` komut kümesi olmayan işlemcileri artık desteklemiyor; eski işlemcilerde uygulama çalışmayabilir.

## Kurulum

README adım adım bir kurulum anlatmıyor, kurulum komutu da vermiyor; yalnız "Release" rozetiyle Releases sayfasına ve "Documentation" bağlantısıyla https://gd.xychr.com/en/docs/ adresine yönlendiriyor. Bu yüzden aşağıdaki yol README'den değil, reponun Releases sayfasındaki dosya adlarından çıkarıldı.

Hazır dosyalar: https://github.com/XiaoYouChR/Ghost-Downloader-3/releases/latest adresinden sistemine uyan dosyayı indir. v4.5.11 sürümündeki dosya adları (komut değil, dosya listesi):

```text
Ghost-Downloader-v4.5.11-Windows-x86_64-Setup.exe
Ghost-Downloader-v4.5.11-Windows-x86_64.zip
Ghost-Downloader-v4.5.11-Windows-arm64-Setup.exe
Ghost-Downloader-v4.5.11-Windows-arm64.zip
Ghost-Downloader-v4.5.11-macOS-arm64.dmg
Ghost-Downloader-v4.5.11-macOS-x86_64.dmg
Ghost-Downloader-v4.5.11-Linux-x86_64.AppImage
Ghost-Downloader-v4.5.11-Linux-x86_64.deb
Ghost-Downloader-v4.5.11-Linux-x86_64.tar.xz
Ghost-Downloader-v4.5.11-Linux-arm64.AppImage
Ghost-Downloader-v4.5.11-Linux-arm64.deb
Ghost-Downloader-v4.5.11-Linux-arm64.tar.xz
Ghost-Downloader-v4.5.11-Android-arm64-v8a.apk
```

Dosyayı indirdikten sonraki adımları (kurulum sihirbazı, `.dmg` açma, `.deb` kurma) README vermiyor; sisteminin alışılmış yoluyla kurulur, ayrıntı için reponun belgelerine bak.

Arch Linux: README'ye göre topluluğun baktığı `ghost-downloader-bin` ve `ghost-downloader-git` AUR paketleri var (bakımcı: @zxp19821005). README bunlar için de komut vermiyor.

Komut satırı aracı: README'nin tek kurulum cümlesi şu: "Install `ghd` from **Settings -> Command Line Tool**." Yani `ghd` komutu ayrı indirilmez, uygulamanın ayarlar ekranından kurulur.

Tarayıcı eklentisi: README eklentiyi özelliklerde anlatıyor ama nereden kurulacağını yazmıyor; repoda `browser_extension` klasörü var, kurulum yolu için belgelere bak.

## İlk çalıştırma

README arayüz için adım adım bir "ilk indirme" örneği vermiyor; ekran görüntüleri ve tanıtım videosu var. Komut içeren tek bölüm komut satırı aracı `ghd`. Uygulamayı açıp **Settings -> Command Line Tool** yolundan `ghd` komutunu kurduktan sonra README'nin örneği:

```sh
# see which files the torrent contains
ghd probe "https://example.com/a.torrent"
# download only files 0 and 3 into ./downloads
ghd add "https://example.com/a.torrent" --select 0,3 -o ./downloads
# wait until the download finishes
ghd wait tsk_0123
```

Sırasıyla: torrent'in içindeki dosyaları gösterir, yalnız 0 ve 3 numaralı dosyaları `./downloads` klasörüne indirir, indirme bitene kadar bekler. Adres ve `tsk_0123` görev kimliği README'nin örnek değerleridir; kendi adresini ve `ghd tasks` çıktısındaki kendi görev kimliğini yaz.

README'deki komut tablosu:

| Komut | Ne yapar |
|:--|:--|
| `ghd probe <url>` | Görev eklemeden adresi önizler |
| `ghd add <url>...` | Görev ekler ve başlatır |
| `ghd tasks` | Görevleri listeler |
| `ghd task <taskId>` | Görevin ayrıntısını gösterir |
| `ghd pause <taskId>...` | Görevleri duraklatır |
| `ghd start <taskId>...` | Görevleri sürdürür |
| `ghd remove <taskId>...` | Görevleri kaldırır, bitmemiş dosyaları siler |
| `ghd wait <taskId>...` | Görevlerin bitmesini bekler |

## Sık takılınan yerler

- `AVX` desteklemeyen işlemcilerde Qt `6.6+` çalışmaz (README uyarısı).
- Linux'ta `glibc 2.35+` gerekir; daha eski dağıtımlarda hazır dosyalar çalışmayabilir (README'nin platform tablosu).
- `ghd remove` yalnız görevi kaldırmaz, bitmemiş dosyaları da siler (README'nin komut tablosu).
- Plugin API'si henüz herkese açık değil: Feature Packs sistemi çalışıyor ama API hâlâ oturuyor (README'nin yol haritası).
- Çeviri dosyalarını değiştiren pull request kabul edilmiyor; çeviriler Crowdin üzerinden yapılır (README).

## Bağlantılar

- Repo: https://github.com/XiaoYouChR/Ghost-Downloader-3
- Site ve belgeler: https://gd.xychr.com/ · https://gd.xychr.com/en/docs/
- Sürümler: https://github.com/XiaoYouChR/Ghost-Downloader-3/releases (son sürüm v4.5.11, 20 Kasım 2026)
- Issues: https://github.com/XiaoYouChR/Ghost-Downloader-3/issues · Discord: https://discord.gg/fKfhkPumEM
- Tanıtım videosu (README'deki): https://www.youtube.com/watch?v=QQRa8nDmNbc
- Çeviri (Crowdin): https://gdi18n.xychr.com
- Video: Haftanın Popüler Repoları, 23–25 Kasım 2026 bölümü (listenin 1. sırası)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W48-2 bölümü](../bolumler/2026-W48-2.md) videosu için hazırlandı: https://www.youtube.com/watch?v=2wDhRoLOWh8. Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
