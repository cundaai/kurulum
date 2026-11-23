---
repo: dream-num/univer
url: https://github.com/dream-num/univer
lisans: Apache-2.0
surum: v1.6.1 (2026-11-23)
readme_tarihi: 2026-11-16
dogrulama: denenmedi
bolum: 2026-W47
ozet: Kendi ürününün içine tablo ve belge düzenleyicisi gömmeni sağlayan, insanların ve agent'ların aynı içerikte çalıştığı açık kaynak Office SDK.
---

# univer — kurulum ve ilk örnek

> **Bu belge, README'nin 16 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

Univer, kendi ürününün içinde Office uygulaması kurmak için bir SDK: düzenleme, hesaplama, ortak çalışma ve dosya işleme sunar; insanlar ve mevcut agent'lar senin verin ve izinlerinle aynı içerik üzerinde çalışır. README'ye göre üç parçası var: Web SDK (içerik modelleri, düzenleme, çizim, formül hesaplama, Facade API), Server SDK (gerçek zamanlı ortak çalışma, geçmiş, Office dosyası içe ve dışa aktarma) ve AI SDK. Bu repo Web SDK'nın çekirdeğini ve Sheets ile Docs düzenlemesini Apache-2.0 ile verir; ticari Web SDK plugin'leri, Server SDK ve ticari AI SDK uzantıları ayrı dağıtılır ve ayrı lisanslanır. Aşağıdaki örnek yalnız açık kaynak paketleri kullanır; ticari lisans ya da Server SDK istemez.

## Gereksinimler

- Vite gibi bir bundler kullanan tarayıcı uygulaması. README Vite, esbuild ya da Webpack 5 öneriyor.
- Paket yöneticisi: README'nin komutları `pnpm` ile yazılmış.
- React: görünüm katmanı React 18 üzerine kurulu; React 18 ve 19 desteklenir, 16.9+ ve 17 için en düşük düzeyde uyumluluk var.
- Tarayıcı: Edge 88+, Firefox 90+, Chrome 88+, Safari 14.1+, Electron 12+.
- `Intl.Segmenter`: hedef tarayıcıda ya da çalışma ortamında yoksa `@formatjs/intl-segmenter` gibi bir polyfill eklenir.
- Düzenleyicisiz (headless) kullanım için Node.js `>=18.17.0`; bu repoyu geliştirmek için Node.js `>=22.18`.

## Kurulum

Tarayıcı uygulamana preset paketlerini ekle (README'nin "Quick start" bölümü):

```bash
pnpm add @univerjs/presets @univerjs/preset-sheets-core
```

Preset'ler plugin'leri bir araya getirir ve Facade API'lerini kaydeder. Plugin'leri tek tek seçmek istersen README'nin "Plugin Mode" başlığındaki elle kurulum var:

```bash
pnpm add @univerjs/core @univerjs/design @univerjs/docs @univerjs/docs-ui \
  @univerjs/engine-formula @univerjs/engine-render @univerjs/network \
  @univerjs/sheets @univerjs/sheets-ui @univerjs/sheets-formula \
  @univerjs/sheets-formula-ui @univerjs/sheets-numfmt \
  @univerjs/sheets-numfmt-ui @univerjs/ui
```

Bu yolun başlatma kodu uzun; README'deki "Advanced: compose plugins manually" bölümünden alınır. İki kip, peer dependency'ler ve framework bağlantıları kurulum kılavuzunda (Bağlantılar).

Repoyu kendin geliştirmek için (README'nin "Local development" bölümü; pnpm sürümü `package.json` içindeki `packageManager` alanından):

```bash
git clone https://github.com/dream-num/univer.git
cd univer
pnpm install
pnpm dev
```

## İlk çalıştırma

1. Univer'i başlatmadan önce kapsayıcıyı oluştur:

```html
<div id="app" style="height: 100vh"></div>
```

2. Preset ile Univer'i başlat ve boş bir çalışma kitabı aç:

```ts
import { UniverSheetsCorePreset } from '@univerjs/preset-sheets-core'
import UniverPresetSheetsCoreEnUS from '@univerjs/preset-sheets-core/locales/en-US'
import { createUniver, LocaleType, mergeLocales } from '@univerjs/presets'
import '@univerjs/preset-sheets-core/lib/index.css'

const { univerAPI } = createUniver({
  locale: LocaleType.EN_US,
  locales: {
    [LocaleType.EN_US]: mergeLocales(UniverPresetSheetsCoreEnUS),
  },
  presets: [
    UniverSheetsCorePreset({
      container: 'app',
    }),
  ],
})

univerAPI.createWorkbook({})
```

README'ye göre sayfada artık bir tablo düzenleyicisi olur; içeriğe dönen `univerAPI` ile erişirsin.

3. Uygulama kodundan hücre yaz (README'nin teklif örneğinden bir parça; örneğin tamamı formül denetleyicilerinin hazır olmasını bekleyen bir adımla başlar, "Connect your application to editable content" bölümünde):

```ts
const workbook = univerAPI.getActiveWorkbook()!
const sheet = workbook.getActiveSheet()

sheet.getRange('A1:D4').setValues([
  ['Item', 'Quantity', 'Unit price', 'Amount'],
  ['Seats', 2, 25, '=B2*C2'],
  ['Support', 1, 100, '=B3*C3'],
  ['Total', '', '', '=SUM(D2:D3)'],
])
```

`workbook.save()` çalışma kitabının JSON anlık görüntüsünü döndürür; saklamak senin uygulamanın işi. README uygulamayı çalıştıracak komutu (dev sunucusu) vermiyor; bu, kullandığın bundler'a bağlı.

## Sık takılınan yerler

- Birlikte sürümlenen `@univerjs/*` paketlerini aynı sürümde tut; ticari paketler de eşleşen sürüm hattında olmalı (README, API Stability Policy).
- React gibi bileşen framework'lerinde `createUniver`'in döndürdüğü `univer` örneğini sakla ve bileşen kaldırılırken `univer.dispose()` çağır (lifecycle kılavuzu).
- Kayıtlı bir anlık görüntü varsa sayfa açılışında boş çalışma kitabı oluşturma; onun yerine `univerAPI.createWorkbook(JSON.parse(saved))` ile geri yükle.
- Anlık görüntü saklama, XLSX içe ve dışa aktarmadan ayrı: XLSX ayrı dağıtılan dosya dönüştürme yetenekleriyle yapılır, bu repodaki açık kaynak paketlerle değil.
- Bundler'ın `package.json` içindeki `exports` alanını desteklemiyorsa (Webpack 4'te yaygın) ek yol eşlemesi gerekebilir.
- Ücretsiz deneme (Freemium) kipindeki ticari uzantılar filigran ve özellik sınırı içerir.

## Bağlantılar

- Repo: https://github.com/dream-num/univer
- Site: https://univer.ai · Belgeler: https://docs.univer.ai
- Kurulum kılavuzu: https://docs.univer.ai/guides/sheets/getting-started/installation · Headless (Node.js): https://docs.univer.ai/guides/sheets/getting-started/node · Lifecycle: https://docs.univer.ai/guides/sheets/getting-started/lifecycle
- Açık kaynak ve ticari özellik karşılaştırması: https://univer.ai/compare/ · Planlar ve lisans: https://univer.ai/pricing/
- Sürümler: https://github.com/dream-num/univer/releases (son sürüm v1.6.1, 23 Kasım 2026)
- Issues: https://github.com/dream-num/univer/issues · Discussions: https://github.com/dream-num/univer/discussions · Discord: https://discord.gg/z3NKNT6D2f
- Video: Haftanın Popüler Repoları, 16–22 Kasım 2026 bölümü

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W47 bölümü](../bolumler/2026-W47.md) videosu için hazırlandı (video henüz yayında değil). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
