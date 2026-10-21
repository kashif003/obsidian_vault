

---
Approach

1. train a AE on normal datasets and then test them on the anomaly dataset.
	* Train an ae model on normal data and make a NN small model to predict the anomaly


---

Data analysis:
+ number of datasets (11)
		+ normal = 1, anomaly = 10
		+ out of 10 anomaly dataset:
			+ 3 have sensor fault
			+ 3 have actuator fault
			+ 4 have combined fault (3=(0,1,4), 1= (0,1,2,3))
+ total columns 31 + one label column "anomaly"


columns that can be dropped
+ event_eq: just like index
+ ts_plc: actual time stamp of recoding data (delta_timestapms can be used).
+ df["gemma"] !=1 data points should be dropüed as we are training ae on normal conditions.
+ fault maybe as it shows if we have a fault condition actively present in the system in normal dataset we have value =0 but in other dataset we hace both values 0 and 1
+ FSx : indicator showing if sensor fault is present on specific sensor
+ TFSx: show if the exact type of sensor fault
+ FCVx: indicator showing if particular acutator fault is present
+ TFCVx: indicator showing what kind of actuator fault is present



---

update: 08.10.2026


- [ ] check co relations between the matrix
- [ ] update the code
- [ ] maybe try to implement the methodology used in CARLA.

----
checking the co-relation between the features
+  while checking the correlation matrix we can see that FVCX, TFCVX has a strong co relation with the anomaly column 
we can drop these but we should keep the other columns.
+ scan id and part id also has strong co-relation


---


work on [[Master Thesis/literature review/DAYPSCI|DAYPSCI]] files.

- [I] problems current clusters are not proper they look they are in a series.
need to find which column is responsible for that.


**Silhoute score:**
-1: worst
0 : neutral
+1 : best

+ "scan_id", "event_id", "part_id", "event_order"
	  anyone or all of the following broke the linear nature of the clusters.
+ still unable to seperate the anomaly with normal data need to check if we can do it without over-fitting?
work on the feature selection:

which columns need to be dropped:




- [x] Need to get the total number of anomaly points and normal points 
	- now we have right amount of datappoint in CM.
	normal = shape: 710, 32, normal: 710, anomaly: 0
	pn_combined_T1_batch11.csv = shape: 786, 32, normal: 370, anomaly: 416
	pn_sensor_T1_batch02.csv = shape: 798, 32, normal: 512, anomaly: 286
	pn_sensor_T2_batch03.csv = shape: 844, 32, normal: 490, anomaly: 354
	pn_sensor_T3_batch04.csv = shape: 710, 32, normal: 516, anomaly: 194
	pn_sensor_T4_batch05.csv = shape: 1190, 32, normal: 481, anomaly: 709
	pn_sensor_T5_batch06.csv = shape: 762, 32, normal: 446, anomaly: 316
	pn_sensor_T6_batch07.csv = shape: 718, 32, normal: 432, anomaly: 286
	pn_valve_T1_batch08.csv = shape: 770, 32, normal: 316, anomaly: 454
	pn_valve_T2_batch09.csv = shape: 812, 32, normal: 280, anomaly: 532
	pn_valve_T3_batch10.csv = shape: 710, 32, normal: 374, anomaly: 336

total = normal: 5167, anomaly: 2933, total points: 8100


 - [x] Need to fix the problems with  CM 
	 - problem was we were adding raw score of other classes i.e 3,5,6 but needed to convert them first into the 0,1 and then add up
	+ now we have 2933 anomaly data points which is the correct number. 
 
---
problems: why the result keeps changing when i load the same model  and same threshold:
	+ because NN model was laoding random weights, now we are loading proper weights which are saved.

---

Brief summary:

+ training model on training data.
+ should we tune the threshold on testing data or training data?
	+ training data: whole pipeline has a boundary after which  model should classify the data as anomolous.*(better approach)*
	+ testing data: the boundary is varying so we are setting the boundary according to test data.

+ we have data, we need to set the boundary according to training data but training should be dont such that the boundary is close and we have normal data samples close to each other.
- [?] How to make the anomaly datapoints far away from normal data points?

---

Changing the approach:

tried the pipeline on training dataset got 100 percent result.

try dbscan for clustering and dense clusters are normal and less dense clusters are anomaly.

----


count of -1 = 451
count of 1 = 259



currently i have a cluster which is not that close need to bring clustering points more together.




