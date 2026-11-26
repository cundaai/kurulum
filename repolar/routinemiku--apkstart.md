---
repo: RoutineMiku/apkstart
url: https://github.com/RoutineMiku/apkstart
lisans: MIT
surum: v0.6.3 (2026-11-22)
readme_tarihi: 2026-11-18
dogrulama: denenmedi
bolum: trend-short/t2026-11-25-1
ozet: Windows'ta APK dosyasına çift tıklayınca Android uygulamasını kuran, ayrı pencerede ve kendi masaüstü ikonuyla çalıştıran açık kaynak araç.
---

# apkstart — kurulum ve ilk örnek

> **Bu belge, README'nin 18 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

ApkStart, Android uygulamalarını Windows'ta yerel bir program gibi kurup kullanmayı amaçlar. README'ye göre `.apk` dosya ilişkilendirmesi açıldıktan sonra Gezgin'de bir APK'ya çift tıklarsın; araç dosyayı çözer, uygulamayı kurar ve masaüstüne uygulamanın kendi ikonunu koyar, arada hiçbir emulator arayüzü açılmaz. Her uygulama ayrı bir pencerede çalışır, Başlat menüsünde adıyla aranır, görev çubuğunda uygulama bazında gruplanır. `%USERPROFILE%\ApkStartShared` klasörü Android içindeki `/sdcard/ApkStartShared` ile iki yönlü eşitlenir. Altta Android Emulator (QEMU) ve Android 14 sistem imajı çalışır; araç bunları kendisi indirir ve yönetir.

README Çince yazılmış. Yazar projenin erken sürüm olduğunu, boş zamanında geliştirdiğini ve "şimdilik çok kullanışlı olmadığını" açıkça söylüyor. Lisansı MIT.

## Gereksinimler

- 64 bit Windows (paket adları `win64` ve `x64`). README belirli bir Windows sürümü yazmıyor.
- Disk alanı: tam taşınabilir paket yaklaşık 830 MB (açılınca ~1,3 GB), kurulum dosyası yaklaşık 1,3 GB, hafif taşınabilir paket 96 MiB (açılınca ~227 MB). Hafif paket ilk açılışta adb, Android Emulator ve sistem imajını internetten indirir.
- Bellek: README'ye göre boşta ön ısıtma açıkken yaklaşık 1 GB bellek sürekli kullanılır (ayarlardan kapatılabilir).
- Donanım hızlandırma (WHPX ya da AEHD) önerilir. README'ye göre hızlandırma olmadan da çalışır ama açılış birkaç dakikaya uzar.
- Kaynaktan derlemek için Node.js ve npm (README sürüm belirtmiyor).

## Kurulum

README'de İngilizce "Installation" başlığı yok; kurulum "下载即用" (indir ve kullan) bölümünde hazır paketlerle anlatılıyor. Paketler Releases sayfasında: https://github.com/RoutineMiku/apkstart/releases

README'deki dört biçim (içerik ve sürüm numarası aynı):

- Mağaza sürümü `ApkStart_<dört haneli sürüm>_x64.msix`: Microsoft Store üzerinden kurulur, çalışma ortamı pakette, güncellemeyi mağaza yapar.
- Tam taşınabilir sürüm `ApkStart-v*-win64-full.zip`: zip'i açıp kullanırsın, emulator ve sistem imajı paketin içinde.
- Kurulum sürümü `ApkStart-v*-Setup.exe`: masaüstü ve Başlat menüsü kısayolu, kaldırma kaydı isteyenler için.
- Hafif taşınabilir sürüm `ApkStart-v*-win64-portable.zip`: önce denemek isteyenler için; çalışma ortamı ilk açılışta indirilir.

Kaynaktan derleme (README'nin "从源码构建" bölümü, olduğu gibi; açıklama satırları Çince):

```powershell
# 1. 安装依赖（含 Electron 运行时）
npm install

# 2. 启动并完成运行时部署（一次性下载 adb / Android Emulator / 系统镜像）
npm start

# 3.（可选）注册 .apk 双击安装
npm run assoc:on

# 4. 打包便携版（免安装，可直接分发）
npm run pack
# 产出 dist/ApkStart-win64-portable/ApkStart.exe
```

Sırasıyla: bağımlılıkları kurar, uygulamayı başlatıp çalışma ortamını bir kez indirir, isteğe bağlı olarak `.apk` çift tıklama ilişkilendirmesini açar, taşınabilir sürümü paketler. README kaynak kodun nasıl indirileceğini (`git clone`) ayrıca yazmıyor.

## İlk çalıştırma

README'nin komut satırı bölümü (olduğu gibi):

```
ApkStart.exe                      打开主界面
ApkStart.exe <路径\to.apk>        安装 APK
ApkStart.exe --launch <包名>      直接启动应用（桌面快捷方式使用此参数）
```

1. `ApkStart.exe` ana arayüzü açar.
2. `ApkStart.exe` komutuna bir APK dosyasının yolunu verirsen o APK kurulur. `.apk` ilişkilendirmesi açıksa Gezgin'de APK'ya çift tıklamak aynı işi yapar ve masaüstüne uygulamanın ikonu gelir.
3. `--launch` ile paket adını verirsen uygulama doğrudan başlar; masaüstü kısayolları bu parametreyi kullanır. Kısayola çift tıkladığında çalışma ortamı açık değilse kendiliğinden başlatılır.

README adım adım bir "ilk uygulama" örneği vermiyor; akış özellik listesinden ve komut satırı bölümünden çıkarıldı.

## Sık takılınan yerler

- "Donanım hızlandırma açık değil" uyarısı: README'ye göre kurulum ve taşınabilir sürümde Ayarlar → ortam denetimi bölümündeki onarma düğmesi bir kez yönetici izni isteyip WHPX'i açar; olmazsa AEHD sürücüsünü kurar. Mağaza sürümü yetki yükseltemediği için Windows özellikleri penceresini açar; orada "Windows Hypervisor Platform" işaretlenir ve bilgisayar yeniden başlatılır. README'nin yönetici PowerShell için verdiği komutlar:

```powershell
dism /online /enable-feature /featurename:HypervisorPlatform /all /norestart
dism /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

- Yalnız "Virtual Machine Platform" açmak yetmez; README'ye göre o WSL2 içindir, emulator "Hypervisor Platform" ister.
- Masaüstü ikonu yanlış ya da boş görünüyorsa: Ayarlar → sistem bütünleşmesi → kısayolları yeniden oluştur. Taşınabilir sürümün klasörünü başka yere taşımak kısayollardaki yolu bozar; yeniden oluşturmak düzeltir.
- `adb devices` cihazı göstermiyorsa: ApkStart'ın adb servisi 5044 numaralı ayrı port'ta çalışır (sistemdeki adb 5037'de). README'nin komutu:

```powershell
$env:ANDROID_ADB_SERVER_PORT = 5044
adb devices              # 会看到 emulator-5554
```

- README `5555` port'una elle `adb connect` yapılmamasını söylüyor: sistem imajı USB hata ayıklama izni penceresi açar ve çalışan uygulamanın üstünü kapatır.
- Yalnız ARM kütüphanesi içeren uygulamalar için ayarlarda "ARM uyumluluk kipi" var. README'ye göre gerçek sensör ya da baseband denetleyen bazı uygulamalar yine de çalışmayabilir.

## Bağlantılar

- Repo: https://github.com/RoutineMiku/apkstart
- Sürümler ve indirme: https://github.com/RoutineMiku/apkstart/releases (son sürüm v0.6.3, 22 Kasım 2026)
- Issues: https://github.com/RoutineMiku/apkstart/issues
- Lisans: https://github.com/RoutineMiku/apkstart/blob/main/LICENSE (README'ye göre dağıtım paketi yalnız ApkStart'ın kendi kodunu ve Electron'u içerir; Android sistem imajı, QEMU ve Google bileşenleri ilk açılışta resmî kaynaklardan indirilir ve kendi lisanslarına tabidir)
- Video: Haftanın Popüler Repoları, 25 Kasım 2026 günlük Short'u

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki trend Short t2026-11-25-1 videosu için hazırlandı: https://www.youtube.com/shorts/y95HvnlfryA. Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
