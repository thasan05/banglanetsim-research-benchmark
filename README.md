# BanglaNetSim Research Benchmark

A reproducible **synthetic broadband network simulation benchmark** with 1,000,000 simulated sessions, data-quality audits, scenario-held-out evaluation, and machine-learning baselines.

> **Important:** BanglaNetSim is synthetic data. It is not a collection of real measurements from Bangladeshi internet service providers, and the reported model scores must not be interpreted as real-world ISP prediction performance.

## Project at a glance

- **Dataset version:** v2.0.1
- **Dataset size:** 1,000,000 rows × 19 columns
- **Data generation:** simulated network sessions across seven scenarios
- **Quality audit:** zero missing cells, zero duplicate session IDs, finite numeric measurements, all records marked synthetic
- **Evaluation design:** train/validation/test splits, with `mixed_stress` and `packet_loss_stress` held out from training
- **Task:** estimate the simulator-defined `performance_class` from context features only

## Baseline results

Results below are from the v2.0.1 context-only experiment on the held-out synthetic test scenarios.

| Model | Test accuracy | Test macro-F1 |
|---|---:|---:|
| Dummy baseline | 52.83% | 0.1728 |
| Decision Tree | 84.14% | 0.7758 |
| Logistic Regression | 76.48% | 0.7178 |
| Random Forest | 84.15% | 0.7758 |

The Random Forest and Decision Tree results are essentially tied on these metrics. They do not establish that either model generalizes to real network measurements.

## Features and target

The model inputs are context features available before measured outcomes:

- Region
- Connection type
- Access type
- Route type
- Congestion level
- Wi-Fi signal quality
- Plan speed

The target is `performance_class` (`Excellent`, `Good`, `Fair`, or `Poor`). In the simulator, this label is generated using rules based on download speed, latency, and packet loss. Those outcome measurements—and upload speed and jitter—are deliberately excluded from the context-only model inputs to avoid directly reconstructing the target from its defining measurements.

## Dataset schema

The dataset contains these 19 columns:

| Column | Description |
|---|---|
| `session_id` | Unique simulated session identifier |
| `timestamp_utc` | Simulated timestamp in UTC |
| `region` | Simulated region category |
| `scenario_type` | Simulation scenario |
| `split` | Train, validation, or test assignment |
| `connection_type` | Simulated connection category |
| `access_type` | Simulated access technology/category |
| `route_type` | Simulated route category |
| `plan_speed_mbps` | Nominal plan speed in Mbps |
| `congestion_level` | Simulated congestion label |
| `wifi_signal_quality` | Simulated Wi-Fi signal category |
| `download_mbps` | Simulated download throughput in Mbps |
| `upload_mbps` | Simulated upload throughput in Mbps |
| `latency_ms` | Simulated latency in milliseconds |
| `jitter_ms` | Simulated jitter in milliseconds |
| `packet_loss_pct` | Simulated packet loss percentage |
| `performance_class` | Rule-derived synthetic target label |
| `is_synthetic` | Synthetic-data marker |
| `random_seed` | Seed metadata recorded by the generator |

## Repository contents

This repository is the reproducible source-code and documentation companion to the dataset. It currently includes a cleaned notebook for the v2.0.1 research pipeline.

- [Reproducible v2.0.1 notebook](./banglanetsim_v2_0_1_reproducible_notebook.ipynb)
- Quality-audit and experiment outputs, charts, and the detailed research report are the next publication step.

**Dataset:** [BanglaNetSim Research Benchmark on Kaggle](https://www.kaggle.com/datasets/thasan05/banglanetsim-research-benchmark)

The full generated dataset is distributed separately to avoid bloating this Git repository.

## Reproducibility

The v2.0.1 experiment used a fixed model random state of 42. The Random Forest baseline used 100 trees, a maximum depth of 20, minimum leaf size of 5, balanced-sub-sample class weights, and all available CPU workers. The feature preprocessing and train/validation/test assignments are described in the experiment notebook.

Reported metrics were obtained from the generated v2.0.1 dataset and the recorded experiment run. Exact replication may vary slightly with library versions and runtime environment. Do not infer real-world validity from reproducibility alone.

## Limitations

1. **Entirely synthetic:** distributions and dependencies are determined by the simulator's assumptions.
2. **Rule-derived target:** `performance_class` is assigned from synthetic network outcomes using predefined thresholds.
3. **Scenario shift:** the test set holds out two generated scenarios; success on these scenarios does not establish real-world domain generalization.
4. **No real ISP validation:** the benchmark has not been validated against independently collected network telemetry.
5. **Not suitable for operational decisions:** do not use these results to rank providers, diagnose actual connections, or make service-quality claims.

## Roadmap

- [x] Generate v2.0.1 synthetic dataset
- [x] Audit dataset integrity
- [x] Run four context-only baseline models
- [x] Generate research report and result package
- [x] Add the reproducible v2.0.1 research notebook
- [x] Publish the dataset on Kaggle and link it here
- [ ] Add the final report and visualization assets

## Citation

If you use this project, please cite the repository URL and record the exact dataset version used. A formal academic citation/DOI may be added if a stable archival release is created.

## License

No license has been selected yet. Until a license is added, do not assume the code or accompanying materials are available for unrestricted reuse.
