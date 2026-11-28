---
repo: altic-dev/FluidVoice
url: https://github.com/altic-dev/FluidVoice
lisans: GPL-3.0
surum: v1.6.13 (2026-11-13)
readme_tarihi: 2026-11-21
dogrulama: denenmedi
bolum: trend-short/t2026-11-28-1
ozet: Mac'te konuştuğunu kullandığın uygulamalara yazan, ses tanımayı cihazda çalıştıran ücretsiz ve açık kaynak dikte uygulaması.
---

# FluidVoice — kurulum ve ilk örnek

> **Bu belge, README'nin 21 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

FluidVoice, Mac'te konuştuğunu zaten kullandığın uygulamalara yazı olarak yazan bir dikte uygulaması. README'ye göre ses tanıma cihazında çalışır, sözlerin yine cihazda düzeltilir ve toplantılarının aranabilir bir kaydı tutulur. Kendini GitHub açıklamasında "A local Wispr Flow alternative" diye tanıtır. Bu repo macOS uygulamasıdır; Windows ve iPhone için beta sürümler, Android ve Linux için bekleme listesi var. İsteğe bağlı olarak OpenAI, Groq ya da kendi seçtiğin bir sağlayıcının API anahtarıyla bulutta metin iyileştirme de eklenebilir. Lisansı GPL-3.0.

## Gereksinimler

- macOS 15.0 (Sequoia) ya da üstü.
- Fluid Speech, Parakeet, Nemotron, Qwen3 ASR, Cohere ve yerel Edit 1.1 modeli için Apple Silicon. Intel Mac'lerde Whisper ile çalışır.
- Seçtiğin ses modeli için yaklaşık 44 MiB ile 2,03 GiB arası disk alanı (kurulum için biraz fazlası).
- İsteğe bağlı yerel metin düzeltme için Ripple'da yaklaşık 600 MB, Splash'te yaklaşık 750 MB.
- Mikrofon izni ve başka uygulamalara yazabilmek için Accessibility (Erişilebilirlik) izni.
- Kaynaktan derlemek istersen Xcode (bağımlılıklar Swift Package Manager ile gelir).

## Kurulum

Homebrew ile (README'nin Quick Start'ı):

```bash
brew install --cask fluidvoice
```

Ya da reponun Releases sayfasından son sürümü indir: https://github.com/altic-dev/FluidVoice/releases/latest

README uyarıyor: fluidvoice.org adresi projeyle ilgili değil. Resmî adresler fluidvoice.com, altic.dev ve fluidvoice.app; resmî indirmeler için reponun GitHub Releases sayfasını kullan.

Kaynaktan derlemek için:

```bash
git clone https://github.com/altic-dev/FluidVoice.git
cd FluidVoice
open Fluid.xcodeproj
```

Xcode'da derleyip çalıştır. İmzalı Debug derlemesi için:

```bash
./build.sh
```

İmzalama kimliğin yoksa imzasız derleme:

```bash
./build.sh unsigned
```

İmzasız derlemelerde her yeniden derlemeden sonra Accessibility, Screen Recording ve mikrofon izinlerini kaldırıp yeniden vermen gerekebilir (README).

## İlk çalıştırma

1. Uygulamayı aç; mikrofon ve Accessibility izni ister. İkisi de dikte ve başka uygulamalara yazmak için gerekli.
2. Ayarlarda her yerden ses kaydını başlatacak genel bir kısayol tuşu (hotkey) seç. Fare tuşları ve F13–F19 için README'deki hotkey sorun giderme belgesine bak.
3. İlk kurulum adımlarında dilini ve hız ihtiyacına göre ses modelini seç: doğruluk için Fluid Speech Pro, daha küçük indirme için Mini ya da Pico, Intel Mac'te Whisper.
4. İsteğe bağlı: cihazda metin düzeltme için Fluid Intelligence'ı açıp Ripple ya da Splash'i seç. README'ye göre bulut hesabı ya da API anahtarı gerekmez.
5. İsteğe bağlı: bulutta iyileştirme için OpenAI, Groq ya da özel bir sağlayıcının anahtarını ekle (`<API_ANAHTARI>`). Anahtarlar macOS Keychain'de saklanır; anahtar erişimi sorulduğunda "Always allow" seç.
6. Herhangi bir uygulamada imleci bir metin alanına koy, kısayol tuşuna bas ve konuş; yazı o alana düşer.

Dikteyi bitirmeden dinlemeyi duraklatmak için **Settings → Dictation → Hold Space to Mute** seçeneği var: dikte sırasında Space'e basılı tutunca mikrofon susar, bırakınca devam eder (varsayılan kapalı).

## Sık takılınan yerler

- Kısayol tuşu çalışmıyorsa (özellikle fare tuşları ve F13–F19) README'nin bağlantı verdiği `docs/HOTKEY_TROUBLESHOOTING.md` belgesine bak.
- Kaynaktan imzalı derlemede her seferinde `DerivedData/Build/Products/Debug/FluidVoice Debug.app` dosyasını aç; README'ye göre macOS Accessibility iznini böyle korur.
- Birden çok geliştirici takımın sertifikası varsa projeyi değiştirmeden takım seçmek için README `FLUIDVOICE_DEVELOPMENT_TEAM=YOUR_TEAM_ID ./build.sh` komutunu verir (`YOUR_TEAM_ID` yerine kendi takım kimliğin).
- Özellikler ve modeller platforma göre değişir; README'deki özellik listesi Mac uygulaması içindir.

## Bağlantılar

- Repo: https://github.com/altic-dev/FluidVoice
- Site: https://fluidvoice.com · https://altic.dev/fluid
- Sürümler: https://github.com/altic-dev/FluidVoice/releases (son sürüm v1.6.13, 13 Kasım 2026)
- Issues: https://github.com/altic-dev/FluidVoice/issues
- Video: Haftanın Popüler Repoları, 28 Kasım 2026 Short'u ("Wispr Flow'a boşuna para ödeme: FluidVoice")

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki trend Short t2026-11-28-1 videosu için hazırlandı (video henüz yayında değil). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
