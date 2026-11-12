---
repo: homeassistant-ai/ha-mcp
url: https://github.com/homeassistant-ai/ha-mcp
lisans: MIT
surum: v9.2.0 (2026-11-11)
readme_tarihi: 2026-11-12
dogrulama: denenmedi
bolum: gunun-reposu/g03
ozet: Home Assistant'ı Claude, ChatGPT gibi yapay zekâ asistanlarıyla yapılandırıp otomatikleştiren 87 araçlı MCP sunucusu.
---

# ha-mcp — kurulum ve ilk örnek

> **Bu belge, README'nin 12 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

ha-mcp, Home Assistant'ı Claude Desktop, claude.ai, Claude Code, ChatGPT, Cursor gibi MCP destekli asistanlara açan bağımsız bir MCP sunucusu. Home Assistant'ın yerleşik MCP entegrasyonu yalnız Assist'e açılmış cihazları kontrol ederken ha-mcp 87 araçla otomasyon, script, sahne, helper ve dashboard oluşturur, trace ve log okuyarak çalışmayan otomasyonu ayıklar, yedek alır. Akıllı evini "verandadaki ışığı gün batımında yak" gibi cümlelerle kurmak ve bakımını yaptırmak isteyen Home Assistant kullanıcıları için.

## Gereksinimler

- Çalışan bir Home Assistant örneği. Önerilen yol (HA-MCP entegrasyonu) için **Home Assistant 2026.8.0 ya da üstü**; 2026.11+ sürümlerde Marketplace yerleşik, 2026.8–2026.10'da HACS gerekir. Core 2026.7 öncesinde entegrasyon hiç yüklenmez ([docs/in-process-server.md](https://github.com/homeassistant-ai/ha-mcp/blob/master/docs/in-process-server.md#requirements)).
- Entegrasyon yolu her kurulum türünde çalışır (OS, Supervised, Container, Core); app (add-on) yolu yalnız OS ve Supervised'da.
- Yönetici yetkisi: kendi hesabında long-lived access token ya da (daha güvenlisi) HA-MCP için ayrı bir yönetici hesabı.
- Bir MCP client: Claude Desktop (ücretsiz claude.ai hesabı yeter), Claude Code (ücretli plan), ChatGPT, Cursor, VS Code, Gemini CLI… (15+ client için [Setup Wizard](https://homeassistant-ai.github.io/ha-mcp/setup/)).
- Home Assistant dışında çalıştıracaksan Docker ya da Python 3.14 + `uvx`.

## Kurulum

README dört yol veriyor; **yalnız birini** kur ve client'ı tek bir URL'ye yönelt.

### 1. yol: HA-MCP entegrasyonu (önerilen; HACS ya da Marketplace)

README'deki adımlar (sürüm 9.0.0 ve sonrası için):

1. **HA-MCP** entegrasyonunu kur. 2026.8–2026.10: HACS'ta **HA-MCP** ara, **Download**. 2026.11+: **Settings → Marketplace**, ilk açılış uyarısını kabul et, **HA-MCP** ara, **Install**. Varsayılan katalogda olduğu için GitHub bağlamak ya da özel repo eklemek gerekmez. Kısayol: [HACS rozeti](https://my.home-assistant.io/redirect/hacs_repository/?owner=homeassistant-ai&repository=ha-mcp&category=integration).
2. **Home Assistant'ı yeniden başlat.**
3. Sunucunun hangi yönetici hesabı adına çalışacağına karar ver: profilinden **Security → Long-lived access tokens → Create token**, ya da **Settings → People → Add person** ile **Allow login**, **Local access only** ve **Administrator** açık ayrı bir hesap (kendi girişini bu hesap için kullanma).
4. **Settings → Devices & Services → Add Integration**, **HA-MCP** ara, **HA-MCP Server**'ı seç. Token'ı yapıştır (ya da ayrı hesabın kullanıcı adı/şifresini gir), client'ların nasıl bağlanacağını seç — **Remote access through Home Assistant** (**Secret URL** ile başla) ve/veya **Network access: Local network** — **Submit**. İkisi de kapalı başlar; başka cihazdaki bir client için birini açman gerekir.
5. Bağlantı URL'sini girdinin **Configure** ekranından kopyala (**Settings → Devices & Services → HA-MCP → HA-MCP Server → Configure**).
6. URL'yi yapay zekâ client'ına yapıştır.

HACS ya da Marketplace yoksa: bu repodaki `custom_components/ha_mcp_tools/` klasörünü Home Assistant'ın `config/custom_components/` dizinine kopyala, yeniden başlat, entegrasyonu yukarıdaki gibi ekle.

### 2. yol: Home Assistant app (add-on) — OS ve Supervised

1. Repoyu ekle: [Add Repository](https://my.home-assistant.io/redirect/supervisor_add_addon_repository/?repository_url=https%3A%2F%2Fgithub.com%2Fhomeassistant-ai%2Fha-mcp); diyalog açılmazsa **Settings → Apps → Install app → ⋮ → Repositories**'e `https://github.com/homeassistant-ai/ha-mcp` yapıştır.
2. **Settings → Apps → Install app**'ten **"Home Assistant MCP Server"**'ı kur, **Start**.
3. **Logs** sekmesindeki MCP URL'sini client'ına gir; token gerekmez.

### 3. yol: Home Assistant dışında (Docker ya da uvx) — Container ve Core kurulumları

README tam komutu [Setup Wizard](https://homeassistant-ai.github.io/ha-mcp/setup/)'a bırakıyor: `ghcr.io/homeassistant-ai/ha-mcp` imajını HTTP modunda, Home Assistant URL'si ve long-lived token ile çalıştırırsın; ya da PyPI paketini `uvx --python 3.14 ha-mcp@latest` ile. stdio taşıması önerilmiyor (aşağıya bak).

### Kendi Home Assistant'ın olmadan deneme (demo sunucu)

Betikler Claude Desktop'ı barındırılan bir demo ortamına stdio ile bağlar; ücretsiz claude.ai hesabı yeter. macOS'ta Terminal'de:

```sh
curl -LsSf https://raw.githubusercontent.com/homeassistant-ai/ha-mcp/master/scripts/install-macos.sh | sh
```

Windows PowerShell'de:

```powershell
irm https://raw.githubusercontent.com/homeassistant-ai/ha-mcp/master/scripts/install-windows.ps1 | iex
```

Linux'ta Claude Code ile:

```sh
curl -LsSf https://raw.githubusercontent.com/homeassistant-ai/ha-mcp/master/scripts/install.sh | sh -s -- --claude-code
```

## İlk çalıştırma

Bağlantı URL'si Configure ekranında iki biçimde gelir: dışarıdan erişim için `https://<ha-alan-adin>/api/webhook/<webhook-id>` (Nabu Casa ya da kendi reverse proxy'n; yerelde `http://<ha-host>:8123/api/webhook/<webhook-id>`), aynı ağdaki client'lar için `http://<ha-ip>:9584/private_<rastgele>`. İlk denemede URL'nin sonuna `/readonly` ekleyerek yalnız okuma modunda bağlanabilirsin; yazma araçları gizlenir.

Client'ta ilk soru README'nin önerisi:

```text
Can you see my Home Assistant?
```

Claude Code'daysan önce `/mcp` ile sunucunun bağlı göründüğünü doğrula. Asistan Home Assistant'taki entity'leri sayıp özetlerse bağlantı tamam; sonraki adım README'deki gibi bir otomasyon istemek: *"Create an automation that turns on the porch light at sunset"*. Sunucu çalışırken kenar çubuğunda yöneticiye özel **HA-MCP** paneli çıkar; araçları açıp kapatma, Read Only Mode ve yedekler burada.

## Sık takılınan yerler

- **Aynı sunucu için iki giriş bağlantıyı dondurur.** Bir client'ta hem yerel `uvx ... ha-mcp@latest` girdisi hem app/entegrasyon URL'si tutmak bilinen bir donma sebebi; tek yöntem seç ([README](https://github.com/homeassistant-ai/ha-mcp#-get-started)).
- **stdio taşımasında bağlantı sorunları var.** Yalnız demo/deneme için; gerçek kurulumda entegrasyon ya da HTTP yöntemlerini kullan ([#1713](https://github.com/homeassistant-ai/ha-mcp/issues/1713)).
- **Ayar değiştirdin, araçlar görünmüyor.** Client araç listesini önbellekte tutar; Home Assistant'ı yeniden başlatmak yetmez, client'ta sunucuyu yeniden bağla/yenile ve yeni sohbet aç ([README, Tool Discovery](https://github.com/homeassistant-ai/ha-mcp#-tool-discovery-for-ai-agents), [SSS](https://homeassistant-ai.github.io/ha-mcp/faq/#chatgpt-stale-tools)).
- **İlk açılış uzun sürüyor ya da sunucu başlamıyor.** Entegrasyon ilk yüklemede `ha-mcp` paketini indirir (bir iki dakika). Başlamazsa **Settings → Repairs**'te nedenini yazan bir onarım kaydı çıkar (ör. port çakışması: **Server port**'u değiştir, girdiyi yeniden yükle). Güncellemeden sonra da Home Assistant'ı yeniden başlatmak şart ([docs/in-process-server.md, Troubleshooting](https://github.com/homeassistant-ai/ha-mcp/blob/master/docs/in-process-server.md#troubleshooting)).
- **Hata bildirimi 24 saatte kapanıyor.** Issue'lara `ha_report_issue` aracının ürettiği rapor (ya da neden olmadığı) eklenmeli ([README, Community](https://github.com/homeassistant-ai/ha-mcp#-community)).

## Bağlantılar

- Repo: https://github.com/homeassistant-ai/ha-mcp
- Belgeler: https://homeassistant-ai.github.io/ha-mcp/ — Setup Wizard: https://homeassistant-ai.github.io/ha-mcp/setup/ — SSS: https://homeassistant-ai.github.io/ha-mcp/faq/
- Sürümler: https://github.com/homeassistant-ai/ha-mcp/releases
- Lisans: MIT — https://github.com/homeassistant-ai/ha-mcp/blob/master/LICENSE

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [Günün Reposu g03](../bolumler/gunun-reposu-g03.md) videosu için hazırlandı (yakında — 13 Kas 12:30). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
