<img src="logo.png" width="96" alt="C-Suite Agent Suite logo">

# C-Suite Agent Suite — documentation

Eleven executive seats plus a Studio OS router for agencies, B2B SaaS and consulting firms. Each job drafts one artefact from a local file board, such as a 13-week cash forecast or a proposal kit. No script sends, pays or signs. Ships with a kill switch, a human queue and a 105-eval test gate.

This repository is the public documentation: what the pack does, real sample outputs, the FAQ and the limits. The skill pack itself (12 skills, scripts, example board) is sold on Agensi and is not in this repository.

- **Demo video (63 s, real runs):** https://youtu.be/domzW8sVSCQ
- **Full session, seven seats on one Monday:** [docs/monday-session.md](docs/monday-session.md)
- **Agensi listing:** the link is added here when the listing goes live.

## What it does

C-Suite Agent Suite is a studio operating system for digital agencies, B2B SaaS startups and boutique consulting firms. It packs 11 executive seats, from CEO and CFO to CLO, plus a Studio OS router, as 12 skills that cover jobs a founder already does each week. An agent loads the router first, picks one seat and runs one named job, such as a 13-week cash forecast or a proposal kit. Each job ends in a draft artefact on a local file board. Cafés, contractors and storefronts fit a secondary lane that drafts deposits, change orders, estimates and CPM schedules.

### Human gate

No script in the pack contains code that sends email, moves money or signs. Each agent drafts and stops, and every send, payment, signature or publish goes to a human queue where a named person accepts, edits or rejects it. A queue item that times out stays unapproved. Writing halt to the kill-switch file stops the morning brief, the daily digest, preflight and the n8n runs on their next call. Shadow metrics compare past drafts with what the human decided, which gives the founder a measured record of how far the agents can be trusted.

### Preflight and cost

The router tells the agent to check the kill switch before any job. One preflight call runs that check, pushes back on requests such as "just send it, I'll approve later" and picks a token route, with scripts first and cheaper model tiers next. No step may invent a send to save tokens. The scripts use only the Python 3 standard library and need no API key on the included example board.

### Tested

A self-eval gate ships inside the pack. It runs 105 evals, 447 scripted simulations and 8 golden cases, and checks 18 named doc claims against the code. 117 of the simulations are adversarial, such as prompt injection and "approve it later" pressure, and each one must fail closed for the gate to pass.

### Connectors and limits

Read-only connectors pull ClickUp and Asana tasks, named Notion pages and a bank GET feed once the operator adds tokens, and they never write back. Optional analyzer APIs (Jev, Cloudflare Clef or OpenAI Decisions) receive the question and the relevant board text only when the operator sets ANALYZER_BACKEND; left blank, every check runs in code. Three n8n workflows come in the box, and the inbound webhook needs header auth and validates every field before anything runs. Linear and Plane have a stub only, and the pack gives no legal, tax, accounting or payroll advice.

### What is in the pack

The pack holds 12 skills, 56 standard-library Python scripts, an example board with 10 workstreams and 10 human-queue items, three n8n workflows and the self-eval gate. The scripts import each other and read the example board, so they run from a local copy of the pack. Quick start: bash scripts/boot-file-board.sh, fill human_of_record in the charter, then python3 scripts/ceo.py --board runtime/board.

## Quick start (after purchase)

```bash
bash scripts/boot-file-board.sh                          # example board -> runtime/board, empty kill switch
# fill human_of_record in runtime/board/CHARTER.md
python3 scripts/ceo.py --board runtime/board             # one morning brief, nothing sent
python3 scripts/self-eval.py --board examples/file-board # the pack's own test gate: PASS or FAIL
```

Requirements: The scripts import each other and read the example board, so they run from a local copy of the pack. That copy needs python3 3.9 or later, bash for the boot script and a folder the agent can read and write. An agent that only loads the skills follows the seat instructions and returns the JSON artefact without running the scripts. Analyzer APIs and connector tokens are optional.

## The 12 skills

| Skill | Job |
|---|---|
| `c-suite-studio-os` | Router: matches the request to one job and one seat; tells the agent to check the kill switch first |
| `c-suite-ceo` | Morning brief, board pack, initiative charter gate |
| `c-suite-cfo` | Cash, 13-week forecast, hire scenario, invoice chase stage |
| `c-suite-coo` | Delivery, capacity hours, board hygiene, contractor estimate and CPM |
| `c-suite-cco` | Design, copy, image and video briefs |
| `c-suite-cmo` | ICP and GTM audit, placement, SEO, GEO, LinkedIn drafts |
| `c-suite-cro` | Pipeline forecast, lead qualification, proposal and champion kit |
| `c-suite-crso` | Understand a question before anyone acts |
| `c-suite-cto` | Build, integrations, incidents, agent platform |
| `c-suite-cdao` | Scorecard, data residency, DPA path, evals |
| `c-suite-chro` | Agent registry, tools, contractor pack |
| `c-suite-clo` | Contract, DPA, IP and licence review drafts |

## Sample outputs

Every screenshot below is real script output on the example board that ships in the pack, as of 2026-11-08. Day counts such as "38 days past due" move with the date. The JSON next to each one is the unedited output.

#### Morning brief: one call for today (CEO)

![Morning brief: one call for today](screenshots/sample-4-ceo-brief.png)

Raw output: [`4-ceo-brief.json`](samples/4-ceo-brief.json)

#### 13-week cash forecast (CFO)

![13-week cash forecast](screenshots/sample-1-cfo-forecast.png)

Raw output: [`1-cfo-forecast.json`](samples/1-cfo-forecast.json)

#### Money already earned: overdue invoice and quiet proposals (CFO)

![Money already earned: overdue invoice and quiet proposals](screenshots/sample-2-followup.png)

Raw output: [`2-followup.json`](samples/2-followup.json)

#### Champion kit for a stalled proposal (CRO)

![Champion kit for a stalled proposal](screenshots/sample-3-cro-kit.png)

Raw output: [`3-cro-kit.json`](samples/3-cro-kit.json)

#### Contractor estimate and CPM schedule (COO)

![Contractor estimate and CPM schedule](screenshots/sample-6-estimate-cpm.png)

Raw output: [`6a-estimate.json`](samples/6a-estimate.json), [`6b-cpm.json`](samples/6b-cpm.json)

#### Guardrails: preflight pushback and the kill switch (Studio OS)

![Guardrails: preflight pushback and the kill switch](screenshots/sample-5-guardrails.png)

Raw output: [`5a-preflight.json`](samples/5a-preflight.json), [`5b-kill.json`](samples/5b-kill.json)

## FAQ

**Will it send emails, pay invoices or sign anything for me?**

No. No script in the pack contains code that sends email, moves money or signs. Each agent drafts and stops, and every send, payment, signature or publish goes to a human queue where a named person accepts, edits or rejects it. An item that times out stays unapproved. Writing halt to the kill-switch file stops the morning brief, the daily digest, preflight and the n8n runs on their next call.

**Which skill do I load first?**

Load c-suite-studio-os first, together with shared/industries.md and shared/sme-startup.md, and add shared/local-services.md for a café or contractor. The router sends one named job to one seat, such as c-suite-cfo for cash or c-suite-cro for a proposal, so the 12 skills never load at once.

**Do I need API keys or a specific model provider?**

No key is needed. The scripts use the Python 3 standard library (3.9 or later) and run on the included file board. Read-only ClickUp and Asana tokens, named Notion pages and a bank feed are optional, and so is an analyzer API: Jev, Cloudflare Clef or OpenAI Decisions receive board text only when the operator sets ANALYZER_BACKEND.

**How do I try it before connecting my own systems?**

Run scripts/boot-file-board.sh once. It copies the example board into runtime/board, creates an empty kill-switch file beside it and asks for a human_of_record name in CHARTER.md. The smoke test is python3 scripts/run_board.py --board runtime/board --kill-switch runtime/kill-switch, and every seat can run on the example data before any real system is connected.

**Does it work for a café, contractor or storefront?**

Yes, as a secondary lane. A café, contractor or storefront gets cash, deposit and change-order drafts, and c-suite-coo builds an estimate with visible overhead, contingency and profit plus a CPM schedule from the operator's own takeoff. Requests for POS, card capture or field dispatch are refused and pointed to tools such as Square, Clover, ServiceTitan or Jobber.

**Does it handle GDPR, VAT and similar rules?**

The pack flags rules and drafts paths; it does not give legal or tax advice. python3 scripts/market.py prints EU, UK and US flags for GDPR, UK GDPR, CCPA, the AI Act, VAT, sales tax and CAN-SPAM, and --lookup handles other countries. c-suite-clo drafts the DPA path for a cross-border client and a human signs it.

**Can I check the pack myself?**

Yes. python3 scripts/self-eval.py --board examples/file-board runs 105 evals, 447 simulations and 8 golden cases on the buyer's own machine and prints pass and fail counts for each suite, then PASS or FAIL. A local edit to a seat can be checked the same way before it is used.

**Which tools does it connect to?**

Read-only GET connectors cover ClickUp and Asana tasks, named Notion pages and a bank feed, and python3 scripts/get_live.py --live --sync overlays live tasks onto the board without writing back. n8n is optional and its three workflows need the Execute Command node. Linear and Plane are not supported yet; the stub says so instead of pretending.

## Limits

Linear and Plane have a stub only, with no client yet. The scripts need the whole pack on disk to run. The three n8n workflows call the Execute Command node, which n8n Cloud does not offer and n8n 2.0 or later turns off by default; shared/n8n.md covers the SSH-node and cron routes. The pack gives no legal, tax, accounting or payroll advice, and the wiring to n8n, ClickUp or a bank stays with the buyer.

More on n8n hosting (Cloud, self-hosted 2.0+, no n8n): [docs/n8n-hosting.md](docs/n8n-hosting.md).

## Licence

Documentation, screenshots and sample outputs © 2026 DesignUp Strategy AZ. You may link to and quote this documentation. The skill pack is licensed separately to buyers on Agensi; nothing here grants a licence to the pack.
