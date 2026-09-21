Updated todo list

Updated todo list

Task 1: Set Up and Understand

This project monitors payment-service. It checks service performance and detects payment timeouts, database errors, high CPU and high memory usage.

Main files:

service_data.json: operational data

anomaly_detector.py: anomaly detection

event_producer.py: event producer

event_topic.py: event topic

event_consumer.py: event consumer

aiops_pipeline.py: complete workflow

Task 2: Analyse Data

Metrics are response_time_ms, cpu_percent and memory_percent.

Log fields are log_level and message.

Timestamps show when each event happened and keep the data in order.

Records from 10:00 to 10:04 and 10:07 to 10:09 were normal.

The 10:05 record had a high response time and payment timeout error.

The 10:06 record had high response time, CPU, memory and a database timeout error.

RESULT->
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




Task 3: Identify Anomalies

Two anomalies were detected:

10:05: high response time and error log.

10:06: high response time, high CPU, high memory and error log.

Normal records were not flagged.

The detector uses fixed thresholds, so it may miss gradual problems.

Task 4: Verify Event Flow

The flow is:

Data -> Detector -> Event -> Producer -> Topic -> Consumer -> AIOps

The producer publishes events, the topic stores them and the consumer receives them.



Task 5: Fix Workflow

The detector was checking only WARNING logs. It was changed to check both WARNING and ERROR.

The producer and consumer were using different topic objects. They were changed to use the same anomaly-events topic.

Task 6: Run Pipeline

Command used:

python3 `aiops_pipeline.py`

Result:

10 records processed

2 anomalies detected

2 events consumed

Task 7: Test and Reproduce
============================================================ test session starts ============================================================
platform linux -- Python 3.13.15, pytest-8.4.1, pluggy-1.6.0
rootdir: /workspaces/github-skills-challenge
plugins: cov-7.1.0
collected 8 items                                                                                                                           

tests/calculations_test.py ....                                                                                                       [ 50%]
tests/test_aiops_pipeline.py ....                                                                                                     [100%]

============================================================= 8 passed in 0.08s =============================================================

Run the pipeline from the project root:

python3 `aiops_pipeline.py`

Run all tests:

python3 -m pytest

Test result:

8 passed