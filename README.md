# PayPilot — Smart Payments & Checkout Advisor

A proof-of-concept built for the PayMe Product home assignment.

**Live demo:** _(add your GitHub Pages link here after you enable Pages — see below)_

PayPilot is a lightweight advisor for Israeli SMB e-commerce merchants. A store
owner enters a short profile (platform, monthly volume, average order value,
current payment provider, whether they sell abroad, and their main pain points),
and PayPilot returns:

- a **diagnosis** of what's most likely hurting them,
- a **recommended payment provider**, scored and weighted to their profile,
- an **estimated annual fee saving**,
- an **estimated recoverable international revenue**, and
- a tailored **checkout-fix checklist**.

It is bilingual (Hebrew / English, RTL-aware) and runs entirely in the browser —
no backend, no API keys, nothing stored.

---

## The opportunity (why this, from the data)

The dataset is a WhatsApp export of an Israeli e-commerce owners' group:
**~25,600 messages over ~20 months, ~970 participants** (store owners plus the
ecosystem of developers, couriers, designers and lawyers around them).

After clustering the conversation, payments is one of the largest and most
commercially relevant themes — and it breaks into a tight set of recurring,
expensive problems:

1. **Provider-selection paralysis.** Endless "which acquirer should I use / is it
   worth switching?" with no objective answer. Providers mentioned hundreds of
   times each (bit, PayPlus, PayPal, Tranzila, Grow, Meshulam, Cardcom).
2. **Fees eating margin** (~150 mentions). e.g. _"the glitches aren't worth
   PayPlus's insane fees."_
3. **International checkout friction** (~260 "international" + ~80 "English" +
   ~30 "ID number"). The canonical quote: _"Meshulam is very inconvenient for
   overseas customers — Hebrew-only UI, requires an Israeli ID. Other than
   PayPal, what are the alternatives?"_ Some merchants run **two separate
   websites** as a workaround — pure lost revenue at checkout.
4. **Failed payments & reconciliation.** Including a real complaint about a
   transaction that failed while the platform marked it "paid," and the money was
   never transferred.
5. **Fraud / chargebacks** (~150 mentions) — 3DS, tourist-vs-Israeli card risk.
6. **Setup / integration / migration** pain across WooCommerce, Shopify and Wix.

PayPilot targets the highest-value intersection of these: **choosing the right
provider, cutting fee waste, and stopping international checkout leakage** — all
squarely in PayMe's domain.

---

## How the "AI" works in this POC

The recommendation is produced by a transparent, **client-side rules engine**:

- Each provider carries scores (1–5) across the dimensions that surfaced in the
  data: fees, international readiness, integration, fraud protection, service.
- The user's stated pains and profile **re-weight** those dimensions, and
  providers are penalized when they're not international-ready but the merchant
  sells abroad.
- Fee savings and recoverable international revenue are computed from the
  merchant's volume using simple, explicit formulas (see `index.html`).
- A templated natural-language layer composes the diagnosis and the "why it fits"
  rationale in the selected language.

This keeps the POC free, fast and key-less. In production the same surface would
be backed by a live provider-rates feed and an LLM explanation layer (see below).

> ⚠️ **Provider attributes and fee figures in this prototype are illustrative
> placeholders**, not quoted commercial rates. They exist to demonstrate the
> product concept and should be validated against real data before any real use.

---

## Run it locally

It's a single static file. Either:

- Double-click `index.html`, or
- Serve it: `python3 -m http.server` then open `http://localhost:8000`.

No build step, no dependencies.

---

## Deploy free on GitHub Pages

1. Create a **public** repository on your personal GitHub account.
2. Upload these files (drag-and-drop in the browser works).
3. Go to **Settings → Pages**, set **Source: Deploy from a branch**, branch
   `main`, folder `/ (root)`, and Save.
4. After a minute your live link appears (e.g.
   `https://<your-username>.github.io/<repo-name>/`). Paste it at the top of this
   README and into your submission.

---

## Production roadmap (what I'd build next)

- **Live rates & capability feed** per provider, replacing the placeholders.
- **LLM explanation layer** for richer, personalized diagnoses and Q&A.
- **Real checkout scan**: read a store URL and detect Hebrew-only checkout,
  missing wallets, the ID-number field, and missing English country list.
- **Provider integration handoff** — one-click move to the recommended provider.
- **Telemetry** for the success metrics below.

## How success would be measured

- Activation: % of merchants who complete a profile and view a recommendation.
- **Provider conversion**: % who switch/adopt the recommended provider.
- **Fee savings realized** and **international conversion uplift** post-fix.
- Retention: merchants returning to re-scan checkout after changes.

---

_PayPilot is a take-home proof of concept built from analysis of real Israeli
e-commerce merchant conversations. Not affiliated with any payment provider._
