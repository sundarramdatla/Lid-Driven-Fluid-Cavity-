# Implementing Chorin's Projection Method

This simulation serves as a benchmarking test on correct implementation of Chorin's projection method and Jacobi's iterative process to solve the 2D incompressible Navier-Stokes equation for a simple problem such as a lid-driven cavity.
The problem involves simulating a fluid that is initially at rest everywhere (i.e fluid velocity is [0, 0] at every mesh point) except for the top boundary, which has a boundary condition of 1.0 m/s to the right (u = [1,0] at every mesh point along the top boundary).
Such a problem is a generic benchmarking test for numerical methods and results match the literature in computational fluid dynamics: https://www.openfoam.com/documentation/tutorial-guide/2-incompressible-flow/2.1-lid-driven-cavity-flow

To summarize the numerical method being implemented here: 
Chorin's projection method is a way to solve the incompressible Navier-Stokes equation by means of Hemholtz-Hodge decomposition of the fluid vector field into the sum of a divergence-free (solenoidal) and irrotational fluid field.
The irrotational field can be written as the gradient of a known scalar field, in this case the pressure field, and thus taking the divergence will yield us a Poisson equation involving the pressure field.
The divergence-free field, which we desire to satisfy the continuity equation of Navier Stokes, is thus obtained by solving the Poisson equation for the pressure and performing the 'pressure correction step' of subtracting the pressure gradient from the intermediary fluid velocity field we get from operator splitting the Navier-Stokes equation.

To solve the pressure Poisson equation, Jacobi's iterative method is utilized, which decomposes a matrix into a sum of its diagonal, lower triangular, and upper triangular submatrices.
This can be performed element wise on the pressure field in the Poisson equation, which achieves convergence within 50 iterations as used in this simulation.

Greater visual detail of the algorithm used can be found in my research paper on hydroenergy through vortex induced vibrations: https://archive.org/details/energy-harvesting-with-vortex-induced-vibrations-archive

The computational domain used is a 1x1 grid of 41x41 mesh points with 500 time step iterations and 50 iterations used for solving Poisson's euations. 
Kinematic viscosity is 0.1, density is 1.0, and a time step length of 0.001 is used. The standard approach to finite difference is used to discretize all partial derivatives and the Laplacian operator.

