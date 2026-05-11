**Human vision** as *inspiration*

![[Pasted image 20260126141241.png]]

However, humans do this:
- *Integrate* vision with **memory**, **language** and **intent**
- Use strong **priors** and **assumptions**
- **Infer** meaning from sparse data
- Robust to **noise** and **ambiguity**

*but*, Robot:
- Measure **pixels**, *not objects* 
- **Lack common-sense** priors
- Require **explicit models** or **training**
- Fail gracefully **only if designed to**

What we can **borrow** from the Human Vision System (HVS)
- **Trichromatic colour perception**
	- Humans perceive colour using three channels in the visible spectrum
		- Cameras an displays adopt RGB
- **Luminance dominates visual perception**
	- Much of what we perceive as structure comes from **intensity**, *not colour*
		- Many vision algorithms operate effectively on **grayscale** images
- **Sensitivity to edges and contrast**
	- The HVS responds strongly to **changes** in **intensity**
		- **Edge detectors** and **feature extractors** are central to vision pipelines
- **Multi-stage visual processing**
	- Visual signals are processed **hierarchically** rather than all at once
		- Machine vision systems use **stages pipelines**

Cameras measure the **light** that hits the sensor at different points (pixels).

There are certain trade-offs in terms of **noise** and **resolution** (both temporal and physical)

![[Pasted image 20260126142237.png]]

There are a couple options for technologies using **vision sensors**, CCD vs CMOS :

![[Pasted image 20260126142440.png]]

![[Pasted image 20260126142519.png]]

Essentially, for the CCD we need more complicated circuitry overall. However, with CMOS we need circuitry for each pixel so resolution may be smaller.

When adding colour, we have 3 different sensors, R, G and B. Refer to Digital Forensics notes on the details of this.

In both single-chip and three-chip cameras, photodiodes are more sensitive to longer wavelengths. 
- As a results, **blue light** is detected **less efficiently** than red and green
- To **compensate**, the **blue channel** is **amplified**, 
- This **increases noise** relative to other channels

The adjustment for this is known as **white balance**
- Achieving **consistent** colour across time and environments remains a challenge for mobile robots

RGB colour space - read from Digital Forensics

There are other colours space: **CMYK** and **HSV**

Why **demoisaicing** matters in Robot Vision
- **Demoisaicing** *reconstructs* missing colour information, not ground truth
- Fine details, edges and repeating patterns may be **misinterpreted**
- **Noise** and **illumination** changes **amplify errors** in the reconstruction

**Resolution**
- Resolution is the number of **spatial samples** in the image
- Determined by the pixel **density** on the sensor
- More pixels -> More detail
- Mobile robots process multiple **frames per second**
- However, **resolution** directly **impacts real-time performance**

Therefore, we often would like to **reduce image resolution**
Common approaches:
- **Spatial downsampling** (fewer pixels)
	- **Remove pixels** reduce image size
	- Decreases spatial detail
	- Can **discard** important structure
	- Often **irreversible**
- **Reducing colour information** per pixel
	- Converting to **grayscale**
		- Moving to using **single intensity values** per pixel rather than 3 in RGB
	- **Thresholding** to **binary images**
		- Reduces data by **simplifying** pixel values, *not* removing them
		- Classified as **foreground** or **background**
		- Based on an **intensity threshold**
		- Produces a **binary image**
		- **Simple** and **computationally cheap**
		- **Sensitive** to **noise** and **lighting**
		- Used for **shape analysis**, **object counting** and **simple segmentation**

We need to **choose** the **right resolution**

Image **formation** determines how light is **mapped** to the samples on the image
- **Resolution** is about *quantity* of data, image **formation** is about *geometry*

Light rays reflect off an object and pass through a point, coming in contact with the film. We use a **lens** to converge parallel rays through our focal point and give more light.
- The **thin lens** equation tells us about the relationship between the distance to an object from the lens and the distance from the lens to the image sensor

![[Pasted image 20260126145247.png]] 

This approach is known as **depth from focus**
- Any object that **satisfies** this equation is **in focus**

![[Pasted image 20260126145408.png]]

**Depth of field** is the distance between the *closest* and *farthest* objects in photo that appears acceptably sharp
- The **size of the aperture** (hole through which light enters the camera) controls the amount of light entering the lens
- A **smaller aperture** increases the range in which the object is approximately in focus

The **field of view** (FOC) of a camera is an **angular measure** of the portion of 3D space that can be **seen** at a given moment through the lens. 
- It is typically measured in **degrees.**

![[Pasted image 20260126145635.png]]

- The field of view is influenced by the **focal length** of the camera lens.
	- **Shorter** lengths generally result in **wider** fields of view (and vice versa)

---

The camera imaging process can be modelled with a **pinhole camera** model

![[Pasted image 20260127150626.png]]

'**Perspective projection**' maps a 3D scene on to a 2D image plane.
Assumptions:
1. The **center of projection** coincides with the **origin of the world**
2. The **camera axis** (optical axis) is **aligned** with the **world's z-axis**
3. We can avoid image inversion by assuming that the image plane is **in oront** of the centre of projection

![[Pasted image 20260127150910.png]]

![[Pasted image 20260127151031.png]]

![[Pasted image 20260127151413.png]]

**Homogenous coordinates**

![[Pasted image 20260127151615.png]]

**Perspective Projection Matrix**

![[Pasted image 20260127151803.png]]

**Intrinsic camera matrix**

![[Pasted image 20260127151836.png]]

This gives us enough information to represent the 2D image points as a set of transformations to the original 3D points in the real world

![[Pasted image 20260127152012.png]]

So, how can we reconstruct the actual 3D points from the 2D images?

![[Pasted image 20260127152116.png]]

---

**Structure from Stereo**

![[Pasted image 20260127152218.png]]

![[Pasted image 20260127152226.png]]

---

**Structure from Motion**

By capturing lots of image from different angles, and matching points between the images, you can reconstruct where the points must lie in 3D.



