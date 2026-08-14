# DIVERTSCAN — MASTER TO-DO (priority-ordered)

**Last updated: Thursday, August 13, 2026 (late).** Replaces the July 7 version.
Update the date whenever you change something.

**TWO TRACKS, ONE LIST.** Product work is ordered by risk to a running system.
Commercial work is ordered by Mark's calendar. They are kept apart on purpose —
nothing that could disrupt live capture gets bumped up the queue for a sales
reason. Items that block the deal are tagged **[BLOCKS DEAL]**.

**System status:** Fully operational. Pi captures + syncs. Client portal login
working. Admin client management + login log via passphrase-gated RPCs. Pi health
monitoring live (temp/throttle/disk every 5 min → admin Scale tab widget).

**Two systems, two ways to edit:**
- **Pi / `scale_capture.py`** — `/home/pi/scale_capture.py`, run by
  `scale_capture.service`. Edit via Termius (SSH). Never paste long files into the
  terminal from iPad (drops chunks) — put the file in the repo and `curl` the raw
  URL down, or use Termius SFTP. Multi-line configs: `sudo tee << 'EOF'` heredocs.
- **`index.html`** (~12.1k lines) — Claude edits an uploaded copy and returns the
  full file; upload renamed to `index.html` via Add file → Upload files → new
  branch + PR → merge → verify. ALWAYS back up first. One change at a time.

---

## 🚨 DO THESE BEFORE ANY THIRD-PARTY CUSTOMER TOUCHES THE SYSTEM

These were housekeeping while it was only DalMex data. The moment Metropolitan's
weights are in play they are negligence. Nothing else on this list matters until
these are done.

- [ ] **Back up `scale_capture.py` + `scale_capture.service` to GitHub.
      STILL NOT DONE — has been top of this list since July.** [BLOCKS DEAL]
      Only copy is the Pi's SD card, which has died before. You cannot sell a
      product whose capture code exists in one place on hardware that has already
      failed. Termius SFTP → iPad Files → GitHub upload. One evening.
- [ ] **Rotate the Supabase anon key.** [BLOCKS DEAL] Hard-coded in four places
      (Pi `scale_capture.py`, Pi `pi_health.py`, `index.html`, `scale.html`) and
      it has appeared in chat. Coordinated pass; Pi buffers so worst case is a
      short sync delay.
- [ ] **Rotate dispatcher tokens.** [BLOCKS DEAL] Leaked in a July 4 screenshot,
      still live. Generate new, redistribute links.
- [ ] **Review the 8 UNRESTRICTED tables/views** — `project_material_t...`,
      `project_summary`, `v_admin_review`, `v_all_drivers`, `v_dispatcher_ro...`,
      `v_driver_logbook`, `v_fleet_tares`, `v_hauler_drivers`. Decide per view
      whether public read is genuinely needed; lock the rest. [BLOCKS DEAL]
- [ ] **Decide tenant separation — and take the cheap option.** [BLOCKS DEAL]
      DivertScan today *is* DalMex's app. Metropolitan's data cannot sit in the
      same Supabase project as a competitor's. Proper multi-tenancy is ~3 months.
      **Clone the stack instead** — separate Supabase project + separate Pages
      site per customer. Ugly at 20 customers, fine at 3, about a week. Write down
      the decision so future-you knows it was deliberate.

## 🔑 STANDING RULES (unchanged)
- CO₂e / carbon = INTERNAL-ONLY. Customer & LEED reports weight-based only.
- Per-project reports LEED-clean; only the internal Portfolio view blends
  LEED + Non-LEED (Hayes = Non-LEED, flagged).
- Hauler approval lives in the `approved_haulers` table — never hard-coded.
- Tickets carry Weigher + Tare Source (Measured/Estimated; standard ~32,540 = Estimated).
- Admin passphrase: never in code, repo, chat, or these instructions.
  Reset in SQL Editor: `select admin_set_passphrase('new one');`
- No passwords, API keys or credentials in code, chat, or this file.

---

# TRACK 1 — PRODUCT

## 📊 WHAT IS SOLD vs WHAT EXISTS (be honest with customers)

| Tier | Price | Built | Gap |
|---|---|---|---|
| Scale House | $350/mo | ~70% | Certificates of destruction, hash chain, public verify |
| Operations | $650/mo | ~0% | Containers, service requests, schedules, draft invoices, close-out, anomaly flags |
| Trading | $950/mo | ~0% | Contracts/rate cards, billing audit, settlement recon, payment matching, index rebates, margin |
| Compliance | +$200/mo | ~0% | Audit trail, ledger hash chain, void/reissue, 6-yr retention |
| Setup | $2,500/site | n/a | One time, on commissioning |

Real today: Pi capture + sync, Pi health, tickets (weigher/tare source), photos +
GPS, diversion + CO₂e, LEED per-project reporting, client portal with login,
multi-ticket OCR import, driver fuzzy match + loadbook, QuickBooks CSV export,
hauler approval table.

**Rule: never sell a tier that isn't built. Date it on the page instead.**

## 🟢 EASY / QUICK WINS
- [ ] Mark averaged-tare DX loads as "Estimated" — one careful SQL UPDATE
      (preview with SELECT first). Tickets with the standard ~32,540 tare.
- [ ] Rename `2_pi_health.py` → `pi_health.py` in the repo.
- [ ] Delete `reset-client.html` from the repo (obsolete).
- [ ] Remove stale `divertscan-capture.service` from repo / project knowledge
      (wrong name + path; real one is `scale_capture.service`).
- [ ] Ops Pulse "Client Logins" tile + notif badge show 0 — still read the locked
      `client_logins` table. Point at a count-only RPC or show "—".

## 🟡 MEDIUM (an evening each)
- [ ] **PDF batch ticket import** — one Adobe Scan PDF → pdf.js page split →
      existing OCR pipeline. Solves out-of-town "upload 50 tickets."
- [ ] **Receivables tracker.** Loads shipped → what is owed → days outstanding →
      mark paid. No bank integration needed. Closes a $31–65k/yr hole at DalMex
      and is the honest first slice of the Trading tier. **Highest $/hour on this
      list.**
- [ ] **Load window check at the scale** — tonnage + bale count against the
      buyer's min/max before the truck leaves. Cheap; nobody else has it.
- [ ] Cellular auto-recovery script (Pi) — checks usb0 IP + default route.
- [ ] Tailscale auto-recovery script (Pi).

## 🟠 BUILD QUEUE — the sold tiers (order matters)
1. [ ] **Certificates of destruction** + hash chain + public verify page.
       Finishes Scale House. ~6 weeks of evenings.
2. [ ] **Wave invoice exporter** for Metropolitan. Wave has no in-app invoice CSV
       import — it goes through **Wave Connect** (Google Sheets add-on): one row
       per line item, rows stay on the same invoice until the customer changes, a
       blank row forces a new invoice. **Get the real column headers first** —
       install Wave Connect, hit *Prepare Input Sheet*, send the headers.
       Wave's GraphQL API needs a paid Pro/Advisor plan — ask Mark which he's on.
       (DalMex is QuickBooks; the existing QBO export stays.)
4. [ ] **Operations tier** — containers/trailers, idle flags, portal service
       requests, recurring schedules, draft invoices, daily close-out, weight
       anomaly flags. ~3–4 months.
5. [ ] **Trading tier** — contracts + rate cards by grade, billing audit (invoice
       vs scale ticket), settlement reconciliation, payment matching by release
       number, monthly index upload → rebate repricing, margin by grade.
       ~4–5 months. Needs invoice ingestion + payment ingestion first.
6. [ ] **Compliance tier** — ledger-wide hash chain, audit trail, void/reissue
       with both versions kept, 6-year retention export. ~6–8 weeks. Cross-cutting
       and hard to retrofit — design it before Operations if selling it early.

## ✨ FROM THE DEMO — port back into the real app when the tier is built
- [ ] Driver **Spanish** (ES/EN, auto-detect from `navigator.language`). Driver
      side only; admin stays English.
- [ ] **Auto day/night** — `prefers-color-scheme` + clock (night 19:00–07:00),
      re-check every minute. Keep intentionally-dark panels constant. Print always light.
- [ ] **QR check-in sign** at the scale. Bilingual. The URL on printed metal must
      be one you control and can redirect — not a deep link.
- [ ] Guided tour / "show me how it works" walkthrough.
- [ ] Offline banner + queued-ticket counter (the Pi already does this; surface it).

## 🔵 CARBON DASHBOARD (internal-only, not urgent)
- [ ] LEED / Non-LEED filter on the Portfolio carbon view (exclude Hayes).
- [ ] Consolidate + correct GWP factors (two hard-coded spots, ~line 1424 and
      ~3650; align to EPA WARM v16, single editable source, cite it).

## 🔴 HARDER / HIGH-STAKES
- [ ] Restart-safe debounce (Pi) — persist lock across restarts (#17/#18 case).
- [ ] Clean duplicate/mistagged historical rows (old 5500 ×4, restart pair).
- [ ] Move Pi serial off USB to 2nd GPIO UART (4G HAT uses primary). Hardware.
- [ ] Long-term: Supabase Auth for the admin app. Passphrase-RPC covers daily
      needs; full Auth is the right end state. Plan properly.

## ⚙️ ONGOING HABITS
- [ ] Never hard-power-cut the Pi: `sudo shutdown -h now`, green LED, unplug.
- [ ] Watch Pi Health on hot afternoons (green <155°F normal; amber = watch;
      red = check airflow now). Blow dust quarterly.
- [ ] index.html: backup → branch + PR → one change → deploy → verify → next.
- [ ] Long file to the Pi? Repo + curl, never paste.

## ⛔ DEFERRED / BLOCKED
- [ ] On-demand Wi-Fi printer — BLOCKED (printer side has no internet).
- [ ] Field diagnostic kit (7" monitor, USB keyboard, micro-HDMI, power bank).
- [ ] Multi-site rollout — Node Hardening Spec for repeatable builds.
- [ ] Restrict hauler visibility for logged-in clients.

---

# TRACK 2 — COMMERCIAL

## 🤝 METROPOLITAN — the deal
- [ ] **Publish the current demo.** Repo `metropolitan-demo`, `index.html`,
      pencil → select all → paste → commit. The live copy was Aug 6 and is missing
      half the app.
- [ ] **Sell Scale House at $350, live in ~8 weeks.** Show Operations and Trading
      on the page with dates against them and say plainly they are not built.
      Discovering it in month two costs far more than saying it in the room.
- [ ] **Decide before the meeting: pricing on a public URL.** It travels.
- [ ] **Bus-factor answer, written down before Mark asks.** Raise it first:
      - Metropolitan owns its data, exportable any time, standard format
      - Nightly export to a bucket **they** control
      - Perpetual royalty-free source licence on defined triggers (death,
        incapacity, ceasing to trade, 30 days without support)
      - Repo + deployment runbook with your attorney, refreshed quarterly
      ReMatter and cieTrade will not escrow source to a $350/mo customer. You can.
- [ ] **Mutual NDA with an explicit non-use clause** — Metropolitan's operational
      data never used for DalMex's benefit. Back it with separate infrastructure,
      not just RLS. Both are Dallas recyclers; he will think about this.
- [ ] **Support window in the contract.** Business hours, best-efforts after.
      Alone, you cannot offer 24/7. Say so before it's tested at 6am.
- [ ] **Write the deployment runbook** — Pi build, schema, deploy process, good
      enough that a contractor could pick it up cold. Doubles as escrow content.
- [ ] Confirm Metropolitan's Wave plan (free vs Pro) — decides Sheets vs API.

## 🏢 DIVERTSCAN AS A BUSINESS
- [ ] **Entity decision.** DivertScan is Robert's, DalMex is a customer. Sole
      prop under SSN vs LLC — decide *before* finishing Stripe onboarding;
      switching after usually means a new account. **Talk to the accountant.**
- [ ] **Stripe.** Bank account must match the entity — not DalMex's. Do not run
      DalMex material revenue through it. Descriptor `DIVERTSCAN`.
      Description: *subscription software for commercial recycling facilities;
      captures truck scale weights and produces weight tickets, custody records
      and diversion reports; US business customers, fixed monthly fee per
      facility, billed in advance; software only.*
- [ ] **Real website** — Stripe wants pricing, terms, contact. The demo won't do.
- [ ] **Stripe Payment Links** → paste into `STRIPE_LINKS` in the demo
      (8 slots: 3 tiers × monthly/annual, plus Compliance both ways).
      Payment Links only. A secret key (`sk_...`) never goes in that file.
- [ ] **Charge DalMex the same rate card.** Written agreement, monthly invoice.
      If discounted, put the discount in writing as a discount.
- [ ] Annual prepay = two months free. Fixes cash, cuts churn, matches what every
      competitor demands anyway.

## 🎯 COMPETITIVE POSITION (Aug 13 scan)
Field: **ReMatter / ScrapRight / ScrapWare / GreenSpark / Nexus** (metal yards,
theft compliance), **cieTrade** (trading ERP — already does index-formula pricing
and rebates, closest competitor to the Trading tier), **Wastebits** (manifests),
ITAD/Recyclesoft (serialised asset destruction).

Lose on: 50-state metals compliance engine, SOC 2, feature depth, support hours.
Win on: **published pricing** (nobody else publishes; ScrapRight demands an annual
commitment), **destruction certificate + landfill diversion on one document**
(nobody does both), **billing audit against your own scale ticket** (nobody
markets it).

Don't chase metal yards or trading depth. The defensible niche is weight-based
product destruction + diversion reporting for operators too small for enterprise
procurement.

---

## 📓 DALMEX MONEY ITEMS FOUND AUG 13 (operational, not code)
Evidence base for the Trading tier. Act on these regardless of what gets built.
- [ ] **$435.20 received and never applied.** Buyer remittance totalled $5,441.15;
      QuickBooks recorded $5,005.95. The missing line is the price adjustment.
- [ ] **$109.30 under-billed** — BOL says SWL (contract $255), invoice billed
      "SW / Softwhite Paper" at $250. If that item is set up wrong in QuickBooks
      it repeats on every white-ledger load. Check.
- [ ] **$10.80 under-billed** — 43,680 lb billed as 21.75 ST instead of 21.84.
      Quarter-ton rounding. A one-off, not policy (the other invoice was exact),
      but ~$3.6–7.2k/yr if it becomes a habit.
- [ ] **Release number mistyped** — BOL `4502713926-S4`, invoice `45022713926-S4`.
      Extra digit. Their payment will quote theirs.
- [ ] **Put the buyer's release number in the BOL Release No. box**, not your own
      T-number. Costs nothing; makes every payment self-match.
- [ ] **Payment terms are three different numbers** — your invoice says Net 10,
      their PO says Net 20, you believed Net 15. Their payments track Net 20.
      Confirm in writing; stop chasing on-time payers.
- [ ] **Turn off card payment for ACH buyers.** 2.99% = $78 on a $2,610 invoice.
      The entire float advantage of the faster buyer is ~$21.
- [ ] **Ask the slower-paying buyer for written terms.** "Usually quick" gives you
      no day to measure against, so a missing payment never looks wrong.
- [ ] **Ask the bank whether ACH addenda come through** on transaction downloads
      or an EDI 820. One buyer already puts release numbers in the addendum —
      if it passes through, payment matching automates itself.
- [ ] **Index vs contract lag.** Rebate formulas reprice the day the Yellow Sheet
      lands; sell prices lag a month. Mar→Jun the index moved OCC +$20/ST, your
      rebate +$14, your contract +$5 — **−$9/ST**. Watch it every month.
- [ ] Fastmarkets/RISI index tables are licensed. Internal use is what you pay
      for; republishing them on a public page probably isn't. Check the terms
      before the demo goes public.
