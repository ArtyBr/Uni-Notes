How a digital image is **formed** and **stored**

The light enters the camera, and goes through the **color filter array (CFA)**
- This consists of 3 colours - Red, green and blue
Then it goes through the Imaging Sensor which does Sampling and Quantisation
Then post processing, which involves gamma correction / human made processes

![[Pasted image 20260116120552.png]]


# Image Sensor

The imaging sensor **converts light energy** into a **proportional electrical voltage**, corresponding to **pixel intensity**

![[Pasted image 20260116121043.png]]

The **sensor array** is an array of photo-sensitive devices; each sensor correspond to a pixel in the final image

There are two types:
- CCD: Charged coupled device
- CMOS: Complementary metal oxide semiconductor seonsors, which are sensitive to the light intensity


# Sampling
The real world is a **continuous space** but a digital image **is not**.

The sensor array is responsible for the **discretisation** of space, known as **sampling**.

The grid spacing **size** in the **sensor array** determines the **spatial resolution** of an image.

![[Pasted image 20260116121336.png]]

The denser the sensors, the finer the discretisation of space (the higher the resolution).

*Sampling in 1 Dimension*

Essentially, sampling is the conversion of a continuous signal into a discrete one.

![[Pasted image 20260116121615.png]]

By multiplying $f(t)$ by $comb(t)$, we receive a discrete sampling of the original function.

*Extending sampling to 2 Dimensions*

![[Pasted image 20260116121813.png]]

Here, we expand our $comb(x, y)$ function to 2 dimensions too.

This is a **non-invertible function**. Once we perform quantisation, we **lose that information**.

# Quantization

After passing the image through the image sensor and sampling, each value (sample) has a continuous value (color/intensity).
- Quantization is the process of discretisation of **intensity values** of the pixels.

![[Pasted image 20260116122028.png]]

The quantisation function takes as input $M$, the continuous set, and $N$, the **size** of the **discretised** set that we want. When we choose different values of $N$, we get different resulting images.

![[Pasted image 20260116122101.png]]

This is how running a quantisation function may look when doing a binary quantisation after sampling. In this case, we threshold: if greater than 0.5, we set to 1, otherwise we set to 0:

![[Pasted image 20260116122337.png]]

This is a **non-invertible function**. Once we perform quantisation, we **lose that information**.

Another example:
We use multiple-value quantisation, where the output intensities are sampled and grouped to the nearest discrete sample intensity.

![[Pasted image 20260116122613.png]]

To extend this to 2D, we do the above process for each line along the vertical height of the image.

![[Pasted image 20260116122844.png]]

The **location** of a pixel corresponds to **sampling**, and the **intensity** corresponds to the **quantisation**
# Colour filter array (CFA)
Before the Imaging sensor, there is a CFA which passes light of frequencies (corresponding to a colour) to pass through

![[Pasted image 20260116123223.png]]

The colour of an object depends on the **wavelength** of the light **reflected** by it

A **colour filter** passes light of a particular wavelength on to the imaging sensor
- Three types of colour filters corresponding to the three primary colours:
- Red, Green and Blue

![[Pasted image 20260116123325.png]]

To mimic human psychology, which has twice as many green light absorption cells as red or blue, we have **more green filters** than red or blue.
- This is called a **Bayer pattern**, and there are different Bayer patterns that different manufacturers use

# CFA Interpolation/de-mosaicing
So, we have R, G, B values at each pixel location. For viewing and further processing, we need to have three colours at each location.

CFA interpolation is the process of **recovering** the complete RGB colour channels from the Bayer pattern array.

![[Pasted image 20260116123557.png]]

**Linear interpolation**

$x$ is a location, $y$ is a value. Given the two locations and intensities, we could identify the **slope** at $y$. 

$$\frac{y-y_0}{x-x_{0}}=\frac{y_{1}-y_{0}}{x_{1}-x_{0}}$$
$$y=\frac{y_{0}(x_{1}-x)}{x_{1}-x_{0}}+\frac{y_{1}(x-x_{0})}{x_{1}-x_{0}}$$

![[Pasted image 20260116123638.png]]

We can extend this to 2 dimensions using **bilinear interpolation** 

![[Pasted image 20260116123826.png]]

# Gamma correction (post-processing)
Our eyes **do not** perceive light the same way that cameras do
- Cameras follow a "**linear**" relationship, i.e. between input light and output pixel intensity
- **Our eyes** however follow a **non-linear** relationship between the actual brightness and the perceived brightness.

Gamma correction accounts for this, which translates the actual luminance to "perceived" luminance according to our eye's light sensitivity

$$v_{(out)}=v^\gamma$$
![[Pasted image 20260116124348.png]]

![[Pasted image 20260116124359.png]]

When $\gamma$ is 1, nothing happens, since we receive just our original image. When it goes higher, we get a darker image. When it goes less than 1, we get a lighter image.

# Video acquisition
A video is a **series of images** captured at regular intervals

A standard video captures **30 frames per second**. This is called **frame rate**. Higher frame rate indicates a **higher temporal resolution**.

Specialised cameras can capture videos at a frame rate of >1,000s fps.

![[Pasted image 20260116124730.png]]

# Image Representation
## RGB and Grayscale images
RGB images are represented by a 3D array
- This can be converted in a 2D grayscale image

![[Pasted image 20260116124848.png]]

- $M, N$ specifies the pixel location. This is determined by **sampling** (q1).
- $L$ specifies the intensity of the pixel, or its value. This is determined by **quantization** (q2).

The most common way to represent an image is using 256 different intensity levels (8 bit image)
- The intensity values vary from 0 to 255, where 0 indicated **black** and 255 indicates **white**
- $L$ determines the **grayscale resolution**. The higher the value of $L$, the better the grayscale resolution.

![[Pasted image 20260116125436.png]]

The storage required to store an image of size (128 x 128) with 16 different intensity levels would be 128 x 128 x **4** bits, since $log_{2}(16)=4$
- The storage needed for an RGB image of same dimension would be **3** x 128 x 128 x 4 bits

