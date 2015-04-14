# Successive Over-relaxation System of Linear Equations Solver

Model Selection and Application
Successive Over-relaxation Method
This method for solving a system of linear equations closely mimics the Gauss-Seidel method. However, with successive over-relaxation, we have an additional weighting constant (named omega in my program) that can be set between the interval (0, 2). Note that the successive over-relaxation method is identical to the Gauss-Seidel method with the relaxation factor omega set to 1. In my selection of the value of omega, we set it to greater than 1 for speeding up convergence of a slow-converging process, while values less than 1 are used to help establish convergence of a diverging iterative process or speed up convergence of an overshooting process. This relaxation factor is defined as a symbolic constant in my program.

Systems of linear equations appear in the form Ax = b where A is an n x n matrix of the coefficients of the unknown variables of the 1 x n x matrix. b is the 1 x n matrix with the constants on the right hand side for each respective equation.

With the successive over-relaxation method implemented in my program, we start with an initial guess for our unknown variables all set to zeroes. Then we can calculate for X0 based on our initial guess. We compare this value with our previous guess of 0 to find the error (which will be 100%). We repeat this process for all the variables and compare it to the previously computed value iteratively until we find convergence. That is, when the error between the new and previously computed value is less than 1%. 

When convergence is reached, we have our 1 x n matrix with the computed values for the n unknown variables.

Model Selection and Application:
Shared Memory and Inter-process Communication
In our implementation, our shared memory block involves an array of the computed values for each variable that is iteratively computed based on the previous value until convergence is met. This required us to allow each computing process to access this shared memory using shm_open and mmap. Initially, this block of shared memory was created using shm_open and truncated to our desired size using ftruncate . Then by mapping it to our pointer, we could write to it.

By forking n processes, each solving for the ith variable, we could call each child running its own version of solveSystem to achieve our answer to the system of equations. We included the appropriate parameters for the A matrix input, the b matrix, and the parameter for Xi for the ith process solving for its appropriate variable.

Synchronization is needed to ensure that the current iteration for computing the values is completed before the next iteration is started. We accomplished this using wait(&status).
