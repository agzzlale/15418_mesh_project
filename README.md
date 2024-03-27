# The Application of CUDA and openMPI in Mesh Simplification 
Alejandro Gonzalez Lazaro (alejand2) and Majd Almudhry (malmudhr)

## URL:


## SUMMARY:

We will implement the mesh simplification algorithm by Michael Garland and Paul Heckbert on our personal multicore processors and GPUs. We will approach this algorithm in two ways, one by subdividing the mesh into sections that are each cost optimized separately to decrease communication at the expense of a more expensive simplification, and another where all the edges are ordered and then distributed across parallel units to achieve smaller simplification cost at the expense of communication.

## BACKGROUND:

The project is about using a mesh simplification algorithm developed at CMU by Michael Garland and Paul Heckbert. From the 15462 assignment, the pseudo code for the algorithm looks like the following:

Compute quadrics (a matrix whose details are in the webpage listed in the resources) for each face.
Compute an initial quadric for each vertex by adding up the quadrics at all the faces touching that vertex. 
For each edge, create an Edge_Record (a data structure) and add it to one global queue.
Until a target number of triangles is reached, collapse the best/cheapest edge (as determined by the queue) and set the quadric at the new vertex to the sum of the quadrics at the endpoints of the original edge.

We are thinking of parallelizing the removal of edges and also any initialization of data structures like the Edge_Record. Here is a picture of a mesh before (left) and after (right) the algorithm is applied. Since meshes have large amounts of vertices, edges and faces and the computations on each of these are the same and only deal with nearby elements, it can be useful to parallelize these computations whenever we find independent points of the mesh to make use of potential locality and lack of thread divergence. The trickier part would be to still keep part of the global ordering of edges by quadric cost across our parallel units to achieve a result that not only converges faster but does not sacrifice cost too significantly.

![Meshes](/docs/assets/meshessimple.png)

Credit: https://github.com/CMU-Graphics/Scotty3D/blob/main/assignments/A2/simplify.md

## THE CHALLENGE: 

### Workload:
In the simplification stage, removing edges that are close to each other in the mesh is a challenge because we need to remove them from the queue and mesh to recompute the cost and this can cause conflicts between parallel units that touch nearby elements. 
Knowing when to synchronize changes to the mesh and edge queue so that we perform our per thread calculations and modifications with the most recent data, since we need to communicate up to date data/existence of edges to dependents.
How to distribute the priority queue / update queue without sacrificing a low cost simplification result.
The way the mesh is currently implemented in Scotty3D may be low locality due to it being implemented with pointers. This data structure can be another parameter to optimize as we could find ways to make meshes store neighbor components (faces, half edges, other vertex) data near each other.
The way the assignment is written, a dictionary is used to keep track of vertex K matrices and this might not have good locality.
The good part of the algorithm is that the operations to be performed per vertex/ edge/face are the same, it is mostly the data dependencies and ordering that can be challenging to manage.
### Constraints:
The final mesh should be a valid halfedge mesh structure and any modifications to this structure need to be handled with caution especially in parallel.
We want to achieve a low-cost simplification which will prove tricky with potential staleness across parallel units of execution.


## RESOURCES:

We will start from the 15462 Scotty3D repo which includes starter code with the mesh data structures and a way to load and visualize meshes https://github.com/CMU-Graphics/Scotty3D. 
	
We will follow the 15462 assignment specifications for mesh simplification found here: https://github.com/CMU-Graphics/Scotty3D/blob/main/assignments/A2/simplify.md for the general algorithm and also reference the paper by Michael Garland and Paul Heckbert on this algorithm https://www.cs.cmu.edu/~./garland/quadrics/quadrics.html 
	
We also anticipate CUDA parallelization to be trickier so we might need some more resources on algorithms like graph coloring to reduce conflicts when parallelizing a mesh on a GPU. We would like to perform our own optimizations first to align with the Garland-Heckbert algorithm, but potentially some ideas from the paper “High-performance simplification of triangular surfaces using a GPU” (https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8341488//) might be useful.


## GOALS AND DELIVERABLES:

### Plan to achieve:
Achieve near linear speedup with an implementation using CUDA where our axis of parallelization across non conflicting edges (kind of with graph coloring). We are saying near linear because we think that using this implementation we will minimize communication time and maximize the computation to communication ratio. We expect the mesh quadric cost to be higher than the sequential version but this depends on the distribution of cost in the mesh itself.
Achieve near linear speedup with an implementation using MPI where our axis of parallelization is across groups of neighboring edges (by partitioning the mesh into non-conflicting sections)
Compare and analyze our results using the two different programming models 


### Hope to achieve:
Implement the same algorithm but with a different axis of parallelization that depends on the global cost of the edges instead of the cost of edges per parallel section, to achieve a lower overall cost. This will be implemented in either CUDA or MPI or both depending on how much time we have left. 

### What to demo:
We plan to show our speedup graphs, cost-speedup comparison graphs, and also some pictures of the 3D images before and after the simplification. We will test our algorithms with different types of meshes (spheres, cows, etc) to show an analysis of how the shape affects the cost/speedup balance.


## PLATFORM CHOICE:

We are planning to run our programs locally. We will use it on a laptop with a 3072 CUDA core 2080 Super Max-Q for the CUDA implementation and a 8 core M1 Mac. 

It makes sense to run these algorithms on our local devices as these operations can be important for 3D rendering applications that would be run in user’s personal devices (3D modeling, scaling of objects with distance in a video game, etc). We would like to experiment with how this can be done efficiently using our personal hardware.


## SCHEDULE: 

Week 0 (March 24-31):
Get the Scotty 3D build system compiling CUDA and MPI (attempt on GHC if possible) and start sequential implementation.

Week 1 (April 1-7):
Finish sequential implementation and sketch out the algorithms for both CUDA and MPI and start implementing them.

Week 2 (April 8-14):
Finish the parallel Edge Record Queue generation (including any data structure reworking for better locality) for the first implementation.

Week 3 (April 15-21):
Finish the parallel Edge Removal loop for the first implementation.

Week 4 (April 22-28):
Finish first implementation if behind or move on to parallelization that depends on cost per edge on the chosen platform (either MPI or CUDA depending on our previous results).

Week 5 (April 28-May 5):
Optimizing the second implementation, collecting all data, writing the report and making the poster.

