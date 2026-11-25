---
repo: nginx/nginx
url: https://github.com/nginx/nginx
lisans: BSD-2-Clause
surum: release-1.31.10 (2026-11-23)
readme_tarihi: 2025-12-04
dogrulama: denenmedi
bolum: 2026-W48-2
ozet: Web server, reverse proxy, load balancer, API gateway ve content cache olarak çalışan açık kaynak NGINX'in resmî kaynak kod reposu.
---

# nginx — kurulum ve ilk örnek

> **Bu belge, README'nin 4 Aralık 2025 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

NGINX ("engine x" diye okunur), README'nin tanımıyla bir web server, yüksek performanslı load balancer, reverse proxy, API gateway ve content cache'tir. Tek büyük process yerine bir "master" process ve bir ya da daha çok "worker" process olarak çalışır: master ayar dosyalarını okur ve worker'ları yönetir, worker'lar istekleri işler. Davranışı metin tabanlı ayar dosyalarındaki "directive"lerle belirlenir; özellikler modüllerle gelir (derleme anında eklenen static modüller ve sonradan yüklenebilen dynamic modüller). Ücretsiz ve açık kaynaktır, 2 maddeli BSD benzeri lisansla dağıtılır; kurumsal dağıtım ve destek F5'ten alınabilir.

README'nin amacı yeni başlayanlara kısa bir giriş vermektir; kurulum, derleme, ayar ve hata ayıklamanın ayrıntısı için https://nginx.org/en/docs/ adresindeki belgelere yönlendirir.

## Gereksinimler

- Hazır paketle kurulum için: desteklenen bir işletim sistemi. README başlıca işletim sistemleri ve Linux dağıtımları için paket olduğunu söyler; tam liste için "Tested OS and Platforms" sayfasına (https://nginx.org/en/#tested_os_and_platforms) yönlendirir.
- Kaynaktan derlemek için (README'nin komutları `apt` kullanan Ubuntu/Debian ve türevlerine göre yazılmış): GNU C derleyicisi ve Make (`gcc`, `make`), en az `libpcre3-dev` ve `zlib1g-dev` kütüphaneleri (rewrite ve gzip için), TLS kullanacaksan OpenSSL kütüphanesi (`libssl-dev`), repoyu çekmek için git.
- Kurulum ve çalıştırma adımları `sudo` ister.
- Windows sürümü README'ye göre "Proof-of-Concept" aşamasındadır; yalnız geliştirme ve deneme için kullanılmalıdır.
- README macOS için ayrı bir kurulum yolu anlatmıyor.

## Kurulum

README iki yol anlatır: hazır paket (binary) kurmak ya da kaynaktan derlemek.

**Hazır paket.** README bu yol için komut vermez, resmî sayfalara yönlendirir:

- Linux: önce resmî NGINX paket deposu sistemin paket yöneticisine eklenir; indirme, doğrulama ve kurulum adımları https://nginx.org/en/linux_packages.html sayfasındadır. Sonraki güncellemeler aynı paket yöneticisiyle yapılır.
- FreeBSD: https://nginx.org/en/docs/install.html
- Windows: çalıştırılabilir dosyalar https://nginx.org/en/download.html sayfasında; ek bilgi https://nginx.org/en/docs/windows.html
- İki sürüm hattı var: stable (yalnız kritik düzeltmeler) ve mainline (master dalından, en yeni özellikler ve düzeltmeler). Hangisinin sana uygun olduğuna sen karar verirsin.

README, Linux dağıtımlarıyla gelen topluluk paketleri yerine resmî paketlerin ya da bu repodaki kaynağın kullanılmasını önerir (en yeni sürüm ve güvenlik yamaları için).

**Kaynaktan derleme** (README'nin "Building from source" bölümü, `apt` kullanan sistemler için).

Önce paket listesini güncelle:

```bash
sudo apt update
```

Derleyici ve Make:

```bash
sudo apt install gcc make
```

Bağımlı kütüphaneler:

```bash
sudo apt install libpcre3-dev zlib1g-dev
```

TLS ile trafik şifreleyeceksen OpenSSL kütüphanesi:

```bash
sudo apt install libssl-dev
```

Repoyu geliştirme klasörüne çek:

```bash
git clone https://github.com/nginx/nginx.git
```

Kaynak kodun kök klasöründe `configure` betiğini çalıştır (bir Makefile üretir; bayraksız çalıştırınca varsayılan seçeneklerle derlenir):

```bash
auto/configure
```

Aynı klasörde derle:

```bash
make
```

Derleme başarılı olunca binary `<NGINX_SRC_ROOT_DIR>/objs/nginx` yolunda oluşur. Kurmak için, yine kök klasörden:

```bash
sudo make install
```

Binary `/usr/local/nginx/` klasörüne kurulur.

## İlk çalıştırma

Kaynaktan kurulan binary'yi çalıştır:

```bash
sudo /usr/local/nginx/sbin/nginx
```

Çalıştığını `curl` ile dene:

```bash
curl localhost
```

README'ye göre çıktı şöyle başlamalı:

```html
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
```

Binary'nin hangi static modüllerle derlendiğini görmek için:

```bash
nginx -V
```

README bundan sonrasını (ayar dosyası yazmak, durdurmak, yeniden yüklemek) anlatmıyor; "Beginner's Guide"a (https://nginx.org/en/docs/beginners_guide.html) yönlendiriyor. Hazır paketle kurulan NGINX'in nasıl başlatılacağı da README'de yok; paket sayfasındaki adımlara bak. Sık kullanılan konular için README'nin verdiği kılavuzlar: HTTPS (https://nginx.org/en/docs/http/configuring_https_servers.html), load balancer (https://nginx.org/en/docs/http/load_balancing.html), rate limiting (https://blog.nginx.org/blog/rate-limiting-nginx), content cache (https://blog.nginx.org/blog/nginx-caching-guide).

## Sık takılınan yerler

- `libpcre3-dev` ve `zlib1g-dev` yalnız en küçük kümedir (rewrite ve gzip). Başka modüllerle derleyeceksen ek bağımlılık gerekebilir; hangi modülün eksik kaldığını `configure` çıktısından izle (README uyarısı).
- TLS kullanacaksan `libssl-dev` kurulmadan derleme yapma; README OpenSSL kütüphanesini ayrıca kurduruyor.
- `auto/configure` bayraksız çalışınca varsayılan seçenek kümesiyle derler. Bütün derleme seçenekleri: https://nginx.org/en/docs/configure.html
- Kullanabileceğin directive'ler, dağıtımına hangi modüllerin eklendiğine bağlıdır; bir directive tanınmıyorsa önce `nginx -V` ile modüllere bak.
- Windows sürümü üretim için değil, yalnız geliştirme ve deneme içindir.
- Dağıtımın kendi deposundaki topluluk paketi eski olabilir; README resmî paketleri ya da bu repodaki kaynağı önerir.

## Bağlantılar

- Repo: https://github.com/nginx/nginx
- Site ve belgeler: https://nginx.org/ · https://nginx.org/en/docs/ (Beginner's Guide: https://nginx.org/en/docs/beginners_guide.html, directive listesi: https://nginx.org/en/docs/dirindex.html)
- Kurulum sayfaları: https://nginx.org/en/linux_packages.html · https://nginx.org/en/docs/install.html · https://nginx.org/en/download.html
- Sürümler: https://github.com/nginx/nginx/releases (son sürüm release-1.31.10, 23 Kasım 2026) · değişiklik günlüğü: https://nginx.org/en/CHANGES
- Issues: https://github.com/nginx/nginx/issues · destek kuralları: https://github.com/nginx/nginx/blob/master/SUPPORT.md · topluluk forumu: https://community.nginx.org · blog: https://blog.nginx.org/
- Video: Haftanın Popüler Repoları, 23–25 Kasım 2026 bölümü (listenin 7. sırası)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W48-2 bölümü](../bolumler/2026-W48-2.md) videosu için hazırlandı (video henüz yayında değil). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
