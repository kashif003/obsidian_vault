
---
## Progress So Far

- The data collection pipeline is ready and currently collects data in two modes:
  1. With a fixed sampling rate
  2. On event change
- Data was manually collected for 60 seconds:
  - **Sampling-rate mode:** CSV file of shape (13965, 4), 0.74 MB
  - **Event-change mode:** CSV file of shape (284, 4), 0.02 MB


## Discussion: Anomaly Injection

Two approaches were proposed:

1. **In the PLC itself:** Add a function that makes the PLC produce faulty data or outputs (this approach was used in one of the reference papers).
2. **In the simulation (real-world scenario):** Add functionality that alters the simulation after a certain period or with a certain probability, so that it behaves differently from the normal run.

**Decision:** After a brief discussion, the second approach (simulation-based injection) was chosen.

## Challenges and Upcoming Tasks

- Collect data for a normal cycle. Currently, objects are dragged manually.
- Decide how to store the data:
  - If storing at a fixed sampling rate, determine how much data to keep and in which format, to avoid data explosion.
- Figure out how to inject anomalies within the simulation itself:
  - For example, change an object's position after a certain time, or use another method.