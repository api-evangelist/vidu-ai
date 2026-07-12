# Vidu (vidu-ai)

Vidu is a generative video AI platform from **Shengshu Technology** (ShengShu / 生数科技), built on the company's U-ViT diffusion-transformer architecture. The Vidu API turns text prompts, still images, and reference subjects into short video clips - text-to-video, image-to-video, reference-to-video (multi-entity / character consistency), start-and-end frame interpolation, and video upscaling.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/vidu-ai/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/vidu-ai/refs/heads/main/apis.yml)

## Access Model (be honest about it)

- **Open access, no application required.** Individual developers and businesses can buy prepaid credits and start calling the API immediately - no high-tier subscription gate.
- **Prepaid, credit-based pricing.** Launch base price ~USD $0.05/credit with a minimum purchase around $10. A four-second clip runs roughly 4-40 credits depending on the feature, model tier, resolution, and aspect ratio. Off-peak mode trades a longer queue window for lower credit cost.
- **Asynchronous create-then-poll.** Every generation endpoint is a `POST` that returns a `task_id` in a `created` state. You then poll `GET /ent/v2/tasks/{id}/creations` for `state = success` (or supply a `callback_url` that Vidu calls on completion). There is **no public WebSocket** - see `review.yml`.
- **Auth.** API key in the `Authorization` header using the token scheme: `Authorization: Token vda_xxx` (not standard HTTP Bearer).
- **Grounding & honesty.** Endpoint paths, HTTP methods, the base URL (`https://api.vidu.com/ent/v2`), model names, auth scheme, and the async pattern are grounded in the public Vidu platform docs. The OpenAPI models request/response **field schemas** from documentation prose (flagged `x-modeled`); the **Upscale** request body in particular is modeled and should be reconciled against the live reference before production use.

## Base URL

`https://api.vidu.com/ent/v2`

## APIs

### Vidu Text-to-Video API

Generate a video from a text prompt alone. `POST /ent/v2/text2video` with a model (viduq3-turbo, viduq3-pro, viduq2, viduq1), prompt, duration, resolution, and aspect ratio.

- **Documentation:** [https://platform.vidu.com/docs/text-to-video](https://platform.vidu.com/docs/text-to-video)
- **Base URL:** `https://api.vidu.com/ent/v2`
- **Tags:** Text-to-Video, Video Generation, Generative AI

### Vidu Image-to-Video API

Animate a single still image into a video clip. `POST /ent/v2/img2video` with a model, one input image (URL or Base64; PNG/JPEG/JPG/WebP up to 50MB), optional prompt, duration, and resolution.

- **Documentation:** [https://platform.vidu.com/docs/image-to-video](https://platform.vidu.com/docs/image-to-video)
- **Base URL:** `https://api.vidu.com/ent/v2`
- **Tags:** Image-to-Video, Video Generation, Animation

### Vidu Reference-to-Video API

Keep one to seven supplied subjects visually consistent across the clip (Vidu's multi-entity consistency). `POST /ent/v2/reference2video` with reference images plus a prompt.

- **Documentation:** [https://platform.vidu.com/docs/reference-to-video](https://platform.vidu.com/docs/reference-to-video)
- **Base URL:** `https://api.vidu.com/ent/v2`
- **Tags:** Reference-to-Video, Character Consistency, Multi-Entity

### Vidu Start-End Frame API

Generate a video that transitions between a supplied first frame and last frame. `POST /ent/v2/start-end2video` with two images and an optional prompt.

- **Documentation:** [https://platform.vidu.com/docs/start-end-to-video](https://platform.vidu.com/docs/start-end-to-video)
- **Base URL:** `https://api.vidu.com/ent/v2`
- **Tags:** Start-End Frame, Interpolation, Video Generation

### Vidu Upscale API

Upscale a previously generated Vidu video to a higher resolution (Upscale Pro). `POST /ent/v2/upscale`. Request/response bodies here are modeled from public docs and should be reconciled on integration.

- **Documentation:** [https://docs.platform.vidu.com/7207545m0](https://docs.platform.vidu.com/7207545m0)
- **Base URL:** `https://api.vidu.com/ent/v2`
- **Tags:** Upscale, Super Resolution, Video Enhancement

### Vidu Task Query API

Retrieve the status and results of an asynchronous task. `GET /ent/v2/tasks/{id}/creations` returns the task id, state (created / queueing / processing / success / failed), credits used, and a creations array of finished video URLs and cover images.

- **Documentation:** [https://docs.platform.vidu.com/7207545m0](https://docs.platform.vidu.com/7207545m0)
- **Base URL:** `https://api.vidu.com/ent/v2`
- **Tags:** Task Status, Polling, Async

## Tags

- Video Generation
- AI Video
- Generative AI
- Text-to-Video
- Image-to-Video
- Reference-to-Video
- U-ViT
- Diffusion

## Common Properties

- [Authentication](authentication/vidu-ai-authentication.yml)
- [GitHub Organization](https://github.com/shengshu-ai)
- [LinkedIn](https://www.linkedin.com/company/shengshu-technology)
- [Website](https://www.vidu.com)
- [Documentation](https://platform.vidu.com/docs)
- [Plans](plans/vidu-ai-plans-pricing.yml)
- [Rate Limits](rate-limits/vidu-ai-rate-limits.yml)
- [Fin Ops](finops/vidu-ai-finops.yml)

## Timestamps

- **Created:** 2026-07-11
- **Modified:** 2026-07-11

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
