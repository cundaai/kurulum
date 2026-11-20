---
repo: openai/codex-security
url: https://github.com/openai/codex-security
lisans: Apache-2.0
surum: npm-v0.10.0 (2026-11-12)
readme_tarihi: 2026-11-11
dogrulama: denenmedi
bolum: trend-short/t2026-11-14-1
ozet: Kodundaki güvenlik açıklarını bulan, doğrulayan ve düzelten OpenAI'ın CLI ve TypeScript SDK'sı.
---

# codex-security — kurulum ve ilk örnek

> **Bu belge, README'nin 11 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

Codex Security, kodundaki güvenlik açıklarını bulmana, incelemene ve düzeltmene yardım eder. README'ye göre üç yolla kullanılır: Codex içinde bir güvenlik workflow'u seçerek, terminalde `@openai/codex-security` CLI'ı ile repoyu tarayarak ya da otomasyonda TypeScript SDK veya GitHub Action ile. Tüm repoyu, seçtiğin klasörleri ya da Git değişikliklerini tarar; Deep scan, Standard taramayı tekrarlayıp tekilleştirilmiş bulguları döndürür. Aday bulguları doğrular, patch üretir, mevcut düzeltmeleri kontrol eder; SECURITY.md taslağı yazar ve threat model kaydeder. Bulguları Linear, Jira ya da GitHub'a aktarabilir, SARIF, JSON ve CSV olarak dışa verebilir; CI ve container içinde de çalışır. Lisansı Apache-2.0.

## Gereksinimler

- Node.js: 22.x içinde 22.13.0 ve üstü, ya da Node.js 24.x veya 26.x.
- Python 3.10 ve üstü. Python 3.10 kullanıyorsan `tomli` paketi de gerekir.
- `cs login` ile oturum açılacak bir hesap. CI için tarama sürecinin ortamında `OPENAI_API_KEY` ya da `CODEX_API_KEY` tanımlı olmalı (README).
- Bazı siber güvenlik istekleri ve korumalı bulgular için Trusted Access for Cyber erişimi gerekir (README).

## Kurulum

CLI'ı global olarak kur ve oturum aç (README'den olduğu gibi):

```bash
npm install --global @openai/codex-security
cs login
```

`cs` ve `codex-security` aynı CLI'dır; kısa ad başka bir komutla çakışırsa `codex-security` yaz. Global kurulum istemiyorsan ya da npm `EEXIST` çakışması verirse, hem oturum açma hem tarama için `cs` yerine `npx @openai/codex-security` kullan.

CI'da anahtarı ortam değişkeni olarak ver, örneğin `OPENAI_API_KEY=<API_ANAHTARI>`. Ekransız (headless) makinede oturum açmak için README'nin bağlantı verdiği device ya da SSH login belgesine bak.

## İlk çalıştırma

1. Taramak istediğin reponun klasörüne gir ve ilk taramayı başlat:

```bash
cs scan .
```

2. Tarama bitince terminaldeki **REPORT** yolundaki Markdown dosyasını aç. Aynı repodan son tamamlanan taramayı yeniden bulmak için `cs scans show` çalıştır.
3. Kapsamı daraltmak ya da derin tarama için README'deki seçenekler:

```bash
# Scan selected paths.
cs scan . --path src --path tests

# Scan committed changes from a base revision to HEAD.
cs scan . --diff origin/main

# Run a deep scan of the repository.
cs scan . --mode deep
```

Standard tarama için tavsiye niteliğinde 15 dakikalık süre: `cs scan . --max-time 15`. Komutları görmek için `cs --help`, tarama seçenekleri, maliyet sınırları ve tarama sonrası patch için `cs scan --help`.

## Sık takılınan yerler

- `cs` komutu başka bir araçla çakışıyorsa `codex-security` adını kullan; npm `EEXIST` hatasında `npx @openai/codex-security` yolu README'de önerilen çözüm.
- Rapordaki hariç tutulanları (exclusions) yorumlamak için README'nin bağlantı verdiği "scan scope and follow-up work" bölümüne bak.
- Taramalar repodaki kök ve bileşen `SECURITY.md` yönergelerini kullanır; yoksa README'deki "Generate SECURITY.md" bölümü bunu yazmayı anlatıyor.
- Birden çok repoya yayılan bir uygulama için "review one system across repositories", birbirinden bağımsız çok sayıda repo için "bulk scans" belgesi var (sdk/typescript/docs/cli.md).

## Bağlantılar

- Repo: https://github.com/openai/codex-security
- npm paketi: https://www.npmjs.com/package/@openai/codex-security
- Belgeler: https://developers.openai.com/codex/security
- CLI belgesi: https://github.com/openai/codex-security/blob/main/sdk/typescript/docs/cli.md
- Sürümler: https://github.com/openai/codex-security/releases (son sürüm npm-v0.10.0, 12 Kasım 2026)
- Issues: https://github.com/openai/codex-security/issues
- Video: Haftanın Popüler Repoları, 14 Kasım 2026 trend Short'u

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki trend Short t2026-11-14-1 videosu için hazırlandı: https://www.youtube.com/shorts/DDA0Nped1yg. Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
