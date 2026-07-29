# NOFA Autonomous Client Delivery System™ — Frontend Prototype

A single-page, fully client-side prototype that simulates how **NOFA AI Factory™** automatically onboards a client, configures their selected AI product, converts them into a subscriber, and routes custom work to the AI Architect — with **Slack demand signals** for every prototype selection.

> **No backend required.** All logic runs in the browser with vanilla JavaScript.

---

## 🎯 What It Demonstrates

```
Select AI Product  →  Judy Onboarding Chat  →  Classification Engine
        ↓                                        ↓
  (live catalog)                    ✅ Standard Flow      🔥 Custom Flow
                                    auto setup animation   summary + escalate
                                        ↓                      ↓
                                  Subscription checkout    Slack alert → Farhad
                                        ↓                      ↓
                                    └──────────►  Client Dashboard  ◄──────────┘
```

---

## ✨ Features

### 1. Live Product Catalog
- Product list loads **live** from `https://www.nofaaifactory.com/api/products` on every page load — new products appear automatically (CORS-enabled API)
- Full snapshot of all current products embedded as an **offline fallback**
- Search filter, status badges (✅ Live SaaS · 🚀 Live Prototype · 🏷 Prototype Ready · 🏗 In Development), and "Learn More" links to real product pages

### 2. Judy AI Onboarding Chat
- Simulated AI typing animation and chat bubbles
- Judy **announces the product's real status** from the catalog (e.g. *"currently a Live Prototype — you're getting early access"*)
- Judy **provides the Stripe link** (`stripe.com`) instead of interrogating the client
- Quick-reply chips for onboarding answers

### 3. Classification Engine
```js
function classifyRequest(answers) {
  if (
    answers.stripe === "yes" &&
    answers.affiliates === "yes" &&
    answers.customRules === "no"
  ) {
    return "standard";
  } else {
    return "custom";
  }
}
```

### 4. ✅ Standard Flow — Self-Serve to Subscriber
- Animated setup sequence: Connecting Stripe ✔ → Creating commission rules ✔ → Initializing dashboard ✔
- **Subscription step**: 3 plans (Starter $49 · Professional $99 · Business $199 /mo)
- Simulated secure checkout (test card pre-filled, processing animation, success state)
- Dashboard shows **Active** status + live subscription card with next billing date

### 5. 🔥 Custom Flow — Escalation with Slack Alert
- Onboarding summary review
- "Escalate to AI Architect" → request routed to **Farhad**
- **Slack notification fired directly from the browser** (no backend) with tool, answers, route, and timestamp
- Client sees "custom quote pending" on the dashboard

### 6. 🚀 Prototype Demand Signals
- Selecting any product that is **not Live SaaS** instantly alerts Farhad via Slack:
  > 🚀 *Prototype demand signal* — Tool, Status, "candidate to prioritize for full SaaS build"
- Every client click becomes **market validation data**

### 7. Client Workspace (auto-provisioned)
Every client immediately gets a full digital workspace — no more "where are we?" emails:
- **📊 Current Status** — tool, status badge, route, subscription, live activity log
- **🚀 Deployment Progress** — visual timeline with % complete (real states from the flow)
- **✅ Tasks** — auto-generated from the route (custom → quote approval; standard → plan selection)
- **📄 Documents** — generated on demand from live data: Onboarding Summary, Deployment Report, Subscription Agreement / Custom Quote Request (downloadable .txt)
- **💬 Messages** — notification feed + one-click live chat with Judy
- **📈 Weekly Reports** — usage stats + activity chart (simulated)
- **🧠 AI Conversation History** — the complete Judy onboarding transcript, captured live and persisted
- **🛟 Support** — Judy chat, support desk email, escalation info, response times

### 8. State Persistence
- Full session saved to `localStorage` — reload restores the dashboard
- "Start New Onboarding" resets the flow

---

## 🧪 Demo Scenarios

| Scenario | Path | Result |
|---|---|---|
| **Fully automated subscriber** | Select a Live SaaS tool → Stripe ✔ → Yes affiliates → No custom rules | Standard Flow → plan → checkout → **Active** |
| **Custom route** | Answer **Yes** to "custom commission rules" | Custom Flow → escalate → Slack alert → **Pending Customization** |
| **Prototype demand signal** | Select any 🚀/🏷/🏗 product | Slack alert fires on selection |

---

## ⚙️ Configuration

All config constants are at the top of the `<script>` block in `index.html`:

| Constant | Value | Notes |
|---|---|---|
| `JUDY_URL` | `https://judyva.vercel.app/?tenant=NOFA-Business-Consulting` | Live Judy assistant |
| `CATALOG_API` | `https://www.nofaaifactory.com/api/products` | Live product feed |
| `SLACK_WEBHOOK_URL` | `''` (empty = simulated mode) | Paste your Slack Incoming Webhook URL to enable real alerts |

### Enabling real Slack alerts
1. Go to [api.slack.com/apps](https://api.slack.com/apps) → **Create New App** → From scratch
2. **Incoming Webhooks** → On → **Add New Webhook to Workspace** → pick your channel
3. Copy the `https://hooks.slack.com/services/...` URL into `SLACK_WEBHOOK_URL`

> Slack alerts post via `fetch(..., { mode: 'no-cors' })` — works from the browser with no server. Until configured, alerts run in **simulated mode** and are clearly labeled as such in the UI and activity log.

---

## 🗂 File Structure

```
app/
├── index.html   # The entire prototype (UI + logic + fallback catalog)
└── README.md    # This file
```

**Tech stack:** Pure HTML · TailwindCSS (CDN) · Vanilla JavaScript · No build step · No frameworks

---

## 🔗 Deep Links from the Showroom

The app accepts a `?tool=` URL parameter so showroom links can drop a client **directly into onboarding** — no re-selection:

```
/?tool=dlyn          → Dlyn-AI™
/?tool=affiliate     → AffiliateLedger AI™
/?tool=commanddesk   → CommandDesk AI™
/?tool=techsupport   → TechSupport AI™
/?tool=<any-slug>    → any catalog product (e.g. ?tool=freshroute-ai)
```

**Load priority:** ① URL tool context (fresh session) → ② saved workspace (returning user) → ③ tool selection screen (new visitor). The "New Onboarding" button clears storage and reloads the base URL.

---

## 🚀 Running It

Open `index.html` in any browser — or deploy the folder as a static site (Vercel, Netlify, GitHub Pages).

---

## 🛣 Roadmap (Production Build)

- [ ] **Firebase** — Auth + Firestore for real client accounts and multi-tenancy
- [ ] **Stripe Billing** — real subscriptions, webhook-driven provisioning
- [ ] **Slack webhook** — activate live alerts (config ready)
- [ ] **CommandDesk AI™** — execute provisioning workflows in production
- [ ] Per-tool onboarding question sets driven by catalog metadata
- [ ] Weekly report emails + real analytics

---

**NOFA AI Factory™** · [nofaaifactory.com](https://nofaaifactory.com) · Questions? supportdesk@nofabusinessconsulting.com
