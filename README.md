# Vidu (vidu-ai)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

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
