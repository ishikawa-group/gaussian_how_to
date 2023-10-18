# Transition state
## Background 
* In this lecture, we will see how to locate the transition state (TS) with Gaussian.
* TS is the geometry connecting the reactant (before reaction) and the product system (after reaction).
* The energy difference between the reactant and the TS is called the *activation energy* (for forward direction).
* The energy difference of the product and the TS is also the activation energy, but for reverse direction.
* The energy difference of the reactant and product states is called the *reaction energy*.

## Computation
* Basically, the TS optimization needs the force constant (Hessian) matrix.
* `opt=TS` will do the TS optimization.
* `calcFC`: Calculate the force constant before doing the TS calculation. Use like `opt(TS, calcFC)`
* `readFC`: Read the pre-calculated force constant. Use like `opt(TS, readFC)`

### Redundant coordinate

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
---

## Exercises
1. Locate the TS of SN2 reaction of Cl-CH3 + Cl– --> Cl- + CH3-Cl.
2. Draw the IRC of the above reaction.
