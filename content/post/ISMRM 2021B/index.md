---
abstract: The BigMac dataset is a unique, to be open access resource that includes extensive MRI and densely sampled microscopy data acquired in a single, whole macaque brain. However, the microscopy data currently only informs on the fibre orientations in 2D, precluding 3D reconstruction of the microscopy connectome. Through precise co-registration and joint modelling of the diffusion MRI and microscopy data, this work aims to reconstruct the microscopy fibre orientations in 3D. This will facilitate future determination of the ultra-high resolution, whole brain, microscopy-inspired connectome, which we expect will provide neuroanatomical insight, as well as play a vital role in validating and advancing techniques for in vivo tractography.
authors:
- Amy FD Howard, Istvan N Huszar, Michiel Cottaar, Greg Daubney, Alexandre A Khrapitchev, Rogier B Mars, Jeroen Mollink, <b>Connor Scott</b>, Nicola Sibson, Adele Smart, Jerome Sallet, Saad Jbabdi, and Karla L Miller
date: "2020-10-09"
doi: ""
featured: false
image:
  caption: 'Image credit: Amy Howard'
  focal_point: ""
  preview_only: false
projects: []
publication: ''
publication_short: ""
publication_types:
- "2"
publishDate: "2020-10-09" 
summary: The BigMac dataset is a unique resource that includes extensive MRI and densely sampled microscopy data acquired in a single, whole macaque brain. However, the high-resolution microscopy currently only informs on the fibre orientations in the 2D plane of sampled slides, precluding 3D reconstruction of the microscopy connectome. Here we use precise co-registration and joint modelling of diffusion MRI and polarised light images to reconstruct the microscopy fibre orientations in 3D. This will facilitate future determination of the whole brain, microscopy-inspired connectome, which we expect will provide neuroanatomical insight, and play a vital role in validating and advancing invivo tractography.
tags:
- Source Themes
title: The microscopy connectome: towards 3D PLI tractography in the BigMac dataset
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