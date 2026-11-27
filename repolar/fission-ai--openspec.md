---
repo: Fission-AI/OpenSpec
url: https://github.com/Fission-AI/OpenSpec
lisans: MIT
surum: v2.2.0 (2026-11-24)
readme_tarihi: 2026-11-17
dogrulama: denenmedi
bolum: 2026-W48-3
ozet: AI coding asistanıyla kod yazmadan önce ne yapılacağını sade Markdown spec'lerde anlaştıran hafif Spec-Driven Development aracı.
---

# OpenSpec — kurulum ve ilk örnek

> **Bu belge, README'nin 17 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

OpenSpec, AI coding asistanına hafif bir spec katmanı ekler: gereksinimler yalnız sohbet geçmişinde kalmaz, kod yazılmadan önce sen ve AI ne yapılacağında anlaşırsınız. Her değişiklik kendi klasörünü alır (`openspec/changes/<ad>/`): `proposal.md` (neden ve ne değişiyor), `specs/` (gereksinimler ve senaryolar), `design.md` (teknik yaklaşım), `tasks.md` (uygulama listesi). Spec'ler özel sözdizimi olmayan düz Markdown. Akış asistanın içinde skill olarak çalışır: `/openspec-explore` ile düşün, `/openspec-propose` ile planla, `/openspec-apply-change` ile uygula, `/openspec-archive-change` ile arşivle. README'ye göre 30'dan fazla AI aracıyla çalışır; ekipler için planlamayı ayrı bir repoda paylaşan "Stores" özelliği beta. README kendini GitHub'ın Spec Kit'ine göre daha hafif, katı faz kapıları olmayan bir araç olarak konumluyor. Lisansı MIT.

## Gereksinimler

- macOS, Linux ya da Windows. Kurulum betiği için ek bir şey gerekmez; npm ile kuracaksan Node.js 22.20.0 ya da üstü (README).
- Desteklenen bir AI coding asistanı (Claude Code, Codex, Cursor vb.; tam liste Supported Tools sayfasında).
- README'nin "Usage Notes" bölümü yüksek akıl yürütmeli modelleri öneriyor (Codex 5.5 ve Opus 4.7). OpenSpec'in kendisi ücretli hesap istemez.

## Kurulum

macOS ya da Linux'ta terminalde:

```bash
curl -fsSL https://openspec.dev/cli/install.sh | sh
```

Windows'ta PowerShell'de:

```powershell
irm https://openspec.dev/cli/install.ps1 | iex
```

Paket yöneticisiyle: `npm install -g @fission-ai/openspec@latest` (Node.js 22.20.0+) ya da Homebrew ile `brew install openspec`. README'ye göre pnpm, yarn, bun ve Nix ile de kurulabiliyor; ayrıntılar sitedeki installation sayfasında.

Sonra proje klasöründe başlat:

```bash
cd your-project
openspec init
```

`openspec init` seçtiğin araçlar için skill'leri kurar ve onları nasıl çağıracağını yazar.

## İlk çalıştırma

Asistanınla konuş:

1. Ne yapacağından emin değilsen `/openspec-explore` ile başla: kodunu okur, seçenekleri tartar, kod yazılmadan önce bir plan çıkarır.
2. Ne istediğini biliyorsan doğrudan `/openspec-propose <what-you-want-to-build>`.
3. Plan onaylanınca `/openspec-apply-change` görevleri uygular, `/openspec-archive-change` değişikliği arşivleyip spec'leri günceller.

README'deki örnek akış karanlık mod eklemek: `/openspec-propose add-dark-mode` → `openspec/changes/add-dark-mode/` altında `proposal.md`, `specs/`, `design.md`, `tasks.md` oluşur.

Çağırma biçimi araca göre değişir: çoğu araçta `/openspec-propose`, Codex'te `$openspec-propose`; ya da işi düz cümleyle istersin. Genişletilmiş akış (`openspec-new-change`, `openspec-verify-change` vb.) için `openspec config profile` ile seç, `openspec update` ile uygula. İlk değişikliğin baştan sona anlatımı sitedeki Quickstart'ta.

## Sık takılınan yerler

- 1.x'ten ve onun `/opsx:*` komutlarından geliyorsan README "Upgrade to 2.0" sayfasına yönlendiriyor.
- CLI'yi güncelledikten sonra makinendeki skill'leri tazelemek için `openspec update` çalıştır; ekip skill'leri Git'te tutuyorsa her projede ayrı ayrı (README).
- README temiz bir context penceresi öneriyor: uygulamaya başlamadan önce context'i temizle.
- Telemetri: anonim kullanım istatistiği (yalnız komut adı ve sürüm; argüman, yol, içerik yok), varsayılan açık, CI'da kendiliğinden kapalı. Kapatmak için `openspec config set telemetry.enabled false` ya da `OPENSPEC_TELEMETRY=0` / `DO_NOT_TRACK=1` (README).

## Bağlantılar

- Repo: https://github.com/Fission-AI/OpenSpec
- Site ve belgeler: https://openspec.dev · https://openspec.dev/docs
- Quickstart: https://openspec.dev/docs/quickstart · Kurulum seçenekleri: https://openspec.dev/docs/installation
- Desteklenen araçlar: https://openspec.dev/docs/supported-tools · CLI: https://openspec.dev/docs/cli
- Stores (beta): https://openspec.dev/docs/stores · 2.0'a geçiş: https://openspec.dev/docs/upgrade-to-2
- Sürümler: https://github.com/Fission-AI/OpenSpec/releases (son sürüm v2.2.0, 24 Kasım 2026)
- Issues: https://github.com/Fission-AI/OpenSpec/issues · Discord: https://discord.gg/YctCnvvshC
- Video: Haftanın Popüler Repoları, 25–27 Kasım 2026 Cuma bölümü (listenin 9. sırası)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W48-3 bölümü](../bolumler/2026-W48-3.md) videosu için hazırlandı (video henüz yayında değil). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
