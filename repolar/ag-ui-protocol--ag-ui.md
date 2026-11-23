---
repo: ag-ui-protocol/ag-ui
url: https://github.com/ag-ui-protocol/ag-ui
lisans: MIT
surum: release/2026-11-21 (2026-11-21)
readme_tarihi: 2026-11-16
dogrulama: denenmedi
bolum: 2026-W47
ozet: AI agent'ları kullanıcıya dönük uygulamalara tek bir olay akışıyla bağlayan açık protokol ve SDK'ları.
---

# ag-ui — kurulum ve ilk örnek

> **Bu belge, README'nin 16 Kasım 2026 tarihli hâline göre yazıldı; komutlar bu bilgisayarda denenmedi.** Takıldığın yerde önce reponun README'sine ve issues sayfasına bak; eksik ya da yanlış gördüğünde bu depoda issue aç.

## Ne işe yarar

AG-UI, AI agent'ların kullanıcıya dönük uygulamalara nasıl bağlanacağını standartlaştıran açık, hafif, olay tabanlı bir protokol. README'nin anlatımıyla agent'lar istek/yanıt kalıbına uymaz: token akıtır, yanıtın ortasında tool çağırır, ortak state'i günceller, insan onayı için durur, subagent başlatır ve her framework bunu başka türlü yapar. AG-UI bu özel bağlantıların yerine iki yönlü tek bir olay akışı koyar: agent tipli olaylar yayar, hangi framework ile yazılmış olursa olsun her istemci bunları gösterebilir. Aynı agent web uygulamasını, mobil uygulamayı, terminali ya da Slack'i besleyebilir. MCP agent'a tool ve bağlam verir, A2A agent'ları birbirine bağlar, AG-UI agent'ı kullanıcıya getirir. 1.0 ile spesifikasyon donduruldu; TypeScript, Python ve .NET SDK'ları ondan üretiliyor. Lisansı MIT.

## Gereksinimler

- Hazır uygulama iskeleti için Node.js (`npx`); README bu yol için sürüm yazmıyor. Üretilen şablonlar OpenAI çağırır, yani bir OpenAI API anahtarı gerekir.
- Protokolü doğrudan konuşan örnek için: agent tarafında Python 3.9+, istemci tarafında Node.js 20+. Bu örnekte API anahtarı gerekmez.
- .NET SDK'sı için `dotnet` komutu (README yalnız kurulum satırını veriyor).

## Kurulum

Yeni uygulama başlatmak (CLI, seçtiğin agent framework'üne bağlı bir frontend kurar: Next.js üzerinde CopilotKit ya da terminal istemcisi):

```bash
npx create-ag-ui-app@latest
```

Yalnız SDK'yı eklemek için README'nin "SDKs" tablosu: TypeScript `npm install @ag-ui/client`, Python `pip install ag-ui-protocol`, .NET `dotnet add package AGUI.Client`. Kotlin, Go, Java, Dart, Rust, Ruby, C++ ve Swift için topluluk SDK'ları repoda `sdks/community` altında.

Protokolü doğrudan denemek için agent tarafı (Python):

```bash
pip install ag-ui-protocol fastapi uvicorn
```

İstemci tarafı (TypeScript; ikinci bir terminalde, yeni bir klasörde):

```bash
npm init -y && npm pkg set type=module
npm install @ag-ui/client tsx
```

## İlk çalıştırma

Hazır iskelet yolu: `npx create-ag-ui-app@latest` bitince ekrana yazdığı sonraki adımları izle. Şablonlar OpenAI çağırdığı için `npm run dev` öncesinde anahtarını ortam değişkenine koy (README'deki örnek değer yerine kendi anahtarın):

```bash
export OPENAI_API_KEY="<API_ANAHTARI>"
```

Anahtarsız yol: README'nin 30 satırlık agent sunucusu ve istemcisi.

1. `server.py` dosyasını oluştur:

```python
# server.py
import uuid

from fastapi import FastAPI, Request
from fastapi.responses import StreamingResponse
from ag_ui.core import (
    PROTOCOL_VERSION,
    RunAgentInput,
    RunStartedEvent,
    RunFinishedEvent,
    TextMessageStartEvent,
    TextMessageContentEvent,
    TextMessageEndEvent,
)
from ag_ui.encoder import EventEncoder

app = FastAPI()

@app.post("/")
async def run_agent(input_data: RunAgentInput, request: Request):
    encoder = EventEncoder(accept=request.headers.get("accept"))

    async def events():
        yield encoder.encode(RunStartedEvent(
            thread_id=input_data.thread_id,
            run_id=input_data.run_id,
            protocol_version=PROTOCOL_VERSION,
        ))
        message_id = str(uuid.uuid4())
        yield encoder.encode(TextMessageStartEvent(message_id=message_id, role="assistant"))
        for chunk in ["Hello ", "from ", "AG-UI!"]:
            yield encoder.encode(TextMessageContentEvent(message_id=message_id, delta=chunk))
        yield encoder.encode(TextMessageEndEvent(message_id=message_id))
        yield encoder.encode(RunFinishedEvent(thread_id=input_data.thread_id, run_id=input_data.run_id))

    return StreamingResponse(events(), media_type=encoder.get_content_type())
```

2. Sunucuyu başlat:

```bash
uvicorn server:app --port 8000
```

3. İstemci klasöründe `client.ts` dosyasını oluştur:

```ts
// client.ts
import { HttpAgent } from "@ag-ui/client";

const agent = new HttpAgent({ url: "http://localhost:8000/" });

agent.addMessage({ id: crypto.randomUUID(), role: "user", content: "Hi there!" });

await agent.runAgent({}, {
  onRunStartedEvent: ({ event }) => console.log(`run started (AG-UI ${event.protocolVersion})`),
  onTextMessageContentEvent: ({ event }) => { process.stdout.write(event.delta); },
  onRunFinishedEvent: () => console.log("\nrun finished"),
});
```

4. İstemciyi çalıştır:

```bash
npx tsx client.ts
```

README'ye göre ekrana `Hello from AG-UI!` akarak gelir. Sunucuyu desteklenen bir framework ile (LangChain, CrewAI, Mastra, Google ADK, Pydantic AI…) değiştirdiğinde istemci aynı kalır.

README `create-ag-ui-app` komutunun sorduğu soruları ve ürettiği klasör yapısını adım adım göstermiyor; yalnız "yazdırılan sonraki adımları izle" diyor.

## Bağlantılar

- Repo: https://github.com/ag-ui-protocol/ag-ui
- Site: https://ag-ui.com · Belgeler: https://docs.ag-ui.com · Spesifikasyon: https://docs.ag-ui.com/spec/1.0
- Canlı örnekler (Dojo): https://dojo.ag-ui.com
- 1.0'a geçiş kılavuzu: https://docs.ag-ui.com/migrating-to-1-0 · Entegrasyonlar: https://docs.ag-ui.com/integrations
- Sürümler: https://github.com/ag-ui-protocol/ag-ui/releases (son etiket release/2026-11-21, 21 Kasım 2026)
- Issues: https://github.com/ag-ui-protocol/ag-ui/issues · Discussions: https://github.com/ag-ui-protocol/ag-ui/discussions · Discord: https://discord.gg/Jd3FzfdJa8
- Video: Haftanın Popüler Repoları, 16–22 Kasım 2026 bölümü

---

*Bu belge, [Haftanın Popüler Repoları](https://www.youtube.com/@haftaninpopulerrepolari) kanalındaki [2026-W47 bölümü](../bolumler/2026-W47.md) videosu için hazırlandı (yakında — 23 Kas 12:30). Metin lisansı [CC BY 4.0](../LICENSE) (Cunda AI); README'den alınan komut parçaları kendi repolarının lisansına tabidir.*
