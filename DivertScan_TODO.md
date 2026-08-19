# RANGER DISPATCH — MASTER TO-DO (priority-ordered)

**Last updated: Wednesday, August 19, 2026 — v5. Replaces v4.**
Update the date whenever you change something.

**Status:** Demo at **Build v0.23.0** — SENT TO DEREK Aug 19 by text + email
(from robert@divertscan.com). He confirmed receipt by text; browsed on his
iPhone; his rendering feedback was fixed same-morning (v0.23.0 phone pass).
NOW AWAITING HIS INPUT — his reply is discovery data; forward it raw into the
next Claude chat. THE REPO IS PRODUCTION NOW: the buyer holds the live link,
so no experiments on main; verify the footer stamp after every push.
No backend exists yet.

## 💵 PRICING (DECIDED Aug 19 — do not renegotiate against yourself)
Monthly: **$249 flat**, everything included, texting at pass-through cost,
locked 24 months, month-to-month after 6, full data export any time in
writing. Drop to $199 only if his Box Tracker invoice is under ~$150 all-in.
Build fee: **$7,500 staged** ($2.5k start / $2.5k dispatch+driver+customer
live / $2.5k texting+billing live) — priced low BECAUSE Robert keeps the
platform (Ranger gets a license + their data forever; Jaguar and every
hauler after stay sellable). If Derek wants exclusive code ownership,
that's a different product: $35k+. Kill fee = keep what's paid, he keeps
his data. Session v0.9→v0.23: everything-batch shipped (recurring, rates
mechanism, photos, QR, offline preview, intake parser, suggest w/ undo,
week strip, audit, storefront, Loose Ends, next-best-job, proof-of-service,
fleet & permits, evidence chips, live export dates, phone pass).

**Version discipline:** the build number in the demo footer bumps on EVERY
shipped change (patch = fix, minor = feature). v1.0.0 is reserved for the first
build on a real backend. A screenshot without the current build number is a
stale cache — check before debugging.

---

## 🎯 CRITICAL PATH (in order — everything else waits on these)
1. [ ] **Get a real Box Tracker invoice from Ranger.** Reveals their actual
       per-can bill, text volume, and fleet size. Sets the price ceiling.
2. [ ] **Discovery call** — walk `RangerDispatch_Discovery_Call_Sheet.md`
       (in project knowledge) with the owner AND the dispatcher. The demo has
       reached the edge of what can be built without their answers.
3. [ ] **Commercial terms in writing** before the backend build starts —
       staged build fee + flat monthly; SMS pass-through; kill fee; data-export
       promise (the anti-Box-Tracker clause).
4. [ ] **10DLC registration** — start the SAME WEEK the deal is real (Ranger's
       EIN, notification campaign, 1–3 wk vetting; the long pole).
5. [ ] **Supabase project** (new, never DivertScan's) — schema tenant-ready
       (org_id on every table), RLS roles admin/dispatcher/driver from day one.

## ✅ DEMO — DONE (v0.10.1)
Nine tabs: Board (day-summary chips = filters + map jump, drag-and-drop,
click-assign, ▲▼ re-sequencing, after-cutoff override, date-picked reschedule
grouped by day), Driver (EN/ES, navigate, call office, tap-to-call, timestamps,
tons + tipping fee, TBD can pick from yard, signature pad, site notes that
persist, dry-run REASON CODES, blocked/contaminated flags, bilingual overweight
warning, REAL phone-GPS stamps with labeled can-location fallback), Customer
(branded page, status timelines, history, prohibited-items acknowledgment
gate on every request), Equipment (map — blue trucks / teal compactors /
navy-or-light cans by theme / orange-red = aging only; yard stock; 0-unaccounted
line), Billing (contamination bill-through flow box, export guard announces
held-back lines, CSV + real IIF), Reports (turnover, leakage, nudges with
per-customer mute, DivertScan teaser), Payback (6-lever ROI calculator, all
inputs editable + flagged invented, pays-for-itself line, production framing =
monthly value report), Setup (live-editable rate sheet incl. contamination fee
+ overweight threshold, texting switches + quiet hours, connections & bolt-ons
with 10DLC pipeline + map-fee explainer, CSV/IIF toggle). Day/night theme
(device default, inverted dark map). "What we guessed" panel = the discovery
agenda; every built feature's guess gets REWRITTEN, never deleted.

## 🔑 STANDING RULES (unchanged)
- DivertScan: separate project/repo/Supabase/Pi. Bridge = API only, later.
- No credentials in code, chat, repo, or UI. Twilio configured server-side.
- No real customer data in the public demo. Fake names, 555 numbers.
- LEED reporting stays DivertScan's product. Teaser card only.
- Real-user exposure → DivertScan discipline (backup, branch+PR, verify).
- Software billing and tonnage billing are separate conversations.
- Every AI review: verify specifics against the actual file; discard invented
  dollars, fabricated bugs ("your code is cut off"), and undeliverable
  promises. Reviews are brainstorms, never patches.
- Do NOT pitch Jaguar until Ranger is signed. Hard confidentiality wall
  between the two haulers forever. Per-tenant 10DLC repeats for Jaguar.

## 🟡 BACKEND BUILD (Phase 0/1 — after terms)
- [ ] Supabase schema + RLS roles (admin/dispatcher/driver; Setup = admin-only)
- [ ] Auth (dispatcher login; driver = magic link or PIN; customer = token)
- [ ] Orders/cans/sites/customers CRUD; yard derives from order history
- [ ] Photo capture + storage on completion (placement / ticket / obstruction)
      → customer-visible history links (dispute killer)
- [ ] Notification engine (event → template → queue), stubbed sends until
      10DLC; quiet-hours release rule (open question: release at quiet-end or
      morning batch?)
- [ ] Multi-dispatcher realtime (Supabase Realtime) + change log
- [ ] Weight-ticket photo + OCR-proposes-driver-confirms (never auto-binds)

## 🔵 DISCOVERY-GATED (built only after Ranger answers)
Per-customer rate sheets · two contacts per account (payer vs super — who gets
which text) · signature required rules by job type · live-load wait billing ·
contamination markup-vs-pass-through policy · per-truck GVW thresholds ·
who may press the cutoff override · default day-6 mutes · booking horizon /
tomorrow pre-builds · offline mode (dead zones? Sherman routes say ask hard) ·
dry-run reason list validation · prohibited-items list per facility ·
scale-house queue times (which facilities back up, how long — gates the
"Queued at Scale" status + facility turnaround analytics idea) ·
fleet paperwork (real VINs, PM schedule, inspection/registration/insurance
dates) and the actual permit set — which of TxDMV/USDOT, Dallas franchise,
IFTA, TCEQ apply, plus county permits and bonds ·
customer auto-accept for trusted accounts (one-line switch, policy decision)

## 🔴 DECLINED (with reasons, on the record)
- Auto route optimization — naive routing reads dumber than the driver;
  costs credibility everywhere else. Arrows + map = the human version.
- Auto-charge / auto-sync anything money — human review before dollars move.
- QBO live API — Intuit app-review gate; CSV/IIF covers both QuickBooks.
- localStorage-as-offline — real offline = SW + IndexedDB + sync queue or
  nothing.

## 💰 VERIFIED ECONOMICS (for the pitch — never use unverified figures)
Box Tracker: $1.95/dumpster/mo, $0.20/text, $200/mo API, ~$1,000 setup cap,
$50 liability cap, no data preservation on termination (EXTRACT BEFORE
CANCELLING), never log into their instance. Twilio wholesale ≈ $0.013/text
→ texting spread alone can fund the subscription. Robert's floor ≈ $50–90/mo
(Supabase Pro 25 + SMS 15–30 + misc). Google Maps: Navigate deep-links free
forever; embedded Dynamic Maps 10k loads/mo free then $2–7/1k → realistically
$0 at Ranger volume but needs card + domain-locked key; stay on OSM until
signed. Payback demo math at placeholder inputs ≈ $16.5k+/yr — DO NOT quote
until inputs are Ranger's.

## 🧭 PITCH ORDER (decided)
Reports ("money sitting uninvoiced today") → 4 PM cutoff bar → customer
request lands on Board → Driver flow in Spanish → close on Payback with the
owner typing his own numbers. Dispatch board last — least differentiated.

## 👤 BUYER PROFILE — DEREK (Ranger's owner)
Extremely smart, strong business acumen. Strategy: honesty IS the pitch —
lead with the "What we guessed" panel, flagged-invented defaults, and the
open cost register; never quote an unverified number in front of him. Expect
owner-grade questions (margin, bus factor, data ownership, why-not-Docket,
the FEF vs his no-hidden-fees positioning — raise that one YOURSELF before
he does). Expect his son (27, engineer) to do a technical diligence pass:
repo hygiene, no keys anywhere, RLS story must hold. Prep doc:
`RangerDispatch_Derek_Prep.md`. Close by handing HIM the iPad on the
Payback tab — a smart buyer convinces himself with his own numbers.
