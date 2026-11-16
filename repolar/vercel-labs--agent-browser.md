---
repo: vercel-labs/agent-browser
url: https://github.com/vercel-labs/agent-browser
lisans: Apache-2.0
surum: v0.43.0 (2026-11-12)
readme_tarihi: 2026-11-05
dogrulama: denenmedi
bolum: 2026-W46
ozet: AI agent'lara sayfaları açıp erişilebilirlik ağacı olarak okuyan ve öğelere tıklayan, komut komut sürülen bir tarayıcı veren Rust CLI.
---

# agent-browser — kurulum ve ilk örnek

> **Bu belge, README'nin 5 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

agent-browser, bir AI agent'ın tek tek kabuk komutlarıyla sürebileceği bir tarayıcı veren, Rust ile yazılmış bir CLI. `snapshot` komutu sayfayı kısa bir metin olarak döndürür ve her öğeye `@e1` gibi bir ref verir; agent bu ref üstünde işlem yapar, sonucu görmek için sayfayı yeniden okur ve böyle devam eder. Tarayıcı komutlar arasında açık kalır: komutlar arka planda çalışan bir daemon'a gider, daemon tarayıcıyı Chrome DevTools Protocol ile yönetir. Varsayılan tarayıcı Chrome for Testing; README'ye göre çalışan bir Chrome'a bağlanmak, başka motorlar, iOS Simulator'da Mobile Safari ya da bulut tarayıcı sağlayıcıları da desteklenir. Agent'lar için sürümüyle uyumlu skill'ler de gelir. Lisansı Apache-2.0.

## Gereksinimler

- npm ile kurulum için Node.js (ya da Homebrew).
- İlk kurulumda indirilen Chrome for Testing (`agent-browser install`).
- Linux'ta tarayıcının sistem kütüphaneleri (`--with-deps` ile kurulabilir).

## Kurulum

```bash
npm install -g agent-browser
agent-browser install           # download Chrome for Testing (first time only)
```

- Homebrew: `brew install agent-browser`, ardından `agent-browser install`.
- Linux: `agent-browser install --with-deps` tarayıcının sistem kütüphanelerini de kurar.
- Kurmadan denemek için: `npx agent-browser open example.com`.

Projeye özel kurulum, kaynaktan derleme, `upgrade` ve `doctor` komutları için: https://agent-browser.dev/installation

## İlk çalıştırma

README'nin hızlı başlangıç örneği:

```bash
agent-browser open example.com
agent-browser snapshot -i       # interactive elements, each with a ref
agent-browser click @e1         # act on a ref from that snapshot
agent-browser get url           # check the result
agent-browser close
```

`example.com` için snapshot çıktısı şöyle bir satırdır: `- link "Learn more" [ref=e1]`. Ref'ler yerine CSS seçicileri, metin, XPath ve anlamsal bulucular da çalışır; örneğin `agent-browser find role button click --name "Submit"`. Adım adım anlatım: https://agent-browser.dev/quick-start

## Sık takılınan yerler

- Ref'ler snapshot alındığı andaki sayfayı anlatır; sayfa değiştikten ya da başka adrese gidildikten sonra ref kullanmadan önce yeni bir snapshot al.
- Hedef gizli, devre dışı, görünür alanın dışında ya da çerez onay bandı gibi başka bir öğenin altındaysa tıklama başarılı görünmek yerine hata verir; üstteki öğeyi kapat, yeni snapshot al ve yeniden dene.
- README'ye göre daemon, bir saat komut gelmezse başsız (headless) tarayıcıyı kapatıp çıkar.

## Bağlantılar

- Repo: https://github.com/vercel-labs/agent-browser
- Site ve belgeler: https://agent-browser.dev (kurulum: /installation, hızlı başlangıç: /quick-start, seçiciler: /selectors)
- Sürümler: https://github.com/vercel-labs/agent-browser/releases (son sürüm v0.43.0, 12 Kasım 2026)
- Issues: https://github.com/vercel-labs/agent-browser/issues
- Video: Haftanın Popüler Repoları, 9–15 Kasım 2026 bölümü (listenin 3. sırası)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W46 bölümü](../bolumler/2026-W46.md) videosu için hazırlandı (video henüz yayında değil). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
