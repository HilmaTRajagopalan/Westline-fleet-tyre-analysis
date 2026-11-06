# Fleet TPMS / Tyre Knowledge Base

Distilled from `reports/Fleet TPMS tyre industry problems.md` and the notes in `research_notes/Fleet TPMS tyre industry problems/`.

**Evidence labels used throughout:** `[SRC]` = cited, but mostly secondary (trade press, vendor, patent, search summary); `[INF]` = analyst inference from physics or general practice, not sourced; `[UNVERIFIED]` = claim whose primary source was not confirmed.
**Caveat:** No TyreSense documentation was found. Most figures are not Indian-truck benchmarks. Never present them as Westline's benchmark.

---

## 1. Physics baseline (thermal swings dwarf leaks)

- Ambient rule: ~1 psi per 10 °F (~2%) for passenger tyres; sources range 1–2 psi/10 °F. Applies to ambient change, not driving heat. [SRC: R1, R2]
- Driven truck tyres run ~5–20 psi above cold; never bleed a hot tyre. Michelin: check cold, ≥3 h after stopping. ETRTO guidance (via UK retailer): hot up to +20% over cold. [SRC: R3, R4, R5]
- Natural permeation: 3–5%/month (patent); 0.5–1.0 psi/month at 70 °F, ~1.5 at 90 °F (1985 U. Michigan). Consumer Reports 12-month test: air −3.5 psi vs nitrogen −2.2 psi. Nitrogen does not remove need for checks. [SRC: R6, R7, R8]
- Altitude: ~0.45–0.5 psi per 1,000 ft (sources disagree). Bengaluru ~920 m. [SRC]
- Cold-correction method [INF]: gauge → absolute (add ~13.1–13.2 psi at Bengaluru), scale by absolute temperature to a common reference, prefer the sensor's own temperature channel; ambient only for parked, thermally settled samples. Residual error ~0.5–1 psi is plausible, comparable to a month of permeation, so slopes below ~1 psi/week need strong uncertainty language. Run sensitivity on the 1-vs-2 psi/10 °F coefficient and reference temperature.

### Failure signatures [INF — tune on Westline data]
| Mechanism | Signature in temperature-compensated data |
|---|---|
| Chronic under-inflation | Persistently below cold spec, flat or slowly falling |
| Over-inflation | Persistently above spec after correction (often filled hot) |
| Slow leak | Negative slope between top-ups; sawtooth with falling peaks |
| Valve/rim/bead leak | Recurring top-ups on one wheel position |
| Puncture/blowout | Step or ramp loss over minutes–hours, no recovery on cooling, possible dropout |
| Overload / dragging brake | Temperature rises while pressure looks normal |

- A nail can hold a tyre for an hour, a day or a week, then let go in minutes. [SRC: R9]
- Patent methods: rapid loss >20% in 24 h vs slow loss via regression slope of temperature-compensated pressure. Unvalidated. No psi/hour thresholds are published for valve/rim/bead leaks. [SRC: R10, R11]

## 2. Consequences of under-inflation (numbers are soft)

- Tyre life: TMC/ATA via FleetOwner — constant 20% under → −30% life; 40% under → −50%. Michelin test: ~20% faster wear. [SRC: R12, R13]
- Fuel: EPA SmartWay 10 psi under → 0.5–1% truck fuel economy; ATMA uses ~0.2%/psi for petrol cars. Bases differ. [SRC: R14, R15]
- Safety: NHTSA 2012 — tyres ≥25% under ~3× as likely to be cited as critical pre-crash events (secondary summary). India tyre-burst accidents 2014: 9,748 accidents, 3,371 deaths; no newer MoRTH data found. [SRC: R16, R17]
- Tyre cost share of Indian fleet opex: sources conflict (7%, ~10%, 20–30%); use a 10–30% planning range, not a point value. [SRC: R18–R20]
- Fuel-only business cases overstate; tyre life and downtime are the larger levers.
- India prevalence (passenger vehicles, NOT trucks): ATMA/ITTAC survey of >130,000 tyres — 53% ≥10% under, 32% >20% under, ~INR 4,500 crore petrol wasted/year. Older Apollo car survey: 22% at spec, ~44% over, ~34% under — "wrong pressure" runs both ways. [SRC: R15, R21, R22]
- Duals: pressure mismatch >5 psi → irregular wear/casing fatigue (Michelin); 5 psi gap on steer axle → pull. TMC-attributed 22% of duals mismatched >5 psi [UNVERIFIED primary]. A 20%-low truck tyre should be treated as flat. [SRC: R23]
- Overloading: IFTRT estimate 70% of national-permit trucks overload [UNVERIFIED]; behaves like under-inflation (heat up, pressure looks normal). [SRC: R24]
- No quantitative data found on potholes, monsoon, rotation or alignment: treat as qualitative.

## 3. Sensor and telemetry faults

- Battery: sealed, ~5–10 years; dying battery → weak/intermittent signal, false low-pressure alerts. [SRC: R25, R26]
- Cap (external) sensors read temperature ~20–30 °F cooler than internal (informal 12-sensor test); forum reports of spurious 300 °F+ spikes. Anecdotal. Judge a sensor against its own history and peers, not absolute thresholds [INF]. [SRC: R27, R28]
- No authoritative source on drift rates, stuck values, unit mix-ups or sentinel codes (0, −1, 255, 65535). Test empirically [INF]: zero or sub-ambient pressure on a moving vehicle is impossible; clusters near 7–9 vs 100–130 vs 700–900 suggest bar vs psi vs kPa.
- Position mapping: direct sensor IDs must be relearned to wheel position after rotation/replacement, otherwise pressures appear under the wrong wheel. Red flags [INF]: a position adopting another axle's level/temperature behaviour after a date; the same sensor ID on two vehicles on overlapping dates. Duplicate/orphaned asset IDs are documented in telematics. [SRC: R29, R30]
- Sleep/gaps: sensors sleep when stationary (e.g. 5 min after standstill in one Porsche NHTSA TSB); cadence ~30–120 s driving, 5–10 min to hours parked (patents/explainers, not datasheets). Overnight silence is normal; gaps during known driving are faults. Interpolating across sleep can invent leak slopes [INF]. [SRC: R31, R32]
- Raw streams carry duplicates and out-of-order packets; normalise to UTC before de-duplicating. A UTC export for an IST (UTC+5:30) fleet pushes evening events to the next date and can split trips across the monthly per-vehicle files [INF] — check the Data Dictionary. [SRC: R33]
- Odometer: reconcile ECM vs GPS vs manual (mismatch, spikes, stagnation, negative deltas). No typical GPS-vs-CAN gap found. [SRC: R34]
- Regulation: FMVSS 138 requires a warning at 25% below placard (light vehicles). Heavy-truck coverage is background knowledge [UNVERIFIED]. Per-position pressures imply a direct system; indirect (ABS) systems cannot report per-tyre values and cannot reliably detect equal loss on all tyres (NHTSA tests). [SRC: R35]

## 4. Fleet operations and process

- Manual checks: hot readings overstate cold by up to ~15%; stick gauges drift/vary; car gauges unreliable on commercial tyres >100 psi. [SRC: R36, R37]
- US EPA (via summary): only 8% of drivers gauge before each trip; under half of combination-truck tyres within 5% of target. No Indian truck equivalent found. [SRC: R14]
- Top-ups clear alerts for a day without fixing the leak (vendor claim). [SRC: R38]
- Required cold pressure depends on size, load range, axle position and load. A single fleet target or the wrong spec-sheet row makes correct tyres look faulty. [SRC: R39]
- Indian truck tyres: >70% retreaded at least once (ICRA); retreads 30–50% cheaper; records mostly manual → unlogged swaps, mixed sizes and sensors left on wrong vehicle corrupt later analysis [INF]. [SRC: R40, R41]
- TPMS value is conditional on alerts becoming work orders: US DOT field test ~1.4–1.8% fuel gain, <1 year ROI for one fleet (partly inconclusive per secondary reports). Vendor 4–6 month paybacks are modelled. Alert-fatigue figures are unsourced vendor assertion. AIS-151 mandate for heavier vehicles is claimed by market-research sites only [UNVERIFIED]. No Indian TPMS case study with measured outcomes. [SRC: R42–R45]

## 5. Analytical pitfalls (mostly generic stats applied to this setting)

- Simpson's paradox: pooling vehicles with different set-points/duty cycles can reverse trends — stratify by vehicle and position; compare within vehicle. [SRC: R46]
- Survivorship bias: replaced tyres, bursts and failed sensors vanish from later months; a repaired leak looks healthy; report denominator per month [INF].
- Averages mix hot and cold states; prefer medians and %-time-below-threshold on cold-normalised data. Never average inner and outer duals.
- Outliers can be real (blowouts, braking heat, overshoot). Remove only physically impossible values, log every exclusion, show with/without.
- "Underinflated" is defined inconsistently (8 psi below spec to 25% below) so prevalence figures are not comparable. [SRC: R47]
- Distance normalisation: leaks are time-driven, punctures/heat distance-driven — present psi/day and per-1,000 km separately; flag vehicle-periods with near-zero km; confirm the daily-km workbook covers the same plates (e.g. `KA22AD7730` vs spaced forms), dates and time zones; deduplicate on vehicle + position + timestamp; check month-boundary overlaps; fewer observed days = lower weight, not healthier [INF].
- Fit slopes separately between re-inflation events; give 95% intervals.
- Six vehicles × two months → wide intervals; no fleet-wide prevalence claims.

### Testing client narratives [INF]
- "Sensors are faulty": check whether faults are wheel/vehicle-specific, step recovery after top-ups, time-of-day correlation, flat-lining/missingness, or a mis-set target.
- "We check daily": compare with recovery steps. "Drivers ignore alerts": pressure recovery after alerts, separating no-response from no air access. "Spec is right": compare with fitted size, load index, axle load. A vehicle steadily below target with no recovery suggests a wrong target before a leak.
- Transcripts and spec sheets are claims to test, not measurements.

### What the data cannot show
Leak root cause without inspection; who re-inflated; load/route conditions unless supplied; tread or age; financial impact without cost data and a causal design; fleet-wide prevalence from six vehicles; events during sensor sleep or missing exports; whether a tyre was truly cold at each reading.
Data that would close gaps: tyre temperature channel, ambient source, service and inflation logs, load/GVW, fitted tyre specs by position.

## 6. Suspect ranking before calling a "leak"
1. Wrong spec target → 2. Hot-vs-cold artefact → 3. Top-up masking → 4. Wheel-position or timestamp error → 5. Sensor fault → 6. True leak. Interesting findings: steps, recurring top-ups at one position, heat without pressure loss. ~1 psi/month loss is normal.

---

## Refs

| ID | Source | URL |
|---|---|---|
| R1 | YouCanic tyre-temperature calculator | https://www.youcanic.com/tire-pressure-temperature-calculator |
| R2 | Brake & Front End — TPMS service, cold tires | https://www.brakeandfrontend.com/tpms-service-cold-tires/ |
| R3 | Truckinginfo — cold weather & tire pressure | https://www.truckinginfo.com/articles/whats-this-cold-weather-doing-to-your-tire-pressure-updated |
| R4 | Michelin Professional — managing tyre pressures | https://business.michelin.com.au/help-advice/managing-tyre-pressures |
| R5 | Bush Tyres (ETRTO guidance) | https://www.bushtyres.co.uk/?p=60605 |
| R6 | Patent US 11179977 (Justia) | https://patents.justia.com/patent/11179977 |
| R7 | TRID — 1985 U. Michigan permeation study | https://trid.trb.org/View/184503 |
| R8 | Consumer Reports — nitrogen (paywalled) | https://www.consumerreports.org/cars/tires/should-you-use-nitrogen-in-car-tires-a6260003694/ |
| R9 | European Rubber Journal — deflation & punctures | https://www.european-rubber-journal.com/article/2081707/deflation-and-punctures-a-tpms-debate |
| R10 | Bridgestone patent US 9162541 | https://patents.google.com/patent/US9162541 |
| R11 | Patent US 12583268 | https://image-ppubs.uspto.gov/dirsearch-public/print/downloadPdf/12583268 |
| R12 | FleetOwner — Tires in real time (TMC/ATA) | https://www.fleetowner.com/equipment/brakes-tires-wheels/article/21164410/tires-in-real-time-managing-psi-and-tread-depth |
| R13 | Driving for Better Business — Michelin warning | https://www.drivingforbetterbusiness.com/articles/michelin-warns-fleets-on-cost-of-driving-on-under-inflated-tyres/ |
| R14 | EPA SmartWay (2016 snapshot) | https://19january2017snapshot.epa.gov/sites/production/files/2016-06/documents/420f16035.pdf |
| R15 | Daily Pioneer — ATMA INR 4,500 cr | https://dailypioneer.com/news/low-tyre-pressure-costs-india-rs-4500-cr-in-fuel-every-year-atma |
| R16 | ERJ — NHTSA 2012 report | https://www.european-rubber-journal.com/2012/05/15/nhtsa-report-tyre-underinflation-low-tread-contribute-to-highway-accidents |
| R17 | Deccan Herald — tyre-burst accidents | https://www.deccanherald.com/amp/story/india%2Faccidents-due-tyre-bursts-serious-2152334 |
| R18 | FleetX — tyre management | https://www.fleetx.io/tyre-management |
| R19 | Pulse/ETAuto — India's tyre problem | https://www.pulse.bot/transportation/news/indias-energy-vulnerability-has-a-tyre-problem-f0c969bf-ad3c-49e1-a6e6-a3fd78253e64/ |
| R20 | FleetX blog — tyre inventory | https://blog.fleetx.io/how-to-manage-tyre-operations-inventory-effectively |
| R21 | Tyre Trends — ATMA survey | https://tyre-trends.com/auto-trends/low-tyre-pressure-costs-india-inr-45-bln-in-wasted-petrol-each-year-says-atma-survey |
| R22 | Tyrepress — Apollo survey (2010) | https://tyrepress.com/2010/01/apollo-registers-tyre-pressure-safety-concerns-on-indian-market |
| R23 | Automotive Fleet — match dual tires | https://www.automotive-fleet.com/340145/match-dual-tires-for-optimal-miles |
| R24 | Outlook Business — Death on wheels | https://www.outlookbusiness.com/trend/death-on-wheels-2197 |
| R25 | Denso — why TPMS sensors fail | https://www.densoautoparts.com/what-causes-tpms-sensors-to-fail |
| R26 | Brake & Front End — TPMS battery issues | https://www.brakeandfrontend.com/identifying-tpms-battery-issues/ |
| R27 | RVTravel — TPMS comparison tests | https://www.rvtravel.com/final-results-of-tpms-comparison-tests/ |
| R28 | AirForums — TST false warnings | https://www.airforums.com/threads/tst-tpms-false-warnings.1434843/post-16141011 |
| R29 | Autel — TPMS relearn | https://autel.us/what-does-it-mean-to-relearn-a-tpms-sensor-when-does-it-need-to-be-done-and-how-do-i-do-it/ |
| R30 | Autosist — fleet data wrong vehicle | https://autosist.com/blog/why-fleet-tracking-data-shows-wrong-vehicle/ |
| R31 | NHTSA TSB (Porsche sleep behaviour) | https://static.nhtsa.gov/odi/tsbs/2015/MC-10198524-9999.pdf |
| R32 | Underhood Service — TPMS RF theory | https://www.underhoodservice.com/tpms-radio-frequency-theory-and-operation/ |
| R33 | routematching — telematics outlier removal | https://routematching-com.pages.dev/gps-data-preprocessing-cleaning-fundamentals/outlier-removal-in-raw-telematics-streams/ |
| R34 | Perspio — meter violation report | https://help.perspio.io/help/meter-violation-report |
| R35 | Federal Register — FMVSS 138 | https://www.federalregister.gov/documents/2005/04/08/05-6741/federal-motor-vehicle-safety-standards-tire-pressure-monitoring-systems-controls-and-displays |
| R36 | Fleet Equipment — hot vs cold readings | https://www.fleetequipmentmag.com/?p=141461 |
| R37 | Gulf Oil — check truck tyre pressure | https://master.gulfoilltd.com/blog/maintenance-advice/how-to-check-truck-tyre-pressure |
| R38 | Oxmaint — TPMS alert to work order | https://oxmaint.com/industries/fleet-management/tpms-alert-to-fleet-work-order-automated-tire-response |
| R39 | Tire Review — coaching fleets on inflation | https://www.tirereview.com/understanding-and-coaching-fleets-about-tires-and-inflation-pressure/ |
| R40 | Tyre Trends/ICRA — 70% retreaded | https://www.tyre-trends.com/spotlight/70-percent-truck-tyres-in-india-are-retreaded-once-icra |
| R41 | FleetX — tyre management module | https://blog.fleetx.io/top-5-benefits-of-utilizing-the-fleetx-tyre-management-module/ |
| R42 | HDT/Truckinginfo — DOT study ROI | https://www.truckinginfo.com/news/dot-study-proves-roi-on-tire-inflation-and-pressure-monitoring-systems |
| R43 | Oxmaint — TPMS fuel efficiency guide 2026 | https://oxmaint.com/industries/fleet-management/tire-pressure-monitoring-tpms-fuel-efficiency-guide-2026 |
| R44 | Motive — cost of delayed action | https://gomotive.com/blog/cost-of-delayed-action-fleet-management/ |
| R45 | Mordor Intelligence — India aftermarket TPMS | https://www.mordorintelligence.com/industry-reports/india-aftermarket-tpms-market |
| R46 | PMC7175433 — Simpson's paradox | https://pmc.ncbi.nlm.nih.gov/articles/PMC7175433 |
| R47 | Tire Review — defining truck underinflation | https://www.tirereview.com/defining-truck-tire-underinflation/ |
| R48 | FleetOwner — Let's talk tires (FMCSA claim, unverified) | https://www.fleetowner.com/industry-perspectives/ideaxchange/article/21133003/lets-talk-tires |

Local sources: `reports/Fleet TPMS tyre industry problems.md`; `research_notes/Fleet TPMS tyre industry problems/*.md`.
