
---

# Instructions from Professor:

1. Use different model for purning.
	1. SWIN, DEIT, CNN and different architectures of VIT(small, medium and large)
2. Do transfer learning and observe the effects.
3. work on report.
	1. add conclusion section seperatly.
	2. work on discussion and related work, *we should he able to find a gap between the two and fill that gap.* 
	3. compare the results with the previous work.


---
# TODO:


+ understand the repo 

[[3.3 learnings- Pruning + XAI]]

---


GETTING THE SCORE FOR VIT TINY.


WORK:


SCORES
- [ ] tiny VIT.
	- [x] GMAR++
	- [x] GMAR
	- [ ] chefar CAM
	- [ ] LRP
	- [ ] MAG
	- [ ] Legrad


- [ ] small VIT.
	- [x] GMAR++
	- [ ] GMAR
	- [ ] chefar CAM
	- [ ] LRP
	- [ ] MAG
	- [ ] Legrad


chat: https://chatgpt.com/c/6a7394de-eea4-83eb-a72a-6fe8d20bc342

---
IMPORTANT:
	+ we can make a confusion matrix  where we can show which method agreees with another method in scoring  the head.


TODO:
1. Doing pruning on vit using gmar++
	- [-] i alrady have the score for GMAR++ so i need to do pruning on vit  **small and tiny** and then compare all the pruning scores.
	- [ ] Get the basline score for tiny and small transformer

2. Prunning CNN:
	- [x] Need to know which cnn to use for prunning.
		- [ ] use torchvision to load resnet models.



---
CNN ARCHITECTURE:
	1. 