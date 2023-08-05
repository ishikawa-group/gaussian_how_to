# Energy calculation
* In this lecture, we will see how to do a single-point energy calculation with Gaussian.
* An input file should `.com` or `.gjf` extension. Here we will use `.com` in the lecture.

* Input example (named `h2o.com`, for example)
```
# HF/6-31G

water energy

0 1
O  -0.464   0.177   0.0	 
H  -0.464   1.137   0.0	 
H   0.441  -0.143   0.0
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
* Diffuse function

## Speficication in route section
* The computational method and basis set should be specificed at the route section.
* It can be simply specified as `method/basis set`. For example, whem perfoming the Hatree-Fock calculation with 6-31G basis set, the route section should be `# HF/6-31G`.

## Geometry of the molecule
* One can use GaussView to make the molecule.
* The generated file can be directly used for the Gausssian, or you can modify some of them if you like.

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