# Westline: geographic check of the findings

Data: GPS fixes from the 12 TyreSense exports (556,310 rows after de-duplication; 519,991 with a fix), the daily km log, the manager transcript. Reference towns use approximate coordinates from my own knowledge, not from the pack. Map: `Westline_GPS_Map.png`.

## What the geography confirms

| Claim | Check | Result |
|---|---|---|
| Truck-to-run assignments | Bounding boxes and stops per truck | T1 reaches Goa (lon 73.84), T2 reaches Karwar/Ankola, T3 reaches Gadag, T4 stays in Hubballi-Dharwad, T5 reaches Shivamogga, Davanagere and Mangaluru, T6 reaches Vijayapura via Bagalkot. All match the brief and transcript. |
| Depots | Overnight position (01:00-04:00) | T1, T3, T4 sleep at their depot every night. T6 sleeps at Belagavi 26 nights and Vijayapura 19. T5 sleeps at Hubballi 31, Shivamogga 13, Mangaluru 1. T2 sleeps at Belagavi 26 nights and Karwar-side 19. Matches the manager: Karwar, Vijayapura and Shivamogga trucks sleep outside. |
| Every depot fill/release in the notebook happened at the depot | GPS at each of 16 session times | All 16 were parked at the labelled depot (Belagavi or Hubballi), 0 km moved. The depot labels are right. |
| Goa truck three times a week | Days with ~278 km | 19 days: 7 Tue, 6 Thu, 6 Sat. Confirmed. |
| Mangaluru run | T5 GPS | One run on 5-6 Jul (night of 6 Jul at Mangaluru, 391 km log vs 378 km GPS). Confirmed. |
| Ghat heat | Where drive-axle temperature peaks on T1 | Peak at ~08:30 on all 19 Goa days, at lat 15.38, lon 74.17-74.27, i.e. west of Anmod village toward Goa (median 8.6 km from Anmod village). Heat appears on the descent. Confirmed. |
| Karwar stop | T2 on 11 Jul | Stopped 12:16-15:32 at 15.660 N, 74.804 E, on T2's normal Karwar-Belagavi route (the same position appears on 21 other days). It is 6.7 km from Kittur town. "Near Kittur" holds. |
| Odometer vs GPS | Per day, 270 truck-days | GPS is 94-99% of the log (lowest day 93.6%, T1) and zero on all 35 zero-km days. |

## What the geography corrects or adds

1. **The synchronised cooling events were mostly parked trucks, not trucks "on different roads".** At the 8 event times checked, on 5 dates (11 Jun, 13 Jun, 27 Jun, 7 Jul, 14 Jul) at least four of the five non-Goa trucks were parked at their depots in Belagavi and Hubballi (T1 was moving near Anmod or leaving Goa on 4 of those). On the other 3 dates (10 Jun, 17 Jun, 10 Jul) 2-4 trucks were moving. Parked trucks in two cities cooling in the same 10-minute bin is more consistent with ambient conditions (or a shared data component) than with driving. The notebook's wording should change; its conclusion (a common cause, not tyre faults) stands.
2. **Hubballi trucks had often already stood 3 hours or more at the fill.** From GPS: T4 stood 3.3-3.6 h before its fills, T5 3.1-3.7 h, T3 2.1-2.8 h. The spec defines cold as "stood at least three hours". So the tyres at 34-44 C at 15:30 were not hot from running (except T3); they were warm from afternoon ambient. The finding that Hubballi tyres sit 7-10 PSI under the card at night stands (it uses raw night readings). The mechanism needs rewording: fills made in afternoon heat land below the card at night temperature. **The recommended fix "cold fill: after 3 h parked" would not change T4 and T5 at all.** The fix has to be filling at the coolest time of day (early morning), or a temperature-corrected card.
3. **On 11 Jul the Karwar truck stopped twice before the final stop, both for ~70 min.** 05:15-06:24 at 14.78 N, 74.52 E (5RI 97 to 90 PSI, twin 113), and 09:16-10:22 at the Hubballi depot position (5RI 69 to 59 PSI, twin 117). Its normal stops there are ~20 min and ~40 min (T2 passes the Hubballi depot position on 39 of 45 days). The second stop was **at a Westline depot with the tyre ~50 PSI below its twin**. The manager's "stopped twice and looked" matches these two stops. This is a stronger missed-intervention point than the notebook makes, and it is a new question for the manager: who was at Hubballi that morning?
4. **GPS "jumps" are small.** Across missing-GPS gaps the largest displacement is 11.8 km (max implied speed 60 km/h), and only 4 jumps exceed 10 km. The notebook says consecutive fixes either side of a gap are "tens of km apart" and describes removing teleports over 60 km: there are none. The manager's "map jumps around" is real but of the order of 2-12 km, concentrated on T1 and T2.
5. **GPS gaps cluster on the coast and ghat.** T1 gaps: 07:00-09:00 and 16:00-17:00, last fix at Panaji, Anmod, Ramnagar. T2 gaps: 03:00-05:00 and 10:00-16:00, last fix at Karwar and Ankola. Both daytime and geographic. T1 and T2 have 121 and 122 gaps of 10 min or more; the other trucks 12-19.

## Limits of the GPS itself (affects what can be claimed)

- **Parked positions are identical to 5 decimals.** T3 has one unique position in 4,050 night fixes; every depot visit is the same coordinate. Real GPS jitters by metres. These look snapped or held (geofence or last-known fix), so parked GPS cannot confirm where on the depot site a truck stood.
- **Tracks are straight lines between a few waypoints** (see the map). Road geometry, the 22 km of Anmod bends, and road quality at Gadag or in Hubballi city cannot be assessed from this GPS.
- **The same coordinates repeat across days** (the Kittur stop position appears on 22 days including 11 Jul, one fix exactly matching). GPS vs odometer agreement is therefore very tight (T6 0.976-0.981 on every day). That shows consistency, not independent validation of either source. The notebook calls the odometer log independent ground truth; the GPS cannot test that.
- Altitude and speed are not in the export, so ghat gradient and braking cannot be tested geographically.

## Not tested

Weather, road condition, load by direction, and whether the fleet manager's "Gadag road is a disgrace" claim. Reference town coordinates are approximate.
