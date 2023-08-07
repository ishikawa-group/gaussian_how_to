# Geometry optimization
## Background
* In this lecture, we will see how to do the geometry optimization with Gaussian.
* Geometry here means the positions of nuclei in the molecule. The most stable positions of nuclei make the minimum of the total energy (electronic energy plus inter-nuclei repulsion) of the molecule. This is achirved, for example, changing the bond length of diatomic molecules (in the diatomic molecule, the relatinoship between coordinate and energy is called the *potential energy curve*).
* Geometry optimization is the procedure to find the energy minima in the potential energy curve.

## Computation
* To perform the geometry optimiztation, you simply put `opt` in the route section.
```
# HF/6-31G opt

Input for geometry optimization

0 1
O  -0.464   0.177   0.000
H  -0.464   1.137   0.000
H   0.441  -0.143   0.000
```

### Important parameters
* `maxcycle`: Number of the maximum steps of the optimizaiton. Default is max(20, 2*number_of_optimizing_variables). Use like `opt(maxcycle=200)`
* `readFC`: Read force constant obtained by frequency analysys. Usually accelerates the optimization. Use like `opt(readFC)`.
* `calcFC`: Calculate force constant before optimization, and use it. Good convergence but costly. Use like `opt(calcFC)`.
* optimization level: `loose`, `tight`, or `verytight`.
* optimization algorithm: `gediis`, `newton`, `steep`. If you are going to locate the local minimum near the current geometry, use `newton` or `steep`.

### Analyzing the output
* Load the `.out` file with GaussView.
* By clicking `Results` -> `Optimization`, you can see the geometry changes during the optimization.

# Vibrational frequency analysis
## Backgroud
*

## Computation
* To perform the frequency analysis, you simple put `freq` in the route section.
```
# HF/6-31G freq

Input for geometry optimization

0 1
O  -0.464   0.177   0.000
H  -0.464   1.137   0.000
H   0.441  -0.143   0.000
```

### Important parameters
* `noraman`: Skip the Raman intensity calculation, which cuts the 10-30% computational time. Default is false so I recommend to always put this keyword (unless you calculating the Raman spectra). Use like `freq=noraman or freq(noraman)`.

### Analyzing the output
* Load the `.out` file with GaussView.
* By clicking `Results` -> `Vibrations`, you can see the geometry changes during the optimization.

---

## Exercises
1. asdf
2. asdf
3. asdf