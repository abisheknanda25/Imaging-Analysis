
#  Medical Image Analysis Project — Report

##  Introduction
Medical imaging plays a major role in diagnosis, treatment planning, and disease monitoring.  
This project focuses on:

- Reading 3D MRI and CT volumes in **NIfTI** format  
- Visualizing slices and segmentation masks  
- Performing basic statistical analysis between full images and segmented structures  
- Exploring segmentation validity using histograms and intensity properties  

This analysis provides insight into anatomical regions and supports future applications like automated segmentation and classification.

---

##  Data Description
The dataset contains MRI and CT scans from different anatomical regions:

| Region | Modality | Includes Segmentation Mask |
|--------|----------|--------------------------|
| Hippocampus | MRI | ✔ Yes |
| Heart | MRI | ✔ Yes |
| Prostate | MRI | ✔ Yes |
| Abdomen | CT | ✔ Yes |

Format: **NIfTI (.nii or .nii.gz)**  
Dimensionality: **3D volumes**, composed of multiple 2D slices  
Masks: **Binary** images marking anatomical structures

Data folder structure example:
```
medical_images/
 ├─ hippocampus/ (img.nii.gz, mask.nii.gz)
 ├─ heart/
 ├─ prostate/
 └─ abdomen/
```

---

## ❓Questions & Answers with Code

###  Q1: How to load and visualize medical images?

```python
import nibabel as nib
import matplotlib.pyplot as plt

img, _, _ = load_nifti("/content/drive/MyDrive/medical_images/hippocampus/img.nii.gz")
mask, _, _ = load_nifti("/content/drive/MyDrive/medical_images/hippocampus/mask.nii.gz")

slice_index = img.shape[2] // 2

plt.imshow(img[:, :, slice_index].T, cmap='gray', origin='lower')
plt.title("MRI Slice")
plt.show()
```
**Answer**: MRI/CT slices and segmentation overlays clearly show anatomical structures.

---

###  Q2: What are the intensity statistics of the whole image vs segmented region?

```python
import numpy as np

whole_stats = compute_stats(img[~np.isnan(img)])
seg_stats = compute_stats(img[mask > 0])

print("Whole image:", whole_stats)
print("Segmented region:", seg_stats)
```
**Answer Summary**
- Segmented region has **more homogeneous** intensities
- Standard deviation is smaller in segmentation → **consistent structure**
- Mean intensity differs → segmentation isolates a distinct tissue type

---

### Q3: How do the intensity distributions compare?

```python
plt.figure(figsize=(10,5))
plt.hist(img.flatten(), bins=100, alpha=0.5, label='Whole image')
plt.hist(img[mask>0], bins=100, alpha=0.5, label='Segmented region')
plt.legend()
plt.title('Intensity Histogram')
plt.show()
```
**Answer**: The segmented mask forms a narrower histogram → reduced intensity variability → segmentation valid.


## Q4: Is the segmentation valid? (Basic check)

```python
seg_voxels = img[mask > 0]
coverage_percent = len(seg_voxels) / img.size * 100
print("Mask coverage:", coverage_percent)
```
 **Answer**:  
The percentage of voxels in the mask falls within a realistic range for an organ — segmentation appears reasonable.

> Full validation would require Dice coefficient using ground truth vs prediction.


##  Optional Advanced Analysis (Attempted)
- 3D max-intensity projections for anatomical views
- Otsu threshold segmentation for comparison (experimental)
- Future scope: classification, registration, deep learning segmentation


##  References
- Nibabel Documentation — https://nipy.org/nibabel/
- Matplotlib Documentation — https://matplotlib.org/
- NumPy Documentation — https://numpy.org/
- NIfTI Format — https://nifti.nimh.nih.gov/
- Dataset Provided by Instructor

---

