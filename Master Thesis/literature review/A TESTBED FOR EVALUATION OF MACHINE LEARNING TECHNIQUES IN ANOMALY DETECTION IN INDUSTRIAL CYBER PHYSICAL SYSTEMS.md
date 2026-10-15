
---

paper: [link](https://www.revistadyna.com/Articulos/Ficha.aspx?IdMenu=33094c58-1bb5-4163-8382-a0d0942862d2&Cod=11516)  
Published on: 27 aug 2025  
AI tool chat link: [link]([https://claude.ai/chat/7063c7d4-6f41-4d8b-9ee2-a89d211f5f0d](https://claude.ai/chat/64f2b605-b64f-4e7f-b8db-829599747750))
same authors as [[Master Thesis/literature review/DAYPSCI]] 

---

Buzz words:

+ DAyPSCI 
+ testbed

---

Components used:
+ 2 PLC'S one for the profinet I/O interface and one as control system.
	+ PLC-2 : S7-1215c       *(PLC as profinet I/O interface)*
	+ PLC-1 : S7-1215c-1PN   *(runs control logic)*
+  HMI pannel and industrail switch and SQL server database for logging.
+ PCSimu digital twin for simulation.

<span style="color: var(--color-red)">Q. what are the both plc'S responsible for?</span>
- **PLC-1** is responsible for the **control logic** — running the GEMMA state machine and deciding when to command actuators based on sensor feedback.
- **PLC-2** is responsible for acting as the **Profinet I/O interface** — since there's no real hardware, it fills the Level 0 gap by translating Profinet I/O communication between PLC-1 and the digital twin (not just "responding instead of hardware," but specifically handling the **network communication protocol** so it looks like real Profinet I/O traffic to PLC-1 and to anyone monitoring the network)
---
PROCESS IN SIMULATION:

+ simple part making process : feed, mark, eject

---
Test-bed used for the generation of dataset:

+ The test-bed used here is hybrid it has physical systems as well as virtual systems, physical hardware include PLC's and virtual system include sensors and actuators.

+ Architecture of testbed:
	+ **Level 0:** Digital twin (virtual process) + PLC-2 (Profinet I/O interface bridging digital twin ↔ PLC-1)
	- **Level 1:** PLC-1 (control logic/workflow), connected to the industrial switch
	- **Level 2:** Industrial switch links everything to HMI, engineering station (TIA Portal), DBSQL, and the Kali Linux/Wireshark monitoring PC
---

Dataset Generation and labelling
qqqqqqq

the variables what they collected are  in the image below:

![[2026-09-21 11_49_55-Greenshot.png]]


procedure:
First, **PC-1 launches the digital twin**, establishing Profinet I/O communication between **PLC-1 and the digital twin — via PLC-2**, which acts as the Profinet I/O interface.
Then the operator configures the batch via the HMI, but **one shared configuration for the whole batch**: number of parts, marking actuation time, and number of marking strikes per part.
The operator then activates the **"dataset switch"** on the HMI, which starts logging events to the **SQL Server database**.

When the system starts, it's in **Gemma mode A1** (stop, initial state), then switches to **F1** (normal production). When a part arrives, sensor **B1** detects it, and **PLC-1**  issues the load command (**YA+**). The actuator then moves, and as it does, the sensors (B_a0, B_a1) and actuator states change — all of these state changes get logged as rows in the dataset.


The authors generated **three datasets**:

1. **Without anomalies** — 100 parts, labeled **Anomaly = 0**
2. **Type 1 (sensor anomalies)** — 15 parts, sensor faults **deliberately introduced in a controlled manner** (via the B0x/B1x fault-simulation switches mentioned in Section 3.1 — these let the operator manually force a sensor to report a false/faulty reading), labeled **Anomaly = 1**
3. **Type 2 (actuator anomalies)** — 15 parts, faults introduced in the **solenoid valves** (Yx+/Yx- switches), labeled **Anomaly = 2**


<span style="color: var(--color-red)">Q. how did they introduce anomaly?</span> 


+ The testbed includes dedicated manual fault-simulation switches — **B0x/B1x** for sensors and **Yx+/Yx-** for the solenoid valves (pre-actuators) controlling each actuator (A, B, C). By flipping these switches, the operator can deliberately force a sensor to misreport its state or a valve to fail to respond to a command, at a chosen point during normal operation. When this happens, the control system (PLC-1) detects the discrepancy between the expected and actual behavior and switches into **Gemma mode D2** (fault diagnosis), notifying the operator via the HMI; after acknowledgment and reset, the system returns to normal operation. Throughout this whole sequence, all sensor/actuator states, timestamps, and the Gemma mode transitions are logged as usual, with the **Anomalies** column explicitly marked **1** for sensor faults or **2** for actuator faults — giving the researchers precise, ground-truth-labeled anomalous data alongside the normal operation data.



---

Methodology for anomaly detection


Three models used
1. KNN     #KNN
2. svm      #SVM
3. random forest   #randomforest

mixed dataset was used contained normal data with anomalous data as well representing type 1 and type 2 anomaly.


Preprocessing was done to make all parts have the same length (same number of rows), since parts naturally generate different numbers of event rows. Anomalous parts likely have more rows (fault-handling adds extra steps), so they're closer to the maximum length. Normal parts have fewer rows, so it's the normal parts that get padded with zero-filled rows (not columns) to match the maximum length.


dataset split: 70,15,15

preformace evaluation: Acc, precision, recall, F1 score, AUC-ROC.


hyperparameter tunning as also done look for the section 3.3.1


---

Results 

+ all the results show that **random forest** achieves the best results

---

linked papers:




outlook:

- [ ] paper suggests to use LSTM and GNN techniques.
- [-] need to look for the dataset and maybe replicate this.
	- No link is provided
- [-] Need to look for other models used.