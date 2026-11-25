---
repo: DietrichGebert/ponytail
url: https://github.com/DietrichGebert/ponytail
lisans: MIT
surum: v5.3.3 (2026-10-29)
readme_tarihi: 2026-11-21
dogrulama: denenmedi
bolum: 2026-W48-2
ozet: AI coding agent'ına "yalnız gerekeni yaz" kuralını öğreten, daha az kodla aynı işi yaptıran skill ve plugin.
---

# ponytail — kurulum ve ilk örnek

> **Bu belge, README'nin 21 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

Ponytail, AI coding agent'ına "en tembel kıdemli geliştirici" gibi düşünmeyi öğreten tek bir prompt'tur (`skills/ponytail/SKILL.md`; kural dosyası okuyan agent'lar için kısa hâli `AGENTS.md`). Repodaki geri kalan her şey bu prompt'u farklı agent'lara yükler. Kuralı "en az token" değil: yalnız görevin gerektirdiğini yaz; doğrulama, hata yönetimi, güvenlik ve erişilebilirlikten kesme. Agent kod yazmadan önce bir merdivenden iner: bu gerekli mi, kod tabanında zaten var mı, standart kütüphane yapıyor mu, platformun kendi özelliği var mı, kurulu bir bağımlılık yapıyor mu, tek satır olabilir mi; ancak ondan sonra çalışan en küçük kodu yazar. Dallanma, döngü, parser, para ya da güvenlik içeren mantık için küçük bir test bırakır; her yanıtın sonunda neyin atlandığını ya da denetlenmediğini söyler.

README'nin kendi ölçümüne göre (Claude Code'da, skill'li ve skill'siz aynı agent, 39 görev) kod %53, süre %41, maliyet %26, token %45 azalıyor; riskli mantığın %98'i testle geliyor (skill'siz %68). Bu sayılar reponun kendi benchmark'ıdır; yöntem ve sınırları `benchmarks/results/2026-10-07-agentic.md` dosyasında. Ayrıca `/ponytail-review` (diff incelemesi), `/ponytail-audit` (bütün repo, önem sırasıyla) ve Claude Code'da prompt'un üstünde maliyet, token ve süreyi gösteren "band" var. Lisansı MIT.

## Gereksinimler

- Desteklenen bir agent. README Claude Code ve Codex için komut verir; Copilot, Cursor, OpenCode, Gemini ve ötekilerin adımları INSTALL.md'de. Rozete göre 20 agent ile çalışır.
- Node.js: Claude Code ve Codex plugin'leri (ve Cursor hook'ları) iki küçük Node.js lifecycle hook'u çalıştırır, bu yüzden `node` PATH'te olmalı (INSTALL.md).
- Komutlar (`/ponytail-review` vb.) skill destekleyen bir agent ister: Claude Code, Codex, Devin CLI, OpenCode, Gemini, Antigravity CLI, pi, Hermes Agent, Qoder, Grok Build. Yalnız kural dosyası okuyan agent'larda (Cursor'ın rule dosyası, Windsurf, Cline, Copilot, Kiro) kurallar yüklenir, komutlar gelmez.
- Band için Claude Code 2.1.290 ya da sonrası (terminal ya da masaüstü uygulaması).
- Ayar dosyası gerekmez (README SSS).

## Kurulum

**Claude Code.** İki komutu iki ayrı prompt olarak gönder (README: "as two separate prompts"):

```
/plugin marketplace add DietrichGebert/ponytail
```

```
/plugin install ponytail@ponytail
```

**Codex:**

```bash
codex plugin marketplace add DietrichGebert/ponytail
codex plugin add ponytail@ponytail
```

Sonra Codex'te `/hooks` ekranını aç, iki lifecycle hook'una güven (trust) ve yeni bir thread başlat.

**Başka agent'lar.** `AGENTS.md` dosyasını projene kopyala ya da agent'ından `skills/ponytail/SKILL.md` dosyasını skill olarak kurmasını iste. Copilot, Cursor, OpenCode, Gemini ve ötekiler için adım adım anlatım INSTALL.md'de; oradan iki örnek:

```bash
opencode plugin add @dietrichgebert/ponytail
```

```bash
gemini extensions install https://github.com/DietrichGebert/ponytail
```

README'nin güvenlik notu: ponytail'i yalnız GitHub'daki `DietrichGebert/ponytail` reposundan ya da npm'deki `@dietrichgebert/ponytail` paketinden kur. Hiçbir zaman `.exe` ya da `.dll` dosyası dağıtmaz; böyle bir kopya yazarına ait değildir.

## İlk çalıştırma

Kurulumdan sonra ayrıca bir şey başlatmak gerekmez: ponytail her oturumda etkindir, açılışta ve seviye değişince geçerli seviyeyi yazar. Agent'a her zamanki gibi bir görev ver; README'nin örneğinde "frontend'e date picker ekle" isteğine skill'siz agent 335 satır yazarken, ponytail repodaki `Input` bileşenini tarayıcının kendi date picker'ıyla birleştirip 10 satırda bitiriyor.

Seviyeyi değiştirmek ya da kapatmak için (README'nin komut tablosu):

```
/ponytail [lite | full | ultra | off]
```

Argümansız yazınca kapalıysa varsayılan seviyede açar, açıksa geçerli seviyeyi söyler. Öteki komutlar:

- `/ponytail-review`: geçerli diff'i inceler (hata, güvenlik, gerçek yük, testsiz riskli kod, yavaş yollar, kesilecek kod). Hedefi düz sözle daraltabilirsin: `uncommitted`, `staged`, `branch` ya da bir PR bağlantısı.
- `/ponytail-audit`: aynı denetimi bütün repo için yapar, en önemlisi başta.
- `/ponytail-debt`: ertelediğin `shortcut:` yorumlarını bir deftere toplar.
- `/ponytail-gain`: benchmark'tan ölçülen etki tablosunu gösterir.
- `/ponytail-help`: komutların kısa özeti.
- `/ponytail band [on | off]` ve `/ponytail band import`: yalnız Claude Code; band'ı gizler ya da gösterir, geçmiş Claude Code oturumlarını board'a ekler. Board verisi varsayılan olarak bilgisayarında, `~/.config/ponytail/band/` altında kalır.

Varsayılan seviyeyi kalıcı ayarlamak istersen isteğe bağlı `~/.config/ponytail/config.json` dosyası ya da `PONYTAIL_DEFAULT_MODE` ortam değişkeni kullanılabilir; README dosyanın içeriğini anlatmıyor.

## Sık takılınan yerler

- Claude Code'da iki `/plugin` komutu tek mesajda gönderilirse kurulum çalışmaz; iki ayrı prompt gerekir (INSTALL.md).
- `node` PATH'te değilse skill'ler yine çalışır ama her hook çağrısında zararsız bir `node: command not found` hatası görünür; Nix ve nvm kullananlarda `node` etkileşimsiz kabuğun PATH'inde olmalı (INSTALL.md).
- Codex'te `/hooks` ekranından iki hook'a güvenmeden ve yeni thread açmadan plugin devreye girmez. Codex CLI ve IDE eklentisinde komutlar plugin'in ad alanında skill'dir: `$ponytail:ponytail-review` ile çağrılır.
- Band, VS Code sohbet panelinde, bulut oturumlarında ve `claude -p` ile görünmez.
- Cursor hook'larıyla yalnız `/ponytail` seviye değişimi çalışır ve düz mesaj olarak yazılır; öteki komutlar gelmez.
- Koda `shortcut:` yorumları yazar (bilerek alınan kestirmeyi ve ne zaman dönüleceğini işaretler). Başka bir sözcük ya da hiç istemiyorsan projenin `CLAUDE.md` ya da `AGENTS.md` dosyasında belirt, sonra `/ponytail-debt <your word>` çalıştır (README SSS).

## Bağlantılar

- Repo: https://github.com/DietrichGebert/ponytail
- Site: https://ponytail.dev
- Kurulum belgesi (bütün agent'lar): https://github.com/DietrichGebert/ponytail/blob/main/INSTALL.md
- Prompt'un kendisi: https://github.com/DietrichGebert/ponytail/blob/main/skills/ponytail/SKILL.md · kısa hâli: https://github.com/DietrichGebert/ponytail/blob/main/AGENTS.md
- Benchmark yöntemi: https://github.com/DietrichGebert/ponytail/blob/main/benchmarks/results/2026-10-07-agentic.md · örnekler: https://github.com/DietrichGebert/ponytail/tree/main/examples
- Sürümler: https://github.com/DietrichGebert/ponytail/releases (son sürüm v5.3.3, 29 Ekim 2026)
- Issues: https://github.com/DietrichGebert/ponytail/issues
- Video: Haftanın Popüler Repoları, 23–25 Kasım 2026 bölümü (listenin 8. sırası)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W48-2 bölümü](../bolumler/2026-W48-2.md) videosu için hazırlandı (video henüz yayında değil). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
