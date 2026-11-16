---
repo: floci-io/floci
url: https://github.com/floci-io/floci
lisans: MIT
surum: 2.4.0 (2026-10-31)
readme_tarihi: 2026-11-12
dogrulama: denenmedi
bolum: gunun-reposu/g04
ozet: Hesap ve token istemeyen ücretsiz yerel AWS emülatörü; AWS SDK, CLI ve Terraform'u localhost:4566'ya yöneltirsin.
---

# Floci — kurulum ve ilk örnek

> **Bu belge, README'nin 12 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

Floci, geliştirme, test ve CI için makinede çalışan ücretsiz, açık kaynak bir AWS emülatörü. S3, SQS, SNS, DynamoDB, IAM, Lambda, API Gateway, Cognito gibi servisleri `http://localhost:4566` üzerinde sunar; AWS SDK, AWS CLI, Terraform, CDK ve test paketin değişmeden çalışır. LocalStack Community'nin Mart 2026'da auth token zorunluluğuyla kapanması üzerine doğrudan yerine geçecek biçimde tasarlandı: imajı değiştirip devam edersin. Lambda, RDS, ElastiCache, ECS, EKS gibi servislerde taklit yerine gerçek Docker konteynerleri kullanır; ~24 ms'de açılır, boşta ~13 MiB bellek tutar.

## Gereksinimler

- Docker 23.0+ ve `docker compose` v2+ (eklenti söz dizimi; eski `docker-compose` değil) ([docs/getting-started/installation.md](https://github.com/floci-io/floci/blob/main/docs/getting-started/installation.md)). Lambda, RDS gibi Docker destekli servisler için Docker soketi konteynere bağlanır.
- Docker istemiyorsan her sürümde `floci-aws` adlı tek dosyalık ikili var (Linux glibc 2.34+, macOS Apple Silicon/Intel, Windows x86_64); Docker olmadan yalnız konteyner başlatmayan servisler çalışır.
- Resmi `floci` CLI'ı için Homebrew (macOS/Linux), kurulum betiği ya da PowerShell; JVM yedeği Java 25+ ister.
- Kendi AWS aracın (AWS CLI, SDK, Terraform). Kimlik bilgisi: boş olmayan herhangi bir değer (`test`/`test`); AWS hesabı gerekmez.
- Kaynaktan derlemek için Java 25+ ve Maven 3.9+.

## Kurulum

### 1. yol: `floci` CLI (en kısa)

CLI ayrı bir repoda ([floci-io/floci-cli](https://github.com/floci-io/floci-cli)); oradaki komutlar:

```sh
brew install floci-io/floci/floci
```

Homebrew yoksa (Linux / macOS):

```sh
curl -fsSL https://floci.io/install.sh | sh
```

Windows (PowerShell):

```powershell
iwr https://floci.io/install.ps1 | iex
```

Sonra emülatörü başlat ve AWS ortam değişkenlerini kabuğa yaz:

```bash
floci start
```

```bash
eval $(floci env)
```

### 2. yol: Docker Compose

`compose.yaml` dosyası oluştur:

```yaml
services:
  floci:
    image: floci/floci:latest
    ports:
      - "4566:4566"
```

Başlat ve AWS ortamını elle ayarla:

```bash
docker compose up
```

```bash
export AWS_ENDPOINT_URL=http://localhost:4566
export AWS_DEFAULT_REGION=us-east-1
export AWS_ACCESS_KEY_ID=test
export AWS_SECRET_ACCESS_KEY=test
```

Lambda, RDS, ECS gibi Docker destekli servisleri kullanacaksan soketi de ver:

```bash
docker run -d --name floci \
  -p 4566:4566 \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -u root \
  floci/floci:latest
```

### 3. yol: Docker'sız ikili

[Sürüm sayfasından](https://github.com/floci-io/floci/releases) platformuna uyan `floci-aws-*` dosyasını indir; kurulum belgesindeki Linux örneği (`VERSION` ve `ASSET` değerlerini kendine göre değiştir):

```bash
VERSION=x.y.z   # the release to install
ASSET=floci-aws-linux-amd64
curl -fsSLO "https://github.com/floci-io/floci/releases/download/$VERSION/$ASSET"
curl -fsSLO "https://github.com/floci-io/floci/releases/download/$VERSION/sha256sums.txt"
sha256sum --ignore-missing -c sha256sums.txt     # shasum -a 256 --ignore-missing -c on macOS
mkdir -p ~/.local/bin
install -m 755 "$ASSET" ~/.local/bin/floci-aws
~/.local/bin/floci-aws
```

Kubernetes için `charts/floci` Helm chart'ı var ([Kubernetes (Helm)](https://floci.io/floci/getting-started/kubernetes/)).

## İlk çalıştırma

Floci açık ve ortam değişkenleri ayarlıyken alışık olduğun AWS CLI komutları:

```bash
aws s3 mb s3://my-bucket

aws dynamodb create-table \
  --table-name demo-table \
  --attribute-definitions AttributeName=pk,AttributeType=S \
  --key-schema AttributeName=pk,KeyType=HASH \
  --billing-mode PAY_PER_REQUEST

aws dynamodb list-tables
```

Son komutun `demo-table`'ı listelemesi beklenir. Ortam değişkenlerini ayarlamadıysan her komuta `--endpoint-url http://localhost:4566` ekle. Tarayıcıdan kaynaklara bakmak için `http://localhost:4566/_floci/ui` (konsol ilk istekte yan konteyner olarak açılır; Docker soketi ister). CLI ile durdurmak için `floci stop`; durum için `floci status`, ortam kontrolü için `floci doctor`.

Veri varsayılan `memory` modunda tutulur, konteyner durunca silinir; yeniden başlatmalar arasında kalsın istiyorsan `FLOCI_STORAGE_MODE=hybrid` (ya da `persistent`, `wal`).

## Sık takılınan yerler

- **Lambda, RDS, ECS çalışmıyor.** Docker destekli servisler konteyner başlatır; `/var/run/docker.sock` bağlı değilse o istekler başarısız olur, Floci açılışta hangi servislerin etkilendiğini loga yazar ([README, Real Docker Integration](https://github.com/floci-io/floci#real-docker-integration)).
- **Uygulama konteynerinden dönen URL'ler `localhost` gösteriyor.** Çok konteynerli Compose'da `FLOCI_HOSTNAME=floci` ver; yoksa SQS `QueueUrl` gibi değerler yanlış konteyneri işaret eder ([README, Configuration](https://github.com/floci-io/floci#multi-container-docker-compose)).
- **İmaj çekilemiyor.** Katmanlar zstd ile sıkıştırılmış; Docker 23.0'dan eski motorlar çekemez ([installation.md](https://github.com/floci-io/floci/blob/main/docs/getting-started/installation.md#requirements)).
- **İkili macOS'ta açılmıyor ya da Linux'ta çalışmıyor.** Tarayıcıyla indirilen ikiliyi Gatekeeper karantinaya alır: `xattr -d com.apple.quarantine floci-aws` (curl ile indirilen karantinaya girmez). Linux'ta glibc 2.34+ gerekir; RHEL 8, Amazon Linux 2 ve Alpine'da imajı kullan ([installation.md](https://github.com/floci-io/floci/blob/main/docs/getting-started/installation.md#requirements-1)).
- **Eski `hectorvent/floci` imajı güncellenmiyor.** Compose'da `image: floci/floci:latest` yap ([README, Quick Start](https://github.com/floci-io/floci#quick-start)).

## Bağlantılar

- Repo: https://github.com/floci-io/floci — CLI: https://github.com/floci-io/floci-cli
- Belgeler: https://floci.io/floci/ — kurulum: https://floci.io/floci/getting-started/installation/ — LocalStack'ten geçiş: https://floci.io/floci/getting-started/migrate-from-localstack/
- Sürümler: https://github.com/floci-io/floci/releases
- Lisans: MIT — https://github.com/floci-io/floci/blob/main/LICENSE

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki Günün Reposu g04 videosu için hazırlandı: https://www.youtube.com/watch?v=s1W4tKSSr-A. Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
