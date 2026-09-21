

```markdown
# AIOps Assessment

## Task 1: Set Up and Understand

This project monitors `payment-service`. It checks service performance and detects payment timeouts, database errors, high CPU usage and high memory usage.

Main files:

- `data/service_data.json`: operational data
- `src/anomaly_detector.py`: anomaly detection
- `src/event_producer.py`: event producer
- `src/event_topic.py`: event topic
- `src/event_consumer.py`: event consumer
- `src/aiops_pipeline.py`: complete workflow
- `tests/`: validation tests

## Task 2: Analyse Data

The metric fields are:

- `response_time_ms`
- `cpu_percent`
- `memory_percent`

The log fields are:

- `log_level`
- `message`

Timestamps show when each event happened and keep the data in order.

Records from `10:00` to `10:04` and `10:07` to `10:09` were normal.

The `10:05` record had a high response time and a payment timeout error.

The `10:06` record had high response time, high CPU usage, high memory usage and a database timeout error.

## Task 3: Identify Anomalies

Two anomalies were detected:

- `10:05`: high response time and concerning error log
- `10:06`: high response time, high CPU, high memory and concerning error log

Normal records were not flagged.

The detector uses fixed thresholds, so it may miss gradual problems that do not cross the configured limits.

The result was:

```text
Records processed: 10
Anomalies detected: 2
Events consumed: 2
```

The detected events were:

```text
Timestamp: 2026-09-20T10:05:00
Reasons: High response time, Concerning log event

Timestamp: 2026-09-20T10:06:00
Reasons: High response time, High CPU utilization, High memory utilization, Concerning log event
```

## Task 4: Verify Event Flow

The event flow is:

```text
Data -> Detector -> Event -> Producer -> Topic -> Consumer -> AIOps
```

The detector checks each record and creates an event when an anomaly is found.

The producer publishes the event to the topic.

The topic stores the event.

The consumer receives and processes the event.

The final AIOps output displays the detected operational issue.

## Task 5: Investigate and Correct the Workflow

Two problems were found.

The first problem was in `anomaly_detector.py`. The detector checked only `WARNING` logs, so `ERROR` logs were not detected.

The correction was to check both `WARNING` and `ERROR` logs.

The second problem was in `aiops_pipeline.py`. The producer and consumer were using different topic objects. Because of this, the consumer could not receive the events published by the producer.

The correction was to use the same `anomaly-events` topic for both the producer and consumer.

The corrected flow is:

```text
AnomalyDetector -> EventProducer -> anomaly-events topic -> EventConsumer -> AIOps
```

## Task 6: Execute the End-to-End Pipeline

The complete pipeline was run using:

```bash
python3 `aiops_pipeline.py`
```

The result was:

```text
Records processed: 10
Anomalies detected: 2
Events consumed: 2
```

This confirms that:

- Operational data was processed.
- Anomalous behaviour was detected.
- Anomaly events were generated.
- Events were published.
- Events were consumed.
- Events were processed successfully.
- The final output showed the payment service problems.

## Task 7: Update the README

This README contains:

- The AIOps scenario
- The monitored service
- The operational problem
- The metrics and log fields
- The normal and unusual observations
- The anomaly detection results
- The event-processing flow
- The workflow problems and corrections
- The final pipeline result
- A limitation of the detection approach
- The commands needed to reproduce the demonstration

To reproduce the demonstration, run these commands from the project root:

```bash
python3 `aiops_pipeline.py`
python3 -m pytest
```
```