# Top Unsolved AI Problems in Oil & Gas — Phase 2 Build Plans

Companion to `YC-AI-problems-OilGas-solutions-plan.md`. While that document mapped YC's top unsolved AI problems (Summer 2026 RFS) into O&G opportunities, **this Phase 2 document tackles 12 unsolved AI problems that are *inherent to oil & gas itself*** — problems YC will not list because they are domain-native.

Each problem is approached with the same structure as Phase 1:
- The Problem (what's unsolved, why now)
- O&G Why-now (current pain + the technological inflection point)
- The Solution (product we build)
- Build Plan (Phase 0 → Phase 3, including data, models, validation)
- Moat / Defensibility
- Initial Customers / Pilot Targets
- Risks & Mitigations

---

## Priority Ranking (by revenue pull × technical specificity × competitive moat)

| # | Problem | Tier | Rationale |
|---|---|---|---|
| 1 | Predictive Maintenance for Rotating Equipment | **Tier 1** | $50k–$500k/day downtime; clear data path; defensible. |
| 2 | Methane Leak Detection & Quantification | **Tier 1** | Regulatory pull (EPA Subpart W, EU Methane Reg); satellite + sensor convergence. |
| 3 | Real-time Drilling Optimization | **Tier 1** | Largest per-event value (kick prevention = life + $$); high-fidelity telemetry exists. |
| 4 | Automated Well Log Interpretation & Lithology | **Tier 1** | High-volume repetitive work; clear training data; vision-models meet petrophysics. |
| 5 | Hazardous Event Prediction (blowouts/spills/gas clouds) | **Tier 1** | High consequence; insurance + regulator pull; explainability required. |
| 6 | Corrosion Rate Prediction | **Tier 2** | Massive TAM (every pipeline+ every well); sensor data maturing. |
| 7 | Energy Price / Margin Forecasting under Shocks | **Tier 2** | Trading desks hungry for non-naive forecasts; geopolitical news NLP angle. |
| 8 | CCUS CO2 Injection Monitoring | **Tier 2** | 45Q + IRA funding wave building; monitoring is the gating constraint. |
| 9 | Supply Chain Disruption Prediction | **Tier 2** | Multi-tier mapping problem (overlaps with YC #9 SparesGraph). |
| 10 | Subsurface Uncertainty Quantification | **Tier 3** | Pairs with Phase 1 #2 ReservoirLoop; long procurement cycles. |
| 11 | H2S Exposure Prediction | **Tier 3** | Niche but very high consequence; sensor + weather + ops data. |
| 12 | Production Forecasting with Limited Data | **Tier 3** | Hard problem; overlaps with #10 and Phase 1 #2. |

---

## #1 — Predictive Maintenance for Rotating Equipment (compressors, pumps, turbines)

### The Problem
A single unplanned compressor trip on a major gas pipeline or midstream facility costs $50k–$500k/day in lost production and emergency repair. A failed ESP on a well can idle 1,000–3,000 bo/d for weeks. Rotating equipment dominates unplanned downtime in oil & gas (≈60% of total), yet most operators run **vibration/temperature thresholds** set in the 1990s. They detect failure after it starts, not before it begins. Failure modes (bearing wear, impeller fouling, seal degradation, surge) have long warning patterns in vibration spectra, lube-oil debris, and process variables — *if* the right signal is extracted.

### O&G Why-now
Three forces converge: (1) cheap industrial vibration+acoustic sensors everywhere (IIoT); (2) high-frequency time-series + foundation models now handle 10–100k sample/sec streams; (3) the previous vendor cycle (Bently Nevada, SKF) sells hardware well but not predictive models. Mid-tier operators are unsatisfied; they want a SaaS-style PdM that doesn't require a 6-month PS engagement to deploy.

### The Solution
**"SpectraGuard"** — a PdM SaaS for O&G rotating equipment that turns raw high-frequency vibration + process data into:
- **Failure-mode-specific early warnings** (e.g., "inner-race bearing defect on Compressor C-201, 14d ± 3d to critical").
- **Remaining-useful-life (RUL) distributions**, not point estimates (per problem-1's requirement for *quantified confidence*).
- **Root-cause attribution** with citations to the exact sensor and feature driving the alarm.
- **Recommended action**: "swap bearing within next planned shutdown window."

Equipment classes covered (sequenced by TAM × ease):
1. **Reciprocating compressors** (midstream gas processing, gas lift).
2. **Centrifugal compressors** (gas pipelines, LNG).
3. **ESP downhole pumps**.
4. **Centrifugal pumps** (water injection, oil transfer).
5. **Gas turbines** (compressors drivers, power co-gen).

### Build Plan

**Phase 0 (4 wks) — Pick one asset class, validate with one operator.**
- Target: **reciprocating compressors** at a gas-processing plant. Volume, high downtime cost, accessible vibration data.
- Get a paid Letter-of-Intent from one operator (e.g., a midstream processor) for a 90-day paid pilot.

**Phase 1 MVP (12 wks) — Single-class, edge + cloud.**
- **Edge collector** on a gateway PC: pull vibration (typically 25.6 kHz), process variables, lube-oil report; compress + forward.
- **Signal-processing pipeline**: spectral features (FFT, envelope analysis, kurtosis), wavelet features, statistical moments — feed into a normalized feature store.
- **Model**: a **mixture-of-experts** model trained on (a) operator's labeled failure history (closed-set) and (b) large-scale synthetic fault injection (rotor dynamics simulators). Outputs a calibrated failure-mode probability + RUL distribution (e.g., bayesian survival model, MC-dropout CNN, or a mixture-density network).
- **Uncertainty quantification is the core differentiator**: not just "alert," but "23% prob. inner-race defect, if so 95% CI of 11–18 days to critical."
- **Citations**: every alert references the specific fault frequency (e.g., 142.3 Hz ≈ BPFI of bearing SKF-6312) and the sensortag that drove it.
- **Operator UX**: alert dashboard ranked by severity × RUL × criticality; integrates with Maximo via API to auto-create a work order when action is recommended.

**Phase 2 (16 wks) — First paying pilot.**
- 5–10 rotating assets at one midstream operator.
- Service pricing: **per-asset-month** ($X/asset/month), paid quarterly with a "success fee" tied to net downtime reduction vs. trailing 12 months.
- Success metric: ≥30% reduction in unplanned downtime events on pilot assets over 6 months; ≥1 actionable alert at least 14 days before a true failure.

**Phase 3 (12 mo) — Generalize across equipment classes.**
- Add classes one by one (centrifugal compressor, ESP, pump, turbine) with a **transfer-learning foundation model** for vibration/rotodynamics.
- Cross-fleet benchmarks ("your C-201 is degrading 2σ faster than peer fleet average").

### Moat
- **Each failure observed and verified is a permanent training data point.** Operators retain ownership but share for fleet benchmarks via federated learning.
- **Fault-injection rigs** (we run a partner rotor-dynamics test rig to inject known faults) generate truthful synthetic data impossible to fake — powers the model before scale.
- Cross-asset benchmark dataset compounds.

### Pilot Targets
- Midstream gas processors with large compressor fleets: **Energy Transfer, Williams, Enterprise, Targa**.
- Independent operators with large ESP fleets: **Civitas, Vital Energy**.
- Equipment OEMs wanting to offer PdM as a service: **Solar Turbines, Elliott, Cameron (SLB)**.

### Risks & Mitigations
| Risk | Mitigation |
|---|---|
| False alarms destroy trust | Calibrate via TCE/ROC on pilot; allow tuning per asset; default to *high precision* over recall until operator trusts us. |
| IT/OT security reviews slow deployment | Edge-first deployment keeps OT segmented; cloud is for analytics, never control. |
| Long approval cycles at midstream | Start with **independent E&Ps** (faster, friendly); midstream follows once proven. |

---

## #2 — Methane Leak Detection & Quantification (LDAR + Continuous Monitoring)

### The Problem
Methane is a 84× CO2-equivalent over 20 years and the fastest lever the oil & gas industry has to cut emissions. **Detection** is solved (satellites, flyovers, OGI cameras all find leaks), but **quantification** at the asset level is unsolved. EPA Subpart W (US) and the EU Methane Regulation require operators to *measure* and report facility-level methane intensity with audit-grade accuracy. Today's reports use emission factors from the EPA Greenhouse Gas Reporting Program — generic, not asset-specific, and now known to be 2–5× wrong on individual facilities. Operators have no defensible way to say "our actual methane intensity is X."

### O&G Why-now
Three converging forces:
1. **LDAR regulations tighten annually.** EU Methane Reg bans routine venting by 2027. US EPA Methane Rule (2024) requires quarterly LDAR surveys + continuous monitoring at high-emitting sites.
2. **Satellites + aerial sensors generate petabytes** of methane imagery (MethaneSAT, Carbon Mapper, GHGSat) — operators can't review it.
3. **Foundation/vision models** can quantify plumes from hyperspectral imagery within ±20% on benchmarked test sets.

### The Solution
**"MethaneIQ"** — a continuous methane detection + quantification + reporting stack. Three layers:
1. **Ingest all available methane observations** at the facility: satellite revisits, aerial OGI campaigns, fixed continuous sensors (point + LIDAR/OP-FTIR), drone flyovers.
2. **Quantification engine**: Bayesian inversion plume model + ML correction. Input: imagery, wind, prior facility emission factor → posterior: facility-level emission rate in kg/hr with a credible interval.
3. **Reporting**: autofills EPA Subpart W, OGCI Methane Tracker, EU Methane Reg, ONE Future, MiQ. Every emission is sourced and quantified; reports are audit-grade.

### Build Plan

**Phase 0 (4 wks) — Wedge to one reporting mandate.**
- Pick **EPA Subpart W petroleum & natural gas systems reporting** (mandatory, annual, well-defined schema, used by 1,700+ US operators).

**Phase 1 MVP (12 wks) — Quantification engine.**
- Data ingest: Carbon Mapper / MethaneSAT open detections; commercial satellite via GHGSat subscription; OGI campaign reports as PDF.
- Model: standard Gaussian/IF plume inversion from hyperspectral imagery; train a residual ML model that learns the facility-specific correction (wind variability, sensor geometry).
- Validation: partner with an operator that has **controlled-release test data** (e.g., a Permian research site with ground-truth release quantity) to validate the model to within ±25%.
- Report generator: pulls quantification → fills Subpart W → produces a PDF report with citations.

**Phase 2 (12 wks) — Paid report + continuous monitoring.**
- Sell as a **report-as-a-service**: $8k–40k/report depending on asset count. 50% off the cost of an outside consultancy.
- Add fixed-sensor integration (Aeromon, Sensirion, LI-COR) for sites needing continuous monitoring per 2024 EPA rule.

**Phase 3 (12 mo) — Real-time LDAR dashboard.**
- Operator sees a methane map of all assets in real time; alerts on new detections with quant.
- Cross-asset benchmarks: how does our facility rank vs. peer facilities in the basin?

### Moat
- **Quantified, audited facility-level emissions data accumulated across customers** = the definitive dataset. New entrants can't reproduce.
- Direct integration with operator compliance workflows.

### Pilot Targets
- Mid-cap operators with public methane commitments: **Civitas, EQT, Devon, Range Resources**.
- NOCs with active methane programs: **Equinor, Saudi Aramco, ADNOC**.
- ESG-focused PE-backed E&Ps needing credible reports.

### Risks & Mitigations
| Risk | Mitigation |
|---|---|
| Quantification uncertainty invites operator skepticism | Show calibrated intervals, not point estimates; provide scenario breakdowns. |
| Satellite-only data insufficient | Combine multiple modalities; quantify the gain per modality. |
| Regulatory uncertainty (Subpart W amendments) | Modular report templates; switch schemas in days. |

---

## #3 — Real-time Drilling Optimization

### The Problem
Stuck pipe, kicks, lost circulation, and torque/drag anomalies cause **non-productive time (NPT)** that costs $50k–$300k/day and is the #1 driver of dry-hole cost overrun on unconventional wells. Today, drilling optimization is (a) reactive (driller sees a problem, reacts) or (b) rule-based (a static setpoint ML system that doesn't adapt). The real-time decision problem — *given current downhole conditions, what weight-on-bit / RPM / flowrate maximizes ROP while staying within the safe operating envelope?* — is unsolved. The data exists (every rig streams WITSML or Pason at <10s latency) but the optimization surface is non-stationary, multi-objective, and high-stakes.

### O&G Why-now
1. **Long laterals + unconventional** = rigs now spend $4–8M/well on drilling alone; a 5% drilling-time saving per well is hundreds of millions per operator per year.
2. **WITSML streaming + edge compute** — modern drilling rigs have gateways that can host ML.
3. **RNN / Transformer models** for time-series optimization have finally surpassed human experts on benchmark industrial-control tasks.
4. **Digital twins of BHA + drilling dynamics** are now realistic (rotor dynamics, hydraulics models).

### The Solution
**"DrillOptix"** — a real-time drilling advisor running on the rig edge. Combines:
1. **Digital twin** of BHA dynamics + hydraulics (continuously calibrated to real-time data).
2. **Multi-objective recommender**: outputs the safe operating envelope (max WOB / max RPM) and a recommended point inside it balancing **ROP**, **MSE** (mechanical specific energy = efficiency), and **risk** (kick, stick-slip, fatigue).
3. **Anomaly detector** that flags early signs of stuck pipe, kick, lost circulation, bit balling — with **explainability** (which sensor drove it).
4. **Driller-facing UI**: large tablet readouts; changes are *advisory* (the human driller is always in the loop) until trust is earned.

### Build Plan

**Phase 0 (4 wks) — Pick one well type.**
- Target: **Permian Basin Wolfcamp horizontal** (most-drilled US formation, abundant data on public completion reports, plenty of contractors willing to pilot).
- Partner: an autonomous-drilling contractor (e.g., **Patterson-UTI's auto-drilling division** or **Saudi Aramco's autonomous rigs**).

**Phase 1 (16 wks) — MVP on simulator + 1 well.**
- Build digital twin: a BHA dynamics + hydraulics model in Python/C++, validated against historical wells.
- Build ML recommender: a constrained Bayesian-optimization agent with safety guardrails; train on historical wells (10k+ offset wells on Enverus) using offline RL.
- Validate in **drilling simulator** (partner with e.g., **Ritter's SIM** or NOV's TerraVolve) before going live.
- Run on **one well** in shadow mode: compare recommendations to the actual driller's choices; build trust.

**Phase 2 (12 wks) — 5-well pilot, advisory mode.**
- Run on 5 wells across one contractor; recommendations are *advised*, not enforced.
- Metric: hit ≥80% acceptance rate from senior drillers; ≥8% ROP improvement vs. offset wells.

**Phase 3 — Closed-loop autonomy** (with operator + contractor + insurance). Only after regulators + safety case are aligned.

### Moat
- Each well is a labeled trajectory: driller-accepted vs. rejected, ROP delta; the data accumulates.
- The digital twin is calibrated to each operator's BHA inventory — switching cost is the calibration, not the algorithm.

### Pilot Targets
- Drilling contractors: **Patterson-UTI, Nabors, Helmerich & Payne (HP)**.
- Operators with autonomous-drilling ambitions: **Saudi Aramco, ADNOC, Exxon, Chevron**.
- Service cos with digital offerings: **SLB (Schlumberger), Halliburton, Baker Hughes**.

### Risks & Mitigations
| Risk | Mitigation |
|---|---|
| Safety incident from bad recommendation | Hard safeguards: agent recommends only inside proven safety envelope; advisory-only for v1; driller override always wins. |
| Driller union pushback | Frame as "the driller's copilot," not replacement; emphasize driller controls what to act on. |
| Rig-vendor lock-in | Run on a vendor-neutral rig gateway (Pason / Verdazo / Pelican). |

---

## #4 — Automated Well Log Interpretation & Lithology Classification

### The Problem
Every well produces a suite of logs (gamma ray, resistivity, density, neutron porosity, sonic, image logs, NMR, formation testers). **Lithology identification + pay flagging** is the foundational petrophysical analysis — done today by humans (log analysts) at 3–8 hours per well per analyst. ~10,000 wells drilled/year in the US alone; backlog of legacy wells to be reinterpretation is hundreds of thousands. Error rates are meaningful (analyst disagreement on the same log is common on subtle lithologies). Multiple log vendors, multiple vintages, multiple log sets — analytical heuristics are siloed per company.

### O&G Why-now
1. **Vision-language and multimodal foundation models** (e.g., Llama, Claude with vision, SigLIP) have crossed the threshold on geoscience visual reasoning benchmarks.
2. **Standardized training labels**: the public USGS / state geological survey core+log datasets provide thousands of pairings.
3. **Operator pain**: backlog of wells never reinterpreted even when new logs/completions arrive; senior petrophysicists retiring.

### The Solution
**"LogLlama"** — an AI log analyst that ingests raw LAS/DLIS log curves + any image logs + core photos, and outputs:
1. **Lithology column** at every depth (with calibrated probability).
2. **Pay flag** (per the operator's own definition, configurable).
3. **Petrophysical properties** (porosity, Sw, permeability index) with uncertainty.
4. **Citation overlay** on the depth track: which curve drove each classification.
5. **QC flags** for tool errors, depth-shift issues, missing curves.

The product runs as **batch reanalysis** for legacy wells + **auto-flagging on delivery** for new wells.

### Build Plan

**Phase 0 (3 wks) — Narrow vertical.**
- Focus on **clastic lithology classification** (sand/shale/coal/dolomite) from conventional triple-combo logs — best training data, fastest path to a benchmark.

**Phase 1 (12 wks) — MVP.**
- Training data: USGS Core Research Center (free, core-photo+log pairs), West Texas / DJ Basin public logs.
- Architecture: a multimodal model that ingests (a) LAS log curves as 1D time-series, (b) image logs (FMI/ OBMI) as 2D rasters, (c) optional core photos. Outputs a lithology column + per-depth uncertainty.
- Validate against held-out USGS wells + a paid blind test from a small operator.

**Phase 2 (12 wks) — Paid batch reanalysis.**
- Target: a mid-cap operator with 500–2,000 legacy wells.
- Sell: **per-well reanalysis fee** ($X/well) + flagging SaaS for new wells (per-well/month).
- Hard constraint: human petrophysicist reviews and signs off — *we are not replacing the analyst, we are eliminating the boring 80%*.

**Phase 3 — Specialized variants.**
- Carbonates (different log response), unconventional minerals, shale plays, image-log interpretation specialists (fractures, vugs, in-situ stress).

### Moat
- Public datasets seed the model; completion of each paid well interpretation adds proprietary labeled data that competitors don't see.
- Industry-standard LAS/DLIS + depth-tracking pipeline is the practical moat — this is engineering-heavy.

### Pilot Targets
- **Public datasets**: USGS, BEG (TX), KGS (KS), NOGS (ND), state surveys.
- **Mid-cap independents with large backlogs**: **Diamondback, Civitas, Devon, EOG, Continental Resources**.
- **NOCs with historical archives**: **Petrobras, Pemex, Saudi Aramco**.

### Risks & Mitigations
| Risk | Mitigation |
|---|---|
| Petrophysicists reject output | Frame as an analyst multiplier (10× throughput, senior reviews the flagged cases). |
| Calibration across formations | Show training-data provenance per prediction; per-basin fine-tuning offered. |

---

## #5 — Hazardous Event Prediction (blowouts, spills, gas clouds) — with explainability

### The Problem
The industry still suffers blowouts (Deepwater Horizon aftermath regs, recent Padre Island incidents), spills (pipeline ruptures, tank overfills), and toxic gas clouds (H2S, H2S releases, LNG vapor dispersion). Predicting and *preventing* these is unsolved. Operators have thresholds (e.g., "stop drilling if kick >X bbl") but no probabilistic look-ahead. **Insurance carriers** and **regulators** (BSEE, EPA, PHMSA) increasingly demand quantified risk models, not just compliance with prescriptive rules.

### O&G Why-now
1. **Multi-source telemetry** (SCADA, IoT gas detectors, drone surveys, weather, pipeline inspection) is increasingly fused.
2. **Foundation models + causal/structural ML** can output not just predictions but explanations ("this gas cloud is 87% likely to reach populated area because of wind direction + sensor lag").
3. **Explainable AI (XAI)** tooling (SHAP, attention maps, surrogate models) has matured; regulators accept shapley-style explanations for high-stakes decisions.

### The Solution
**"RiskLens"** —a hazard-prediction + explainability platform covering:
1. **Blowout prediction** (drilling + workover): real-time Pore-Pressure-Fracture-Gradient monitoring + ML anomaly detection; predicts kick → blowout likelihood 5–30 min ahead.
2. **Pipeline-spill prediction**: integrates MFL/ILI inspection history, SCADA pressure, soil/groundwater sensors, weather → predicts rupture probability per segment over 7/30/90-day windows.
3. **Gas-cloud dispersion (H2S, LNG vapor)**: real-time release detection + Gaussian/LSTM dispersion model → predicts populated area exposure with uncertainty.
4. **Explainability layer**: every prediction is annotated with the top-3 contributing sensor signals + a SHAP-style attribution. Includes a plain-English narrative.
5. **Reporting**: counters for BSEE SEMS, EPA RMP, PHMSA, CFATS.

### Build Plan

**Phase 0 (6 wks) — Vertical choice & hazard class.**
- Pick **pipeline-spill prediction** first. PHMSA Mega-Rule (2023+) + the new 2024 methane rules have created intense regulatory demand for *proactive* risk assessment.
- Permian Delaware basin has 20k+ miles of gathering pipelines + many known corrosion hotspots — perfect pilot.

**Phase 1 (16 wks) — MVP.**
- Data: pipe segment specs (age, diameter, grade, MAOP), ILI/MFL historical anomalies, SCADA pressure, soil corrosivity, weather.
- Model: gradient-boosted survival model + a transformer that ingests SCADA sequences. Outputs: per-segment rupture probability per 30-day window.
- Uncertainty: Bayesian posterior; calibration validated against PHMSA incident database historic events.
- XAI layer: SHAP attributions per prediction + a generative narrative ("Segment 47 is high-risk: 1984-vintage pipe, two severity-3 metal-loss features within 50ft, recent cathodic-protection drop, soil resistivity 2× regional average — recommended action: dig + replace").
- API + dashboard.

**Phase 2 (12 wks) — First paid pilot.**
- Customer: a Permian midstream operator with a known incident history.
- Output: ranked list of top-50 segments by 90-day rupture risk. Operator digs the top 5; statistically, we'd expect ≥1 incipient corrosion pit found.
- Success metric: ≥1 real finding that prevents an incident within 90 days; ROI proof to insurer + auditor.

**Phase 3 — Add blowout & gas-cloud modules.**
- Same platform, same XAI spine — different data connectors.
- Insurer partnerships: Munich Re, Liberty Mutual; their willingness to *price based on our risk score* = commercial credibility.

### Moat
- **Verified-caught-it-real-life cases** are the strongest case-study asset in this space. Insurers + regulators care.
- Integrated XAI narrative is hard to replicate; pure-PdM vendors don't carry explainability.

### Pilot Targets
- Midstream pipeline operators with active integrity programs: **Kinder Morgan, Energy Transfer, Williams, Enbridge**.
- Drilling-focused E&Ps needing blowout prediction: **BP, Exxon, Chevron, Saudi Aramco**.
- **Insurance loss-control teams** willing to fund pilots in exchange for risk-reduction evidence.

### Risks & Mitigations
| Risk | Mitigation |
|---|---|
| Negligence liability ("you warned but they didn't act") | Build the audit trail in: every prediction + action-recommendation is logged + signed by a person. |
| Regulator skepticism of AI risk predictions | Frame as "operator decision support"; keep the human in the decision. |
| Availability of labeled incidents | Augment limited real incidents with synthetic injection from PHMSA/Operators data + safety-case literature. |

---

## #6 — Corrosion Rate Prediction (pipelines + downhole)

### The Problem
Corrosion is responsible for ~25% of pipeline incidents and billions in replacement/downtime costs. Today's corrosion management is split between (a) coupon testing + (b) ILI/MFL inline inspections every 5–7 years. The interval *between* inspections is a black box; operators use generic corrosion-rate lookups from NACE standards. **CO2/H2S/sour-service corrosion in downhole tubulars** is even harder to predict (multiphase flow + chemistry + temperature + stress). Every operator would pay for a model that says "your pipe is corroding 1.4× faster than baseline because of these sensors; dig here next."

### O&G Why-now
1. Modern **fiber-optic DAS + DTS** distributed sensing in pipelines gives continuous strain+temperature, which couples with corrosion models.
2. **Multi-physics ML** combining fluid chemistry, multiphase flow, electrochemistry is feasible with current libraries.
3. **ILI vendor data** (Rosen, GE Pipeline Solutions, T.D. Williamson) is partially de-identified-listed; combined with soil/resistivity + cathodic-protection data, a real model is possible.

### The Solution
**"CorroScope"** —a corrosion-rate prediction engine for (a) pipelines (external + internal) and (b) downhole tubulars (sweet + sour service). Inputs:
- Fluid chemistry (H2S, CO2, O2, water cut, salinity).
- Multiphase flow regime + temperature + pressure profile.
- Cathodic-protection status (pipe-to-soil potential readings).
- ILI historical feature growth (matched across runs to compute actual rate).
- Soil resistivity, drainage, burial depth.
- For downhole: BHA + completion metallurgy, deviation, bottomhole temperature profile.

Output:
- Per-segment/per-joint **corrosion-rate distribution** with confidence.
- **Time-to-critical-wall-thickness** distribution.
- **Recommended action** (ILI re-run prioritized, coupon install, replacement schedule, inhibitor dose adjustment).
- **Driver attribution**: top-3 contributions (e.g., "this joint is corroding 2× baseline due to low CP + high soil resistivity + low inhibitor residual").

### Build Plan

**Phase 0 (4 wks)** — Pipeline external corrosion first. Most data available; fewest stakeholders; clearest first wedge.

**Phase 1 MVP (12 wks)**
- Data: assimilate a public PHMSA incident dataset + a partner operator's CP/soil data + ILI history (de-identified).
- Model: physics-informed corrosion-rate predictor. Domain base model (NACE/DEWaard-Milliams CO2 corrosion) wrapped in an ML residual that learns facility-specific deviations.
- Validate: cross-ILI-rate-on-de-identified-ILI-history.
- Bring your own data (BYOD) ingestion: pipeline operator uploads CP/soil/ILI/fluid; outputs reports.

**Phase 2 (12 wks)** — Pilot at one midstream operator on a representative segment. Output: ranked pipe segments to re-dig.

**Phase 3** — Extend to (a) downhole corrosion in sweet/sour wells (ESP/completion strings), (b) inhibitor dose optimization (loop corrosion rate + inhibitor residual sensor feedback to dosing pump).

### Moat
- ILI multi-run matched datasets across operators generate **proprietary corrosion-growth features** no vendor has consolidated.

### Pilot Targets
- **Midstream pipeline operators** (most external-corrosion spend): **Kinder Morgan, Williams, Enbridge, Energy Transfer**.
- **Upstream operators with sour-service wells**: **Saudi Aramco, ADNOC, Occidental, ConocoPhillips**.
- **ILI vendors** wanting to enrich their reports with predictive analytics: **Rosen, GE PII**.

### Risks & Mitigations
| Risk | Mitigation |
|---|---|
| Wrong corrosion-rate = premature dig or missed failure | Calibrated intervals, not point estimates; conservative defaults; align with operator's own NACE audit methodology. |
| Data availability | Start with public PHMSA + de-identified operator data; build BYOD ingestion early. |

---

## #7 — Energy Price / Margin Forecasting under Geopolitical Shocks

### The Problem
Crude oil, natural gas, NGL, refined product prices swing on hurricane outages, OPEC announcements, Russia-Saudi tensions, China demand surprises, weather, and LNG flows. Standard time-series models (ARIMA, GARCH, ML) trained only on prices **don't incorporate geopolitical text + supply/demand fundamentals well**. Trading desks at supermajors, utilities, refiners, and large E&Ps use human analysts to *interpret* headlines + supply data and forecast prices — and consistently add value over naive baselines during shocks.

### O&G Why-now
1. **Satellite + alternative data** (tanker AIS, storage levels via satellite, satellite-derived refinery runs) is now real-time.
2. **LLM news reading** can extract structured events (e.g., "Saudi Aramco extends 1 mb/d cut for another month") with high reliability.
3. **Geopolitical event databases + supply/demand fundamentals** are now structured enough to serve as model features.

### The Solution
**"GeoEdge"** — a price/margin forecasting platform combining:
1. **Alternative-data ingest**: tanker AIS (BloombergLP / Kpler / Vortexa), satellite imagery (Orbital Insight), refinery throughput, storage levels, weather.
2. **News/event extraction**: LLM pipeline that turns headlines + ministerial statements + OPEC communiques into structured supply/demand shocks with provenance.
3. **Hybrid forecasting model**: physics/structural supply-demand balance + ML residual on price; produces both point forecasts AND probability distributions.
4. **Margin forecast extension**: crack spreads, basis differentials, NGL frac spreads.
5. **Explainability**: every forecast shift attributed to top-3 causal drivers (e.g., "your Brent forecast moved +2.30 today because: (a) AIS shows 3 Saudi tankers heading west (likely loading), (b) Hurricane Marco path shift, (c) Libya restart delayed").
6. **Stress scenarios**: pre-computed forward curves under geopolitical shocks (e.g., "WTI if Russia fully shuts gas to EU this winter").

### Build Plan

**Phase 0 (4 wks)** — Target one product: **Henry Hub natural gas forward curve** (most volatile US commodity, high LM/utility buyer need, accessible data).

**Phase 1 MVP (12 wks)**
- Ingest: EIA storage reports, weather (NOAA), LNG cargo flow data, pipeline bulletins, gas rig count (Baker Hughes).
- News extraction LLM pipeline on Reuters/Argus/Platts headlines.
- Produce: 30-day forward price distribution + driver attribution.

**Phase 2 (12 wks)** — Sell to a mid-cap E&P + a mid-cap utility. They forecast both upstream commodity exposure (E&P) and input cost (utility regulated business).
- Pricing: SaaS subscription $50k–500k/year per seat or desk.

**Phase 3** — Extend to WTI/Brent, refined products, NGLs; build portfolio-level margin forecasts.

### Moat
- **Tradeable forecast accuracy** is the moat: track realized forecast errors in real time across all customers; transparent leaderboard.
- Alternative-data integration with LLM-extracted events is hard to replicate quickly.

### Pilot Targets
- **Mid-cap E&P trading desks**: **Occidental, Hess (**now Chevron**)**, Continental, EOG.
- **Midstream / utilities** with active hedging: **Kinder Morgan, Sempra Infrastructure**, **Calpine**.
- **Refiners**: **Marathon, Valero, Phillips 66** — most exposed to crack spread forecasting.

### Risks & Mitigations
| Risk | Mitigation |
|---|---|
| In-House quant team is the buyer | Position as "your team's multiplier," not replacement. |
| Forecast shocks wrong → reputational damage | Always publish a distribution, never a point estimate; transparent leaderboard (own worst critic). |

---

## #8 — CCUS CO2 Injection Monitoring

### The Problem
Carbon Capture, Utilization & Storage (CCUS) is scaling under the 45Q tax credit ($85/ton for geologic storage) and Inflation Reduction Act funding. But **MRV (Measurement, Reporting, Verification) is the gating constraint** — every ton of CO2 stored must be auditable. Today, operators rely on sparse well monitoring + periodic 3D seismic + benchmark simulations. Static accounting is at risk of being challenged (Denbury/Denver-Julesburg basin, Gulf Coast Class VI injection wells) as volumes scale.

### O&G Why-now
1. **Class VI injection well permits are accelerating** (~140 active, EPA target 100s/year).
2. **Satellite InSAR (surface deformation)** can measure mm-level uplift over injection zones — a direct proxy for stored volume + pressure.
3. **4D seismic + DAS** (distributed acoustic sensing) deployed in observation wells give high-resolution signals.
4. **EPA Subpart RR** requires annual monitoring, reporting, verification — operators struggle with quantification.
5. **Insurance + 45Q claim integrity** is increasingly litigated.

### The Solution
**"CO2Seal"** — CCUS MRV (Measurement, Reporting, Verification) + risk platform:
1. **Multi-modal data ingest**: InSAR (Sentinel-1 commercial), 4D seismic, DAS in observation wells, gas sampling in overlying formations, injection well telemetry (rate, pressure, temperature).
2. **Reservoir-pressure + plume simulator** (calibrated to data) coupled with a learned residual that improves over time per-site.
3. **Verification outputs**: per-quarter stored-tonnage with calibrated uncertainty; plume extent; pressure-front migrations; potential-leakage indicator (e.g., soil-gas-CO2 anomalies, vegetation stress from InSAR).
4. **Subpart RR auto-reporting**: every required field filled, every claim auditable.
5. **Risk layer**: leak-back probability per storage complex over 1/10/100-year horizons; cited risk drivers.

### Build Plan

**Phase 0 (6 wks)** — Vertical: **Gulf Coast Class VI saline-storage sites** (active, growing fast, deep expertise available).

**Phase 1 MVP (14 wks)**
- Ingest InSAR, injection-well telemetry, 4D seismic reprocessed against baseline.
- Build a **reservoir simulation surrogate** (parameterized TOUGH2 outputs) + ML residual for sites with >6 months data.
- Quantify stored CO2 mass with credible interval.

**Phase 2 (14 wks)** — Pilot with a Class VI operator (e.g., **ExxonMobil (Baytown/Lee County), Talos, 1PointFive (Oxy)**).
- Sell: per-injection-well/year MRV-as-a-service ($X/well/year).
- Deliver: Subpart RR report (drafted) + InSAR-derived uplift map + leakage-risk dashboard.

**Phase 3** — Add MMV scenarios for lower-pressure Class II EOR storage; pre-acquisition diligence (storage prospect sell-side).

### Moat
- **Multi-site time-series of InSAR + reservoir state** — proprietary data asset.
- Successful Subpart RR accentances + EPA relationships = regulatory moat.

### Pilot Targets
- **Class VI operators**: **ExxonMobil, Talos, 1PointFive (Oxy subsidiary), Denbury (now Exxon)**.
- **45Q-active cos**: **Chevron, BP, Devon**.
- **Storage prospect developers**: **Carbon America, Vaulted Deep**.

### Risks & Mitigations
| Risk | Mitigation |
|---|---|
| EPA rule changes | Modular Subpart RR templates; versioning. |
| Reservoir sim complexity | Use ML surrogate as the fast path; full physics as ground truth. |

---

## #9 — Supply Chain Disruption Prediction (rig availability, tubular goods, sand, water)

### The Problem
The O&G supply chain — drilling rigs, tubulars, frac sand, water, chemicals, completion crews — is managed with phone calls + spreadsheets. **A single missing proppant trailer or a rig stuck in Alaska** can hold up a $5M completion. Today's operators find out **too late**. The signals that predict disruption (rig moves visible on satellite/AIS, sand mine production visible on satellite imagery, weather disruptions, road/permit backlogs) exist but aren't aggregated or predicted.

### O&G Why-now
1. **AIS + satellite + customs + permit data** all available at mass scale.
2. **Graph ML** for multi-tier supply-chain risk is now practical (cf. YC's #9 Supply Chain 2.0).
3. **Urgent operator pain**: post-pandemic + post-hurricane disruption has cost operators billions.
4. **OE-supplier data** (Halliburton, SLB rig counts) published regularly; competitive intelligence is openly analyzable.

### The Solution
**"WellStock"** — a predictive supply-chain intelligence platform for well-level operations:
1. **Rig-availability forecast**: today's rig counts + velocity → 30/60/90-day forward availability by basin; rig-move patterns predicting future availability.
2. **Sand supply forecast**: monitor frac-sand mine production (satellite), rail demand, inventory at terminals → forecast sand tightness.
3. **Tubular forecast**: import data + mill lead times + queue times at threading plants → forecast getting OCTG/P110/L80.
4. **Water & chemicals**: source well capacity, weather-driven supply swings.
5. **Per-well-level disruption probability**: for each scheduled well, compute the probability of slip + the bottleneck driver.

### Build Plan

**Phase 0 (4 wks)** — Pick the **Permian frac sand** market (most disrupted supply chain in US oil & gas, massive TAM, satellite-detectable).

**Phase 1 (12 wks)**
- Data: Sentinel-2 imagery over Permian sand mines (Atlas, Proppant Logistics, High Roller, US Silica sites) — detect production proxies (vehicle count, conveyor activity) weekly.
- AIS on rail cars (where legal) for sand delivery.
- Build sand-tightness score: a weekly tightness signal validated against industry contacts.
- Output: dashboard, email alerts.

**Phase 2 (12 wks)** — Paid pilot: a mid-cap Permian operator (Civitas, Vital, Crescent). Metric: how many quarterly frac schedule slips we predicted correctly in advance.

**Phase 3** — Extend to rig availability (satellite-AIS tracking of drilling rigs), tubular tracking.

### Moat
- **Time-series of supply signals + verified disruption events** = proprietary supply-chain ground-truth dataset.

### Pilot Targets
- **Permian-focused operators**: **Civitas, Vital Energy, Diamondback, Continental, EOG**.
- **Drilling contractors + service cos**: **Patterson-UTI, Halliburton, SLB**.
- **Sand suppliers** wanting demand-foresight: **Smart Sand, Atlas Sand**.

### Risks & Mitigations
| Risk | Mitigation |
|---|---|
| Customers don't act on foresight | Make reports actionable; integrate with operator's well-scheduling workflow. |

---

## #10 — Subsurface Uncertainty Quantification (UQ)

### The Problem
Every E&P decision (where to drill, how to complete, how to value a prospect) hinges on subsurface properties (porosity, permeability, saturation, fracture density, fluid contacts) that are **inferred from sparse data and uncertain**. Today, reservoir engineers run a handful of deterministic scenarios + a Monte Carlo simulation with crude distributions. **Calibrated, quantified UQ — i.e., a credible interval on porosity that contains truth X% of the time** — is essentially unsolved in industry practice. The cost: billions in dry holes, mis-allocated capex, missed pay.

### O&G Why-now
1. **Bayesian deep learning + simulation-based inference** matured 2022–2025.
2. **Public geoscience datasets** are larger than ever (NLB Core, USGS, NLOG).
3. **Hardware (H100s)** makes per-property uncertainty inference tractable.
4. **EXPLORATION ECONOMICS** are tightening; majors increasingly demand probabilistic prospect volumes, not deterministic best-estimates.

### The Solution
**"UQ-Engine"** — calibrated subsurface UQ for:
1. **Petrophysical properties** (per-depth, per-zone): posterior distributions.
2. **Reservoir volumetrics** (OOIP, GIP): Monte Carlo posterior with calibrated coverage, sensitivity-attributed.
3. **Production forecasts**: posterior over EUR with credible interval per type-well; **analog-aware** (matches production to analogous reservoirs, weighted by similarity).
4. **Decision-theoretic overlay**: given posterior + cost model + risked value, automatically flag prospects whose P50 risked value has moved >X% in the latest update.

The engine sits between data (wells, seismic, core, production) and the asset team's decisions (drill-here, complete-this-way, exit-this-license).

### Build Plan

**Phase 0 (4 wks)** — Start with **production-forecast UQ using type-well analog matching**. The smallest workflow with the cleanest training data.

**Phase 1 (14 wks)**
- Build analog distance model over public + customer wells: features = log signatures, completion design, geology embeddings, geographic basin distance.
- Given a target type-well, surface top-N analogs → weighted EUR posterior with credible interval (Bayesian hierarchical model).
- Validate: out-of-sample backtest. Verify 80% P10–P90 interval contains truth ~80% of the time (calibration).

**Phase 2 (12 wks)** — Pilot with a mid-cap unconventional producer. Output: revised type-curve PDFs for every landing zone in their Permian portfolio. Comparison to in-house probabilistic type-curves.

**Phase 3** — Extend to (a) reservoir volumetrics (static model + UQ), (b) full prospect risk, (c) decision-economic overlay.

### Moat
- **Proprietary analog distance metric** built per-customer across hundreds of thousands of wells. Switching cost = learned embeddings.
- Calibrated coverage claims verified against public backtests.

### Pilot Targets
- **Mid-cap unconventional operators**: **Civitas, Continental, EOG, Diamondback, Devon, Range Resources**.
- **NOCs** (longer procurement but larger spend): **PDVSA, Petrobras, Saudi Aramco, ADNOC**.
- **A&D advisory firms needing probabilistic prospect volumes**: **RBC Richardson Barr, Pickering Energy Partners, Tenex Capital**.

### Risks & Mitigations
| Risk | Mitigation |
|---|---|
| Customers dismiss UQ as "too academic" | Frame as A&D decision support; show dollar impact (e.g., "your P10 dry-hole prob went 18%→24%"). |
| Calibration hard to prove | Build a public backtest; transparency is moat. |

---

## #11 — Hydrogen Sulfide (H2S) Exposure Prediction for Worker Safety

### The Problem
H2S is deadly at 100 ppm + (1 breath at 1000 ppm = death). Workers in sour-service operations (sour wells, refineries, gas plants, sour-water tanks) wear respirators + carry personal H2S monitors. **Exposure incidents** still occur (multiple US fatalities/year). Predicting transient H2S exposure — given weather, recent release history, work activity — is unsolved; current practice is fixed monitor placement + reactive alarms.

### O&G Why-now
1. **Cheap low-power H2S sensors** (electrochemical + laser-based) are now ubiquitous on sites.
2. **Personal monitors + wearable badges** log per-worker exposure per minute.
3. **OSHA / NIOSH / API updated H2S standards** (RP 2210, ANSI Z390.1) push toward quantified exposure modeling.
4. **Insurance + OSHA recordable injury costs** make avoidance ROI massive.

### The Solution
**"H2SGuard"** — an H2S exposure-prediction + mitigation system:
1. **Site-wide atmospheric model**: combines fixed + personal + vehicle-mounted sensors, weather, facility geometry, ongoing activities, recent release events.
2. **Real-time risk visualization**: heatmap of predicted H2S concentrations at ±25 ppm min, 15-min lookahead.
3. **Per-worker exposure tracking**: cumulative exposure per shift per worker, with predictive alerts ("you are forecasted to approach 10 ppm OSHA PEL limit in the next 90 min on this path; take this alternate route").
4. **Work-permit pre-check**: before authorizing hot work in a sour area, simulate plume dispersion under today's forecast; approve/reject with quantified risk.
5. **Reporting**: OSHA 300 log automation, RSS 2210A compliance, NIOSH HHE briefings.

### Build Plan

**Phase 0 (4 wks)** — Site type: a **sour-gas processing plant** or **sour oil tank battery**, 10–20 fixed sensors, ~50 workers, high turnover in activities.

**Phase 1 (14 wks)**
- Integrate fixed sensors (4–20 mA, Modbus), personal badges, weather feed (NOAA onsite).
- Atmospheric dispersion model (AERMOD or STADGAN-style GAN for refinery-specific micro-weather) coupled to sensor stream.
- Per-worker exposure modeling: track worker location (RFID/gate logs) + task + predicted exposure.
- Real-time heatmap + push alerts.

**Phase 2 (12 wks)** — Pilot at one sour-gas facility. Metric: reduce OSHA recordable H2S incidents to zero over 12 months; cut false-alarm rate by 70% (less nuisance deshelter).

**Phase 3** — Extend to refinery FCC units, sour-water knockout drums, drilling rigs drilling sour zones.

### Moat
- **Per-worker × per-shift × per-zone exposure history** is the regulatory-required document; once we generate it, switching cost is the audit trail.

### Pilot Targets
- **Sour-gas operators**: **Devon (OK/KS sour-gas), ExxonMobil, Apache, South Texas operators**.
- **Canadian sour-gas** (heavy regulation): **Tourmaline, Crescent Point, Cenovus**.
- **Refineries with FCC/sour units**: **Marathon, Phillips 66, Valero**.

### Risks & Mitigations
| Risk | Mitigation |
|---|---|
| Wrong exposure prediction harms trust | Conservative defaults; clear "this is a forecast, not a measurement"; false-negatives flagged hardest. |
| Worker privacy (location tracking) | Aggregate + privacy-by-default; opt-in per-worker badges. |

---

## #12 — Production Forecasting with Limited Data (analog reservoir matching)

### The Problem
When acquiring a new asset, evaluating an unproven play, or modeling an exploration prospect, production forecasting is often done with **sparse or no type-wells**. Today's approach: build a deterministic type-curve by analogy, often by hand-picking 3–5 "comparable" analog wells. This is unreliable: analogs are subjective, the chosen metric may not transfer across basins, and uncertainty is barely quantified. For new ventures, this is often the dominant uncertainty in valuation.

### O&G Why-now
1. **Large public completion/production databases** (FracFocus, state agencies, Enverus) — millions of wells available for analog.
2. **ML embeddings of geology + completion design** can encode what makes wells truly similar.
3. **Bayesian hierarchical models** naturally handle limited-data-with-large-analog-set.

### The Solution
**"AnalogMind"** — production forecasting for limited-data scenarios:
1. **Analog discovery**: given 5–50 wells in a prospect, surface top-N globally analogous wells (across basins) with similarity scores; not just same operator / same basin.
2. **Type-well construction**: hierarchical Bayesian model that produces a P10/P50/P90 EUR curve + 12/24/36-month cumulative production distributions per well, per landing zone, per lateral length.
3. **Forecast decomposition**: every forecast can be unpacked to its top-N analog contributions and similarity scores (explainability per problem-1's requirement).
4. **Scenario overlay**: completions-design sensitivity ("if you switch to a hybrid slickwater + 5-cluster perf design, here's the revised type-curve with uncertainty").

### Build Plan

**Phase 0 (4 wks)** — Wedge: unconventional horizontal type-curve forecasting. Permian Wolfcamp or Bakken as a prototype.

**Phase 1 (12 wks)**
- Training set: ~50,000 horizontal wells (Enverus public).
- Embedding: a well2vec model that learns geology + completion embeddings; tests show similar wells predict similar production.
- Hier Bayesian model produces per-target-zone posterior.

**Phase 2 (12 wks)** — Pilot with a PE-backed E&P evaluating a new acquisition; show before/after A&D value adjustment.

**Phase 3** — Extend to (a) conventional reservoirs (less data), (b) deepwater prospects, (c) carbon-storage analogs (per CCUS).

### Moat
- Customer's own well data plus preferred-analog sets are proprietary.

### Pilot Targets
- **PE-backed E&Ps / new ventures**: **Kayne Anderson, Kimmeridge, Aspect Energy**.
- **A&D advisory firms**: **Pickering, RBC Richardson Barr, Tudor Pickering**.
- **NOCs evaluating frontier acreage**: **Petronas, Petrobras, Saudi Aramco**.

### Risks & Mitigations
| Risk | Mitigation |
|---|---|
| Customer dismisses analog across basins | Show calibration per basin; allow per-customer analog preferences. |
| Forecasts differ wildly from in-house | Side-by-side comparison service; let customer tune the analog distance metric. |

---

## Cross-Cutting Notes (apply to all 12 problems)

### Shared technical spine (build once, consume across the portfolio)
- **O&G data connectors library** — LAS/DLIS/WITSML/OPC-UA/PI/MAXIMO/SAP/Enverus. Single most valuable asset.
- **O&G entity graph** — well ↔ facility ↔ equipment ↔ sensor tag ↔ work order.
- **Physics-grounded agent harness** — every answer about the physical world is grounded in citations + physics constraints; no hallucinated numbers.
- **Uncertainty framework** — Bayesian / conformal / quantile-regression wrappers used consistently across #1, #2, #5, #6, #10, #12 (all ask for *quantified confidence*, not point estimates).

### Regulatory / safety posture
- **Never let an LLM do math** — allocation, mass balance, royalty math → deterministic engines only.
- **Cite every physical-world assertion** — P&ID, sensor tag, log depth, monitoring station. No answer without provenance.
- **Cyber/OT hygiene** — read-only on control systems unless we're explicitly an actuation product; air-gap where required.

### Sequencing recommendation (12-month roadmap to a coherent sub-portfolio)

A portfolio of AI-native O&G products lines up well:

| Quarter | Ship | Rationale |
|---|---|---|
| **Q1** | #1 Predictive Maintenance (rotating equipment MVP) + #4 Automated log interpretation | Two fast-to-revenue products; clear customer pilots possible in 90 days. |
| **Q2** | #5 Hazardous event prediction (pipeline spill module); pair with #2 Methane LEED for ESG-side of same operators | Same data connectors, similar operators. |
| **Q3** | #3 Real-time drilling optimization (advisor); pilot on one contractor's rig | Largest unit-economics. |
| **Q4** | #6 Corrosion prediction; assemble cross-operator pipelines dataset | Builds the multi-tier visibility + sensor-fusion assets. |
| **Year 2** | #7 Energy price forecast, #9 Supply chain, #10 Subsurface UQ, #8 CCUS CO2 monitoring, #11 H2S, #12 Limited-data forecasting | Sequential completion of remaining 6 problems, each adding to the platform. |

End-state: a portfolio company whose data spine + physics-grounded agents + uncertainty quantification backs **all twelve operational AI problems** a single O&G operator faces — revenue streams compound, switching costs deepen, ESG/digital/operational all served from one platform.

---

## Final mapping: 12 O&G-native AI problems → products

| # | Problem | Product | Tier |
|---|---|---|---|
| 1 | Predictive Maintenance for Rotating Equipment | SpectraGuard | 1 |
| 2 | Methane Leak Detection & Quantification | MethaneIQ | 1 |
| 3 | Real-time Drilling Optimization | DrillOptix | 1 |
| 4 | Automated Well Log Interpretation & Lithology | LogLlama | 1 |
| 5 | Hazardous Event Prediction with Explainability | RiskLens | 1 |
| 6 | Corrosion Rate Prediction | CorroScope | 2 |
| 7 | Energy Price / Margin Forecasting | GeoEdge | 2 |
| 8 | CO2 Injection Monitoring (CCUS MRV) | CO2Seal | 2 |
| 9 | Supply Chain Disruption Prediction | WellStock | 2 |
| 10 | Subsurface Uncertainty Quantification | UQ-Engine | 3 |
| 11 | H2S Exposure Prediction | H2SGuard | 3 |
| 12 | Production Forecasting w/ Limited Data | AnalogMind | 3 |

Together with the Phase 1 (YC RFS → O&G) plan, this represents **23 distinct opportunity areas** — a coherent AI-native O&G portfolio.
