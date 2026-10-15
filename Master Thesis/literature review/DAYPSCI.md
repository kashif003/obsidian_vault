
---


paper: [link](https://pmc.ncbi.nlm.nih.gov/articles/PMC13505426/pdf/main.pdf)
Published on: 15 aug 2026
AI tool chat link: [link](https://claude.ai/chat/7063c7d4-6f41-4d8b-9ee2-a89d211f5f0d)

----
#gemma 

Buzz words: 
+ cyber physical system (cps).
+ GEMMA Guidelines

---
what did they record?
+ Changes in system variables are recorded rather than fixed sampling rate.
+ each event is associated with inter-event time.(delta t)
---

Data description:

Three files:

1. CSV file: processed event based dataset, contains info about variables, labels and features.
2. PCAPNG fle: raw network traffic collected form profinet based communication.
3. Markdown file: detailed description of the setup.

---
Experiments and design:
#s71200 #s71500

TOOLS USED:
PLC = S7-1200, S7-1500

digital sensors, electormagnatic actuators plus  a digital twin.

---

Data acquisition strategy:

+ it was event based where events were generated upon relevant change in system variables.

each event has the timestamp which is obtained form the PLC system which correspond to system change, these events were temporarily stored in the internal ring buffer within PLC memory and periodically transmitted to an external storage.


----

Understanding the dataset:

+ they also have scan id's and event order to preserve the logical execution order within PLC scan.
+ They have labelled the dataset based on two things: 1. when they injected the anomalies, 2. GEMMA states
	+ 2. these are the states that we had in the turbine dataset like status type id.
	+ they used it because this id is changes by the logic of the plc so if the plc takes extra time to detect the change in reading that extra time is also labelled accordingly.

---


How did they capture and filter the data?

They used Wireshark to capture the raw network traffic (Profinet packets) via port mirroring on the switch, and saved this capture **unfiltered and undecoded**, as a complete PCAPNG file per batch. The actual data filtering/selection in this dataset happens separately, at the **PLC control-program level**, where only a predefined set of process variables (specific sensors and actuators) are monitored for edge changes — this filtered event stream is what feeds into the CSV, not the PCAPNG.

----

DATA Description.

This section just mentions the description of the data which column is what etc.

----

Fault injection:

- **Fault types:** 6 sensor faults (stuck@0, stuck@1, delay/jitter, noise, glitch, degradation) + 3 valve faults (stuck off, stuck on, delay) + 1 combined type.
- **Planning tool:** Before the experiment, researchers configure batch size, random seed, class distribution, eligible sensors/valves, fault type, and parameters (p1/p2) → generates a fault-injection plan (part → channel → target → type → params).
- **Live injection, not post-hoc editing:** Faults are applied to the actual running system (real + digital twin) via an **external PLC-2**, while data is being recorded — not added into clean data afterward. 
	- Sequence: Fault Injection → Applied to CPS → System Response → Observable Effects → Labeling.
- **Mechanism not disclosed:** Paper confirms faults are injected externally via PLC-2 but doesn't explain the exact technical method (signal override vs. register overwrite vs. digital-twin substitution).
- **Why external:** PLC-1's control program stays completely unchanged → keeps fault injection cleanly separated from process logic.

Ground truth labelling

- **Two label sources merged:**
    - _Externally defined_ (from fault injection plan) = the "cause" — known in advance since researchers configured it.
    - _Runtime GEMMA states_ = the "effect" — how PLC-1's own control logic actually reacts as the fault plays out live.
- **Merge point:** Config parameters are attached to each event **at acquisition time**, alongside the observed GEMMA state → final row has both cause fields (`Fault`, `FSx/TFSx`, `FCVx/TFCVx`) and effect field (`Anomaly` 0–4).
- **Why two sources:** Fault _activation_ and anomaly _manifestation_ can be temporally decoupled (e.g., fault set at t=10s, but system logic doesn't "feel" it until t=13s) — dual labeling lets you trace that gap


---



Questions:

<span style="color: var(--color-red)">Q: when we have multile events happening at the same time how can we get the data sequence then and how can we still check the operation cycle?</span>

*ans:* they have made a sacan id and event order to tackle this problem.

**`scan_id`**: This gives an identifier to the PLC scan cycle during which an event occurred. If multiple events happen within the same scan cycle (i.e., appear simultaneous at the timestamp/cycle resolution), they will share the same `scan_id`.

**`event_order`**: Since `scan_id` alone can't distinguish the order between events that share the same scan cycle, `event_order` records the actual internal execution order of events _within_ that same scan — telling you which one the PLC logically processed first, second, etc.

Together, `scan_id` + `event_order` solve the "simultaneous events" problem: `scan_id` groups events into the same cycle, and `event_order` breaks the tie to recover their true sequence.

**`part_id`**: This solves a _different_ problem — not simultaneity, but **interleaving of multiple objects** moving through the system at once. Since the whole system logs _all_ events from _all_ parts into a single continuous event stream, `part_id` tags each event with **which physical part/object** it belongs to. This lets you filter the global (mixed) event stream down to only the events belonging to one specific part, allowing you to reconstruct that part's individual journey through the process (e.g., sensor1 → sensor2 → sensor3 → sensor4) even though its events are physically interleaved in the log with events from other parts moving through the line at overlapping times




----


Linked papers:

1. <span style="color: var(--color-yellow)">literature review paper</span> : [link](https://www.mdpi.com/2079-9292/15/1/20)
2. <span style="color: var(--color-red)">Paper from same author</span> : [link](https://www.revistadyna.com/Articulos/Ficha.aspx?IdMenu=33094c58-1bb5-4163-8382-a0d0942862d2&Cod=11516)



OUTLOOK:


- [x] Need to look into the references of the paper to find interesting papers.
+ Need to explore this dataset.
	+ maybe train a trail model on this dataset.
+ Need to confirm if we can follow the similar path for creating a dataset for our use-case.


---
work on dataset [[Master Thesis/Benchmarks/DAYPSCI|DAYPSCI]]
