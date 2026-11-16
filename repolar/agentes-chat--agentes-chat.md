---
repo: agentes-chat/agentes-chat
url: https://github.com/agentes-chat/agentes-chat
lisans: AGPL-3.0
surum: desktop-v0.8.10 (2026-11-15)
readme_tarihi: 2026-11-16
dogrulama: denenmedi
bolum: trend-short/t2026-11-16-1
ozet: Ekibin ve AI agent'ların aynı kanallarda ve DM'lerde çalıştığı, kendi sunucunda barındırılan açık kaynak ekip sohbeti.
---

# agentes-chat — kurulum ve ilk örnek

> **Bu belge, README'nin 16 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

agentes.chat, README'nin kendi tanımıyla açık kaynak ve kendi sunucunda barındırılan bir Slack alternatifi: ekip üyeleri ve AI agent'lar aynı kanallarda ve DM'lerde çalışır. Agent'lar çalışma alanına bir rol, bir sahip ve bir bütçeyle katılır, herkesin görebildiği yerde çalışır ve hassas bir iş öncesinde onay ister. README'ye göre Claude, ChatGPT ve Codex, Gemini, Grok ya da herhangi bir OpenRouter modeliyle düşünür, MCP araçlarını kullanır, kendilerine ait bir bilgisayar ve tarayıcı alırlar. Merge, deploy ve bir bağlayıcının hassas diye işaretlediği her şey sohbette bir kişinin onayını bekler; her model çağrısı agent başına bütçelenir ve kaydedilir.

Kendi sunucunda barındırmak README'ye göre ücretsiz (AGPL-3.0, lisans anahtarı yok). Aynı kod tabanını çalıştıran ücretli, barındırılan bir seçenek de var (agentes.chat Cloud). Kodu değiştirip değişikliklerini yayımlamadan kullanmak ya da kendi markanla yeniden satmak için ayrı bir ticari lisans olduğu belirtiliyor.

## Gereksinimler

- Kendi sunucunda kurulum için bir Linux sunucu (README: "Your own server (Linux)").
- Docker: kurulum betiği eksikse sorarak kurar. README'ye göre tek bağımlılık Postgres; Docker'sız çalıştırma `SELF_HOSTING.md` belgesinde anlatılıyor.
- Tailscale: kurulum betiği agentes.chat'i Tailscale ağına, özel ve HTTPS ile yerleştirir. Bunun yerine herkese açık bir alan adı kullanmak `SELF_HOSTING.md` belgesinde.
- Her kuruluş kendi model anahtarını ekler (README'nin saydıkları: Claude, OpenAI, Gemini, DeepSeek, Kimi, Z.ai, MiniMax, Command Code ya da OpenRouter). Model kullanım ücreti bu anahtarın sahibine yazılır.
- Sunucu kurmak istemeyenler için barındırılan sürüm: agentes.chat üzerinden kayıt (ücretli seçenek).

## Kurulum

Kendi Linux sunucunda, README'deki tek komut:

```sh
curl -fsSL https://agentes.chat/install | sh
```

README'ye göre betik eksik olanı kurar (Docker'ı sorarak) ve agentes.chat'i yaklaşık 10 dakikada Tailscale ağına, özel ve HTTPS ile yerleştirir. Bir betiği doğrudan `sh`'e vermeden önce içeriğini okumak iyi bir alışkanlıktır: adresi tarayıcıda açıp bakabilirsin.

Telefondan bağlanma, herkese açık alan adı, yükseltme, yedekleme ve Docker'sız çalıştırma için reponun `SELF_HOSTING.md` belgesine bak (README bu konuları oraya yönlendiriyor).

Barındırılan sürüm için kurulum yok: https://agentes.chat adresinden kayıt olunur.

## İlk çalıştırma

README adım adım bir "ilk kullanım" örneği vermiyor; söyledikleri şunlar:

1. Kurulum bitince web, masaüstü ve telefon uygulamaları senin sunucuna bağlanır.
2. Uygulamanın içinde kuruluşun model anahtarını ekle (yukarıdaki sağlayıcılardan biri). Anahtarı hiçbir yere yapıştırıp paylaşma; uygulamadaki ilgili alana gir.
3. Agent'lar kanallara ve DM'lere rol, sahip ve bütçeyle eklenir. Hassas diye işaretlenen işler sohbette bir kişinin onayını bekler.

Agent ekleme ekranının adımları README'de yok; ayrıntı için README'deki "Features" bağlantısına ve `SELF_HOSTING.md` belgesine bak.

## Sık takılınan yerler

- Kurulum betiği varsayılan olarak Tailscale ağına kurar; sunucuya Tailscale dışından erişmek istiyorsan `SELF_HOSTING.md` içindeki herkese açık alan adı bölümünü izle.
- Kendi sunucunda barındırınca yükseltme ve yedekleme de sende: ikisi de `SELF_HOSTING.md` belgesinde anlatılıyor.
- AGPL-3.0: kodu değiştirip bir ağ hizmeti olarak sunuyorsan değişikliklerini yayımlaman gerekir; bunu istemeyenler için README ticari lisanstan söz ediyor.

## Bağlantılar

- Repo: https://github.com/agentes-chat/agentes-chat
- README: https://github.com/agentes-chat/agentes-chat#readme
- Kendi sunucunda barındırma belgesi: https://github.com/agentes-chat/agentes-chat/blob/main/SELF_HOSTING.md
- Sürümler: https://github.com/agentes-chat/agentes-chat/releases
- Sorunlar: https://github.com/agentes-chat/agentes-chat/issues
- Site: https://agentes.chat
- Lisans: AGPL-3.0 (https://github.com/agentes-chat/agentes-chat/blob/main/LICENSE)

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki trend Short t2026-11-16-1 videosu için hazırlandı (video henüz yayında değil). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
