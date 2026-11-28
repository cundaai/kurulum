---
repo: anomalyco/opencode
url: https://github.com/anomalyco/opencode
lisans: MIT
surum: v1.19.6 (2026-11-17)
readme_tarihi: 2026-11-10
dogrulama: denenmedi
bolum: 2026-W48-3
ozet: Terminalde, masaüstünde çalışan, plan ve build kipleri olan açık kaynak AI coding agent.
---

# opencode — kurulum ve ilk örnek

> **Bu belge, README'nin 10 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

OpenCode açık kaynak bir AI coding agent: terminalde çalışan bir arayüzle (TUI) kod okur, dosya düzenler, komut çalıştırır. Masaüstü uygulaması da var (README'de BETA olarak geçiyor). İçinde iki hazır agent bulunur ve aralarında `Tab` tuşuyla geçilir:

- **build**: varsayılan, tam yetkili geliştirme agent'ı.
- **plan**: yalnız okuyan analiz agent'ı; dosya düzenlemeyi varsayılan olarak reddeder, bash komutu çalıştırmadan önce izin ister. Tanımadığın bir kod tabanını incelemek ya da değişiklik planlamak için uygun.

Karmaşık aramalar ve çok adımlı işler için bir de **general** alt agent'ı var; mesajda `@general` yazarak çağrılır. README Türkçe dahil 20'den fazla dilde. Lisansı MIT.

## Gereksinimler

- macOS, Linux ya da Windows (masaüstü uygulaması üçünde de var; kurulum betiği ve paket yöneticileri README'de macOS ve Linux için gösteriliyor).
- Kurulum yoluna göre: `curl` ve `bash`, ya da npm/bun/pnpm/yarn, Homebrew, pacman, paru, mise veya Nix.
- README model sağlayıcısını (hangi LLM'in, hangi anahtarla kullanılacağını) anlatmıyor; yapılandırma için belgelere yönlendiriyor: https://opencode.ai/docs
- README'nin uyarısı: kurmadan önce 0.1.x'ten eski sürümleri kaldır.

## Kurulum

README'deki komutlar (tek betik ya da paket yöneticileri; birini seç):

```bash
# YOLO
curl -fsSL https://opencode.ai/v2/install | bash

# Package managers
npm i -g @opencode/cli@latest      # or bun/pnpm/yarn
brew install anomalyco/tap/opencode-v2 # macOS and Linux (recommended, always up to date)
brew install opencode              # macOS and Linux (official brew formula, updated less)
sudo pacman -S opencode            # Arch Linux (Stable)
paru -S opencode-beta              # Arch Linux (Latest from AUR)
mise use -g opencode               # Any OS
nix run github:anomalyco/opencode  # Nix
```

Kurulum betiği OpenCode'u `$HOME/.opencode/bin` klasörüne kurar.

Masaüstü uygulaması (BETA): https://opencode.ai/download adresinden indir. README'deki dosyalar: macOS Apple Silicon için `opencode-desktop-mac-arm64.dmg`, Intel Mac için `opencode-desktop-mac-x64.dmg`, Windows için `opencode-desktop-win-x64.exe`, Linux için `.deb`, `.rpm` ya da `.AppImage`. macOS'ta Homebrew ile:

```bash
# macOS (Homebrew)
brew install --cask opencode-desktop
```

## İlk çalıştırma

README ilk çalıştırmayı adım adım anlatmıyor; komut adı, sağlayıcı bağlama ve ilk görev için https://opencode.ai/docs adresine yönlendiriyor. README'den çıkan kullanım bilgisi şu kadar:

1. Uygulamayı (terminal arayüzü ya da masaüstü uygulaması) aç.
2. Varsayılan agent **build**'dir; önce yalnız incelemek istiyorsan `Tab` ile **plan** agent'ına geç. Plan kipinde dosya düzenlenmez, bash komutları için izin sorulur.
3. Görevini yaz; karmaşık bir arama ya da çok adımlı iş için mesajda `@general` kullan.

Agent'ların ayrıntısı: https://opencode.ai/docs/agents

## Sık takılınan yerler

- 0.1.x'ten eski bir OpenCode kuruluysa önce onu kaldır (README'deki ipucu).
- İki Homebrew yolu var: `anomalyco/tap/opencode-v2` README'ye göre önerilen ve her zaman güncel olan; resmî `opencode` formülü daha seyrek güncelleniyor.
- Kurulum betiği dosyaları `$HOME/.opencode/bin` altına koyar; komut bulunamıyorsa bu klasörün PATH'te olup olmadığına bak.

## Bağlantılar

- Repo: https://github.com/anomalyco/opencode
- Site ve belgeler: https://opencode.ai · https://opencode.ai/docs · agent'lar: https://opencode.ai/docs/agents
- Masaüstü indirme: https://opencode.ai/download
- Türkçe README: https://github.com/anomalyco/opencode/blob/dev/README.tr.md
- Sürümler: https://github.com/anomalyco/opencode/releases (son sürüm v1.19.6, 17 Kasım 2026)
- Issues: https://github.com/anomalyco/opencode/issues · Discord: https://discord.gg/opencode
- Video: Haftanın Popüler Repoları, 25–27 Kasım 2026 Cuma bölümü (listenin 1. sırası)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W48-3 bölümü](../bolumler/2026-W48-3.md) videosu için hazırlandı: https://www.youtube.com/watch?v=AYiNLSGngiY. Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
