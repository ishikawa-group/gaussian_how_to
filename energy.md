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
* The blanc line in the last is needed.

* The input file has following structure

1. Route section
* Specify the keyword to control the calculation, should start with `#` symbol.
* `#P` gives more detailed output, such as SCF convergence procedure. Better to switch it on.
* Route section consists of keyword and option pairs. Options can be specified in any of the following forms:
   * keyword=option
   * keyword(option)
   * keyword=(option1, option2, …)
   * keyword(option1, option2, …)
   
2. Title section
* you can put any keyword, phrases, but in *one line*

3. Charge and multiplicity
* Two integers specifying charge (0: neutral, 1: +1 cation, -1: -1 anion) and spin multiplicity (1: singlet, 2: doublet, etc)

4. Geometry section
* Each line should have an element symbol, xyz coordinates (in angstrom unit)
* For geometry section, there are several ways to specify the coordinate (internal coordinate, Z-matrix) but above Cartesian style is most general so we'll use it

## Computational method
* As stated above, the computational condition is specified by the keywords in route section.
* The computational method such as Hartree-Fock or density functional theory (DFT) should be selected.
* In addition, one should specify the *basis set* for expanding the wavefunction.
* Following are the example of computational methods, often used.
  | Keyword | Method                                   |
  | ------- | ---------------------------------------- |
  | HF      | Hartree-Fock                              |
  | MP2     | Moller-Plesset perturbation theory       |
  | CCSD    | Coupled cluster with singles and doubles |
  | B3LYP   | DFT with B3LYP functional                |

* In Hartree-Fock or DFT calculation, self-consistent field (SCF) problem is used, i.e. following the Hartree-Fock-Roothaan problem is solved.
```math
{\bf FC} = {\bf SC\epsilon}
```
* ${\bf F}$ is the Fock matrix, ${\bf C}$ is the molecular orbital (MO) coefficient matrix, ${\bf S}$ is the overlap matrix, and $\epsilon$ is the orbital energy matrix.
* ${\bf F}$ and ${\bf S}$ is given before solving the above eigenvalue problem, while ${\bf C}$ and $\epsilon$ is calculated and these makes the MOs.

### Electron correlation
* The Hartree-Fock method lacks the energy component, which is called *electron correlation energy*.
* The electron correlation is the energy contribution originating in the electron-electron repulsion, which cannot be covered by the self-consistent field method.
* Several ways to recover the electron correlation is reported;
  * configuration interaction (CI)
  * Moller-Plesset perturbation theory, n-th order (MPn)
  * coupled cluster (CC)
* These are called *post Hatree-Fock methods*, because they all use the HF as the starting point.
* Each method has *singles*, *doubles*, *triples*, in their computational levels. This means the level of excitation, that is, the excited configuration included in the calculation. 
* Including higher level of excitation gives higher accuracy, but larger computatinoal cost.
* Among them, second-order Moller Plesset (MP2), MP3, MP4, or coupled cluster singles and doubles (CCSD), CCSD plus perturbative triples (CCSD(T)) are popular methods.
* DFT includes the electron correlation in a completely different way --- via exchange-correlation (XC) functional. Thus DFT *models* the electron correlation, and put it in the XC functional.
* Computational cost of DFT is lower than that of HF + electron correlaion calculation. However, unlike the post HF methods, systematic improvements on accuracy is almost impossible.

## Basis set
* To solve the Schrodinger equation for molecules, we need to have their wavefunction. Because the solution of the Schrodinger equation is unavailable for molecules, we need some approximation.
* A good starting point to approximate the molecular wavefunction is to use the known answer, such as the hydrogen atom case.
* The exact solution for the Schrodinger equation of hydrogen atom is the exponentially decaying function w.r.t distance from a nucleus $r$, or $\exp(-\alpha r)$. This is called the *Slater-type orbital (STO)*.
* Unfortunately, using STO for quantum chemistry calculation face some difficulty in evaluating their integrals. However, a similar *Gaussian-type orbital (GTO)* $\exp(-\alpha r^2)$ removes this difficulty. For this reason, using GTO is more popular among the quantum chemistry community.
* The GTOs are used in assembled manner, to make "sharp" function like to Slater-basis function $\exp(–\alpha r)$.
* For this, several GTOs are summed into one GTO function sets. This process is called *contraction* and summed GTO is called *contracted GTO*. The uncontracted GTO is called *primitive GTO*.
* The simplest basis set category is called *STO-NG*, where $N$ is the number of Gaussian function to mimic the STO.
* Major basis sets are like *6-31G* basis set, which means six GTOs to represent the inner shell of the atom, and three and one basis sets for the outer (valence) shell of the atom.
* For example, the inner shell of the carbon atom (C) is the 1s shell, and the valence shell is the 2s and 2s shells. The inner shell is said to be "harder", meaning that the wave function does not change considerably by the environment (such as neighboring atoms). On the other hand, the valence shell is "softer" than the inner shell.
* The chemical bond is always formed by electrons in the valence shell (called *valence electrons*). In the C atom, valence electrons are 2s and 2p electrons.
* As the valence electrons are softer, so the basis set representing them should be flexible enough. To achieve this, two or three independent GTOs are used.
* When two GTOs are used for valence part, the basis set is called *double-zeta (DZ)* where zeta means the exponent of the GTOs. When three GTOs are used, it is called *triple-zeta (TZ)*.
* The "31" part of the 6-31G basis means "one contracted GTO with three primitive GTO" and "one (uncontracted) GTO".

* The figure shows that how to approximate the STO with GTO. Taking summation of GTOs with several exponents (GTO1 has the largest exponent and GTO3 has the smallest) makes good approximation.
<p align="center">
<img src=./fig/sto_and_gto.png width=80%>
</p>

### Polarization and diffuse functions
* Polarization function
* Increasing the flexibility of the basis set, by adding the *function with higher angular momentum*.
* For example in H atom, the valence electron is 1s so p function is not necessary.
* But adding this will enable the flexible description of the wavefunction.
* For C atom, d function is added as the polarization function.
* In Gaussian, the polarization function is represented by "\*", e.g. 6-31G -> 6-31G*.

### Diffuse function
* Functions with larger exponent (thus spatially more spread) is added as diffuse function.
* These functions are sometimes necessary, for example in anions.
* In Gaussian, the diffuse function is represented by "+", e.g. 6-31G -> 6-31+G.

### cc-pVXZ
* Another category of basis set often used is *correlation consistent basis set (cc-pVXZ) series*. Here X can be D(double), T(triple), Q(quadruple) or higher.
* As its name indicates, the parametrization of this basis set is intended to cover the electron correlation effectively.
* This basis set automatically includes the polarization function so no need to add.
* To add the diffuse function, use *aug-cc-pVXZ* (aug means augmentation).
* Usually, cc-pVXZ basis has more primitive Gaussian functions than 6-31G series basis set. This means lager number of integrations are necessary, thus longer computational time. For example the geometry optimization using cc-pVDZ takes longer time than that using 6-31G\*.

### Summary
* To summarize the popular basis set levels:

| quality     | basis set                |
| ----------- | ------------------------ |
| low         | STO-3G, 3-21G, 6-31G     |
| medium      | 6-31G\*                  |
| medium-high | 6-31+G\*                 |
| high        | 6-311G\*\*, cc-pVDZ      |
| very high   | aug-cc-pVXZ (X = T or Q) |

* In scientific papers, 6-31G\* or cc-pVDZ is often used. For STO-3G or 6-31G, referees may complain for the lack of accuracy.

### Reference
* The details of the basis set can be found the Gaussian website: https://gaussian.com/basissets/

## Energy calculation: specification in route section
* The computational method and basis set should be specified at the route section.
* It can be simply specified as `method/basis set`. For example, when performing the Hartree-Fock calculation with 6-31G basis set, the route section should be `# HF/6-31G`.

## Geometry of the molecule
* One can use GaussView to make the molecule.
* The generated file can be directly used for the Gaussian, or you can modify some of them if you like.

## Link-0 command
* In the input file, you can specify the *number of CPUs*, *amount of memory*, and the *name of the checkpoint file*.
* Checkpoint file (.chk) is the intermediate file having the computational results. Sometimes you need to have this file for visualization or restarting the calculation.
* Checkpoint file (binary) can be converged to *formatted checkpoint file (.fchk)* (text file) by linux command `formchk XXX.chk XXX.fchk`.
* Usually, there is a default setting for the number of the CPUs and amount memory.

### Element-wise specification of basis set
* You can specify the different basis set for different elements. To do, you should put `gen` keyword in the route section.
```
# B3LYP/Gen
     
HF/6-31G(d) on O and 6-31G on H

0 1
O  -0.464   0.177   0.000
H  -0.464   1.137   0.000
H   0.441  -0.143   0.000
    
O 0
6-31G(d)
****
H 0
6-31G
****

```

---

## Output files
* In energy calculation, the calculated energy is the most important information in the output file.
* The calculated energy is found in the following part
```
SCF Done ... 
```
* The energy unit is *atomic unit*, where the energy is called *Hartree*.
* Conversion from *Hartree* to other units are as follows:
  | Convert to | Factor       |
  | ---------- | ------------ |
  | eV         | 27.211       |
  | kcal/mol   | 627.51       |
  | kJ/mol     | 4.187*627.51 |

---

## Molecular orbital visualization
* To see the molecular orbital (MO), see `pop=reg` or `pop=full` in route section.
* There are two ways to visualize molecular orbital.
    1. Using formatted checkpoint (fchk) file
    * Visualization software like GaussView can make MO plot from the fchk file.
    * The fchk file is made from chk file like `formchk h2o.chk h2o.fchk`
    * `formchk` is a part of Gaussian so you should do `module load gaussian16` first.
    * After generating fchk file, transfer it to your local PC and then
        1. In GaussView, load fchk file.
        2. Click "MO Editor" tab.
        3. Click MOs from the MO diagram.
        4. In "Visualize" tab, adjust the isovalue and click "Update".
    2. Using cube file
    * when you have specific MO to write, use `cubegen` which is a part of Gaussian.
        1. `module load gaussian16`
        2. `cubegen 1 mo=homo h2o.fchk homo.cube`
        3. transfer the cube file to your local PC, and plot it with some software
    * Acutually GaussView (and other visualization software) does the same thing on the viewer side.

---

## Molecular viewer issue
* Molecular viewer for Gaussian is acturally a big problem if you would like to do everything *free*.
* If you afford *GaussView*, this is the easiest way as it moves on Win/Mac, and also enables the MO visualization.
* GaussView is installed on TSUBAME so you can use it via ssh + X-Window, *but this is extremely slow!*
* For Tokyo-tech student, you can ask TSUBAME administrator to have GaussView media for your lab. Ask administrators.

* For free software, *VESTA*, *Avogadro*, *Avogadro2*, *VMD* etc. are available.
* Among them, *Avogadro* and *Avogadro2* is easiest for molecular modeling (i.e. adding atoms, moving atoms). However, *Avogadro* is not compatible with 64-bit Windows and *Avogadro2* (currently) has some problem in MO visualization.
* *VESTA* works both on Win/Mac, and it makes MO visualization when you have cube file, but molecular modeling is not easy.
* Thus I can recommend to use *Avogadro* for Mac users, and *Avogadro2* + *VESTA* for Windows users (but there would be better option...).
* There is another software ChemCraft, which is not free but you can use it as trial for several months. This could be an option for Windows user.
* Other softwares like ChemDraw/ChemOffice would be useful in making molecular coordinate.

| App       | Windows | Mac | Modeling | MO drawing   |
| --------- | ------- | --- | -------- | ------------ |
| VESTA     | yes     | yes | no easy  | from cube    |
| Avogadro  | no      | yes | easy     | yes          |
| Avogadro2 | yes     | yes | easy     | some problem |
| ChemCraft | yes     | no  | so-so    | yes          |

---

## Exercises
1. Plot the occupied MOs of H2O with small basis set (like HF/STO-3G or HF/6-31G), and interpret them in terms of conventional chemical bonds, such as O-H sigma bond, O lone pair etc.
2. Compare the calculated energy of H2O with HF method, at several basis sets (same geometry). Summarize them in a table, or plot the calculated energy with respect to the number of basis functions. The number of basis function is found in `NBasis` in output file. Confirm the variational principle.
