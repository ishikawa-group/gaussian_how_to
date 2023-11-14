# Molecular Dynamics
## Background
* In this lecture, we will see the molecular dynamics (MD) and how to do it with Gaussian.

## Hellmann-Feynman theorem
$$
\frac{\partial}{\partial\lambda}\braket{\Psi|\hat{H}|\Psi}
=\braket{\Psi|\frac{\partial\hat{H}}{\partial\lambda}|\Psi}
$$
* For wave functions that are not completely optimized with respect to all parameters (in most cases!), the Hellmann-Feymann theorem does not hold,, so the left-hand-side and right-hand-side of the above equation is not identical.
* The difference between these two values are generally not negligible, but becomes smaller by improving the accuracy; it increases computational cost, of course.

* For calculating the gradient of the energy with respect to the atomic positions (${\bf R}$), the above theorem becomes
$$
\begin{align*}
\frac{\partial E}{\partial {\bf R}} = \frac{\partial}{\partial{\bf R}}\braket{\Psi|\hat{H}|\Psi} &= \braket{\frac{\partial\Psi}{\partial{\bf R}}|\hat{H}|\Psi} + \braket{\Psi| \frac{\partial\hat{H}}{\partial{\bf R}} |\Psi} + \braket{\Psi| \hat{H}|\frac{\partial\Psi}{\partial{\bf R}}} \\
&= E\braket{\frac{\partial\Psi}{\partial{\bf R}}|\Psi} + E\braket{\Psi|\frac{\partial\Psi}{\partial{\bf R}}} + \braket{\Psi| \frac{\partial\hat{H}}{\partial{\bf R}} |\Psi}
\end{align*}
$$
* The first two terms are the difference from the Hellmann-Feymann force, and it is called *Pulay force*.
* The correction to the Pulay force is necessary when the wave function is dependent on the nuclear position (${\bf R}$).
* The Gauss functions used in the Gaussian is nuclear-position-dependent, because we put these functions on the atomic positions.
* However, the plane-wave basis set is not dependent on the nuclar position. So one can calculate the energy gradient without the Pulay force.
* This *reduces the computational cost of gradient calculation, and increases the accuracy of the energy gradient*. Since the MD calculation heavily rely on the energy (and more) on the force, this difference in the accuracy is of practical importance.

## Computation
* Running AIMD with Gaussian is not impossible but its function is limited. *So I do not recommend use Gaussian for AIMD*.
* Here we will use *Atomic simulation environment (ASE)* and call Gaussian from it.

---

## Exercises
1. to be uploaded
