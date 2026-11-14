---
repo: meta/astryx
url: https://github.com/meta/astryx
lisans: MIT
surum: v0.7.15 (2026-11-12)
readme_tarihi: 2026-11-12
dogrulama: denenmedi
bolum: trend-short/t2026-11-12-1
ozet: Meta'nın React 19 ve StyleX üzerine kurulu, 150'den fazla bileşen, tema ve CLI ile gelen, agent'lara hazır açık kaynak design system'ı.
---

# Astryx — kurulum ve ilk örnek

> **Bu belge, README'nin 12 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

Astryx, Meta'nın içinde sekiz yılda büyüyüp 13.000'den fazla uygulamada kullanılan design system'ın açık kaynak sürümüdür. 150'den fazla erişilebilir React bileşeni, marka düzeyinde tema, dark mode, hazır sayfa şablonları ve bir CLI tek paket hâlinde gelir. Stilleri StyleX ile yazılmıştır ama kullanan tarafa derlenmiş CSS olarak ulaşır; build plugin'i, PostCSS ya da Babel ayarı gerekmez, `className` ile Tailwind ya da düz CSS üstüne yazılabilir. CLI'ı bileşen belgelerini agent'ın okuyacağı biçimde verdiği için insan ve AI asistanı aynı kaynaktan çalışır. Henüz Beta.

## Gereksinimler

- Node.js ve npm, pnpm ya da yarn. Kullanıcı tarafı için README Node sürümü belirtmiyor; repoya katkı için Node 22+ (aktif LTS) ve pnpm 11 istiyor.
- React 19 ve üzeri; `react` ve `react-dom` `@astryxdesign/core` paketinin peer dependency'si.
- Next.js, Vite ya da bundler'sız CDN sayfası; Tailwind isteğe bağlı.
- Hesap ya da anahtar gerekmez.

## Kurulum

Core paketi, bir tema ve StyleX runtime'ı, ayrıca geliştirme bağımlılığı olarak CLI:

```bash
# npm
npm install @astryxdesign/core @astryxdesign/theme-neutral @stylexjs/stylex
npm install -D @astryxdesign/cli

# pnpm
pnpm add @astryxdesign/core @astryxdesign/theme-neutral @stylexjs/stylex
pnpm add -D @astryxdesign/cli

# yarn
yarn add @astryxdesign/core @astryxdesign/theme-neutral @stylexjs/stylex
yarn add -D @astryxdesign/cli
```

CLI'ı makine geneline de kurabilirsin; proje içinde yine projenin kendi sürümü çalışır (README ikisini birden öneriyor):

```bash
npm i -g @astryxdesign/cli
```

CLI'a güvenilir erişim için `package.json` dosyasına şu script'i ekle; `npm run astryx -- component --list` biçiminde kullanılır ve AI asistanlarının yol hatası almasını önler:

```json
"scripts": {
  "astryx": "node node_modules/@astryxdesign/cli/clients/cli/bin/astryx.mjs"
}
```

Agent ile çalışıyorsan bir kez `npx @astryxdesign/cli init` çalıştır: bileşen dizinini `AGENTS.md` / `CLAUDE.md` dosyana yazar, asistan bileşenleri tahmin etmek yerine oradan bulur.

## İlk çalıştırma

En basit kurulum Next.js: üç CSS import'u ve bir tema sağlayıcısı (kaynak: [packages/core/README.md](https://github.com/meta/astryx/blob/main/packages/core/README.md#quick-start)). Sıra önemli: reset, bileşen stilleri, tema.

`src/app/globals.css`:

```css
@import '@astryxdesign/core/reset.css';
@import '@astryxdesign/core/astryx.css';
@import '@astryxdesign/theme-neutral/theme.css';
```

`src/app/providers.tsx`:

```tsx
'use client';

import Link from 'next/link';
import {Theme} from '@astryxdesign/core/theme';
import {LinkProvider} from '@astryxdesign/core/Link';
import {neutralTheme} from '@astryxdesign/theme-neutral/built';

export function Providers({children}: {children: React.ReactNode}) {
  return (
    <Theme theme={neutralTheme}>
      <LinkProvider component={Link}>{children}</LinkProvider>
    </Theme>
  );
}
```

`src/app/layout.tsx` içinde `./globals.css` import edilir ve `<body>` içeriği `<Providers>` ile sarılır. Sonra ilk bileşen:

```tsx
import {Button} from '@astryxdesign/core/Button';

export default function Page() {
  return <Button label="Hello Astryx" variant="primary" />;
}
```

Sayfada neutral temalı bir primary düğme görmelisin. Bileşen belgelerine CLI'dan bak:

```bash
npx @astryxdesign/cli component Button        # full docs for a component
npx @astryxdesign/cli component --list        # list all components
```

Vite'ta aynı CSS import'ları ve provider'lar geçerli; bundler'sız sayfa için `npx @astryxdesign/cli template --cdn` hazır bir `cdn.template.html` yazar.

## Sık takılınan yerler

- Scope'suz `astryx` komutu, `@astryxdesign/cli` bağımlılık olarak kurulana kadar npm'deki ilgisiz bir pakete gider; her zaman `npx @astryxdesign/cli ...` ya da kurulumdan sonra `pnpm exec astryx ...` kullan ([core README](https://github.com/meta/astryx/blob/main/packages/core/README.md)).
- Tailwind ile kullanırken `@layer reset, theme, base, astryx-base, astryx-theme, components, utilities;` satırı `@import "tailwindcss"` satırının üstünde olmalı; altına koyarsan cascade sessizce tersine döner ([core README → Next.js + Tailwind](https://github.com/meta/astryx/blob/main/packages/core/README.md#nextjs--tailwind)).
- CDN sayfasında `<html data-astryx-theme="neutral">` yoksa tema uygulanmaz; `?external=react,react-dom` yoksa esm.sh kendi React'ını paketler ve hook'lar `useState` hatası verir; `react/jsx-runtime` import map'te yoksa modül çözülemez ([core README → No build step](https://github.com/meta/astryx/blob/main/packages/core/README.md#no-build-step-cdn)).
- Repoya katkı için `corepack enable` sonrası `pnpm install`; `corepack` yoksa pnpm'i doğrudan kur ([README → Contributing](https://github.com/meta/astryx#contributing)).
- `@astryxdesign/charts` ve `@astryxdesign/vega` yalnız `@canary` etiketiyle yayımlı, kararlı sürümü yok; `@astryxdesign/lab` npm'de hiç yok ([README → Packages](https://github.com/meta/astryx#packages)).

## Bağlantılar

- Repo: https://github.com/meta/astryx
- Belgeler: https://astryx.atmeta.com, [Storybook](https://astryx.atmeta.com/storybook/), [Sandbox](https://astryx.atmeta.com/sandbox/), [packages/core/README.md](https://github.com/meta/astryx/blob/main/packages/core/README.md), [packages/cli/README.md](https://github.com/meta/astryx/blob/main/packages/cli/README.md)
- npm paketi: https://www.npmjs.com/package/@astryxdesign/core
- Sürümler: https://github.com/meta/astryx/releases
- Lisans: https://github.com/meta/astryx/blob/main/LICENSE (MIT)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki trend Short t2026-11-12-1 videosu için hazırlandı: https://www.youtube.com/shorts/UuXvMFLqKHI. Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
