# Excited state calculation
## Background
* See `theory.md`.

## Computational setup
* Specifying `TD` in the route section indicates the time-dependent Hatree-Fock (TDHF) or time-dependent DFT (TDFT) calculation.
```
# HF/6-31G TD

water energy

0 1
O  -0.464   0.177   0.000
H  -0.464   1.137   0.000
H   0.441  -0.143   0.000

```
* Specifying the DFT functional in the route section and giving `TD` will perform the TDDFT calculation. So `# B3LYP/6-31G TD` will do the TDDFT calculation using B3LYP functional.
* To do the configuration interaction singles (CIS) calculation, use `CIS` in the route section. The CIS and TDHF are equivalent so it gives the almost same result.

### Options
* `NStates=M`: The number of excited states. The default value is 3, so in most cases, you need to increase this value. Use like `TD(NStates=10)`.
* `Singlets`: Solves for the spin-singlet cases. This is the default for closed-shell systems.
* `Triples`: Solves for the spin-triplet cases.

## Analyzing results
* The related part in the Gaussian output is
```
 Excitation energies and oscillator strengths:

 Excited State   1:      Singlet-A"     2.9967 eV  413.74 nm  f=0.0049  <S**2>=0.000
      61 -> 68         0.10770
      62 -> 68         0.13062
      63 -> 71         0.11394
      67 -> 68         0.66646
 This state for optimization and/or second-order correction.
 Total Energy, E(TD-HF/TD-DFT) =  -1527.51939508
 Copying the excited state density for this state as the 1-particle RhoCI density.

 Excited State   2:      Singlet-A'     3.2869 eV  377.21 nm  f=0.0017  <S**2>=0.000
      63 -> 68         0.52785
      65 -> 68        -0.29767
      67 -> 71        -0.31726
```
* The character and number following the `Excited state 1:` is the character of the excitation and the excitation energy. The `f` is the oscillator strength, so only non-zero excitations can be observed experimentally.
* The line like `61 -> 68  0.10770` shows the nature of the excited state. The integer (61 and 68 in this case) is the MO number, so the excitation 1 mainly occurs via the excitation from 67th MO to 68th MO.

### UV-Vis spectrum
* With GaussView, you can plot the UV-Vis spectrum from the output file: `Results` -> `UV-Vis`.
* You can save the text file: `Plots` -> `Save Data`.
