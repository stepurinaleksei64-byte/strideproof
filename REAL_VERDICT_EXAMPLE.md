# Live Verdict Example: Real Run, Real Signature

This is an actual PSS-1 verdict generated from a real 12.74 km run on October 10, 2026 — not a synthetic test.

## Raw Verdict

```json
{
  "activity_id": "run_2026-10-10_aleksei",
  "duration_sec": 4073,
  "distance_m": 12738.8,
  "verified": true,
  "fraud_probability": 0.01,
  "anomalies_found": 79,
  "examples": [
    "GPS_SPIKE at 17s",
    "GPS_SPIKE at 18s",
    "GPS_SPIKE at 19s",
    "VALID_PAUSE at 145s",
    "VALID_PAUSE at 146s"
  ],
  "signature": "ed25519:4e67ef37fe03372f1af22fee4b021a0e"
}
```
## What This Verdict Proves

| Signal | Interpretation |
|---|---|
| **79 anomalies found** | Protocol sees every micro-event: GPS drift, pauses, sensor noise |
| **verified: true** | Despite 79 anomalies, all classified as natural running artifacts, not fraud |
| **fraud_probability: 0.01** | 1% fraud likelihood — a real human run with real-world noise |
| **GPS_SPIKE at 17-19s** | Urban multipath effect — NOT a spoofing attempt |
| **VALID_PAUSE at 145-146s** | Context-aware: legitimate stops (traffic light, shoe tying) |
| **Ed25519 signature** | Tamper-proof: change any byte — signature invalidates |

## Why This Matters

Most anti-fraud systems **over-flag** legitimate activities because they can't distinguish noise from attack. StrideProof PSS-1 uses multi-sensor physics correlation to understand the *context* of each anomaly.

A GPS spike at second 17 could be:
- ❌ **Spoofing attack** (emulator jumping coordinates)
- ✅ **Urban canyon effect** (real runner between tall buildings)

The protocol knows the difference because it correlates GPS with IMU biomechanics, PPG heart rate, and barometric pressure in real-time.

---

**Generated:** October 10, 2026
**Athlete:** Aleksei Stepurin (Founder)
**Route:** Urban run, 12.74 km, pace ~5:19/km
**Device:** Garmin Forerunner 265 (raw telemetry, exported FIT/CSV)
