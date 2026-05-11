Example of application of compression techniques in forensics:

![[Pasted image 20260213121009.png]]

Raw images are ready for viewing in the RGB colour space.
- If we store the image in a file with **no compression**, we get a **TIFF** file
In practice, it's not very common to store uncompressed images
- Images with digital cameras are natural photographic images with high amounts of **redundancy**
- **Lossy** or **lossless** compression is usually applied
- **Lossless** image compression algorithms **are reversible**
- **Lossy** algorithms allow some of the **original image data** to be **discarded**

![[Pasted image 20260213121207.png]]

**Compression** is the process of **reducing** the amount of **bits required** to **store/represent** some given information
- Consider two set representations of the **same information**: $A$ and $B$
$A$ uses $n_{1}$ bits and $B$ uses $n_2$ bits, the **compression ratio $c_r$** is defined as:
$$c_{r}=\frac{n_{1}}{n_{2}}$$

The **data redundancy** is defined as:
$$r_{d}=1-\dfrac{1}{c_{r}}$$
**Compression algorithms** attempt to **remove redundancy** in images

- **Spatial redundancy**: 
	- Pixel values are **highly correlated** with their **neighbouring pixels**.
	- Correlations also exist on a **higher level** with **repeating patterns** and **structures**
	- ![[Pasted image 20260213121838.png]]
- **Psychovisual redundancy**:
	- **Details** in images that **our** visual system **cannot see**
	- e.g. image contents corresponding to the **high frequency coefficients** of the 2D DCT
		- (some of them - e.g. edges are high frequency and we want to preserve them, however some noise is high frequency and we want to get rid of that)
	- ![[Pasted image 20260213121937.png]]
- **Coding redundancy**:
	- Using **more bits** *per pixel* than needed
		- ![[Pasted image 20260213122041.png]]
		- This image has 4 different gray levels, so representing it using an 8-bit image will introduce coding redundancy (can use 2-bit instead)

---

The image compression **steps** are as follows:
- Original
- **Transform mapping**
	- Removes *spatial redundancy*
- **Quantizer**
	- Removes *psychovisual redundancy*
- **Entropy Coding**
	- Removes *coding redundancy*
- Compressed

During *decompression* we go the **other way around**

---

**JPEG** compression

- JPEG is **lossy** compression, which means every time you compress an image using JPEG, some **information** will be **lost**

Steps in JPEG compression:

- **Colour space conversion**
	- Convert from RGB to YCbCr
		- Y: **Luminance** channels
		- CbCr: **Colour** channels
	- The basic idea is to **separate** the **intensity** and **colour** components, so that the colour and luma channels can be compressed differently.
	- The **resolution** of Cb and Cr can then be reduced by a factor of 2 or more by **chroma subsampling**
		- Lets us achieve higher image compression without losing visual quality, since humans are less sensitive to colour details than intensity details
- **Division into subimages**
	- An image is usually **divided** into non-overlapping blocks, and **encoding** is done on **each** block **independently**
	- The image blocks is called a **macroblock** or **minimum encoded unit**
	- In general, both the level of compression and computational complexity increase as the block size increases. 
		- The most popular block sizes are $8\times8$ or $16\times16$
- **Discrete Cosine Transform**
	- ![[Pasted image 20260213123207.png]]
	- ![[Pasted image 20260213123214.png]]
	- We can then do DCT mapping per-block
	- ![[Pasted image 20260213123646.png]]
- **Quantization**
	- This is the **main lossy step** in JPEG
	- **Each** DCT coefficient is **quantized** by a pre-defined **factor**
	- ![[Pasted image 20260213123739.png]]
	- ![[Pasted image 20260213124025.png]]
	- Simple example:
	- ![[Pasted image 20260213124039.png]]
	- However, when we try to convert a quantized image **back** to the original through de-quantisation (multiplying the components back through by the quantisation factor), since we initially rounded them, we lose information on the way back.
	- ![[Pasted image 20260213124309.png]]

So, using DCT removes spatial redundancy
- But only a few DCT bases can represent **most** of the information

After performing quantization and removing psychovisual redundancy, we get lots of entries with 0's in them. This shows that we have lots of **coding redundancy** here now.
- We can use **huffman coding** to remove such reduancy

![[Pasted image 20260217141015.png]]

We can put the coefficients into a **row vector** in the order shown in the image (top left down to bottom right)
- We can observe that sometimes these values fall into a smaller **range** than that of 0-255

We can calculate the **Probability Mass Function** of the pixel values in a quantized image.

![[Pasted image 20260217141531.png]]

We assign codewords to elements that occur at highest probability, in order of probability

Example:

![[Pasted image 20260217142057.png]]

We need to make sure that each message is **uniquely decodable**
- Since Huffman coding is **prefix-free**, it is uniquely decodable
- Therefore each word will be unique and possible to decode without ambiguity

By using **variable length encoding**, we save a lot of storage space
- This is helped by using the probabilities to assign smaller length encoded words to more popular letters/symbols

---

Is this the best we can do?
To find this out, we introduce **Entropy Coding**

 Entropy coding is a **lossless** compression technique applicable to **any data**
 - Entropy is a fundamental concept in information theory
 - It is a **measure** of **information**

Shannon proposed to measure information in terms of **uncertainty** or **randomness** in data

![[Pasted image 20260217143607.png]]

For multiple events, we can measure like this (rather than just binary case)

![[Pasted image 20260217143752.png]]

![[Pasted image 20260217144433.png]]

So the higher quality level we go, the better huffman coding does compared to the minimum threshold (entropy), and compared to fixed length coding (would be 4b/s here)

