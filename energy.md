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

## Keywords in route section
* As stated, the computational condition is controled by the keywords in route section.
* The computational method such as Hatree-Fock or density functional theory (DFT) should be specified.
* In addition, one should specify the *basis set* for expanding the wavefunction.
* Following methods can be used as the computational method.
    |Keyword|Method|
    |-------|------|
    |HF|Hatree-Fock|
    |MP2|Moller-Presset perturbation theory|
    |CCSD|Coupled cluster with singles and doubles|
    |B3LYP|DFT with B3LYP functional|
