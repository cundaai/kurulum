---
repo: sherlock-project/sherlock
url: https://github.com/sherlock-project/sherlock
lisans: MIT
surum: v0.16.2 (2026-09-08)
readme_tarihi: 2026-11-01
dogrulama: denenmedi
bolum: 2026-W46
ozet: Bir kullanıcı adının 400'den fazla sosyal ağda hesabı olup olmadığını komut satırından tarayan açık kaynak OSINT aracı.
---

# sherlock — kurulum ve ilk örnek

> **Bu belge, README'nin 1 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

Sherlock, verdiğin bir kullanıcı adını 400'den fazla sosyal ağda arar ve hangi sitelerde o adla bir hesap bulunduğunu listeler. Python ile yazılmış bir CLI; bulunan hesaplar kullanıcı adıyla aynı adlı bir metin dosyasına yazılır, istersen CSV ya da xlsx dosyası da üretir. Lisansı MIT. Bu bir OSINT (açık kaynak istihbaratı) aracıdır: yalnız kendi hesaplarını taramak ya da izinli araştırmalar için kullan; başkasının izni olmadan iz sürmek yasal ve etik sorun doğurabilir.

## Gereksinimler

- `pipx`, `pip` ya da `uv` ile kurulum için Python ortamı (README `pipx` yerine `pip` ya da `uv` kullanılabileceğini söylüyor).
- Ya da Docker (`docker run` yolu).
- Fedora gibi `dnf` kullanan dağıtımlarda paket yöneticisi yolu da var.
- İnternet bağlantısı: araç her siteye istek atar.

## Kurulum

README'deki tablodan üç yol:

```bash
pipx install sherlock-project
```

`pipx` yerine `pip` ya da `uv` kullanılabilir. Docker ile kurmadan çalıştırmak için:

```bash
docker run -it --rm sherlock/sherlock
```

`dnf` kullanan sistemlerde:

```bash
dnf install sherlock-project
```

Debian (13 ve üstü), Ubuntu (22.10 ve üstü), Homebrew, Kali ve BlackArch için topluluğun bakımını yaptığı paketler de var; README bunların Sherlock ekibince desteklenmediğini belirtiyor. Öbür kurulum yolları: https://sherlockproject.xyz/installation

## İlk çalıştırma

Tek bir kullanıcı adını aramak için:

```bash
sherlock user123
```

Birden fazla kullanıcı adı için:

```bash
sherlock user1 user2 user3
```

Bulunan hesaplar kullanıcı adıyla aynı adlı bir dosyaya yazılır (örneğin `user123.txt`). Bütün seçenekleri görmek için `sherlock --help`; README'deki yardım çıktısına göre örneğin `--site` aramayı belirli sitelerle sınırlar, `--csv` ve `--xlsx` tablo dosyası üretir, `--timeout` her isteğin bekleme süresini (varsayılan 60 saniye) ayarlar, `--print-found` yalnız bulunan siteleri yazar.

## Sık takılınan yerler

- README uyarısı: ParrotOS ve Ubuntu 24.04 için üçüncü tarafın bakımını yaptığı paketler bozuk görünüyor; bu sistemlerde `uv`/`pipx`/`pip` ya da Docker kullan.
- `--ignore-exclusions` seçeneği yukarı akıştaki dışlamaları kapatır; yardım metnine göre daha çok yanlış pozitif dönebilir.

## Bağlantılar

- Repo: https://github.com/sherlock-project/sherlock
- Site: https://sherlockproject.xyz · Kurulum yolları: https://sherlockproject.xyz/installation
- PyPI: https://pypi.org/project/sherlock-project/ · Homebrew: https://formulae.brew.sh/formula/sherlock
- Sürümler: https://github.com/sherlock-project/sherlock/releases (son sürüm v0.16.2, 8 Eylül 2026)
- Issues: https://github.com/sherlock-project/sherlock/issues
- Video: Haftanın Popüler Repoları, 9–15 Kasım 2026 bölümü (listenin 1. sırası)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W46 bölümü](../bolumler/2026-W46.md) videosu için hazırlandı (video henüz yayında değil). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
