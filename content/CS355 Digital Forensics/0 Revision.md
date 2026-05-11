Sampling & Quantisation
- Which process in the imaging device determines $M,N$?
	- Sampling
- What about $L$?
	- Quantisation

CFA Interpolation (Post processing)
- Bayer pattern, bilinear interpolation process
Gamma correction, remember formula, motivation and properties

Y (luminosity), Cb, Cr (colour spaces)
- Chroma subsampling - schemes
- Describe what is A, b, c in A : b : c
- Be able to calculate subsampled image using given scheme

Comparing images - SSIM and its properties
- Less sensitive to image distortions:
	- Luminance comparison
	- Contrast comparison
	- Structural comparison (cross correlation)
- On *patch* pairs

---

Gamma correction vs Histogram equalisation
- PMF - Probability mass function

Does the size of the image matter for histogram matter?
- No - just need the reference CDF
- But the intensity - value range (grayscale resolution) - matters

Gaussian noise - mean filter
- Averaging reduces noise, but also causes information loss (blurry)
Impulse noise (salt & pepper) - median filter

---

Fourier spectrum - know where are the zero or low frequency signals
- How to use low-pass filter for noise removal

Notch filters - repetitive patterns

DCT - should be able to calculate the corresponding DC component (average)

