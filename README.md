# Kling Motion Control API (`kling-v2-6-motion-control`)

<!-- conv-kit:v1 -->

<p align="center">
  <img src="assets/badges/price.svg" alt="observed unit price"> <img src="assets/badges/billing.svg" alt="billing model"> <img src="assets/badges/compat.svg" alt="OpenAI-compatible endpoint">
</p>

<p align="center">
  <img src="assets/01-product-ad-ceramic-mug-thumb.jpg" width="820" alt="Sample video frame generated through APIMart">
</p>

> **from $0.0571 per second** on the APIMart route — satu endpoint yang kompatibel dengan OpenAI di `https://api.apimart.ai/v1`, tanpa paket bulanan. *(diamati 2026-10-12)*

**[Dapatkan API key](https://go.apimart.ai/k-8ea394)** · **[Harga live](https://go.apimart.ai/k-fe99ee)** · **[Halaman model](https://go.apimart.ai/k-d61522)** · [⚡ Quickstart 60 detik](#quickstart)

**Alasan tim memakai Kling Motion Control (`kling-v2-6-motion-control`) lewat APIMart**

- **Satu key untuk seluruh katalog.** base URL `https://api.apimart.ai/v1` dan header `Authorization` yang sama menjangkau Kling Motion Control (`kling-v2-6-motion-control`) plus 300+ model gambar, video, dan bahasa lainnya — cukup ganti field `model`, bukan klien Anda.
- **Minimum $1, bayar sesuai pemakaian.** Tanpa langganan dan tanpa paket prabayar yang harus dihitung di awal: isi saldo mulai $1 dan pakai untuk panggilan.
- **Biaya dikembalikan di respons.** Setiap task yang selesai melaporkan jumlah yang ditagih (`cost` / `credits_cost`); task berstatus `failed` dikembalikan dananya.
- **Asinkron sejak awal.** Kirim, ambil `task_id`, polling `GET /v1/tasks/{id}` — batching dan retry hanyalah pekerjaan antrean biasa, bukan integrasi khusus.

<!-- /conv-kit:v1 -->



Kling Motion Control transfers motion from a reference video to your image with Kling V2.6 or V3.

## Model id and routes

| Route | `model` value | Billing | Notes |
| --- | --- | --- | --- |
| Text-to-video / image-to-video | `kling-v2-6-motion-control` | per generated second, by resolution | see the [model page](https://go.apimart.ai/k-d61522) for the exact tiers |

Endpoint: `POST https://api.apimart.ai/v1/videos/generations`, then poll `GET /v1/tasks/{task_id}`.

## Pricing

<!-- pricing:model:start -->
| Resolution | List price | Effective price |
| --- | --- | --- |
| default | 0.0714 | 0.05712 |


<!-- pricing:model:end -->

Prices are a snapshot; the [pricing page](https://go.apimart.ai/k-fe99ee) and [`data/model.json`](data/model.json) are refreshed by CI, and a completed task reports the exact amount in its `cost` field.

## Request parameters

| Field | Type | Default | Notes |
| --- | --- | --- | --- |
| `model` | string | required | the model id shown in the route table above |
| `prompt` | string | required | the shot description; camera and subject motion belong here |
| `duration` | integer | route default | clip length in seconds — billing is per generated second |
| `resolution` | string | route default | 480P / 720P / 1080P or the route’s own tiers |
| `size` / `aspect_ratio` | string | `16:9` | output framing |
| `image_urls` | string[] | — | first frame, first + last frame, or style references, where supported |
| `seed` | integer | — | fix the seed to reproduce a take |

The table lists the fields accepted by the APIMart `videos/generations` endpoint. Check the [model page](https://go.apimart.ai/k-d61522) for anything this model adds on top.

## Quickstart

```bash
curl --request POST --url https://api.apimart.ai/v1/videos/generations \
  --header "Authorization: Bearer $APIMART_API_KEY" --header 'Content-Type: application/json' \
  -d '{"model":"kling-v2-6-motion-control","prompt":"A bamboo forest at sunrise, slow push-in","duration":5,"resolution":"720P","size":"16:9"}'
```

```python
import os, time, requests

BASE = "https://api.apimart.ai/v1"
HEADERS = {"Authorization": f"Bearer {os.environ['APIMART_API_KEY']}", "Content-Type": "application/json"}

created = requests.post(f"{BASE}/videos/generations", headers=HEADERS, timeout=60, json={
    "model": "kling-v2-6-motion-control", "prompt": "A bamboo forest at sunrise, slow push-in", "duration": 5,
}).json()
task_id = created["data"]["id"]

while True:
    task = requests.get(f"{BASE}/tasks/{task_id}", headers=HEADERS, timeout=60).json()["data"]
    if task["status"] in ("completed", "failed"):
        break
    time.sleep(5)
print(task.get("cost"), task["result"]["videos"][0]["url"])
```

Runnable versions: [`examples/`](examples). The task lifecycle is `pending → processing → completed | failed`.

## FAQ

**Apa id model Kling Motion Control?**

`kling-v2-6-motion-control`. Lihat tabel rute di atas.

**Bagaimana penagihannya?**

**from $0.0571 per second** on the APIMart route. Task yang selesai melaporkan jumlah tepatnya di field `cost`; task yang gagal dikembalikan dananya.

**How do I get a key?**

Use the [Get an API key](https://go.apimart.ai/k-8ea394) link — $1 minimum top-up, no plan required.

<!-- related-repos:auto -->
## Related repositories

- **Same model, this language** — the other half of this pair: [kling-motion-control-prompts-id](https://github.com/piyp6nb9kitofl/kling-motion-control-prompts-id)
- **Same model, other languages** — 10 more language builds: English [kling-motion-control-api](https://github.com/apimartbv/kling-motion-control-api) · 中文 [kling-motion-control-api-cn](https://github.com/apimartbwktto3/kling-motion-control-api-cn) · Deutsch [kling-motion-control-api-de](https://github.com/i-jko4u/kling-motion-control-api-de) · Español [kling-motion-control-api-es](https://github.com/o7qx530ue48k/kling-motion-control-api-es) · Français [kling-motion-control-api-fr](https://github.com/apimartbwvl/kling-motion-control-api-fr) · Relay/gateway `kling-motion-control-api-gateway` · 日本語 [kling-motion-control-api-ja](https://github.com/apimartby6kb1/kling-motion-control-api-ja) · 한국어 [kling-motion-control-api-ko](https://github.com/apimartc/kling-motion-control-api-ko) · Português [kling-motion-control-api-pt](https://github.com/l0igvr8/kling-motion-control-api-pt) · Русский `kling-motion-control-api-ru`
- **More models in this language** — [midjourney-api-id](https://github.com/cvrwq0/midjourney-api-id) · [gemini-2-5-flash-image-preview-api-id](https://github.com/gmrg0s/gemini-2-5-flash-image-preview-api-id) · [nano-banana-api-id](https://github.com/vsa185x9ecrd/nano-banana-api-id) · [gemini-3-1-flash-image-preview-api-id](https://github.com/p61qrgpg/gemini-3-1-flash-image-preview-api-id) · [nano-banana-2-api-id](https://github.com/n1rf4ezzz/nano-banana-2-api-id) · [gemini-3-pro-image-preview-api-id](https://github.com/s78ve5v4x9ji2/gemini-3-pro-image-preview-api-id)
- **Topic hubs** — every build of this model, all languages: [kling-motion-control](https://github.com/topics/kling-motion-control) · [kling-motion-control-api](https://github.com/topics/kling-motion-control-api)

This repository is one build in a cross-linked set of API guides for the same model family; each language build links back to the others.
<!-- /related-repos:auto -->
## Attributed links (how this repository is measured)

| Purpose | Attributed link | Target |
| --- | --- | --- |
| Model page | <https://go.apimart.ai/k-d61522> | `apimart.ai/id/model/kling-v2-6-motion-control` |
| Live pricing | <https://go.apimart.ai/k-fe99ee> | `apimart.ai/id/pricing` |
| Get an API key | <https://go.apimart.ai/k-8ea394> | `apimart.ai/id/keys` |

Outbound APIMart links are minted through the promo link API; hand-made tracking parameters are rejected by
`tools/check_links.py` in CI.

## Disclosure

Kling Motion Control is a third-party model served through APIMart; this repository documents how to call it and publishes
model ids and prices, and does not claim official status. Model names, prices and documentation belong to their
respective owners.

## License

MIT — see [LICENSE](LICENSE).
