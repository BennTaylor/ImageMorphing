# Image Morphing
This repository contains code for producing image morphing videos - a smooth transformation between two images. Files containing code are:
- utils.py: includes function for writing video file from frames.
- feature_mapping_selector.ipynb: includes interactive widget functionality for click-based feature selection (preprocessing step for morphing). Detailed usage instructions are inside.
- image_morphing.ipynb: includes all computation related to morphing algorithm(s)


## image_morphing.ipynb
This file is broken up into sections.

- Method 1 - Cross Dissolve: the function cross_dissolve computes the basic time-linear interpolation between images. Also with homography where we first project images of the same scene into the same plane.
- Mehod 2 - Image morphing with Delaunay Triangulation: This is the real morphing algorithm. To create an image morphing the function DelaunayMorphing should be called. Inputs are the two images, the feature mapping, and the desired number of steps. More details are given in comments.