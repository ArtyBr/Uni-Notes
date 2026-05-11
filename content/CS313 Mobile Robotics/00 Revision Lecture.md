More applied - less about memorising things and more about how you apply it
- Recursive bayes is the backbone of the module
- Series of 5 steps
	- Apply bayes filter
	- Apply sensor independence
	- Apply markov
	- Do some total probability

How to set it up as a recursive bayes inference problem?
- A state given every control I've done and every measurement, follow the 5 steps through to a recursive bayes

Look at CS313 Exam Style Practice questions - more relevant than past papers
- Turned up the dial on applied questions

Answer 4 of 6
- Questions generally themed
	1. Sensors
	2. Vision
	3. Discrete Filter
	4. Linear Gaussian Filters (maybe combined with non-linear)
	5. Non-linear Gaussian Filters
	6. Particle filters, Mapping, Planning and Control

- **Sensors**
	- Sensor classificaton
		- What they measure
		- Internal/External
		- Passive or Active
			- Reading something or putting something in then reading that back
		- Characteristics of sensors
		- Commonly used sensors
		- Range sensors
			- ToF
				- Discrete signals
			- Phase shift
				- Continuous monitoring of distance
			- Triangulation
		- Trilateration (beacons, GPS)
		- Structure from Stereo
- **Vision**
	- Convolusion with Discrete Images
		- Masks/Kernels passed over images to extract features
			- Smoothing
			- Noise reduction
			- Box filter and Gaussian filter
			- Sharpening, Salt & Pepper noise
			- Line detection
		- Canny Edge Detector
			- Describe the stages
				- Gaussian smoothing, 
					- taking the gradient
					- 2 step or single step
				- Gradient magnitude
				- Gradient direction
					- Prewitt and Sobel filters
				- Non-maximum suppression
				- Hysteresis thresholding
		 - Corner detection (points rather than lines)
			 - High intensity of variation in both directions rather than just one direction (like canny)
			- Work your way down to the structure tensor by rewriting it as a matrix equation
			- Remember the fact that you're shifting the window, looking at sum of squared differences over u and v
			- Use first order taylor expansion to work through that
			- Probing understanding of $M$ - what does it tell you?
			- Look at the two eigenvalues
				- One large eigenvalue - edge
				- Two large eigenvalue - corner
				- None - flat region
			- Don't have scale invariance - depends on size of image and size of window if you can correctly identify corners/edges
		- SIFT gives scale invariance, how is this given?
			- Describe each step (maybe)

- **State estimation**
	- Recursive bayes formulation
	- Two *data streams*
		- Sensor measurement $z_t$
		- Control data $u_{t}$
	- Estimate the **state** of the system *given* the observations $z$ and controls $u$
		- $p(x_{t}|z_{1:t},u_{1:t})$
	- Goal: Move to basing state estimation at time $t$ on state estimation at time $t-1$
	- Start with all of our current belief, then apply a bayes filter
	- Measurement is only dependent on current state (everything before doesn't matter)
	- Then use total probability law
		- Probability of new state - is dependent on the probabilities of all the old states
	- Discrete filters
