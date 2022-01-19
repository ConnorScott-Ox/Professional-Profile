---
abstract: Immunohistochemistry (IHC) images are often used as a microscopic validation tool for MRI. Acquisition of MRI and IHC in the same ex-vivo tissue sample can enable direct correlation between MRI measures and purported sources of image contrast derived from IHC, ideally at the voxel level. However, most IHC analyses still involve manual intervention (e.g. setting of thresholds). Here, we describe an end-to-end pipeline for automatically extracting stained area fraction maps to quantify the IHC stain for a given microstructural feature. The pipeline has improved reproducibility and robustness to histology artefacts, compared to manual MRI-histology analyses that suffer from inter-operator bias.
authors:
- Amy FD Howard, Istvan N Huszar, Michiel Cottaar, Greg Daubney, Alexandre A Khrapitchev, Rogier B Mars, Jeroen Mollink, <b>Connor Scott</b>, Nicola Sibson, Adele Smart, Jerome Sallet, Saad Jbabdi, Karla L Miller
date: "2020-10-10"
doi: ""
featured: false
image:
  caption: 'Image credit: Daniel Kor'
  focal_point: ""
  preview_only: false
projects: []
publication: ''
publication_short: ""
publication_types:
- "2"
publishDate: "2020-10-10" 
summary: Here, we describe an end-to-end pipeline for the extraction of a histological metric from IHC stains to quantify a microstructural feature. We compare the pipeline's reproducibility and robustness to histology artefacts, relative to manual MRI-histology analyses.
tags:
- Source Themes
title: The microscopy connectome towards 3D PLI tractography in the BigMac dataset
url_code: ""
url_dataset: ""
url_pdf: 
url_poster: ""
url_project: ""
url_slides: ""
url_source: ""
url_video: ""
---
<b>Introduction:</b>

Comparison of MRI to stained tissue slides from the same sample provides insight into the cellular and molecular correlates of MRI signals, which are notoriously non-specific. Immunohistochemistry (IHC) is a histological staining technique that uses diaminobenzidine (DAB) to stain a target protein brown, and hematoxylin to counterstain background tissue purple. While many studies use IHC to validate MR measures, there is no standardised pipeline for extracting quantitative metrics from IHC slides, with most analyses requiring manual intervention1,2,3.

The stained area fraction (SAF) is a common metric that counts how many pixels in a digitised slide correspond to the protein targeted by DAB. SAF is quantified by separating hematoxylin and DAB stains using literature-based colour information 4,5,6 before setting a manual threshold for the DAB channel to segment microstructural tissue compartments from non-specific background staining4,5,7. These workflows suffer from issues that compromise robustness and reproducibility, including: inability to capture colour variation, poor robustness to histological artefacts, and inter-operator bias in setting the threshold. 

Here, we propose an automated pipeline that receives input RGB images and outputs a SAF map. Key features are: 1) a clustering approach to derive slide-specific colour information, 2) automated determination of the segmentation threshold, and 3) local thresholding to account for within-slide variations. We compare the pipeline’s reproducibility and robustness to previously reported histological quantification workflows.


<b>Methods:</b>

Pipeline overview: The pipeline consists of four steps (Figure 1). 1: Slide-specific RGB colour vectors for both stains are derived via k-means clustering (k=2) 9. 2: Stains are separated using colour deconvolution10 with non-negativity constraints. 3: The structure-specific DAB is segmented from non-specific background staining using an automatically determined threshold (methods outlined below). 4: SAFs, defined as the ratio of the positive pixels in the cell mask to the area outside of the tissue mask, are calculated in patches to form a map. 

Thresholding methods: We compared three thresholding methods for Step 3. All cases are based on Otsu’s method11 for separating foreground from background pixels. 1. Global thresholding estimates and applies a slide-specific threshold by applying Otsu to the slide’s DAB density histogram. 2. Local thresholding aims to account for variations in the DAB intensity across slides by adapting the threshold to local neighbourhoods. Local thresholds become more sensitive to histogram features, necessitating two refinements. First, we found Otsu performs better after pre-filtering the background stain with a Gaussian-2-inverse-gamma mixture model12. Second, Otsu fails when given histograms for ‘singleclass’ columns (background only); these columns are detected using a weighted object variance13, and a slide-wide default threshold is used. 3. Batchwise thresholding is the most common manual method, where a trained operator optimises a threshold by eye on multiple slides before applying it to the entire dataset. We simulate this by taking the median of all slides’ default thresholds. 

Acquisition and basic processing: We acquired an IHC dataset to test reproducibility, consisting of 26 slides from 7 human brains. For each brain, 3-4 adjacent slides (6 μm thick) were sectioned from the primary motor cortex (face region). Slides were stained for activated microglia (CD68) and then scanned with a digital scanner (2.5x2.5cm2;0.5μm/pixel). These images exhibited vertical intensity variations introduced by the background light intensity. Consequently, our local method applies Otsu to column-wise histograms (32-pixels wide). Adjacent slides from the same subject were co-registered8 and compared pixelwise to assess the SAF maps’ reproducibility, under the assumption that adjacent slides are similar at the SAF map resolution (128-512μm). 

Pipeline evaluation: The pipeline was evaluated for reproducibility and robustness. For all methods, absolute difference maps of SAF between co-registered adjacent slides were computed to evaluate reproducibility. Robustness to artefacts is measured with the coefficient of variation (COV, the standard deviation divided by the mean). Since our images exhibited strongest variation along the horizontal direction, we collapse the vertical dimension by calculating the column-wise average SAF before calculating the COV across this horizontal plot. We compare local thresholding to global and batch-wise thresholding using a COV ratio, where values >1 indicate reduced horizontal variation for the local method and an increased robustness to artefacts (both slow drift and vertical striping).

<b>Discussion and Results:</b>

Figure 2 visually compares stain separation with colour information derived from literature or the slide. 

Figure 3 shows the absolute difference maps for all pairwise comparisons using local thresholding. The adjacent SAF maps are generally similar, particularly within the white matter (~20% error). The largest errors are at tissue edges, suggesting residual misalignment that may bias comparisons. This indicates good reproducibility.

Figure 4 shows that the median percentage change was observed to be around 30% for all methods. The local method produces the lowest variance across all subjects, indicating a higher consistency in the difference of SAF maps. This suggests that the local method produces more alike SAF maps between adjacent slides. 

Figure 5 highlights the COV ratio comparing local thresholding with both batchwise (dashed) and global (solid) thresholding. This COV ratio is >1 for 20 out of 26 slides, indicating that the COV is reliably lower when using local thresholding. This suggests reduced impact of artefacts in SAF maps if a local thresholding method is used.

<b>Conclusions and Future Work:</b>

We have developed a fully-automated pipeline for generating SAF maps. Our results suggest some benefit in using local over global thresholds. Future works will consider other stains targeting myelin, iron and neurofilaments, and their pixelwise correlation with MRI data7.

<b>References:</b>
<b>Introduction:</b>

Multimodal datasets, which combine diffusion MRI (dMRI) and microscopy data that offers a ‘ground truth’ estimate of the connectome, provide a unique opportunity to both assess the performance of tractography and, crucially, drive algorithm design. To facilitate fair comparison, the multimodal data is ideally i) acquired in the same single brain with ii) whole brain coverage, iii) the microscopy and MRI data are co-registered together with high precision and iv) the microscopy is informative of the 3D connectome and not limited to 2D analysis (as is typical with many 2D microscopy techniques).

The BigMac dataset 1 is a novel resource that has the potential to fulfil these criteria. The BigMac dataset combines extensive MRI data (both in vivo and postmortem) with multi- contrast microscopy acquired throughout a single, macaque brain (Figure 1, criteria i&amp;ii). In this work we demonstrate precise co-registration of the MRI and microscopy data (criterion iii), and reconstruct the 2D microscopy fibre orientations as full 3D vectors in MRI space (criterion iv). This will facilitate future whole brain reconstruction of the 3D microscopy connectome at ultra-high spatial resolution. We expect this microscopy connectome to both provide new anatomical insight, and be a valuable resource for the diffusion modelling community. Upon publication, the BigMac data and tools will be made openly available.

<b>Methods:</b>

The BigMac dataset includes polarised light imaging 2,3,4 data (PLI, Figure 1 bottom) that estimates the primary fibre orientation of myelinated fibres per microscopy pixel. In BigMac, Coronal PLI data were acquired every 350 μm along the rostro-caudal axis, with an in-plane resolution of 4 μm per pixel. Here we take the PLI estimate of the fibre orientations within the microscopy plane, and approximate the through-plane ‘inclination’ angle with that from dMRI (the ball and stick model 5,6 ), to reconstruct 3D ‘hybrid dMRI-microscopy fibre orientations’ at the resolution of the microscopy data.

Diffusion MRI data (b=10ms/μm 2 , 1000 gradient directions, 1mm isotropic) were analysed using the ball and stick 5,6 (BAS) model to estimate fibre populations per voxel, with 50 fibre orientation estimates or ‘samples’ per population. The PLI images were co-registered to the dMRI data using an optimised TIRL 7 protocol (Figure 2). The in-plane angle was warped into the diffusion space (accounting for any necessary rotations) and compared to the BAS samples within the corresponding voxel. To facilitate fair comparison, the BAS samples were projected onto the microscopy plane. Samples from BAS fibre populations with signal fractions &lt;0.1 were excluded. Finally, the microscopy through-plane angle was approximated by that from the most similar BAS sample. This produced a hybrid dMRI-microscopy 3D fibre orientation per microscopy pixel. To have fibre orientations populate a substantial volume in dMRI space, the process was repeated for 30 consecutive PLI sections. The pixelwise fibre orientations were then combined into 3D fibre orientation distributions (FODs) for comparison with dMRI equivalents. Here, a set of voxels were defined in dMRI space at some specified resolution. In each voxel, the hybrid dMRI-microscopy fibre orientations populated a 3D ‘frequency histogram’ defined by 256 points evenly spaced across the sphere. Spherical harmonics of order 8 were then fitted to the normalised histogram. Because these FODs were output as spherical harmonic coefficients, the hybrid dMRI-microscopy FODs could then be visualised in standard MRI viewers 9,10 and input into existing tractography methods 10 .

For comparison, the dMRI data were also processed using constrained spherical
deconvolution (CSD) 8.

<b>Results:</b>

Figure 2 shows excellent registration of the 2D PLI images into the 3D MRI volume, with close alignment of the tissue boundaries.

In our method, the through-plane microscopy orientation is approximated by that from the BAS model. This assumption is most valid when the PLI and BAS fibre orientations in the microscopy plane are highly similar, as demonstrated throughout the majority of the white matter (Figure 3 top).

Figure 3 bottom shows example hybrid dMRI-PLI 3D fibre orientations from a single microscopy section. Near the microscopy plane (LR-IS), we retain high resolution information from the microscopy data (4x4 μm). The through-plane information is of coarser resolution, comparative to the MRI data.

The hybrid dMRI-PLI fibre orientations were then reconstructed into FODs at varying spatial resolutions (Figures 4&amp;5). Reassuringly, the hybrid dMRI-microscopy FODs show smoothly varying patterns in all three dimensions, but notably reconstruct fewer crossing fibre populations than that suggested by dMRI (Figure 4). Furthermore, we observe smoothly varying through-plane fibre orientations even when the hybrid FODs are reconstructed at a higher spatial resolution than the dMRI data (0.6 versus 1 mm, Figure 5). Finally, the hybrid FODs can be reconstructed at very high in-plane resolutions, up to the native microscopy resolution (4x4 μm in-plane, 350 μm though-plane).


<b>References:</b>
1. Howard AFD, et al. The BigMac dataset: ultra-high angular resolution diffusion imaging and
multi-contrast microscopy of a whole macaque brain, ISMRM 27th Annual Meeting, 2019.
2. Axer H, et al. Quantitative estimation of 3-D fiber course in gross histological sections of the
human brain using polarized light, Journal of Neuroscience Methods, 2001
3. Larsen L, et al. Polarized light imaging of white matter architecture, Microscopy Research
and Technique, 2007
4. Axer M, et al. High-Resolution Fiber Tract Reconstruction in the Human Brain by Means of
Three-Dimensional Polarized Light Imaging, Frontiers in Neuroinformatics, 2011
5. Behrens TE, et al. Characterization and propagation of uncertainty in diffusion-weighted MR
imaging. Magnetic Resonance in Medicine, 2003
6. Behrens TE, et al. Probabilistic diffusion tractography with multiple fibre orientations. What
can we gain?  NeuroImage , 2007.
7. Huszar IN, et al. Tensor Image Registration Library: Automated Non-Linear Registration of
Sparsely Sampled Histological Specimens to Post-Mortem MRI of the Whole Human Brain,
bioRxiv, 2019
8. Tournier JD, et al. Robust determination of the fibre orientation distribution in diffusion
MRI: non-negativity constrained super-resolved spherical deconvolution. Neuroimage, 2007
9. McCarthy P, FSLeyes, Zenodo, 2020
10. Tournier JD, et al. MRtrix3: A fast, flexible and open software framework for medical image
processing and visualisation. NeuroImage, 2019
11. Andersson JLR, et al. Non-linear registration, aka spatial normalisation, FMRIB technical
report TR07JA2, 2010