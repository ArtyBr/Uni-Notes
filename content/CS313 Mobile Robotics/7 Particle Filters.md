Kalman filters rely on the assumption that we only guess that we're in **one place**or in **one state** at a time
- Single hypothesis assumption

However, this breaks down where we can have multiple identical possible locations or states with the same sensor reading:

![[Pasted image 20260217102747.png]]

Instead, we can use a **particle filter**

We still want to use a Bayes filter, but instead of using a mean and covariance, we represent the distribution using a **collection of samples**, often called particles
- Each particle corresponds to a **possible state of the system** 
- By **maintaining** **many** of them at once, we can keep **multiple hypotheses** alive and let them **compete** as the system evolves and new data arrives
- The **trade-off** is **computational cost** 
	- Representing and updating a **full distribution** with **many samples** is **more expensive** than working with a **compact parametric form**

One particle = one hypothesis
Many particles - a **distribution**
- We can have either uniform or weighted particles

![[Pasted image 20260217103131.png]]

For a uniform distribution we can use a random number generator
- There is a very simple way to generate random numbers that approximate a Gaussian distribution:

![[Pasted image 20260217103339.png]]

But what about for other distributions?

- **Rejection** sampling
	- Goal is to generate samples from a target distribution when it's awkward to sample from directly
	- ![[Pasted image 20260217103438.png]]
	- However, this isn't very good because we're throwing away computation by first generating points and then throwing some away
- **Important** sampling
	- ![[Pasted image 20260217103609.png]]
So that is basically what the particle filter algorithm does - **importance** sampling, **recursively**

1. **Sample** the particles using the **proposal distribution**
	- We can **define** the **proposal distribution**
	- ![[Pasted image 20260217103922.png]]
2. **Compute** the **importance weights**
	- **Compensate** for the **difference** through the application of a **weight**
	- ![[Pasted image 20260217103932.png]]
3. **Resampling**
	- **Draw** samples from weighted sample set **with replacement**

![[Pasted image 20260217104043.png]]

We have a Recursive Bayes Filter:
- Prediction - draw from the proposal
- Correction - weight by the difference between the target and proposal

**Non-parametric** approach - can deal with non gaussians, better able to cope with non-linear models
- Represents the distribution by samples
- Need a proposal distribution in order to provide the samples

**Localisation**:
1. Each particle is a **pose hypothesis**
2. Proposal distribution is the **motion model**
	- ![[Pasted image 20260217105602.png]]
![[Pasted image 20260217105621.png]]

You have a set of particles that can approximate the predicted state distribution
- How would you incorporate the measurement $z_t$ and generate a set of particles that can approximate the posterior?

Should all particles be treated equally?
- Some agree more with the sensor measurement $z_t$
- Others might represent some nonsense

so,:
3. **Correction** via the observation model

![[Pasted image 20260217105754.png]]

So, we **use the sensor observations** in order to **weight the particles**

We now use **resampling**

![[Pasted image 20260217105908.png]]

![[Pasted image 20260217105926.png]]

One of the problems with this is **particle depletion**, since we're getting rid of some particles representing the hypothesis of the system

![[Pasted image 20260217110046.png]]

There are other ways of doing **resampling**:
 **Low variance resampling**

![[Pasted image 20260217110224.png]]

All in one formula:

![[Pasted image 20260217110335.png]]

![[Pasted image 20260217110407.png]]

![[Pasted image 20260217110416.png]]

So, overall this:
- Handles non-gaussian distributions
- Works well in low dimensional spaces
- Can easily integrate multiple sensing modality
- Robust
- Easy to implement

However:
- Problematic in high-dimensional state spaces
- Particle depletion
- Given the random sampling necessary for a particle filter, it is unlikely that two particles filters, even with identical intiail states and observation over time would follow the same trajectory (non-deterministic)
- Really good observation models result in bad particle filters

