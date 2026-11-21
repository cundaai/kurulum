---
repo: we-promise/sure
url: https://github.com/we-promise/sure
lisans: AGPL-3.0
surum: v0.7.6 (2026-10-31)
readme_tarihi: 2026-11-12
dogrulama: denenmedi
bolum: gunun-reposu/g10
ozet: Maybe Finance'in topluluk çatalı olan, Docker ile kendi sunucunda barındırabileceğin açık kaynak kişisel finans uygulaması.
---

# Sure — kurulum ve ilk örnek

> **Bu belge, README'nin 12 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

Sure, geliştirmesi durdurulan Maybe Finance uygulamasının topluluk tarafından sürdürülen çatalıdır: hesaplar, işlemler, bütçe, net varlık, yatırım fiyatları ve AI asistanı olan tam bir kişisel finans uygulaması. Ruby on Rails ile yazılmıştır; tarayıcıdan, macOS uygulamasından, mobil uygulamadan, API'den ve LLM agent'larından kullanılabilir. Finans verisini kendi makinesinde tutmak isteyen ve Docker Compose ile tek servis ayağa kaldırabilen herkes için.

## Gereksinimler

- Kendi sunucunda barındırmak için Docker Engine ve Docker Compose (README'nin yönlendirdiği [docs/hosting/docker.md](https://github.com/we-promise/sure/blob/main/docs/hosting/docker.md) kılavuzu).
- Gizli anahtar üretmek için `openssl`; yoksa kılavuzda `/dev/urandom` ile alternatif komut var.
- Bellek: resmi image 512 MB RAM'de çalışır (ölçülen kararlı kullanım yaklaşık 352 MB); AI özellikleri için en az 1 GB.
- Geliştirici kurulumu için Ruby 3.4.9 (`.ruby-version` dosyası), PostgreSQL 9.3 üzeri ve Redis 5.4 üzeri.
- Hesap ya da anahtar gerekmez; SMTP, banka bağlantısı ve AI sağlayıcı anahtarları isteğe bağlıdır.

## Kurulum

### Docker Compose ile (önerilen)

Docker'ın çalıştığını doğrula:

```bash
# If Docker is setup correctly, this command will succeed
docker run hello-world
```

Uygulama için bir dizin aç ve örnek compose dosyasını indir:

```bash
# Create a directory on your computer for Docker files (name it whatever you like)
mkdir -p ~/docker-apps/sure

# Once created, navigate your current working directory to the new folder
cd ~/docker-apps/sure
```

```bash
# Download the sample compose.yml file from the GitHub repository
curl --fail --location --silent --show-error --output compose.yml https://raw.githubusercontent.com/we-promise/sure/main/compose.example.yml
```

Ortam dosyasını indir ve gizli anahtarı üret:

```bash
curl --fail --location --silent --show-error --output .env https://raw.githubusercontent.com/we-promise/sure/main/.env.example
```

```bash
openssl rand -hex 64
```

`.env` dosyasında en az şu iki değeri doldur (`SECRET_KEY_BASE` için varsayılan yok; ürettiğin değeri yaz, veritabanı şifresini kendin seç):

```txt
SECRET_KEY_BASE="replacemewiththegeneratedstringfromthepriorstep"
POSTGRES_PASSWORD="replacemewithyourdesireddatabasepassword"
```

Uygulamayı HTTPS arkasından (reverse proxy ile) sunacaksan `compose.yml` içinde `RAILS_ASSUME_SSL: "false"` satırını `"true"` yap. SMTP, passkey ve IPv6 ayarları kılavuzun aynı bölümünde.

### Geliştirici kurulumu (katkı için)

Ruby, PostgreSQL ve Redis kuruluyken:

```sh
cd sure
cp .env.local.example .env.local
bin/setup
bin/dev

# Optionally, load demo data
rake demo_data:default
```

Mac, Linux ve Windows için ayrı kurulum kılavuzları [wiki'de](https://github.com/we-promise/sure/wiki). Render, PikaPods ve Hostim için tek tıkla kurulum düğmeleri README'de.

## İlk çalıştırma

```bash
docker compose up
```

Resmi image çekilir ve loglar terminale akar. Tarayıcıda `http://localhost:3000` adresini aç; Sure giriş ekranı gelmeli. İlk kullanımda "create your account" ile e-posta ve şifre girip hesabını oluştur; bu ilk kullanıcı instance'ın super admin'i olur.

Çalıştığını görünce `Ctrl+C` ile durdur ve arka planda başlat:

```bash
docker compose up -d
```

`docker compose ls` ile çalıştığını doğrula. Başkalarının kayıt olmasını istemiyorsan **Settings > Self-Hosting > Onboarding** altından **Closed** seç. Geliştirici kurulumunda demo veriyi yüklediysen README'deki demo hesabıyla (`user@example.com`, şifresi README'de) giriş yapabilirsin.

Güncelleme için önce yedek al, sonra image'ı çek:

```bash
docker compose run --rm backup create # Back up the current version (optional, recommended)
docker compose pull web worker # Pull the newest image for your tag
docker compose up -d # Restart on the new version; database migrations run automatically
```

## Sık takılınan yerler

- `SECRET_KEY_BASE` için artık varsayılan yok; boş bırakırsan uygulama açılmaz. Veri oluştuktan sonra bu değeri (ya da `ACTIVE_RECORD_ENCRYPTION_*` anahtarlarını) değiştirirsen şifreli veriler okunamaz olur, girişler ve banka senkronları bozulur ([docker.md → Step 3](https://github.com/we-promise/sure/blob/main/docs/hosting/docker.md#step-3-required-configure-your-environment)).
- İlk açılışta `ActiveRecord::DatabaseConnectionError`: Docker, Postgres'i önceki bir denemeden kalan farklı bir rolle başlatmıştır. Kılavuzdaki `docker compose down` ve `docker volume rm sure_postgres-data` adımlarıyla veritabanını sıfırla; bu, var olan veriyi siler ([docker.md → Troubleshooting](https://github.com/we-promise/sure/blob/main/docs/hosting/docker.md#troubleshooting)).
- `.env` ya da `compose.yml` değiştirince `docker compose restart` yeni ayarları almaz; `docker compose up -d` kullan ([docker.md → Applying changes](https://github.com/we-promise/sure/blob/main/docs/hosting/docker.md#sending-email-smtp)).
- 512 MB'lık sunucuda demo veri üreteci belleği aşıp OOM ile ölür ve yükleme hiç bitmez; worker'ın bellek sınırını geçici yükselt ([docker.md → 512 MB hosts](https://github.com/we-promise/sure/blob/main/docs/hosting/docker.md#running-sure-on-small-512-mb-hosts)).
- Fork alırsan "Maybe" adı ve logosu Maybe Finance Inc. markasıdır, kullanılamaz; README fork'un ilişkisiz olduğunu belirtmeni istiyor ([README → Forking and Attribution](https://github.com/we-promise/sure#forking-and-attribution)).

## Bağlantılar

- Repo: https://github.com/we-promise/sure
- Belgeler: [docs/hosting/docker.md](https://github.com/we-promise/sure/blob/main/docs/hosting/docker.md) (self-host), [docs/clients.md](https://github.com/we-promise/sure/blob/main/docs/clients.md) (istemciler), [wiki](https://github.com/we-promise/sure/wiki), site https://sure.am
- Docker image: https://github.com/we-promise/sure/pkgs/container/sure (`stable` ve `latest` etiketleri)
- Sürümler: https://github.com/we-promise/sure/releases
- Lisans: https://github.com/we-promise/sure/blob/main/LICENSE (AGPL-3.0)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki Günün Reposu g10 videosu için hazırlandı: https://www.youtube.com/watch?v=7z32awD3FYM. Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
