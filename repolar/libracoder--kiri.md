---
repo: libracoder/kiri
url: https://github.com/libracoder/kiri
lisans: Apache-2.0
surum: v0.2.1 (2026-11-15)
readme_tarihi: 2026-11-14
dogrulama: denenmedi
bolum: trend-short/t2026-11-19-1
ozet: macOS, Windows ve Linux için Rust ile yazılmış, Docker CLI ve Compose ile çalışan küçük ve hızlı bir container runtime.
---

# kiri — kurulum ve ilk örnek

> **Bu belge, README'nin 14 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

Kiri, macOS, Windows ve Linux için küçük ve hızlı bir container runtime. README'ye göre Docker Desktop ve OrbStack'in hafif bir alternatifi: Docker imajlarını çalıştırır, Docker Engine API'ını konuşur, bu yüzden zaten kullandığın `docker` CLI, Compose ve buildx onunla çalışır. Container'ları tek bir küçük Linux VM'de (Linux'ta doğrudan çekirdek üzerinde) çalıştırır; VM 60 saniye boşta kalınca uyur. README'nin öne çıkardıkları: `http://<name>.kiri.local` biçiminde yerel alan adları (Kiri'nin kendi sertifika otoritesiyle HTTPS), tek komutla tek düğümlü Kubernetes, Linux makineleri, container, imaj, volume ve network'ler için bir pencere. Lisansı Apache-2.0; "Kiri" adı ve logosu lisansın dışında.

ngrok alternatifi konusu: reponun GitHub açıklaması "free Ngrok alternative" diyor. Kaynak kodundaki açıklamaya göre (`crates/kiri/src/share.rs`) `kiri share`, yerel bir porta ya da container'a public HTTPS adresi verir ve trafik Kiri'nin kendi sunucusundan (`edge.runkiri.com`) geçer. README'nin 14 Kasım 2026 hâli bu komutu anlatmıyor; kullanımını bu yüzden buraya yazmadık, güncel bilgi için reponun README'sine ve sitesine bak.

## Gereksinimler

README'deki tabloya göre:

- macOS 14 ve üzeri (Apple silicon ya da Intel).
- Windows 10 ve 11 (x86-64 ya da Arm; WSL 2 üzerinden).
- Linux (Ubuntu, Fedora ve diğerleri; `.deb` ya da `.rpm`).

README başka bir gereksinim saymıyor.

## Kurulum

README bir kurulum komutu vermiyor; kurulum, sürümler sayfasından indirilen dosyayla yapılıyor (https://runkiri.com/releases/):

- macOS: `.dmg`
- Windows: `.msi`
- Linux: `.deb` ya da `.rpm`

README'ye göre Kiri ilk açıldığında bir `kiri` Docker context'i kurar; kendini günceller ve her yeni sürümün imzasını önce denetler.

## İlk çalıştırma

README'nin "Quick start" bölümü, olduğu gibi:

```sh
docker --context kiri run --rm alpine echo hello
docker --context kiri compose up -d
kiri k8s start                      # single-node Kubernetes as the `kiri` kubectl context
kiri machine create dev --distro ubuntu && kiri machine shell dev
kiri import                         # bring images, volumes and containers over from OrbStack or Docker Desktop
```

İlk satır `kiri` context'iyle bir alpine container'ı çalıştırıp "hello" yazdırır; ikincisi bulunduğun klasördeki Compose projesini başlatır. `kiri k8s start` tek düğümlü Kubernetes'i `kiri` kubectl context'i olarak açar, `kiri machine` bir Linux makinesi oluşturup shell'ine girer, `kiri import` imaj, volume ve container'ları OrbStack ya da Docker Desktop'tan taşır. README'ye göre Docker komutları `kiri` komutu olarak da çalışır (`kiri run`, `kiri ps`, `kiri compose up`) ve her zaman Kiri'yi hedefler.

## Sık takılınan yerler

Reponun özellik belgesindeki (docs/features.md) "Known issues" bölümünden:

- Mac'te bağlı bir USB cihazı, Mac uyuyunca container'lardan düşer; Mac uyanınca Kiri cihazı kendisi yeniden bağlar. Uzun süren işlerde Mac'i uyanık tutmak gerekir (belge `caffeinate -s` öneriyor).
- BuildKit ile `docker build --add-host name:host-gateway` hata verir (`host-gateway is not supported by the docker-container driver`). Belgeye göre yerleşik builder gelene kadar adresin kendisi (`--add-host name:172.18.0.1` Windows ve Linux'ta, Mac'te `198.19.248.254`) ya da `DOCKER_BUILDKIT=0` kullanılır.
- Linux'ta `kiri` grubuna üyelik, `docker` grubunda olduğu gibi root yetkisine eşdeğerdir.

## Bağlantılar

- Repo: https://github.com/libracoder/kiri
- Site: https://runkiri.com · indirme: https://runkiri.com/releases/
- Özellikler ve bilinen sorunlar: https://github.com/libracoder/kiri/blob/main/docs/features.md
- Kaynaktan derleme ve mimari: https://github.com/libracoder/kiri/blob/main/docs/development.md
- Sürümler: https://github.com/libracoder/kiri/releases (son sürüm v0.2.1, 15 Kasım 2026)
- Issues: https://github.com/libracoder/kiri/issues
- Video: Haftanın Popüler Repoları, 19 Kasım 2026 trend Short'u

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki trend Short t2026-11-19-1 videosu için hazırlandı: https://www.youtube.com/shorts/nR3skSXZcxg. Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
