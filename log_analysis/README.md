# Log Analysis Datasets

---

## Six-Month Monitoring Dataset From a Ten-Turbine Onshore Wind Farm in Greece (SMD10TOWFGR)

### Link
- Zenodo descriptor/article record: https://zenodo.org/records/15781269
- Article DOI page: https://doi.org/10.1109/IEEEDATA.2025.3574640
- AIT publication page: https://publications.ait.ac.at/en/publications/descriptor-six-month-monitoring-dataset-from-a-ten-turbine-onshor/
- Dataset DOI/PID reported inside the descriptor: https://doi.org/10.5281/zenodo.14546479

### Introduction
SMD10TOWFGR is a real-world SCADA monitoring dataset from a **10-turbine onshore wind farm in Greece**, covering **1 January 2020 to 30 June 2020**. The descriptor reports about **26,000 measurements from 131 sensors** and about **24,000 event-log entries per turbine**. The content is overwhelmingly **physical/operational** rather than network-centric, with examples including pitch angle, power output, generator speed, bearing temperature, wind conditions, and turbine alarms/events.

### Specific data fields
The official descriptor states that the consolidated release contains **20 per-turbine files/tabs**: **10 sensor files** named `WTxx_data.csv` and **10 log files** named `WTxx_logs.csv`, where `xx` runs from `01` to `10`. Sensor readings are sampled every **10 minutes**. Each `WTxx_data.csv` file contains a **timestamp column** plus **104 sensor columns** and **27 status-information columns**.

The 104 sensor columns are described as being grouped into categories such as:

- **Generator**
- **Gear**
- **Nacelle**
- **Rotor**
- **Ambient**
- **Grid**
- **Controller**
- **Blade**
- **HV Transformer**
- **Power Metrics**

The status information includes **Hour Counters** plus **System Logs and Alarms**. The descriptor also notes that the measurements include different aggregation types such as **maximum**, **minimum**, **absolute**, **average**, **standard deviation**, and **raw values**.

Each `WTxx_logs.csv` file follows the same event/alarm schema:

- `Code`
- `Description`
- `Detected`
- `Device ack (Acknowledgment)`
- `Reset/Run`
- `Duration`
- `Event Type`
- `Severity`

`Severity` is defined as an integer from **1 to 1000**.

### Suggested usage
This dataset is best suited to **condition monitoring**, **predictive maintenance**, **operational anomaly detection**, and **short-horizon forecasting** for wind turbines. For supervised fault classification, labels would typically need to be constructed from the **alarm/event logs** or from time windows around alarm codes, because the release is organized as **per-turbine sensor files plus separate event/alarm logs**, rather than as a ready-made one-row-one-label benchmark.

---

## Sherlock: A Dataset for Process-aware Intrusion Detection Research on Power Grid Networks

### Link
- Zenodo dataset page (v3): https://zenodo.org/records/18467070
- ACM DOI page: https://doi.org/10.1145/3714393.3726006
- Accessible paper PDF: https://www.comsys.rwth-aachen.de/fileadmin/papers/2025/2025-wagner-sherlock.pdf
- Dataset documentation site: https://sherlock.wattson.it/

### Introduction
Sherlock is a **process-aware intrusion detection dataset for power grid networks** generated with the **Wattson** co-simulator. It combines **power-grid process behavior** with **IEC 60870-5-104 communication**, and is designed for both **process-aware** and **network-based** intrusion detection research. The dataset contains **three scenarios**: `01-Basic`, `02-Semiurban`, and `03-Rural`. The first two provide **attack-free train sets** plus **attack-containing test sets**, while `03-Rural` provides **test data only** to support transferability studies.

The paper’s scenario metadata reports the following setup:

- `01-Basic`: train/test, **4 vantage points**, **12 h** each, **17 attacks** in test, **10 benign events**
- `02-Semiurban`: train/test, **6 vantage points**, **12 h** each, **29 attacks** in test, **10 benign events**
- `03-Rural`: test only, **8 vantage points**, **12 h**, **28 attacks**, **8 benign events**

### Specific data fields
Sherlock is **not a single CSV dataset**. It is a **multi-artifact release**. The Zenodo description states that each scenario contains:

- **network captures** of primarily **IEC 60870-5-104**
- **accurate labels** for attacks, recoveries, benign events, and normal operation
- **ground truth data**
- **device logs**
- **captures transcribed into IPAL (Industrial Protocol Abstraction Layer) format**

The `01-Basic.zip` preview exposes the concrete file structure clearly. Under `raw/train` and `raw/test`, the release includes artifacts such as:

- `control-center.zip`
- `data-point-map.json`
- `events.jsonl`
- `log/` with files such as `mtu-n1-ccx-service.log`, `mtu-n1-vcc-service.log`, and many `rtu-n***-rtu-service.log`
- `pcap/` with switch-mirror captures such as `switch-n302-pcap-mir2.pcap`
- `physical.zip`
- `sherlock-config.yml`
- `docs/` with network/power-grid diagrams

Under `ipal/train` and `ipal/test`, the preview shows:

- `events.json`
- `initial_state.json`
- `rules.py`

The same preview also shows state snapshots such as `train.n302.state.gz` and `test.n302.state.gz` for `01-Basic`.

The paper further explains the protocol/process semantics behind these files:

- Between the control center and RTUs, IEC 104 is used with a **10-second interval** for periodic unsolicited measurement transmissions.
- Floating-point values such as **voltages, currents, and power** are transmitted periodically.
- Booleans such as **circuit-breaker states**, **tap positions**, and **connectivity states** are sent spontaneously when they change.
- MTU control commands are acknowledged by RTUs, with negative confirmations for invalid commands.

For the IPAL representation, the paper states that Sherlock logs the **initial system state** and then parses each intercepted **IEC 104 packet** to update the state while mapping abstract **IOAs** to **human-readable identifiers**. The resulting observed state is logged **every second** from a single vantage point.

Sherlock covers **four attack families** in its examples:

- **DoS**
- **Industroyer-like control command insertion**
- **Drift Off** false-data injection
- **Control & Freeze** attacks

### Suggested usage
Sherlock is best suited to **sequence-aware**, **time-aware**, and **multi-view** intrusion detection research. Good experimental setups include:

- **train on benign `01-Basic` or `02-Semiurban`, test on attacked test sets**
- **cross-scenario transfer**, especially **train on `02-Semiurban` and test on `03-Rural`**
- **PCAP vs. IPAL vs. device-log comparisons**
- **process-aware detection** versus **network-only detection**

The dataset paper explicitly recommends reporting **Detected Attacks**, **False Alarms**, and **Average Time to Detection (TTD)** rather than relying on static point metrics such as plain F1-scores.

A reduced text/log benchmark can also be derived from files such as `mtu-n1-vcc-service.log` together with `events.jsonl`, but that would be a **custom subset of Sherlock**, not the official full-format release.




