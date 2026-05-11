In general, we use a **transformation** to transform the image into the **frequency space** (domain).
- "Encoder-Decoder" structure

Three such transforms covered:
- 2D Discrete Fourier Transform (DFT)
- 2D Discrete Cosine Transform (DCT) - particularly useful for *compression*
- 2D Discrete Wavelet Transform (DWT)

---

Recap: Vector space

![[Pasted image 20260127104742.png]]

Orthogonal/Orthonormal vectors

![[Pasted image 20260127104820.png]]

Orthonormal matrix

![[Pasted image 20260127104836.png]]

This property of the orthonormal matrix, that $WW^T=I$, is very useful for certain applications, including PCA.

Another example, of Eigenfaces:

![[Pasted image 20260130121452.png]]

Finally, we have another example with a 1D discrete function representation:

![[Pasted image 20260130121739.png]]

---

So, we have the idea that an **input** signal can be treated as a **sum of basis vectors** 

![[Pasted image 20260130122225.png]]

We should be able to calculate the **coefficients** of each of the basic vectors in order to get the final function.

![[Pasted image 20260130122332.png]]

The basic idea of the **fourier transform** is that **any functinon** can be expressed in terms of a bunch of **harmonic functions** with varying frequency

![[Pasted image 20260130122409.png]]

The fact we focus on **harmonic functions** means we focus just on some specific frequencies when trying to re-construct the original function.

So, we get a 1D **Discrete Fourier Transform (DFT)**
- We plug the terms into the equation to get the expanded version of it:

![[Pasted image 20260130122725.png]]

We would like to make this form more **compact**.
- We can use a **matrix** to represent it

![[Pasted image 20260130122959.png]]

We can notice that the matrix is **symmetric**

![[Pasted image 20260130123220.png]]

![[Pasted image 20260130123435.png]]



---

**Forward and Inverse DFT**

![[Pasted image 20260130123624.png]]

![[Pasted image 20260130123630.png]]

![[Pasted image 20260130124309.png]]

---

2D Fourier basis

In 2D functions, frequency can change in **two directions**
- Both **horizontal** and **vertical** directions

![[Pasted image 20260130124711.png]]

These can be combined to produce diagonal frequency changes

![[Pasted image 20260130124730.png]]

An image is a function of **two spatial variables**, $f(x,y)$
- An image with size $M\times N$ can be epxressed as a **weighted superposition** of 2d fourier basis of **varying frequencies.**

![[Pasted image 20260130125011.png]]

So, the equations change to being in the following form(s):

![[Pasted image 20260130125242.png]]

So, with PCA we can show how the Eigenfaces look as a **combination** of a collection of **basis** faces

![[Pasted image 20260130125316.png]]

---

**2D Fourier Basis**

The **definition** of the 2D DFT indicates that the frequency coordinates run from **the origin** at the **top left corner**
- The DC component is the **Direct Current** component - the 0Hz component, capturing the **lowest frequency** information from the image. This is essentially an **average intensity** of the pixels in the image.
- AC components (Alternating current) are the **high frequency** components - they represent the details of the image, such as corners, lines etc.
- Named as such because Direct current is constant - it doesn't change, whereas AC alternates and oscillates over time
	- DC is the 'volume' (sets the base amount of light, overall brightness of the image)
	- AC is the 'melody' (provides the specific patterns, shapes and details of the image)

![[Pasted image 20260203101514.png]]

It is **practical** to center the 2D DFT basis by **shifting** its origin to the **center** of the array, which is easier for image filtering in the frequency domain

![[Pasted image 20260203101526.png]]

The fourier spectrum has a **complex magnitude**
- e.g. for complex number $3+4j$, the **spectrum** will be $\sqrt{3^{2}+4^{2}}=5$

![[Pasted image 20260203101641.png]]

The spectrum is **symmetric** around its origin
- In fact, the spectrum has 4x the size of the original image

![[Pasted image 20260203101855.png]]

For every 2D basis vector, we **know** the $u$ and $v$
- Knowing this, we can trace it back to the fourier spectrum
- Knowing the **magnitude** of each basis, we know how **important** it is to the spectrum

![[Pasted image 20260203102011.png]]

Although the input image and spectrum have the **same size**, there is **no** 1-1 mapping between the spatial/frequency domain

![[Pasted image 20260203102044.png]]

---

**Imaging de-noising**
Sometimes it's quite hard to perform image de-noising using just the pixel domain
- We would like to design a **mask** to block out certain frequencies, hoping to remove noise
- But how to design this mask?

Most noise appears in the **high frequency compoenents**. So we need to **modify** or **remove** the **high frequency coefficients** to remove noise.
- Simply, set the high frequency components to **zero**. This will **remove the noise**, however it will also **lose information**

There are a few options for **masks** for noise removal:

- **Ideal low pass** filter
	- ![[Pasted image 20260203103009.png]]
	- We just have **one parameter, $D_0$** - the **cut-off** frequency
	- If within the threshold, set to 1, and 0 otherwise
	- ![[Pasted image 20260203103057.png]]
- **Gaussian low pass** filter
	- ![[Pasted image 20260203103351.png]]
- **Butterworth low pass** filter
	- ![[Pasted image 20260203103411.png]]
	- $n$ is the hyper-parameter

We could also use **high pass filters**:

![[Pasted image 20260203103637.png]]

![[Pasted image 20260203103750.png]]

---

Next, there are **other types** of noise:

e.g.,
- **Period Structures**:
	- ![[Pasted image 20260203104013.png]]
	- ![[Pasted image 20260203104021.png]]
	- We can use **Notch filters**
		- They can be used to **remove repetitive spectral noise** from an image
		- A **narrow mask** that **notches out** particular frequencies
		- **Zero out** a selected frequency component (and maybe some of its neighbours) and leave other frequencies intact
		- An **ad hoc** procedure - requires a **human expert** to determine which frequencies need to be removed to clean up the image
	- ![[Pasted image 20260203104201.png]]
	- ![[Pasted image 20260203104215.png]]

---

**Discrete Cosine Transform (DCT)**

DCT is similar to DFT
- It **transorms** a **discrete input** from space or time domain to the **frequency domain** by expressing the input as a weighted sum of basis functions
- DCT uses **only cosines** as basis, which makes it a **real transform** (real basis function and real coefficients), whereas DFT has complex basis and complex coefficients

![[Pasted image 20260203104850.png]]

Note the forward and inverse transforms have the **same basis**. DCT basis matrix is **orthonormal, real and symmetric**.
- DCT is of the **same length** as the **input function** (same as DFT)

![[Pasted image 20260203104939.png]]

Next, we can get the 2D extension of the DCT

![[Pasted image 20260203105040.png]]

The DCT **basis** can be visualised:

![[Pasted image 20260203105225.png]]

DCT is **popular** because:
- Can represent an **input** in a **more compact** manner
	- Unlike the DFT, Only a **smalll number** of DCT coefficients are **large**, most are very small
	- This is very useful for **compression**, where we can **discard coefficients** with **relatively small values** without introducing visual distortion in the reconstructed image

![[Pasted image 20260203105448.png]]

---

Catchup: Wavelets

---

**Forward Discrete Wavelet Transform (DWT)**

The steps for the 1D DWT, operating on a real-valued vector $x$:
1. Signal $x$ is **low-pass** filtered and the resulting coefficients are grouped as the first 8 elements of the vector
	- ![[Pasted image 20260206121307.png]]
2. The signal $x$ is **high-pass** filtered and the resulting coefficients are grouped as the last 8 elements of the vector
	- ![[Pasted image 20260206121342.png]]

![[Pasted image 20260206121359.png]]

![[Pasted image 20260206121411.png]]

The **highest frequencies** will be **isolated** and **localised** in H1, followed by intermediate frequencies in H2, H3

In the lower frequencies, the **resolution** is **reduced** by **half** for each level of the wavelet transform
- Thus, lower frequencies cannot be localised at the same resolution as higher frequencies

---

**2D DWT**

![[Pasted image 20260206121545.png]]

Based on 1D DWT

**One-level**:
- First, we perform 1D DWT along the **rows**
	- Half the size of the iamge will be low frequencies representation
	- Same for high frequency representation
	- And each component in between these

- Second step - we apply 1D DWT along the **columns**
	- We then get **pairs** of images, in which we have performed either low-pass first (on the rows) and then high pass (or low pass) on columns, or other way around (all combinations of low-pass/high-pass first and second)

![[Pasted image 20260206121900.png]]

We can also perform on multiple levels:

**3-level**:

![[Pasted image 20260206121851.png]]

This has good properties
- We can construct the **inverse DWT**. As such, we can **reconstruct the L2 LL**, and **L1 LL** and the overall image itself.

![[Pasted image 20260206121944.png]]

The purpose of using 2D DWT is for **image denoising**

![[Pasted image 20260206122008.png]]

Example:

![[Pasted image 20260206122126.png]]

---

Comparison between DFT, DCT and DWT:

![[Pasted image 20260206122245.png]]

