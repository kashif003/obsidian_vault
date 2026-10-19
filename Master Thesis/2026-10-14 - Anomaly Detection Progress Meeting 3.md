---
created: 2026-10-14
updated: 2026-10-14
tags:
  - meetingNote
  - "#masterThesis/anomalyDetection"
  - meeting/masterThesis/anomalyDetection
  - meeting/sitlog
type: meetingNote
topic: Anomaly Detection Progress Meeting
masterThesis: Digital-Twin-Based Synthetic Data Generation for Anomaly Detection and Localization in Intralogistics Systems using Machine Learning
student: Kashif Riyaz
---
## Anomaly Detection Progress Meeting 3

### Literatur review
- Paper 3 [[& 2026-10-14 Deep Learning for Time Series Anomaly Detection A Survey]]
	- Types of Anomalies
	- Model Types
		1. Classical ML
		2. Forecasting-based
		3. Reconstruction-based
		4. Representation-based
	 ![[anomaly_detection_progress_2_2_1.png|300]]
- Paper 4 [[& 2026-10-14 CARLA Self-supervised contrastive representation learning for time series anomaly detection]]
	- CARLA (Self-supervised ContrAstive Representation Learning for Time Series Anomaly Detection)
	- Representation-based model
	- detects the anomaly types covered in the previous paper
	- Injects anomalies into specific windows
	- Evaluated on several other datasets
	![[anomaly_detection_progress_2_2_2.png|300]]

### Automatic Simulation loop
- Looped simulation runs automatically
- Current approach: change actuator input through Simulation
- Better approach: change actuator input through PLC

![[anomaly_detection_progress_2_2_3.png|300]]


### Strategy for injecting anomalies
- Flickering sensor: dirty sensor gives unstable on/off readings
- Jammed goods: sensor failure, or an object stuck on the belt or chain
- Time delay: slowdowns caused by traffic on the line
- False signal: something stuck on the belt or chain triggers it


### Next Steps & Goals
1. Conduct more literature review
2. Record the normal dataset in automatic loop
3. Inject anomalies in the simulation environment
4. Perform EDA, and maybe train a model


---
## Meeting Notes
### Overview
In the meeting we collected three possible anomalies for anomaly detection in the digital twin: flickering sensors, flickering at the turntable, and slowed-down goods. The definitions will then go to Nico, who will integrate them into the simulation software.


### 1 Flickering Sensor (Photoelectric Sensor)
A misadjusted or disturbed photoelectric sensor delivers an on-off-on-off signal instead of a clean occupied signal. Typical indicator: constant on/off edges even though there are no goods on the system.

**Target State:** The sensor is adjusted so that it triggers at the edge of the pallet. It is normally mounted at a slight angle to the mirror (reflector).

**Possible Causes:**
- **Sensor set too low:** The beam hits the pallet legs. The gaps between the legs produce an on-off-on-off signal for each pallet.
- **Incident Light:** External light makes the sensor flicker, typically always at certain times of day.
- **Reflective Packaging:** The film wrapping of the goods acts like the mirror and reflects the light. The pallet is not detected and travels “incognito”, which is especially critical on accumulation conveyors.

**Consequences on accumulation conveyor:** Pallets traveling one behind the other as a train are meant to be separated at the separation station. If the beam shines through the pallet, the controller assumes the pallet has already been separated, and the pallet does not move on. The PLC only detects this through the timeout monitoring of the next sensor.

**What is an accumulation conveyor (Pulkförderer)?** A conveyor section that holds up to 4 pallets, which travel together as a train. A separation station then separates the pallets again.

**Note:** The conveyed goods vary, so the possible anomalies also differ depending on the goods.


### 2 Flickering at the Turntable
The turntable moves all the way up or stands at a slight angle, but the goods still move on because the distance is still within tolerance. The flickering causes the goods to stop too often-

In many cases the handover still works normally because the gap is small enough. However, it can also lead to handover problems.

--> Under which conditions does the gap lead to handover problems?


### 3 Slowed-Down Good (Jammed Goods)
The material flow stays continuous, but the travel times of the goods gradually drift away from the target value. The PLC only reports an error once a fixed time limit is exceeded.

**Possible causes:**
- Defective motor brake (stuck or partially stuck): goods become slower.
- Oil on the belt: goods become faster.

**Example:** The target time for a section is 5 s, but the PLC only reports an error after 10 s. If the travel time creeps up over time (5.2 s → 5.6 s → … → 8.0 s → … → 10 s), the PLC does not see it until the very end.

**Detection approach:**
- Evaluate based on the mean or median: does the travel time rise or fall on average over time (deviation from the target value)?
- Individual outliers above or below are local events and are not an anomaly on their own.
- Result: an early hint to the operator to check the affected area before the PLC reports an error.



### 4 Reference: Occupancy Errors in the PLC
The PLC triggers an occupancy error (Belegtfehler) after the following times:

| Sensor                            | Occupancy error after |
| --------------------------------- | --------------------- |
| Photoelectric sensor (photo cell) | 3 s                   |
| Limit switch                      | 200–300 ms            |


---
## Brainstorm
Beyond the three anomalies from the meeting, most faults in a conveyor system with turntables and lifts show up as one of four signal patterns: 
- Timing drift
- Unusual sensor edge patterns 
- Sequence violations 
- Count mismatches 
The candidates below are unvalidated ideas to discuss.

### Sensors

| Anomaly | Possible cause | Signature in the data | Detection idea |
| --- | --- | --- | --- |
| Stuck sensor (always on / always off) | Dirty lens, cable break, reflector knocked off | No edges although upstream sensors report traffic, or permanently occupied | Compare expected vs. actual edges per pallet passing upstream |
| Contact bounce at limit switch | Worn switch, vibration | Several edges within a few ms | Count edges within a short time window |
| Shifted detection time | Sensor or reflector moved | Occupied signal starts earlier or later relative to neighboring sensors | Track offset between consecutive sensors over time |
| Sequence violation | Swapped wiring, ghost signal | Downstream sensor triggers before upstream sensor | Check sensor order against the conveyor topology |

### Conveyor drives

| Anomaly                    | Possible cause                            | Signature in the data                                     | Detection idea                                 |
| -------------------------- | ----------------------------------------- | --------------------------------------------------------- | ---------------------------------------------- |
| Delayed start              | Worn contactor, frequency converter issue | Longer time between start command and first sensor change | Trend of start-up delay per section            |
| Belt or chain slip         | Worn belt, low chain tension, heavy load  | Motor running but travel time longer and more variable    | Variance of travel time, not just the mean     |
| Micro-stops                | Loose connection, intermittent fault      | Short unplanned stops and restarts                        | Frequency of stops without a downstream reason |
| Conveyor runs without load | Missing energy-saving logic, stuck sensor | Motor active while section is empty                       | Motor on-time vs. occupancy time               |

### Turntables

| Anomaly | Possible cause | Signature in the data | Detection idea |
| --- | --- | --- | --- |
| Rotation time drift | Gearbox wear, brake dragging | Time from rotate command to end-position switch increases | Trend of rotation time per direction |
| Overshoot or undershoot | Worn brake, wrong parameters | End-position switch flickers, correction movements | Count corrections per rotation |
| Rotation with pallet not fully on | Interlock or sensor fault | Rotation starts while entry sensor is still occupied | Rule check on the sensor sequence |
| Asymmetric behavior | Mechanical damage on one side | Clockwise and counterclockwise times differ increasingly | Compare both directions over time |

### Lifts

| Anomaly | Possible cause | Signature in the data | Detection idea |
| --- | --- | --- | --- |
| Travel time drift up/down | Brake wear, counterweight issue, heavy load | Ascent or descent time increases; up/down ratio changes | Trend per direction and level |
| Level not reached precisely | Chain or belt elongation | Level switch reached late or corrected; handover problems | Count leveling corrections per trip |
| Lift sinks while stopped | Brake slip | Position or level switch changes without a command | Detect state changes during idle |
| Frequent safety stops | Light curtain triggered, door opened | Rising number of safety events | Count per shift or time of day |

### Material flow and tracking

--> I think, for this we also need information from the MFR system.

| Anomaly | Possible cause | Signature in the data | Detection idea |
| --- | --- | --- | --- |
| Lost or ghost pallet | Sensor fault, manual removal or insertion | Pallets in ≠ pallets out for a section | Balance counts per section |
| Wrong routing | Scanner misread, wrong destination data | Pallet appears on an unexpected path | Compare actual path with planned route |
| Rising read errors | Dirty scanner, damaged labels | More barcode or RFID no-reads | No-read rate per scanner over time |
| Gap between pallets too small or large | Separation fault, timing issue | Unusual spacing between consecutive pallets | Distribution of gaps between edges |
| Growing queue or throughput drop | Bottleneck, slow downstream element | Upstream sections stay occupied longer | Occupancy time and throughput per section |
| Manual intervention | Operator moves a pallet by hand | Tracking jumps, unexpected sensor edges | Edges that fit no tracked pallet |

### Controls and communication

| Anomaly | Possible cause | Signature in the data | Detection idea |
| --- | --- | --- | --- |
| Signal latency or jitter | Fieldbus load, network problems | Timestamps shift or spread more | Variance of known fixed intervals |
| Missing signals | Dropped telegrams | Gaps in the data stream | Check for missing cycles |
| Time-of-day effects | Cold start in the morning, incident light, shift change | Slower travel or more errors at certain times | Compare behavior per hour of day |
