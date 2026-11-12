---
repo: Nutlope/hallmark
url: https://github.com/Nutlope/hallmark
lisans: MIT
surum: sürüm etiketi yok
readme_tarihi: 2026-11-11
dogrulama: denenmedi
bolum: 2026-W45
ozet: Claude Code, Codex, Cursor ve OpenCode için yapay zekâ işi gibi görünmeyen web sayfaları üreten, elli temalı tasarım skill'i.
---

# hallmark — kurulum ve ilk örnek

> **Bu belge, README'nin 11 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

Together AI'nin yaptığı, coding agent'ına "AI slop" görünmeyen sayfalar yaptıran bir tasarım skill'i. Elli malzemeden (kâğıt, yazı tipi, renk, yüzey, bileşen) üç tema önerir, sayfayı ziyaretçinin yapmaya geldiği işin çevresine kurar ve sonucu gerçek tarayıcıda makine işi belirtilerine karşı denetler: her başlığın üstündeki eyebrow, tracked-capital etiketler, başlıkta tek renkli kelime, ekrana sığmayan hero, her sayfanın sonundaki e-posta formu gibi. Kapılar kademeli: **Floor** (kontrast, odak, azaltılmış hareket, uydurma bilgi, bozuk telefon düzeni) asla geçmez; **Reflex** gerekçeyle aşılabilir; **Finish** yalnız bilgi verir. Kendi ürününün URL'sini ya da dev sunucusunu verirsen ekran görüntülerini kendisi alır; `redesign` mevcut sayfayı içerik ve bağlantıları koruyarak yeniden kurar, `audit` yalnız rapor verir.

## Gereksinimler

- Node 18 veya üstü (`npx skills add` için).
- Desteklenen bir harness: Claude Code, Codex CLI, Cursor, OpenCode (README'de `github-copilot` de `--agent` seçeneği olarak geçiyor) ve o aracın kendi hesabı.
- Tarayıcı denetimleri (`sloplint --render`, ekran görüntüleri) için `npm i -g puppeteer-core` ve Chrome; bunlar yoksa yalnız statik denetimler çalışır.
- Hesap/anahtar: skill'in kendisi için gerekmiyor.
- İşletim sistemi: README belirtmiyor.

## Kurulum

Tüm projeler için (`-g`); yalnız içinde bulunduğun projeye kurmak için `-g` olmadan:

```
npx skills add nutlope/hallmark -g
```

Nereye kurulduğu ve nasıl çağrıldığı (README tablosu):

| Harness | Kurulduğu yer | Çağırma |
| --- | --- | --- |
| Claude Code | `~/.claude/skills/hallmark` | `/hallmark <prompt>` ya da yeni bir sayfa tarif et |
| Codex CLI | `~/.agents/skills/hallmark` | `$hallmark <prompt>` ya da yeni bir sayfa tarif et |
| Cursor | yukarıdaki kopyaları bulur | `/hallmark <prompt>` |
| OpenCode | yukarıdaki kopyaları bulur | `/hallmark <prompt>` |

Tek harness'a kurmak için:

```
npx skills add nutlope/hallmark -g --agent claude-code
```

(`codex`, `opencode`, `cursor`, `github-copilot` de olur.) Claude Code plugin'i olarak:

```
/plugin marketplace add nutlope/hallmark
/plugin install hallmark@hallmark
```

Bu yolda skill `/hallmark:hallmark` olarak çağrılır. Klondan kurmak için `node skills/hallmark/scripts/install.mjs` (kopyalamadan önce her temayı ve referansı doğrular); `install-hook.mjs` Claude Code'a düzenleme anında Floor denetimi ekler.

## İlk çalıştırma

Claude Code'da (Codex'te `$hallmark`):

```
/hallmark <yapmak istediğin sayfanın tarifi>
```

README'ye göre akış: ön kontrol projeyi okur (yazı tipleri, palet, framework, kilitli `design.md`) ve malzemeyi bir kez sorar (fotoğraf, ekran görüntüsü, gerçek sayılar; hiçbir şey uydurulmaz); `scripts/theme.mjs deal` prompt'a uyan üç tema önerir; önce hero tek başına kurulup soğuk gözle incelenir; sonra `scripts/sloplint.mjs --render` sayfayı sekiz genişlikte denetler ve ayrı bir subagent taze gözle son incelemeyi yapar. Çıktı tek dosyalık, kendi içinde HTML ve CSS; aldığı kararlar ilk CSS yorumuna yazılır. Tema istemiyorsan **custom** de.

Mevcut bir sayfa için:

```
/hallmark redesign ./src/app
/hallmark audit ./index.html
```

Herhangi bir sayfayı tek başına denetlemek için:

```
node skills/hallmark/scripts/sloplint.mjs <file-or-dir> --render
```

## Sık takılınan yerler

- **Kurulumdan sonra SKILL.md içindeki göreli bağlantılar kopuk.** `npx skills add` yalnız `skills/hallmark/` klasörünü kopyalar; `../../site/css/tokens.css` gibi bağlantılar `~/.claude/skills/hallmark/` altında boşa düşer ve agent tema eksen değerlerini açamaz. Açık issue: [#76](https://github.com/Nutlope/hallmark/issues/76).
- **Tarayıcı denetimleri sessizce atlanıyor.** `puppeteer-core` ve Chrome yoksa `sloplint --render` ve ekran görüntüleri çalışmaz, yalnız statik denetimler kalır. Kaynak: README, "Install".
- **Skill kendiliğinden devreye girmiyor.** Adını vererek iste: "use hallmark to build ...". Kaynak: README, "Install".
- **Plugin yolunda `/hallmark` bulunamıyor.** Plugin olarak kurulduğunda komut `/hallmark:hallmark`. Kaynak: README, "Install".

## Bağlantılar

- Repo: https://github.com/Nutlope/hallmark
- Belgeler: https://www.usehallmark.com (elli temanın tamamı); temalar repoda `skills/hallmark/references/themes/`
- Sürümler: https://github.com/Nutlope/hallmark/releases (sürüm etiketi yok)
- Lisans: https://github.com/Nutlope/hallmark/blob/main/LICENSE (MIT; fotoğraf listesi Wikimedia Commons, her biri kendi lisansıyla)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W45 bölümü](../bolumler/2026-W45.md) videosu için hazırlandı: https://www.youtube.com/watch?v=AkiRwaGZDZA. Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
