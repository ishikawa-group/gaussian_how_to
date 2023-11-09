# Transition state
## Background 
* In this lecture, we will see how to locate the transition state (TS) with Gaussian.

<p align=center>
<img src=./fig/sn2.jpg width=70%>

Fig. Reactant, transition state, and product of SN2 reaction.
</p>

* TS is the geometry connecting the reactant (before reaction) and the product system (after reaction).
* The energy difference between the reactant and the TS is called the *activation energy* (for forward direction).
* The energy difference of the product and the TS is also the activation energy, but for reverse direction.
* The energy difference of the reactant and product states is called the *reaction energy*.

## Computation
* Basically, the TS optimization needs the force constant (Hessian) matrix.
* `opt=TS` will do the TS optimization.
* `calcFC`: Calculate the force constant before doing the TS calculation. Use like `opt(TS, calcFC)`
* `readFC`: Read the pre-calculated force constant. Use like `opt(TS, readFC)`
* Gaussian checks the number of imaginary frequency during the TS optimization by default (called *eigentest*).
* Usually it is better to cut this optiion, especially at early stage of TS optimization. To do this, specify `opt=(TS, noeigentest)`.

### Redundant coordinate
* By using *redundant coordinate*, you can put some constraint during the geometry optimization.
* To do the geometry optimization with redundant coordinate, use `opt=modredundant`.
* The details of the redundant coordinate should be provided *under* the element coordinate section.
* Details of redundant coordinate can be seen https://gaussian.com/modred/.
* Some typical cases:
    * Fixing the position of atom 1: `X 1 F`
    * Fixing the bond of 1 and 2: `B 1 2 F`
    * Fixing the angle between 1-2-3: `A 1 2 3 F`
    * Fixing the dihedral angle of 1-2-3-4: `D 1 2 3 4 F`
* For example in fixing the one H-O bond in H2O,
```
# HF/6-31G opt=modred

Input for geometry optimization

0 1
O  -0.464   0.177   0.000
H  -0.464   1.137   0.000
H   0.441  -0.143   0.000

B 1 2 F

```


## Analyzing output
* As stated in Background, the TS is the saddle point in the potential energy surface. This mathematically means that the second-derivative matrix (or Hessian) should have one negative eigenvalue. In vibrational frequency, the negative eigenvalue corresponds to the imaginary number as it takes the square root of the eigenvalue.
* To confirm the above thing, you should perform the vibrational analysis on the TS geometry, and find the vibrational frequency part
```
...
****** 1 imaginary frequencies (negative Signs) ******
Diagonal vibrational polarizability:
 147.4087161 17.7993849 22.2001409
Harmonic frequencies (cm**-1), IR intensities (KM/Mole), Raman scattering
activities (A**4/AMU), depolarization ratios for plane and unpolarized
incident light, reduced masses (AMU), force constants (mDyne/A),
and normal coordinates:
                   1        2        3
                   A        A        A
Frequencies -- -442.5060 232.2243 232.2244
Red. masses --   12.6450   4.2936   4.2936
Frc consts  --    1.4588   0.1364   0.1364
IR Inten    -- 1093.2683  34.1842  34.1842
Atom  AN    X     Y     Z     X     Y     Z     X     Y     Z
  1    6  0.90 -0.06  0.18  0.09 -0.17  0.42 -0.00  0.43  0.17
  2   17 -0.17  0.01 -0.03  0.02  0.04 -0.09  0.00 -0.09 -0.04
  3    1  0.08 -0.02  0.10 -0.23 -0.17  0.42 -0.03  0.46  0.18
  4    1  0.11 -0.07 -0.04 -0.05 -0.18  0.45  0.12  0.44  0.21
  5    1  0.11  0.07 -0.01 -0.01 -0.18  0.46 -0.11  0.46  0.16
  6    9 -0.27  0.02 -0.05  0.04  0.07 -0.17  0.00 -0.18 -0.07
...
```
* You can see one imaginary frequency. This is a sign of the correct TS.
* The vibrational model corresponding the imaginary frequency can be visualized by GaussView. Send .out or .fchk file to GaussView, and see the vibrational motion (just like you have done in vibrational analysis). That vibrational motion is the motion around the saddle point, so basically it is a reaction coordinate connecting the reactant and product states.

## Intrinsic reaction coordinate (IRC)
* Is a calculated TS really connect the reactant and product states? To answer this question, you can perform the **intrinsic reaction coordinate (IRC)** analysis.
* To do this calculation, you should have the fchk file of the vibrational frequency analysis at TS. Then, one should do *two* calculations with following route sections;
```
#P RHF/6-31+G(d) 6D IRC=(READCARTESIANFC,FORWARD,MAXPOINTS=20,STEPSIZE=20)
 GEOM=CHECKPOINT GUESS=CHECKPOINT

forward

-1 1
...
```
```
#P RHF/6-31+G(d) 6D IRC=(READCARTESIANFC,reverse,MAXPOINTS=20,STEPSIZE=20)
 GEOM=CHECKPOINT GUESS=CHECKPOINT

reverse

-1 1
...
```
* These are IRC starting from TS to forward and reverse directions, respectively. Note that forward and reverse directions are somewhat arbitrary so does not correspond to reactant and product, respectively; *always check the geometry.*
* IRC calculation can be also visualized with GaussView.

## Typical procedure for TS optimization
1. First you have to have the geometry of reactant. So optimize the reactant geometry first.
2. After having the reactant geometry, try to modify the molecular structure with GaussView (or other viewers). For example, make two reacting atoms closer (so as to approach the TS-like structure).
3. Then make Gaussian input for that structure, and do geometry optimization by fixing that modified coordinate with *redundant coordinate*.
4. After the optimization, get vibratoinal frequency at that geometry, and see whether the negative eigenvalue is included or not.
5. If negative frequency is found, go next. If not, try to build new geometry (for example, making two atoms even closer) and repeat optimization and freuency analysis.
6. Do geometry optimization for TS.
    * When calculating force constant matrix from the beggining, do `opt=(TS, noeigentest, calcFC)`.
    * You can use calculated force constant by copying previous checkpoint file (.chk) to new checkpoint file (don't forget to specify .chk by setting `%chk=new.chk` in input). Then do `opt=(TS, noeigentest, readFC)`.
    * Redundant coordinate is no longer necessary.
7. When optimization is done and you find *one* imaginary frequency, you finished your job.

* It is better to start with low- or mid-level basis set.
* There are other approaches to do TS optimization, such as using *QST2* or *QST3*, that uses two or three geometries that approximates the TS geometry (close to the dimer method often used in VASP). Try by yourself.

---

## Exercises
1. Locate the TS of SN2 reaction of Cl-CH3 + Cl– --> Cl- + CH3-Cl.
2. Draw the IRC of the above reaction.
