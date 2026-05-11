Can be done with 360-viewing sensors, e.g. LiDAY or a Ring of Sonars.
- Place recognition can be done by taking measurements at different locations, and learning their characteristics
- Essentially **building a database** of the **chosen places**: a map
- Can only recognise **learned places**

![[Pasted image 20260224151305.png]]

Firstly, the robot needs to be placed in each **target location**
- To learn its appearance (in the view of the sensor used)
- The raw measurements are stored to describe the place: a descriptor, or signature

Afterwards, when the robot is **placed** in one of the **locations**
- It takes a measurement (or set of measurements)
- Checks which saved signature matches **best** to the **new measurements**
- Decides its **current location** - which place it is in

![[Pasted image 20260224151353.png]]

Next, we **compare** the observed measurement with **saved signatures**:
- Can be done via  a **correlation test**, measuring the **sum of squared differences**
- Differences $D_{k}$ between the measurement (histogram) $H_{m}(i)$ and saved signature (histogram) $H_{k}(i)$:
$$D_{k}=\sum\limits_{i}(H_{m}(i)-H_{k}(i))^{2}$$

Then, we **retrieve** the **most likely** place
- The saved location with **lowest $D_{k}$
- If $D_{k}$ is above a **threshold** - the robot is in **none** of the known places

What about the **orientation** of the robot?
- If the test histogram and one of the saved signatures can agree with each other by only a shift, then the robot is in that location, but rotated
- The amount of shift to get the best arrangement is a measurement of the robot rotation

![[Pasted image 20260224151833.png]]

So, we can use **rotation invariant matching**
- To save the computational cost of looking at every rotation angle, we can build a signature which is **invariant** to robot rotation
- One way is to use the **histogram of occurrences**
- - Matching test can be carried out directly, and orientation estimation only needs to be done for the found location

![[Pasted image 20260224151946.png]]

---
**Visual** place recognition

'Old school' methods often rely on **sparse visual features**
- Feature detection -> Feature description -> Feature matching
- Pose estimation (if necessary)

![[Pasted image 20260224152049.png]]

Represent places as a **visual bag-of-words**
- Use a set of images to build a visual *vocabulary*
- Detect and extract visual features (using SIFT/SURF)

![[Pasted image 20260224152138.png]]

A **visual word** (in the vocabulary) **represents** the *cluster* of a certain appearance in the real-world
- A **place** is characterised by its **visual Bag of Words**.
	- Similar to the idea of histogram of occurrences
![[Pasted image 20260224152407.png]]

Place recognition is achieved by **inference** on **probabilistic models**
- For each location, construct a **generative model**: What visual words are **likely** to be seen at this location?
- Compare the Bag of Words of the query image with the models of the locations

![[Pasted image 20260224152454.png]]

---

'New school' methods

*Replace* the pipeline with **Convolutional Neural Networks** 
- Use **deep** **networks** for **place recognition** and **pose regression**
- Estimate 6DoF robot poses given observed images

![[Pasted image 20260224152600.png]]

**Simultaneous Localisation and Mapping** (SLAM)

When we need SLAM:
- a robot must be **truly autonomous** (no human input)
- When **little** or nothing is known in **advance** about the **environment**
- WHen we **can't place** or use infrastucture such as artificial beacons or GPS
- And when the robot actually  needs to know where it is

In SLAM:
- We **build** a **map** **incrementally**
- And localise the robot with respect to that map as it grows and is gradually refined
- Iterative process
- Joint process of estimating the map and the pose

In SLAM, we have a few components:

![[Pasted image 20260224153244.png]]

The **front-end** abstracts sensor data into **models** that are amenable for **estimation**
- Often sensor-dependent:  extracts relevant features from the particular type of sensor data

The **backend-end** performs inference on the abstracted data produced by the front-end
- Essentially an **optimiser**
- Compute the **maximum a posteriori** (MAP) estimation of the variable $X$, given a set of measurements $Z:X^{*}=argmax(P(X_|Z))=argmax(P(Z|X)P(X))$
- $X$: Robot Poses + MAP (e.g. position of landmarks)
- $Z$: Sensor measurements

Most SLAM front-ends **rely** on **scene features**
- Laser/Sonar sensors - Wall segments, planes, corners
- Vision sensors - Salient point features, lines, textured surfaces

Good features *should*:
- Be **distinctive** and **easily recognisable** from different **viewpoints**
- Enable **reliable matching** (aka *correspondence* or *data association*)

![[Pasted image 20260224153743.png]]

We **represent** the *Join uncertainty*:

![[Pasted image 20260224154034.png]]

So what about with a **monocular camera**?

![[Pasted image 20260224154201.png]]

Further, we can do '**Metric** SLAM'

![[Pasted image 20260224154219.png]]

![[Pasted image 20260224154622.png]]

![[Pasted image 20260224154628.png]]

![[Pasted image 20260224154636.png]]