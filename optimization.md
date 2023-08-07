# Geometry optimization
## Background
* In this lecture, we will see how to do the geometry optimization with Gaussian.
* Geometry here means the positions of nuclei in the molecule.
* The most stable positions of nuclei make the minimum of the total energy (electronic energy plus inter-nuclei repulsion) of the molecule.
* This is achirved, for example, changing the bond length of diatomic molecules.
* In the diatomic molecule, the relatinoship between coordinate and energy is called the *potential energy curve*.
* Geometry optimization is the procedure to find the energy minima in the potential energy curve.

## Computation
* To perform the geometry optimiztation, you simply put `opt` in the route section.
```
# HF/6-31G opt

Input for geometry optimization

0 1
O  -0.464   0.177   0.0	 
H  -0.464   1.137   0.0	 
H   0.441  -0.143   0.0
```
# Vibrational frequency analysis
## Backgroud
*

## Computation
* To perform the frequency analysis, you simple put `freq` in the route section.
```
# HF/6-31G opt

Input for geometry optimization

0 1
O  -0.464   0.177   0.0	 
H  -0.464   1.137   0.0	 
H   0.441  -0.143   0.0
```