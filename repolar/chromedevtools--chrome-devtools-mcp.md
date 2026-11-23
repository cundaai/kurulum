---
repo: ChromeDevTools/chrome-devtools-mcp
url: https://github.com/ChromeDevTools/chrome-devtools-mcp
lisans: Apache-2.0
surum: chrome-devtools-mcp-v1.13.0 (2026-11-11)
readme_tarihi: 2026-11-09
dogrulama: denenmedi
bolum: 2026-W47
ozet: Coding agent'ının canlı bir Chrome tarayıcısını yönetip incelemesini sağlayan, Chrome DevTools'u MCP sunucusu ve CLI olarak sunan araç.
---

# chrome-devtools-mcp — kurulum ve ilk örnek

> **Bu belge, README'nin 9 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

Chrome DevTools for agents (`chrome-devtools-mcp`), coding agent'ının (README'nin örnekleri: Antigravity, Claude, Cursor, Copilot) canlı bir Chrome tarayıcısını yönetmesini ve incelemesini sağlar. Hem MCP sunucusu hem CLI olarak gelir. Üç ana işi var: performans trace'i kaydedip ondan uygulanabilir bulgular çıkarmak; network isteklerini, ekran görüntülerini ve console mesajlarını (source map'li stack trace ile) incelemek; puppeteer ile Chrome'da işlemleri otomatikleştirip sonucunu beklemek. Paketle birlikte agent'a DevTools kullanımını öğreten skill'ler de gelir (erişilebilirlik, cookie, LCP, bellek sızıntısı, bağlantı sorunları). README'nin uyarısı: araç tarayıcının içeriğini MCP client'ına açar; paylaşmak istemediğin kişisel ya da hassas bilgiyi o tarayıcıda açma. Lisansı Apache-2.0.

## Gereksinimler

- Node.js, LTS sürümü.
- Chrome, güncel stable sürüm ya da daha yenisi.
- npm.
- MCP destekleyen bir client (Antigravity, Claude Code, Cursor, VS Code gibi).
- Resmî destek yalnız Google Chrome ve Chrome for Testing içindir; öbür Chromium tabanlı tarayıcılar çalışabilir ama garanti edilmez.

## Kurulum

MCP client'ının ayarına şu yapılandırmayı ekle (README, "Getting started"):

```json
{
  "mcpServers": {
    "chrome-devtools": {
      "command": "npx",
      "args": ["-y", "chrome-devtools-mcp@latest"]
    }
  }
}
```

`chrome-devtools-mcp@latest` yazınca client her zaman son sürümü kullanır. Editörüne ya da agent'ına özel adımlar README'de değil, Client Configurations Guide belgesinde (`docs/client-configurations.md`).

Plugin olarak kurmak MCP sunucusunu ve bütün skill'leri birlikte getirir:

- Claude Code: `/plugin marketplace add ChromeDevTools/chrome-devtools-mcp`, ardından `/plugin install chrome-devtools-mcp@chrome-devtools-plugins`.
- Copilot / VS Code: **Chat: Install Plugin From Source** komutunu çalıştırıp `ChromeDevTools/chrome-devtools-mcp` yaz.
- Gemini CLI: `gemini extensions install --auto-update https://github.com/ChromeDevTools/chrome-devtools-mcp`.

Agent'ın plugin desteklemiyorsa skill klasörleri elle kopyalanır (skill'ler npm paketinin içindedir):

```sh
npm i chrome-devtools-mcp@latest -g
# Example for Claude Code. Use the skills directory of your agent.
mkdir -p ~/.claude/skills
cp -R "$(npm root -g)/chrome-devtools-mcp/skills/chrome-devtools-cli" ~/.claude/skills/
```

Kullanım istatistikleri varsayılan olarak açıktır; kapatmak için sunucuyu `--no-usage-statistics` ile başlat:

```json
"args": ["-y", "chrome-devtools-mcp@latest", "--no-usage-statistics"]
```

## İlk çalıştırma

Her şeyin çalıştığını görmek için MCP client'ına README'deki ilk prompt'u yaz:

```
Check the performance of https://developers.chrome.com
```

Client'ın tarayıcıyı açıp bir performans trace'i kaydetmesi gerekir. Tarayıcı, client tarayıcı gerektiren bir aracı ilk kullandığında kendiliğinden başlar; yalnız sunucuya bağlanmak tarayıcıyı açmaz.

MCP olmadan, terminalden denemek için paketin `chrome-devtools` CLI'ı kullanılır:

```sh
npm i chrome-devtools-mcp@latest -g
chrome-devtools navigate_page 1 --url "https://developers.chrome.com"
chrome-devtools take_screenshot 1 --filePath screenshot.png
chrome-devtools stop
```

## Sık takılınan yerler

- Sunucuya bağlandın ama tarayıcı açılmadıysa bu beklenen davranıştır; tarayıcı ilk araç çağrısında başlar (README'nin notu).
- `chrome-devtools-cli` skill'i CLI ister; skill'i kullanmadan önce paketi global kur.
- Skill'ler belirli bir sürümün araçlarını anlatır; MCP sunucusunu ya da CLI'ı her güncellediğinde skill'leri de güncelle. Eski skill, değişmiş araç ya da parametre önerebilir. Elle kurulumda paketi güncelleyip klasörleri yeniden kopyala.
- Performans araçları, gerçek kullanıcı verisi almak için trace adreslerini Google CrUX API'ına gönderebilir; kapatmak için `--no-performance-crux`.
- İstatistik toplama `CHROME_DEVTOOLS_MCP_NO_USAGE_STATISTICS` ya da `CI` ortam değişkeni tanımlıysa da kapalıdır; Chrome'un kendi istatistik ayarından bağımsızdır.
- Yalnız temel tarayıcı işleri için `--slim` daha az araç açar. `--headless`, `--isolated`, çalışan bir Chrome'a bağlanma ve `--executablePath` gibi konular README'de anlatılmıyor; Configuration Guide ve Advanced Usage Guide belgelerine yönlendiriliyor.
- Bağlantı sorunlarında README troubleshooting belgesini gösteriyor (`docs/troubleshooting.md`).

## Bağlantılar

- Repo: https://github.com/ChromeDevTools/chrome-devtools-mcp
- Ana sayfa: https://developer.chrome.com/docs/devtools/agents
- npm paketi: https://npmjs.org/package/chrome-devtools-mcp
- Client ayarları: https://github.com/ChromeDevTools/chrome-devtools-mcp/blob/main/docs/client-configurations.md
- Araç listesi: https://github.com/ChromeDevTools/chrome-devtools-mcp/blob/main/docs/tool-reference.md · CLI: https://github.com/ChromeDevTools/chrome-devtools-mcp/blob/main/docs/cli.md
- Sorun giderme: https://github.com/ChromeDevTools/chrome-devtools-mcp/blob/main/docs/troubleshooting.md
- Sürümler: https://github.com/ChromeDevTools/chrome-devtools-mcp/releases (son sürüm chrome-devtools-mcp-v1.13.0, 11 Kasım 2026)
- Issues: https://github.com/ChromeDevTools/chrome-devtools-mcp/issues
- Video: Haftanın Popüler Repoları, 16–22 Kasım 2026 bölümü

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W47 bölümü](../bolumler/2026-W47.md) videosu için hazırlandı (yakında — 23 Kas 12:30). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
