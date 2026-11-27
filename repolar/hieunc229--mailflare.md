---
repo: hieunc229/mailflare
url: https://github.com/hieunc229/mailflare
lisans: AGPL-3.0
surum: sürüm etiketi yok
readme_tarihi: 2026-11-24
dogrulama: denenmedi
bolum: 2026-W48-3
ozet: Kendi alan adınla e-posta, takvim, randevu, Drive ve AI agent'ları sunan, Cloudflare'de ya da tek Docker container'ında kendin barındırdığın Google Workspace alternatifi.
---

# mailflare — kurulum ve ilk örnek

> **Bu belge, README'nin 24 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

Mailflare, README'nin deyişiyle Google Workspace'e kendi barındırdığın bir alternatif. Kendi alan adınla kişisel ya da ortak posta kutuları açarsın; gelen postayı saklayan, yönlendiren, reddeden ya da sıralayan kurallar yazarsın (bülten ve tanıtım postaları için tek kurallık bir filtre dahil). Yanında tekrarlayan etkinlikli takvim, herkese açık bağlantıyla randevu alma, dosya paylaşmak için Drive ve postada arama yapan, yanıt taslağı yazan, etkinlik yöneten AI agent'ları var; başka AI araçları MCP ile bağlanabilir. Gönderim Cloudflare Email, Resend ya da Amazon SES ile; veritabanı varsayılan olarak Cloudflare D1, istenirse PostgreSQL, MySQL, Turso/libSQL ya da yerel SQLite. IMAP, Gmail Takeout, `.eml` ve `.mbox` dosyalarından içe aktarma yapar. Arayüz Türkçe dahil 34 dilde. Lisansı AGPL-3.0.

## Gereksinimler

- Cloudflare yolu: bir Cloudflare hesabı ve aynı hesapta bir alan adı. Uygulama Cloudflare Workers'ta çalışır; D1, R2 ve queue'lar kullanır.
- `CF_TOKEN` zorunlu: README'ye göre belirli izinlerle kısıtlanmış bir Cloudflare API token'ı (`<CLOUDFLARE_API_TOKENI>`). İzin listesi aşağıda.
- Docker yolu: herhangi bir sunucuda Docker ve Docker Compose; tek container, SQLite ve yerel dosyalarla çalışır.
- Yerel geliştirme: Node.js ve npm (README sürüm vermiyor).
- Maliyet (README): posta almak ve göndermek ücretsiz olabilir. Cloudflare Email Routing ile almak ücretsiz; göndermek için Resend'in ücretsiz katmanı (ayda 3.000 e-posta) ya da Amazon SES. Cloudflare'in kendi gönderimi ayda 5 dolarlık Paid Worker planı ister. Yeni Amazon SES hesapları sandbox'ta başlar; üretim erişimi AWS'den istenir.

## Kurulum

**Cloudflare'e kurulum** (README'nin "Deploy" bölümü, düğmeyle; komut yok):

1. README'deki **Deploy to Cloudflare** düğmesine tıkla (https://deploy.workers.cloudflare.com/?url=https://github.com/hieunc229/mailflare). Uygulama adını `mailflare` olarak bırak; README'ye göre değiştirirsen bozulur.
2. Uygulamayı `/setup` adresinde aç ve yönetici hesabını oluştur.
3. Aynı Cloudflare hesabındaki bir alan adını ekle, sonra ilk posta kutunu aç.

`CF_TOKEN` için README'nin istediği izinler:

- All accounts: Email Sending:Edit, DNS Settings:Edit, Email Routing Addresses:Edit
- All zones: DNS Settings:Edit, Email Routing Rules:Edit, Zone Settings:Edit, DNS:Edit

Diğer seçenekler README'de değil, `docs/deployment.md`'de.

**Docker ile kendi sunucunda** (repoyu sunucuna aldıktan sonra repo klasöründe):

```bash
cp .env.docker.example .env.docker
docker compose up -d --build
```

Derlemeden hazır image ile: `docker compose -f docker-compose.image.yml up -d`. Ayrıntılar `docs/self-hosting.md`'de.

**Yerel geliştirme:**

```bash
cp .dev.vars.example .dev.vars
npm install
npm run db:migrate:local
npm run dev
```

Cloudflare bilgilerini `.dev.vars` dosyasına yaz, sonra http://localhost:3000 adresini aç.

## İlk çalıştırma

1. Kurulumdan sonra uygulamanın `/setup` sayfasını aç ve yönetici hesabını oluştur. README'ye göre uzak veritabanı migration'larını bu sayfa çalıştırır.
2. Alan adını bağla ve ilk posta kutusunu aç.
3. Posta göndermek için bir gönderim sağlayıcısı seç (Cloudflare Email, Resend ya da Amazon SES; ayrıntılar `docs/providers.md`).

Yerel geliştirmede örnek veri yüklemek için `npm run db:seed` (README).

## Sık takılınan yerler

- Worker adı tam olarak `mailflare` kalmalı; README'ye göre başka ad uygulamayı bozar.
- `CF_TOKEN` olmadan Cloudflare kurulumu çalışmaz; token'ı yukarıdaki izinlerle kısıtla (README, issue #24'teki açıklamaya bağlanıyor).
- Yeni Amazon SES hesabı sandbox'ta başlar; gerçek alıcılara göndermek için AWS'den üretim erişimi iste (README).
- Şema değişikliği her veritabanı türü için ayrı migration ister (README, `docs/databases.md`).
- README'de bir AI agent'a yapıştırılacak hazır kurulum metni de var; bu belgeye alınmadı. Agent'a kurdurmayı düşünüyorsan token'ları komut argümanı olarak vermemeye dikkat et, ayrıntılar `docs/deployment.md`'de.
- Lisans AGPL-3.0: değiştirdiğin bir sürümü ağ üzerinden başkalarına hizmet olarak sunarsan, değiştirilmiş kaynak kodu da o kullanıcılara açman gerekir. Ticari kullanımdan önce LICENSE dosyasını oku.
- Sorun giderme için README `docs/troubleshooting.md`'ye yönlendiriyor.

## Bağlantılar

- Repo: https://github.com/hieunc229/mailflare
- Site: https://mailflare.co
- Kurulum ve ayar: https://github.com/hieunc229/mailflare/blob/main/docs/deployment.md
- Docker ile barındırma: https://github.com/hieunc229/mailflare/blob/main/docs/self-hosting.md
- Veritabanları: https://github.com/hieunc229/mailflare/blob/main/docs/databases.md · Gönderim sağlayıcıları: https://github.com/hieunc229/mailflare/blob/main/docs/providers.md
- E-posta asistanı ve MCP: https://github.com/hieunc229/mailflare/blob/main/docs/email-assistant-and-mcp.md · Google Takeout'tan aktarma: https://github.com/hieunc229/mailflare/blob/main/docs/import.md
- Sorun giderme: https://github.com/hieunc229/mailflare/blob/main/docs/troubleshooting.md
- Sürümler: sürüm etiketi yok (kod `main` dalından)
- Issues: https://github.com/hieunc229/mailflare/issues
- Video: Haftanın Popüler Repoları, 25–27 Kasım 2026 Cuma bölümü (listenin 10. sırası)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W48-3 bölümü](../bolumler/2026-W48-3.md) videosu için hazırlandı (yakında — 27 Kas 12:30). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
