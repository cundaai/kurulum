---
repo: getarcaneapp/arcane
url: https://github.com/getarcaneapp/arcane
lisans: BSD-3-Clause
surum: v3.2.0 (2026-11-27)
readme_tarihi: 2026-11-14
dogrulama: denenmedi
bolum: 2026-W48-3
ozet: Docker container'larını, image'larını ve Compose projelerini tarayıcıdan yöneten, kendi sunucuna kurduğun açık kaynak web arayüzü.
---

# arcane — kurulum ve ilk örnek

> **Bu belge, README'nin 14 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

Arcane, kendi tanımıyla "Modern Docker Management, Designed for Everyone": Docker'ı komut satırı yerine bir web panelinden yönetmeni sağlar. Container'ları, image'ları ve Compose projelerini tek ekranda görür, başlatır, durdurur, düzenlersin. Backend Go ile yazılmış; panel tek bir container olarak çalışır ve Docker socket'ine bağlanır. `main` dalı Arcane v3; son 2.x sürümü `release/2.x.x` dalında. Arayüz Crowdin üzerinden çok dile çevriliyor. Lisansı BSD-3-Clause.

## Gereksinimler

- Docker ve Docker Compose kurulu bir makine ya da sunucu. Repodaki örnek dosyalar Podman için de ayrı Compose dosyaları içeriyor (`compose.podman-rootful.yaml`, `compose.podman-rootless.yaml`).
- Panel Docker socket'ine (`/var/run/docker.sock`) erişir; bu, makinedeki Docker üzerinde tam yetki demektir.
- 32 karakterlik rastgele bir şifreleme anahtarı (`ARCANE_ENCRYPTION_KEY`).
- Ücretli hesap gerekmez.

## Kurulum

**README kurulum adımı vermiyor.** README'nin "Documentation" bölümü kurulum, ayar ve geliştirme için resmî belge sitesine yönlendiriyor: https://getarcane.app. Asıl kurulum yolu için önce oraya bak.

Repoda hazır bir örnek Compose dosyası var: `docker/examples/compose.basic.yaml`. 27 Kasım 2026'daki hâli aşağıda, olduğu gibi (yalnız şifreleme anahtarı yer tutucuyla değiştirildi; `/host/path/to/...` yollarını kendi klasörlerinle değiştir):

```yaml
services:
  arcane:
    image: getarcaneapp/manager:latest
    container_name: arcane
    ports:
      - '3552:3552'
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - arcane-data:/app/data
      - /host/path/to/projects:/host/path/to/projects
      - /host/path/to/builds:/builds
    environment:
      - ARCANE_ENCRYPTION_KEY=<RASTGELE_32_KARAKTER>
      - ARCANE_PROJECTS_DIRECTORY=/host/path/to/projects
      - TZ=UTC
    cgroup: host
    healthcheck:
      test: ['CMD', './arcane', 'health', '--timeout', '2s']
      interval: 10s
      timeout: 3s
      retries: 5
      start_period: 15s
    restart: unless-stopped
    stop_grace_period: 15s

volumes:
  arcane-data:
```

Örnek dosyadaki yorum satırları (ek proje klasörleri, `PUID`/`PGID`, saat dilimi) burada kısaltıldı; tamamı repoda. Repodaki `.env.example` güvenli değer üretmek için `openssl rand -base64 32` öneriyor. Aynı klasörde reverse proxy (`compose.proxy.yaml`), uzak makine için agent (`compose.agent.yaml`) ve OpenTelemetry (`compose.otel.yaml`) örnekleri de var.

## İlk çalıştırma

README ilk çalıştırmayı anlatmıyor. Örnek Compose dosyasına göre panel 3552 portunu açar; container ayağa kalkınca tarayıcıda o makinenin 3552 portunu aç. İlk giriş, yönetici hesabı ve uzak Docker makinelerinin (agent) eklenmesi için belge sitesine bak: https://getarcane.app. Buradaki adımlar oradan doğrulanmadı.

## Sık takılınan yerler

- `main` dalı v3; 2.x kullanıyorsan `release/2.x.x` dalına bak (README).
- Özellik istekleri artık issues'ta değil, Discussions'taki "feature-requests" kategorisinde; issues yalnız hata bildirimi için (README).

## Bağlantılar

- Repo: https://github.com/getarcaneapp/arcane
- Belgeler: https://getarcane.app
- Örnek Compose dosyaları: https://github.com/getarcaneapp/arcane/tree/main/docker/examples
- SBOM: https://getarcane.app/sbom
- Sürümler: https://github.com/getarcaneapp/arcane/releases (son sürüm v3.2.0, 27 Kasım 2026)
- Issues: https://github.com/getarcaneapp/arcane/issues · Özellik istekleri: https://github.com/getarcaneapp/arcane/discussions/categories/feature-requests
- Çeviri: https://crowdin.com/project/arcane-docker-management
- Video: Haftanın Popüler Repoları, 25–27 Kasım 2026 Cuma bölümü (listenin 8. sırası)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W48-3 bölümü](../bolumler/2026-W48-3.md) videosu için hazırlandı (yakında — 27 Kas 12:30). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
