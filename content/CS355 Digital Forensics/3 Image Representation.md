**RGB Color model**
One of the most common colour spaces, used for **acquisition** and **display**
- When combined in different proportions, they will yield different colours

Can represent possible binary combinations of R, G and B as such:

![[Pasted image 20260120101318.png]]

Any point in the 3-Dimensional RGB colour space is a unique **combination** of R,G,B

![[Pasted image 20260120101403.png]]

RGB is device-dependent.
- Different devices capture, or reproduce, a given RGB value slightly differently.
- This is because colour filters and their response vary from manufacturer to manufacturer

![[Pasted image 20260120101547.png]]

Thus, an RGB value does not define exactly the same colour across devices

---
**Y'UV**
Can be converted to a 'luma-chroma' color space
- Y': The **luma** component, represents the **brightness** information
- U, V: The **chrominance** components, contain the **colour differences**

![[Pasted image 20260120101647.png]]

Unlike RGB, this has **sepearate channels** for **color (UV) and intensity(Y')**
- Y'UV was developed for analog TV transmissions to provide compatibility between black and white and colour TV

**Y'CbCr**
Digital images/videos usually use a variant of Y'UV called Y'CbCr

![[Pasted image 20260120101817.png]]

Likewise, we can convert a Y'CbCr image back to RGB using:

![[Pasted image 20260120101842.png]]

**Redundancy in Colour Channels**
Human eyes are much more sensitive to **brightness** information compared to **colour** information.

![[Pasted image 20260120101929.png]]

We want to separate the intensity from the colour channels
- There is a lot of **shared** (redundant) information in RGB images, however CbCr channels show colours as being more similar, allowing us to reduce redundancy and storage needs during compression without losing significant quality (chroma-subsampling)

---
**Chroma subsampling**
In general, subsampling is taking some result of a part of it as being the average of that area.
- ![[Pasted image 20260120102427.png]]
However, if we simply do it like this, we might lose some image details.

- ![[Pasted image 20260120102552.png]]
Since the human eye is less sensitive to **colour** than **luminance or intensity**.
- Chroma sub-sampling can be performed on the Luma-Chroma colour space, reducing colour quality, but this is not as noticeable for humans

![[Pasted image 20260120102714.png]]

The subsampling scheme is commonly **expressed** as a three-part **ratio**: A:b:c
- A = **width** of the region in which subsampling is performed
	- Usually, A=4
- b = number of Cb/Cr (chroma) samples in each row of A pixels (horizontal factor)
- c = number of changes in Cb, Cr samples between first and second row (vertical factor)

![[Pasted image 20260120102944.png]]

![[Pasted image 20260120102954.png]]

Subsampling approaches/techniques (depends on manufacturer):
- **Average**: The subsampled chroma component is the **average** of the 2x2 original chroma block
- **Left**: The subsampled chroma component is the **average** of the **two leftmost** chroma pixels of the block
- **Right**: The subsampled chroma component is the **average** of the **two rightmost** chroma pixels of the block
- **Direct**: The subsampled chroma component is the **top left** chroma pixel

Note: when taking average, perform **floor** operation to get integer value

---
We would like to have **quantitative** measures to detect changes between images

**Mean squared error**
Simplest method: Simply take the squared difference between pixel values and average this difference.
$$MSE(X,Y)=\frac{1}{N}\sum\limits^{N}_{i=1}(y_{i}-x_{i})^{2}$$
Where X and Y are the two different images (and $x_{i}$ and $y_{i}$ their pixels)

![[Pasted image 20260120104340.png]]

Note: both images must be the same size to compute MSE

![[Pasted image 20260120104459.png]]

**Correlation**
The **correlation coefficient** (Pearson's $r$) measures statistical relationship between **two variables**
- Measures only a **linear relationship** - both **strength** and **direction**

![[Pasted image 20260120104601.png]]

Treating images as random variables:
- Imagine a random process that is generating these numbers (pixel values) as the outcome of a spatially-varying function
- We can think of an **image** as a **discrete random variable $X$**
	- With integer values between 0 and 255

![[Pasted image 20260120104706.png]]

We can calculate:

![[Pasted image 20260120104727.png]]

Consider another image $Y$

**Covariance** measures the **relationship** between $X$ and $Y$

![[Pasted image 20260120104905.png]]

For images $X$ and $Y$, the **sample correlation coefficient** can be computed as:

![[Pasted image 20260120104802.png]]

However, these methods may be **limited** by **image distortion**

![[Pasted image 20260120105411.png]]

**Structural similarity**

Images have certain structural patterns:
- Spatial patterns
- Edges
- Shapes
- Corners

The **Structural Similarity (SSIM)** index is widely used in the imaging industry for **benchmarking** device performance
- Image **distortions** have **less impact** on SSIM
- *Reason*: It **separates** structural and non-structural information in images, measuring both types of differences between two images.

![[Pasted image 20260123121314.png]]

SSIM compares compares two images at **pixel level**
- A **square patch** $x$ centered at a pixel in $X$ is compared with the **corresponding square patch** $y$ in $Y$

![[Pasted image 20260123121418.png]]

SSIM performs:
- **Luminance comparison**
	- Local luminance is modelled by the **mean intensity** of the local region
	- ![[Pasted image 20260123121440.png]]
	- Where $C_1$ is a hyper-parameter (human-chosen)
- **Contrast comparison**
	- Local contrast is modelled by **standard deviation** of intensity of the local region
	- ![[Pasted image 20260123121602.png]]
- **Structural comparison**
	- Structural similarity is modelled by **correlation** between **normalised patches**
	- ![[Pasted image 20260123121659.png]]

**Local** SSIM score:

![[Pasted image 20260123121716.png]]

Set each similarity to the **power of** user-defined (hyper-parameters) $\alpha, \beta, \gamma$ and **multiply them together**

**Properties** of SSIM:
- **Symmetry**: SSIM($x, y$) = SSIM($y, x$)
- **Boundedness**: SSIM($x, y$) $<= 1$
- **Unique maximum**: SSIM($x, y$) $=1$ only when $x=y$

To get the global SSIM score:

![[Pasted image 20260123122211.png]]

However, there is also another way (Minkowski pooling), where you can set it to the power $p$

![[Pasted image 20260123122221.png]]

Examples comparisons using SSIM:

![[Pasted image 20260123122402.png]]