

---


#lrp 

<span style="color: var(--color-red)">Q.what is Cascading accuracy degradation?</span>
	Pruning doesn't effect all the classes equally, when the accuracy  of certain class drops the lrp score of that class drops as well causing the neurons related to that class fire less and eventually getting pruned which leads to more accuracy drop.


- This paper adds accuracy aware control layer on top of LRP Pruning.
- measures harmonic mean of class wise accuracy (A) after each pruning step and applies two mechanisms when accuracy drops:
	1. change in pruning rate.
	2. change in pruning order.


ALGORITHM/METHODOLOGY:

1. Run the small amount of images limited to target class through the model.
2. compute the relevance of filter via LRP.
3. Rank filters by total relevance, remove bottom 5%.
4. recalculate relevance on pruned model and repeat it.

<span style="color: var(--color-green)">they perform normal LRP and get the relevance of the filters, then prune the bottom filters and calculate harmonic mean of class wise accuracy. if this is dropped compared to before pruning they halve the pruning rate and  retry the same and repeat this process until only one filter if left to prune at a time. if the accuracy still drops they change the strategy and change the relevance of  lowest filter to more and dont prune it for a while and start pruning the next lowest filter repeating this each time for 10 trials and selecting which ever gave the best accuracy.</span>

# Results

DPX-SD consistently achieved **higher accuracy retention at every pruning rate**, **prevented single-class collapse**, worked well even with fewer reference images, generalized to non-LRP criteria and a real medical dataset — at the cost of extra (but offline, one-time) computation



# Summary


Dynamic pruning with the class imbalance in mind.

we can try this on transformer.

we can also check the class imbalance caused by accuracy.

+ could be interesting if we are doing LRP



---
# revision


---

Cascading effect done by LRP pruning.

Below are the effects caused by the LRP pruning.
1. Pruning is never uniform across all classes.
2. Lower accuracy on certain class means low lrp score for that output node which  eventually means more puning.

3. when we are calculating top-1 percent accuracy most of the classes stay unaffected while some classes have almost zero accuracy.

DPX-SD intervention.:
	Add monitor and two control mechanism.
	1. get harmonic mean for calculating accuracy which is more effective than arithmetic mean which drags the pruning ratio low.
	2. drag the pruning ratio to half and then again until we are removing single filter.
	3. if the single filter can not avoid drop in harmonic mean then protect the lowest score and start dropping second lowest or third lowest scores accordingly. try this for 10 trails and pick which ever has the lowest drop in accuracy.
EXPERIMENTATION:
	All of this was  done on four CNN models
		1.vgg 16 with and without batchnorm
		2. resnet 18 and resnet 5o


---- 
No, code available, asked them for code waiting for response
