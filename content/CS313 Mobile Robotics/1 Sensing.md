**Fundamental questions**
- *Where am I?*
- *Where am I going?*
- *How should I get there*

**Dealing with Uncertainty**
- We need to be able to *accommodate* the enormous **uncertainty** that **exists in the physical world**
- Sensors are *limited* in what they can perceive. 
- Robot actuation involves **motors** that are to some extent **unpredictable**.
- There is also **uncertainty** in the robot's **software**. All internal models are **abstractions** of the real world and such they only partially model the underlying physical processes of the robot and environment.
- Robots are **real-time** systems, limitation the amount of computation that can be carried out.

**Probabilistic Methods**
- Probabilistic methods represent information using **probability distributions** over a whole space of guesses.
- Typically more robust in the face of:
	- **sensor limitations**
	- **sensor noise**
	- **environment dynamics**, etc.
- However, two most frequently cited **limitations** are **computational complexity** and a **need to approximate**

![[Pasted image 20260113151111.png]]

---

Sensors are used to measure **parameters** of:
- The **robot**
- and the **environment**
Increasing the **sensor capability** could:
- Increase **flexibility**
- Save **time**
- Increase **productivity**
- Increase **safety**

**![[Pasted image 20260113152430.png]]

Sensors never give **perfect information**
- Sensors only see **part of the world**
- Measurements are **affected by noise**
- The robot is **moving** and the world may be too
- The robot's **internal model** is always an **approximation**

A sensor gives **raw data**, for example:
- A *voltage*
- A *distance reading*
- A *pixel value*

On its own, this data has no meaning. To understand the world, robots combine:
- Raw sensor data
- Models
- Context

---

**Sensor classification**
Robot sensors may be **classifies** based on characteristics:

![[Pasted image 20260113152657.png]]

![[Pasted image 20260113153047.png]]

![[Pasted image 20260113153058.png]]

---

Some things we might consider when choosing a sensor could be:
- **Response time/Speed of operation**
	- Rate at which measurement is returned
	- The number of measurements per second is expressed at Hz (Hertz)
- **Accuracy**
	- Difference between real and estimated output
	- Accuracy = $1-(\dfrac{\text{measured value}-\text{true value}}{\text{true value}})$
	- Error = $\text{measured value} - \text{true value}$
	- Two types of error:
		- **Systematic**
			- Predictable and caused by factors that can be modelled
			- Consistent and repeatable
		- **Random**
			- Not predictable but can be treated in a probabilistic way
			- Varies from measurement to measurement
- **Repeatability** (Precision)
	- Difference between successive measurements of the same parameter
	- **Precision** - often confused with accuracy
	- Related to **reproducibility** of results
	- Precision = $\frac{\text{range of values}}{\sigma}$
	- ![[Pasted image 20260113154102.png]]
- **Resolution**
	- Smallest possible increment
	- Minimum range between 2 values that can be detected
- **Linearity**
	- Variation of output signal as a function of input signal
	- A plot of a linear input output would be a straight line
	- ![[Pasted image 20260113154158.png]]
- **Sensitivity**
	- How much target input changes the output signal
	- A good sensor should be:
		- **sensitive** to the **measured property**
		- **insensitive** to **any other property**
	- Ratio of output change to input change
	- **Cross sensitivity** is a measure of sensitivities orthogonal to the target parameter
		- e.g. a magnetic sensor can be very sensitive to magnetic north but also sensitive to ferrous building materials such as iron and steel – the sensor may be useless in some indoor environments
- **Measurement range**
	- Difference between minimum and maximum measurement
- **Error rate**
- **Robustness**
	- Noise/disturbance tolerance
- **Power, weight, size**
- **Cost**

---

**Calibration**
Usually required to **calibrate** a sensor before it can be used for reliable measurement of a quantity

Calibration plays a major role in signal conversion and may involve one or more of the following:
- Finding **limits**
- Fixing/storing **reference points**
- **Linearisation**

It looks a sensor's **raw output** to a **real-world** quantity. Otherwise, a sensor reading is just a number.

--- 

**Errors**
Systematic vs Random error, and their *blurring*

- **Systematic**
	- Predictable and caused by factors that can be modelled
	- Consistent and repeatable
	- *Caused by* mounting angle or calibration bias
- **Random**
	- Not predictable but can be treated in a probabilistic way
	- Varies from measurement to measurement
	- *Caused by* noise, reflections, timing jitter

![[Pasted image 20260113155634.png]]

So while the robot is stationary, the error looks like it is a **systematic** error
- However, when the robot starts *moving*, the error now looks like it's **random** since the angle is *changing* and the reflection failures are *unpredictable*

![[Pasted image 20260113155644.png]]

A: The fact that the walls are close to the robot create specular reflections and increase risk of error/failure

![[Pasted image 20260113155651.png]]

---

**Wheel/Motor encoders**

They measure wheel **rotation** in order to **estimate**:
- Wheel *speed*
- *Distance* travelled
- Wheel motion can be integrated to estimate robot *position* - **Odometry**
- Optical encoders are proprioceptive sensors
- Operate best in the robot's local reference frame

Key properties:
- Simple
- Widely available
- Low cost
- Require line-of-sight between emitter and detector

![[Pasted image 20260119141628.png]]

How they **work**:
- A light emitter **shines** onto a patterned disc
- A detector **measures reflected light**
- Output is a **square wave**
- Each rising edge can be used to infer how much the disc has roate

Example:
- 4 pulses = 1 wheel roation
- Detect 20 rising edges -> 20 pulses
- Wheel diameter = 10cm
- How far has the wheel travelled?
	- ![[Pasted image 20260119141816.png]]
- Because of this - **resolution is important** - reduces uncertainty between the extremes.
- So in reality, you would have much smaller sections

A **regular encoder** counts the number of transitions, but cannot tell the **direction**of motion

In robotics it is common to use a **quadrature encoder** - it uses two sensors in **quadrature-phase shift**. 
- The **ordering** of which wave produces a **rising edge first** tells you the **direction of motion**. 
- Additionally, the **resolution** is **4 times bigger**
- A single slot in the outer track generates a reference pulse per revolution

![[Pasted image 20260119142152.png]]

Wheel encoders measure wheel **rotation**, *not* **motion** through the world
- They **assume** perfect **contact** between wheel and ground
- They **cannot detect slip, skidding or wheel lift**
- **Errors** **accumulate** over time
- Odometry is locally accurate, but globally unreliable

Excellent for short-term control, but poor for long-term localisation

---

**Heading sensors**
Estimate **orientation** relative to a **reference**
- Orientation + Velocity -> Dead reckoning
- Small orientation errors lead to large position errors!
- Can be:
	- **Proprioceptive** (gyroscopes, accelerometers)
	- **Exteroceptive** (compass, inclinometer)

**Gyroscopes**
What it measures:
- **Rotation rate**
	- Measured in degrees or radians
	- **rotation** *not* absolute **direction**

Strengths:
- Works in the dark
- Not affected by wheel slip
- Very fast readings (high bandwidth)


**Inclinometer**
Measure **tilt** relative to **gravity**
Used when robots must cope with:
- **ramps**
- **bumps**
- **uneven terrain**
Two types of inclinometers are commonly found in practice:
- **Mercury Switch**
	- ![[Pasted image 20260119142635.png]]
	- Output is **binary** so to gain information about degree of tilt requires several sensors fixed at different orientation.
- **Electrolyte sensor**
	- ![[Pasted image 20260119142624.png]]
	- Two or more electrodes are immersed in a conductive fluid. Conduction between them is a function of the orientation of the sensor relative to gravity.
	- The sensor produces an **analogue signal** proportional to the degree of tilt

---

**Accelerometers**

![[Pasted image 20260119142817.png]]


**MEMS (micro-electromechanical system) accelerometers**

![[Pasted image 20260119142958.png]]

**Inertial Measurement Unit (IMU)**

![[Pasted image 20260119143043.png]]

---

**Light sensors**

![[Pasted image 20260119143544.png]]

---

**Temperature Sensors**

![[Pasted image 20260119143558.png]]

---

**Touch sensors**

Two categories:
- **Contact sensors**
	- Send a signal when a part of the robot is in contact with an object

![[Pasted image 20260119143757.png]]

- **Tactile sensors**
	- Sense more than just presence by measuring the roughness of an object's surface - try to detect the object shape and type

![[Pasted image 20260119143807.png]]


---

**PIR sensors** (Passive infrared)

![[Pasted image 20260119144033.png]]

**Active infrared**

![[Pasted image 20260119144052.png]]

---

**Range sensors**

![[Pasted image 20260119144215.png]]

**Range sensors - Time of flight ranging**

![[Pasted image 20260119144234.png]]

![[Pasted image 20260119144241.png]]

**Ultrasonic sensors**

![[Pasted image 20260119144254.png]]

![[Pasted image 20260119144302.png]]

![[Pasted image 20260119144311.png]]

**Radar**

![[Pasted image 20260119144424.png]]

**Laser Range Finders** (LIDAR)

![[Pasted image 20260120150702.png]]

![[Pasted image 20260120150719.png]]

![[Pasted image 20260120150729.png]]

![[Pasted image 20260120150739.png]]

![[Pasted image 20260120150915.png]]

![[Pasted image 20260120151443.png]]

Using 3D lidar to get 3D estimation
- Using robot's motion
- Take the horizontal scan and then moving, taking a scan, etc.
- Doing this allows you to get a 3D representation of the space (using robot's motion to get 3rd dimension)

---

**Range sensors - Phase Shift**

![[Pasted image 20260120151621.png]]

![[Pasted image 20260120151632.png]]

---

**Range sensors - Triangulation**

![[Pasted image 20260120152233.png]]

---

Question: An autonomous unmanned security car is to patrol the University campus. What type of sensors would it need for safe and uninterrupted operation?

---

**Beacons and GPS**

![[Pasted image 20260120154032.png]]

![[Pasted image 20260120154044.png]]

![[Pasted image 20260120154052.png]]

![[Pasted image 20260120154059.png]]

**GPS**

![[Pasted image 20260120154926.png]]

Why do we need 4 satellites?

![[Pasted image 20260120154941.png]]

![[Pasted image 20260120154951.png]]

**Differential GPS**

![[Pasted image 20260120155155.png]]

3