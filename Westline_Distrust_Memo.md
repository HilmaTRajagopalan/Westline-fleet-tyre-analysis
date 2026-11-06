# Distrust memo — what in the Westline pack should not be trusted

*One page. Evidence: `Westline_Tyre_Analysis.ipynb` §1, §10, §13.*

**Bottom line.** Pressure, temperature and odometer data are usable *once corrected*. What misleads: alerts built on raw PSI, two record errors that silently flip or drop data, and several of the manager's beliefs.

## Do not trust — and what I did

1. **Raw PSI against the spec card.** The spec is a *cold* pressure; sensor readings swing ~14 PSI on a typical day from temperature alone, so fixed-threshold or "pressure dropping" alarms mix air loss with heat. *Did:* referred every reading to 25 °C.
2. **The sensor temperature as gas temperature.** On parked (0-km) days pressure moves 0.71 PSI per reported °C, ~1.9× the gas law (490 tyre-days, r≈0.97); textbook compensation would under-correct by half. *Did:* calibrated empirically. The cause is a hypothesis.
3. **The Data Dictionary's side codes.** It says "C = co-driver side (right)" yet "CF1 = front left". Tyre serials show **C = left, D = right** (34/34, 34/34); following the dictionary flips every left/right tread comparison. *Did:* mapped by serial.
4. **T1's July file.** Every row is labelled `KA25B4471` (27,082 rows); a join on `Vehicle` drops T1's July. *Did:* keyed on file name.
5. **June/July exports** overlap by 12 h (2,869 duplicate rows, 100% identical). *Did:* de-duplicated.
6. **The tyre register as history.** It is a 15 Jul snapshot: the failed tyre (T2 5RI, …-0242) was overwritten by its replacement, so its brand and mount date are gone. T1's 3LO/3RO carry each other's serials after the 28 Jun swap: **a TPMS slot is a position, not a tyre.** *Did:* confirmed the swap through sensor batteries; tracked wear by serial.
7. **The tread sheets.** T1 has two readings, T5/T6 none. The failed tyre and its twin (15.9 / 12.0 mm) are recorded *identically at all three gauge points*, unlike the other 68 readings, and differ by 3.9 mm (all other twin pairs within ~1 mm). *Did:* used T1 only for rates; treated the mismatch as a question.
8. **The manager's recollections.** Rotation was on the **Goa** truck, not Karwar; fills are every 2–5 weeks, not 10–15 days; "Hubballi leaks more" is a hot-fill artefact (leak rates equal, p=0.24); "every afternoon" was 10 of 30 June days. *Did:* checked each against data.
9. **The brief's "no warning".** The failed tyre lost air from ~03:15 on 11 Jul; the truck stopped at 12:16: **8 h 56 min** of visible loss (but no slow precursor).
10. **GPS on T1/T2** (~15% missing by day), **`Avg Battery`** (mean of sensors that reported) and **row averages** (cadence ~18 s active, ~2 min parked). *Did:* odometer for distance; per-slot battery; fixed time bins.

## Cannot be verified from the pack
Vendor alert rules; weather (cooling events hit 4–6 trucks within the same 10-minute bin 100+ km apart, consistent with a regional front but unusually tight); load; speed/altitude; any absolute gauge offset (gauge and TPMS agree to ~1 PSI with each other); battery discharge curve; cause of the Karwar failure.

## Trust (checked)
The **odometer log** (chains perfectly; GPS within 2–6%); **TPMS pressure/temperature/battery relationships** after correction; **timestamps** (receiver vs GPS within 3 s).
