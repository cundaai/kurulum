---
repo: erkamyaman/rust-anything
url: https://github.com/erkamyaman/rust-anything
lisans: MIT
surum: v2.1.0 (2026-11-19)
readme_tarihi: 2026-11-19
dogrulama: denenmedi
bolum: trend-short/t2026-11-20-1
ozet: Uygulamanı parça parça Rust'a taşırken her parçayı özgün kodun kaydedilmiş davranışıyla karşılaştıran CLI ve MCP sunucusu.
---

# rust-anything — kurulum ve ilk örnek

> **Bu belge, README'nin 19 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

rust-anything, bir uygulamayı parça parça Rust'a taşırken davranışın aynı kaldığını denetler. README'ye göre Node.js (TypeScript/JavaScript), Python, C, Go, Ruby, Java ya da Kotlin uygulamanın bugün ne yaptığını kaydeder; Rust'a taşınan her parçayı ilerlemeden önce bu kayıtla karşılaştırır. CLI'lar, HTTP sunucuları ve kütüphaneler için çalışır.

Kodu kendisi çevirmez (README: "rust-anything does not translate code"): Rust'ı sen ya da coding agent'ın (Claude Code, Codex, Cursor, OpenCode…) yazar. Aracın verdiği: zamanın nerede harcandığı, neyin önce taşınacağı, kodun geri kalanının Rust'ı aynı API ile çağırmasını sağlayan bir bridge, port'un tutturması gereken kayıtlı davranış ve port'un bittiğini söyleyen `gate` denetimi. README'deki dört adım: Record (özgün kodu çalıştırıp döndürdüğünü ya da fırlattığını kaydeder), Port (`bridge` Rust crate'ini ve sarmalayıcıyı üretir), Check (`diff` her kaydı Rust'a karşı yeniden oynatır), Lift (her şey geçince uygulama saf Rust olur; özgün koda dokunulmaz). Lisansı MIT.

## Gereksinimler

- Rust araç zinciri (`cargo`): crates.io'dan kurulum için.
- Rust araç zinciri yoksa: README'ye göre her GitHub release'ine macOS, Linux ve Windows için hazır dosyalar ekli; JavaScript projelerinde `npm install -g rust-anything` aynı dosyayı kurar (bunun için Node.js ve npm).
- Agent ile kullanmak için kurulu bir coding agent: Claude Code, Codex, Cursor, Gemini CLI ya da OpenCode (`setup` bunları kendisi bulur). Başka MCP istemcilerinde `rust-anything mcp` elle stdio sunucusu olarak eklenir.
- Taşınacak uygulamanın kendi çalışma ortamı (README'deki örnekte `node bench.mjs` çalıştırılıyor). README ayrı bir sürüm şartı listesi vermiyor; ayrıntı için docs/install.md.

## Kurulum

README'nin önerdiği yol, şu metni coding agent'ına yapıştırmak:

```text
Install rust-anything: run `cargo install rust-anything`, then `rust-anything setup`.
Tell me when to restart you.
```

Kendin kurmak için:

```sh
cargo install rust-anything   # from crates.io
rust-anything setup           # registers it with Claude Code, Codex, Cursor, Gemini CLI and OpenCode
```

Rust araç zinciri yoksa Releases sayfasındaki hazır dosyayı indir ya da JavaScript projelerinde:

```sh
npm install -g rust-anything
```

Claude Code'da plugin olarak da kurulabilir (MCP sunucusu ve skill; yukarıdaki CLI `PATH`'te olmalı):

```text
/plugin install rust-anything --marketplace erkamyaman/rust-anything
```

`setup` bulduğu her agent'a MCP sunucusunu ekler ve agent skill okuyorsa iş akışı skill'ini kurar. README'ye göre öbür ayarlarını korur, yeniden çalıştırmak güvenlidir, `--remove` geri alır; var olan bir dosyayı değiştirmeden önce yanına kopyasını kaydeder (`<file>.rust-anything.bak`).

OpenCode'u elle bağlamak için sunucu `~/.config/opencode/opencode.json` dosyasına yazılır ve repodaki `skills/rust-anything/` klasörü `~/.config/opencode/skills/rust-anything/` altına kopyalanır:

```json
{
  "mcp": {
    "servers": {
      "rust-anything": { "type": "local", "command": ["rust-anything", "mcp"] }
    }
  }
}
```

## İlk çalıştırma

Agent ile: kurulumdan sonra agent'ı yeniden başlat ve README'deki örneklerden birini yaz:

- "Rewrite this CLI in Rust with rust-anything, end to end, and prove it behaves the same."
- "This search feels slow. Profile it with rust-anything and move the hot loop to Rust."
- "Plan a migration of `src/parser/` to Rust with rust-anything; port the first unit and prove it."

Adımları kendin çalıştırmak için README'deki örnek (bir TypeScript fonksiyonunu, `src/fuzzy.ts` içindeki `fuzzyScore`'u taşıyor):

```sh
rust-anything profile . -- node bench.mjs              # what is hot, and who calls it
rust-anything bridge . src/fuzzy.ts --batch fuzzyScore:text
# write src/port.rs (or let your agent), then:
rust-anything record rust/fuzzy-rs && rust-anything build rust/fuzzy-rs
rust-anything diff rust/fuzzy-rs                       # every recorded case must match
rust-anything bench rust/fuzzy-rs                      # faster? the verdict says
rust-anything gate rust/fuzzy-rs                       # done means this passes
```

Sıra: `profile` neyin yavaş olduğunu ve onu kimin çağırdığını gösterir, `bridge` Rust crate'ini üretir, sen ya da agent'ın `src/port.rs` dosyasını yazar, `record` ve `build` kaydı alıp derler, `diff` her kaydın tutmasını ister, `bench` hız kararını verir, `gate` geçince port bitmiş sayılır. Dosya adları README'nin örneğinden; kendi projende kendi dosyalarını yazarsın.

## Sık takılınan yerler

- Araç Rust kodunu yazmaz; yalnız kaydeder, karşılaştırır ve denetler. Rust'ı yazan sen ya da agent'ın.
- Claude Code plugin'i tek başına yetmez: README'ye göre CLI ayrıca kurulmuş ve `PATH`'te olmalı.
- Aynı çıktı yeterli değil: README, uygulamanın gönderdiklerinin de (e-posta, webhook, veritabanı) `app record --effects` ile kaydedilmesini anlatıyor; aynı çıktıyı basıp iki kez e-posta atan bir port `app replay`'de kalır (docs/whole-apps.md).
- `setup` listede olmayan bir MCP istemcisini bulamaz; orada `rust-anything mcp` elle eklenir (docs/install.md).

## Bağlantılar

- Repo: https://github.com/erkamyaman/rust-anything
- npm paketi: https://www.npmjs.com/package/rust-anything
- Sürümler ve hazır dosyalar: https://github.com/erkamyaman/rust-anything/releases (son sürüm v2.1.0, 19 Kasım 2026)
- Kurulum belgesi: https://github.com/erkamyaman/rust-anything/blob/main/docs/install.md
- Bütün uygulamalar ve yan etkiler: https://github.com/erkamyaman/rust-anything/blob/main/docs/whole-apps.md
- Issues: https://github.com/erkamyaman/rust-anything/issues
- Video: Haftanın Popüler Repoları, 20 Kasım 2026 Short'u

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki trend Short t2026-11-20-1 videosu için hazırlandı (video henüz yayında değil). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
