jWe often need to enhance images for forensic application. Enhancements include:
- **Improving illumination**
- Improving **contrast**
- Removing **unwanted structures** (noise) or **motion blur**
- **Sharpening** an image

Pixel (or *spatial*) domain processing: directly work on **pixel values** and change them as needed
- For simplicity, we use **grayscale images**

![[Pasted image 20260123123210.png]]

Most of the techniques can be **easily extended** to colour images by applying the **enhancement techniques** to each colour channel **separately**

---

**Histograms**
A histogram is a plot that shows the underlying **frequency distribution** of a set of **continuous data**.
- First, split the data into **bins** (i.e. **intervals** that cover a **range of values**)
- **Count** the corresponding occurrences

![[Pasted image 20260123123502.png]]

An *image histogram*

Given an image with $L$ gray levels, an **image histogram** $h(k)$ is a **discrete function** defined by:
$$h(k)=n_k$$
- Where $k$ denotes the **gray level** $k=0,1,2,...L-1$ (i.e. **bins**)
- $n_k$ denotes the **number of pixels** with **gray level $k$** (i.e. **frequency**)

![[Pasted image 20260123123659.png]]

What does a histogram **tell you?**

![[Pasted image 20260123123725.png]]

A **high contrast** image ideally should have a **flat histogram** spanning the *entire range* of intensity

---

**Contrast enhancement: Histogram equalisation**
A simple yet effective technique for enhancing contrast in images
- The **main goal** of histogram equalisation is to **convert** a given image histogram to a **flat one**

![[Pasted image 20260123124132.png]]

First, we introduce the **probability mass function** (PMF) to **normalise** the histogram
- Do this by dividing each bin frequency by the total number of values
- Gets you %'s rather than exact values

![[Pasted image 20260123124214.png]]

Then we use the **cumulative distribution function** (CDF).
- For a **continuous random variable $X$**, the CDF is defined as:
  $$F_{X}(X)=Prob(X\le x)$$
- Or for the **discrete case**:
- $$F_{X}(X)=Prob(X\le x)=\sum\limits_{x_{i}\le x}Prob(X=x_{i})$$
![[Pasted image 20260123124504.png]]

**Example**:

![[Pasted image 20260123124705.png]]

In the input image, we should adjust the pixels in 'bin' $k_x$ to $k_y$ such that they have the **same accumulated probability** (as shown in CDF)

![[Pasted image 20260123125029.png]]

![[Pasted image 20260123124718.png]]


Histogram equalisation can **also be applied** to **RGB** images by applying it on each channel separately.

Alternatively, convert it to the YCbCr colour space, in which case only apply it to the Luma channel (Y channel).

![[Pasted image 20260123125440.png]]

The idea of **histogram equalisation** can be **generalised**
- It is possible to **transform** and image histogram to **other histograms**

When going from a **histogram to uniform distribution** - *histogram equalisation*
When going from a **histogram to another image** - *histogram matching*

---

**Noise**
Unwanted structures in images that *degrade* an image
- Occurs due to **imperfections** in the device, environment, transmission, compression
- Noise is a **random** quantity, often unknown

For practical purposes, we made **assumptions** about noise:
- It is **additive**
- A **random variable** that can be modeled as a **known distribution function** (e.g. Gaussian noise)

![[Pasted image 20260127101757.png]]

**Gaussian noise**: $n(x, y) \approx N(0, \sigma^2)$ 
- Each location/point follows a **zero-mean** Gaussian distribution
- It is **independent** of pixel locations/values
	- Mutually independent
These two conditions are known as the IID (**independent and identically distributed**)

![[Pasted image 20260127102020.png]]

![[Pasted image 20260127102041.png]]

How to **remove** gaussian noise?

We can use **local averaging**
- Replace every pixel value by the **mean** (i.e. expected value) of its **neighbouring pixel values**

![[Pasted image 20260127102410.png]]

Using a **larger mask** smooths it too much, meaning the image becomes **blurry** and loses detail

Local averaging is a **convolution** operation
- Multiply each pixel value by the kernel matrix and add them together to get the final value
- We can have different kernels

---

Image **frequencies**

**High** frequencies occur at **sharp scene details**
- Whereas **low** frequencies occur at smooth areas

![[Pasted image 20260127103113.png]]

We can use **low-pass** or **high-pass** filters, which give us information about different structures of the image:

![[Pasted image 20260127103153.png]]

---

**Impulse noise** (aka Salt and Pepper noise)
Cause by **faulty sensors**
- **only a few** pixels are modified and are replaced by **black** or **white** pixels (*dead* pixels)
- The observed pixel $g(x, y)$ can be modelled as:

![[Pasted image 20260127103320.png]]

This could be reduced by the **median filter**
- We use a similar mask, but we **sort** the entries and choose the **one in the middle**
- Replace original value with this

![[Pasted image 20260127103504.png]]

We use median filter because the mean would be **influenced heavily** by the one rogue/dead pixel
- Median filter is **less likely** to be affected by **extreme** values caused by salt and pepper noise

![[Pasted image 20260127103628.png]]

