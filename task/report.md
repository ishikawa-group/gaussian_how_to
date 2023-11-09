## Report for Materials Simulation
* Choose one of the following two tasks, depending on your background.
* After finishing the task, send some file by e-mail that shows your jobs is completed (e.g. Gaussian output file, screenshot capture/movie). Long document is not necessary.

## 1. Olefin metathesis
### Background
* Olefin metathesis is one of the popular reaction in organic/organometallic chemistry, which converts the C=C double bond between two olefins.

<p align=center>
<img src=../fig/olefin_metathesis.png width=60%>
</p>

* To proceed this reaction, some catalysts are needed.
* The one called **Grubbs catalyst** below is often used for olefin metathesis.
* Below is an example of Grubbs catalysts (called first generation Grubbs catalysts)

<p align=center>
<img src=../fig/grubbs.png width=30%>
</p>

* The mechanism for olefin metathesis is considered as below.

<p align=center>
<img src=../fig/grubbs_mech.png width=90%>
* Ref.: JACS, 2002, 124, 8965-8973 9 8965
</p>

1. One of the phosphine group is removed, and some olefin coordinate to the metal center.
2. Coordinating olefin and an original olefin (with Ru=C part) makes *metallacyclic* structure, which is a transition state (TS).
3. The two C=C bonds are interchanged.

### Task
* Optimize the TS, and confirm that the TS gives correct new olefin product (from the imaginary frequency).

#### Hint
* Any method is OK, but DFT (with some popular functional such as B3LYP) is recommended.
* Low-mid level basis set such as 3-21G* or 6-31G* is OK.
* The phosphine group can be modelled with small ligands such as PH3.

## 2. UV/Vis spectrum of metal complex
### Background
* Some metal complexes are luminescent material, so they can be used for many purposes like making displays, LED, solar cell, etc.
* A metal complex has the metel center and ligands. Because of this, several types of excitations are known for metal complex.
    * MC: metal centered
    * LC: ligand centered
    * LLCT: ligand-to-ligand charge transfer
    * MLCT: metal-to-ligand charge transfer
    * LMCT: ligand-to-metal charge transfer

### Task
* By taking the square-planer Pt complex as an example, perform the excited state calculation and confirm that several types of above excitations happens.

<p align=center>
<img src=../fig/pt_complex.png width=30%>
</p>

#### Hint
* Do geometry optimization first.
* Use DFT, but better functions like CAM-B3LYP is recommended.
* Low-mid level basis set like 3-21G* or 6-31G* is OK.
* Increase the number of excited states in TD-DFT calculations when your calculation doesn't cover wide spectrum region.
