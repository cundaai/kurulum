---
repo: chatwoot/chatwoot
url: https://github.com/chatwoot/chatwoot
lisans: belirtilmemiş
surum: v4.19.1 (2026-10-24)
readme_tarihi: 2026-11-12
dogrulama: denenmedi
bolum: gunun-reposu/g06
ozet: Intercom ve Zendesk'e açık kaynak alternatif: canlı sohbet, e-posta ve WhatsApp'ı tek gelen kutusunda toplayan destek masası.
---

# Chatwoot — kurulum ve ilk örnek

> **Bu belge, README'nin 12 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

Chatwoot, kendi sunucunda barındırabildiğin açık kaynak müşteri destek platformu; Intercom, Zendesk ve Salesforce Service Cloud'a alternatif. Web sitesi canlı sohbeti, e-posta, Facebook, Instagram, WhatsApp, Telegram, Line ve SMS'ten gelen konuşmaları tek gelen kutusunda toplar; etiket, hazır yanıt, otomatik atama, takımlar, raporlar, yardım merkezi portalı ve "Captain" adlı yapay zekâ ajanı var. Müşteri verisi kendi elinde kalsın isteyen ekipler ve destek masası kurmak isteyen küçük şirketler için. Ruby on Rails ve Vue.js ile yazılmış; PostgreSQL ve Redis ister.

## Gereksinimler

README sürüm ve donanım belirtmiyor; kurulum belgesi ([developers.chatwoot.com/self-hosted](https://developers.chatwoot.com/self-hosted/)) şunları veriyor:

- En az 2 CPU çekirdeği, 4 GB RAM, 20 GB SSD, Ubuntu 20.04+ ya da uyumlu bir Linux. Üretim için 4+ çekirdek, 8+ GB RAM, 50+ GB SSD, PostgreSQL 12+, Redis 6+ ve önde Nginx gibi bir reverse proxy.
- Docker yolu için Docker 20.10.10+ ve Docker Compose v2.14.1+ (eklenti söz dizimi `docker compose`).
- Linux betiği Ubuntu 24.04 LTS için yazılmış; HTTPS istiyorsan bir alan adı ve kuruluma giden sunucu IP'sini gösteren bir `A` kaydı (Let's Encrypt betiğin içinde).
- Hesap ya da anahtar gerekmez; e-posta bildirimleri için SMTP, kanallar için ilgili platform anahtarları sonradan eklenir.

## Kurulum

README'nin kendisi kurulum anlatmıyor: Heroku ve DigitalOcean düğmeleri ile [chatwoot.com/deploy](https://chatwoot.com/deploy) sayfasına yönlendiriyor. Aşağıdaki komutlar o yönlendirmenin vardığı [developers.chatwoot.com](https://developers.chatwoot.com/self-hosted/) belgelerinden. Kararlı sürüm için `master` dalı ya da `v*` etiketleri; `develop` geliştirme dalı.

### Kurulumsuz: bulut sürümü ya da tek tıkla dağıtım

En kısa yol Chatwoot'un barındırdığı sürüm: https://app.chatwoot.com/app/auth/signup (ücretsiz deneme; fiyatlar chatwoot.com/pricing). Kendi hesabında ama kurulum istemeden: [Heroku'ya tek tıkla dağıt](https://heroku.com/deploy?template=https://github.com/chatwoot/chatwoot/tree/master) (ortam değişkenlerini [bu listeye](https://developers.chatwoot.com/self-hosted/configuration/environment-variables) göre ayarlamazsan bazı özellikler bozulur) ya da [DigitalOcean 1-Click Kubernetes](https://marketplace.digitalocean.com/apps/chatwoot?refcode=f2238426a2a8).

### Kendi sunucuna: Linux kurulum betiği (Ubuntu 24.04, en kısa self-host yolu)

Betiği indir ve çalıştır ([linux-vm belgesi](https://developers.chatwoot.com/self-hosted/deployment/linux-vm)):

```bash
wget https://get.chatwoot.app/linux/install.sh
chmod +x install.sh
```

```bash
./install.sh --install
```

Betik alan adı kurulumunu sorar; `yes` deyip alan adını girersen Nginx ve Let's Encrypt SSL'i de ayarlar. Daha sonra güncellemek için `cwctl --upgrade`.

### Kendi sunucuna: Docker Compose

Belgedeki adımlar ([docker belgesi](https://developers.chatwoot.com/self-hosted/deployment/docker)). Ortam ve compose dosyalarını indir:

```bash
wget -O .env https://raw.githubusercontent.com/chatwoot/chatwoot/develop/.env.example
wget -O docker-compose.yaml https://raw.githubusercontent.com/chatwoot/chatwoot/develop/docker-compose.production.yaml
```

İki dosyayı düzenle (Postgres şifresi ikisinde de aynı olmalı):

```bash
nano .env
nano docker-compose.yaml
```

Veritabanını hazırla, servisleri başlat:

```bash
docker compose run --rm rails bundle exec rails db:chatwoot_prepare
```

```bash
docker compose up -d
```

Güncelleme:

```bash
docker compose pull
docker compose up -d
docker compose run --rm rails bundle exec rails db:chatwoot_prepare
```

Kubernetes için resmi Helm chart ([helm-chart belgesi](https://developers.chatwoot.com/self-hosted/deployment/helm-chart)); AWS, Azure ve GCP için ayrı sayfalar aynı belge sitesinde.

## İlk çalıştırma

`.env.example`'daki en küçük değişiklik seti (dosyadaki yorumlara göre): `SECRET_KEY_BASE` (`rake secret` ile üretilen uzun bir değer; örnekteki `replace_with_lengthy_secure_hex` kalmasın), `FRONTEND_URL` (kurulumun dışarıdan görünen adresi; örnek `http://0.0.0.0:3000`), `POSTGRES_PASSWORD` (boş gelir, compose dosyasıyla aynı olmalı), `RAILS_ENV=production`. `ENABLE_ACCOUNT_SIGNUP` örnekte `false`: giriş sayfasından yeni hesap açılmaz, ilk hesap bunun dışında kurulur (aşağıda).

- Linux betiği: tarayıcıda `http://{your_ip_address}:3000`, alan adı verdiysen `https://chatwoot.mydomain.com`.
- Docker: konteynerler yalnız `127.0.0.1:3000`'e bağlanır; sunucuda `curl -I localhost:3000/api` komutu `200` dönmeli. Dışarıdan geçici bakmak için compose'da rails servisinin `127.0.0.1:3000:3000` satırını `3000:3000` yap (sonra geri al); kalıcı erişim için önüne Nginx gibi bir reverse proxy koy.

Yeni kurulumda ilk açılışta giriş sayfası yerine bir onboarding ekranı gelir; ilk hesabı ve yönetici kullanıcıyı orada oluşturursun. Sonra bir gelen kutusu (ör. web sitesi canlı sohbeti) ekleyip verilen widget kodunu sitene yerleştir; ilk mesaj gelen kutusuna düşer. Hesap ve kanal ayarlarının anlatımı [yardım merkezinde](https://www.chatwoot.com/help-center).

## Sık takılınan yerler

- **İlk kurulumda `rails db:migrate` hata veriyor.** İlk veritabanı hazırlığı için `rails db:chatwoot_prepare` kullanılmalı ([docker belgesi](https://developers.chatwoot.com/self-hosted/deployment/docker)).
- **Postgres'e bağlanamıyor.** `.env` ve `docker-compose.yaml`'daki Postgres şifresi aynı olmalı ([docker belgesi](https://developers.chatwoot.com/self-hosted/deployment/docker)).
- **Dışarıdan 3000 portuna ulaşılamıyor.** Konteynerler bilerek `127.0.0.1`'e bağlı; yukarıdaki geçici port değişikliği ya da reverse proxy gerekir ([docker belgesi](https://developers.chatwoot.com/self-hosted/deployment/docker)).
- **Çok eski kurulumu güncellerken hata.** Doğrudan `latest`'e atlama; ara imaj etiketlerinden geçip her adımda `db:chatwoot_prepare` çalıştır. Yalnız topluluk sürümü istiyorsan `latest-ce` ya da `v*-ce` etiketleri ([docker belgesi](https://developers.chatwoot.com/self-hosted/deployment/docker)).
- **Onboarding yerine giriş ekranı geliyor.** Yeni kurulumda ilk hesabı oluşturan onboarding ekranı gelmezse SSS'deki Rails konsolu komutuyla `CHATWOOT_INSTALLATION_ONBOARDING` Redis bayrağını `true` yapıp yeniden başlat ([SSS](https://developers.chatwoot.com/self-hosted/faq)).

## Bağlantılar

- Repo: https://github.com/chatwoot/chatwoot
- Belgeler: https://developers.chatwoot.com/self-hosted/ (kurulum) — https://www.chatwoot.com/help-center (kullanım)
- Sürümler: https://github.com/chatwoot/chatwoot/releases
- Lisans: GitHub'ın SPDX tespiti yok (NOASSERTION). README "MIT License" diyor; [LICENSE](https://github.com/chatwoot/chatwoot/blob/develop/LICENSE) dosyasına göre `enterprise/` dizini dışındaki her şey MIT (Expat), `enterprise/` altı ise `enterprise/LICENSE`'daki ayrı lisansa tabi.

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki Günün Reposu g06 videosu için hazırlandı (yakında — 16 Kas 15:30). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
