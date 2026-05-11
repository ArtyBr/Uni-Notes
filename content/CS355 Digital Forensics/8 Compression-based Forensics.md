Double-compression can leave some interesting patterns
- But the **key step** when discovering compression changes is the **quantization** step
- Since it is **lossy**, information after quantization (and then reversal) will be different than before

Compression-based forensic techniques largely rely on **detecting artefacts** introduced due to **multiple quantization**

---

**Splicing forgery**

Assume we have a JPEG image with quality $q_2$
- Splicing forgery is a forgery where the attacker wants to **merge** two images into one:

![[Pasted image 20260220122058.png]]

![[Pasted image 20260220122211.png]]

We will study a couple types of compression-based forensic techniques
- Both the methods try to uncover the **multiple quantization artefacts**

**Double compression**:
- Can only tell us if an image has been compressed multiple times.
- This can be intentional or accidental
- Forgery is a possibility but not confirmed

Used as a **screening process**
- If unusual artefacts are found, further forensic analysis is needed to confirm forgery

![[Pasted image 20260220122311.png]]

**JPEG Ghost**
- Can detect **splicing forgery**, and even can **localise** the regions that come from a **different image**

![[Pasted image 20260220122405.png]]

---

**Double compression**

Quantization **changes the DCT coefficients**
- Double quantization artefacts will be visible in the **distribution** of DCT coefficients

Given a variable $u$, with entries samples from a 1D discrete function $f(x)$:
- Quantization may be considered as an entry-wise operation described by a one-parameter family of functions

![[Pasted image 20260220122715.png]]

The **de-quantization** process can also be described:

![[Pasted image 20260220122736.png]]

We can visualise an example:

![[Pasted image 20260220122925.png]]

The shape of the histogram after quantization has a much smaller range, since we have divided the numbers by the quantization factor

Also, the **frequency** or the amount in each bin is much larger since we have merged lots of values into single bins
- Can see that originally y axis goes from 0 to 20, but after it goes from 0 to 400

![[Pasted image 20260220123311.png]]


If we apply this to all the bins, we see such a pattern:

![[Pasted image 20260220123729.png]]

With another example, we apply the quantization factor of 3 *followed by* 2

![[Pasted image 20260220123806.png]]

So, we get different features of **periodicity** based on different quantization factor orders

![[Pasted image 20260220123847.png]]

So, quantization changes DCT coefficients
- **Double quantization** artefacts will be visible in **distribution** of DCT coefficients

Here is what the distribution might look like:

![[Pasted image 20260220124227.png]]

![[Pasted image 20260220124318.png]]

If we look at the distributions for different components, we get the following graphs:

![[Pasted image 20260220124439.png]]

- We can see that DC looks quite sporradic but over all other blocks, we see a gaussian distribution

**But** if we compress the image again with JPEG quality of 50, the graph looks like so:

![[Pasted image 20260220124558.png]]

So, we end up getting period patterns:

![[Pasted image 20260220124803.png]]

![[Pasted image 20260220124835.png]]

---

Let's start with a simple quantization example, showing the difference between values before and after quantization

![[Pasted image 20260224101159.png]]

Based on $c_{1}^{*}$, we can apply a **second quantization** (with a different factor):
- Notice that at the end of de-quantization, all values are now multiples of $q_2$ (Same was true for first quantization example)

![[Pasted image 20260224101335.png]]

After this, we can calculate the **squared difference** between the two images:
- Compare the **difference** between $c_{1}^{*}$ and $c_{2}^{*}$ and **add the squared differences**

![[Pasted image 20260224101404.png]]

Now, based on $c_{1}^{*}$ we apply **another** quantization with a different quantization factor (14):

![[Pasted image 20260224101628.png]]

We can notice that **the difference** (SSD) between them **increases** as the **quantization factor** $q_{2}$ **increases**
- Also, if $q_{2}=q_{1}$, then the **difference** will be **0**
	- OR if $q_{1}$ is a **multiple** of $q_2$
- ![[Pasted image 20260224101928.png]]

How about when $q_{2}<q_{1}$?

![[Pasted image 20260224102008.png]]

- Small error

The graph looks like this: (Function of the quantization error against the second quantization factor $q_2$)

![[Pasted image 20260224102157.png]]

Then, let's say we do this multiple times?  a couple cases, based on  $e.g. q_{1}=5, q_{2}=3$
- $q_{3}=8\neq q_{1}\neq q_{2}$
	- Then the error will be very large - same as with $q_{1}$ and $q_{2}$
	- $SSD(c_{2}^{*},c_{3}^{*})=69$
	- ![[Pasted image 20260506114200.png]]
- $q_{3}=q_{2}=3$
	- Then $SSD(c_{2}^{*},c_{3}^{*})=0$
	- Obviously, since $q_{3}=q_{2}$, so there will be no difference between the quantised versions of those values
	- ![[Pasted image 20260506114012.png]]
- No lets say $q_3=q_1=5$
	- This would give a small SSD
	- $SSD(c_{2}^{*},c_{3}^{*})=4$
	- ![[Pasted image 20260506114034.png]]

This is a **second minimum** essentially!
- If it's plotted on a graph:
- ![[Pasted image 20260506114333.png]]
- Since the coefficients were initially quantised by $q_{1}$, one expects to find a **second minimum** when $q_{3}=q_{1}$

Now if we look at these SSD differences on a graph:

![[Pasted image 20260507162026.png]]

# Image Forensics: JPEG Ghosting

## 1. What is a JPEG Ghost?
JPEG ghosting is an image forensics technique used to detect image manipulation (like splicing). When an element from one JPEG image is pasted into another and the composite is re-saved, the spliced region often has a different **JPEG compression history** than the rest of the image.

*   **The Minimums Concept:** If you take a forged image and systematically re-compress it across all possible quality levels ($q \in [1, 100]$), you can calculate the difference between the test image and the re-compressed versions. 
*   **Single Compression:** A normal, unmodified JPEG will show a single global minimum (difference of zero) at its actual saved quality level ($q_{final}$).
*   **Double Compression (The Ghost):** A spliced region that was previously compressed at a different quality level ($q_{original}$) will exhibit a **strong local minimum** at that specific $q_{original}$ quality level. When plotted, this secondary minimum reveals the "ghost" of the previous compression.

## 2. Computational Challenges (DCT Domain)
To find these compression differences, one approach is to analyze the image in the **DCT (Discrete Cosine Transform) domain**. 
*   This requires analyzing the quantization artifacts across all 64 DCT frequency components for every $8 \times 8$ block in the image.
*   Running this statistical procedure across every component to detect multiple quantization steps is highly **computationally expensive**.

## 3. The Pixel Domain Solution
Instead of analyzing DCT coefficient histograms, it is much more computationally efficient to operate in the **pixel (spatial) domain**.

*   **Correcting the "Lossless" Misconception:** While JPEG compression itself is strictly *lossy*, the mathematical transformation from the quantized DCT coefficients back to the pixel grid (the Inverse DCT) is a linear mapping. This means that the quantization footprints left in the frequency domain are directly preserved in the spatial pixels.
*   Because of this mathematical relationship, analyzing differences in the pixel domain perfectly captures the underlying DCT quantization discrepancies without the heavy computational overhead of frequency analysis.

## 4. The Pixel-Domain Detection Method
To detect JPEG ghosts efficiently in the pixel domain, we use the following process:

1.  **Re-compression:** Take the target image and re-compress it at every quality level ($q = 1$ to $100$).
2.  **Pixel Comparison:** For each quality level $q$, calculate the difference (usually the squared error) between each pixel of the original image and the re-compressed image.
3.  **Spatial Averaging:** Average these differences over a sliding window (e.g., $16 \times 16$ pixels) to reduce noise. 
4.  **Ghost Mapping:** This results in a set of spatial difference maps. If a spliced region appears significantly darker (indicating a strong local minimum/lower difference) at a specific quality level compared to the rest of the image, you have successfully located a **JPEG ghost**.

## 5. Locating Ghosts and Distinguishing False Positives

### Identifying the Location
Because we are operating in the spatial (pixel) domain, identifying the physical location of the forgery is straightforward. By looking at the smoothed difference map $\bar{D}_q$ for a specific quality level, the spatial coordinates $(x, y)$ of the "ghost" naturally align with the image. 

The ghost is located wherever a distinct cluster of pixels drops to a local minimum (appearing as a dark silhouette) while the surrounding background pixels remain at a higher difference value.

### The False Positive Problem
Not every dark region on a difference map is a spliced ghost. The algorithm is prone to **false positives** in regions that lack high-frequency detail. 

Common culprits include:
*   Smooth, textureless areas (e.g., clear blue skies, flat painted walls).
*   Completely blown-out highlights (pure white) or crushed shadows (pure black).

**Why this happens:** JPEG compression discards high-frequency detail. If a region of an image is a perfectly flat color, it has almost no high-frequency detail to lose. Therefore, when you re-compress it at *any* quality level, the resulting pixels barely change. The pixel difference calculation $[I(x, y) - I_q(x, y)]^2$ will return values near zero, making the region appear as a dark "ghost."

### Distinguishing True Ghosts from False Positives
To separate a genuine JPEG ghost from a flat-texture false positive, we look at two factors:

1.  **The "Dip" Profile Across All Quality Levels:**
    *   **True Ghost:** Will show a distinct, sharp minimum at a *specific* quality level $q_{original}$, and the difference will rise again for other qualities.
    *   **False Positive:** Will appear dark (low difference) consistently across almost *all* quality levels because the flat region simply does not degrade when re-compressed.
2.  **Spatial Variance Masking:**
    *   Before analyzing the difference maps, calculate the local variance (texture complexity) of the original image $I$. 
    *   We can apply a mathematical threshold to ignore or mask out regions with extremely low variance. If a suspected "ghost" aligns perfectly with a zero-texture region, we can confidently dismiss it as a false positive.

---

**Copy-Move forgery**

![[Pasted image 20260227121220.png]]

This is the case where the **tampered region** has the **same compression quality** as the other image areas

![[Pasted image 20260227121255.png]]

However, we would like to be able to **locate** the **tampered region**

![[Pasted image 20260227121321.png]]

So how can we detect the forged areas?
- The **forged segment** is usually a **connected component**
	- Not a single pixel - it's a small block or area of pixels, copied from the same image
	- Search for **identical** blocks in the image?
		- Lossy (JPEG) compression may change the appearance slightly. 
		- For example, cloned region may be placed into four different DCT blocks, and compressed (slightly) differently from the original copied region
	- Search for **very similar blocks**
		- For each block, go over the rest of the block to **find matches**
		- Computational cost can be high
		![[Pasted image 20260227121703.png]]
Detection will be based on the (dis)-similarity between the **original segment** and the **pasted ones**

Must:
- Allow for an **approximate match** of small image segments
- Work in a **reasonable time** while **minimising false positives**

Assumption:
- Forged segment is usually a **connected component** rather than a collection of very small patches or individual pixels

---

First, we could do an **exhaustive search**
- Circular shift and match

We use a **for loop** with two variables, $k$ (row) and $l$ (column) 
- Circularly **shift** the image by $k, l$
- **Compare** the **circularly shifted** image with the **original** version until the original and cloned segments are matched

If with copy-move forgeries, we can find a suitable $k, l$ indicating identical (or very similar) regions in the same image

![[Pasted image 20260227122255.png]]

![[Pasted image 20260227122314.png]]

In lots of cases, a number of pixel pairs may produce the differences **below the threshold** $t$, and yield **false positives**.

![[Pasted image 20260227122635.png]]

We have to process these regions further:
- Leverage the **connected component assumption**
- Apply **morphological processing**:
	- **Erosion** - The value of the output pixel is the **minimum value** of all the pixels in the input pixels neighbourhood. 
		- In a binary image, if any of the pixels is set to 0, the output pixel is set to 0
	- **Dilation**
		- The value of the output pixel is the **maximum value** in the input pixels neighbourhood.
		- In a binary image, if any of the pixels is set to 1, the output pixel is set to 1.
	- Have to do them in that order - **erosion first**, then **dilation**
- ![[Pasted image 20260227122815.png]]
Now we perform all of these operations for **each $k, l$ pair**

![[Pasted image 20260227123054.png]]

And we have this **algorithm**:

![[Pasted image 20260227123106.png]]

---

Since the previous method was very computationally expensive, we can instead use:

**Block matching**
- A **fast searching method** for *similar blocks*
	- Block-to-vector for matching
	- **Spatial** feature vector
	- **Frequency** feature vector

![[Pasted image 20260227123309.png]]

We can **sort** the features **lexicographically**
- A *generalisation of alphabetic* order
	- Sorting strings made up of characters
	- Helps us sort **mathematical sets** and **tuples**


![[Pasted image 20260227123601.png]]

Algorithm, in the **spatial domain**:

![[Pasted image 20260227123618.png]]

This **dis-similarity** metric is: **Average** the **absolute difference** between two rows.

![[Pasted image 20260227123735.png]]

Now we can also do a similar technique in the **frequency domain**, using DCT and quantization:

![[Pasted image 20260227123914.png]]

However, this gives us lots of **false positives**:
- DCT coefficients encode the local block information
	- e.g. DC component encodes all the zero-frequency in the local block
		- Highly overlapped blocks might be very similar
	- Some **post-processing** is required to **reduce** the **false positives**
		- Again, using the **connected component assumption**

After sorting, we calculate all the similarities between blocks
- If there is only one match, it is likely that it is a false positive
- However if for certain regions, there are **lots of** similar DCT coefficients, then this is **likely** to be a **true match**

We can also use **more advanced features**
- Image features

A feature vector is a **signature** of an image (region) that captures its important characteristics
- Good features should be **compact** and **robust** representations of the image region

Features can be **global** or **local**, depending on whether they capture properties of the **entire image** or a **small part** of it.

- Raw pixel values
- Features in the frequency domain
- Statistical features
- Feature that encode shape, colour, texture

![[Pasted image 20260227124442.png]]

The features can be used for various **pattern recognition** tasks, including:
- Block/image matching
- Object tracking
- Classification

Some simple recognition system examples:

![[Pasted image 20260227124736.png]]

![[Pasted image 20260227124742.png]]

In real world cases, there is **imbalanced data**:
- Positive samples are **rare**
- Large amounts of negative samples

Application requirements:
- Medical diagnosis/screening system
	- Not tolerant to false negatives (i.e. diseases undetected)
- Law enforcement
	- Not tolerant to false positives (i.e. an innocent person is convicted)

**Accuracy** can be **meaningless**
- We also want to know the **error types**

We can use a **confusion matrix**:

![[Pasted image 20260227125106.png]]

There are other **common evaluation metrics**:

![[Pasted image 20260227125128.png]]

![[Pasted image 20260227125453.png]]

![[Pasted image 20260227125458.png]]

---

**Local Binary Pattern** (LBP)

LBP is a popular and important **feature** that can efficiently encode the **texture information** of an image region
- A texture is characterised by **complex patterns** composed of **sub-patterns**

![[Pasted image 20260303141649.png]]

![[Pasted image 20260303141658.png]]

Steps for computing LBP:
- Consider a $P$-connect neighbourhood
- **Threshold** the neighbourhood pixel values using the **centre pixel value**
- Create LBP code

![[Pasted image 20260303141743.png]]

We can create **rotation invariant** LBP

![[Pasted image 20260303141751.png]]

Next, we would like to get the LBP encoding of an **image region**:

Consider an 8-neighbourhood. Each LBP code is 8-bit.
- There are 256 possible LBP codes from 00000000 to 11111111 (0 to 2555).
- Each pixel is represented by **one** of these 256 LBP codes

Just like the pixel value histogram, we can now compute the **LBP histogram**
- For copy-move forgery detection we can use **block/region-based LBP histogram**

![[Pasted image 20260303142007.png]]

We want to use a **compact** LBP feature
- Less bins in the LBP histogram

**Uniform** LBP are those LBPs that have **at most 2** transitions 
(1 -> 0 or 0 -> 1)
- 11110000 - uniform LBP (1 transition)
- 10111111 - uniform LBP (2 transitions)
- 10101100 - non-uniform LBP (5 transitions)

![[Pasted image 20260303142314.png]]

For each sub-block:
- We use 58 histogram bins for uniform LBP codes, and
- 1 single bin for all non-uniform LBP codes

![[Pasted image 20260303142356.png]]

---

**Histogram of oriented gradients** (HoG)

HoG is one of the most popular features in image analysis and computer vision
- How do we **compute gradients** (derivatives) of an image?

In 1D;
- **Discrete approximations** of first order derivatives are given by:
- ![[Pasted image 20260303142720.png]]

This comes from:

![[Pasted image 20260303142749.png]]

![[Pasted image 20260303142756.png]]

![[Pasted image 20260303142813.png]]

![[Pasted image 20260303142829.png]]

![[Pasted image 20260303142842.png]]

![[Pasted image 20260303142854.png]]

So, we arrive at the following kernels:

![[Pasted image 20260303142913.png]]

In total, with an example:

![[Pasted image 20260303142934.png]]

![[Pasted image 20260303142947.png]]

![[Pasted image 20260303142954.png]]

So, to get the actual Histogram of Oriented Gradients:

![[Pasted image 20260303143039.png]]

![[Pasted image 20260303143047.png]]

Finally, we can perform **Dis-similarity** measurements in order to identify if there are parts that have been copied over

![[Pasted image 20260303143145.png]]

Now, we can use this to perform pattern recognition

For example, was the image captured by the claimed device?

![[Pasted image 20260303143221.png]]

Or, **which camera** captured the image?

![[Pasted image 20260303143237.png]]