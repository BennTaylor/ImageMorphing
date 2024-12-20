# Image Morphing
This repository contains code for producing image morphing videos - a smooth transformation between two images. Relevant code files are:
- **utils.py:** Function for writing video file from frames.
- **feature_mapping_selector.ipynb:** Interactive widget functionality for click-based feature selection (preprocessing step for morphing). Detailed usage instructions are inside.
- **image_morphing.ipynb:** All computation related to morphing algorithm


## Usage
To create an image morphing the function DelaunayMorphing should be called. Inputs are the two images, the feature mapping, and the desired number of steps. More details are given in comments.

Feature mappings are a preprocessing step which is done manually. Code is provided to ease this process in *feature_mapping_selector.ipynb*. Detailed usage instructions are inside.

## Approach
This project implements image morphing via Delaunay Triangulation. Here is an outline of the steps:

Let $\text{im}_1$ refer to the initial image, and $\text{im}_2$ the final image.
1. ***Key feature mapping:***  Identify corresponding points in each image representing features of the depicted objects/scenes we intend to morph into each other (i.e. human pupil to dog pupil, outline of facial structure, etc.). This process is done manually via a GUI we made in the notebook.

2. ***Compute Delaunay Triangulation for $\textbf{im}_\textbf{1}$ then map it to a triangulation of $\textbf{im}_\textbf{2}$:*** In this step, we partition each image into triangles whose vertices are the key feature points. Crucially, we compute the triangles only for the first image, then extract a triangulation of the second by mapping the triangles to their corresponding vertices in $\text{im}_2$ according to the feature mapping. 

*The following steps are done for each frame of the resulting morphing animation.* 

3. ***Interpolate between the triangulations:*** According to the progress in the morphing, we compute a weighted average of $\text{im}_1$’s and $\text{im}_2$’s triangulations. This is easily done by computing the weighted average of the key feature vertices and then mapping the triangulation as before. Call this the intermediate triangulation (*IT*). In effect, this produces a smooth blend between the geometries of the images 

4. ***Project triangles to intermediate triangulation:*** In this step, we construct the morphed image triangle by triangle. For each triangle in $\text{im}_1$ we compute the affine transformation mapping its vertices to those of its correspondent in the *IT*. Importantly, this transformation has the effect of preserving the interiors of the triangles. The triangle is then projected to the morphed image’s plane based on the transformation. The same is done for $\text{im}_2$.

5. ***Interpolate between projections:*** According to the progress in the morphing, we compute a weighted average of the pixel content of the two projections from the previous part. In effect, this produces a smooth blend between the colors/textures of the two images.

Once all the intermediate images are computed from these steps, a video can be made giving a smooth morphing from $\text{im}_1$ to $\text{im}_2$ (assuming a sufficient number of frames and quality of key feature mapping).

## Sources and Implentation Details
Here is a breakdown of the packages and resources utilized for specific parts of the implementation:

- Overview and motivation for technique from UToronto lecture slides: [UToronto slides](https://www.cs.toronto.edu/~guerzhoy/320/lec/morphing.pdf)
- More detailed algorithm description for image morphing from Delaunay triangulation: [FaceMorphing](https://devendrapratapyadav.github.io/FaceMorphing/ )
- Math reference for computing affine transformation between triangles: [math.stackexchange](https://math.stackexchange.com/questions/1092002/how-to-define-an-affine-transformation-using-2-triangles)
- Code reference for isolating points within boundary of a polygon (efficiently): [stackoverflow](https://stackoverflow.com/questions/21339448/how-to-get-list-of-points-inside-a-polygon-in-python)
- <u>OpenCV Package</u> used to compute Delaunay Triangulation (cv2.Subdiv2D). Also for image warping and processing files. 
- <u>Matplotlib Package</u> used for visualization tools. Also used to extract points inside a triangle (matplotlib.Path).
- <u>ChatGPT</u> used throughout for NumPy array manipulation and resolving small (mostly type-error related) bugs. Also used to create interactive widget functionality for feature mapping. 
