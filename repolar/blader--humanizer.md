---
repo: blader/humanizer
url: https://github.com/blader/humanizer
lisans: MIT
surum: v4.0.0 (2026-11-10)
readme_tarihi: 2026-11-10
dogrulama: denenmedi
bolum: 2026-W45
ozet: Yapay zekâ yazısının izlerini (26 kalıp) anlamı değiştirmeden silen, Wikipedia'nın rehberine dayanan agent skill'i.
---

# humanizer — kurulum ve ilk örnek

> **Bu belge, README'nin 10 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

AI'nin yazdığı metni, söylediğini değiştirmeden bir insanın yazdığı gibi okunur hâle getiren bir agent skill'i. Wikipedia editörlerinin AI metnini yakalamak için kullandığı [Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing) rehberine dayanır; 26 kalıbı en güçlüden en zayıfa sıralar ("not X but Y", tek satırlık kapanışlar, derin görünen sözler, sahneleme, kimseyle tartışma, zorlama üçlemeler, süs olarak kalın yazı, sohbet artıkları…) ve bulduğu her ipucunu işaretleyip yeniden yazar. Kör testte hakemler 16 denemenin 16'sında Humanizer'ın yazdığını tercih etmiş. Hedef insan okur; AI dedektörlerini atlatmayı amaçlamaz ve dedektörler çıktının çoğunu hâlâ işaretler. Claude Code, Codex ve skill destekleyen her agent'ta çalışır; blog yazısı, duyuru, belge yazan herkes için.

## Gereksinimler

- Claude Code 2.1.142 veya üstü (plugin yolu); daha eski sürümde `npx skills add` yolu.
- Ya da Codex, pi, Gemini CLI, GitHub Copilot, Windsurf (Skills CLI ile) ya da Claude.ai / Claude Desktop (ZIP yükleme).
- `npx skills add` için Node.js (README sürüm belirtmiyor).
- Hesap/anahtar: skill için gerekmiyor; agent'ın kendi hesabı yeterli.
- İşletim sistemi: README belirtmiyor.

## Kurulum

Kurulduktan sonra `/humanizer` ile çağrılır.

### Claude Code

```text
/plugin marketplace add blader/humanizer
/plugin install humanizer@humanizer
```

Plugin Claude Code 2.1.142 veya üstünü ister; eski sürümlerde:

```bash
npx skills add blader/humanizer --global --agent claude-code
```

### Codex

```bash
npx skills add blader/humanizer --global --agent codex
```

### pi

```bash
pi install git:github.com/blader/humanizer
```

Yalnız bu projeye kurmak için `-l` ekle; `pi update --extensions` yeni sürümü çeker.

### Claude.ai ve Claude Desktop

Repoyu ZIP olarak indir (**Code → Download ZIP**) ve Settings'te skill olarak yükle.

### Diğer agent'lar

```bash
npx skills add blader/humanizer --global --agent '*'
```

Skills CLI'nin tanıdığı her agent'a (Gemini CLI, GitHub Copilot, Windsurf dahil) kurar. Yukarıdaki komutların hepsinde `--global` olmadan yalnız bulunduğun projeye kurulur. CLI'nin tanımadığı bir agent için `SKILL.md` dosyasını onun skill klasörüne kopyala.

## İlk çalıştırma

Skill'i doğrudan çağır ve metni yapıştır:

```
/humanizer

[paste your text here]
```

Ya da düz dille iste:

```
Please humanize this text: [your text]
```

Bir dosyayı yeniden yazdırmak için yolunu ver:

```
Humanize the prose in docs/launch-post.md
```

Beklenen: metin yapıştırdığında ilk yeniden yazım, hâlâ yapay duran yerler için kısa bir eleştiri ve son sürüm; dosya verdiğinde yalnız düzyazı değişir, kod, veri, frontmatter ve bağlantı hedefleri olduğu gibi kalır. Özeti kaç bölümü incelediğini söyler. Kendi sesine benzemesi için 2–3 paragraf kendi yazından örnek ekle; ritmini, kelime seçimini ve noktalamasını (tire kullanıyorsan tireleri bile) takip eder.

## Sık takılınan yerler

- **`npx skills add` tüm repoyu kopyalıyor.** `SKILL.md` repo kökünde olduğu için Skills CLI `.git` dışında her şeyi (README, CHANGELOG, `AGENTS.md`, `.github/`…) skill klasörüne alıyor; bazı agent'lar `AGENTS.md` dosyasını kendiliğinden yüklediğinden reponun bakım kuralları kurulu skill'in yanına geliyor. Açık issue (12 Kasım 2026): [#336](https://github.com/blader/humanizer/issues/336).
- **Claude Code'da plugin kurulmuyor.** Plugin 2.1.142'den eski sürümlerde çalışmaz; `npx skills add blader/humanizer --global --agent claude-code` kullan. Kaynak: README, "Installation".
- **Çıktı AI dedektöründe hâlâ işaretleniyor.** Beklenen davranış; skill insan okur için düzenler, dedektör atlatmayı amaçlamaz. Kaynak: README, giriş.
- **Agent'ım listede yok.** `SKILL.md` dosyasını agent'ın skill klasörüne elle kopyala. Kaynak: README, "Other agents".

## Bağlantılar

- Repo: https://github.com/blader/humanizer
- Belgeler: https://skills.sh/blader/humanizer; kalıp listesi README'de ("The 26 patterns"); sürüm notları [CHANGELOG.md](https://github.com/blader/humanizer/blob/main/CHANGELOG.md)
- Sürümler: https://github.com/blader/humanizer/releases
- Lisans: https://github.com/blader/humanizer/blob/main/LICENSE

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W45 bölümü](../bolumler/2026-W45.md) videosu için hazırlandı: https://www.youtube.com/watch?v=AkiRwaGZDZA. Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
