# Project 1: Household water leak detection

**Scenario.** A smart water meter on a house (`home_12`) reports flow every 5 minutes for 14 days. The utility wants to warn households about leaks early, because a slow leak can waste thousands of litres before anyone notices.

## Files
| File | Contents |
|---|---|
| `water_meter_readings.csv` | **Input.** `timestamp, meter_id, flow_lpm` (litres per minute) |
| `labels.csv` | Answer key: one label per row (keep hidden from trainees) |
| `events_ground_truth.csv` | Answer key: one row per event, with start and end |

## What's hidden in the data
| Event | When | Correct response |
|---|---|---|
| Garden irrigation (×7) | Every 2nd day, 05:00–05:30, ~12 L/min | **No alert** (scheduled, normal) |
| Stuck meter | Mar 3, 10:00–13:00, frozen at 3.42 | Maintenance alert |
| Burst pipe | Mar 5, 14:20–15:00, +30 L/min | Urgent alert |
| Missing data | Mar 7, 02:00–04:00 (rows absent) | Flag; don't fill blindly |
| **Slow leak** | From Mar 9 00:00 to the end, +0.8 L/min | Leak alert |

## The twist
A slow leak is **tiny** (0.8 L/min) next to normal showers and taps (5–20 L/min), so it's invisible on a normal plot. But a healthy house uses **zero** water at 2–4 am. The key signal is the **minimum night flow**: it never returns to zero once the leak starts. Meanwhile the 05:00 irrigation is a large night-time flow that is completely normal.

## Tasks
1. **EDA:** plot flow by hour of day; find the night window where usage should be zero.
2. **Clean:** detect the stuck meter (zero variation) and the gap.
3. **Detect:** build a "minimum night flow" feature per day and alert when it stays above zero for 2+ nights.
4. **Burst:** flag sudden, sustained flow far above the household's normal maximum.
5. **Bonus:** estimate how many litres the leak wasted before your alert fired.

## Suggested methods
Rolling minimum, daily aggregation, thresholds per time of day, CUSUM for small persistent shifts, Isolation Forest on daily features.

## Scale up with real data
- **BattLeDIM** (L-Town water network, SCADA flow and pressure sensors): a well-known leak-detection benchmark.
- **Lab leak benchmark** (Aghashahi et al., Data in Brief 2023): accelerometer, hydrophone and pressure sensors with labelled leak types.
