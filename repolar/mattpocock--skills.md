---
repo: mattpocock/skills
url: https://github.com/mattpocock/skills
lisans: MIT
surum: v1.3.2 (2026-10-29)
readme_tarihi: 2026-11-12
dogrulama: denenmedi
bolum: 2026-W45
ozet: Matt Pocock'un Claude Code, Codex ve benzeri coding agent'lar için günlük mühendislik skill paketi (grill-me, tdd, triage).
---

# skills — kurulum ve ilk örnek

> **Bu belge, README'nin 12 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

Matt Pocock'un her gün kullandığı agent skill'leri: coding agent'ın (Claude Code, Codex, Copilot, Gemini CLI, Cursor…) işe başlamadan önce seni sorguya çekmesi (`/grill-me`), projenin ortak sözlüğünü kurması (`/grill-with-docs`), kırmızı-yeşil-refactor döngüsüyle test yazması (`/tdd`), zor hataları disiplinli teşhis etmesi, konuşmayı spec'e ve ticket'lara çevirmesi, kod incelemesi yapması. Skill'ler küçük, düzenlenebilir ve birleştirilebilir; herhangi bir modelle çalışıyor. README'ye göre GSD, BMAD, Spec-Kit gibi süreci devralan yaklaşımlara karşı kontrolü sende bırakan bir alternatif.

## Gereksinimler

- İşletim sistemi: README belirtmiyor; skill'ler metin dosyası, agent'ın çalıştığı her yerde.
- Skill'leri çalıştıracak bir coding agent kurulu ve oturum açılmış: Claude Code, Codex, GitHub Copilot (CLI ya da VS Code), Gemini CLI; ya da Cursor, OpenCode, Devin, Windsurf, Amp, pi gibi `skills` CLI'nin tanıdığı bir agent.
- "Diğer agent" yolu için Node.js ve `npx` (README sürüm belirtmiyor).
- Hesap/anahtar: skill'lerin kendisi için gerekmiyor; agent'ın kendi hesabı yeterli. `/triage`, `/to-spec`, `/to-tickets` gibi skill'ler GitHub ya da GitLab issue tracker'ına yazıyorsa o hesabın erişimi gerekir (kurulum skill'i hangi tracker'ı kullanacağını sorar).

## Kurulum

README "30 saniyelik kurulum" diyor. Agent'ına göre **tek** yol seç: plugin kendini günceller, skills.sh ise düzenlenebilir dosyaları projene kopyalar ve elle güncellersin. İkisini birden kurarsan her skill iki kez görünür.

### Claude Code

```bash
claude plugin install mattpocock-skills@claude-plugins-official
```

Varsayılan olarak kendini günceller.

### Codex

```bash
codex plugin marketplace add mattpocock/skills
codex plugin add mattpocock-skills@mattpocock
```

### GitHub Copilot (CLI ve VS Code)

```bash
copilot plugin marketplace add mattpocock/skills
copilot plugin install mattpocock-skills@mattpocock
```

Sonra bir kez `~/.copilot/settings.json` dosyasına ekle:

```json
{
  "extraKnownMarketplaces": {
    "mattpocock": { "source": { "source": "github", "repo": "mattpocock/skills" }, "autoUpdate": true }
  }
}
```

VS Code'da **Chat: Install Plugin From Source** komutunu çalıştırıp `https://github.com/mattpocock/skills` adresini gir; günde bir güncellenir.

### Gemini CLI (elle güncelleme)

```bash
gemini skills install https://github.com/mattpocock/skills.git --path skills/engineering
gemini skills install https://github.com/mattpocock/skills.git --path skills/productivity
```

Güncellemek için iki komutu yeniden çalıştır.

### Diğer agent'lar ya da düzenlenebilir dosyalar (elle güncelleme)

```bash
npx skills@latest add mattpocock/skills -a <agent>  # cursor, opencode, devin, windsurf, amp, pi; omit -a to choose
```

Kurulum hangi skill'leri alacağını sorduğunda `setup-matt-pocock-skills` skill'ini dahil et. Güncelleme: `npx skills@latest update`; yeni eklenen skill'ler için `add` komutunu yeniden çalıştır.

## İlk çalıştırma

1. Agent'ını bir projede aç ve kurulum skill'ini her repoda bir kez çalıştır:

   ```
   /setup-matt-pocock-skills
   ```

   Üç şey sorar: hangi issue tracker (GitHub, GitLab, yerel dosya ya da tarif ettiğin başka bir şey), ticket'ları triage ederken kullandığın etiketler (`/triage` etiketlerle çalışır) ve üretilen belgelerin nereye kaydedileceği.

2. En küçük örnek: yapmak istediğin değişikliği anlatıp sorguya çekilmeyi iste:

   ```
   /grill-me
   ```

   Agent, tasarım ağacının her dalı çözülene kadar sana soru sorar; README'ye göre en çok kullanılan skill bu. Kodla çalışırken aynı işi proje sözlüğü (`GLOSSARY.md`) ve ADR'lerle birlikte yapan `/grill-with-docs` skill'ini kullan. Hangi skill'in uyduğunu bilmiyorsan `/ask-matt` yönlendirir.

## Sık takılınan yerler

- **Claude Code plugin'i eski sürümde kalıyor.** Resmi marketplace (`claude-plugins-official`) plugin'i sabit bir commit'e sabitliyor; `/plugin update` "güncel" diyor ama eski sürümde kalıyor. Issue'daki çözüm: resmi kurulumu kaldırıp reponun kendi marketplace'ini eklemek (`claude plugin marketplace add mattpocock/skills`, sonra `claude plugin install mattpocock-skills@mattpocock`). Kaynak: [#1164](https://github.com/mattpocock/skills/issues/1164).
- **"Plugin not found in any configured marketplace".** Bazı makinelerde kurulum komutu plugin'i bulamıyor; birkaç denemede buluyor ya da önce marketplace eklemek gerekiyor. Kaynak: [#692](https://github.com/mattpocock/skills/issues/692).
- **Skill'ler iki kez görünüyor.** Hem plugin hem skills.sh yoluyla kurduysan olur; agent başına tek yol seç (README, "Installation").
- **Mühendislik skill'leri nereye yazacağını bilmiyor.** `/setup-matt-pocock-skills` o repoda çalıştırılmadan `/triage`, `/to-spec`, `/to-tickets` tracker'ı ve belge klasörünü bilmez (README, adım 2).

## Bağlantılar

- Repo: https://github.com/mattpocock/skills
- Belgeler: https://aihero.dev/skills (her skill'in açıklaması README'nin "Reference" bölümünde, SKILL.md dosyaları `skills/` altında)
- Sürümler: https://github.com/mattpocock/skills/releases
- Lisans: https://github.com/mattpocock/skills/blob/main/LICENSE

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W45 bölümü](../bolumler/2026-W45.md) videosu için hazırlandı: https://www.youtube.com/watch?v=AkiRwaGZDZA. Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
