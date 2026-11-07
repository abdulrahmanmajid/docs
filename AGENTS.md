> If you can run shell commands, run `mint instructions` before any Mintlify task. It prints current guidance for this CLI version.
> If `mint` is not installed or says "Unknown command", run `npx mint@latest instructions`.

# Documentation project instructions

## About this project

- This is Kallabot's public documentation (docs.kallabot.com), built on [Mintlify](https://mintlify.com).
- Pages are MDX files with YAML frontmatter. Configuration lives in `docs.json`.
- Three hand-written areas and one generated one:
  - `index.mdx`, `quickstart.mdx` and `guides/`: product docs for business owners and office managers.
  - `developers/`: API guides for developers.
  - `changelog.mdx`: one `<Update>` per release, newest first.
  - `api-reference/openapi.json`: **generated** from the backend (`make openapi-public` in `backend/`). Never hand-edit it. Mintlify builds every endpoint page from it.

## Source of truth

The code is the source of truth, not marketing copy.

- Backend routes: `backend/kallabot/api/v1/*.py`. Scopes: `backend/kallabot/security/authorization.py`.
- Errors: `backend/kallabot/api/errors.py`. Webhook events: `backend/kallabot/api/schemas/events.py`.
- Agent configuration: `backend/kallabot/api/schemas/agents.py`.
- Product screens: `frontend/src/app/(main)/(pages)/`.

Check every claim against the code before you write it.

## Terminology

- "workspace", not "organization" or "account" (the API's `organization_id` is the workspace).
- "agent", not "bot" or "assistant".
- "minutes" for usage. Never put prices in the docs; link to https://kallabot.com/pricing.
- "web chat" (two words) for the website widget; "web call" for a browser voice call.
- "API key", not "token" or "secret key". Keys look like `kb_live_…`.
- Support is contact@kallabot.com.

## Style preferences

- Guides: plain words, short sentences, no jargon. The reader is a non-technical owner.
- Developers: precise and copy-paste ready. Every example must match the real request and response shapes.
- Use active voice and second person ("you").
- Use sentence case for headings.
- Bold for UI elements: Click **Settings**.
- Code formatting for file names, commands, paths, headers and fields.

## Content boundaries

- Document only what works end to end today. Do not advertise anything simulated, in preview or unbuilt.
- Campaigns are not documented (the product shows sample data and there is no campaigns API yet).
- Do not document internal or staff-only endpoints (`/v1/admin`, provider callbacks, internal tools).
- Do not publish prices (plans and minute packs are described, prices live on kallabot.com/pricing).
