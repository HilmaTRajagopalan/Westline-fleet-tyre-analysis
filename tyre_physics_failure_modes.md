# Tyre pressure physics and failure modes in commercial fleets (hot-climate, Indian/Bengaluru context)

Research note: ~14 searches/fetches. Several primary documents (NHTSA FMVSS 138 RIA, Consumer Reports, NHTSA service bulletin PDF) could not be read directly (403 / paywall / unreadable PDF), so some figures come via search-result summaries and are flagged as secondary. No TMC document was retrieved directly; TMC figures are quoted via trade press.

## 1. How do under-inflation, over-inflation, slow leaks, fast punctures and valve/rim leaks present in pressure data?

### Takeaway
Signatures are distinguished by rate and by whether the loss survives temperature correction. Natural diffusion is only about 3-5% per month; a puncture or valve/rim leak is faster, and a nail-in-tyre leak can be slow then suddenly fast. Public sources give no standard per-hour thresholds, so these must be derived from the fleet's own data.

### Cited Findings
- Natural (no puncture) pressure decrease is as slow as about 3% to 5% per month; puncture detection by decrease rate is the standard approach, and manual re-inflation can be mistaken for a puncture by naive rate logic (from patent text) — [Patent summary, Justia 11179977](https://patents.justia.com/patent/11179977), [ERJ TPMS debate](https://www.european-rubber-journal.com/article/2081707/deflation-and-punctures-a-tpms-debate)
- Pressure lost by diffusion takes weeks or months to become dangerous. A nail left in place can keep a tyre inflated for an hour, a day, or a week; if it falls out the tyre can lose all pressure in minutes — [European Rubber Journal](https://www.european-rubber-journal.com/article/2081707/deflation-and-punctures-a-tpms-debate)
- Bridgestone patent logic: separate "natural drop due to outside temperature change" from slow puncture by temperature-compensating pressure to a reference (pressure, temperature) and monitoring the slope of the compensated value; flat slope = no leak, negative slope = slow or fast leak — [Google Patents US9162541](https://patents.google.com/patent/US9162541), [Patent search results](https://patents.google.com/patent/US20240142332A1/en)
- Indirect TPMS has limits: a patent on axle cross-comparison states prior techniques do not distinguish rapid from slow leak; the regulatory spec only requires a warning at 20% loss (ERJ) / FMVSS 138 defines severe underinflation as >=25% below recommended cold pressure — [USPTO 12115821](https://image-ppubs.uspto.gov/dirsearch-public/print/downloadPdf/12115821), [NHTSA FMVSS 138](https://www.nhtsa.gov/sites/nhtsa.gov/files/fmvss/TPMS-2005-FMVSS-No138.pdf)
- Michelin: a truck tyre 20% below recommended pressure "must be considered flat" and be removed and inspected; gauge tolerance of 2 psi is typical and gauges drift — [FleetOwner, Tires in real time](https://www.fleetowner.com/equipment/brakes-tires-wheels/article/21164410/tires-in-real-time-managing-psi-and-tread-depth)
- Consumer retailer rules of thumb: slow puncture roughly 1-5 psi per week (single retail blog, low quality) — [HiQ slow puncture guide](https://www.hiqonline.co.uk/advice/customer-support/tyres-fitting/slow-puncture-guide)
- FMCSA-related finding (secondhand via FleetOwner column, unverified against report): only ~15% of tyre-related roadside events involve sudden damage; the rest stem from slow leaks missed at inspection — [FleetOwner, Let's talk tires](https://www.fleetowner.com/industry-perspectives/ideaxchange/article/21133003/lets-talk-tires)
- Dragging brakes can raise tyre temperature measurably (P.S.I. testing) — a heat-without-pressure-loss signature — [FleetOwner](https://www.fleetowner.com/equipment/brakes-tires-wheels/article/21164410/tires-in-real-time-managing-psi-and-tread-depth)

### Inferences
(Physical reasoning, not from a single source; the report writer should label as analyst inference.)
- Under-inflation, steady state: compensated pressure sits persistently below placard cold pressure, flat or slowly falling; often a long-standing offset from last service.
- Over-inflation: persistently above placard after temperature correction (often from filling hot or over-correcting after seeing hot readings), or after a cold night when someone topped up from a high ambient reading.
- Slow leak: a compensated downward slope (order of psi per day to week) overlaid on a daily thermal sine wave; raw data may look like "saw-tooth" with lower peaks each day. Distinguish from diffusion (~3-5%/month, i.e. roughly 1-2 psi/month on a 35-40 psi tyre; this is arithmetic from the cited percentage).
- Fast puncture/blowout: step or ramp loss over minutes-hours, often with temperature spike beforehand if running flat; the dual-mate may then show a rise in temperature/pressure from carrying extra load.
- Valve/rim/bead leaks: intermediate rate, often repeatedly re-inflated (sawtooth with upward steps from top-ups); a leak that recurs on one wheel position after refills points to valve core, rim corrosion or bead seat, not tyre casing. No quantified published signature found.
- Missing sensor data/bursts of readings after long gaps must be treated separately from true leaks.

### Gaps
- No peer-reviewed or manufacturer-published time-series signatures with psi/hour thresholds for valve vs rim vs puncture leaks found.
- FMCSA 15%/slow-leak statistic not verified against original report.

## 2. Temperature, hot vs cold pressure, altitude, cold-correction

### Takeaway
Pressure follows the ideal gas law at roughly 1 psi per 10 F (about 1 psi per 5.5 C, about 2% per 10 F) for passenger tyres; truck tyres at 100+ psi change proportionally more in absolute terms. Hot pressure after driving is commonly several to ~20% above cold and must never be bled down. Cold-correct using absolute temperatures.

### Cited Findings
- NHTSA-hosted manufacturer service bulletin: a 10 F temperature change is equivalent to about 1 psi — [NHTSA TSB MC-10213757](https://static.nhtsa.gov/odi/tsbs/2022/MC-10213757-9999.pdf) (as reported by search summary; PDF text not machine-readable for me)
- Rule is consistent across consumer sources, about 2% per 10 F; 70 F to 20 F drop gives about 4.7 psi (ideal-gas check) — [Performance Plus Tire](https://www.performanceplustire.com/Blog/tire-pressure-cold-weather-2026), [Under Hood Service](https://www.underhoodservice.com/?p=146395), [YouCanic](https://www.youcanic.com/tire-pressure-temperature-calculator)
- Rule applies to ambient change, not to the cold-vs-driven-warm difference — [Performance Plus Tire](https://www.performanceplustire.com/Blog/tire-pressure-cold-weather-2026)
- ETRTO guideline cited by a UK tyre retailer: hot pressure up to 20% higher than cold; their small experiment showed +4.3% in shade, +14.3% in sun — [Bush Tyres](https://www.bushtyres.co.uk/?p=60605) (retailer, not primary)
- Michelin truck guidance: check tyres cold, at least 3 hours after stopping; never bleed air from hot tyres. Bridgestone: do not adjust pressure when hot because OEM recommendations are cold values — [Michelin Professional](https://business.michelin.com.au/help-advice/managing-tyre-pressures), [Michelin Middle East](https://b2b.middle-east.michelin.com/help-advice/managing-tyre-pressures)
- Pirelli: emergency hot adjustment only up to +0.3 bar above cold spec, recheck cold — [Pirelli](https://www.pirelli.com/global/en-ww/road/cars/tyres/safety-first-how-tyre-pressure-changes-with-heat-53412/)
- Altitude: gauge pressure falls about 0.5 psi per 1000 ft of altitude gain if filled at sea level (atmospheric pressure drops); Denver-to-LA example up to 2.5 psi; sources disagree on rate (some say 1 psi/1000 ft) — [Wikipedia-derived Cold inflation pressure](https://wiki.krisyotam.com/content/wikipedia_en_all_nopic_2026-03/Tire_pressure), [Horizons Unlimited thread](https://www.horizonsunlimited.com/hubb/tech/altitude-and-tire-pressure-85032-print)

### Inferences
- Cold-correction formula (ideal gas, absolute units): P_cold_abs = P_meas_abs x (T_ref_K / T_meas_K), with P_abs = P_gauge + P_atm (14.7 psi at sea level, about 13.1-13.2 psi at Bengaluru's ~920 m, my calculation from standard atmosphere). Using the simple rule: P_ref = P_meas - 0.1 psi per F (about 0.18 psi per C) x (T_meas - T_ref) scaled by pressure/35 for other pressure levels. The proportional form is more accurate for high-pressure truck/bus tyres (about 100-120 psi): about 0.2% per C of gauge-plus-atmosphere, i.e. ~2.3 psi per 10 C at 110 psi gauge.
- Important: the temperature measured by a TPMS sensor is gas/rim temperature, not ambient; while driving it is well above ambient (tens of C). Correcting live readings to a reference temperature (e.g. 20-25 C) using sensor temperature is the right way; ambient only suits parked/overnight readings.
- Bengaluru climate: daily swing perhaps 10-15 C and sun-parked vs shade differences mean 1-3 psi diurnal swings in passenger tyres (estimate; no citation) — this is why uncorrected raw alerts generate false positives.
- Altitude: a Bengaluru-filled tyre driven to the coast would read ~1.5 psi higher; to Ooty/Coorg, lower by roughly 0.5-1 psi. Minor but non-zero for intercity routes (arithmetic from the cited 0.5 psi/1000 ft rule).

### Gaps
- Could not read the NHTSA bulletin or find a TMC (RP 235 / RP 216) document stating the temperature rule or a recommended hot-pressure rise for trucks directly.
- No direct citation for typical driven-hot pressure rise for Indian summer conditions.

## 3. Natural loss rates; nitrogen vs air

### Takeaway
Typical natural loss is about 1-2 psi/month (3-5% per month) for air; nitrogen reduces the loss modestly. Consumer Reports' 12-month test: 3.5 psi (air) vs 2.2 psi (N2), i.e. ~1.3 psi difference per year, so nitrogen does not remove the need for monthly checks.

### Cited Findings
- Natural loss 3-5% per month — [Patent text via Justia](https://patents.justia.com/patent/11179977)
- Consumer Reports 12-month test, 31 all-season models, 30 psi start, outdoors: air averaged -3.5 psi, nitrogen -2.2 psi; recommends monthly checks — [Consumer Reports](https://www.consumerreports.org/cars/tires/should-you-use-nitrogen-in-car-tires-a6260003694/) (paywalled; figures via search summary)
- Clemson University study (reported secondhand) claimed nitrogen retained pressure 35-55% better than air — [Edmunds](https://www.edmunds.com/car-care/should-you-fill-your-cars-tires-with-nitrogen.html) (secondary; contradicts the small gap in CR in practical terms)
- Nitrogen claimed to reduce temperature-driven pressure change by ~30% — single consumer source, treat as unreliable — [Hakaru guide](https://hakaru.io/guides/tire-pressure-calculator-guide)
- A 2026 Onmanorama article discusses nitrogen advantages in India — [Onmanorama](https://www.onmanorama.com/news/kerala/2026/07/21/tire-pressure-fuel-efficiency.html) (not reviewed in depth)

### Inferences
- Annual CR loss implies ~0.3 psi/month (air ~0.29, N2 ~0.18) from a 30 psi start, notably lower than the 3-5%/month patent figure (~1-1.5 psi/month at 30 psi); the discrepancy is probably due to tyre age, rim/valve condition and temperature. Use ~0.3-1.5 psi/month as a plausible range for healthy passenger tyres (analyst inference). Truck tyres at high pressure lose more absolute psi.
- Because moisture-free nitrogen has no meaningful effect on the ideal-gas temperature coefficient, any TPMS temperature compensation should be the same for both.

### Gaps
- No authoritative Indian or truck-specific loss-rate data; no CR primary page readable.

## 4. Consequences of under-inflation (fuel, wear, blowouts, heat, casualties, cost)

### Takeaway
Under-inflation costs a few percent of fuel, 10-30%+ of tread life, and is the largest preventable driver of tyre-related roadside events and heat-driven failures. Fuel figures vary by basis (per psi vs per % of pressure), so quote carefully.

### Cited Findings
- NHTSA 2012 "Tire-Related Factors in the Pre-Crash Phase" (2005-2007 data, 5,470 sampled crashes): tyres under-inflated by 25% or more were about 3x as likely to be cited as critical pre-crash events; ~9% of vehicles had tyre problems pre-crash; tyres at 0-2/32" tread had problems ~26% vs ~8% for 3-4/32" — [European Rubber Journal](https://www.european-rubber-journal.com/2012/05/15/nhtsa-report-tyre-underinflation-low-tread-contribute-to-highway-accidents), [Fox News summary](https://www.foxnews.com/auto/study-finds-low-tire-pressure-a-factor-in-many-crashes) (secondary summaries; original NHTSA DOT HS 811 617 not read)
- TMC/ATA via FleetOwner: constant 20% under-inflation cuts tyre life 30%; 40% under-inflation cuts it 50% — [FleetOwner](https://www.fleetowner.com/equipment/brakes-tires-wheels/article/21164410/tires-in-real-time-managing-psi-and-tread-depth)
- Michelin test via fleet press: 20% under-inflated tyre wears ~20% faster (25,000 to 20,000 miles); other attribution: 10-33% tread-life loss — [Driving for Better Business](https://www.drivingforbetterbusiness.com/articles/michelin-warns-fleets-on-cost-of-driving-on-under-inflated-tyres/), [Overdrive](https://www.overdriveonline.com/overdrive-extra/article/15752530/will-excess-tire-pressure-improve-my-trucks-fuel-economy)
- EPA SmartWay: truck tyres 10 psi below recommended can reduce fuel economy 0.5-1% — [EPA 420F16035](https://19january2017snapshot.epa.gov/sites/production/files/2016-06/documents/420f16035.pdf) (via Overdrive/search summary)
- TMC/ATA reported: 10% under-inflation (about 10-11 psi on a 105 psi tyre) may lower fuel economy 1.5% — [Overdrive](https://www.overdriveonline.com/overdrive-extra/article/15752530/will-excess-tire-pressure-improve-my-trucks-fuel-economy); the common 1%-per-10%-under-inflation rule — [Trucking Info](https://www.truckinginfo.com/articles/keep-up-the-pressure) (note conflicting bases)
- ATMA/ITTAC India survey (>130,000 tyres checked): over half of passenger tyres under-inflated (53% per NewsBytes); loss of >42 crore litres petrol, >Rs 4,500 crore per year (~0.75% of national vehicle fuel); rule of thumb ~0.2% fuel efficiency loss per 1 psi — [Daily Pioneer](https://dailypioneer.com/news/low-tyre-pressure-costs-india-rs-4500-cr-in-fuel-every-year-atma), [Acko Drive](https://ackodrive.com/news/low-tyre-pressure-wastes-4-500-crore-worth-of-petrol-every-year-in-india-atma/), [NewsBytes](https://www.newsbytesapp.com/news/auto/atma-survey-finds-53-of-indian-passenger-vehicle-tires-under-inflated/tldr), [Tyre Trends](https://tyre-trends.com/auto-trends/low-tyre-pressure-costs-india-inr-45-bln-in-wasted-petrol-each-year-says-atma-survey) (petrol passenger vehicles only; news coverage, not the study)
- Europe: Bridgestone 2010 survey of 52,400 vehicles: 26.5% on seriously under-inflated tyres (>=0.5 bar low); ETRTO via EU Parliament question: under-inflation raises fuel consumption (CO2 +5-25%, wide) and cuts tyre life 20-50% — [What Car](https://www.whatcar.com/news/soft-tyres-waste-46bn-fuel/), [EU Parliament question E-5-2001-2074](https://www.europarl.europa.eu/doceo/document/E-5-2001-2074_EN.pdf) (industry/secondary)
- Heat: under-inflation causes excessive internal heat build-up leading to failure; TMC-type systems alert on temperature by axle group — [FleetOwner](https://www.fleetowner.com/equipment/brakes-tires-wheels/article/21164410/tires-in-real-time-managing-psi-and-tread-depth)
- Maintaining pressure allows casing reuse; Michelin warrants casings for up to 3 retreads (Len Dubois Trucking) — same FleetOwner source
- Fleet outcomes: FMCSA 12-month fleet field test associated TPMS/auto-inflation with fewer tyre road calls and about 1.4% fuel gain, but report called partly inconclusive; underinflation ~1 road call per tractor-trailer per year — [Trucking Info FMCSA field tests](https://www.truckinginfo.com/news/fmcsa-fleet-field-tests-show-promise-for-atis-tpms), [FMCSA Tire Condition Sensors report](https://webarchive.library.unt.edu/web/20130220154813/http://@fmcsa.dot.gov/facts-research/research-technology/report/Commercial-Vehicle-Tire-Condition-Sensors-StudyReport-and-Appendix-A.pdf); FleetNet: average fleet ~29,000 miles between tyre road calls vs >124,000 for best fleet (Q4 2018) — [Trucking Info](https://www.truckinginfo.com/346356/onboard-tire-management-systems-are-changing-the-game)
- India tyre-burst accidents (2014): 9,748 accidents, 3,371 killed, 9,081 injured — [Deccan Herald](https://www.deccanherald.com/amp/story/india%2Faccidents-due-tyre-bursts-serious-2152334) (news; newer MoRTH figures not found)
- India truck cost structure: fuel ~70% of operating cost budget, tyres ~10%; rolling resistance up to 30-33% of truck fuel cost — [India Transport Portal slides](https://fr.slideshare.net/ArnaudRenard3/india-rolling-resistance-and-fuel-saving-2016), [Wick Auto blog](https://wickauto.odoo.com/blog/wick-s-tyre-rakhshak-1/unveiling-the-hidden-costs-of-tyre-deflation-in-india-s-trucking-and-logistics-sector-1) (low-quality)

### Inferences
- Order-of-magnitude fleet cost: if a tyre set is 10% of operating cost and under-inflation of 20% cuts life by 20-30%, the annual cost of chronic 20% under-inflation is equivalent to ~2-3% of operating cost from tyres alone, plus 1-2% fuel (analyst arithmetic).
- Fuel-economy rules differ by about 2x depending on vehicle class and basis; for cars use ATMA 0.2%/psi, for trucks use EPA/TMC range with caveats.

### Gaps
- NHTSA FMVSS 138 RIA fuel/tread figures not retrieved (403). Original NHTSA 2012 report not read.
- No Indian truck/bus-specific under-inflation fuel study; no ARAI/MoRTH quantification of blowouts caused by under-inflation vs overloading; latest (2022-24) MoRTH tyre-burst counts not found.
- Pulse.bot article ("India's Energy Vulnerability Has a Tyre Problem") appeared but was not reviewed.

## 5. Load, specified pressure per axle, dual mismatch, rotation, alignment

### Takeaway
Pressure must match load and axle; mismatches between duals or across an axle cause irregular wear, casing fatigue and handling pull. Overload effectively behaves like under-inflation.

### Cited Findings
- Dual tyre pressure mismatch above 5 psi causes circumference difference, irregular wear and premature casing fatigue; a 5 psi difference between steer tyres pulls toward the lower side (Michelin) — [FleetOwner](https://www.fleetowner.com/equipment/brakes-tires-wheels/article/21164410/tires-in-real-time-managing-psi-and-tread-depth)
- Federal minimum tread depths: 4/32" steer, 2/32" drive/trailer; inspect on the lowest tread depth, not average — same source
- Indian Journal of Engineering & Materials Sciences (2004) studied inflation pressure and overload effects on rolling loss/fuel use for car and truck/bus (11R22.5) tyres, noting fleet operators control both — [Typeset PDF](https://typeset.io/pdf/effect-of-tyre-overload-and-inflation-pressure-on-rolling-4gqq7l40s5.pdf)
- Overloading cited as a main factor in Indian commercial-vehicle road accidents, with tyre/rim stress — [TRID overloading paper](https://trid.trb.org/View/182417), [SAE India truck accidents paper](https://saemobilus-test.cld.sae.org/papers/overview-truck-accidents-india-economic-loss-estimation-2021-26-0007); long trips, high speed, higher temperature and overload listed as burst factors — [Rushlane](https://www.rushlane.com/car-tyre-burst-prevention-12234202.html)
- Indian regulatory context: MoRTH moved to align tyre standards with global norms (rolling resistance, wet grip, noise labelling from October 2022) — [Policy Circle](https://www.policycircle.org/opinion/road-safety-and-tyre-quality/)

### Inferences
- Rotation/alignment: no quantified source retrieved; general principle only (misalignment gives shoulder/feathering wear; unequal duals cause scrub). Do not present figures.
- Pressure set per axle load (load-inflation tables, steer vs drive vs trailer; light vehicles use placard cold pressures, often a separate fully-laden value) means a single fleet-wide target pressure is wrong; a TPMS data model needs per-vehicle, per-axle reference cold pressure and a load state flag.
- An overloaded tyre at correct pressure deflects like an under-inflated one: heat rises with pressure reading normal, so temperature alerts add information that pressure alone misses.

### Gaps
- No Indian per-axle recommended pressure tables retrieved (consult Westline's own Tyre Inflation Spec Sheet in the candidate pack).
- No quantitative misalignment/rotation wear data found; ARAI/ATMA technical guides not located.
