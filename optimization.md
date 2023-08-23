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

### Zero-point energy
* The vibrational analysis gives the **zero-point energy (ZPE)** of molecule. This quantum-mechanical term is always present (even at zero-temperature), so should be always added to the total energy component, especially when comparing the energy of different structures.

### Free energy
* By carrying out the vibrational analysis, one can have the **Gibbs free energy** of molecules.
* The Gibbs free energy is defined as follows:
$$
G = H - TS = U + pV - TS
$$
where $H$ is enthalpy, $U$ is internal energy, $T$, $p$, $V$ are temperature, pressure, and volume respectively.
* The internal energy $U$ and entropy $S$ terms have *translational*, *rotational*, and *vibrational* components.
$$
U = U_{\rm elec} + U_{\rm trans} + U_{\rm rot} + U_{\rm vib} \\
S = S_{\rm elec} + S_{\rm trans} + S_{\rm rot} + S_{\rm vib}
$$
* The translational and rotational components are easily calculated with the particle-in-a-box and rigid rotor approximation, respectively.
* The vibrational component needs *vibrational frequencies*, so one should perform a quantum-mechanics-level calculation to have it.
$$
S_{\rm vib} = R\sum_k\left[ \frac{h\nu_k/k_B T}{\exp(h\nu_k/k_B T)}-\ln(1-\exp(-h\nu_k/k_B T)) \right] \\
U_{\rm vib} = R\sum_k\frac{h\nu_k}{k_B T}\left[\frac{1}{2}+\frac{1}{\exp(h\nu_k/k_B T)-1}\right]
$$
where $\nu_k$ is the vibrational frequency (of $k$-th vibrational level), $h$ and $k_B$ are the Planck constant and the Boltzmann constant. $R$ is the universal gas constant.
* The vibrational analysis in Gaussian provides the vibrational frequency so all information for calculating the Gibbs energy is obtained.

* The Gibbs energy is seen in the output file as
```
Zero-point correction=                  .023261 (Hartree/Particle)
Thermal correction to Energy=           .026094
Thermal correction to Enthalpy=         .027038
Thermal correction to Gibbs Free Energy= .052698
Sum of electronic and zero-point Energies=-527.492585    # E0 = Eelec + ZPE
Sum of electronic and thermal Energies= -527.489751      # E = E0 + Evib + Erot + Etrans
Sum of electronic and thermal Enthalpies=-527.488807     # H = E + R
Sum of electronic and thermal Free Energies=-527.463147  # G = H - TS
```
* The last line is the Gibbs free energy (in Hatree or a.u.).

* In chemical reaction, not the absolute Gibbs energy but *the Gibbs energy difference ($\Delta G$) is important.
* To calculate $\Delta G$, just take the difference of the Gibbs energies of two geometries.

---

## Exercises
1. Do a geometry optimization of water molecule with B3LYP/6-31G level.
2. Do a frequency analysis of water molecule, at above-optimized structure.
3. Do a geometry optimization of water dimer with B3LYP/6-31G level. Try to start from several initial geometries.