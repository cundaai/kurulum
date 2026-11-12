---
repo: JuliusBrussee/caveman
url: https://github.com/JuliusBrussee/caveman
lisans: Apache-2.0
surum: v4.1.0 (2026-11-08)
readme_tarihi: 2026-11-12
dogrulama: denenmedi
bolum: gunun-reposu/g01
ozet: Claude Code, Codex, Gemini CLI gibi kodlama ajanlarına daha az token harcatan skill ve yerel proxy; kod birebir kalır.
---

# Caveman — kurulum ve ilk örnek

> **Bu belge, README'nin 12 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

Caveman, Claude Code, Codex, Gemini CLI, OpenCode gibi kodlama ajanlarının ürettiği ve okuduğu token sayısını düşürür. "Say less" kuralıyla ajan kısa, "caveman" tarzı cevap verir; kod, komut, dosya yolu ve hata mesajları karakteri karakterine korunur. "Read less" ile log, JSON, CSV ve test çıktıları modele ulaşmadan küçültülür; "Auto" her istek için model ve çaba düzeyini seçer. Token faturasını ya da bağlam penceresini dert eden, Claude Pro/Max ya da API anahtarıyla çalışan geliştiriciler için.

## Gereksinimler

- İşletim sistemi: macOS, Linux, Windows (PowerShell 5.1+).
- Node.js 22.13+ (tek komutluk kurulum ve `caveman` CLI için). Hook'ların kendiliğinden açılması için `PATH` üzerinde Node.js 18+ yeterli (INSTALL.md).
- Kurulu en az bir kodlama ajanı: Claude Code, Codex, Gemini CLI, OpenCode… (30'dan fazla ajan [INSTALL.md](https://github.com/JuliusBrussee/caveman/blob/main/INSTALL.md)'de listeli).
- Hesap gerekmez; kendi API anahtarın ya da Claude Pro/Max girişin kullanılır. Yalnız "Auto" özelliği ücretsiz bir Caveman hesabı ister (`caveman login`).

## Kurulum

En basit yol tek komut; ajanlarını ve girişlerini bulur, neyi değiştireceğini gösterir, Enter'a basarsın:

```bash
npx @caveman-ai/cli@latest
```

4.0 öncesi bir caveman kuruluysa `@latest` demek şart; yoksa makinedeki eski kopya çalışır. Kurulum ekranını yeniden açmak için `caveman setup`.

Yalnız kısa cevap istiyorsan (proxy'siz, sadece skill):

```bash
npx skills add JuliusBrussee/caveman -g
```

Claude Code eklentisi olarak (her etkileşimli oturumda ve alt ajanlarda kendiliğinden açılır):

```bash
# Claude Code plugin, auto-starts every interactive session, subagents too
claude plugin marketplace add JuliusBrussee/caveman && claude plugin install caveman@caveman
```

Claude Code'un içindeysen aynı şeyin slash hâli:

```text
/plugin marketplace add JuliusBrussee/caveman
/plugin install caveman@caveman
```

Gemini CLI için:

```bash
gemini extensions install https://github.com/JuliusBrussee/caveman
```

### macOS / Linux (makinedeki tüm ajanlar + Claude Code durum çubuğu rozeti)

```bash
curl -fsSL https://raw.githubusercontent.com/JuliusBrussee/caveman/v4.1.0/install.sh | bash
```

### Windows (PowerShell 5.1+)

```powershell
irm https://raw.githubusercontent.com/JuliusBrussee/caveman/v4.1.0/install.ps1 | iex
```

Kaldırmak için (npm 12+ ise `--allow-git=root` da ekle):

```bash
npx -y github:JuliusBrussee/caveman -- --uninstall
```

Yalnız skill'i aldıysan `npx skills remove JuliusBrussee/caveman -g` geri çıkarır.

## İlk çalıştırma

Kurulumdan sonra terminalde neyin açık olduğuna bak, sonra ajanını caveman üzerinden başlat:

```bash
caveman status                # what is on, which agents it reaches, one next step
caveman claude                # start an agent on caveman: codex · gemini · opencode · aider · kilo · qwen · hermes · openclaw · pi
```

Ajanın içinde herhangi bir soru sor; cevabın kısa gelmesi beklenir (README örneği: 63 token'lık açıklama 20 token olur). Kendiliğinden başlamazsa `/caveman` yaz (Codex'te `$caveman`), daha da kısası için `/ultracave`. Durumu `/caveman status` gösterir; `stop caveman` ya da `/caveman off` eski hâle döndürür. Parçaları tek tek kapatıp açabilirsin: `caveman off browse`, `caveman on browse`. Sorun olursa `caveman doctor` her satıra bir sorunu ve çözümünü yazar.

CLI varsayılan olarak kullanım istatistiği gönderir (çalışan komutlar, token sayıları, rastgele kurulum kimliği; prompt ve kod asla). Kapatmak için `caveman telemetry off` ya da `DO_NOT_TRACK=1` (README, Privacy).

## Sık takılınan yerler

- **Eski tek satırlıklar npm 12+ ile çalışmıyor.** npm 12 git kurulumunu engelliyor; v3.1.0 ve öncesine sabitlenmiş komutlar hata verir. Yukarıdaki v4.1.0 komutlarını ya da `npx --allow-git=root -y github:JuliusBrussee/caveman#v4.1.0` kullan ([README, "Other ways in"](https://github.com/JuliusBrussee/caveman#say-less-the-caveman-voice)).
- **Kendiliğinden açılmıyor.** Hook'lar küçük Node.js betikleri; `PATH` üzerinde Node.js 18+ yoksa eklenti sessizce kenara çekilir ve caveman ancak `/caveman` yazınca başlar ([INSTALL.md, Troubleshooting](https://github.com/JuliusBrussee/caveman/blob/main/INSTALL.md#troubleshooting)).
- **Windows'ta hook'lar başarısız.** `install.sh` değil `install.ps1` kullan; Git Bash kabuk tarafını kurar ama hook tarafı PowerShell ister. `$PSVersionTable.PSVersion` ile 5.1+ olduğunu kontrol et ([INSTALL.md](https://github.com/JuliusBrussee/caveman/blob/main/INSTALL.md#troubleshooting)).
- **`claude -p` (print modu) sıkıştırmıyor.** Abonelik (OAuth) girişiyle etkileşimli oturumda sıkıştırma çalışırken print modunda istekler motora hiç ulaşmıyor ([#1020](https://github.com/JuliusBrussee/caveman/issues/1020)).
- **Bazı ajanlarda etkisi yok.** Antigravity CLI'da durum "not tracked by this host" kalıyor ve çıktı kısalmıyor ([#1285](https://github.com/JuliusBrussee/caveman/issues/1285)); Opus 5.5'te hook'lar her tur çalışsa da ultracave'in yok sayıldığı bildirildi ([#1318](https://github.com/JuliusBrussee/caveman/issues/1318)).

## Bağlantılar

- Repo: https://github.com/JuliusBrussee/caveman
- Belgeler: https://docs.caveman.so/caveman — kurulum matrisi: [INSTALL.md](https://github.com/JuliusBrussee/caveman/blob/main/INSTALL.md)
- Sürümler: https://github.com/JuliusBrussee/caveman/releases
- Lisans: Apache-2.0 — https://github.com/JuliusBrussee/caveman/blob/main/LICENSE

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [Günün Reposu g01](../bolumler/gunun-reposu-g01.md) videosu için hazırlandı: https://www.youtube.com/watch?v=mjaNUVKMbwQ. Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
