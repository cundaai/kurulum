---
repo: Tracer-Cloud/opensre
url: https://github.com/Tracer-Cloud/opensre
lisans: Apache-2.0
surum: v0.1.2026.11.25 (2026-11-25)
readme_tarihi: 2026-11-21
dogrulama: denenmedi
bolum: 2026-W48-2
ozet: Başarısız CI çalıştırmasını okuyup nedenini bulan ve incelemen için düzeltmeyi gönderen açık kaynak CLI agent.
---

# opensre — kurulum ve ilk örnek

> **Bu belge, README'nin 21 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

OpenSRE, CI/CD güvenilirliği için açık kaynak bir CLI agent. README'nin anlatımıyla CI sana bir şeyin bozulduğunu söyler ama nedenini ve nasıl düzeleceğini söylemez; OpenSRE başarısız çalıştırmayı okur, nedenin izini sürer ve düzeltmeyi incelemen için PR dalına gönderir. Hatanın CI'dan mı koddan mı kaynaklandığını ayırır; hata oranı, güvenilmez CI yüzünden kaybedilen geliştirici saati ve `main` dalının ne kadar kırmızı kaldığı gibi ölçüleri gösterir. Yalnız CI/CD değil: aynı agent, bağladığın entegrasyonlarla (README'de 60'tan fazla araç: Kubernetes, AWS, Grafana, Datadog, Slack, GitHub…) canlı ortam olaylarını da inceler ve özeti Slack, PagerDuty ya da Telegram'a yazar. README projeyi "Public Alpha" olarak işaretliyor: temel akışlar denenebilir ama henüz tam kararlı değil, API'ler ve entegrasyonlar değişebilir. Lisansı Apache 2.0.

## Gereksinimler

- Bir kod projesinin içinde açılmış bir terminal (README: "A terminal open in a code project").
- Bir Git reposu; README bunu tam akışı görmek için öneriyor.
- macOS, Linux ya da Windows (PowerShell). Kurulum betiği `sudo` istemez.
- Linux'ta hazır dosyalar glibc 2.35 ve üstünü ister (Ubuntu 22.04+ ya da benzeri) ve Alpine Linux'ta çalışmaz (depodaki `docs/install/linux-local.mdx`); bu durumda belgelerdeki Docker yolu önerilir.
- Bir OpenSRE hesabı: `opensre` ilk açılışta oturum açmanı ister ve barındırılan modeli etkinleştirir; kurulum belgesine göre bu yolda ayrıca LLM anahtarı gerekmez. Oturum açma tarayıcıda yapılır.
- README başka bir sürüm gereksinimi (Python, Node vb.) yazmıyor; kaynak koddan kurulum `docs/DEVELOPMENT.md` belgesinde.

## Kurulum

macOS ya da Linux:

```bash
curl -fsSL https://install.opensre.com | bash -s -- -gh
```

Windows (PowerShell):

```powershell
& ([scriptblock]::Create((irm https://install.opensre.com/install.ps1))) -gh
```

README'ye göre kurulum betiği en güncel derlemeyi `main` dalından indirir ve `sudo` gerektirmez. `opensre` komutu bulunamazsa betiğin yazdığı PATH yönergelerini izle ya da yeni bir terminal aç.

Kendi sunucunda çalıştırmak için README üç yol sayıyor: OpenSRE Cloud'un yönetilen gateway'i, AWS EC2 üzerinde gateway (`make build-gateway-image`, sonra `make deploy-gateway`; yalnız Telegram gateway'i, Docker yok) ve self-hosted container (repodaki `Dockerfile`; `MODE` seçilir, `LLM_PROVIDER` ve onun API anahtarı ayarlanır). Adımların tamamı README'de değil, `DEPLOYMENT.md` dosyasında.

## İlk çalıştırma

1. OpenSRE'yi başlat; ilk açılış oturum açtırır ve barındırılan modeli etkinleştirir:

```bash
opensre
```

   Alt komut vermeden çalıştırınca hesabını doğrular ve etkileşimli bir kabuk (REPL) açar; gerçek bir terminal (TTY) gerekir ve kabuk yalnız etkin bir hesapla açılır.
2. Kabukta olayı düz dille anlat ve agent'ın çalışmasını izle. Oturum için slash komutları: `/help`, `/status`, `/cost`, `/sessions`, `/resume`, `/compact`, `/new`, `/exit`; entegrasyonlar için `/integrations list` ve `/integrations verify`. Ctrl+C, süren turu oturumu kaybetmeden iptal eder.
3. Etkileşimsiz tek tur (terminalden, betikten ya da CI işinden):

```bash
opensre ask "why is checkout-api slow?"
```

README'deki öbür yararlı komutlar:

```bash
opensre integrations setup
opensre update
opensre uninstall   # remove opensre and all local data
```

README ayrıca agent'ı kendi Python kodundan sürmek için bir örnek veriyor (`from surfaces.embedded import start_embedded_session`); bu yol kaynak kodun indirilmiş olmasını ister ve ayrıntısı Python API belgesinde. README adım adım bir "başarısız CI çalıştırmasını düzelt" örneği vermiyor; bunun için `docs/cicd/overview.mdx` belgesine yönlendiriyor.

## Sık takılınan yerler

- `opensre` komutu bulunamıyorsa: kurulum betiğinin yazdığı PATH yönergelerini izle ya da yeni bir terminal aç (README). Betik çoğunlukla `~/.local/bin` altına kurar; Windows'ta `%USERPROFILE%\.local\bin` (depodaki kurulum belgeleri).
- README'deki kurulum komutu `-gh` bayrağıyla, depodaki `docs/install/` belgeleri aynı komutu `-dc` bayrağıyla yazıyor; README farkı açıklamıyor. Bu belgede README'deki hâli kullanıldı.
- Oturum açma ekranı yeniden geliyorsa `opensre account status` oturumdaki sorunu söyler (`docs/install/install-local.mdx`). Sunucuda ya da SSH üzerinden oturum açarken yazdırılan bağlantı başka bir cihazın tarayıcısında açılıp gösterilen kod yapıştırılır (`docs/install/linux-local.mdx`).
- Linux'ta kurulum betiği GitHub CLI (`gh`) yoksa onu da indirir; bu adımı atlamak için `OPENSRE_SKIP_GH_INSTALL=1` (`docs/install/linux-local.mdx`).
- Ürün analitiği ve Sentry hata bildirimi varsayılan olarak açık (opt-out). Kapatmak için README'deki komut: `export OPENSRE_NO_TELEMETRY=1`.
- `opensre uninstall` yalnız programı değil, bütün yerel veriyi de siler (README'deki açıklama satırı).

## Bağlantılar

- Repo: https://github.com/Tracer-Cloud/opensre
- Site: https://opensre.com
- Belgeler: https://www.opensre.com/docs · Quickstart: https://www.opensre.com/docs/getting-started/quickstart · yerel kurulum: https://www.opensre.com/docs/install/install-local · SSS: https://opensre.com/docs/guides/faq
- Depodaki belgeler: https://github.com/Tracer-Cloud/opensre/blob/main/DEPLOYMENT.md (sunucuya kurulum), https://github.com/Tracer-Cloud/opensre/blob/main/SETUP.md (yerel ortam, Windows, MCP), https://github.com/Tracer-Cloud/opensre/blob/main/docs/DEVELOPMENT.md (kaynaktan kurulum, telemetri)
- Sürümler: https://github.com/Tracer-Cloud/opensre/releases (son sürüm v0.1.2026.11.25, 25 Kasım 2026)
- Issues: https://github.com/Tracer-Cloud/opensre/issues · Discord: https://discord.gg/7NTpevXf7w
- Video: Haftanın Popüler Repoları, 23–25 Kasım 2026 bölümü (listenin 5. sırası)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W48-2 bölümü](../bolumler/2026-W48-2.md) videosu için hazırlandı (video henüz yayında değil). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
