# Analysis

## Molecular orbital (MO)
1. Do energy calculation with `pop=full`
2. Confirm that `.chk` file is generated
3. Convert it to `.fchk` file by executing `formchk XXX.chk`
4. Copy `.fchk` file to your local environment (by `scp` command)
5. Start the GaussView and clich the MO tab
6. Select the MOs you want to draw, and wait

## Population analysis
* Electron population analysis gives you the electronic charge of elements by calculated wavefunctions.
* In molecules, the chemical bonds are often a mixture of *covalent bond* and *ionic bond*.
* For example, CO molecule is mostly formed by covalent bond so C and O atoms are close to the electrically neutral state.
* However, molecules like LiH has larger contribution of ionic bond so Li is positively charged but H is negatively charged.
* By doing population analysis, you can evaluate the atomic charge of molecules.
* Two types of population analysis is common; 
**Mulliken population analysis** and **Natural population anaolysis (NPA)**.
1. Mulliken
```
pop=full
```

2. NPA
```
#P B3LYP/6-31G(d,p) SCF=TIGHT POP=NBOREAD

water

0 1
(coordinates)

$nbo bndidx $end

``````
* The values can be seen by GaussView.