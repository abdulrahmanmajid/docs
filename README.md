# Kallabot docs

The public documentation for Kallabot, published at [docs.kallabot.com](https://docs.kallabot.com). Built with [Mintlify](https://mintlify.com) on the official [Mintlify starter](https://github.com/mintlify/starter).

## What's here

| Path | What it is | Who writes it |
| --- | --- | --- |
| `docs.json` | Site config: theme, brand, navigation, navbar, redirects | By hand |
| `index.mdx`, `quickstart.mdx`, `guides/` | **Guides** tab: product docs for business owners | By hand |
| `developers/` | **Developers** tab: API guides | By hand |
| `api-reference/introduction.mdx` | API reference overview page | By hand |
| `api-reference/openapi.json` | The public OpenAPI spec. Mintlify generates every endpoint page from it | **Generated. Never edit by hand.** |
| `changelog.mdx` | **Changelog** tab, one `<Update>` per release | By hand |
| `logo/`, `favicon.svg`, `images/` | Brand assets, copied from `frontend/public/brand/kallabot-kit` | Copied |
| `AGENTS.md` | Writing rules for AI tools working on these docs | By hand |

## Run locally

You need Node.js 20 or later.

```bash
npx mint dev
```

Open [http://localhost:3000](http://localhost:3000). Or install the CLI once with `npm i -g mint` and run `mint dev`.

Before you push:

```bash
npx mint validate        # strict build, including the OpenAPI spec
npx mint broken-links    # every internal link resolves
```

If the preview looks stale, run `mint update`.

## How the API reference is generated

The reference is built from the backend's own route definitions, so it can't drift from what the API does.

1. In the backend repo (`kallabot-backend`), `kallabot/api/public_docs.py` walks the FastAPI app and keeps the routes that are public: their path is in one of its `PUBLIC_GROUPS` (Agents, Calls, Scheduled calls, Web calls, Tools, Knowledge, Numbers, SIP trunks, Inbox, Web chat, Webhooks, Minutes) and their scope gate accepts API keys.
2. For each operation it adds the tag, the `apiKey` bearer security scheme, `x-required-scopes` (read from the route's `scoped(...)` gate), the standard `application/problem+json` error responses and the `RateLimit-*` / `X-Request-Id` headers.
3. Run it from the backend:

   ```bash
   cd backend
   make openapi-public
   ```

   This writes `openapi.public.json` in the backend and, when this repo is checked out next to the backend as `docs/`, copies it to `api-reference/openapi.json` here.
4. Commit the new spec in this repo. Mintlify regenerates every endpoint page on deploy.

Endpoint titles and descriptions come from the route's `summary` and docstring, and field descriptions from the Pydantic models. To improve an endpoint page, change the backend, not this repo.

The API reference tab in `docs.json` points at the spec with a group-level `"openapi": "api-reference/openapi.json"`, so new endpoints appear automatically.

## Writing rules

See [`AGENTS.md`](AGENTS.md). The short version: check every claim against the code, document only what works today, keep guides in plain words, never publish prices.

## Publishing

Pushes to `main` deploy through the Mintlify GitHub app. Work on a branch and open a pull request; Mintlify builds a preview for it.
