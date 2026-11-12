---
repo: NVIDIA/auto-ontology
url: https://github.com/NVIDIA/auto-ontology
lisans: Apache-2.0
surum: sürüm etiketi yok
readme_tarihi: 2026-11-12
dogrulama: denenmedi
bolum: 2026-W45
ozet: NVIDIA'nın kurumsal metadata'dan ontoloji üretip agent'lara MCP, CLI ve API ile doğrulanmış text-to-SQL sunan Python çatısı.
---

# auto-ontology — kurulum ve ilk örnek

> **Bu belge, README'nin 12 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

Agent'ların veritabanına doğru soru sorabilmesi için bir bağlam katmanı kurar: bağlı veritabanlarının metadata'sından iş terimleri (Customer, Invoice Line…), tablolar arası ilişkiler ve onaylı SQL öznitelikleri üretir; insanlar bunları düzeltip sertifikalar; agent her soruya ürettiği SQL ve izlediği adımlarla birlikte yanıt verir, yanıtlayamadığını söyler. Veri ekipleri ve "veriyle konuşan agent" kuran geliştiriciler için. Postgres + pgvector üstünde Next.js web arayüzü, FastAPI backend, ingestion servisi ve salt okunur bir MCP server'dan oluşur; Docker Compose ya da Helm ile kurulur. Proje dışarıdan katkı (PR) kabul etmiyor; hata ve entegrasyon istekleri issue olarak alınıyor.

## Gereksinimler

- Docker ve Compose v2 (Quickstart yolu). Kubernetes için Helm chart ayrı ([DEPLOYMENT.md](https://github.com/NVIDIA/auto-ontology/blob/main/DEPLOYMENT.md)).
- [uv](https://docs.astral.sh/uv/): örnek veritabanını yükleyen seed betiği için.
- git.
- NVIDIA API anahtarı (`nvapi-` ile başlar), [build.nvidia.com](https://build.nvidia.com) üzerinden; `.env` içinde `DEFAULT_MODELS_API_KEY`. Kendi NIM'in ya da OpenAI uyumlu bir endpoint de olur (`<ROL>_ENDPOINT`, `<ROL>_API_KEY`, `<ROL>_MODEL`).
- Boş portlar: 3000 (web), 3001 (backend), 3002 (ingestion), 3003 (MCP), 5432 (Postgres).
- İşletim sistemi: README belirtmiyor; Docker'ın çalıştığı her yer. Konteyner dışında çalıştırmak için bileşen tablosu Python 3.12+ ve Next.js 16 diyor; Quickstart'ta gerekmiyor.
- İsteğe bağlı: HashiCorp Vault (bağlantı parolaları için), `KUMO_RFM_API_KEY` (tahmin soruları; tabloları Kumo'ya yükler, önce veri incelemesi yap).

## Kurulum

README'nin Quickstart'ı Microsoft'un örnek satış veritabanı Wide World Importers ile çalışır.

1. Repoyu klonla ve `.env` oluştur:

   ```bash
   git clone https://github.com/NVIDIA/auto-ontology.git && cd auto-ontology
   cp .env.example .env
   ```

   `.env` içinde şu değerleri ayarla (README'deki alanlar; anahtar ve parolaların yerine kendi değerlerini yaz):

   ```dotenv
   DEFAULT_MODELS_API_KEY=<NVIDIA_API_ANAHTARI>
   AUTH_SECRET=<uzun rastgele bir dizi, ör. openssl rand -base64 32 çıktısı>
   APP_URL=http://localhost:3000
   POSTGRES_PASSWORD=<yeni bir parola>
   CONNECTION_STRINGS=postgresql://postgres:<aynı parola>@postgres:5432/wideworldimporters
   AUTO_ONTOLOGY_ADMIN_PASSWORD=<yeni bir parola>
   PGADMIN_PASSWORD=<yeni bir parola>
   ```

   Compose, Postgres'i, pgAdmin'i ve web uygulamasını host'a açar; bu yüzden varsayılan parolaların hepsini başlatmadan önce değiştir. `.env.example` her model rolü için ayrı bir anahtar satırı da içerir: ya hepsine kendi anahtarını yaz ya da o satırları sil ki `DEFAULT_MODELS_API_KEY` değerine düşsünler.

2. Postgres'i başlat ve örnek veritabanını yükle (seed betiği `auto-ontology-eval` reposunda; az önce seçtiğin Postgres parolasını ver):

   ```bash
   docker compose up -d --wait postgres
   git clone https://github.com/NVIDIA/auto-ontology-eval.git ../auto-ontology-eval
   (cd ../auto-ontology-eval && uv sync && cp .env.example .env && \
     POSTGRES_PASSWORD='<the same password>' \
     uv run python scripts/seed/seed_wwi.py --database-name wideworldimporters --drop)
   ```

3. Yığının kalanını derleyip başlat:

   ```bash
   docker compose up -d --build
   ```

## İlk çalıştırma

1. <http://localhost:3000> adresini aç; `.env` içindeki `AUTO_ONTOLOGY_ADMIN_EMAIL` ve `AUTO_ONTOLOGY_ADMIN_PASSWORD` ile giriş yap.
2. **Settings → Semantic Compilation** bölümünü aç. Ingestion başlangıçta çalışır; derleme hemen başlar ve bitince **Semantic compilation is ready** der. README'nin örneğinde bir derleme 31 terim üretiyor.
3. **Chat** ekranında README'deki soruyu sor:

   > What was revenue for the USB food flash drive - sushi roll in Pennsylvania in June 2015?

   Beklenen: yanıt (README'de 588,80 $, 3 fatura satırı), üretilen SQL ve adımlar (çözümlenen terimler, kurulan join'ler, doğrulanan filtreler).
4. Verinin yanıtlayamayacağı bir soru sor (ör. reklam harcaması getirisi); agent veride reklam verisi olmadığını söyleyip netleştirme ister.

MCP üzerinden kendi agent'ına bağlamak için (web uygulaması ayakta olmalı):

```bash
AUTO_ONTOLOGY_API_URL=http://localhost:3000 \
  uvx --from "git+https://github.com/NVIDIA/auto-ontology.git#subdirectory=mcp" auto-ontology-mcp
```

Cursor için `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "auto-ontology": { "url": "http://localhost:3003/mcp" }
  }
}
```

Claude Desktop için README `mcp-remote` köprüsünü veriyor. Yapılandırmayı değiştirdikten sonra istemciyi yeniden başlat; istediğinde giriş yap. Ayrıntı: [docs/mcp.md](https://github.com/NVIDIA/auto-ontology/blob/main/docs/mcp.md).

## Sık takılınan yerler

- **Hosted NVIDIA API ile SQL üretimi başarısız.** Açık issue (6 Kasım 2026, başka bir kullanıcı da doğruladı): `integrate.api.nvidia.com` yapılandırılmış çıktı isteğini `[400] unknown field guided_json` ile reddediyor, SQL üretimi `None` dönüyor ve Quickstart sorusu tam yanıt alamıyor. Kaynak: [#440](https://github.com/NVIDIA/auto-ontology/issues/440).
- **Arayüz sol menüsüz açılıyor ya da "default secret" hatası.** Eski oturum çerezi (`AUTH_SECRET` değişince ya da veritabanı sıfırlanınca) ya da `.env` içinde `AUTH_SECRET` / `APP_URL` eksik; ikisi de zorunlu. `localhost:3000` için site verisini temizleyip yeniden giriş yap. Kaynak: [troubleshooting.md](https://github.com/NVIDIA/auto-ontology/blob/main/skills/nvidia-ontology-setup/references/troubleshooting.md).
- **Chat/ingest çalışmıyor, embedding'ler boş.** `DEFAULT_MODELS_API_KEY` eksik; ayrıca `EMBED_ENDPOINT` ve `EMBED_MODEL` ingestion ile sorgu tarafında aynı olmalı, yoksa "hiçbir şey bulunamadı" gibi yanıtlar gelir. Kaynak: aynı troubleshooting belgesi.
- **`POSTGRES_PORT` konteyner içinde bir şey değiştirmiyor.** Yalnız host tarafındaki portu değiştirir; konteynerler her zaman 5432'ye gider. Kaynak: aynı troubleshooting belgesi.
- **3001 ve 3002 portlarını dışarı açma.** FastAPI, frontend'in ilettiği kullanıcı kimliğine güvenir; ingestion servisinin uç noktalarının kendi kimlik doğrulaması yok. İstemciler 3000 üzerinden geçmeli (README, bileşen tablosu).

## Bağlantılar

- Repo: https://github.com/NVIDIA/auto-ontology
- Belgeler: repodaki `docs/` klasörü: [docs/mcp.md](https://github.com/NVIDIA/auto-ontology/blob/main/docs/mcp.md), [docs/connections.md](https://github.com/NVIDIA/auto-ontology/blob/main/docs/connections.md), [docs/authentication.md](https://github.com/NVIDIA/auto-ontology/blob/main/docs/authentication.md), [DEPLOYMENT.md](https://github.com/NVIDIA/auto-ontology/blob/main/DEPLOYMENT.md) (Helm); agent skill'leri `skills/` altında, giriş [AGENTS.md](https://github.com/NVIDIA/auto-ontology/blob/main/AGENTS.md)
- Sürümler: https://github.com/NVIDIA/auto-ontology/releases (sürüm etiketi yok; `main` dalı kullanılıyor)
- Lisans: https://github.com/NVIDIA/auto-ontology/blob/main/LICENSE (Apache-2.0; üçüncü taraf bileşenler `THIRD_PARTY_NOTICES.md` içinde)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W45 bölümü](../bolumler/2026-W45.md) videosu için hazırlandı: https://www.youtube.com/watch?v=AkiRwaGZDZA. Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
