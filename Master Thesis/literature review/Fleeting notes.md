

---


Papers regarding anomaly detection



old:
1. ([TABOR: A Graphical Model-based Approach for Anomaly Detection in Industrial Control Systems](https://dl.acm.org/doi/pdf/10.1145/3196494.3196546))  2018
2. [Detecting Anomalies in Programmable Logic Controllers through Parameter Modeling](https://essay.utwente.nl/fileshare/file/88654/Raghuraman_MA_EEMCS.pdf)  2021



new:
1. [Predictive maintenance in IOT, early fault detection in industrail equipment](https://www.americaspg.com/journal/18/article/2087/pdf)
	+ using the real data but they augmented it for anomalies.
2. [Hybrib deep learning for predictive mantenance in industrial machinery using  LSTM and MLP models](https://www.mdpi.com/2075-1702/14/2/191)
	+ using the synthetic data from the digital twin which is bound to be failing at certain point in time.
3. https://pmc.ncbi.nlm.nih.gov/articles/PMC13505426/pdf/main.pdf
	+ dataset generation using S7-1200 and S7-1500 devices

4. [paper](https://ieeexplore.ieee.org/abstract/document/11623646?casa_token=HaZ1Ttofr6oAAAAA:Aj6fiFzuFn8EcjzZr-S7TAP4NJPHGvmhP2PjQzKPDkZ-MWEb12cBjeT61_gUieIBuSCA1OAoNoWn6Q)
	+ used LSTM model on swat dataset and converted into SCL

5. https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=10905775 , [another paper](https://pdf.sciencedirectassets.com/312019/1-s2.0-S2352711024X00059/1-s2.0-S2352711024003753/main.pdf?X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLWVhc3QtMSJHMEUCIQDCiRL6De1klS18me5XR6L4lvS54xXwLv29SAPPFVZfQgIgIrNZ7lj%2FGiEuAOXav5RUopeUeghtZky9Mf2Hu7tQ4mgqsgUITxAFGgwwNTkwMDM1NDY4NjUiDOn%2F5RUrrlrX2%2F14%2BSqPBUZGDy5MCU%2Bak%2Bwiz0RdhEfaPZ3kRoI69Wdqxsy8IiMiinyMspaXXR89zUOTdxl6RUorkBkyWFuovNPpP%2FPhnsK35gBe31%2BaN6cJdqcce7kMivbz6HYHI9%2FFpQ3vGc3BovI%2BdgfnXffEz57ke2zcoOxNVFDeXOUgYX%2FqWJhChdZvZ173jEwY3kS1oWsoWIqGWRwA8yTs1ZDTm3NEObitMM7bM0kvLYOrXC623nWb8u15bJ4qBDqyjAOX9BEW1YD0Vnzf3J349Y842Cf4VUtreP%2BMT2DBJdnebLkBRb03HbbKHyZNiPHuolLZWfINbVxeSmehk1RQB3FrI57M5X3ps5B1T9mdmniA1kHqgYpMMoun1svv5eSmhfHzareuEIECbD1%2B%2FyIt9Iobir9be3vFky9eLoFSnC14JW0F2kWBumU0aqV26FyzchESlKnotf6QnixgfPAZidXNQTWwoYr3qjyzmDJ2gsA%2F1tf22zhsU6vi52c8cjd3SZiohsAORGybb6Bg8gxwtfuAyaJ7vVwOuPeCPb7qrnG1fAQAA8cGIKoLH%2BFQIkXD%2BH2DcgYlWZVZWkivMhsH2YjpOTxe6C5oSbWvODMBCs4XRAO63HHBBgP7Ai2wOD%2FL91yIpGlo9YLh%2FFBVgchLg41fR0l%2FbG9v8xdVZPkhoCK6Dkg9IS3yDoxKWFimFw8aUFAWJf7Rpn0mjZjfcyHFFQoUCRV5ORRgMCZzGoONm4ltSf4yEO0PIGw%2BwBu1dH6ACyX%2BGpUn3ewSGg3JLIUPAHqgM9KrTFH0%2BleIH0ebErFu7s8%2Bzt3xz11cLw3qji8wLY2YC7steTSfm9RdUUNNTEdLwqtNtn9jh4nTwmFPZmttXh1%2F3vkn6Vkw7bn%2B1AY6sQEC8ABw8PRmDXgZJYWopZ6AT9p0n8L%2F%2BWCjioR8kwGtfY6tWdyuBGSIGvH9BAzj3BQw23HXgjAzn6YSmFduNusQ6%2Brq8sqG8xn8ZTkjMUD5hQpaif6qWUSqOVZlxsjjbHs4cXRdCeS%2BSx1YR2EytONy5euOc47FY40XBRW4ToOpgAjSr2wyCttPOSe6CQX8oGKxCjhN5htMCSo%2FhMQixjivri8n9unPGo%2BkZENZnAg%2B1nc%3D&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20260908T071307Z&X-Amz-SignedHeaders=host&X-Amz-Expires=300&X-Amz-Credential=ASIAQ3PHCVTY6SAH5SVW%2F20260908%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=2ea8fb0fa303b3319d42391dfc1505a8cff19fc34703b234bc6525c59fa34387&hash=df4256f8e675a7490e15a459fb3a72e80ae827622745cfef816ab6d9c298a28b&host=68042c943591013ac2b2430a89b270f6af2c76d8dfd086a07176afe7c76c2c61&pii=S2352711024003753&tid=spdf-8d2e214d-7010-4b4e-894c-1bba08c6049c&sid=599f468a4e2de94af42bd7138492ae92fbd6gxrqb&type=client&tsoh=d3d3LnNjaWVuY2VkaXJlY3QuY29t&rh=d3d3LnNjaWVuY2VkaXJlY3QuY29t&ua=0103025753025659515a&rr=a37c26180f4cf48c&cc=de)
	+ papers by which we convert the models into SCL language to prevent offline inference.

6.  [Ti-iLSTM](https://github.com/MJ636UoW/TinyML_based_increamental_LSTM)
	+ Trains the LSTM model on swat dataset.

----
BUILDING DATASET: *(what normal process generally looks like)*
1. Write the SCL/FBD program controlling the process under normal conditions.
2. Build the physical process model in SIMIT.
3. Run the PLC program on S7-PLCSIM Advanced and couple it to the SIMIT process model.
4. Use the PLCSIM Advanced API (or OPC UA) to subscribe to the tags you care about and write them to CSV/Parquet with timestamps.
5. Record normal operation cycles as your baseline.
6. Inject anomalies (for process anomalies), running each anomaly type across multiple episodes — and log the exact start/end timestamp of each injected episode as it happens.
7. Structure and label the dataset properly (normal vs. anomalous segments, anomaly type per episode, data dictionary).




Q: Are we using SIMIT software for simulating the real hardware?



- [ ] Need to check [swat dataset](https://github.com/jonathanwvd/awesome-industrial-datasets/blob/master/markdown/secure_water_treatment__swat__dataset.md) and the dataset from the paper 3.




---
 DATASETS.
1. SWAT
2. WADI
3. PAPER_3

--- 
- [ ] GO through paper_3





----
SIM MEETING:
1. They need to get the software working next week on Wednesday. *as they need to figure out why it is not working with my PC*
2. They are not sure if we can log the live data, but are mentioned maybe the can log windowed data into SQLDB, 
3. They also mentioned that the data maybe extracted from the PLCSIM, as the API form the FE.screen sim can be too slow.
4. They mentioned that they need to talk to fe.screen sim software guys in order to know if they can extract the data from the fe.screen-sim.
5. They showed me a simulation on their own computer and connected with the PLCSIM, and we changed the data values of actuators from True to False.
6. we discussed about the Paper and they told me to show this to andre and have discussion with us regarding the paper.
