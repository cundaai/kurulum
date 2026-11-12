---
repo: github/spec-kit
url: https://github.com/github/spec-kit
lisans: MIT
surum: v1.4.0 (2026-11-10)
readme_tarihi: 2026-11-12
dogrulama: denenmedi
bolum: 2026-W45
ozet: Coding agent'lara spec'ten plan, görev ve koda giden yapılı süreçler, şablonlar ve belgelenmiş çıktılar veren GitHub araç seti.
---

# spec-kit — kurulum ve ilk örnek

> **Bu belge, README'nin 12 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

GitHub'ın açık kaynak araç seti: AI coding agent'ına yapılı süreçler, yeniden kullanılabilir şablonlar ve belgelenmiş çıktılar verir. Üç hazır giriş var: **Spec-Driven Development** (ne yapılacağını ve nedenini yazıp bunu plana, görevlere ve çalışan koda çevirme; çekirdekte gelir), **bug fixing** (nedeni değerlendir, kapsamlı düzelt, doğrula) ve **idea assessment** (bir fikre yatırım yapılıp yapılmayacağına kanıtla karar); son ikisi gerektiğinde kurulan extension'lar. Preset, extension, workflow ve bundle ile kendi sürecini de kurabilirsin. `specify` CLI projeyi kurar; asıl iş agent'ın sohbetinde `/speckit-*` skill'leriyle yapılır. Copilot, Claude Code, Gemini CLI, Codex, Cursor ve daha birçok agent için entegrasyon var.

## Gereksinimler

- Linux, macOS ya da Windows (PowerShell betikleri WSL olmadan destekleniyor).
- Python 3.11+.
- [uv](https://docs.astral.sh/uv/) (önerilen) ya da pipx / pip.
- Desteklenen bir AI coding agent ve onun hesabı; entegrasyon anahtarları (`copilot`, `claude`, `gemini`, `codebuddy`, `pi`, `omp`…) [belgelerde](https://github.github.io/spec-kit/integrations/index.html). Agent'sız iskelet için `none`.
- Git: isteğe bağlı; yalnız git extension'ı açıkken gerekir.

## Kurulum

README'nin en kısa yolu (örnek Copilot ile; Claude Code için `--integration claude`):

```bash
uv tool install specify-cli
specify init my-project --integration copilot
cd my-project
```

Kurulum belgesinin önerdiği yol, bir release etiketine sabitlenmiş kaynak kurulumu (`vX.Y.Z` yerine [Releases](https://github.com/github/spec-kit/releases) sayfasındaki etiketi, başındaki `v` ile yaz):

```bash
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git@vX.Y.Z
```

PyPI üzerinden alternatifler:

```bash
# Using uv (recommended)
uv tool install specify-cli

# Or using pipx
pipx install specify-cli

# Or using pip
pip install specify-cli
```

Kurulumu doğrula:

```bash
specify version
```

Mevcut bir projeye kurmak için (önce yerel değişiklikleri commit'le ya da yedekle):

```bash
specify init --here --force --non-interactive --integration claude
```

Betikler seçilen türe göre `.specify/scripts/bash/`, `.specify/scripts/powershell/` ya da `.specify/scripts/python/` altına kurulur (`--script sh|ps|py`).

## İlk çalıştırma

Proje klasöründe coding agent'ını başlat ve şunları **agent'ın sohbetinde**, terminalde değil, teker teker çalıştır; her sonucu inceleyip sonrakine geç. (Copilot'un skill sözdizimi; Claude Code ve diğerleri için [çağırma sözdizimi](https://github.github.io/spec-kit/integrations/cli.html#command-invocation).) Constitution projede bir kez yazılır, kalan adımlar her özellik için tekrarlanır:

```text
/speckit-constitution Create principles focused on code quality, testing, and maintainability.
/speckit-specify Build a photo organizer with albums grouped by date and a tile preview of each album.
/speckit-plan Use Vite with vanilla JavaScript. Keep images local and store metadata in SQLite.
/speckit-tasks
/speckit-implement
/speckit-converge
```

`implement` ve `converge` adımlarını convergence `Converged` deyinceye kadar tekrarla. Netleştirme (`/speckit-clarify`), checklist ve tutarlılık analizi (`/speckit-analyze`) SDD walkthrough'da.

Hata düzeltme için proje klasöründe, terminalde:

```bash
specify extension add bug
```

Sonra sohbette:

```text
/speckit-bug-assess "Submitting an empty password crashes the login form." slug=login-crash
/speckit-bug-fix slug=login-crash
/speckit-bug-test slug=login-crash
```

Raporlar `.specify/bugs/login-crash/` altında; son karar `verified`, `partial` ya da `failed`.

## Sık takılınan yerler

- **`command not found: uv`.** Önce uv'yi kur: [install/uv](https://github.github.io/spec-kit/install/uv.html). Kaynak: kurulum belgesi.
- **Eski `specify-cli` yerinde kalıyor.** `uv tool install specify-cli` kurulu sürümü korur; eski CLI ile `specify init --integration ...` `No such option: --integration` der. Aynı araçla zorla yenile: `uv tool install --force specify-cli`, `pipx install --force specify-cli` ya da `pip install --upgrade specify-cli`. Farklı bir araçla kurarsan ikinci kopya oluşur ve eski `specify` PATH'te önde kalabilir. Kaynak: [kurulum belgesi](https://github.github.io/spec-kit/installation.html).
- **Skill'ler terminalde çalışmıyor.** `/speckit-*` komutları agent'ın sohbetinde çalışır, kabukta değil. Kaynak: README.
- **Etkileşimsiz kurulum Copilot'u seçiyor.** CI ya da pipe'lı çalışmada `--integration` verilmezse varsayılan GitHub Copilot. Kaynak: kurulum belgesi.
- **Windows'ta betik türü.** Varsayılan `ps`; bash istiyorsan `--script sh`. Kaynak: kurulum belgesi.

## Bağlantılar

- Repo: https://github.com/github/spec-kit
- Belgeler: https://github.github.io/spec-kit/ ([kurulum](https://github.github.io/spec-kit/installation.html), [SDD walkthrough](https://github.github.io/spec-kit/quickstart.html), [entegrasyonlar](https://github.github.io/spec-kit/integrations/index.html), [upgrade](https://github.github.io/spec-kit/upgrade.html))
- Sürümler: https://github.com/github/spec-kit/releases
- Lisans: https://github.com/github/spec-kit/blob/main/LICENSE

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W45 bölümü](../bolumler/2026-W45.md) videosu için hazırlandı: https://www.youtube.com/watch?v=AkiRwaGZDZA. Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
