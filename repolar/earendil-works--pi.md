---
repo: earendil-works/pi
url: https://github.com/earendil-works/pi
lisans: MIT
surum: v1.6.1 (2026-11-25)
readme_tarihi: 2026-11-05
dogrulama: denenmedi
bolum: 2026-W48-3
ozet: Eklentiler, skill'ler ve temalarla kendine göre şekillendirdiğin, terminalde çalışan sade ve genişletilebilir coding agent.
---

# pi — kurulum ve ilk örnek

> **Bu belge, README'nin 5 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

Pi, kendi iş akışına göre şekillendirdiğin sade bir agent harness'ı: terminalde çalışan bir coding agent ve onu oluşturan kütüphaneler. README'nin fikri "Pi'yi iş akışına uydur, tersini değil": sub-agent ve plan kipi gibi özellikler hazır gelmez; bunları Pi'ye yaptırırsın ya da bir paket kurarsın. Extension, skill, prompt template ve tema ile özelleştirilir; bunlar Pi paketi olarak npm ya da git üzerinden paylaşılır.

Etkileşimli kullanılabilir, print ya da JSON kipinde otomasyona bağlanabilir, RPC ile yönetilebilir ya da Pi TypeScript SDK ile uygulama yazılabilir. Monorepo'da çoklu sağlayıcılı LLM API'si (`pi-ai`: OpenAI, Anthropic, Google vb.), agent çalışma zamanı, terminal arayüzü kütüphanesi ve dayanıklı konuşma/görev çalışma zamanı da var. Lisansı MIT.

## Gereksinimler

- Node.js 22.19 ya da daha yeni (README: macOS, Linux ve Windows kurulum betikleri gerekirse Node.js'i kendileri kurar).
- macOS, Linux ya da Windows; Nix yolu Linux ve macOS'ta ARM64 ve x86-64 destekler.
- Bir AI sağlayıcısı: Pi içinde `/login` ile bir abonelik ya da API anahtarı bağlanır.
- README'ye göre Pi'nin kendi izin sistemi yok: dosya sistemi, süreç, ağ ve kimlik bilgisi erişimini kısıtlamaz, onu başlatan kullanıcının yetkileriyle çalışır. Daha sıkı sınır istiyorsan container ya da sandbox içinde çalıştır (aşağıda).

## Kurulum

Komut satırı arayüzü (README'nin önerdiği yol; bağımlılıkları sabitler, güncelleme `pi update` ile):

```bash
curl -fsSL https://pi.dev/install.sh | sh
```

Windows'ta:

```shell
powershell -c "irm https://pi.dev/install.ps1 | iex"
```

Ya da doğrudan npm ile (README: bu yol dolaylı bağımlılıkları sabitlemez):

```bash
npm install -g --ignore-scripts @earendil-works/pi-coding-agent
```

Nix ile çalıştırma (`stable` son sürümü gösterir; Nix Pi'yi kaynaktan derler):

```bash
nix run github:earendil-works/pi/stable
```

Kalıcı kurmak için `nix profile add github:earendil-works/pi/stable`, güncellemek için `nix profile upgrade pi`. Belirli bir sürüme sabitlemek için `github:earendil-works/pi/v1.0.0` gibi bir sürüm etiketi kullanılır.

## İlk çalıştırma

1. Pi'nin çalışacağı klasöre geç ve başlat:

```bash
cd /path/to/project
pi
```

2. Hazır bir AI sağlayıcısı için Pi içinde `/login` yazıp bir abonelik ya da API anahtarı bağla.
3. Pi'ye görevini yaz.

Tam kurulum ve kullanım için belgeler: https://pi.dev/docs/latest

## Sık takılınan yerler

- İzin sistemi olmadığı için Pi senin yetkilerinle çalışır. README sınır için üç yol veriyor: Gondolin extension (`pi` ve sağlayıcı oturumu bilgisayarda kalır, araçlar ve `!` komutları yerel bir Linux micro-VM'de çalışır), düz Docker (bütün `pi` sürecini container'da çalıştırmak) ve OpenShell (politika denetimli sandbox). Ayrıntı: packages/coding-agent/docs/containerization.md
- npm ile kurarken README'deki `--ignore-scripts` bayrağını koru; README normal kurulumun bağımlılık lifecycle betiklerine ihtiyaç duymadığını söylüyor.
- Yeni katkıcıların açtığı issue ve PR'lar varsayılan olarak otomatik kapatılıyor; bakımcılar bunları her gün gözden geçiriyor (README'nin başındaki not, CONTRIBUTING.md).

## Bağlantılar

- Repo: https://github.com/earendil-works/pi
- Site ve belgeler: https://pi.dev · https://pi.dev/docs/latest
- Paketleme ve özelleştirme: packages/coding-agent/docs/ (extensions.md, skills.md, prompt-templates.md, themes.md, packages.md, cli.md, rpc.md, sdk.md, containerization.md)
- npm paketi: https://www.npmjs.com/package/@earendil-works/pi-coding-agent
- Sürümler: https://github.com/earendil-works/pi/releases (son sürüm v1.6.1, 25 Kasım 2026)
- Issues: https://github.com/earendil-works/pi/issues · Discord: https://discord.com/invite/3cU7Bz4UPx
- Video: Haftanın Popüler Repoları, 25–27 Kasım 2026 Cuma bölümü (listenin 2. sırası)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W48-3 bölümü](../bolumler/2026-W48-3.md) videosu için hazırlandı (video henüz yayında değil). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
