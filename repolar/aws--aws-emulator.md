---
repo: aws/aws-emulator
url: https://github.com/aws/aws-emulator
lisans: CC0-1.0
surum: v1.0.500 (2026-11-18)
readme_tarihi: 2026-11-16
dogrulama: denenmedi
bolum: trend-short/t2026-11-18-1
ozet: AWS API'lerini kendi bilgisayarında, sunucunda ya da CI'da taklit eden, AWS hesabı istemeden kodunu test etmeni sağlayan yerel emülatör.
---

# AWS Emulator — kurulum ve ilk örnek

> **Bu belge, README'nin 16 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

AWS Emulator, AWS API'lerinin yerel bir emülatörüdür. Yazılımını AWS'e hiç istek atmadan kendi makinende, bir geliştirme sunucusunda ya da CI'da test edersin; README'ye göre AWS hesabı gerekmez, istekler AWS'e değil emülatöre gider ve gerçek kaynaklar yerine taklit kaynaklarla çalışırsın. Emülatörün ayrıca bir hizmet ücreti yok; yalnız gerçekten AWS'e deploy ettiğinde AWS kaynaklarının ücretini ödersin. Grafik arayüzü yoktur: yerel bir program olarak tek bir AWS API endpoint'i sunar, AWS CLI ya da AWS SDK'yi ortam değişkenleriyle bu endpoint'e yönlendirirsin. Embedded kipte ise emülatörün HTTP client'ını doğrudan SDK'ye verirsin. README açıkça uyarıyor: yalnız geliştirme ve test için; production iş yükleri ya da güvenlik testi için kullanılmamalı. Lisansı CC0-1.0.

## Gereksinimler

- AI coding agent yoluyla kurulum için AWS CLI 2.35.0 ya da üstü.
- Yerel süreç ya da Docker ile çalıştırma için README hazır paketi önerir; adımlar AWS Emulator User Guide'da (aşağıdaki bağlantılar).
- Kaynaktan derleme (emülatörü değiştirmek, checkout'tan çalıştırmak ya da embedded kip) yalnız **Linux (x86_64 ve aarch64)** üzerinde destekleniyor; Windows ve macOS'ta Docker kullanılır. Bu yol için:
  - Cargo'lu bir Rust toolchain (rustup ile),
  - GCC ya da clang gibi bir C derleyicisi,
  - `target/` için en az 100 GB boş disk (her AWS servisi ayrı bir crate),
  - çok çekirdekli bir makine (ilk tam derleme uzun sürer, sonraki derlemeler hızlı).

## Kurulum

README üç yol veriyor.

**1. AI coding agent ile.** `aws-emulator` skill'ini kur, sonra agent'ından emülatörü kurmasını iste:

```bash
aws agent-toolkit add-skill --skill-name aws-emulator --region us-east-1
```

**2. Yerel süreç (hazır paket).** README bu yolun komutlarını kendi içinde vermiyor; User Guide'daki "Getting started" sayfasına yönlendiriyor. Windows ve macOS için "Getting started with Docker" sayfası var.

**3. Kaynaktan derleme (Linux).**

```bash
cargo build -p aws-emulator-router
cargo run   -p aws-emulator-router
```

Embedded kipte emülatörü projene dev-dependency olarak eklersin; ilk `cargo test` emülatörün servis crate'lerini de derler. README'nin önerisi: projeler arasında tek bir `CARGO_TARGET_DIR` kullan, CI'da `target/`'i önbelleğe al (ya da sccache) ve emülatörü bir sürüm commit'ine sabitle.

## İlk çalıştırma

1. Emülatörü başlat (yerel süreç, Docker ya da kaynaktan `cargo run -p aws-emulator-router`). Kaynaktan çalıştırıldığında README'ye göre `127.0.0.1:9297` adresinde HTTPS sunar (self-signed sertifika).
2. AWS CLI ya da SDK'ni ortam değişkenleriyle bu endpoint'e yönlendir. Hangi değişkenlerin ayarlanacağı README'de değil, User Guide'daki "Pointing your AWS SDK at AWS Emulator" sayfasında.
3. Kodunu her zamanki gibi çalıştır; istekler AWS yerine emülatöre gider.

README adım adım bir örnek proje vermiyor; akış README'nin Quick start bölümünden çıkarıldı.

## Sık takılınan yerler

- Telemetri varsayılan olarak açık (README: istek içerikleri, kod, kimlik bilgileri ve kişisel veri toplanmaz). Kapatmak için tek süreçte:

```bash
aws-emulator --telemetry off
```

  ya da kabukta:

```bash
export EMULATOR_TELEMETRY=off
aws-emulator
```

  Container'da `docker run` komutunda imaj adından sonra `--telemetry off` eklenir; kalıcı kapatma için `~/.aws/emulator/config.toml` dosyasında `[telemetry]` altında `enabled = false`.
- Her servis AWS'i aynı yakınlıkta taklit etmiyor: her sürümle birlikte yayımlanan fidelity puanları Releases sayfasında. Çalışmayan API ve davranışlar repodaki `issues/things-that-do-not-work/` klasöründe, düzeltilenler `issues/things-that-now-work/` klasöründe izleniyor.
- Kaynaktan derleme büyük: debug derlemesi 100 GB'a yaklaşan `target/` ister ve servis eklendikçe büyür.

## Bağlantılar

- Repo: https://github.com/aws/aws-emulator
- User Guide: https://docs.aws.amazon.com/emulator/latest/userguide/what-is-service.html
- AI coding agent ile başlangıç: https://docs.aws.amazon.com/emulator/latest/userguide/getting-started-agent.html
- Yerel süreç: https://docs.aws.amazon.com/emulator/latest/userguide/getting-started.html · Docker: https://docs.aws.amazon.com/emulator/latest/userguide/getting-started-docker.html
- SDK'yi emülatöre yönlendirme: https://docs.aws.amazon.com/emulator/latest/userguide/point-sdk-to-emulator.html
- Embedded kip: https://docs.aws.amazon.com/emulator/latest/userguide/getting-started-embedded.html
- Projen çalışır mı: https://docs.aws.amazon.com/emulator/latest/userguide/check-project.html
- Güvenlik: https://docs.aws.amazon.com/emulator/latest/userguide/security.html · Telemetri: https://docs.aws.amazon.com/emulator/latest/userguide/telemetry.html
- Sürümler ve fidelity puanları: https://github.com/aws/aws-emulator/releases (son sürüm v1.0.500, 18 Kasım 2026)
- Issues: https://github.com/aws/aws-emulator/issues

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki trend Short t2026-11-18-1 videosu için hazırlandı (video henüz yayında değil). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
