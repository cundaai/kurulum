---
repo: vercel-labs/agent-desktop
url: https://github.com/vercel-labs/agent-desktop
lisans: Apache-2.0
surum: v1.3.1 (2026-11-13)
readme_tarihi: 2026-11-11
dogrulama: denenmedi
bolum: 2026-W46
ozet: AI agent'ların masaüstü uygulamalarını ekran görüntüsü yerine işletim sisteminin erişilebilirlik ağacından okuyup kullanmasını sağlayan computer use CLI'ı.
---

# agent-desktop — kurulum ve ilk örnek

> **Bu belge, README'nin 11 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

agent-desktop, bir AI agent'a macOS, Windows ve Linux'ta masaüstü uygulamalarını görme ve kullanma yeteneği (computer use) verir; bunu ekran görüntüsü ya da piksel eşleştirme olmadan yapar. Uygulamanın erişilebilirlik ağacını okur ve JSON olarak döndürür; agent'ın üzerinde işlem yapabileceği her öğenin bir "ref"i vardır. Agent bir ref'e tıklar ya da yazı yazar, sonucu görmek için arayüzü yeniden okur ve böyle devam eder. Döngü senin agent'ında çalışır; agent-desktop'ın içinde model yoktur. CLI ve C kütüphanesi olarak gelir. Lisansı Apache-2.0.

## Gereksinimler

- macOS, Windows ya da Linux; x64 ya da ARM64 işlemci (pakette iki mimarinin hazır dosyaları var).
- npm ile kurmak için Node.js (README: Bun, Yarn ve pnpm de aynı paketi kurar). macOS ve Linux'ta Homebrew de olur.
- macOS'ta Erişilebilirlik (Accessibility) izni: `agent-desktop permissions --guide` adım adım gösterir.
- Onu kullanacak bir agent: agent-desktop'ın kendi modeli yok, komutları agent'ın çalıştırır.

## Kurulum

npm ile kur, sonra durumu ve izinleri kontrol et:

```bash
npm install -g agent-desktop
agent-desktop status                # platform, permissions, supported surfaces
agent-desktop permissions --guide   # macOS: walks you through the Accessibility grant
```

macOS ve Linux'ta Homebrew ile:

```bash
brew install agent-desktop
```

JavaScript başlatıcısı olmadan doğrudan çalıştırmak için GitHub Releases'ten bilgisayarına uygun arşivi indir ve içindeki yardımcı dosyayı çalıştırılabilir dosyanın yanında tut. README'ye göre resmî Scoop paketi henüz yok; arşivlerden ve kaynaktan kurulum, sitedeki Installation sayfasında anlatılıyor.

## İlk çalıştırma

README'nin Quick start örneği Finder'la çalışır:

```bash
agent-desktop snapshot --app Finder -i      # interactive elements, each with a ref
agent-desktop click @s8f3k2p9:e8            # act on a ref from that snapshot
agent-desktop snapshot --app Finder -i      # observe again to verify
```

1. `snapshot` Finder penceresindeki etkileşimli öğeleri, her biri bir ref ile, JSON olarak yazar.
2. `click` o snapshot'tan bir ref'e tıklar. Örnekteki `@s8f3k2p9:e8` README'nin örneği; kendi snapshot'ının döndürdüğü ref'i kullan.
3. Sonucu doğrulamak için yeniden `snapshot` al.

Her komut tek bir JSON zarfı döndürür (`"ok": true`, `"command"`, `"data"` alanlarıyla).

## Sık takılınan yerler

- Ref'ler alındıkları snapshot'a aittir: çıplak `@e8` `INVALID_ARGS` ile reddedilir. Arayüz değişince işlem `STALE_REF` ya da `AMBIGUOUS_TARGET` döndürür; yeni snapshot al ve onun ref'lerini kullan.
- Birden çok pencere `AMBIGUOUS_TARGET` verir; `snapshot` aday pencere kimliklerini `error.details` içinde listeler, birini `--window-id` ile seç.
- Kalabalık uygulamalarda önce `snapshot --skeleton --app Slack -i --compact` ile haritayı çıkar, sonra `snapshot --root @s8f3k2p9:e3 -i --compact` ile tek bölgeye in.
- Varsayılan olarak fareyi oynatmaz ve odağı almaz; gerçek fare ve klavye girdisi için genel `--headed` bayrağı gerekir.
- PowerShell'de her ref'i tırnak içine al (`'@s8f3k2p9:e8'`), çünkü çıplak `@token` orada splatting işlecidir.
- Gizli metinleri `type`, `set-value` ve `clipboard-set` komutlarına `--stdin` ile ver ki komut satırı argümanlarında görünmesin.
- Snapshot'lar, pano okumaları, ekran görüntüleri ve hata ayıklama çıktıları masaüstündeki özel içeriği taşıyabilir; README bunları herkese açık loglara ve yüklemelere koymamayı söylüyor.

## Bağlantılar

- Repo: https://github.com/vercel-labs/agent-desktop
- Site: https://agent-browser.dev/desktop
- Kurulum: https://agent-browser.dev/desktop/installation · Quick start: https://agent-browser.dev/desktop/quick-start
- Sürümler: https://github.com/vercel-labs/agent-desktop/releases (son sürüm v1.3.1, 13 Kasım 2026)
- Issues: https://github.com/vercel-labs/agent-desktop/issues
- Video: Haftanın Popüler Repoları, 9–15 Kasım 2026 bölümü (listenin 6. sırası)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W46 bölümü](../bolumler/2026-W46.md) videosu için hazırlandı (video henüz yayında değil). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
