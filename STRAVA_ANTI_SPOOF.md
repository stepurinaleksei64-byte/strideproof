# Strava Anti-Spoofing Analysis: How PSS-1 Eliminates Fake KOMs

Current server-side heuristics (speed/distance thresholds) fail against sophisticated spoofing. StrideProof shifts validation to the edge using multi-sensor physics correlation.

## 🚨 Vulnerability 1: GPS Emulators & "Shaking" Apps
* **The Attack:** Software generates perfect GPS coordinates along a segment. Speed and distance are mathematically flawless.
* **The PSS-1 Defense:** **IMU-GPS Cross-Validation.** A real human running at 4:00/km generates a specific vertical oscillation (8-10cm) and ground contact time (~240ms) at a cadence of ~175spm. An emulator has GPS, but zero IMU physics. PSS-1 flags `human_verified: false` instantly.

## 🚨 Vulnerability 2: Vehicle-Assisted Runs (E-Bikes / Cars)
* **The Attack:** User drives a car or e-bike but logs it as a "Run" to farm Corporate Challenge rewards.
* **The PSS-1 Defense:** **Biomechanical Signature.** Vehicles lack the high-frequency shockwave of a footstrike. The edge node's 1000Hz accelerometer detects the absence of the "running gait" frequency spectrum (2.5 - 3.5 Hz). 

## 🚨 Vulnerability 3: Device Farms (Vitality/Corporate Fraud)
* **The Attack:** 50 smartwatches strapped to a shaking table to farm insurance/brand rewards.
* **The PSS-1 Defense:** **Proof-of-Humanity (PoH) + Ed25519.** 
  1. Optical PPG (heart rate) on a shaking table lacks the respiratory sinus arrhythmia (HRV) signature of a live human cardiovascular system.
  2. The payload is signed by a unique hardware-bound private key. Cloning the signal requires physical access to the Secure Enclave of each node.

## 📊 API Response Example (Spoofed Activity)
```json
{
  "activity_id": "strava_998234",
  "pss1_score": 12,
  "flags": ["missing_imu_gait_signature", "gps_emulator_detected"],
  "action": "REJECT_REWARD",
  "proof_hash": "ed25519:8f7a..."
})
```
## 🛡️ Integration Footprint
StrideProof requires **zero changes** to Strava's core ingestion pipeline. It acts as an asynchronous middleware:
1. Strava webhook pushes new activity FIT/JSON to StrideProof API.
2. StrideProof returns `verified: true/false` + `fraud_probability`.
3. Strava Trust & Safety dashboard highlights anomalies for manual review or auto-ban.
