We would like to be able to tell if an **image is authentic**, and to **localise** which part has been **tampered**

Digital watermarking is an **activate** approach to answer this question
- Insert a **signature pattern** into the data before it is distributed

Digital watermarking is the process of **embedding** a digital pattern within a cover or carrier of data, such as an image/video/audio.

![[Pasted image 20260206122847.png]]

---

**Types** of digital watermarks

- **Blind/not blind**
	- Blind watermarking **does not** require **access** to the original un-watermarked data to **recover** the watermark
- **Visible/Not visible**
- **Private vs Public**
	- Only **authorised users** can detect the **private** watermark
- **Robust/Fragile**
	- **Robust**: Designed to **survive** intentional and unintentional modification
	- **Semi-fragile**: Designed for detecting any **unauthorised** modification, at the same time allowing some basic modifications such as rotation, scaling and cropping
	- **Fragile**: Used to detect **any** unauthorised modification

**Applications**
- **Ownership claims**
	- ![[Pasted image 20260206123129.png]]
- **Unauthorised copy detection**
	- ![[Pasted image 20260206123141.png]]
- **Tampering detection**
	- ![[Pasted image 20260206123210.png]]

---

**Components** of watermarking:

**Encoder** (E)

![[Pasted image 20260206123254.png]]

**Decoder** (D)

![[Pasted image 20260206123313.png]]

**Comparator** (C)

![[Pasted image 20260206123326.png]]

---

**Image Bitplanes**

The **simplest** watermarking approach involves **changing pixel values** in a  given image by changing its **less important bitplanes**

The concept:

![[Pasted image 20260206123419.png]]

Essentially, bitplanes refer to the **significance of the bits** they refer to

(Can also have the second LSB)

![[Pasted image 20260206123604.png]]

Or, MSB

![[Pasted image 20260206123626.png]]

![[Pasted image 20260206123725.png]]

The **sum** of all the bitplanes gives you back the **original image**

![[Pasted image 20260206123903.png]]

Whereas for bitplane 7, each pixel is **black** if a pixel had value >128, otherwise white.
- Whereas in bitplane 0, this is basically if the pixel was odd or even

---

**Bitplane substitution**

![[Pasted image 20260206124024.png]]

The watermark can be **smaller** or **equal** to the size of the image. We can insert mujltiple watermarks, if needed.


Two types:
- **Content-dependent watermark**
	- Extracted from the host image
- **Content-independent watermark**
	- A different image, such as a logo

![[Pasted image 20260206124120.png]]

(this is an example of content-independent watermarking)

We replace one of the planes with the watermark
- We would like to replace the **least significant** pixel values because it will **hardly** change the values, and so doesn't affect the image content very much

On the other hand, we could choose **bitplane 7**, which would make it a **very obvious** watermark

![[Pasted image 20260206124234.png]]

So, to create a **visible watermark**, we replace the **more significant planes** and for **invisible** watermarks, we replace the **least significant** planes.

![[Pasted image 20260206124318.png]]

---

**Content-dependent watermarking**

![[Pasted image 20260206124456.png]]

![[Pasted image 20260206124604.png]]

How to **select** watermark **embedding locations**?
- Can be embedded in **specific bit planes** over the **entire image** (at all pixel locations)
	- Give from the previous example (warwick logo)
- Can be embedded in **specific bit planes** of **selected image regions**
	- E.g. $N\times N$ region **centered** at location ($x, y$)
- Pixel locations can be **chosen randomly**. To **decode** such a watermark, a **key** is required.
	- The key is usually the **seed** to a **Random Number Generator**

---

Watermark **extraction**

![[Pasted image 20260206124803.png]]

We need to know the exact locations from which to extract the watermark:
- **Bitplane** location
- **Embedded pixel locations** in the image

---

Watermarking by bitplane substitution **is**:
- Extremeley **simple** and **fast**
- Can create **visible**or **invisible** watermarks
- Does not necessarily require the original image to recover the watermark
	- Just need the bitplane location and embedded pixel locations
- The watermarked are **fragile/semi-fragile**
	- Simple attacks like **cropping** may **destroy** such a watermark
	- Pixel locations spread over the entire image is more **robust** to modifications like cropping
	- The **entire watermark** can be **removed** by **removing the LSB plane**
	- Survives **compression** to some extent

---

Watermarking in the **Frequency Domain**

Main idea is to embed the watermark in the perceptually important regions of the image, so that the watermark is hard to remove without degrading the visual quality of the image.
- This creates **invisible** watermarks
- Known to be **more robust** to common watermarking attacks

The DCT coefficients help us determine the **perceptually significant regions** to decide **where** to put the watermark
- The **DC** components, $w_{00}$, carries the **most energy** (or information) about the image.
- We also have **AC** coefficients (the rest of them)
	- ![[Pasted image 20260210101738.png]]
	- They carry less energy, and we would rather embed the watermark in one of the **low/mid frequency regions**
	- It's **not** a good idea to embed it in the **high frequency** parts, since when we do image compression, those parts are the ones we **get rid of first**

We can index the frequencies of the coefficients of the image in this zig-zag way:

![[Pasted image 20260210101943.png]]

We have a few approaches to actually doing the watermarking now.

---

**LSB substitution**

Let's say we specify the following 4 locations where we would like to embed the watermark:

![[Pasted image 20260210102150.png]]

We look at the coefficient, take just the **integer part**, convert it to **binary**, and then we adjust the **least significant bit**

![[Pasted image 20260210102203.png]]

![[Pasted image 20260210102302.png]]


---

**Spread Spectrum**

We would like to **embed the Gaussian** 

Create a watermark $w={w_{i}},i=1,2,...,m$ that is a **random, Gaussian-distributed** sequence
- The SS watermark is **spread** over **many frequency bins** so that the energy in any one bin can be **very small** and **certainly unpredictable**

We apply 2D Fourier transform (DCT)
- We get $h={h_i}$ represent the **perceptually important** DCT coefficients of the cover image as a vector

We can combine them together, and we get $h*=(h,w)$, which is some **function** operating on $h$ and $w$

![[Pasted image 20260210102819.png]]

- And $\alpha$ is a user-defined parameter (hyper-parameter)

Example:

![[Pasted image 20260210102938.png]]

We can also **decode** this watermark by:
- Take $h*$, and original image's coefficients $h$, we can reverse the function to get the watermark

![[Pasted image 20260210103252.png]]

- Note that **we cannot extract** the watermark **without access** to the **original un-watermarked image**

---

**DWT (Discrete Wavelet Transform)** based watermarking

Since DWT gives us different parts of the image with specific frequency spectra at the different levels, we can **choose** which one we want to embed the watermark into.
- This is the process of choosing the embedding location

![[Pasted image 20260210103548.png]]

Next, we just multiply hyper-parameter $\alpha$ by $W$ and add it to the spectra/DWT component that we choose, which creates a watermarked image after applying 2D Inverse DWT

![[Pasted image 20260210103424.png]]

We can then **recover/decode** the watermark by using DWT in the inverse, knowing the original image and subtracting that level from each other.

![[Pasted image 20260210103708.png]]

---

**Hybrid watermarking**

Block-based approaches help embed watermarks in **both** *spatial* and *frequency* domains.
- These are also called **hybrid watermarking**

One simple approach:
- Divide an image into **blocks**
- Choose in **which blocks** we want to embed the watermark
- Take DCT of **each block**, embed watermark
- Decoder needs information about **which blocks to decode**

![[Pasted image 20260210103850.png]]

Another example:
- Divide into blocks
- For each block take the **2D DCT**
- Choose **two locations** in the DCT coefficient matrix that are of **equal perceptual importance**
	- How?
	- We look at the DCT of the image block, which is the same size as the original image
	- The image is a **linear combination** of the coefficients and the DCT bases.
	- ![[Pasted image 20260210104024.png]]
	- We look at the two locations, and we look at the values that they correspond to in DCT
	- The difference between them is not much, so if we swap the values, that wouldn't change much in terms of degrading the image's visual quality.
	- Therefore, we basically can just find DCT components that have very similar DCT values.

Example:

![[Pasted image 20260210104205.png]]

![[Pasted image 20260210104215.png]]

Next we can perform **decoding:**

![[Pasted image 20260210104311.png]]

---

Watermarking in the frequency domain is **known to be more robust** to common attacks:

![[Pasted image 20260210105015.png]]

---

**Comparing** watermarks

In simple cases, we can use **Mean squared error (MSE)**, the correlation coefficient or SSIM to **match** the watermarks (between the original watermark and recovered one)

![[Pasted image 20260210105111.png]]

---

**Attacks** on watermark

- Compression attacks
- Filtering attacks: Smoothing (low-pass)
- Jitter attack
	- Basic idea is to **change the locations** of embedded watermarks so that **it cannot be recovered**
	- **Split** the audio/image into a number of small chunks
	- Duplicate or delete the data points at random
	- Imperceptible in image, even in classical music
	- ![[Pasted image 20260210105238.png]]

