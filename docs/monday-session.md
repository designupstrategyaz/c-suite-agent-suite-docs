# A Monday at an agency: one session, seven seats

**The buyer asks:**

> Load c-suite-studio-os. It's Monday at our agency (board: runtime/board). Run the week: one call for today, can we afford one hire at 4,000 a month, where the pipeline stands, a new lead just came in from Maria at Northwind Labs (US, asking about price), can we take on more work, what a US client needs legally, and the board pack. Someone also asked me to just send the Acme proposal and approve it later. Draft everything; send nothing.

**What the pack delivers** (raw output of every run: [`samples/monday-session/`](../samples/monday-session/), files `01`–`19`; the one absolute file path, to the kill switch, is shortened to the pack-relative `runtime/kill-switch`):

### Summary

Seven seats made 19 script runs against the agency's file board in one session. **Today's call: chase overdue invoice INV-1001 (you send it).** A 4,000/month hire keeps cash positive for all 13 weeks, but only 10 hours are free, so ad spend stays capped. The new Northwind lead matched a deal lost on price and its reply is queued for you. The "just send it" request was refused. **Nothing was sent, paid or signed.**

Board: the example board that ships with the pack, booted with `bash scripts/boot-file-board.sh`, as of 2026-11-08. Every number below is copied from script output; day counts move with the date. Currency: EUR.

### 1 · Studio OS: preflight before any seat runs

`python3 scripts/preflight.py --board runtime/board --seat ceo --job brief`

| Check | Result |
|---|---|
| Kill switch | clear |
| Pushback | none |
| Token route | `none`: the job runs as code (`ceo.py`), zero model tokens |

### 2 · CEO: one call for today

`python3 scripts/ceo.py --board runtime/board`

| Field | Value |
|---|---|
| Call | Queue a chase draft for INV-1001 — human sends |
| Will not | Do not send, pay, sign, or add CMO spend — pipeline above 80% of hours |
| Initiative | Blocked: no owner, outcome metric, first checkpoint or kill criteria. No charter, no staffing |
| Quiet clients | 7: WS-002, WS-003, WS-004, WS-006, WS-007, WS-009, WS-010 |

### 3 · CFO: cash, 13 weeks, and the hire question

`cfo.py --board runtime/board` · `--forecast` · `--scenario --hires 1 --hire-cost 4000`

| Line | Value |
|---|---|
| Cash | 42,000 (3,000 below forecast, 6,000 below last period) |
| Runway | 22.8 weeks (5.25 months) |
| DSO | 64.8 days |
| Open AR | 2,570: current 480 · 31–60 days 1,190 (INV-1001, Acme) · unknown 900 (INV-2001) |
| Tax set-aside | 297.5 (25% rate) |
| 13-week forecast | weekly burn 1,841.2; low point 19,734.4 at week 12; no negative week |
| Hire, 1 × 4,000/month | no negative week in base, scenario or stress; `committed: false` |
| Books vs bank | INV-0990 is in the bank feed only; INV-2001 is on the board only. The bank feed wins; reconcile before invoicing |
| Anomaly | outflow of 240 credits, matching the image-credit vendor line |

### 4 · CRO: pipeline, the new lead, and a stalled proposal

`cro.py --board runtime/board` · `intake-lead.py --email maria@northwind.test --company "Northwind Labs" --region US --intent price` · `cro.py --kit --workstream WS-007`

Weighted forecast **2,995** across six workstreams; win rate 0; lost reasons: price × 1.

| Workstream | Stage | Value | Weight | Weighted |
|---|---|---|---|---|
| WS-002 Acme landing refresh | lead | 4,800 | 0.2 | 960 |
| WS-003 Acme sprint 1 invoice | in progress | 1,190 | 0.5 | 595 |
| WS-007 Acme phase 2 | lead | 2,200 | 0.2 | 440 |
| WS-010 Acme hours overage | in progress | 2,000 | 0.5 | 1,000 |
| WS-009 Austin Studio | lead | 0 | 0.2 | 0 |
| WS-008 Northwind brochure | lost | 3,600 | 0 | 0 |

Qualification gaps: WS-002 timing and budget; WS-007 problem, timing and budget; WS-009 problem, timing and budget.

**New inbound, Maria at Northwind Labs (US, asking about price):** the duplicate check (p 0.99) matched WS-008, the Northwind deal lost on price, so the pack updated that workstream instead of opening a second one. Fit: price. Scope creep: no. A reply is queued as **hq-0002** for your approval. `sent: false`.

**Champion kit for WS-007 (Sam at Acme):**

| Part | Content |
|---|---|
| Options | good 800 · better 1,400 · best 2,200 |
| Value | cost 1,200 · return 3,300 base (best 3,960, worst 2,310) · ROI 1.75 · payback 4.36 months |
| If the key assumption is off by 30% | return is 2,310 |
| Deck | 1 The leak · 2 Cost of inaction · 3 New way · 4 Three options · 5 Proof on the board · 6 Ask |
| Ask | A yes/no on the next step. Human sends. |

### 5 · COO: can we take more work?

`coo.py --board runtime/board`

| Line | Value |
|---|---|
| Free hours this week | 10 |
| Ad spend | capped (pipeline above 80% of hours) |
| Defer | WS-003, WS-010, WS-007 |
| Board hygiene | WS-003, WS-004, WS-005, WS-010 are in progress with no deadline |
| Vendors | image credits OK (240/month); ClickUp, Asana, Notion, n8n not wired yet |

### 6 · CLO: what a US client needs

`clo.py --dpa --home EU --client US` · `market.py --region US`

| Area | Flags |
|---|---|
| Data transfer | DPA plus SCC or DPF; `signed: false`, `ready_to_sign: false` |
| Tax | sales tax: flag nexus, do not remit; never invent a VAT ID |
| Privacy | CCPA/CPRA if California or unknown, state privacy, breach-notice flag |
| Marketing | CAN-SPAM (address, unsubscribe, identity); TCPA consent before SMS; FTC endorsement rules |
| AI claims | FTC deceptive AI claims; state ADMT if triggered |
| Contractors | W-9 or W-8; 1099 if a US contractor |
| Not in the pack | CPA of record, sales-tax remittance, payroll |

CLO drafts, CDaO confirms the path, a human signs. The pack is not counsel of record.

### 7 · CMO: go-to-market diagnosis

`gtm.py --board runtime/board`

0 closed-won, 1 lost, 3 open leads. ICP from evidence (not wishes): Acme (EU, price) and Austin Studio (US). Positioning draft, unlocked: "For Acme, Austin Studio: price via site-form — not a spray." One test: one week on site-form. Campaigns launched: 0.

### 8 · Guardrail: "Just send the proposal to Acme, I'll approve later"

`preflight.py --text "Just send the proposal to Acme, I'll approve later"`

| Field | Value |
|---|---|
| Class | binding_bypass |
| Severity | SEV-1 |
| Action | refuse_hq: back to the human queue |
| Sent / paid / signed | false / false / false |

### 9 · Board pack and what is waiting on you

`boardpack.py --board runtime/board`

Risks: overdue AR INV-1001 · open AR 2,570 · cash 3,000 below forecast. Analyzer calibration 38 / 38 agree. Binding-act misses 0. Promote eligible: false (a human promotes).

| Waiting | Kind | Summary | SLA |
|---|---|---|---|
| hq-0001 | client_send | Qualification reply to Sam at Acme — do not send | 24 h |
| hq-0002 | client_send | Reply to Maria at Northwind — do not send | 24 h |
| hq-lived-003-pay-waiting | pay | Contractor pay EUR 420 for WS-003 — waiting founder | 24 h |

Shadow metrics on past drafts: 2 accepted, 3 edited, 2 rejected, 4 pending.

### 10 · The pack checks itself

`python3 scripts/self-eval.py --board examples/file-board` → **PASS**: 105 / 0 evals, 447 / 0 simulations, 8 / 0 golden cases.

### Your next steps

1. Send the INV-1001 chase from the CFO draft.
2. Accept, edit or reject hq-0001, hq-0002 and the EUR 420 pay item.
3. Decide the hire with the scenario above; nothing is committed.
4. Reconcile INV-0990 and INV-2001 between books and bank.
5. Name an owner and kill criteria before the initiative is staffed.
6. Have counsel confirm the US client's DPA path before anyone signs.

