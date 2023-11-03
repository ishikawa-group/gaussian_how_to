# Geometry optimization
## Background
* In this lecture, we will see how to do the geometry optimization with Gaussian.
* Geometry here means the positions of nuclei in the molecule. The most stable positions of nuclei make the minimum of the total energy (electronic energy plus inter-nuclei repulsion) of the molecule. This is achieved, for example, changing the bond length of diatomic molecules (in the diatomic molecule, the relationship between coordinate and energy is called the *potential energy curve*).
* Geometry optimization is the procedure to find the energy minima in the potential energy curve.

## Computation
* To perform the geometry optimization, you simply put `opt` in the route section.
```
# HF/6-31G opt

Input for geometry optimization

0 1
O  -0.464   0.177   0.000
H  -0.464   1.137   0.000
H   0.441  -0.143   0.000

```

### Important parameters
* `maxcycle`: Number of the maximum steps of the optimization. Default is max(20, 2*number_of_optimizing_variables). Use like `opt(maxcycle=200)`
* `readFC`: Read force constant obtained by frequency analysis. Usually accelerates the optimization. Use like `opt(readFC)`.
* `calcFC`: Calculate force constant before optimization, and use it. Good convergence but costly. Use like `opt(calcFC)`.
* optimization level: `loose`, `tight`, or `verytight`.
* optimization algorithm: `gediis`, `newton`, `steep`. If you are going to locate the local minimum near the current geometry, use `newton` or `steep`.

### Analyzing the output
* Load the `.out` or `.fchk` file with GaussView.
* By clicking `Results` -> `Optimization`, you can see the geometry changes during the optimization.
* The process of geometry convergence can be tracked by the keyword `Converged`.

# Vibrational frequency analysis
## Background
* See `theory.md`.

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
* Specifing both `opt` and `freq` does frequency analysis *after* the optimization, which is useful.

### Important parameters
* `noraman`: Skip the Raman intensity calculation, which cuts the 10-30% computational time. Default is false so I recommend to always put this keyword (unless you calculating the Raman spectra). Use like `freq=noraman or freq(noraman)`.

### Analyzing the output
* The important part of the output of frequency calculation is below:
```
                      1                      2                      3
                      A                      A                      A
 Frequencies --     92.2415               123.8872               234.9248
 Red. masses --      4.9328                 3.6897                 4.4624
 Frc consts  --      0.0247                 0.0334                 0.1451
 IR Inten    --      0.8536                 0.0000                 1.2840
 ...
```
* Vibrational frequencies are in ${\rm cm}^{-1}$ unit.
* The IR(infrared) intensity is shown. The IR active vibrational mode has non-zero intensity.

#### Visualization
* Load the `.out` or `.fchk` file with GaussView.
    * when doing `opt + freq`, `.out` has both optimization trajectory and frequency result, but `.fchk` has only the frequency result.
* By clicking `Results` -> `Vibrations`, you can see the geometry changes by `Animation`. This motion shows the *vibrational normal mode*.
* You can visualize the IR spectrum by clicking `spectra` tab in the Vibration tool.
* You can save the IR spectrum by text file, by `Plots` -> `Save Data`.

### Zero-point energy
* The vibrational analysis gives the **zero-point energy (ZPE)** of molecule. This quantum-mechanical term is always present (even at zero-temperature), so should be always added to the total energy component, especially when comparing the energy of different structures.

### Free energy
* By carrying out the vibrational analysis, one can have the **Gibbs free energy** of molecules.
* Usually, the experiments are done in fixed number of particles ($N$), fixed pressure ($P$), and fixed temperature ($T$), so corresponds to the $NPT$ ensemble.
* In this case the Gibbs free energy is the correct thermodynamic potential.
* The chemical reaction with negative $\Delta G$ (the Gibbs energy difference: $G(\rm{products}) - G(\rm{reactants})$ is said to be *spontaneous* so it is thermodynamically favored.
* The Gibbs free energy is defined as follows:
```math
G = H - TS = U + pV - TS
```
* Here $H$ is enthalpy, $U$ is internal energy, $T$, $p$, $V$ are temperature, pressure, and volume respectively.
* The internal energy $U$ and entropy $S$ terms have *translational*, *rotational*, and *vibrational* components.
```math
\begin{align*}
& U = U_{\rm elec} + U_{\rm trans} + U_{\rm rot} + U_{\rm vib} \\
& S = S_{\rm elec} + S_{\rm trans} + S_{\rm rot} + S_{\rm vib}
\end{align*}
```
* The translational and rotational components are easily calculated with the particle-in-a-box and rigid rotor approximation, respectively.
* The vibrational component needs *vibrational frequencies*, so one should perform a quantum-mechanics-level calculation to have it.
```math
\begin{align*}
& S_{\rm vib} = R\sum_k\left[ \frac{h\nu_k/k_B T}{\exp(h\nu_k/k_B T)}-\ln(1-\exp(-h\nu_k/k_B T)) \right] \\
& U_{\rm vib} = R\sum_k\frac{h\nu_k}{k_B T}\left[\frac{1}{2}+\frac{1}{\exp(h\nu_k/k_B T)-1}\right]
\end{align*}
```
* $\nu_k$ is the vibrational frequency (of $k$-th vibrational level), $h$ and $k_B$ are the Planck constant and the Boltzmann constant. $R$ is the universal gas constant.
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
* The last line is the Gibbs free energy (in Hartree or a.u.).
* If you want to have the Gibbs energy other than default temperature and pressure (298.15 K and 1 atm), specify `Temperature=XXX` or `Pressure=XXX` in the route section and do the frequency calculation (in Kelvin and atm, respectively).

---

## Exercises
1. Do a geometry optimization of water molecule with B3LYP/6-31G level.
2. Do a frequency analysis of water molecule, at above-optimized structure.
3. Form or download (from somewhere) a structure of anthracene, and do optimization and frequency analysis. Visualize the IR spectrum of it, and compare with experimental result.
4. Optimize the water dimer, and calculate the Gibbs free energy difference of water dimer formation. Is it a thermodynamically favored process?