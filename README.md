# The Application of CUDA and openMPI in Mesh Simplification 
Alejandro Gonzalez Lazaro (alejand2) and Majd Almudhry (malmudhr)


## SUMMARY:

We will implement the mesh simplification algorithm by Michael Garland and Paul Heckbert on our personal multicore processors and GPUs. We will approach this algorithm in two ways, one by subdividing the mesh into sections that are each cost optimized separately to decrease communication at the expense of a more expensive simplification, and another where all the edges are ordered and then distributed across parallel units to achieve smaller simplification cost at the expense of communication.

![Meshes](/docs/assets/meshessimple.png)

Credit: https://github.com/CMU-Graphics/Scotty3D/blob/main/assignments/A2/simplify.md

## SCHEDULE:
Week 0 (March 24-31):
Get the Scotty 3D build system compiling CUDA and MPI and start sequential implementation.

Week 1 (April 1-7):
Finish sequential implementation 

Week 2 (April 8-14):
Sketch out the algorithms for both CUDA and MPI and start implementing them.
Data structure reworking 
MPI Partitioning

Week 3.1 (April 15-17):
Implement edge record pass for MPI  (Alejandro)
Start implementing the “relaxed priority queues” for CUDA (Majd) 

Week 3.2 (April 18-21):
Implement collapsing pass for MPI (Alejandro)
Finish implementing the “relaxed priority queues” for CUDA (Majd) 

Week 4.1 (April 22-24):
Implement gather and rebuild from partitions for MPI (Alejandro)
Implement the locking part for CUDA (Majd)

Week 4.2 (April 25-28):
MPI alternate mesh partitioning with KD-trees (Alejandro)
Partitioning algorithm with CUDA (Majd)

Week 5.1 (April 28-May 1):
Optimization/Data collection CUDA  (Majd)
Optimization/Data collection MPI (Alejandro)
Edge collapsing for partitioning algorithm  (Majd & Alejandro)

Week 5.2 (May 2-May 5):
Poster and report  (Alejandro & Majd)

## PROPOSAL URL:
https://docs.google.com/document/d/13vQRTEH8dcyKOkdxwu2RBkG24wPOiYFiHQDjRZhcpcI/edit?usp=sharing

## PROJECT MILESTONE REPORT URL:
https://docs.google.com/document/d/1yHcyYbR6AOp56O6Ecy0u2nk4vsgWCoEUkRCMBGLWxfk/edit?usp=sharing

