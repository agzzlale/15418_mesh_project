# The Application of CUDA and openMPI in Mesh Simplification 
Alejandro Gonzalez Lazaro (alejand2) and Majd Almudhry (malmudhr)


## SUMMARY:

We will implement the mesh simplification algorithm by Michael Garland and Paul Heckbert on our personal multicore processors and GPUs. We will approach this algorithm in two ways, one by subdividing the mesh into sections that are each cost optimized separately to decrease communication at the expense of a more expensive simplification, and another where all the edges are ordered and then distributed across parallel units to achieve smaller simplification cost at the expense of communication.

![Meshes](/docs/assets/meshessimple.png)

Credit: https://github.com/CMU-Graphics/Scotty3D/blob/main/assignments/A2/simplify.md

## PROPOSAL URL:
https://docs.google.com/document/d/13vQRTEH8dcyKOkdxwu2RBkG24wPOiYFiHQDjRZhcpcI/edit?usp=sharing

