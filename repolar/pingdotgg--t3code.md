---
repo: pingdotgg/t3code
url: https://github.com/pingdotgg/t3code
lisans: MIT
surum: v0.1.9 (2026-11-11)
readme_tarihi: 2026-11-12
dogrulama: denenmedi
bolum: 2026-W45
ozet: Bilgisayarındaki coding agent'ları (Claude Code, Codex, Cursor…) masaüstü, web ve telefondan tek arayüzle yöneten açık kaynak kontrol düzlemi.
---

# t3code — kurulum ve ilk örnek

> **Bu belge, README'nin 12 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

T3 Code, bilgisayarında kurulu coding agent'ları tek bir hızlı arayüzden çalıştırır: masanda bir thread başlatır, sonraki adımı telefondan onaylarsın. README'ye göre Claude Code, Codex, Cursor, OpenCode, Grok Build, Antigravity, Kiro, Pi, Muse Code, fx ve ACP destekleyen her agent ile çalışır; bilgisayarında kurulu olan her agent'ı yönetebilir. Token satmaz, zaten oturum açtığın aboneliklerini kullanır. Tek sunucu, aynı thread'ler masaüstü, web ve telefon uygulamasında; her thread kendi git worktree'sinde çalışabilir, diff'i inceleyip tek düğmeyle commit, push ve pull request açılır. Lisansı MIT.

## Gereksinimler

- macOS, Windows ya da Linux: masaüstü uygulaması ya da `t3` komutu. Telefon için iOS ve Android uygulaması var.
- Kurulu ve oturum açılmış en az bir agent (README: "T3 Code needs one agent that is installed and signed in"): örneğin Claude Code kurup `claude auth login`, ya da Codex CLI kurup `codex login`.
- Kurmadan tek seferlik deneme için Node.js (`npx t3@latest`).
- Telefondan ya da başka makineden bağlanmak için aynı yerel ağ, Tailscale ya da T3 Connect (README'nin remote access belgesi).
- Intel Mac için `t3` komutunun hazır dosyası yok (install belgesi); masaüstü uygulaması var, komut satırı sunucusu kaynaktan derlenir.

## Kurulum

Masaüstü uygulaması: https://t3.codes/download adresinden indir ya da paket yöneticisiyle kur. README'deki tablo:

```bash
brew install --cask t3-code
```

Windows: `winget install T3Tools.T3Code` ya da `scoop install extras/t3code`. Arch Linux: `yay -S t3code-bin`. Nix: `nix profile add github:NixOS/nixpkgs/nixos-unstable#t3code`. Debian ve Ubuntu: Releases sayfasındaki `.deb` dosyası.

Komut satırı (`t3` komutunu `~/.local/bin` altına kurar; `t3` sunucuyu başlatıp web uygulamasını açar):

```bash
curl -fsSL https://t3.codes/install.sh | sh
```

Windows'ta PowerShell:

```powershell
irm https://t3.codes/install.ps1 | iex
```

Kurmadan tek seferlik deneme: `npx t3@latest` (önce `t3` komutunu kurmayı teklif eder).

Telefon: App Store ya da Google Play'den T3 Code uygulamasını kur, sonra README'nin remote access belgesine göre bilgisayarına bağla.

## İlk çalıştırma

1. Bir agent'ta oturum aç: Claude Code için `claude auth login`, Codex CLI için `codex login` (README'nin örnekleri; öbür agent'ların adımları install belgesinin "Providers" bölümünde).
2. Sunucuyu başlat ve web uygulamasını aç:

```bash
t3
```

   Tarayıcı açmadan yalnız sunucu için `t3 serve`; belirli bir proje klasöründe başlamak için `t3 ./my-project` (install belgesi). Masaüstü uygulamasında uygulamayı açman yeter.
3. Arayüzde proje klasörünü seçip yeni bir thread başlat, agent'ı seç ve görevi yaz. Telefondan onaylamak için uygulamayı `t3 up` ve remote access belgesindeki adımlarla bağla.

README adım adım bir "ilk görev" örneği vermiyor; akış install belgesindeki komut tablosundan (`t3`, `t3 serve`, `t3 up`, `t3 update`, `t3 uninstall`) çıkarıldı.

## Sık takılınan yerler

- `~/.local/bin` PATH'te değilse kurulum betiği bunu söyler ve zsh, bash ve fish için eklenecek komutu yazar; Windows'ta yeni PowerShell pencereleri için klasörü PATH'e eklemek gerekir (install belgesi).
- `t3` "already running server" derse o sunucuya bağlan; yenisini başlatmadan önce eskisini durdur ya da ayrı bir `--base-dir` ver (install belgesi).
- `npx t3` her zaman npm'in seçtiği sürümü çalıştırır, kurulu `t3` olsa bile; nightly kullanıyorsan kurulu `t3` komutunu çalıştır (install belgesi).
- Intel Mac'te komut satırı sunucusu yalnız kaynaktan derlenir (`git clone`, `vp i`, `vp run build:desktop`, `node apps/server/dist/bin.mjs`); bu yolda `t3 update` çalışmaz, `git pull` ve yeniden derleme gerekir (install belgesi).
- README "limited review capacity" diyor: özellik istekleri önce Ideas tartışmasında onay ister; hata bildirmeden önce CONTRIBUTING.md okunmalı.

## Bağlantılar

- Repo: https://github.com/pingdotgg/t3code
- Site ve indirme: https://t3.codes · https://t3.codes/download
- Belgeler: https://github.com/pingdotgg/t3code/blob/main/docs/README.md (kurulum: docs/user/install.md, uzaktan erişim: docs/user/remote-access.md, izin kipleri, kaynak kontrol, MCP ile dış agent'lar, SSS)
- Sürümler: https://github.com/pingdotgg/t3code/releases (son sürüm v0.1.9, 11 Kasım 2026)
- Issues: https://github.com/pingdotgg/t3code/issues · Ideas: https://github.com/pingdotgg/t3code/discussions/categories/ideas · Discord: https://discord.gg/jn4EGJjrvv
- Video: Haftanın Popüler Repoları, 2–8 Kasım 2026 bölümü (listenin 1. sırası)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W45 bölümü](../bolumler/2026-W45.md) videosu için hazırlandı: https://www.youtube.com/watch?v=AkiRwaGZDZA. Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
