# Clienteling AI — Agentic Commerce Demo

A white-labeled, generalized build of the agentic personalization storefront, branded
**Clienteling AI** for a global ecommerce audience. Functionally identical to the original
solution — only the branding has changed.

> This is a **separate** build. Brand strings and local-storage keys have been renamed so it
> will not collide with or be confused for the original ASO demo (you can even run both in the
> same browser without cart/session cross-talk).

---

## What's in here

| File | What it is |
|------|------------|
| `App.jsx` | The full React storefront (drop into your Vite app's `src/App.jsx`). |
| `worker.js` | The Cloudflare Worker proxy (AI, email, Shopify Admin, agent state/cron). |
| `Clienteling-AI-one-pager.html` | A print-ready solution overview for sharing / leave-behind. |

## What it does (unchanged from the original)

- **Persona-aware browse** — same catalog re-orders per shopper; hero, copy, and assistant tone re-curate live.
- **Agent-driven UI** — the assistant searches, filters, adds/removes from cart, and checks out from a prompt.
- **Fulfillment-aware cart/PDP** — delivery promise from the shopper's stored ZIP → nearest store → live inventory.
- **Autonomous lifecycle agent** — in-session + server-side cron continue abandonment recovery even after the tab closes; one-click incentive accept applies instantly on return.
- **Disruption recovery** — delay/cancel scenarios resolve to in-store pickup or an AI-drafted alternate, with one-click cancel-and-reorder write-back.
- **Two-way loop** — in-app orders write back to the commerce backend; inventory decrements; history grows; the next interaction is richer.
- **Real-time self-signup** — creates a real customer on the commerce backend; personalization starts on the first click.

## Branding changes applied

- Wordmark: `Academy` + `Sports + Outdoors` → `Clienteling` + `AI`
- All AI system prompts now describe **"Clienteling AI, a modern retail brand"**
- All email signatures → **"The Clienteling AI Team"**; sandbox sender → `Clienteling AI <onboarding@resend.dev>`
- Footer, order notes, worker service name → Clienteling AI
- Local-storage keys: `aso-demo-auth` → `clienteling-auth`, `aso-demo-dynamic-customer` → `clienteling-dynamic-customer`

The three demo personas (their names and category content) are **kept as-is** — they are the
personalization *demonstration*. If you want a generic catalog/personas for a specific vertical,
that's a content pass on top of this build.

## Deploy

**Storefront** (Vite + GitHub Pages or any static host):
```bash
cp App.jsx <your-app>/src/App.jsx
# build & deploy your Vite app as usual
```

**Worker** (Cloudflare):
```bash
cp worker.js <your-worker>/worker.js
npx wrangler deploy
```

### Worker configuration (env vars / secrets)
- `ANTHROPIC_API_KEY` — AI proxy
- `DEEPGRAM_API_KEY` — voice transcription
- `SHOPIFY_ADMIN_TOKEN` — order + customer write-back (needs `write_orders`, `write_customers`)
- `SHOPIFY_STOREFRONT_TOKEN`, `SHOPIFY_STORE_DOMAIN` — storefront reads
- `RESEND_API_KEY` (+ optional `RESEND_FROM` for a verified sending domain)
- `AGENT_KV` — KV namespace binding for agent state
- A cron trigger (e.g. `*/2 * * * *`) for the lifecycle agent

### Notes for a clean demo
- The lifecycle agent writes cart state to KV **only when the cart changes** (free-tier friendly).
- Sandbox email delivers only to the account owner's verified address until a sending domain is verified.
- The time-warp lever and unauthenticated email accept-links are demo aids; production uses real cadences and signed links.
