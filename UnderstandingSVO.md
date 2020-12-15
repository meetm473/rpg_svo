# Understanding SVO
> This Markdown file contains short notes, drafted to better understand the SVO algorithm.

**Original Paper |** SVO: Fast Semi-Direct Monocular Visual Odometry <br>
**Authors 	|** Christian Forster, Matia Pizzoli, Davide Scaramuzza <br>
**Institution |** University of Zurich, Robotics & Perception Group <br>
**Paper published @** ICRA-2014

---
### Abstract
> *We propose a semi-direct monocular visual odom-
etry algorithm that is precise, robust, and faster than current
state-of-the-art methods. The semi-direct approach eliminates
the need of costly feature extraction and robust matching
techniques for motion estimation. Our algorithm operates
directly on pixel intensities, which results in subpixel precision
at high frame-rates. A probabilistic mapping method that
explicitly models outlier measurements is used to estimate 3D
points, which results in fewer outliers and more reliable points.
Precise and high frame-rate motion estimation brings increased
robustness in scenes of little, repetitive, and high-frequency
texture. The algorithm is applied to micro-aerial-vehicle state-
estimation in GPS-denied environments and runs at 55 frames
per second on the onboard embedded computer and at more
than 300 frames per second on a consumer laptop. We call our
approach SVO (Semi-direct Visual Odometry) and release our
implementation as open-source software.*

### Introduction
Algorithms estimating motion through camera output can be divided into 2 categories:
1. Feature-based methods:
	- Extract sparse set of salient features on image -> Match the features on successive frames -> Recover camera motion -> Optimize output
	- Use descriptors
	- Drawback: detection and matching thresholds; feature detectors rely on speed rather than precision; need multiple measurements to average to final value
	- Popular VO algos are: PTAM (designed for AR)
2. Direct methods:
	- Estimate structure and motion directly from intensity values
	- Local intensity gradient magnitude and direction is used for optimization
	- Utilize all the information present in the image
	- Drawback: Computation of photometric error is more intensive than the reprojection error, as it involves warping and integrating large image regions.

### What SVO does?
- Performs feature extraction only when a keyframe is selected to initialize new 3D points
- Uses many small patches on an image
- > A sparse model-based
image alignment algorithm for motion estimation is used. We demonstrate that sparse information of depth is sufficient to get a rough estimate of the motion and to find feature-correspondences.
- > As soon as feature correspondences and
an initial estimate of the camera pose are established, the
algorithm continues using only point-features; hence, the
name “semi-direct”.
- The algorithm uses two parallel threads:
	1. Estimating the camera motion:
		1. *Pose initialisation* via sparse model-based image alignment - the camera pose relative to the previous frame is found through minimizing the photometric error between pixels corresponding to the projected location of the same 3D points.
		2. The 2D coordinates corresponding to the reprojected points are through alignment of the corresponding feature-patches.
		3. Refining the pose and the structure through minimizing the reprojection error introduced in the previous feature-alignment step.
	2. Mapping as the environment is being explored (Will be dealt with later)

### Other documentations to learn better
- [Link 1](https://github.com/Adu143/FINken-EYE)
- [Tutorial on VO, part 1](http://www.eng.auburn.edu/~troppel/courses/7970%202015A%20AdvMobRob%20sp15/literature/vis%20odom%20tutor%20part1%20.pdf)
- [Tutorial on VO, part 2](https://www.zora.uzh.ch/id/eprint/71030/1/Fraundorfer_Scaramuzza_Visual_odometry.pdf)
- [PPT explaining VO and SVO](http://mrsl.grasp.upenn.edu/loiannog/tutorial_ICRA2016/VO_Tutorial.pdf)
- 