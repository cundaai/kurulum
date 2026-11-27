---
repo: marclou/mailcheap
url: https://github.com/marclou/mailcheap
lisans: AGPL-3.0
surum: sürüm etiketi yok
readme_tarihi: 2026-11-06
dogrulama: denenmedi
bolum: trend-short/t2026-11-26-1
ozet: Bülten yazıp listene gönderen ve web'de yayımlayan, kendi sunucuna kurup kendi Amazon SES ya da PostShiba hesabınla çalıştırabildiğin açık kaynak bülten uygulaması.
---

# mailcheap — kurulum ve ilk örnek

> **Bu belge, README'nin 6 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

MailCheap bir bülten uygulaması: sayıyı yazarsın, listene gönderirsin, web'de yayımlarsın; bunu panelden ya da AI agent'ından yaparsın. README'ye göre bu repo uygulamanın tamamı: kendi Amazon SES ya da PostShiba hesabınla kendi sunucunda çalıştırırsın ya da aynı kodu çalıştıran MailCheap Cloud'u kullanırsın. Panelin yaptığı her şey MCP server üzerinden bir AI agent'tan ve REST API üzerinden koddan da yapılır; MCP gönderimi geri alınamaz bir işlem olarak işaretler, agent önce sana sorar. beehiiv, Substack ve Mailchimp'in CSV dışa aktarımları olduğu gibi içe alınır (Kit, MailerLite, Buttondown, Ghost, EmailOctopus ve listmonk da). Her bültenin arama motorlarının bulabileceği bir web sitesi olur; kendi domain'inden gönderim için DKIM, return path ve DMARC kayıtlarını ve denetimlerini verir. Teslim edilebilirlik korumaları (kademeli gönderim, liste temizliği, bounce ve şikâyet sınırları, tek tıkla abonelikten çıkma) yerleşik. Lisansı AGPL-3.0.

Kendi sunucunda çalıştırdığında kurulum ve bakım sende. README açıkça yazıyor: kendi sunucuna kurulumda destek yok, issues kapalı ve kurulum soruları yanıtsız kalır.

## Gereksinimler

README'nin "What you need" tablosu:

- MongoDB (README MongoDB Atlas'ı gösteriyor): kişiler, kampanyalar, istatistikler.
- Amazon SES ya da PostShiba: gönderim. README'ye göre PostShiba şimdilik yalnız davetle.
- Upstash QStash: arka planda gönderim ve zamanlama.
- Vercel ya da Node.js 22+ çalıştıran herhangi bir sunucu.
- İsteğe bağlı: Resend (giriş kodları ve uyarılar; yoksa SES ya da PostShiba üzerinden gider), Upstash Redis (sunucu örnekleri arasında ortak hız sınırı), Amazon S3 + CloudFront ya da Vercel Blob (görsel yükleme; yoksa görsel yükleme kapalı).
- Gönderim yapacağın bir domain ve DNS kayıtlarını düzenleme yetkisi.

README kurulum için yaklaşık bir saat diyor; çoğu DNS'i ve sağlayıcının üretim gönderimi onayını beklemekle geçiyor. Hizmetlerin ücretleri README'deki tabloda yazıyor; ücretler değiştiği için burada yinelenmedi.

## Kurulum

README'nin "Self-hosting" bölümü altı adım.

**1. Veritabanı.** MongoDB Atlas'ta bir cluster oluştur, parolalı ve "Read and write to any database" rollü bir kullanıcı ekle, Network Access'te `0.0.0.0/0` adresine izin ver (README: Vercel'in sabit IP adresi yok). Connect > Drivers'tan bağlantı dizesini kopyala ve `?` işaretinden önce bir veritabanı adı ekle (README örneği: `...mongodb.net/mailcheap?retryWrites=true`). Bu senin `MONGODB_URI` değerin.

**2. Deploy.** README'deki **Deploy with Vercel** düğmesine bas ve şu değişkenleri doldur: `MONGODB_URI`, `BETTER_AUTH_SECRET`, `SUPPRESSION_HASH_KEY`, `ADMIN_EMAILS` (kendi e-postan) ve `EMAIL_FROM` (Resend hesabının ya da e-posta sağlayıcının doğruladığı bir domain'de). İki gizli değer en az 32 karakter olmalı ve birbirinden farklı olmalı; README ikisini de şu komutla üretmeyi söylüyor:

```bash
openssl rand -base64 48
```

Gizli değer kısaysa MailCheap başlamayı reddeder ve nedeni log'da yazar. Giriş kodlarının sana ulaşması için README'nin en hızlı dediği yol Resend: `EMAIL_FROM` domain'ini orada doğrula ve `RESEND_API_KEY` ekle (`<API_ANAHTARI>`). Sonra `https://<your-project>.vercel.app/signin` adresini açıp e-postanla giriş yap.

Vercel dışında, Node.js çalıştıran başka bir sunucuda aynı değişkenlerle ve HTTPS arkasında:

```bash
npm ci && npm run build && npm start
```

`/api/health` adresi 200 döner; sorun varsa 503 ile neyin düzeltileceğini söyler. Panel için kendi domain'ini kullanacaksan şimdi ekle ve `APP_URL` değerini ona ayarla: e-postalardaki bağlantılar `APP_URL` kullanır, ilk gönderimden önce son hâlinde olmalı.

**3. Kim giriş yapabilir.** Yalnız listelediğin e-postalar: `ADMIN_EMAILS` (bunlar `/admin` sayfasını da açar) ve `ALLOWED_EMAILS` (virgülle ayrılmış adresler ve `@domain` girdileri). İki liste de boşsa kimse giriş yapamaz.

**4. E-posta sağlayıcısı.** Amazon SES ya da PostShiba. SES için README'nin adımları: üretim erişimi iste (yeni hesaplar sandbox'ta), olaylar için bir SNS topic oluştur ve imza sürümünü 2 yap:

```bash
aws sns set-topic-attributes --topic-arn <topic-arn> --attribute-name SignatureVersion --attribute-value 2
```

Sonra bu topic'e HTTPS protokolüyle `https://<your-app>/api/webhooks/ses` endpoint'ini abone et (raw message delivery kapalı), README'deki IAM policy ile MailCheap için bir IAM kullanıcısı ve access key oluştur, `SES_REGION`, `SES_ACCESS_KEY_ID`, `SES_SECRET_ACCESS_KEY`, `SES_AWS_ACCOUNT_ID` ve `SES_SNS_TOPIC_ARN` değişkenlerini ekleyip yeniden deploy et. IAM policy'nin tam metni ve PostShiba adımları README'nin "4. Email provider" bölümünde.

**5. QStash.** Upstash konsolundan `QSTASH_TOKEN`, `QSTASH_CURRENT_SIGNING_KEY` ve `QSTASH_NEXT_SIGNING_KEY` değerlerini kopyala (varsayılan dışında bir bölge seçtiysen `QSTASH_URL` de), ekle ve yeniden deploy et. QStash her partiyi göndermek için `APP_URL/api/jobs/...` adresini çağırır; uygulama internetten erişilebilir olmalı.

**6. Gönderimi aç.** Aşağıdaki "İlk çalıştırma" bölümü.

Bütün değişkenler, isteğe bağlı olanlarla birlikte reponun `.env.example` dosyasında açıklamalarıyla listeli.

## İlk çalıştırma

README'nin "6. Turn sending on" adımları:

1. MailCheap'te **Domains** sayfasını aç ve bülteninin gönderileceği domain'i ekle. README `mail.example.com` gibi bir subdomain öneriyor; böylece ana domain'inin e-postası etkilenmez. Listelenen DNS kayıtlarını ekle (DKIM, return path, kök domain'inde yoksa DMARC; DNS'in Cloudflare'deyse "DNS only"), sonra **Check now** düğmesine bas.
2. Bültenini oluştur. Gönderen adresi o domain'de olmalı; posta adresi her e-postanın altında görünür.
3. `SENDING_ENABLED=true` yap ve yeniden deploy et. Bu değer `true` olmadan okurlara hiçbir şey gitmez.
4. Bir kampanya yaz, kendine test gönder, sonra gönder. Yeni kişilere ilk gönderim kendiliğinden kademeli çıkar; yeni bir domain'de README önce en ilgili okurlara ulaşmak için sınır koymayı, ilk adım sağlıklı görününce rapordaki **Send to more** ile devam etmeyi öneriyor.

## Sık takılınan yerler

- `BETTER_AUTH_SECRET` ya da `SUPPRESSION_HASH_KEY` 32 karakterden kısaysa, ya da ikisi aynıysa sunucu başlamaz (README). `BETTER_AUTH_SECRET` değişirse herkesin oturumu kapanır ve gönderilmiş e-postalardaki bağlantılar bozulur; `SUPPRESSION_HASH_KEY` değişirse "asla gönderme" listesi boşalır.
- Yeni AWS hesapları SES sandbox'ındadır: günde 200 e-posta ve yalnız doğrulanmış alıcılar. Üretim erişimi istemek gerekir; README AWS'nin genellikle bir gün içinde yanıt verdiğini söylüyor.
- Resend yoksa giriş kodları SES üzerinden `EMAIL_FROM` adresinden çıkar; o domain ilk girişten önce AWS hesabında doğrulanmış olmalı.
- Redis yoksa hız sınırları her sunucu örneğinin belleğinde ayrı sayılır ve her deploy'da sıfırlanır; README yoğun bir siteye kayıt formu koymadan önce Redis eklemeyi söylüyor.
- S3 ya da Vercel Blob yoksa görsel yükleme kapalıdır; HTML ve Markdown kampanyaları başka yerde barınan görselleri kullanabilir.
- Kendi sunucuna kurulumda destek yok: issues kapalı (README).

## Bağlantılar

- Repo: https://github.com/marclou/mailcheap
- Site: https://mailcheap.ai
- API belgeleri: https://mailcheap.ai/docs · MCP server: https://mailcheap.ai/docs/mcp
- Değişken listesi: https://github.com/marclou/mailcheap/blob/main/.env.example

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki trend Short t2026-11-26-1 videosu için hazırlandı: https://www.youtube.com/shorts/2ZupLXdnIhk. Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
