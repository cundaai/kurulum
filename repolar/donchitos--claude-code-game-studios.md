---
repo: Donchitos/Claude-Code-Game-Studios
url: https://github.com/Donchitos/Claude-Code-Game-Studios
lisans: MIT
surum: v1.3.0 (2026-11-09)
readme_tarihi: 2026-11-09
dogrulama: denenmedi
bolum: 2026-W45
ozet: Claude Code'u oyun stüdyosuna çeviren şablon: bir cümlelik fikirden brief ve oynanabilir tarayıcı oyunu, 49 agent ve 75 skill.
---

# Claude-Code-Game-Studios — kurulum ve ilk örnek

> **Bu belge, README'nin 9 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

Klonlayıp kendi oyununa başladığın bir Claude Code şablonu. Bir oyun tarif edersin; Claude Code türe göre bir tur soru sorar, `design/game-brief.md` dosyasına tek sayfalık brief yazar ve `prototypes/<ad>/` altında çift tıkla açılan bir tarayıcı oyunu kurar; headless tarayıcıda açılıp tepki verdiğini kontrol eder, brief'in her satırını "yapıldı / eksik / bozuk" diye işaretler. Sonra değişiklikleri isteyerek yaparsın; beğendiğinde Vite + TypeScript web projesine büyür, her oturumda bir özellik eklenir. Takımlar ve uzun projeler için "studio mode" var: 49 agent (yönetmenler, bölüm liderleri, uzmanlar), 75 skill, hook'lar, yol kapsamlı kurallar, 62 belge şablonu; Godot 4, Unity ve Unreal Engine 5 için uzman agent setleri. Hafif yol varsayılan; README'nin ölçümüne göre ağır süreç daha iyi oyun üretmedi.

## Gereksinimler

README'nin "Prerequisites" listesi:

- [Git](https://git-scm.com/).
- [Claude Code](https://code.claude.com/docs/en/setup) ve oturum açılmış bir Anthropic hesabı; native kurucu önerilir (`npm install -g @anthropic-ai/claude-code` Node.js 22+ ile de olur).
- [Python 3](https://www.python.org/): her skill ve hook `project.yaml` dosyasını onunla okur; yoksa her ayar sessizce varsayılana döner.
- Bash: hook'ları çalıştırır; macOS ve Linux'ta var, Windows'ta [Git for Windows](https://git-scm.com/downloads/win) ile gelir.
- Chrome, Edge ya da Chromium: `/prototype` oyunu bunda oynar ve ekran görüntüsü alır; yoksa o denetimler "not assessed" der.
- Önerilen: Node.js (oyunu web projesine büyütmek, 3B oyun için three.js, web engine'in build ve testleri) ve jq (hook doğrulaması).
- İşletim sistemi: birincil geliştirme Windows 10 + Git Bash; hook'lar macOS (stok bash 3.2) ve Linux'ta da test edilmiş.

## Kurulum

1. Şablonu klonla (ya da GitHub'da "Use this template"):

   ```bash
   git clone https://github.com/Donchitos/Claude-Code-Game-Studios.git my-game
   cd my-game
   ```

2. Claude Code'u projenin **kök** klasöründe (`CLAUDE.md` ve `project.yaml` dosyalarının olduğu yer) başlat:

   ```bash
   claude
   ```

   `src/` gibi bir alt klasörde başlatırsan projenin hook'ları, deny kuralları ve "önce sor" izin modu devreye girmez.

Eski bir sürümden geçiyorsan [UPGRADING.md](https://github.com/Donchitos/Claude-Code-Game-Studios/blob/main/UPGRADING.md) hangi dosyaların üzerine yazılabileceğini, hangilerinin elle birleştirileceğini anlatıyor.

## İlk çalıştırma

Claude Code oturumunda fikrini yaz:

```
/prototype a fishing game where the rare fish only bite at dusk
```

Olacaklar (README, "Make me a game"):

1. Bir tur soru: türe göre seçimler, his, görünüm, zorluk, hedef. Her sorunun olağan bir yanıtı var, "you pick" demek yeterli.
2. `design/game-brief.md`: senin sözlerin, görünüm, ilk dakika, zorluk, özellikler, sonraya kalanlar. "Build it" dersin ya da önce düzeltirsin.
3. `prototypes/<name>/` altında ilk oyun; çift tıkla açılır.
4. Teslim kartı neyin geçtiğini, neyin bozuk olduğunu ve neyin tahmin edildiğini söyler; "Not yet played by a person" ile biter. Gerçek test senin oynaman.

Nereden başlayacağını bilmiyorsan `/start` durumunu sorar ve doğru akışa yönlendirir. Diğer girişler: `/next-step` (tek sonraki adım), `/brainstorm`, `/setup-engine godot 4.7`, `/project-stage-detect` (mevcut proje). Ayarları görmek ve değiştirmek için `/project-settings`; süreç ağırlığı `project.yaml` içindeki `modes.rigor` (`minimal` varsayılan, `standard`, `full`).

## Sık takılınan yerler

- **Hook'lar ve izin kuralları çalışmıyor.** Claude Code alt klasörde başlatılmış olabilir; kök klasörden (`CLAUDE.md` olan yer) başlat. Kaynak: README, "Setup" 2. adım.
- **Ayarlar değişmiyor.** Python 3 yoksa `project.yaml` okunamaz ve her ayar sessizce varsayılana döner; ayrıca veri JSON'u içeren commit Python olmadan engellenir. Kaynak: README, "Prerequisites".
- **Windows'ta hook'lar kapalı.** Bash (Git for Windows) yoksa hook'lar kendini atlar ve oturum başında denetimlerin kapalı olduğu söylenir. Kaynak: README, "Prerequisites".
- **Commit reddediliyor.** `git` kuralları komutun herhangi bir yerinde `push` sonrası ` +`/` -f`, `clean -f`, `reset --hard` görürse engeller; `git commit -m "push + pull"` gibi bir mesaj bile reddedilir, mesajı yeniden yaz. Kaynak: README, "Hooks, Permissions and Rules".
- **Çok fazla onay sorusu.** Bir kullanıcı sürecin her adımda elle onay beklemesinden şikâyetçi ([#29](https://github.com/Donchitos/Claude-Code-Game-Studios/issues/29), açık). README'ye göre `modes.automation` ayarının `guided` / `autonomous` değerleri ve Claude Code'un auto modu bu soruları azaltır; yollar `.claude/docs/setup-requirements.md` içinde.

## Bağlantılar

- Repo: https://github.com/Donchitos/Claude-Code-Game-Studios
- Belgeler: README; ayar şeması `.claude/docs/effects-map.md`; [UPGRADING.md](https://github.com/Donchitos/Claude-Code-Game-Studios/blob/main/UPGRADING.md); [GitHub Discussions](https://github.com/Donchitos/Claude-Code-Game-Studios/discussions)
- Sürümler: https://github.com/Donchitos/Claude-Code-Game-Studios/releases
- Lisans: https://github.com/Donchitos/Claude-Code-Game-Studios/blob/main/LICENSE

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W45 bölümü](../bolumler/2026-W45.md) videosu için hazırlandı: https://www.youtube.com/watch?v=AkiRwaGZDZA. Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
