# BanglaNetSim v2.0.1 — Research Report

Generated: 2026-10-11T20:37:40.291264+00:00

## Dataset and scope

- Rows: 1,000,000
- Columns: 19
- Data status: entirely synthetic; not real ISP measurements.
- Target: simulator-generated `performance_class`.
- Task: estimate the simulator-defined class from context features.

## Experimental design

- Training rows: 571,545
- Validation rows: 142,991
- Test rows: 285,464
- Test scenarios: `mixed_stress`, `packet_loss_stress`
- Features: region, connection_type, access_type, route_type, congestion_level, wifi_signal_quality, plan_speed_mbps
- Excluded outcome measurements: download, upload, latency, jitter, packet loss.
- Excluded metadata: scenario, split, timestamp, session ID, random seed.

## Test results

| Model | Accuracy | Macro-F1 | Weighted-F1 |
|---|---:|---:|---:|
| Dummy baseline | 0.5283 | 0.1728 | 0.3653 |
| Decision Tree | 0.8414 | 0.7758 | 0.8534 |
| Logistic Regression | 0.7648 | 0.7178 | 0.7757 |
| Random Forest | 0.8415 | 0.7758 | 0.8535 |

## Important interpretation

The target is derived from download speed, latency, and packet loss using rules inside the simulator. Those measurements were excluded from the predictive model inputs to avoid directly reconstructing the target from its defining measurements.

Because both features and labels are synthetic, these results measure how well models learn this simulator's patterns on held-out scenarios. They do not establish performance on real Bangladeshi networks or prove generalization to real ISP data.

The test split contains held-out scenarios. This is a useful stress test, but results may reflect differences between generated scenarios rather than real-world domain generalization.

## Recommended next step

Validate the simulator against independently collected, consented, real network measurements before making claims about actual network performance.
