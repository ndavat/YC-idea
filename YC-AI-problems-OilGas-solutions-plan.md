# YC "Top Unsolved AI Problems" → Oil & Gas Working Solutions

Source: Y Combinator **Requests for Startups — Summer 2026** (https://www.ycombinator.com/rfs).
This document takes each AI-core RFS and reframes it as an oil & gas specific opportunity, with a phased build plan (architecture, data, MVP, validation, GTM) and pilot targets.

---

## How to read this plan

Each entry follows the same structure:

- **YC Problem** — the unsolved AI problem YC is asking founders to solve.
- **O&G Why-now** — why this breaks open in oil & gas specifically.
- **The Solution** — the product we'd build.
- **Build Plan** — phased (Phase 0 discovery → Phase 1 MVP → Phase 2 pilot → Phase 3 scale).
- **Moat / Defensibility** — what makes it hard to copy.
- **Pilot Targets** — first customers to approach.
- **Risks & Mitigations**.

Prioritization (by O&G revenue pull + technical feasibility):
- **Tier 1 (build first):** #2 Discovery Engines, #3 Service Companies, #5 Company Brain, #7 SaaS Challengers, #10 OS for Companies.
- **Tier 2:** #6 Counter-Swarm (asset protection), #8 Software for Agents, #9 Supply Chain.
- **Tier 3 (longer/hardware):** #4 Dynamic Interfaces, #11 Inference Chips, #1 Hardware Supply Chain.

---

## #2 — AI-Native Discovery Engines (YC: Jon Xu)
*Closed-loop hypothesis → experiment → analysis systems for science.*

### O&G Why-now
Subsurface engineering is the original "design-make-test-analyze" loop, but the "make/test" step costs $10–50M per well and 3–6 months of rig time. Reservoir models are rebuilt from sparse well logs + 3D seismic, and 60–70% of the uncertainty is in rock & fluid properties that models guess at. Foundation models now do PhD-level geoscience reasoning; closed-loop discovery has never been applied to reservoir characterization, EOR chemistry, or drilling-fluid design.

### The Solution
**"ReservoirLoop"** — an AI-native discovery engine for subsurface workflows. It runs a closed loop: (1) ingest all client well logs, core, seismic, production history; (2) propose reservoir-property realizations and EOR/drilling-fluid hypotheses; (3) auto-design the *next-best experiment* (which log to run, which core test, which pilot injectivity); (4) ingest results and update. The output is a continuously-improving digital twin of the reservoir that tells the geoscientist "this is the highest-value next data acquisition," not just a model.

### Build Plan
**Phase 0 (4 wks) — Verticalize the discovery loop.**
- Pick one workflow: **EOR surfactant/polymer screening** (chemistry-heavy, lab-testable, fast feedback) OR **petrophysical inversion** (log-derived properties). EOR chemistry is the better wedge: smaller data, clear lab validation path.
- Domain corpus: SPE papers, OnePetro, USGS, DOE EOR reports, core-analyst datasets (SCAL).

**Phase 1 (8 wks) — Closed-loop MVP on a single basin.**
- Data layer: parsers for LAS/DLIS well logs, SEG-Y seismic, production (CSV/PHDWin), PVT reports. Normalize to a common reservoir object graph.
- Reasoning agent harness (Claude/GPT harness like the one YC's Ankit Gupta references) wrapped with geo-domain tools: a PVT calculator, a Buckley-Leverett solver, a surfactant-phase-behavior simulator, an uncertainty sampler (PyMC/NumPyro).
- "Design-make-test-analyze": agent proposes 5 candidate EOR formulations → routes to a partner lab (e.g., a university core flood rig) → results flow back → agent updates the surrogate.
- Validation: reproduce a published EOR coreflood to within 10% oil-recovery error on blind data.

**Phase 2 (12 wks) — Paid pilot with an operator.**
- Target: a mid-cap Permian or unconventional player with an active EOR/pilot program. Deliver: a ranked list of "next 3 injectant designs" with predicted incremental recovery + confidence intervals, beating the operator's internal chemist team on one slot.
- Success metric: operator runs at least one recommended formulation in a field mini-pilot.

**Phase 3 — Expand the loop to drilling fluids + completion chem.**
- Same engine, new "make" step (mud-rheology lab, frac-fluid break tests).

### Moat
- Accumulating closed-loop results create a proprietary "what experiments actually moved recovery" dataset no lab has structured.
- Basin-specific fine-tunes; physics-informed losses prevent hallucinated chemistry.

### Pilot Targets
Mid-cap operators with EOR programs: **Occidental, Diamondback, Permian Resources**, plus NOCs (**ADNOC, Saudi Aramco, Petrobras**) which run large EOR portfolios and tolerate longer pilots.

### Risks & Mitigations
| Risk | Mitigation |
|---|---|
| Lab turnaround kills loop speed | Use high-throughput microfluidic screening partners; in-silico surrogate as the inner loop, wet lab as the outer loop. |
| Operators won't share data | Start with public/DOE datasets + synthetic reservoirs; sell insight, not storage. |

---

## #3 — AI-Native Service Companies (YC: Gustaf Alströmer)
*Don't sell software — sell the completed service.*

### O&G Why-now
O&G outsources enormous service spend that is document- and rule-heavy: **regulatory reporting** (BOEM/BSEE, EPA, state RRC), **land & lease administration**, **HSE incident investigations**, ** royalty audits**, **decommissioning (P&A) planning**. These are run by armies of consultants (the big service cos + Bouttes/Landmark-type shops) billing $150–400/hr for work that is now 80% automatable. Operators buy the *outcome* (a filed report, an audited royalty statement), which is exactly the "sell the service, not the tool" wedge.

### The Solution
**"Compliance-as-a-Service for O&G"** — an AI-native company that *files* regulatory submissions and *delivers* audited deliverables. The operator hands us access to their data; we return a completed, signed, submitted regulatory package. We charge per-filing or per-asset-month, not per-seat.

Three service lines, sequenced:
1. **Production & emissions reporting** (RRc PR forms, BOEM production, EPA Subpart W/GHG, EIA). Highest volume, most standard.
2. **Land/lease obligation tracking & royalty audits** — parse leases, compute obligations, audit operator-paid royalties, deliver variance reports.
3. **HSE incident investigation reports** — ingest witness statements, SCADA, drone footage; deliver a root-cause report in the operator's template.

### Build Plan
**Phase 0 (3 wks)** — Pick the RRC Texas monthly production report (Form PR). It's mandatory, monthly, high-volume, formulaic.

**Phase 1 MVP (10 wks)**
- Pipeline: operator's SCADA/tank-strapping/allocation data → normalization → allocation engine → populated PR form → human-in-the-loop reviewer (a single licensed P.Eng.) → e-file via RRC online.
- Accuracy bar: 0 errors on submitted values vs. operator's own numbers over 3 consecutive months on one asset.
- Agent stack: extraction agents (PDF/Excel ingest), a deterministic allocation calculator (don't let the LLM do math), and a reviewer agent that flags anomalies.

**Phase 2 (12 wks)** — Get paid to file for 3–5 operators' Permian assets. Charge $X/lease/month. Margins come from 10× throughput per reviewer.

**Phase 3** — Layer BOEM (offshore), EPA GHGRP, EIA. Then cross-sell royalty audit (a service line with contingent-fee upside).

### Moat
- Regulatory schema + operator-specific allocation rules accumulate as structured configs.
- The reviewer P.Eng. licensure + e-filing credentials are a slow-to-replicate operational moat.
- Once we file for an asset monthly, switching cost is real (we hold the clean normalized data).

### Pilot Targets
Operators that hate regulatory overhead: **independents in Texas/New Mexico** (Civitas, Permian Resources, Vital Energy), plus PE-backed private E&Ps that lack back-office scale.

### Risks & Mitigations
| Risk | Mitigation |
|---|---|
| Liability for mis-filings | E&O insurance; human-in-the-loop licensed reviewer signs every filing; deterministic calc engine, LLM only for extraction. |
| Operators insist on software, not service | Offer both; the service is the wedge, self-serve is the upsell. |

---

## #5 — Company Brain (YC: Tom Blomfield)
*A living, executable skills file of how a company actually works.*

### O&G Why-now
O&G operators run on **decades of tribal procedure**: how to respond to a well-control event, how to permit a frac stage in each county, how to swap a gas-compressor seal, how to execute a turnaround. This lives in (a) 100,000-page PDF operating procedures that nobody reads, (b) retiring boomers' heads, (c) Maximo/SAP ticket history, (d) shift handover logs. Crews make mistakes because they can't find "the way we actually do this here." An incident-response agent is useless without this knowledge; this is the missing layer YC describes.

### The Solution
**"FieldBrain"** — an O&G company brain that ingests procedures, Maximo work-order history, incident learnings, PI/SCADA tag metadata, and shift logs, then emits an **executable skills file** (MCP-style tool definitions + decision procedures) that field agents call. Think: a new field tech asks "how do I isolate the high-pressure separator at Battery 7 for the annual inspection?" and gets the *site-specific* procedure, the lockout/tagout sequence, the exact valve IDs from the current P&ID, and the last time it was done + what went wrong.

### Build Plan
**Phase 0 (4 wks)** — Scope to one facility type: a **gas-processing plant** or **SWD saltwater disposal battery** (limited P&ID universe, clear procedures, high incident-repetition).

**Phase 1 MVP (12 wks)**
- Ingest: P&IDs (parse PDFs + connect to SPRECHER+ engineering source if available), operating procedures (SOPs), Maximo/PM work orders, PI Historian tag list, PHA/HAZOP reports, last 5 yrs incident learnings.
- Entity resolution: build a graph linking valve tag ↔ P&ID symbol ↔ procedure step ↔ SCADA tag ↔ work history. This graph *is* the brain.
- Skills compiler: convert each procedure into a structured, tool-calling skill (e.g., `isolate_separator(battery_id, sep_id)` returns ordered steps + current valve states via SCADA).
- Retrieval: hybrid (graph traversal + vector over procedure text). Ground every answer in a citation to a specific P&ID/sheet and procedure section.

**Phase 2 (12 wks)** — Pilot at one midstream plant. Measure: time-to-answer for ops questions, accuracy of procedure steps vs. senior operator's "golden" answer, reduction in procedure deviations.

**Phase 3** — Extend to drilling (well-control response) and refineries (turnaround execution agents).

### Moat
- The entity-resolution graph per facility is deeply embedded and rebuilt only with pain; switching cost is enormous.
- Physics/safety grounding: answers must cite P&IDs — incumbents doing "chat over PDFs" fail this and get thrown out by ops.

### Pilot Targets
Midstream operators with many similar facilities (**Energy Transfer, Williams, Enterprise, Targa**) — they get facility-template leverage. Also refinery turnaround specialists.

### Risks & Mitigations
| Risk | Mitigation |
|---|---|
| Hallucinated procedure = safety incident | Hard rule: never generate a step not traceable to a cited source; ops lead signs off before agent answers go live. |
| P&ID parsing quality | Treat as an engineering problem, use vision models + symbol libraries; accept 95% auto + human QA. |

---

## #7 — SaaS Challengers (YC: Jared Friedman)
*AI-native replacements for invulnerable legacy codebases — ERPs, industrial control, supply chain.*

### O&G Why-now
The O&G back office runs on three categories of "untouchable" legacy: **PI System / AVEVA Historian** (ops data, $$$$$, 1990s architecture), **Maximo/SAP PM** (asset/work management, deeply customized), and **land/production accounting suites** (e.g., P2 Energy, Quorum). These are 10M-line codebases, sold per-seat at huge cost, hated by users, and — per YC's thesis — newly vulnerable because AI collapsed the cost to rebuild. Operators explicitly want out.

### The Solution
**Two challengers** (pick one to start):

**(a) "Pulse" — AI-native ops data platform to replace PI Historian.** Ingest high-frequency SCADA at the edge, store in a columnar time-series engine, and expose a natural-language + agent API. "Show me every time this compressor surged in the last year and what preceded it" returns a query plan + chart, not a tag hunt. Undercut PI on licensing and on the consulting bill required to build PI displays.

**(b) "Maintain" — AI-native CMMS to replace Maximo.** Work orders, asset hierarchies, and PMs — but the PM generation is an agent that reads OEM manuals + failure history and writes the PM schedule itself. Mobile-first for technicians.

Start with **(a) Pulse** — ops data is the O&G data spine; owning it unlocks everything else.

### Build Plan
**Phase 0 (4 wks)** — Confirm wedge: a **single asset class** (reciprocating compressors or ESPs) where PI is overpriced and analytics are manual.

**Phase 1 MVP (12 wks) — Pulse**
- Edge collector (lightweight agent on a gateway PC) ingesting OPC-UA/Modbus at the wellsite, compressing, forwarding.
- Cloud: time-series DB (TimescaleDB/ClickHouse) + asset graph + NL query agent.
- Killer feature: **auto-generated asset context** — agent reads the P&ID + tag list and labels every time series with its function/equipment, so users query by equipment name, not tag.
- Import path from PI (PI OLEDB / Web API) so we can run side-by-side without ripping PI out.

**Phase 2 (12 wks)** — Pilot: replace PI for one basin's worth of wells at an independent. Prove parity + 60% cost reduction + 10× faster ad-hoc analytics.

**Phase 3** — Build the analytics/agent ecosystem on top; this becomes the data layer for #5 FieldBrain and #10.

### Moat
- Once we're the ops-data spine, every other tool depends on us; data gravity.
- Edge collector + asset-context labeling is a compounding data flywheel.

### Pilot Targets
Independents tired of AVEVA pricing: **Civitas, Vital Energy, Crescent Energy, SM Energy**; midstream: **DCP, Targa**.

### Risks & Mitigations
| Risk | Mitigation |
|---|---|
| Operators won't rip out PI | Don't ask them to. Run side-by-side; win net-new assets first. |
| Real-time reliability bar | Edge-first architecture; cloud is for analytics, not control. |

---

## #10 — The AI Operating System for Companies (YC: Diana Hu)
*An intelligence layer that makes the whole company queryable and closed-loop.*

### O&G Why-now
An O&G operator is a sprawling set of open loops: drilling plans vs. actuals reviewed weeks later; production forecasts vs. actuals reconciled monthly; HSE leading indicators reviewed quarterly. Diana Hu's thesis — turn the company into a closed loop that monitors, compares, and adjusts — maps perfectly onto **asset operations**, where the signals (SCADA, drilling telematics, work orders, financials) already exist but are siloed. The first operator to run "closed-loop asset ops" ships more barrels at lower cost.

### The Solution
**"OpsOS"** — an intelligence layer that fuses drilling telemetry, production SCADA, work-order history, commodity prices, and the operator's own plan/budget into one queryable graph. It (1) detects deviations from plan in near-real-time (e.g., "Rig 3 is drilling 12% slower than your plan for this geology"), (2) surfaces root-cause hypotheses, (3) proposes corrective actions, (4) tracks whether the correction worked — closing the loop.

### Build Plan
**Phase 0 (4 wks)** — Pick one loop: **drilling performance vs. plan** (rich telemetry, clear KPIs: ROP, NPT, days-per-10k-ft).

**Phase 1 MVP (12 wks)**
- Connectors: WITSML (drilling telemetry), production (PI/opendaphne), AFE/budget (Excel/SAP), geological prognoses.
- The "plan" object: parse the operator's drilling program into a structured expected-trajectory + time-depth curve.
- Deviation agent: stream WITSML → compare to plan → classify variance (geology-induced vs. tool vs. crew) → push alert to drilling supervisor's phone with a suggested action.
- Closed-loop: track whether the suggested action improved ROP; feed back as labeled training data.

**Phase 2 (12 wks)** — Pilot on 5–10 wells with one operator. Metric: reduce non-productive time (NPT) by ≥10% vs. the operator's prior offset wells.

**Phase 3** — Add loops: production vs. forecast, LOE (lease operating expense) vs. budget, turnaround schedule vs. actual.

### Moat
- Closed-loop training data: every correction that worked/not-worked improves the model; competitors without the loop can't catch up.
- Cross-asset benchmarking (how does this crew/geology compare across all our wells) is only possible from inside.

### Pilot Targets
**Drilling-focused independents**: **Endeavor, Fasken, Mewbourne** (private, fast-moving, drilling-heavy); plus drilling contractors (**Nabors, Patterson-UTI**) who can deploy across many operators.

### Risks & Mitigations
| Risk | Mitigation |
|---|---|
| "We already have dashboards" | Sell the closed loop + auto root-cause, not the dashboard. |
| Real-time data quality | Trust-but-verify connectors; flag, don't act, on bad data. |

---

## #6 — Counter-Swarm Defense (YC: Tyler Bosmeny)
*Defending against coordinated autonomous drone attacks — defense that looks like Cloudflare, not Raytheon.*

### O&G Why-now
O&G critical infrastructure (refineries, LNG terminals, offshore platforms, pipelines, loading terminals) is a prime drone-attack target — and per YC, the cost asymmetry favors attackers ($500 FPV drone vs. $3M Patriot). A 2024 AWS data center was reportedly taken out by an Iranian drone swarm; O&G sites are softer and more valuable. Perimeter security today is fragmented cameras + guards + jammers that don't talk to each other. Operators (and their insurers, and DHS/CFATS) are actively shopping.

### The Solution
**"SwarmShield"** — a counter-swarm defense stack purpose-built for fixed O&G facilities. Sensor fusion (RF, radar, EO/IR, acoustic) into one real-time common operating picture + autonomous intercept coordination + non-kinetic defeat (RF/geolocation spoofing of the swarm's autonomy, navigation denial). Software-first, hardware-agnostic: ingest from whatever sensors the facility already has, add the missing ones, run the fusion + decision loop.

### Build Plan
**Phase 0 (6 wks)** — Pick facility: **LNG export terminal** (highest consequence, strict CFATS/regulatory mandate, deep pockets). One site survey.

**Phase 1 MVP (16 wks)**
- Sensor fusion: integrate 1 radar + 2 EO/IR cameras + 1 RF detector (vendor-agnostic via a pluggable driver layer). Single fused track picture.
- Threat logic: classify track (hobby vs. reconnaissance vs. attack), estimate intent (loitering on a tank = bad), trigger tiered response.
- Defeat: start with **non-kinetic** — geofencing alerts, RF uplink denial, GNSS spoofing zone (where legally permitted). Add kinetic interceptor integration later.
- Decision-support UI for the site security ops center, with auto-playbook responses.

**Phase 2 (16 wks)** — Live pilot at one terminal. Measure: detection rate, time-to-classify, false-positive rate vs. recorded drone test flights.

**Phase 3** — Multi-site federated detection; shared threat intelligence across an operator's footprint.

### Moat
- The fusion/decision software, not the sensors — works with any hardware, hard to replicate the tuned threat models.
- Cross-site threat intel network effect (a drone signature seen at Terminal A is known at Terminal B).

### Pilot Targets
LNG operators & terminals: **Cheniere, Sempra, Venture Global, NextDecade**; majors with offshore platforms; pipeline operators under TSA security directives.

### Risks & Mitigations
| Risk | Mitigation |
|---|---|
| Regulatory (kinetic/spectrum) | Lead with detection + non-kinetic where lawful; partner with licensed defeat vendors for kinetic. |
| Long sales cycles to critical infra | Start with a *detection-only* paid pilot (easier PO), expand to defeat. |

---

## #8 — Software for Agents (YC: Aaron Epstein)
*Rebuild every software category agent-first — APIs/MCP/CLIs instead of human UIs.*

### O&G Why-now
Every O&G operator is now deploying internal agents ("agent that monitors my wells," "agent that drafts my regulatory filing"). These agents are forced to scrape human-facing apps or call brittle vendor APIs. The O&G-specific data sources that *need* agent-native interfaces — **IHS/Enverus (well/completion data), BSEE/BOEM (permitting), pricing (Argus/OPIS), SCADA (Pi/AVEVA), land (Quorum/P2)** — are all human-portal-first. The first credible agent-native layer for O&G data becomes infrastructure every agent calls.

### The Solution
**"PetroMCP"** — an agent-first data layer exposing O&G datasets via a unified MCP (Model Context Protocol) server + REST + a documented agent-onboarding flow. Agents can: search wells by API number, pull completion reports, fetch current spot gas price for a hub, query a facility's real-time tags, file a permit draft. Built so an agent can discover, authenticate, and start calling in minutes — exactly YC's "Make Something Agents Want."

### Build Plan
**Phase 0 (4 wks)** — Catalog the 10 most-called O&G data needs (well header, production history, permit status, pricing, rig count, facility tags). 

**Phase 1 MVP (10 wks)**
- MCP server exposing 5 high-value data sources: Enverus (or a partner), EIA/Argus public, BSEE public, plus a connector to the operator's own PI/SCADA.
- Auth: API-key + scoped agent credentials; usage metering (agents pay per call).
- Start with **public data** (BSEE/BOEM, EIA, state RRCs, Enverus-free) — no licensing friction.
- Agent docs: a single `petro_mcp` package an agent installs and is immediately productive with; include typed schemas + examples.

**Phase 2 (12 wks)** — Add licensed/commercial data via revenue-share (Enverus, Argus) behind paid keys. Add the operator's *own* PI/Maximo via an on-prem connector (this is where #7 Pulse and #5 FieldBrain plug in).

**Phase 3** — Become the default agent gateway; monetize per-call + premium connectors.

### Moat
- Network effect: agents standardize on it; data vendors want to be listed.
- Once embedded in operators' internal agents, removal is painful.

### Pilot Targets
O&G operators building internal agent platforms (**Exxon, Chevron, Shell's internal AI teams**); O&G SaaS vendors wanting to be "agent-callable."

### Risks & Mitigations
| Risk | Mitigation |
|---|---|
| Data licensing | Start public; let commercial vendors come to you for distribution. |
| Being a thin pipe | Win on unified schemas + agent ergonomics, not raw data. |

---

## #9 — Supply Chain 2.0 (YC: Diana Hu) — adapted to O&G
*Multi-tier visibility, real-time allocation, risk monitoring — for the O&G equipment & spares chain.*

### O&G Why-now
The semicon RFS could be copy-pasted onto **O&G drilling & completion supply chains**: a frac job is held up by a missing zipper sleeve; an ESP failure idles a well for 3 weeks waiting on a bespoke downhole motor; turbine parts cross 6 countries. Operators see their direct suppliers (the OEM) but have zero visibility into sub-tier (the foundry, the heat-treat shop). A $5k part holds up a $50k/day well. Managed in SAP + spreadsheets + phone calls, exactly as YC describes for chips.

### The Solution
**"SparesGraph"** — multi-tier supply-chain visibility + risk engine for O&G critical equipment (ESP downhole, compressors, turbines, BOPs). Maps the bill of materials down to sub-tier suppliers, monitors risk (geopolitical, single-source, lead-time drift), and predicts stockout risk for critical spares — telling the operator "this ESP motor will be short in 8 weeks, pre-buy now."

### Build Plan
**Phase 0 (4 wks)** — Pick one critical-asset category: **ESP downhole equipment** (high-failure-rate, multi-tier, costly downtime).

**Phase 1 MVP (12 wks)**
- BOM explosion: parse OEM BOMs + supplier records into a multi-tier graph (operator → OEM → sub-tier → raw).
- Enrichment: trade data (Panjiva/ImportGenius), supplier financials, geopolitical risk feeds, lead-time signals from PO history.
- Risk scoring: single-source flags, lead-time-drift early warning, concentration risk.
- UI: a graph view + a "critical spares at risk this quarter" report the supply-chain VP actually uses.

**Phase 2 (12 wks)** — Pilot with one operator's procurement team. Metric: catch ≥1 real stockout risk 30+ days before it bites, on a part that would've idled a well.

**Phase 3** — Add automated pre-buy recommendations and a supplier-discovery marketplace.

### Moat
- The multi-tier graph (sub-tier supplier relationships) is proprietary and accumulates.
- Cross-operator demand signal (who's about to need what) enables a marketplace.

### Pilot Targets
Operators with large ESP/compressor fleets: **Permian independents**, plus OEMs (**SLB, Baker, Halliburton**) wanting to manage their own sub-tiers.

### Risks & Mitigations
| Risk | Mitigation |
|---|---|
| Supplier data sparse | Combine trade-data inference + operator PO history + OEM disclosure; ship confidence scores. |

---

## #4 — Dynamic Software Interfaces (YC: Ankit Gupta) — O&G angle
*Users' coding agents radically customize the software they consume.*

### O&G Why-now
O&G is *the* "forward-deployed engineer" industry — every major software vendor (TIBCO/Spotfire, AVEVA, Enverus) ships consultants who hand-build dashboards per asset team. A drilling engineer, a production tech, a revenue accountant, and a trader all use the same production data in wildly different ways. Coding agents can now let each role self-customize — exactly the dynamic-interface future YC describes, with O&G's extreme role-specialization as the proving ground.

### The Solution
**"RoleCanvas"** — an O&G data product that ships *primitives* (a well object, a production time-series, a P&ID, a price curve) + an agent-customizable canvas. Each user's agent assembles their own interface: the drilling engineer gets a time-depth/ROP view; the accountant gets a lease-netback view; the trader gets a basis-differential view — all from the same primitives, each generated/maintained by the user's coding agent.

### Build Plan (condensed)
- Phase 0: scope to **production-data views** (one dataset, many roles).
- Phase 1 (12 wks): build typed primitives + an agent harness (Claude Code-style) that generates a React canvas from a user's natural-language spec; persist per-user layouts.
- Phase 2 (12 wks): pilot at one operator; measure whether each role prefers its agent-built view over the incumbent standard dashboard.
- Moat: the primitive library + accumulated per-role templates; switching cost once agents depend on the primitives.

### Pilot Targets
Operators with mixed asset teams + pain with off-the-shelf dashboards; data-platform-mature independents.

---

## #1 — Hardware Supply Chain / Rapid O&G Part Iteration (YC: Nicolas Dessaigne)
*US hardware iteration is weeks vs. Shenzhen's days.*

### O&G Why-now
O&G constantly needs **bespoke replacement parts** (a worn burner tip, a custom flange, an obsolete pump impeller) where lead times are weeks and a downed compressor costs $50k/day. The YC "rapid iteration" thesis — design to physical part in days — applies directly to **O&G maintenance parts**, especially with on-demand manufacturing + reverse engineering from scans.

### The Solution
**"PartForge"** — design-to-part in days for O&G maintenance: 3D-scan the worn/obsolete part → generative-design a replacement (often topology-optimized, sometimes additively-manufacturable) → route to a vetted network of US shops (CNC, DMLS, casting) → deliver. Handles metallurgy certs for sour-service/H2S compliance.

### Build Plan (condensed)
- Phase 0 (4 wks): one part family — **compressor valve plates / wear components** (high-repeat, metallurgy-known).
- Phase 1 (12 wks): scan-to-CAD pipeline (photogrammetry + structured light) → AI auto-feature-recognition → parametric CAD → DFM check → quote across 3–5 shops.
- Phase 2 (12 wks): paid pilots with midstream maintenance teams; metric: deliver qualified part in ≤7 days vs. 6-week OEM lead time.
- Moat: certified, metallurgy-compliant part library + shop network tuned to O&G specs (NACE MR0175 etc.).

### Pilot Targets
Midstream maintenance teams (**Kinder Morgan, Energy Transfer, Williams**); refinery turnaround planners.

---

## #11 — Inference Chips for Agent Workflows (YC: Diana Hu) — O&G angle
*Silicon designed for the agent loop, not prompt-in/response-out.*

### O&G Why-now
The O&G edge (rigs, offshore platforms, remote wellpads) is **bandwidth-starved and latency-critical**: you can't round-trip a well-control anomaly to a cloud model and back in time. Agent workflows that loop (monitor → reason → act) *must* run at the edge. Today's GPUs are 30–40% utilized on these workloads (per YC) and burn too much power for a solar/battery wellpad. Purpose-built low-power agent-inference silicon is an O&G-edge enabler — but this is a capital-intensive, long-horizon play.

### The Solution
**"EdgeAgent Silicon"** — low-power inference SoC optimized for bursty agent loops (persistent KV cache, fast context switching, speculative decoding) for deployment on rigs/platforms/wellpads running closed-loop control agents. Sold as a module + compiler stack (the Groq insight YC flags: the compiler is the moat).

### Build Plan (condensed, long-horizon)
- Phase 0 (3 mo): partner with an O&G operator to define the edge-agent workload + power envelope; design on an FPGA prototype.
- Phase 1 (9 mo): FPGA-in-the-loop at a wellpad running a real closed-loop agent (e.g., autonomous ESP optimization); prove the utilization/power win.
- Phase 2 (12–18 mo): tape out an ASIC; the compiler stack is the real deliverable.
- Moat: compiler + the loop-specific architecture; deep O&G edge workload expertise.
- Note: only pursue if founding team has chip-design + compiler background; otherwise consume existing edge silicon.

### Pilot Targets
Operators with remote/offshore edge + autonomy ambitions: **offshore majors (Shell, BP, Equinor)**, **autonomous drilling contractors**.

---

## Cross-Cutting Build Notes (applies to all)

**Shared technical spine (build once, reuse across products):**
- **O&G data connectors lib:** WITSML, PRODML, OPC-UA/Modbus, PI Web API, LAS/DLIS, SEG-Y, Maximo/SAP REST. This is the single most valuable reusable asset.
- **O&G entity graph:** well ↔ facility ↔ equipment ↔ SCADA tag ↔ work order ↔ document. Every product above needs it.
- **Physics-grounded agent harness:** every agent answer about the physical world must be checkable against a physics/model constraint; pure LLM answers get operators killed or fined.
- **Human-in-the-loop where liability lives:** regulatory filings, safety procedures, control actions — a licensed human signs.

**Regulatory/safety posture:**
- Never let an LLM do math (allocation, royalties, mass balance) — deterministic engines only.
- Cite every physical-world answer to a source (P&ID, procedure, sensor tag).
- Cybersecurity: ICS/OT segmentation; we are read-only on control systems unless explicitly an actuation product.

**Sequencing recommendation (12-month roadmap to a coherent company):**
1. **Months 0–3:** Ship #3 Compliance-as-a-Service (fastest revenue, lowest technical risk) — funds the rest.
2. **Months 2–6:** In parallel, build the shared data-connectors + entity-graph spine (reused by #5, #7, #10).
3. **Months 4–9:** Ship #5 FieldBrain on top of the spine (highest operator pull, defensible).
4. **Months 6–12:** Layer #10 OpsOS closed-loop + #7 Pulse data platform — these three together become "the AI operating system for an O&G operator," which is the strategic endgame.

This sequence turns five separate YC RFSs into **one coherent company**: an AI-native O&G operating platform (data spine + company brain + closed-loop ops + regulatory service) — exactly the kind of "huge company" YC's "sell to the biggest companies" RFS (#14) describes.

---

## Mapping Summary

| # | YC RFS (Summer 2026) | O&G Solution | Tier |
|---|---|---|---|
| 2 | AI-Native Discovery Engines | ReservoirLoop (EOR/subsurface closed loop) | 1 |
| 3 | AI-Native Service Companies | Compliance-as-a-Service (regulatory filings) | 1 |
| 5 | Company Brain | FieldBrain (executable procedure/skills file) | 1 |
| 7 | SaaS Challengers | Pulse (replace PI Historian) / Maintain (replace Maximo) | 1 |
| 10 | AI OS for Companies | OpsOS (closed-loop asset operations) | 1 |
| 6 | Counter-Swarm Defense | SwarmShield (facility anti-drone) | 2 |
| 8 | Software for Agents | PetroMCP (agent-first O&G data layer) | 2 |
| 9 | Supply Chain 2.0 | SparesGraph (multi-tier critical-spares risk) | 2 |
| 4 | Dynamic Software Interfaces | RoleCanvas (agent-customized role views) | 3 |
| 1 | Hardware Supply Chain | PartForge (scan-to-part in days) | 3 |
| 11 | Inference Chips for Agents | EdgeAgent Silicon (low-power edge agent SoC) | 3 |

*Omitted (no strong O&G angle): AI for Low-Pesticide Agriculture, AI Personalized Medicine, Electronics/Industrial Capabilities in Space.*
