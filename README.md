# Lab 6: Mini Project – Image Processing Pipeline

## Goal
Combine all DSP-image concepts—filtering, frequency, edges, and enhancement—into one small pipeline.

---

## 1) Load your own image

**Code snippet:**
I = im2double(rgb2gray(imread('your_image.jpg')));

yaml
Copiar código

**Explanation:**
A custom image (object, face, landscape, etc.) is loaded and converted to grayscale for simplicity.  
This step defines the input of the processing pipeline.

**Image shown:** Original input image (included in the repository).

---

## 2) Pre-process: noise removal

**Code snippet:**
I_filt = medfilt2(I,[3 3]);

yaml
Copiar código

**Explanation:**
A *median filter* is applied to remove impulse noise while preserving edges.  
This stage corresponds to spatial-domain filtering for denoising.

**Image shown:** Denoised version of the original image (included in the repository).

---

## 3) Enhance contrast

**Code snippet:**
I_enh = imadjust(I_filt,[0.2 0.8],[0 1]);

yaml
Copiar código

**Explanation:**
Contrast enhancement stretches the intensity range of the image, making details more visible.  
This represents an image enhancement operation commonly used before feature extraction.

**Image shown:** Enhanced image with improved contrast (included in the repository).

---

## 4) Extract features (edges or frequency)

**Code snippet:**
edges = edge(I_enh,'Canny',[0.1 0.25]);

yaml
Copiar código

**Explanation:**
Edges are extracted using the *Canny detector*, which applies Gaussian smoothing, gradient calculation, and hysteresis thresholding.  
This step highlights the structural features of the image.

**Image shown:** Edge map generated from the enhanced image (included in the repository).

---

## 5) Optional frequency-domain mask

**Code snippet:**
F = fftshift(fft2(I_enh));
[M,N]=size(F);
[u,v]=meshgrid(-N/2:N/2-1,-M/2:M/2-1);
H = double(sqrt(u.^2+v.^2)<60);
I_lp = real(ifft2(ifftshift(F.*H)));

yaml
Copiar código

**Explanation:**
This optional step performs low-pass filtering in the *frequency domain* using a circular mask.  
High frequencies (fine details or noise) are attenuated, and the result is reconstructed with an inverse FFT.  
This demonstrates the link between spatial and frequency-domain processing.

**Image shown:** Low-pass filtered (LP) result (included in the repository).

---

## 6) Visualization

**Code snippet:**
figure; montage({I, I_filt, I_enh, edges, I_lp},'Size',[1 5]);
title('Original | Denoised | Enhanced | Edges | LP result');

yaml
Copiar código

**Explanation:**
All stages of the processing pipeline are displayed side by side for comparison.  
This montage shows the progressive transformation from raw image to the final processed result.

**Image shown:** Combined view of all steps (included in the repository).

---

## 7) Report

**Describe your processing pipeline:**  
The pipeline includes noise reduction, contrast enhancement, edge extraction, and frequency-based filtering.  
Each stage builds upon the previous one to improve clarity and extract relevant features.

**Explain how each stage relates to DSP operations:**  
* Median filtering * → spatial-domain smoothing and noise suppression.  
* Contrast adjustment * → point-wise intensity transformation.  
* Canny edges * → gradient-based detection (high-frequency emphasis).  
* Frequency mask * → low-pass filtering in the frequency domain (high-frequency attenuation).

**Discuss improvements or limitations:**  
* The pipeline effectively enhances and extracts details while reducing noise.  
* However, fixed parameters (thresholds, filter size) may not adapt well to all images.  
* Adaptive methods could further improve edge sharpness and noise suppression.
