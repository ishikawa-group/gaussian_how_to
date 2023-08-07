# Energy calculation
* In this lecture, we will see how to do a single-point energy calculation with Gaussian.
* An input file should `.com` or `.gjf` extension. Here we will use `.com` in the lecture.

* Input example (named `h2o.com`, for example)
```
# HF/6-31G

water energy

0 1
O  -0.464   0.177   0.000
H  -0.464   1.137   0.000
H   0.441  -0.143   0.000
```
* The input file has following structure

1. Route section
* Specify the keyword to control the calculation, should start with `#` symbol.
* Route section consists of keyword and option pairs. Options can be specified in any of the following forms:
   * keyword = option
   * keyword(option)
   * keyword=(option1, option2, …)
   * keyword(option1, option2, …)
   
2. Title section
* you can put any keyword, phrases, but in *one line*

3. Charge and multiplicity
* Two integers specifying charge (0: nuetral, 1: +1 cation, -1: -1 anion) and spin multiplicity (1: singlet, 2: doublet, etc)

4. Geometry section
* Each line should have an element symbol, xyz coordinates (in angstrom unit)
* For geometry section, there are several ways to specify the coordinate (internal coordinate, Z-matrix) but above Cartesian style is most general so we'll use it

## Computational method
* As stated, the computational condition is controled by the keywords in route section.
* The computational method such as Hatree-Fock or density functional theory (DFT) should be specified.
* In addition, one should specify the *basis set* for expanding the wavefunction.
* Following are the example of computational methods, often used.
  | Keyword | Method                                   |
  | ------- | ---------------------------------------- |
  | HF      | Hatree-Fock                              |
  | MP2     | Moller-Presset perturbation theory       |
  | CCSD    | Coupled cluster with singles and doubles |
  | B3LYP   | DFT with B3LYP functional                |

* In Hatree-Fock or DFT calculation, self-consisten field (SCF) problem is used, i.e. following the Hartree-Fock-Roothaan problem is solved.
$$
{\bf FC} = {\bf SC\epsilon}
$$
where ${\bf F}$ is the Fock matrix, ${\bf C}$ is the molecuar orbital (MO) coefficient matrix, ${\bf S}$ is the overlap matrix, and $\epsilon$ is the orbital energy matrix.
* ${\bf F}$ and ${\bf S}$ is given before solving the above eigenvalue problem, while ${\bf C}$ and $\epsilon$ is calculated and these makes the MOs.

### Electron correlatin
* The Hatree-Fock method lacks the energy component, called *electron correlation energy*.
* The electron correlation is the energy contribution originating in the electron-electron repulsion, which cannot be covered by the self-consistent field method.
* Several ways to recover the electron correlation is reported;
  * configuration interaction (CI)
  * Moller-Plesett peturbation theory, n-th order (MPn)
  * coupled cluster (CC)
* Each method has the level of excited electron configuration, corresponding to the accuracy, like singles, doubles, triples, etc.
* Among them, second-order Moller presset (MP2), MP3, MP4, or coupled cluster singles and doubles (CCSD), CCSD plus perturbative triples (CCSD(T)) are popular methods.

## Basis set
* Then, one should specify the basis set. In Gaussian, all the basis set is Gaussian-type orbitals (GTOs). The GTOs are used in assembled manner, to make "sharp" function like to Slater-basis function $\exp(–\alpha r)$.
* For this, several GTOs are summed into one GTO function sets. This process is called *contraction* and summed GTO is called *contracted GTO*. The uncontracted GTO is called *primitive GTO*.
* Major basis sets are like *6-31G* basis set, which means six GTOs to represent the inner shell of the atom, and three and one basis sets for the outer (valence) shell of the atom.
* For example, the inner shell of the carbon atom (C) is the 1s shell, and the valence shell is the 2s and 2s shells. The inner shell is said to be "harder", meaning that the wave function does not change considerably by the environment (such as neighboring atoms). On the other hand, the valence shell is "softer" than the inner shell.
* The chemical bond is always formed by electrons in the valence shell (called *valence electrons*). In the C atom, valence electrons are 2s and 2p electrons.
* As the valence electrons are softer, so the basis set representing them should be flexible enough. To achieve this, two or three independent GTOs are used.
* When two GTOs are used for valence part, the basis set is called *double-zeta (DZ)* where zeta means the exponent of the GTOs. When three GTOs are used, it is called *triple-zeta (TZ)*.
* The "31" part of the 6-31G basis means "one contracted GTO with three premitive GTO" and "one (uncontracted) GTO".

### Polarization and diffuse functions
* Polarization function
* Increasing the flexibility of the basis set, by adding the *function with higher angular momentum*.
* For example in H atom, the valence electrion is 1s so p function is not necessary.
* But adding this will enable the flexible description of the wavefunction.
* For C atom, d function is added as the polarization function.
* In Gaussian, the polrization function is represented by "*", e.g. 6-31G -> 6-31G*.

* Diffuse function
* Functions with larger exponent (thus spacially more spreaded) is added as diffuse function.
* These functions are sometimes necessary, for example in anions.
* In Gaussin, the diffuse function is represented by "+", e.g. 6-31G -> 6-31+G.

### Reference
* The details of the basis set can be found the Gaussian website: https://gaussian.com/basissets/

## Speficication in route section
* The computational method and basis set should be specificed at the route section.
* It can be simply specified as `method/basis set`. For example, whem perfoming the Hatree-Fock calculation with 6-31G basis set, the route section should be `# HF/6-31G`.

## Geometry of the molecule
* One can use GaussView to make the molecule.
* The generated file can be directly used for the Gausssian, or you can modify some of them if you like.

## Link-0 command
* In the input file, you can specify the *number of CPUs*, *amount of memory*, and the *name of the checkpoint file*.
* Checkpoint file (.chk) is the intermediate file having the computational results. Sometimes you need to have this file for visualization or restarting the calculation.
* Checkpoint file (binary) can be converged to *formatted checkpoint file (.fchk)* (text file) by linux command `formchk XXX.chk XXX.fchk`.
* Usually, there is a default setting for the number of the CPUs and amount memory.

---

## Output files
* In energy calculation, the calculated energy is the most important information in the output file.
* The calculated energy is found in the following part
```
SCF Done ... 
```
* The energy unit is *atomic unit*, where the energy is called *Hatree*.
* Conversion from *Hatree* to other units are as follows:
  | Convert to | Factor     |
  | ---------- | ---------- |
  | eV         | 27.211     |
  | kcal/mol   | 627.51     |
  | kJ/mol     | 4.1*626.51 |

---

## Exercises
1. Write the input file for do the B3LYP calcuation (with any basis set) for H2O molecule, and then run the Gaussian.
2. Compare the calculated energy of H2O with several basis sets (same geometry). Summarize them in a table, or plot the calculated energy with respect to the number of basis functions. The number of basis function is found in `NBasis` in output file.
