Transfer learning is a **deep learning** approach in which a model that has been trained for one task is used as a **starting point** that performs a *similar task*
- We can take a **pre-existing** neural net, modify it slightly, then retrain for a more **specific task**

![[Pasted image 20260119160352.png]]

Fine-tuning a network with TL is usually **faster** and **easier** than training a network from scratch
- Possible to achieve **higher model accuracy** in a **shorter time**

**Benefits**
- Enables us to train models with **less labelled data** by reusing models that have been pre-trained on large data.
- It can **reduce training time** and **computing resources**, since weights are **not learned from scratch**
- We can take **advantages of good model architectures** developed by the deep learning community (AlexNet, GoogLeNet, ResNet)

---

Transfer learning workflow

1. **Load pre-trained network**
	- Two parts:
		- Early layers, which have learned the low-level features (edges)
		- Final layers that have learned task-specific features
2. **Replace final layers**
	- New layers are learned to specific features of the data-set
3. **Train the network**


![[Pasted image 20260119163152.png]]

