# Analytical pitfalls in fleet TPMS / tyre-pressure time-series analysis

Method note: 4 rounds of web search (summaries only; pages were not fetched in full). Much of the technical-method literature found is patents and trade press, not peer-reviewed. Items without a URL are labelled Inference and are reasoning, not sourced fact.

## 1. Confounders and temperature normalisation (ambient temp, time of day, load, driving vs parked, altitude); separating slow leak from thermal cycling and re-inflation

### Takeaway
Sealed-tyre gauge pressure moves with absolute temperature (about 1 psi per 10°F ambient for passenger tyres), so raw pressure must be converted to a cold-reference value before any leak-rate calculation. Driving heat (5 to 20 psi on linehaul trucks), altitude (about 0.45 to 0.5 psi per 1,000 ft) and load are separate effects. Natural permeation (about 0.5 to 1+ psi/month) is small next to daily thermal swings.

### Cited Findings
- Rule of thumb is about 1 psi per 10°F (about 2%) of ambient change; 32 psi at 70°F becomes about 28 psi at 30°F. A worked absolute-pressure calculation (70°F to 20°F) gave a 4.7 psi drop, matching the rule. — [Tires/TPMS temperature explainer](https://www.youcanic.com/tire-pressure-temperature-calculator); [Brake & Front End](https://www.brakeandfrontend.com/tpms-service-cold-tires/)
- The ambient rule does not apply to the cold-vs-warm-from-driving difference; a separate offset is needed for driven tyres. — [Brake & Front End](https://www.brakeandfrontend.com/tpms-service-cold-tires/)
- Truck tyres in linehaul service commonly rise about 5 to 20 psi over cold pressure from heat, depending on speed, load, ambient and road temperature. Hot readings should not be read as overinflation, and bleeding a hot tyre leaves it underinflated when cool. — [Truckinginfo](https://www.truckinginfo.com/articles/whats-this-cold-weather-doing-to-your-tire-pressure-updated); [Tire Review](https://www.tirereview.com/understanding-and-coaching-fleets-about-tires-and-inflation-pressure/)
- A vendor blog says tyres may read about 25% high at highway temperature. This is a weak source (trailer accessory vendor); the "10 to 15% buffer" there is a vendor recommendation, not a standard. — [Fanttik](https://fanttik.com/blogs/support/trailer-towing-tire-pressure-heat-dynamics)
- Required cold pressure depends on load per tyre position, taken from manufacturer load/inflation tables (example: 295/75R22.5 steer at 5,780 lb needs 100 psi cold at 70°F). — [Tire Review](https://www.tirereview.com/understanding-and-coaching-fleets-about-tires-and-inflation-pressure/)
- Altitude: gauge pressure rises about 0.45 to 0.5 psi per 1,000 ft because absolute pressure in the tyre is unchanged while ambient falls (forum-grade sources, sources disagree slightly). A Denver (5,300 ft) to Los Angeles (300 ft) descent could leave tyres about 2.5 psi under. — [Cold inflation pressure (Wikipedia)](https://en.wikipedia.org/wiki/Cold_inflation_pressure); [Tire Review altitude/temperature](https://www.tirereview.com/?p=221235)
- Natural permeation: a 1985 University of Michigan study of passenger tyres found 0.5 to 1.0 psi/month at 70°F, about 1.5 psi/month at 90°F; valve/bead leaks were usually minor. Consumer sources say 1 to 3 psi/month, so the figure depends on source. ASTM F1112 expresses loss as percent per month at a fixed temperature. — [TRID abstract](https://trid.trb.org/View/184503); [Tire Rack](https://www.tirerack.com/upgrade-garage/how-much-does-tire-air-pressure-change-over-time); [ASTM F1112 sample](https://cdn.standards.iteh.ai/samples/47216/dcec07e07ab045f88087434f44531d2c/ASTM-F1112-06.pdf)
- Published cloud-based leak detection removes daily temperature swings using ambient temperature, then looks at the loss rate rather than a fixed threshold. Another family separates a rapid-leak model (more than 20% in 24 h) from a slow-leak model (one day to several weeks, regression slope). Some use only yard-parked data to reduce dispersion. These are patents and conference abstracts, not validated results. — [US patent PDF 12583268](https://image-ppubs.uspto.gov/dirsearch-public/print/downloadPdf/12583268); [Tire Technology Expo abstract](https://tiretechnology-expo.com/ttx26c-tire-technology-expo-conference/intelligent-tire-pressure-event-detection-enhancing-safety-through-real-time-insights); [Tire Pressure Monitoring Using Cold Pressure Leak-Rate Analysis](https://eureka.patsnap.com/triz-case/cold-pressure-leak-rate)
- A Chinese automotive study uses cleaning, sliding-window filtering, interpolation and temperature compensation before labelling slow-leak segments. — [CAST journal](https://castjournals.cast.org.cn/joweb/qcgcs/EN/PDF/10.20104/j.cnki.1674-6546.20220073)
- Many TPMS sensors report pressure only, which limits compensation, and compensated pressure still has increased dispersion. — [US patent PDF 12583268](https://image-ppubs.uspto.gov/dirsearch-public/print/downloadPdf/12583268)
- NHTSA's dashboard warning triggers at 25% below the manufacturer's recommended pressure, so OEM warning flags are a coarse benchmark. — [Brake & Front End](https://www.brakeandfrontend.com/tpms-service-cold-tires/)
- Sensor behaviour: sensors sleep when stationary and wake on motion; driving transmission is roughly every 30 to 120 s; parked intervals range from 5 to 10 min to about 6 h or event-driven, depending on manufacturer. Sudden pressure loss triggers immediate transmission. The numbers come from patents and aftermarket explainers, not datasheets. — [Radial acceleration patent](https://image-ppubs.uspto.gov/dirsearch-public/print/downloadPdf/10414216); [TPMS RF theory](https://www.underhoodservice.com/tpms-radio-frequency-theory-and-operation/)

### Inferences
- Recommended normalisation: convert gauge to absolute (add local atmospheric pressure, about 14.7 psi at sea level, adjusted for site altitude), then P_cold = P_abs,meas x (T_ref / T_meas) minus atmospheric, with temperatures in Kelvin (or Rankine). Use the same reference temperature (for example 20°C or 70°F) across all vehicles. This is Gay-Lussac physics; I did not find a source naming it for TPMS.
- Use the tyre's own temperature channel if the export has one. Using ambient air temperature during or just after driving will under-correct. If the export has only pressure and ambient, restrict leak-rate estimates to parked, thermally settled samples (for example, readings 2+ h after the last drive, overnight minimum or pre-dawn), as the yard-parked patent approach does.
- Time-of-day and driving state are confounded with temperature; any pooled regression of pressure on date will pick up the diurnal and "driven vs parked" cycle. Tag each reading as parked/driving (via speed, motion flag, or daily km greater than 0) before aggregating.
- Constant volume is an approximation: tyres expand a little with pressure and load, and moisture/humidity adds minor effects. Residual error of about 0.5 to 1 psi after correction is plausible, comparable to monthly permeation, so slow-leak claims below roughly 1 psi/week need strong uncertainty language.
- Compare leak-rate estimates in percent of cold pressure per day (or per week) alongside psi/day, since permeation standards use percent per month and different tyres run different pressures.

### Gaps
- No authoritative source found for a heavy-vehicle (bus/truck) tyre-temperature-vs-ambient driving offset or for load-dependent hot-pressure rise; would need manufacturer data (Michelin, Goodyear, Bridgestone) or the client's spec sheet.
- No published false-alarm or detection-lead-time figures from real fleet deployments found.
- Exact stationary sensor reporting cadence is not confirmed from a manufacturer datasheet; check the client's Data Dictionary.

## 2. Detecting events: re-inflation, puncture, slow leak, sensor fault (physics rules before ML)

### Takeaway
Define events from cold-normalised pressure changes and timing: a step-up that exceeds thermal noise and occurs at a parked time is re-inflation; a fast negative step is puncture/rapid loss; a persistent small negative slope is slow leak; flat-line or implausible values are sensor faults. Published methods split rapid and slow regimes, and an ML approach appears only after temperature compensation.

### Cited Findings
- Rapid/slow split by timescale (more than 20% in 24 h vs days-to-weeks slope) is used in published fleet leak patents. — [US patent PDF 12583268](https://image-ppubs.uspto.gov/dirsearch-public/print/downloadPdf/12583268)
- A random-forest slow-leak model trained on yard-parked data exists in a patent family; and a Chinese study trains ML only after cleaning, filtering, interpolation, temperature compensation and labelling. — [EPO EP4140783](https://data.epo.org/publication-server/rest/v1.2/patents/EP4140783NWA1/document.html); [CAST journal](https://castjournals.cast.org.cn/joweb/qcgcs/EN/PDF/10.20104/j.cnki.1674-6546.20220073)
- Prior art that only compares compensated pressure to a threshold can trigger unneeded alerts; rate-of-change gives severity so vehicles need not be pulled needlessly. — [US patent PDF 12583268](https://image-ppubs.uspto.gov/dirsearch-public/print/downloadPdf/12583268)
- Rapid-leak detection patent example. — [US7432801B2](https://patents.google.com/patent/US7432801)

### Inferences
Suggested physics-first rules (my construction; thresholds must be tuned to the data):
- Re-inflation: cold-normalised pressure rises by more than a noise band (for example greater than 3 psi, well above 1 psi per 10°F-equivalent residual) within a short window while parked/speed zero. Other signs: temperature spike at fill (compressor air is warm), followed by decay to ambient within hours. A step-up of the same size in all tyres simultaneously at a morning start is more likely thermal or a calibration/reporting artefact than a service visit. Always check that a rise coincides with a drop in temperature-sensor reading before concluding "inflated".
- Puncture/rapid loss: negative step greater than X% (for example more than 20% of cold pressure, or more than 5 psi) within minutes to 24 h, not reversing after cooling, with no matching ambient fall. Tyre-temp fall or sensor dropout after pressure fall also characterises a burst/valve failure.
- Slow leak: robust (Theil-Sen or regression on parked-only, cold-normalised readings) slope between two re-inflation events, with a confidence interval; flag only if slope exceeds permeation baseline (about 0.5 to 1.5 psi/month, so roughly 0.02 to 0.05 psi/day) by a margin and R squared / residuals show a monotone trend. Fit each inter-inflation segment separately; a regression across a re-inflation step produces a wrong slope.
- Sensor fault: constant value for many hours across driving and temperature change (flat-line), physically impossible values (negative, greater than max rating), isolated spikes of one sample, one wheel diverging from its dual-axle partner while temperature channel stays normal, and cadence gaps (sleep mode explains gaps while parked; do not treat as missing events).
- Gaps in data are informative: stationary sleep mode and reporting-on-change mean "no data" often means "no change", and imputing or interpolating can invent leak slopes. Prefer last-observation carried forward only within defined staleness limits.

### Gaps
- No peer-reviewed benchmark with labelled fleet events and published precision/recall found; patent claims are unvalidated.
- Typical sensor noise floor for the client's sensor model not found; estimate it empirically from stationary flat periods.

## 3. Normalising by distance; reconciling sources; duplicates, missing months, partial exports

### Takeaway
Per-km metrics are valid only if the km source is joined on the same vehicle ID and date key and the km coverage matches the sensor coverage. I found no dedicated TPMS literature on this, so the guidance below is inference from general data-quality practice.

### Cited Findings
- Aggregations hide subgroup behaviour; different aggregation methods (weighted vs normalised) yield different results, so be explicit about weighting (see section 4). — [Statsig on Simpson's paradox](https://statsig.com/perspectives/simpsons-paradox-aggregates-mislead)
- I found no sourced material specific to reconciling telematics exports with spreadsheet odometer data, or interview transcripts with sensor data.

### Inferences
- Rate per km is a ratio: if the denominator (km) is small or zero for parked vehicles, per-km events (for example re-inflations per 1,000 km) explode or are undefined. Report numerator and denominator, and exclude or flag vehicle-periods below a minimum distance.
- Leaks are largely time-driven (permeation, parked leaks) rather than distance-driven; punctures and heat effects are distance-driven. Do not normalise a slow-leak psi/day figure by km; present both per-day and per-1,000-km views and say which mechanism each supports.
- Key hygiene steps: confirm vehicle identity mapping between sources (registration formats such as KA22AD7730 vs spaces/case), time zones and day boundaries (sensor UTC vs daily-km local day), and units (psi/bar/kPa; km vs miles). Check that the daily-km workbook covers the same date range as every export (for example Jun and Jul 2026), and that no vehicle has a month export but no km (or vice versa).
- Duplicates: de-duplicate on (vehicle, tyre position, timestamp); check overlaps between monthly files (boundary timestamps), and duplicates with different values (conflicting readings) rather than silently keeping first. Log how many rows were removed.
- Missing months/partial exports: compare row counts and last timestamp per file, check for truncated final rows, and per-vehicle date coverage. Treat a vehicle with fewer observation days as lower-weight, not as healthy.
- Transcripts and spec sheets are claims, not measurements: use them for hypotheses (which vehicle, which route, which threshold) and test them against the data; record where they disagree (for example manager says tyre X on vehicle Y was inflated on date Z but the data shows no step).
- When spec-sheet pressure applies to a different tyre size, axle load or position than the vehicle in question, the threshold is wrong for that vehicle (see section 4).

### Gaps
- No literature found on odometer/telematics reconciliation for TPMS specifically; no source on missing-data mechanisms for stationary-sleep sensors beyond the sensor-behaviour patents above.

## 4. Common errors: Simpson's paradox, survivorship bias, averages, spec thresholds, dual tyres, causation, outlier removal, small samples

### Takeaway
Most errors come from pooling heterogeneous vehicles/tyre positions, using one threshold for all, and over-claiming from few vehicles. Dual-tyre mismatch is an established, separately measured problem.

### Cited Findings
- Simpson's paradox: aggregate results can reverse within every subgroup; typical causes are confounding and unequal subgroup sizes, and it is advised to check subgroup-level trends before trusting aggregates. — [PMC7175433](https://pmc.ncbi.nlm.nih.gov/articles/PMC7175433); [Statsig](https://statsig.com/perspectives/simpsons-paradox-aggregates-mislead); [Inderscience](https://www.inderscience.com/info/inarticle.php?artid=9174)
- Dual tyres: a TMC Tire & Wheel study group figure reports 22% of inspected dual tyres mismatched by more than 5 psi, and only 44% of tyres within +/-5 psi of recommended. A three-company study found 79% of duals within 0 to 5 psi difference, 16% at 6 to 10, 5% at 11+. A 2025 vendor (Revvo) analysis of about 146,000 tyres reported about 21% of dual pairs with more than 5 psi difference. Primary TMC report not confirmed; vendor data. — [Automotive Fleet](https://www.automotive-fleet.com/340145/match-dual-tires-for-optimal-miles); [Truckinginfo](https://www.truckinginfo.com/articles/regular-tire-pressure-checks-are-worthwhile); [Revvo](https://www.revvo.ai/?p=22988)
- Industry dual-match tolerance is within 5 psi; a 5 psi gap can create up to 5/16 inch circumference difference (TMC RP 235A). — [Automotive Fleet](https://www.automotive-fleet.com/340145/match-dual-tires-for-optimal-miles)
- "Underinflated" is defined inconsistently: DOT counted 8 psi or more below recommendation; Michelin UK fleet study had 'dangerously' categories; NHTSA's warning is 25% below. Prevalence figures (32% light trucks, 46% of fleet tyres correct, 12% ATS) are not comparable and mostly from tyre/service vendors. — [FleetOwner](https://fleetowner.com/news/article/21681355/one-third-of-light-trucks-have-under-inflated-tires-dot-says); [Leasing Options](https://www.leasingoptions.co.uk/news/latest-news/only-46-of-fleet-drivers-tyres-driven-at-right-pressure/4442); [Tyrepress ATS](https://tyrepress.com/2022/11/12-of-fleet-vehicles-running-on-under-inflated-tyres-ats); [Tire Review: Defining Truck Tire Underinflation](https://www.tirereview.com/defining-truck-tire-underinflation/)
- Load/inflation tables are tyre-size, load-range and per-position specific, so a single fleet-wide target pressure may not match each position. — [Tire Review](https://www.tirereview.com/understanding-and-coaching-fleets-about-tires-and-inflation-pressure/)

### Inferences
- Simpson's paradox in this setting: fleet-wide pooled "average pressure vs temperature (or vs km)" can show the wrong sign if vehicles with different set-points or duty cycles are unevenly sampled across months. Use within-vehicle (fixed-effects/difference) comparisons, and stratify by vehicle and tyre position.
- Survivorship bias: vehicles that were retired, off-road, or whose sensors failed disappear from later months, and tyres that burst or were replaced vanish from the series. Report the denominator of vehicles/tyres per month and handle censoring explicitly. Also, tyres that were repaired after a leak look healthy afterward.
- Averages: mean pressure hides bimodality (two tyres under, two over), and a mean across hot and cold readings mixes states. Prefer distributions, medians and per-tyre percent time below threshold, always cold-normalised, with the state (parked/driving) shown.
- Spec thresholds: apply the threshold of the right tyre size, load range, axle position and load state; compare to the client's own spec sheet and flag mismatch. A threshold in psi at a given reference temperature must be compared with normalised pressure, not raw.
- Dual and axle differences: analyse inner/outer tyre pairs and axles separately; the inner dual is physically hidden and cooler/hotter differently; a persistent inner-outer gap may be a valve extension or sensor issue rather than a leak. Do not average duals.
- Correlation vs causation: pressure and fuel use/tyre wear/incident correlations are confounded by route, load, driver and age; claims of "low pressure caused X" need within-vehicle before/after evidence at minimum. Temperature correlation with pressure is physics, not evidence about maintenance.
- Outlier removal: extreme readings can be real (burst, hot tyre after braking, re-inflation overshoot). Remove only physically impossible values and sensor faults, log every exclusion, and report results with and without the removal.
- Small samples: with a handful of vehicles (the pack has 6 plates and 2 months) any rate has wide intervals; report n per vehicle and per tyre, use bootstrap or exact intervals, and avoid fleet-wide extrapolation from them.

### Gaps
- No primary source found for the TMC 22% figure or for heavy-vehicle-specific Simpson's/survivorship examples in TPMS data; the statistical points come from generic sources.

## 5. Reporting uncertainty and what the data cannot answer

### Takeaway
Report ranges, state assumptions (reference temperature, normalisation method, thresholds), and distinguish measured facts from hypotheses. The data cannot show true cold pressure without temperature data, cause of a leak, or events during sensor sleep or missing coverage.

### Cited Findings
- Compensated pressure still carries extra dispersion, and some systems have only pressure, so compensation uncertainty is unavoidable. — [US patent PDF 12583268](https://image-ppubs.uspto.gov/dirsearch-public/print/downloadPdf/12583268)
- Even accepted rules of thumb vary: 1 to 2 psi per 10°F, 0.45 to 0.5 psi per 1,000 ft, 0.5 to 3 psi/month permeation depending on source. Show sensitivity to these parameters. — [Truckinginfo](https://www.truckinginfo.com/articles/whats-this-cold-weather-doing-to-your-tire-pressure-updated); [Tire Rack](https://www.tirerack.com/upgrade-garage/how-much-does-tire-air-pressure-change-over-time); [TRID](https://trid.trb.org/View/184503)

### Inferences
- Give leak rates as slope plus 95% confidence interval (for example bootstrap by segment), the number of readings used, and the share of days with parked data. Show a sensitivity table: results under alternative reference temperature, alternative thermal coefficient (1 vs 2 psi per 10°F), and with/without excluded rows.
- Label every conclusion: Measured (from the exports), Derived (normalised), Reported (transcript/spec), Hypothesis. Keep spec and transcript statements apart from data findings.
- What the data cannot answer: root cause of a leak (valve, bead, puncture, rim) without inspection; whether a re-inflation was done by staff versus self-correction; actual load and route conditions unless provided; tyre-level health (tread, age); financial impact (fuel, wear) without cost data and a causal design; fleet-wide prevalence from a few vehicles over two months; events while sensors slept or exports were missing; whether the tyre was truly "cold" at each reading.
- Present recommended actions as ranked by confidence, and list data requests that would close gaps (tyre temperature channel, ambient source, service/inflation logs, load/GVW, tyre specs by position).

### Gaps
- No standard or guideline found on reporting uncertainty specifically for TPMS analytics; the above is general good practice.
