---
repo: thedotmack/claude-mem
url: https://github.com/thedotmack/claude-mem
lisans: Apache-2.0
surum: v15.18.3 (2026-11-12)
readme_tarihi: 2026-11-12
dogrulama: denenmedi
bolum: 2026-W45
ozet: Coding agent'ın oturumlar arasında unuttuğu bağlamı kaydedip sıkıştıran ve sonraki oturuma geri veren kalıcı hafıza eklentisi.
---

# claude-mem — kurulum ve ilk örnek

> **Bu belge, README'nin 12 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

Coding agent'ın her yeni oturumda sıfırdan başlar: aynı dosyaları yeniden okur, aynı soruları sorar, düzelttiğin hatalara geri döner. Claude-Mem agent'ın yanında ikinci bir agent çalıştırır; o agent iş sırasında not alır, oturum sonunda özet yazar ve bir sonraki oturum projeye dair kısa bir dizinle açılır, ayrıntıyı yalnız gerektiğinde çeker. Hafıza bilgisayarında `~/.claude-mem` altında SQLite'ta durur; bulut eşitleme isteğe bağlı. Claude Code, Codex, Cursor, OpenCode ve 11 agent daha kaydediliyor; Copilot CLI, Goose, Roo Code ve Warp yalnız hafızada arama yapabiliyor. Claude Code ile uzun projelerde çalışan geliştiriciler için.

## Gereksinimler

- Node.js 20.19 veya üstü (20.x hattı) ya da 22.12 veya üstü.
- Desteklenen bir agent: güncel, plugin destekli Claude Code ya da README'deki "Other agents" listesinden biri (OpenCode 1.3.4+, T3 Code, Antigravity CLI, OMP, Pi, DeepSeek Harness, Grok Bot).
- Bun (JavaScript runtime; yoksa kurulum kendisi kurar), uv (vektör arama için Python paket yöneticisi; yoksa kendisi kurar), SQLite 3 (paketle gelir).
- Hesap: kurulum sonunda tarayıcıda claude-mem'e giriş istenir (e-posta ile magic link, kart gerekmez). "claude-mem observer" seçilirse 30 günlük deneme bir ödeme yöntemi ister; deneme bitince hafıza Anthropic planına döner. Girişi atlamak için `--provider` bayrağı ver ya da `CLAUDE_MEM_ONLINE_OPTIN=false` ayarla. Hafıza sağlayıcısı olarak kendi OpenRouter ya da Gemini anahtarın ya da Anthropic planın da seçilebilir.
- İşletim sistemi: macOS, Linux, Windows (Windows notları README'de).

## Kurulum

En kısa yol; kurulum bilgisayarındaki agent'ları bulur ve hangilerini kuracağını sorar:

```bash
npx claude-mem install
```

Ya da Claude Code içinden plugin marketplace ile:

```bash
/plugin marketplace add thedotmack/claude-mem

/plugin install claude-mem
```

Sonra Claude Code'u yeniden başlat. README'nin notu: `npm install -g claude-mem` yalnız SDK/kütüphaneyi kurar, hook'ları ve worker servisini kurmaz; her zaman yukarıdaki iki yoldan birini kullan.

Tek bir agent'ı doğrudan kurmak için `--ide` ver (tam liste `npx claude-mem --help`):

```bash
npx claude-mem install --ide opencode
```

Windows: kurulum `claude-mem` başlatıcısını `%LOCALAPPDATA%\claude-mem\bin\claude-mem.exe` altına koyar ve klasörü kullanıcı Path'ine ekler; sonrasında terminali ya da IDE'yi yeniden başlat. Git Bash gerekmez.

## İlk çalıştırma

1. Kurulumdan sonra projede normal çalışmaya başla; ilk prompt'tan itibaren gözlemler akar.
2. Aynı projede **ikinci** oturum, birincinin hafızasıyla açılır (hafıza ekleme ikinci oturumda başlar). Tüm repoyu baştan öğretmek istersen isteğe bağlı `/learn-codebase` (tipik bir repoda yaklaşık 5 dakika).
3. Durumu denetle:

   ```bash
   npx claude-mem doctor
   ```

   Bun, uv, hook başlatıcısı ve worker'ı kontrol edip neyi düzelteceğini söyler.
4. Başlangıçta yazdırılan worker adresinde **My Work** (canlı istekler, inceleme kuyruğu) ve `/viewer` (hafıza, oturumlar, ayarlar) açılır.
5. Gözlemleri Türkçe yazdırmak için `~/.claude-mem/settings.json`:

   ```json
   {
     "CLAUDE_MEM_MODE": "code--tr"
   }
   ```

   Sonra Claude Code'u yeniden başlat. Anonim kullanım analitiği varsayılan açık; kapatmak için `npx claude-mem telemetry disable`.

## Sık takılınan yerler

- **`npm install -g claude-mem` ile kurdum, hiçbir şey olmuyor.** Bu yol yalnız kütüphaneyi kurar; hook ve worker yok. `npx claude-mem install` ya da `/plugin` komutlarıyla kur. Kaynak: README, "Quick Start" notu.
- **Hook'lar `Executable not found in $PATH: "claude-mem"` diyor.** `npx claude-mem install` çalıştır, terminali ve Claude Code'u yeniden başlat, `npx claude-mem doctor` ile bak. Kaynak: README "Windows Setup Notes" ve [docs: Troubleshooting](https://docs.claude-mem.ai/troubleshooting#claude-mem-command-not-found-in-hooks).
- **`/plugin install claude-mem` plugin'i bulamıyor.** Önce `/plugin marketplace add thedotmack/claude-mem`. Kaynak: [docs: Troubleshooting](https://docs.claude-mem.ai/troubleshooting).
- **Windows'ta `npm : The term 'npm' is not recognized`.** Node.js ve npm kurulu ve PATH'te olmalı; kurulumdan sonra terminali yeniden başlat. Kaynak: README, "Windows Setup Notes".
- **İlk oturumda hafıza görünmüyor.** Normal: bağlam ikinci oturumdan itibaren eklenir. Kaynak: README, "How It Works".

## Bağlantılar

- Repo: https://github.com/thedotmack/claude-mem
- Belgeler: https://docs.claude-mem.ai/ (kurulum, yapılandırma, sorun giderme; Türkçe README: `docs/i18n/README.tr.md`)
- Sürümler: https://github.com/thedotmack/claude-mem/releases
- Lisans: https://github.com/thedotmack/claude-mem/blob/main/LICENSE (Apache-2.0; kapsam notları `docs/license.md`)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W45 bölümü](../bolumler/2026-W45.md) videosu için hazırlandı: https://www.youtube.com/watch?v=AkiRwaGZDZA. Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
