---
repo: CopilotKit/OpenBot
url: https://github.com/CopilotKit/OpenBot
lisans: MIT
surum: v1.0.3 (2026-11-14)
readme_tarihi: 2026-11-16
dogrulama: denenmedi
bolum: 2026-W46
ozet: Her birine kendi tarayıcısı, dosyaları ve araçları verilen, her adımı onaya bağlanıp kaydedilen AI çalışma arkadaşları için kendi sunucunda çalışan açık kaynak çalışma alanı.
---

# OpenBot — kurulum ve ilk örnek

> **Bu belge, README'nin 16 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

OpenBot, AG-UI'nin yapımcısı CopilotKit'in açık kaynak "AI coworker" çalışma alanı. Her Bot'un kendi bilgisayarı var: bir tarayıcı, dosyalar ve araçlar. Bot bir işi tarayıcıda yapar, sonucu etkileşimli olarak gösterir ve göndermeden önce senin onayını bekler; gerektiğinde işi devralırsın. Her eylem önceden kurallara (boundaries) göre değerlendirilir ve sonradan denetim kaydına (audit) yazılır. Modelini seçersin (OpenAI, Anthropic, Google), zaten çalıştırdığın AG-UI agent'larını bağlarsın, altyapı senin olur. Lisansı MIT.

## Gereksinimler

En kolay yol olan masaüstü uygulaması için terminal ya da geliştirici aracı gerekmez (README). Kaynaktan çalıştırmak için README'nin listesi:

- Docker (PostgreSQL, API sunucusu ve Bot'ların bilgisayarları için; imajlar hazır çekilir, bir şey derlenmez).
- Bun 1.4 ya da üstü (bazı testler 1.3'te düşer).
- Bir CopilotKit hesabı (CopilotKit Intelligence için; ücretsiz plan yerel kurulumu ve yönetilen projeyi karşılıyor).
- Bir model anahtarı: örnek ayar OpenAI kullanır; Anthropic ve Google da `BOT_PROVIDER` ile çalışır.
- Windows'ta bütün adımlar WSL 2 içindeki Ubuntu'da yapılır.

Kubernetes yolu için amd64 node'lu bir cluster, varsayılan StorageClass, `kubectl`, Helm, deneme ya da üretim lisansı ve model anahtarı gerekir.

## Kurulum

Masaüstü uygulaması (önyüz ve Docker tabanlı arka ucu tek uygulamada birleştirir): README'deki bağlantılardan indir.

- Mac / Apple silicon: https://downloads.copilotkit.ai/latest/OpenBot-mac-arm64.dmg
- Mac / Intel: https://downloads.copilotkit.ai/latest/OpenBot-mac-x64.dmg
- Windows: https://downloads.copilotkit.ai/latest/OpenBot-windows-x64-setup.exe

Kaynaktan çalıştırma (workspace'i, markayı, Bot'ları ve skill'leri özelleştirmek için):

```sh
git clone https://github.com/CopilotKit/OpenBot.git
cd OpenBot
```

1. `.env` dosyasını oluştur ve bağımlılıkları kur:

```sh
cp .env.example .env
bun install
```

2. Intelligence'ı bağla. Varsayılan yol bu makinede (README'ye göre Docker Desktop'lı macOS için bir önizleme):

```sh
npx --yes copilotkit@latest login
npx --yes copilotkit@latest local setup
npx --yes copilotkit@latest local connect --approve-connection
```

   `local connect` iki adresi ve proje anahtarını `.env` dosyasına kendisi yazar. Yerel kurulum olmuyorsa yönetilen proje yolu README'nin "Quick start" 2. adımında anlatılıyor.
3. `.env` içinde kalan zorunlu değeri doldur: `OPENAI_API_KEY=<API_ANAHTARI>`, ya da Anthropic için `ANTHROPIC_API_KEY` ve `BOT_PROVIDER=anthropic`. Örnekteki `KEY_ENCRYPTION_KEY` herkese açıktır, yalnız yerel deneme için uygundur; kendininkini üretmek için:

```sh
openssl rand -base64 32
```

Tek konteyner yolu (klonlama ve derleme yok, aynı `.env` ile; uygulama 3001 portunda):

```sh
# The published image. Nothing to clone and nothing to build.
docker run -p 3001:3001 --env-file .env \
  -e EMBEDDED_POSTGRES=on -v openbot-data:/var/lib/postgresql \
  ghcr.io/copilotkit/openbot:latest
```

Kubernetes (EKS, AKS, GKE ya da kendi cluster'ın) için Helm chart'ı ve `openbot-values.yaml` örneği README'nin "Deploy on Kubernetes" bölümünde.

## İlk çalıştırma

1. Kaynaktan kurulumda başlat:

```sh
bash scripts/start.sh
```

   Betik imajları çeker, Docker servislerini, 3001 portunda API sunucusunu ve 3010 portunda uygulamayı başlatır. Durdurmak için `scripts/stop.sh` (veriler silinmez).
2. Tarayıcıda http://localhost:3010 adresini aç (tek konteyner yolunda 3001).
3. README'nin "Try it" bölümündeki ilk denemeler:
   - `/bot` sayfasını aç ve şunu sor: `Open news.ycombinator.com and tell me the top story.`
   - Bot'tan https://httpbin.org/forms/post formunu doldurmasını iste, sonra `/admin/audit` sayfasında kaydı incele.
   - `/admin/boundaries` sayfasında bir yasak kuralı ekle ve aynı tarayıcı işlemini tekrar dene.
   - `/bots` sayfasından bir Bot oluştur, ona kalıcı bir rol ver ve onunla bir kanal başlat.

## Sık takılınan yerler

- Örnek ayar `OPENBOT_SINGLE_USER=true` ile gelir: erişebilen herkes yönetici olur. Başkasına açmadan önce sign-in ayarla; Kubernetes denemesinde port-forward'u yerel tut (README).
- API sunucusu `DATABASE_URL`, `KEY_ENCRYPTION_KEY`, `INTELLIGENCE_API_URL`, `INTELLIGENCE_GATEWAY_WS_URL` ve `CPK_INTELLIGENCE_API_KEY` olmadan başlamaz (README'nin Configuration bölümü).
- Yerel Intelligence lisansı 30 gün sürer; `npx copilotkit@latest local renew` ile yenilenir. `local connect` komutunu `.env` dosyasının olduğu OpenBot kök klasöründe çalıştır (README).
- Tarayıcısı olmayan makinede (ör. SSH) giriş için `npx --yes copilotkit@latest login --device` (README).
- Composio hazırlığı tamamlanamazsa OpenBot yine açılır ama uygulamalar (apps) kullanılamaz; `bun run setup:composio` ile yeniden dene (README).
- Windows'ta uygulamayı Windows tarayıcısından http://127.0.0.1:3010 ile aç ve kullanırken Ubuntu terminalini açık tut (README).

## Bağlantılar

- Repo: https://github.com/CopilotKit/OpenBot
- Site: https://open.bot
- Sürümler: https://github.com/CopilotKit/OpenBot/releases (son sürüm v1.0.3, 14 Kasım 2026)
- Belgeler (repoda): docs/configuration.md, docs/deployment.md, docs/development.md (Windows WSL 2), docs/automatic-learning.md
- Issues: https://github.com/CopilotKit/OpenBot/issues
- Video: Haftanın Popüler Repoları, 9–15 Kasım 2026 bölümü (listenin 10. sırası)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W46 bölümü](../bolumler/2026-W46.md) videosu için hazırlandı: https://www.youtube.com/watch?v=UADSMY2Z4eY. Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
