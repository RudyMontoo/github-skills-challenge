AIOps Payment-Service Monitoring Project

# Task 1: Set Up and Understand

 This project monitors **payment-service**. It checks service performance and detects:

 - Payment timeouts
- Database errors
- High CPU usage
- High memory usage

 ## Main Files

 | File | Purpose |
| --- | --- |
| `service_data.json` | Operational data |
| `anomaly_detector.py` | Anomaly detection |
| `event_producer.py` | Event producer |
| `event_topic.py` | Event topic |
| `event_consumer.py` | Event consumer |
| `aiops_pipeline.py` | Complete workflow |

# Task 2: Analyse Data

 ## Metrics

 The monitored metrics are:

 - `response_time_ms`
- `cpu_percent`
- `memory_percent`

 ## Log Fields

 The log fields are:

 - `log_level`
- `message`

 Timestamps show when each event happened and keep the data in chronological order.

 ## Data Analysis

 - Records from **10:00 to 10:04** and **10:07 to 10:09** were normal.
- The **10:05** record had a high response time and a payment timeout error.
- The **10:06** record had high response time, CPU, and memory usage, along with a database timeout error.

 ## Result

```
==================================================
AIOps Pipeline Result
==================================================
Records processed: 10
Anomalies detected: 2
Events consumed: 2

Detected Events:

Service: payment-service
Timestamp: 2026-09-20T10:05:00
Type: ANOMALY
Reasons: High response time, Concerning log event

Service: payment-service
Timestamp: 2026-09-20T10:06:00
Type: ANOMALY
Reasons: High response time, High CPU utilization, High memory utilization, Concerning log event
```

 # Task 3: Identify Anomalies

 Two anomalies were detected:

 1. **10:05:** High response time and an error log.
2. **10:06:** High response time, high CPU, high memory, and an error log.

 Normal records were not flagged.

 > **Limitation:** The detector uses fixed thresholds, so it may miss gradual problems.

 # Task 4: Verify Event Flow

 The event flow is:

```
Data → Detector → Event → Producer → Topic → Consumer → AIOps
```

 - The **producer** publishes events.
- The **topic** stores the events.
- The **consumer** receives the events.

 # Task 5: Fix Workflow

 Two workflow issues were fixed:

 1. The detector was checking only `WARNING` logs. It was changed to check both `WARNING` and `ERROR`.
2. The producer and consumer were using different topic objects. They were changed to use the same `anomaly-events` topic.

 # Task 6: Run Pipeline

 ## Command

```
python3 aiops_pipeline.py
```

 ## Result

 - **10** records processed
- **2** anomalies detected
- **2** events consumed

 # Task 7: Test and Reproduce

 ## Test Output

```
============================================================ test session starts ============================================================
platform linux -- Python 3.13.15, pytest-8.4.1, pluggy-1.6.0
rootdir: /workspaces/github-skills-challenge
plugins: cov-7.1.0
collected 8 items

tests/calculations_test.py ....                                       [ 50%]
tests/test_aiops_pipeline.py ....                                     [100%]

============================================================= 8 passed in 0.08s =============================================================
```

 ## Run the Pipeline

 Run the following command from the project root:

```
python3 aiops_pipeline.py
```

 ## Run All Tests

```
python3 -m pytest
```

 ## Test Result

 **8 tests passed successfully.**
