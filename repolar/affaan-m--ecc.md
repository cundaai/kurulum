---
repo: affaan-m/ECC
url: https://github.com/affaan-m/ECC
lisans: MIT
surum: v2.2.3 (2026-10-01)
readme_tarihi: 2026-11-19
dogrulama: denenmedi
bolum: 2026-W47
ozet: Claude Code, Codex ve öbür coding agent'lara planlama, test, inceleme, hafıza ve güvenlik taraması kazandıran hazır agent, skill ve hook paketi.
---

# ECC — kurulum ve ilk örnek

> **Bu belge, README'nin 19 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

ECC, coding agent'ına hazır bir çalışma düzeni kurar: önce plan yapar, değişikliği testle doğrular, işini temiz bir bağlamda yeniden inceler, önemli olanı hatırlar. README bu akışı `plan -> test -> implement -> review -> verify -> remember -> improve` diye özetliyor. Pakette README'ye göre 72 agent, 328 skill ve 99 eski komut kısayolu, ayrıca hook'lar, kurallar, hafıza, sürekli öğrenme ve AgentShield güvenlik taraması var. En iyi Claude Code ile çalışır; Codex ve Qoder için kendi plugin yolu, Cursor, OpenCode, Gemini, Zed, GitHub Copilot gibi araçlar için yetenekleri sınırlı uyarlayıcıları bulunur. Lisansı MIT.

**Yalnız resmî kaynaklardan kur.** README'nin en üstündeki uyarı: ECC yalnız doğrulanmış kanallardan kurulmalı. Bunlar GitHub'daki `github.com/affaan-m/ECC` reposu, `ecc-universal` ve `ecc-agentshield` npm paketleri, GitHub App, `ecc@ecc` plugin adı ve `ecc.tools` sitesi. Başkalarının yeniden yüklediği kopyalar ve resmî olmayan aynalar proje tarafından incelenmiyor ve zararlı yazılım içerebilir.

## Gereksinimler

- Node.js 18 ya da üstü (README: "The universal package requires Node.js 18 or newer").
- Claude Code plugin kurulumu için Git ve PATH'te Claude Code 2.1 ya da üstü. Sürümü `claude --version` ile görürsün.
- Codex ya da Kimi Code için o aracın kurulu olması; öbür araçların desteği README'deki "Platform Support" tablosunda.
- Windows'ta README PowerShell ile kurulumu anlatıyor; repoyu indirmek gerekmiyor.

## Kurulum

README'nin önerdiği yol, yönlendirmeli kurulum. Claude Code plugin'ini kurar, günceller, kapsamını ve hook profilini değiştirir:

```bash
npx ecc-universal@2.2.3 setup
```

Windows'ta önce gereksinimleri doğrula, sonra aynı komutu PowerShell'de çalıştır:

```powershell
node --version
git --version
claude --version
```

README'ye göre tipik kişisel kurulumda **Global user** ve **Standard** hook seçilir. Aynı kurulum `pnpm dlx ecc-universal@2.2.3 setup`, `yarn dlx ecc-universal@2.2.3 setup` (Yarn 2+) ve `bunx ecc-universal@2.2.3 setup` ile de yapılır.

Birden çok agent'ı (Claude Code, Codex, Kimi Code) tek akışta kurmak için:

```bash
npx ecc-universal@2.2.3 install --guided
```

Diske yazmadan önce denemek için README'nin örneği:

```bash
npx ecc-universal@2.2.3 install --guided --harness codex --dry-run
```

Bunların yerine Claude Code'un kendi plugin komutlarını da kullanabilirsin (Claude Code içinde yazılır):

```text
/plugin marketplace add https://github.com/affaan-m/ECC
/plugin install ecc@ecc
```

README iki yolun aynı `ecc@ecc` plugin'ini kurduğunu söylüyor: birini seç, üstüne elle tam kurulum yapma. README ayrıca sürüm sabitlemenin güvenlik denetimi olmadığını, paket kodunu çalıştırmadan önce sürüm kaynağına bakılmasını yazıyor.

## İlk çalıştırma

1. Yeni bir Claude Code oturumu aç ve plugin'in açık olduğunu doğrula (README: `ecc@ecc` listede "enabled" görünmeli):

```text
/plugin list
```

2. Bütün kataloğu değil, ihtiyacın olan akışı başlat. README'nin yeni özellik örneği:

```text
/ecc:plan "Add authentication"
```

   Planlayıcı uygulama planını çıkarır; ardından `tdd-workflow` skill'i önce test yazdırır, `/code-review` yazılan kodu temiz bağlamda inceler.
3. README'deki öbür giriş noktaları: bozuk derleme için `/build-fix`, kod temizliği için `/refactor-clean`, bağlam yükünü görmek için `/context-budget`, uzun oturumu kapatırken `/save-session`, sonra sürdürmek için `/resume-session`, agent ayarlarını denetlemek için `/security-scan`.

Plugin kurulumunda komutlar `/ecc:` önekiyle yazılır; elle kurulumda kısa hâli (`/plan`) çıkabilir. Kurulumdan sonra ayarları değiştirmek için `/ecc:configure-ecc` var.

## Sık takılınan yerler

- ECC iki kez görünüyor ya da hook'lar iki kez çalışıyorsa sebep genellikle plugin'in üstüne `./install.sh --profile full` çalıştırmaktır. README'nin çözümü: plugin kurulumunu kaldır, ECC klasöründe `node scripts/ecc.js uninstall --dry-run` çalıştır, elle kopyaladığın kural klasörlerini sil, tek yolla yeniden kur.
- "Duplicate hooks file" hatası: `.claude-plugin/plugin.json` dosyasına `"hooks"` alanı ekleme; Claude Code v2.1+ `hooks/hooks.json` dosyasını kendiliğinden yükler.
- `npx ecc-install --profile minimal --target claude` yazma: `ecc-install`, `ecc-universal` paketinin içindeki bir komut adı, ayrı bir npm paketi değil.
- npm sürüm ya da önbellek hatası verirse önce `npm view ecc-universal version` ile kayıtlı sürümü doğrula.
- Yarn Classic 1'de `yarn dlx` yok; `npx` kullan.
- Kurulum sorununda README'nin kısa formu: https://github.com/affaan-m/ECC/issues/new?template=install-problem.yml ya da `ecc feedback`. ECC tanılama verisini kendiliğinden göndermez.

## Bağlantılar

- Repo: https://github.com/affaan-m/ECC
- Site: https://ecc.tools
- npm paketleri: https://www.npmjs.com/package/ecc-universal · https://www.npmjs.com/package/ecc-agentshield
- Sorun giderme: https://github.com/affaan-m/ECC/blob/main/TROUBLESHOOTING.md · Güvenlik: https://github.com/affaan-m/ECC/blob/main/SECURITY.md
- Sürümler: https://github.com/affaan-m/ECC/releases (son sürüm v2.2.3, 1 Ekim 2026)
- Issues: https://github.com/affaan-m/ECC/issues · Discord: https://discord.gg/36yGMHGFbR
- Video: Haftanın Popüler Repoları, 16–22 Kasım 2026 bölümü

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W47 bölümü](../bolumler/2026-W47.md) videosu için hazırlandı: https://www.youtube.com/watch?v=6jXWzYzxfKI. Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
