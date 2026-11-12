---
repo: milind-soni/OpenMausBot
url: https://github.com/milind-soni/OpenMausBot
lisans: Apache-2.0
surum: v0.1.154 (2026-11-12)
readme_tarihi: 2026-11-12
dogrulama: denenmedi
bolum: 2026-W45
ozet: Claude, Codex ve Grok CLI'larını sohbet uygulamasında bot kadrosuna çeviren, yerel çalışan açık kaynak Grok Bot alternatifi.
---

# OpenMausBot — kurulum ve ilk örnek

> **Bu belge, README'nin 12 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

xAI'nin Grok Bot'undan esinlenen, açık kaynak ve yerel öncelikli bir masaüstü uygulaması: kenar çubuğundaki her "kişi" aslında bilgisayarında kurulu `claude`, `codex` ya da `grok` CLI'sıyla çalışan gerçek bir agent; her botun kendi kişiliği, modeli, (isteğe bağlı) bulut bilgisayarı ve bağlı uygulamaları var. Botlarla mesajlaşır gibi konuşursun, çalışmalarını izlersin, riskli işlemleri (kabuk komutu, dosya düzenleme) sohbet içindeki Allow/Deny kartlarıyla onaylarsın. Mevcut aboneliklerinle çalışır, araya proxy girmez; yazışmalar ve anahtarlar `~/.openmausbot` altında kalır. Elinde Claude Code ya da Codex aboneliği olan ve birden fazla agent'ı tek yerden yönetmek isteyen geliştirici için.

## Gereksinimler

- İşletim sistemi: macOS (Apple silicon ya da Intel), Windows x64 ya da Ubuntu 24.04 x64 (beta; desteklenen masaüstü GNOME).
- En az bir agent CLI kurulu ve giriş yapılmış: [`claude`](https://claude.com/claude-code), [`codex`](https://github.com/openai/codex) ya da [`grok`](https://x.ai/cli). Model seçicide kendiliğinden görünürler; yeni hesap gerekmez.
- Kaynaktan derlemek ya da terminalden çalıştırmak için Node 24+ ve pnpm (kaynak yolu). Hazır paketlerde harness server gömülü; ek kurulum yok.
- İsteğe bağlı anahtarlar (yerel sohbet bunlarsız çalışır): Composio proje anahtarı (Gmail, GitHub, Slack… bağlamak için), Boat API anahtarı (bota bulutta Linux masaüstü; deneme sonrası ücretli), ElevenLabs ya da Fish Audio anahtarı (sesli yanıt ve arama).

## Kurulum

### Hazır paket (önerilen)

Son sürümü [releases sayfasından](https://github.com/milind-soni/OpenMausBot/releases/latest) indir:

- **macOS (Apple silicon):** `OpenMausBot.dmg`; Applications'a sürükle, aç. İmzalı ve notarize.
- **macOS (Intel):** `OpenMausBot-intel.dmg`; aynı uygulama, Intel derlemesi.
- **Windows (x64):** `OpenMausBot-setup.exe`; tek tık, kullanıcı başına, yönetici hakkı istemez. Kurucu henüz kod imzalı değil; SmartScreen "unknown publisher" uyarısında **More info → Run anyway**.
- **Ubuntu 24.04 (x64):** `OpenMausBot-amd64.deb` (APT ile kurulum önerilir) ya da `OpenMausBot.AppImage` (çalıştırılabilir yap, aç). Beta. Ayrıntı: [docs/linux-desktop.md](https://github.com/milind-soni/OpenMausBot/blob/main/docs/linux-desktop.md).

### Kaynaktan

```sh
git clone https://github.com/milind-soni/OpenMausBot && cd OpenMausBot
pnpm install

pnpm dev:server    # harness server → 127.0.0.1:8799
pnpm dev           # app → http://127.0.0.1:5199
pnpm dev:desktop   # Electron shell; keep the two commands above running
```

Masaüstü paketini kendin üretmek için `pnpm package:mac` (Swift/Xcode araçları gerekir), `pnpm package:win`, `pnpm package:linux`.

### Terminalden ya da bir sunucuda

Node 24 veya üstüyle:

```sh
npm install -g openmausbot
openmausbot
```

Global kurmadan `npx openmausbot` da olur. Sunucu için `npx openmausbot serve` ve erişim seçenekleri (`--public-url`, `--domain`, `--tailscale`, `--lan`, `--tunnel`) [docs/self-hosting.md](https://github.com/milind-soni/OpenMausBot/blob/main/docs/self-hosting.md) içinde.

## İlk çalıştırma

1. Uygulamayı aç. Kurulu agent CLI'ları model seçicide görünür; olmayan sağlayıcılar nedeniyle birlikte soluk gösterilir.
2. Yeni bir bot oluştur, modelini seç ve bir mesaj yaz; yanıt akarak gelir, araç çalıştırmaları etkinlik çipleri olarak görünür.
3. Bot bir kabuk komutu ya da dosya düzenlemesi yapmak istediğinde sohbette onay kartı çıkar: **Allow / Deny** ya da soruya yanıt.
4. Computer panelini açarsan bot için bulut masaüstü kurulur (Boat anahtarı gerekir) ya da "This computer" ile kendi makineni verirsin (macOS ve Windows'ta destekli, Ubuntu'da sınırlı).
5. Entegrasyon anahtarlarını kenar çubuğu altındaki dişli (App Settings) üzerinden bir kez yapıştır; arayüz anahtarı bir daha göstermez, yalnız "configured" der.

Terminal yolunda ilk açılış ok tuşlarıyla yönlendirir: AI erişimi, giriş ya da gizli anahtar, model, isteğe bağlı telefon bağlantısı. `--no-open` tarayıcıyı açmaz; Ctrl-C sunucuyu durdurur, kayıtlı işler kalır.

## Sık takılınan yerler

- **Windows'ta SmartScreen kurucuyu engelliyor.** Kurucu henüz kod imzalı değil; "unknown publisher" uyarısında **More info → Run anyway**. Kaynak: README, "Quick start".
- **Ubuntu'da bot bilgisayarı kontrol edemiyor.** Wayland'da yerel kontrol güvenlik kapısı nedeniyle kapalı; Xorg'da açık onay gerekir. Sohbet, önizleme, Cloud ve Local VM her ikisinde çalışır. Takip: [#29](https://github.com/milind-soni/OpenMausBot/issues/29) (açık) ve README'nin "Desktop capability status" tablosu.
- **Model seçicide hiç sağlayıcı yok.** Botlar yerel `claude`, `codex`, `grok` CLI'larına dayanır; en az biri kurulu ve giriş yapılmış olmalı. Kaynak: README, "Quick start" gereksinimleri.
- **Bulut bilgisayar ücret çıkarıyor.** Boat deneme sonrası ücretli, bulut bilgisayar kullanımı ücret doğurabilir; Composio ve Boat ayrı hesap ve koşullara sahip üçüncü taraf servisler. Kaynak: README, anahtar tablosu.
- **Terminal yolunda anahtarlar düz metin.** `openmausbot setup` ve sunucu, anahtarları `config.json` içinde şifresiz (Unix'te `0600`) saklar; masaüstü paketi işletim sistemi şifrelemesini kullanır. Kaynak: README, "Run from a terminal or on a server".

## Bağlantılar

- Repo: https://github.com/milind-soni/OpenMausBot
- Belgeler: https://www.openmausbot.com ve repodaki `docs/` klasörü ([linux-desktop.md](https://github.com/milind-soni/OpenMausBot/blob/main/docs/linux-desktop.md), [cli-onboarding.md](https://github.com/milind-soni/OpenMausBot/blob/main/docs/cli-onboarding.md), [mcp-server.md](https://github.com/milind-soni/OpenMausBot/blob/main/docs/mcp-server.md), [composio.md](https://github.com/milind-soni/OpenMausBot/blob/main/docs/composio.md))
- Sürümler: https://github.com/milind-soni/OpenMausBot/releases
- Lisans: https://github.com/milind-soni/OpenMausBot/blob/main/LICENSE (Apache-2.0; `enterprise/` klasörü ayrı lisanslı, ayrıntı `LICENSING.md`)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W45 bölümü](../bolumler/2026-W45.md) videosu için hazırlandı: https://www.youtube.com/watch?v=AkiRwaGZDZA. Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
