# TPMS sensor and telemetry data-quality problems in fleet deployments

Method note: web search returned mostly vendor blogs, forums and retailer pages. Primary or regulatory sources were scarce, and two vendor pages (Powerfleet KB, Schrader) returned 403 on fetch, so only search snippets were used for them. Source quality is flagged per item. No TyreSense-specific documentation was found.

## 1. Sensor hardware issues (battery, drift, cap vs internal, stuck/spike, units, temperature offset)

### Takeaway
Battery depletion is the most commonly cited failure. It shows up as weak or intermittent signal, so expect dropouts and false low-pressure alerts. Cap (external) sensors read temperature much cooler than internal sensors but give similar pressure. I found no authoritative source on stuck readings, unit mix-ups or drift rates, so the analyst should treat those as data-driven checks.

### Cited Findings
- Sensor batteries are built in, have limited life, and typically last about 5 to 10 years. They are non-replaceable, so the whole sensor is replaced. Cold reduces battery capacity. — [Denso Auto Parts](https://www.densoautoparts.com/what-causes-tpms-sensors-to-fail) (search snippet; retailer, moderate quality)
- As battery voltage drops, the signal weakens and communication with the vehicle computer becomes intermittent. This can produce false low-pressure alerts. — [Brake & Front End](https://www.brakeandfrontend.com/identifying-tpms-battery-issues/) (snippet); a lower-quality guide makes the same false-alert claim at [liveworkdream](https://liveworkdream.com/2018/09/26/tpms-false-alarm/)
- Battery voltage can be read per sensor with a TPMS diagnostic tool. A low voltage reading confirms depletion. — [Brake & Front End](https://www.brakeandfrontend.com/identifying-tpms-battery-issues/)
- Cap sensors reportedly give spurious temperature spikes. Owners of TST cap sensors report readings of about 300F to 356F that trigger alarms at normal pressure. Anecdotal, forum. — [AirForums](https://www.airforums.com/threads/tst-tpms-false-warnings.1434843/post-16141011)
- Cap vs internal accuracy test (informal, 12 sensors, RV author): pressure readings showed no meaningful difference. External temperature ran about 20F to 30F cooler than internal because the cap reads heat conducted through the valve stem. The author says TPMS tyre temperature is not precise enough to find hot spots. — [RVTravel, final results](https://www.rvtravel.com/final-results-of-tpms-comparison-tests/) and [RVTravel, internal vs external temperature](https://www.rvtravel.com/internal-vs-external-tpms-temperature-test-results-for-warmer-weather/)
- Vendor claim: internal sensors last 5+ years versus about 1 to 1.5 years for external. Cap sensors can be moved between trailers. This is marketing, not independent. — [etrailer](https://www.etrailer.com/question-857697.html) (snippet)
- Typical aftermarket accuracy is about +/-0.1 bar (+/-1.5 psi). This is a single generic vendor guide and not a truck-specific spec. — [Lazada guide (A510)](https://auto.lazada.sg/a510-tpms-vs-popular-alternatives-accuracy-features-and-value-compared-sg/) (low quality)
- A fleet gateway vendor lists a fixed high-temperature alert at 90C. — [FMS-Tech](https://fms-tech.com/products/fms-tire-pressure-monitoring-system) (snippet)

### Inferences
- Cap-sensor temperatures should sit systematically below ambient-adjusted internal norms. A fleet-wide cooler offset is expected behaviour, not a fault. Compare a sensor with its own history and its peers, not with an absolute threshold.
- A sensor with a dying battery likely looks like clusters of missing rows or a lower reporting rate before it stops. This is an inference from the "intermittent signal" claim.
- Temperature values above physical plausibility (for example 150C or more) are likely sensor spikes. Pressure should be checked against temperature (roughly proportional) to separate real heating from artefacts.
- Unit mix-ups: the physical ranges differ by factor, roughly 1 bar = 14.5 psi = 100 kPa. A column whose values are about 7 to 9 (bar) or about 100 to 130 (psi) or about 700 to 900 (kPa) for the same tyre class is the tell. This is my own arithmetic and not a sourced finding.

### Gaps
- No authoritative source on drift or calibration-error rates for TPMS sensors, or on frozen/stuck-value signatures.
- No source on psi/kPa/bar mix-ups in exports specifically.
- No TyreSense documentation found.
- No quantitative cap-sensor failure statistics.

## 2. Wheel-position / sensor-ID mapping errors

### Takeaway
Direct TPMS sensors have unique IDs that the receiver maps to wheel positions. After rotation or replacement the mapping must be relearned, otherwise pressures display under the wrong wheel. Some vehicles auto-relearn, some need a tool.

### Cited Findings
- Each sensor has its own ID and the vehicle must re-associate the ID with the new position. Without a relearn after rotation, the dashboard shows pressures at the wrong positions. Methods include OBD relearn, auto-relearn by driving, and stationary relearn with a trigger tool. The required method depends on the manufacturer. — [Schrader TPMS relearn fundamentals](https://www.schradertpms.com/en/node/1477206) (search snippet only), [Autel](https://autel.us/what-does-it-mean-to-relearn-a-tpms-sensor-when-does-it-need-to-be-done-and-how-do-i-do-it/), [tiregrades](https://tiregrades.com/tire-maintenance/pressure/tpms/how-to-relearn-tpms-sensors)
- Some vehicles relearn automatically after rotation, per owners (forum anecdotes). — [Chevy Equinox forum](https://www.chevyequinoxforum.com/threads/tpms-relearn.6007/latest)
- Indirect systems do not need position relearning and do not report individual pressures. — [tiregrades](https://tiregrades.com/tire-maintenance/pressure/tpms/rotating-tires-with-tpms/)
- Fleet telematics generally suffers from duplicate asset identities (one truck with two IDs, orphaned device IDs left after sale). Suggested fix: match on VIN, plate, asset tag and keep a canonical asset record. — [OxMaint](https://oxmaint.com/industries/fleet-management/fleet-data-integration-avoiding-duplicate-vehicle-records), [Autosist](https://autosist.com/blog/why-fleet-tracking-data-shows-wrong-vehicle/) (vendor blogs)

### Inferences
- A position swap shows as a step change: after a date, the "front-left" series takes on the level and temperature behaviour of another position (for example steer axle pressures vs drive axle pressures). Axle-specific target pressures make this detectable.
- Trailer or cap sensors that are moved between assets can end up under the wrong vehicle. A sensor ID that appears on two vehicles on overlapping dates is a flag.
- Sensor ID reuse and the same sensor ID in two positions at the same timestamp indicate mapping or duplication errors.

### Gaps
- No quantified rate of mis-mapping in fleets.
- No source specific to heavy-truck relearn practice or to TyreSense pairing.

## 3. Telemetry / gateway issues (gaps, sleep, duplicates, timestamps, sentinels)

### Takeaway
Gaps when parked are normal (sensors sleep). Duplicates, out-of-order records and timezone/clock errors are well-documented telematics issues. Sentinel and zero-coded missing values were not found in any source and should be tested for empirically.

### Cited Findings
- Sleep behaviour is manufacturer-specific. Example: Porsche gen 1.x sensors transmit continuously, gen 3.x start on wheel rotation, and wheel electronics sleep after 5 minutes of standstill. — [NHTSA TSB MC-10198524](https://static.nhtsa.gov/odi/tsbs/2015/MC-10198524-9999.pdf) (primary, passenger car)
- A secondary source gives 60 to 90 s transmit intervals while driving and 5 to 10 minutes when stationary. Unverified. — [lambdageeks](https://lambdageeks.com/?p=248585)
- Owners report Ford sensors sleep after about 30 minutes of inactivity and need rotation to wake. Anecdotal. — [Ranger5G forum](https://www.ranger5g.com/forum/threads/tpm-system-question.25350/post-606523)
- Raw telematics streams contain duplicate pings, out-of-order packets and missing intervals. Recommended: enforce monotonic time order per vehicle and normalise to UTC before deduplicating. Clock drift or timezone mishandling causes false outliers. — [routematching.com](https://routematching-com.pages.dev/gps-data-preprocessing-cleaning-fundamentals/outlier-removal-in-raw-telematics-streams/) (developer article)
- A telematics device write-up reported 436 duplicates from lost acknowledgements, 1,728 out-of-order arrivals and 843 positions lost to full memory, and said retransmission behaviour drove false alerts. — [DEV Community](https://dev.to/crtdrops/the-right-position-at-the-wrong-time-gp0)
- Timezone errors do not change vehicle identity but put trips under the wrong shift or day. — [Autosist](https://autosist.com/blog/why-fleet-tracking-data-shows-wrong-vehicle/)
- A large gap between asset and system timestamps may indicate delayed uploads or device clock drift. — [Perspio meter violation report](https://help.perspio.io/help/meter-violation-report)

### Inferences
- A day-boundary artefact is likely if a dataset is UTC and the fleet works in IST (UTC+5:30): evening IST events fall on the next UTC date, and monthly export files may split trips across month files (a CSV per vehicle per month is a candidate for this). This is my reasoning, not sourced.
- Overnight gaps with no rows are expected from sleep and should not be counted as sensor faults. Gaps during known driving are the real defect signal.
- Zero pressure or zero temperature in a moving vehicle is physically impossible and likely a missing-value code.

### Gaps
- No source on sentinel values (0, -1, 255, 65535) in TPMS exports.
- No source on sampling-rate changes or batch export artefacts specific to TPMS.
- Heavy-truck sleep and transmit intervals were not found.

## 4. Alert / threshold problems

### Takeaway
False alarms come from sensor spikes, weak batteries, RF interference and genuine temperature effects. I found no study on alert fatigue or unacknowledged alerts in fleets.

### Cited Findings
- Pressure falls in cold and rises in heat, so some alerts reflect real physics rather than faults, particularly across large temperature swings. — [Patsnap Eureka](https://eureka.patsnap.com/article/preventing-false-tpms-alarms-in-extreme-temperature-swings) (snippet), [The News Wheel](https://thenewswheel.com/?p=133238)
- Weak batteries can produce false low-pressure alerts (see section 1). Spurious temperature spikes trigger alarms (see section 1).
- One forum report says a vendor blamed nearby RF activity for false warnings. Unverified single anecdote. — [AirForums](https://www.airforums.com/threads/tst-tpms-false-warnings.1434843/post-16141011)

### Inferences
- Many alerts from a few sensors, or alerts that repeat at the same level, suggest threshold chatter or faulty sensors rather than many real events. Check alert counts per sensor.
- Alerts that clear without a pressure change (inflation event) likely indicate a spike or signal fault, not a leak.

### Gaps
- No evidence on alert-fatigue rates, threshold-setting norms for fleets, or acknowledgement compliance.

## 5. Odometer / distance inconsistencies

### Takeaway
Fleet platforms treat ECM (CAN) vs GPS odometer mismatch, odometer spikes and stagnation, and negative deltas as standard anomalies. Sources of distance should be reconciled before use.

### Cited Findings
- Powerfleet's anomaly report lists ECM/GPS odometer mismatch, odometer spikes and odometer stagnation. Mileage and maintenance intervals are often computed from GPS when ECM data are missing. — [Powerfleet KB](https://kb.powerfleet.com/hc/en-us/articles/34684663564178) (search snippet; page returned 403)
- Perspio flags negative odometer deltas. — [Perspio](https://help.perspio.io/help/meter-violation-report)
- Telematics data cleanup checklists recommend reconciling duplicate and mismatched records. — [OxMaint checklist](https://oxmaint.com/industries/fleet-management/telematics-data-cleanup-checklist-kpis-and-cmms-workflow), [HVI](https://heavyvehicleinspection.com/blog/post/telematics-data-quality-for-maintenance-triggers) (vendor)

### Inferences
- Daily km from GPS typically differs a few percent from CAN odometer. Use the ratio per vehicle and flag outliers. A daily value that is zero while TPMS shows driving (or the reverse) indicates a source conflict.

### Gaps
- No quantified typical GPS vs CAN difference from an authoritative source.

## 6. Direct vs indirect (ABS-based) TPMS limits

### Takeaway
Indirect systems infer pressure from wheel-speed differences and cannot reliably detect equal loss on all tyres, which is why regulators were wary. They give no per-tyre pressure values, so any file with per-position kPa/psi is from a direct system.

### Cited Findings
- If all four tyres lose the same pressure, an indirect system may not trigger. — [Autopian](https://www.theautopian.com/how-indirect-tire-pressure-monitoring-systems-work-and-why-we-should-celebrate-their-return/) and [NHTSA 2001 proposal, Federal Register](https://www.govinfo.gov/content/pkg/FR-2001-07-26/pdf/01-18637.pdf)
- NHTSA's VRTC tested four ABS-based indirect systems. None met requirements, and none could detect equal under-inflation of all four tyres. — [NHTSA final rule summary](https://www.nhtsa.gov/sites/nhtsa.dot.gov/files/tpmsfinalrule_6.pdf)
- The 2005 final rule (FMVSS 138) requires warning when one or more tyres, up to all four, are 25 percent under placard pressure. — [Federal Register 2005](https://www.federalregister.gov/documents/2005/04/08/05-6741/federal-motor-vehicle-safety-standards-tire-pressure-monitoring-systems-controls-and-displays)
- Some newer indirect systems use spectral analysis to detect simultaneous loss. Vendor claim, not verified. — [Modern Tire Dealer](https://www.moderntiredealer.com/articles/17166-nira-dynamics-and-audi-are-getting-ready-to-introduce-an-indirect-tpms-that-meets-u-s-government-guidelines)

### Inferences
- FMVSS 138 covers light vehicles (under 4,536 kg GVWR). From my background knowledge, not verified in this session, it does not apply to heavy trucks, so fleet TPMS for trucks is aftermarket and unregulated for data quality.
- UN ECE R64 was not researched.

### Gaps
- UN ECE R64 text not reviewed. Heavy-vehicle regulation not verified. Sources are mostly 2001 to 2008 and indirect-system advances through 2026 were not covered.
