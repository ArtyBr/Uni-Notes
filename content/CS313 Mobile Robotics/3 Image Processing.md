**Pointwise transformations**

![[Pasted image 20260127152827.png]]

**Convolutions**

![[Pasted image 20260127152847.png]]

Convolutions with discrete images:

![[Pasted image 20260127152950.png]]

![[Pasted image 20260127153509.png]]

These kernels are very **quick** for doing things to images

**Smoothing**

![[Pasted image 20260127153537.png]]

Replace each pixel with the **average** of its **neighbouring pixels**

Instead, we can use a **gaussian filter** to prioritise the influence of the central pixels

![[Pasted image 20260127153809.png]]

---

**Line detection**

We can use other types of kernels to detect different lines

![[Pasted image 20260127153906.png]]

![[Pasted image 20260127153918.png]]

Use a **median filter** to remove **salt and pepper noise**

![[Pasted image 20260127154112.png]]

---

**Edge features**

![[Pasted image 20260127154154.png]]

Edges can be very sharp or quite shallow/ramped/gradual

![[Pasted image 20260127154327.png]]

![[Pasted image 20260127154343.png]]

![[Pasted image 20260127154353.png]]

Sometimes, we have noise with these edges, so you can get poor results:

![[Pasted image 20260127154515.png]]

---

Developing an **edge detector**

![[Pasted image 20260127154624.png]]

Could use an image **gradient**

![[Pasted image 20260127154644.png]]

![[Pasted image 20260127154650.png]]

Next, we would like to apply this to **discrete images**

![[Pasted image 20260127154812.png]]

There exist the following filters to find these:

![[Pasted image 20260127154845.png]]

Next, there are points that lie on an edge that can be identified by using the second-order partial derivative

![[Pasted image 20260127155129.png]]

This is called the **laplacian** for edge detection

![[Pasted image 20260127155143.png]]

Why would you use second-order differentiation for edges?

![[Pasted image 20260127155216.png]]

And here is the filters with **noise**:

![[Pasted image 20260127155241.png]]

![[Pasted image 20260127155247.png]]

---

**Canny Edge Detector**

1. Smooth image with Gaussian filter
	- ![[Pasted image 20260202141124.png]]
2. Computer **gradient** of the image e.g. using the **sobel operator**
	- ![[Pasted image 20260202141144.png]]
3. Find the gradient **magnitude** and **orientation**
	- Edges correspond to places where **gradient magnitude** is large
	- ![[Pasted image 20260202141211.png]]
4. Apply **non-maximum suppression** to thin the edges
	- ![[Pasted image 20260202141229.png]]
5. **Track edges** by **hysteresis**
	- ![[Pasted image 20260202141245.png]]
	- ![[Pasted image 20260202141254.png]]
6. Edge **linking**
	- ![[Pasted image 20260202141319.png]]
Overall:

![[Pasted image 20260202141625.png]]



**Strengths**:
- **Thin**, **well-localised** edges
- Robust to **noise**
- Produces **clean**, connected contours
- Still widely used in practice

**Limitations**
- **Parameters** matter ($\sigma$, thresholds)
	- ![[Pasted image 20260202141741.png]]
- **Edges** only - no descriptors

Edges may **fail** for some reasons:
- Edges alone are:
	- **Ambiguous**
	- **Hard** to **match**

Image **matching** *requires*:
- **Repeatable points**
- Discriminative neighbourhoods

---

**Image points** and **corners**

Next, we would like to identify **interest points** and **features**from images

Corresponding pairs of pixels between images
- ![[Pasted image 20260202142122.png]]

Need it to be a **repeatable** detector
- So it can be applied to different images with the same output

We need it to be a **discriminative** detector
- So that different features are distinct to each other

So, what makes a **good feature**?
- **Accuracy**:
	- Detect *all* (or most) **true** interest points
	- No **false** interest points
- **Precision**:
	- Well-localised
	- Area of the points to be quite small
- **Repeatable** across different images
- **Robust** with respect to noise
- **Efficient** detection

---

**Corner detection**

This is **not** a **flat region** or an **edge**

Usually, based on:
- **Brightness** of images
	- Usually **bright derivatives**
- **Boundary extraction**
	- First step edge detection
	- Curvature analysis of edges

Imagine placing a small window over an image and **sliding** it slightly
- **Flat region**: Sliding the window barely changes the pixel values
- **Edge:**: Sliding **perpendicular** to the edges causes change, but sliding along it does not
- **Corner**: Sliding in **any direction** causes a **big change**

![[Pasted image 20260202142526.png]]

So, we introduce the **Harris Corner**

![[Pasted image 20260202142547.png]]

![[Pasted image 20260202142557.png]]

![[Pasted image 20260202142604.png]]

![[Pasted image 20260202142614.png]]

So, overall the harris detector:

![[Pasted image 20260202142631.png]]

Example:

![[Pasted image 20260202143319.png]]

![[Pasted image 20260202143329.png]]

**Properties** of the **Harris detector**

- **Rotation Invariance**
	- The **eigenvalues** stay the **same direction** of corresponding eigenvectors changes
	- So, *corner response $R$* is **invariant** to **image rotation**
- *Partial* invariange to **intensity change**
	- Only **derivatives** are used
	- Invariance to intensity shift:
	- $I\rightarrow I+b$
	- ![[Pasted image 20260202143541.png]]
- *Not* invariant to **image scale**
	- ![[Pasted image 20260202143557.png]]

---

**SIFT**

SIFT finds good **locations** of corners
- Scale invariant feature transform

1. Find extrema in scale space
	- Potential locations for finding features
2. Find the location of key points
	- Accurately locating the feature key
3. Assign an orientation to each keyponts
4. Describe the keypoint using the SIFT descriptor

