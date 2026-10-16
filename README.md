# Wan 2.7 Image Pro API (`wan2.7-image-pro`)

<!-- conv-kit:v1 -->

<p align="center">
  <img src="assets/badges/price.svg" alt="observed unit price"> <img src="assets/badges/billing.svg" alt="billing model"> <img src="assets/badges/compat.svg" alt="OpenAI-compatible endpoint">
</p>

<p align="center">
  <img src="assets/01-product-hero-perfume.jpg" width="820" alt="Sample render via the APIMart image API">
</p>

> **from $0.0544 per image** on the APIMart route — one OpenAI-compatible endpoint at `https://api.apimart.ai/v1`, no monthly plan required. *(observed 2026-10-12)*

**[Get an API key](https://go.apimart.ai/k-a595ad)** · **[Live pricing](https://go.apimart.ai/k-6dabf2)** · **[Model page](https://go.apimart.ai/k-74b6a7)** · [⚡ 60-second quickstart](#quickstart)

**Why teams call Wan 2.7 Image Pro (`wan2.7-image-pro`) through APIMart**

- **One key, entire catalog.** The same `https://api.apimart.ai/v1` base URL and `Authorization` header reach Wan 2.7 Image Pro (`wan2.7-image-pro`) and 300+ other image, video and language models — switch the `model` field, not your client.
- **$1 minimum, pay as you go.** No subscription and no prepaid plan to size up front: top up from $1 and spend it on calls. There is no free quota to burn through first, so the price in this table is the price you pay.
- **The charge comes back in the response.** Every call reports the amount billed (`cost` / `credits_cost`), so a spend number is read per call instead of guessed at month end.
- **Async by design.** Submit, take the `task_id`, poll `GET /v1/tasks/{id}` — batching and retries are ordinary queue work, not a bespoke integration.

<!-- /conv-kit:v1 -->



Wan 2.7 Image Pro (wan2.7-image-pro) is Wan's flagship text-to-visual model in Alibaba's Wan lineup. Create stunning outputs with a top-tier Wan engine. The Wan API supports fast Wan 2.7 Image Pro generation, and pricing keeps Wan 2.7 Image Pro generation workflows affordable for creators across the Wan community.

## Model id and routes

| Route | `model` value | Billing | Notes |
| --- | --- | --- | --- |
| Per-image | `wan2.7-image-pro` | per delivered image, by resolution | see the [model page](https://go.apimart.ai/k-74b6a7) for the exact tiers |

Endpoint: `POST https://api.apimart.ai/v1/images/generations`, then poll `GET /v1/tasks/{task_id}`.

## Pricing

<!-- pricing:model:start -->
| Output | List price | Effective price |
| --- | --- | --- |
| default | 0.068 | 0.0544 |


<!-- pricing:model:end -->

Prices are a snapshot; the [pricing page](https://go.apimart.ai/k-6dabf2) and [`data/model.json`](data/model.json) are refreshed by CI, and a completed task reports the exact amount in its `cost` field.

## Request parameters

| Field | Type | Default | Notes |
| --- | --- | --- | --- |
| `model` | string | required | the model id shown in the route table above |
| `prompt` | string | required | what to generate, or what to change when editing |
| `size` | string | `auto` / `1:1` | aspect ratio, or `auto` where the route supports it |
| `resolution` | string | `1K` | output resolution tier — cost scales with it |
| `n` | integer | `1` | number of images per task, where the route allows more than one |
| `image_urls` | string[] | — | reference images as public HTTP(S) URLs or base64 data URLs |
| `nsfw_check` | boolean | `false` | runs `omni-moderation-latest` before submitting |

The table lists the fields accepted by the APIMart `images/generations` endpoint. Check the [model page](https://go.apimart.ai/k-74b6a7) for anything this model adds on top.

## Quickstart

```bash
curl --request POST --url https://api.apimart.ai/v1/images/generations \
  --header "Authorization: Bearer $APIMART_API_KEY" --header 'Content-Type: application/json' \
  -d '{"model":"wan2.7-image-pro","prompt":"A bamboo forest at sunrise, slow push-in","size":"1:1","resolution":"1K"}'
```

```python
import os, time, requests

BASE = "https://api.apimart.ai/v1"
HEADERS = {"Authorization": f"Bearer {os.environ['APIMART_API_KEY']}", "Content-Type": "application/json"}

created = requests.post(f"{BASE}/images/generations", headers=HEADERS, timeout=60, json={
    "model": "wan2.7-image-pro", "prompt": "A bamboo forest at sunrise, slow push-in",
}).json()
task_id = created["data"]["id"]

while True:
    task = requests.get(f"{BASE}/tasks/{task_id}", headers=HEADERS, timeout=60).json()["data"]
    if task["status"] in ("completed", "failed"):
        break
    time.sleep(5)
print(task.get("cost"), task["result"]["images"][0]["url"])
```

Runnable versions: [`examples/`](examples). The task lifecycle is `pending → processing → completed | failed`.

## FAQ

**What is the Wan 2.7 Image Pro model id?**

`wan2.7-image-pro`. See the route table above.

**How is it billed?**

**from $0.0544 per image** on the APIMart route. The completed task reports the exact amount in its `cost` field; a failed task is refunded.

**How do I get a key?**

Use the [Get an API key](https://go.apimart.ai/k-a595ad) link — $1 minimum top-up, no plan required.

<!-- related-repos:auto -->
## Related repositories

- **Same model, this language** — the other half of this pair: [wan-2-7-image-pro-prompts](https://github.com/u3cdzay/wan-2-7-image-pro-prompts)
- **Same model, other languages** — 10 more language builds: 中文 `wan-2-7-image-pro-api-cn` · Deutsch `wan-2-7-image-pro-api-de` · Español [wan-2-7-image-pro-api-es](https://github.com/pht10ui5daic/wan-2-7-image-pro-api-es) · Français `wan-2-7-image-pro-api-fr` · Relay/gateway `wan-2-7-image-pro-api-gateway` · Bahasa [wan-2-7-image-pro-api-id](https://github.com/g8aowr0uot/wan-2-7-image-pro-api-id) · 日本語 [wan-2-7-image-pro-api-ja](https://github.com/jhz35cfz/wan-2-7-image-pro-api-ja) · 한국어 `wan-2-7-image-pro-api-ko` · Português [wan-2-7-image-pro-api-pt](https://github.com/gvq5ox1pycj/wan-2-7-image-pro-api-pt) · Русский [wan-2-7-image-pro-api-ru](https://github.com/jsgotg/wan-2-7-image-pro-api-ru)
- **More models in this language** — [grok-image-api](https://github.com/hpcsd0apt/grok-image-api) · [minimax-h3-api](https://github.com/apimartchlp3td/minimax-h3-api) · [minimax-hailuo-02-api](https://github.com/hnqqarqc5jlt/minimax-hailuo-02-api) · [hailuo-02-api](https://github.com/apimartbdiseyt/hailuo-02-api) · [flux-3-video-api](https://github.com/apimarta1akronfb/flux-3-video-api) · [grok-video-api](https://github.com/apimartb63/grok-video-api)
- **Topic hubs** — every build of this model, all languages: [wan-2-7-image-pro](https://github.com/topics/wan-2-7-image-pro) · [wan-2-7-image-pro-api](https://github.com/topics/wan-2-7-image-pro-api)

This repository is one build in a cross-linked set of API guides for the same model family; each language build links back to the others.
<!-- /related-repos:auto -->

## Attributed links (how this repository is measured)

| Purpose | Attributed link | Target |
| --- | --- | --- |
| Model page | <https://go.apimart.ai/k-74b6a7> | `apimart.ai/model/wan-2-7-image-pro` |
| Live pricing | <https://go.apimart.ai/k-6dabf2> | `apimart.ai/pricing` |
| Get an API key | <https://go.apimart.ai/k-a595ad> | `apimart.ai/keys` |

Outbound APIMart links are minted through the promo link API; hand-made tracking parameters are rejected by
`tools/check_links.py` in CI.

## Disclosure

Wan 2.7 Image Pro is a third-party model served through APIMart; this repository documents how to call it and publishes
model ids and prices, and does not claim official status. Model names, prices and documentation belong to their
respective owners.

## License

MIT — see [LICENSE](LICENSE).
