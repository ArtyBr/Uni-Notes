There are two cases for the application of this.
1. **Is** the image captured by the claimed device? **Binary classification**
	- Verification problem - 1:1
	- ![[Pasted image 20260306120846.png]]
2. **Which** camera captured the image?
	- Recognition problem - 1:N
	- ![[Pasted image 20260306120925.png]]

How can we do this?
- **Chrominance subsampling** (from lab 2)
	- Only useful if subsampling methods are different for different source devices
- EXIF (Exchangeable Image File) header
	- Meta information: Camera model, exposure, date/time, resolution etc.
	- However, header data may not be available if the image is re-saved in a different format or re-compressed
- Watermarking
	- Some camera models tried to embed invisible watermarks for the sake of device identification
	- An 'active approach' - may become useless unless **all cameras** insert watermarks
- Machine learning approach
	- An early work based on image features and SVMs
		- With an accuracy of 78-95% for 5 classes, this is **not high enough** for court-case evidence
	- Is it a good idea to directly extract image features for this task?
	- For example:
		- Raw pixels
		- LBP
		- HoG
		- DCT
	- Maybe not - the **signature** of this camera should be **independent of the image contents**

So, what should the signature look like?

First, we could use **defective pixels** as a signature
- If the same pixels are defective or dead between images, we can deduce the source camera based on this.
- However, this is limited only to **broken cameras**
- ![[Pasted image 20260306121508.png]]

**Sensor pattern noise**

Imperfections and noise characteristics of imaging sensors allow forensic experts to match an image to its source device

These noise components are:
- **Intrinsic** to the image acquisition process and hence **cannot be avoided**
- Can survive **processing** of images
	- e.g. JPEG compression
- Often considered as 'biometrics' for image sensors
	- Or referred to as 'devicemetrics'

![[Pasted image 20260306121951.png]]

**Sensor noise**
- Assume the acquisition of an image in an evenly lit, homogeneous scene
	- We expect to have the **same amount** of light energy incident on two pixels
	- However, the image will still exhibit **small changes** in intensity between **two individual pixels**
		- This is called **sensor noise**

![[Pasted image 20260306122330.png]]

![[Pasted image 20260306122339.png]]

In sensor noise, there are two parts - **shot noise** (which is *random* - not useful), and **sensor pattern noise** (which is *deterministic* - and therefore **useful** to us)

**Shot noise**
This arises due to the varying **number of photons** arriving at each sensor depending on exposure and pixel locations
- This is a **random** phenomena
- The number of photons collected by a given sensor can be modelled by a **poisson distribution**
	- Expresses the probability of a given number of photons arriving in a sensor

**Sensor Pattern Noise (SPN)**
This is a **deterministic distortion component**
- It stays approximately **the same** if multiple images are taken using the same camera
- It is present in **every image** a given sensor takes, and thus can be used a characteristic/fingerprint of the imaging device

![[Pasted image 20260306122704.png]]

**Fixed pattern noise** (FPN)
This is the **variation** in **pixel sensitivity** when the sensor array is **not exposed to light**
- aka dark current noise

Consider this is a **small offset** from the average value across the imaging array at a particular setting but without external illumination
- FPN is **additive**, and it depends on **exposure** and **temperature**
- It can be **easily suppressed** by subtracting a dark image (taken by the same sensor) from a given image
	- Many middle to high-end consumer cameras will do this

![[Pasted image 20260306122842.png]]

**Photo response non-uniformity** (PRNU)
This is a **dominant part** of SPN
- The primary component of PRNU is the **pixel non-uniformity pattern** (PRU)
- This arises because of the different sensitivity levels of pixels to light
	- Due to the in-homogeneous nature of **silicon wafers** and **imperfections** during the sensor manufacturing process
	- A wafer is a thin slice of semiconductor material used to manufacture integrated circuits
- This is unique, and can be used for source device identification

![[Pasted image 20260306123026.png]]

The other component of PRNU is **low frequency defects**
- These arise due to **light refraction** on dust particles and optical surfaces, interference with other sensors
- Seen as slowly varying distortions - appearing in the low frequency components of the image

![[Pasted image 20260306123135.png]]

---

Modelling the acquisition process

![[Pasted image 20260306123154.png]]

The output of the imaging sensor is **not** the output of the whole system
- $y_{ij}$ is the output of the imaging sensor, which is a combination of the PRNU multiplied by the addition of the light incident on the sensor and the shot noise, and then the addition of dark current noise and random noise

![[Pasted image 20260306123447.png]]

![[Pasted image 20260306123452.png]]

We want to use PRNU (the dominant part of SPN) for identification, 
- How do we estimate it?

We can simplify our earlier equation by removing the shot noise and random noise

![[Pasted image 20260306123535.png]]

Approximating PRNU will require access to the sensor output **before processing** 
- ($y_{ij}$)

**SPN Estimation**
It is not possible to **directly estimate** PRNU due to:
- The non-linear nature of the camera processing
- No access to sensor outputs before camera processing

SPN looks just like noise - **high frequency components**
- Often **mixed** with **other** high frequency signals in the image, e.g. scene details

Given a device we can have **multiple images taken**, then we can *approximate* SPN based on its **deterministic nature**

Consider multiple reference images (ideally in a uniformly lit scene) taken by a particular camera
$$p^{k},k=1,2,...,K$$
Suppress the scene content from each reference image by **de-noising**. Let these de-noised image be:
$$F(p^{k}),k=1,2,...,K$$
- Think of this as an averaging/weighted averaging process

Subtract the de-noised version from the original image to get the noise residual:
$$n^{k}=p^{k}-FP(p^{k}),k=1,2,...,K$$
Average the noise residual to approximate the reference SPN
$$SPN_{ref}=\frac{1}{K}\sum\limits_{k}n^k$$
Example:
- ![[Pasted image 20260306124040.png]]
- ![[Pasted image 20260306124048.png]]
- The SPN of a single image is:
- $$n=p-F(p)$$
- Denoising is difficult in textured regions of an image
- Single image SPN approximation;
	- High frequency components mixed with SPN
	- ![[Pasted image 20260306124132.png]]
- PRNU is largely suppressed in dark areas, where $x_{ij}\approx 0$
- ![[Pasted image 20260306124151.png]]

**Understanding SPN**

Make an assumption - there is **no camera processing**
$y=p$

![[Pasted image 20260306124431.png]]

![[Pasted image 20260306124447.png]]

![[Pasted image 20260306124607.png]]

Now, with SPN how do we do source device identification?

![[Pasted image 20260306124826.png]]

Source **verification**:

![[Pasted image 20260306124841.png]]

Source **recognition**:

![[Pasted image 20260306124847.png]]

