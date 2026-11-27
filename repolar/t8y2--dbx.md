---
repo: t8y2/dbx
url: https://github.com/t8y2/dbx
lisans: Apache-2.0
surum: v0.6.75 (2026-11-26)
readme_tarihi: 2026-11-26
dogrulama: denenmedi
bolum: 2026-W48-3
ozet: MySQL, PostgreSQL, Redis, MongoDB dahil 100'den fazla veritabanını tek 25 MB'lık uygulamadan yöneten, AI asistanlı ve MCP sunuculu açık kaynak veritabanı istemcisi.
---

# dbx — kurulum ve ilk örnek

> **Bu belge, README'nin 26 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

DBX, Rust ve Tauri ile yazılmış hafif, çok platformlu bir veritabanı istemcisi: README'ye göre 25 MB, Java ya da gömülü Chromium gerektirmiyor. MySQL, PostgreSQL, SQLite, Redis, MongoDB, DuckDB, ClickHouse, SQL Server, Oracle, Elasticsearch ve daha birçok olmak üzere 100'den fazla veritabanını destekler; Kafka, RabbitMQ, MQTT gibi mesaj kuyruklarına ve Nacos, Consul, etcd gibi ara katmanlara da konsol sunar.

Öne çıkanlar: CodeMirror 6 tabanlı sorgu editörü, düz dille SQL yazdıran AI asistanı (Claude, OpenAI, Ollama ile yerel modeller ya da OpenAI uyumlu her uç nokta; AI'ın ürettiği SQL çalışmadan önce güvenlik denetiminden geçer), ER diyagramı, şema karşılaştırma, veri aktarımı, DBeaver ya da Navicat'ten bağlantı içe aktarma, SSH tüneli. Ayrı bir MCP sunucusuyla Claude Code, Cursor gibi coding agent'lar DBX'te kurduğun bağlantılar üzerinden veritabanını sorgulayabilir. Masaüstü (macOS, Windows, Linux), Docker ile web sürümü ve CLI olarak gelir. Lisansı Apache-2.0.

## Gereksinimler

- Masaüstü uygulaması: macOS, Windows ya da Linux; ek çalışma zamanı gerekmiyor.
- Web sürümü: Docker (amd64 ya da arm64 imajı).
- MCP sunucusu ve CLI: npm yolu için Node.js; README'ye göre Node.js gerektirmeyen hazır dosyalar da Releases sayfasında var.
- AI asistanı için kendi seçtiğin bir sağlayıcı (Claude, OpenAI ya da yerel model).
- Yalnız kaynaktan geliştirme için: Node.js >= 22.13.0, pnpm 10.34.7, Rust >= 1.88; Linux'ta ek sistem paketleri.

## Kurulum

En son sürümü [Releases](https://github.com/t8y2/dbx/releases/latest) sayfasından indir ya da paket yöneticisiyle kur.

macOS (Homebrew):

```bash
brew install --cask dbx
```

Windows (Scoop):

```bash
scoop bucket add dbx https://github.com/t8y2/scoop-bucket
scoop install dbx
```

Windows (WinGet):

```
winget install t8y2.dbx
```

Linux (Flatpak):

```bash
flatpak remote-add --if-not-exists flatpark https://dl.flatpark.org/flatpark.flatpakrepo
flatpak install flatpark com.dbxio.dbx
```

Web sürümü, Docker ile (bağlantı bilgilerini şifreleyen anahtar kalıcı `/app/data` biriminde tutulur):

```bash
# The default keeps the key in the persistent /app/data volume.
docker run -d --pull=always --name dbx -p 4224:4224 \
  -v dbx-data:/app/data \
  t8y2/dbx:latest
```

Docker Compose ile yayınlanmış imajı kurmak için:

```bash
docker compose -f deploy/docker-compose.release.yml up -d
```

Coding agent'lar için MCP sunucusu (masaüstü uygulamasıyla birlikte gelmez, ayrı kurulur):

```bash
npx @dbx-app/mcp-server
```

Terminal ve betikler için CLI:

```bash
npm install -g @dbx-app/cli
# or via Homebrew
brew tap t8y2/tap && brew install dbx-cli
dbx agent setup
dbx connections list --json
dbx query local "select 1" --json
```

## İlk çalıştırma

1. Masaüstü uygulamasını aç ve bir bağlantı ekle; README bağlantı ekleme ekranını adım adım anlatmıyor, ayrıntı resmî belgelerde: https://dbxio.com/en/docs/what-is-dbx
2. Docker ile kurduysan tarayıcıda `http://localhost:4224` adresini aç.
3. Bir coding agent'a veritabanını açmak için projendeki `.mcp.json` dosyasına ekle:

```json
{
  "mcpServers": {
    "dbx": { "command": "npx", "args": ["-y", "@dbx-app/mcp-server"] }
  }
}
```

   Agent'ın hangi bağlantılara, hangi yetkiyle (**Read only**, **Data read/write**, **Full access**) erişeceğini **DBX Settings → MCP** ekranından ayarla. Web ya da Docker kurulumunda MCP sunucusuna `DBX_WEB_URL` (örneğin `http://localhost:4224`) ve web girişi parolalıysa `DBX_WEB_PASSWORD` ortam değişkenini (değeri `<WEB_PAROLASI>`: web giriş ekranındaki parolanın aynısı) ver; README'deki ikinci `.mcp.json` örneği bunu gösteriyor.
4. CLI ile hızlı deneme: `dbx connections list --json` bağlantıları listeler, `dbx query local "select 1" --json` `local` adlı bağlantıda sorgu çalıştırır. `dbx agent setup` resmî DBX Agent Skill'ini `~/.agents/skills/dbx` altına kurar.

## Sık takılınan yerler

- Bağlantı, eklenti, AI ve tünel kimlik bilgileri `dbx.db` dosyasına şifreli yazılır. Docker ve web kurulumunda anahtar `${DBX_DATA_DIR}/.dbx/secret.key` dosyasındadır ve `dbx.db` ile birlikte yedeklenmelidir; anahtar kaybolursa şifreli bilgiler açılmaz. Üretim kurulumunda README anahtarı `DBX_SECRET_KEY_FILE` ya da `DBX_SECRET_KEY` ile bir secret yöneticisinden vermeyi öneriyor.
- Düz metin kimlik bilgisi saklayan eski bir sürümden geçiyorsan uygulama önce **Data Security Upgrade** sihirbazını açar; CLI ya da MCP `DATA_MIGRATION_REQUIRED` hatası verirse önce aynı veri klasörünü masaüstü ya da web sürümünde açıp yükseltmeyi tamamla.
- Windows taşınabilir sürümünde MCP yapılandırmasına `DBX_DATA_DIR` (`DBX.exe` yanındaki `data` klasörü) eklenmeli; bu sürümde anahtar `data/.dbx/secret.key` içinde durduğu için klasörü bütün olarak taşı ve yedekle.
- Kaynaktan derlerken DuckDB derlemesi uzun sürer; DuckDB üzerinde çalışmıyorsan `make dev-fast`, `make cargo-check-fast` kullan (README'deki ipucu).
- Açık issue sayısı yüksek (yaklaşık 1.900); hata bildirmeden önce benzerini ara.

## Bağlantılar

- Repo: https://github.com/t8y2/dbx
- Site ve belgeler: https://dbxio.com · https://dbxio.com/en/docs/what-is-dbx
- MCP sunucusu: packages/mcp-server/README.md · CLI: packages/cli/README.md · örnekler: examples/
- Arka planda yedekleme: docs/background-database-backups.md · veri güvenliği yükseltmesi: docs/data-security-migration.md
- Sürümler: https://github.com/t8y2/dbx/releases (son sürüm v0.6.75, 26 Kasım 2026)
- Issues: https://github.com/t8y2/dbx/issues · Discord: https://discord.gg/W7NyVDRt6a
- Video: Haftanın Popüler Repoları, 25–27 Kasım 2026 Cuma bölümü (listenin 4. sırası)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W48-3 bölümü](../bolumler/2026-W48-3.md) videosu için hazırlandı (yakında — 27 Kas 12:30). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
