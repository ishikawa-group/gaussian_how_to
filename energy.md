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
    * route section: specify the keyword to control the calculation
    * title section: you can put any keyword, phrases, but in *one line*
    * charge and multiplicity: two integers specifying charge (0: nuetral, 1: +1 cation, -1: -1 anion) and spin multiplicity (1: singlet, 2: doublet, etc)
    * geometry section: each line should have an element symbol, xyz coordinates (in angstrom unit)

* For geometry section, there are several ways to specify the coordinate (internal coordinate, Z-matrix) but above Cartesian style is most general so we'll use it
