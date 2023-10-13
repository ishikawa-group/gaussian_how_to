---
marp: true
math: mathjax
---
<!-- headingDivider: 2 -->

## Why quantum chemistry?
* The behavior of the particles is governed by the equation of motion, and its classical mechanical version is known as the Newton's law.
* The proper description of atoms, molecules, and electrons is given by the laws of quantum mechanics. For this reason, we need to consider the **Schrödinger equation**, which is a quantum-mechanical equation of motion.
* If the solutions of the Schrodinger equations are generated without reference to experimental data, the methods are usually called "ab initio" (latin: "from the beginning") or "first principle".

## Schrodinger equation
* SE is

## Hatree-product

## Hartree-Fock method
* The Hartree product obeys the Pauli' exclusion principle only to some extent; in the Hatree product wave function, each electronic state is occupied by one electron. However, it does not take into account the anti-symmetry character of the wave function.
* Mathemtatically, this requres that the sign of $\Psi$ changes when two electrons are exchanged.

## Slater determinant
* The requirement is overcome by using a detarminant. This is called a **Slater determinant**.
* The Slater determinant is constructed from the single-particle wave functions by

$$
\Psi({\bf r_1, r_2}\cdots {\bf r_N}) 
= \frac{1}{\sqrt{N!}}
\begin{vmatrix}
\psi_1({\bf r_1}) & \cdots & \psi_1({\bf r_N}) \\
\vdots            & \ddots & \vdots \\
\psi_N({\bf r_1}) & \cdots & \psi_N({\bf r_N}) \\
\end{vmatrix}
$$

## 

* Then the expectation value of the total energy becomes
$$
\begin{split}
\braket{\Psi|H|\Psi} 
&= \sum_{i=1}^N\int d^3r \psi_i^*({\bf r}) \left(-\frac{\hbar^2}{2m}\nabla^2 + v_{ext}({\bf r}) \right) \psi_i({\bf r}) \\
+& \frac{1}{2}\sum_{i,j=1}^N\int d^3r d^3r' \frac{e^2}{|r-r'|}
\psi_i^*({\bf r})\psi_i({\bf r})\psi_j^*({\bf r'})\psi_j({\bf r'}) \\
-& \frac{1}{2}\sum_{i,j=1}^N\int d^3r d^3r' \frac{e^2}{|r-r'|}
\psi_i^*({\bf r})\psi_i({\bf r'})\psi_j^*({\bf r'})\psi_j({\bf r}) \\
+& V_{nuc-nuc}
\end{split}
$$

##

* The last term $V_{nuc-nuc}$ is the nuclar-nuclear repulsion term, and the second last term is called **exchange repulsion term**.
* If we minimize the above expression with respect to the $\psi_i^*$ under the constraint of normalization.
* Lagrange constrained minimization
* Finally, we have **Hartree-Fock equation**.
$$
\begin{split}
&\left[ -\frac{1}{2}\nabla^2 + v_{ext}({\bf r}) + v_H({\bf r}) \right] \psi_i({\bf r}) \\
& -\sum\int\frac{e^2}{|r-r'|}\psi_j^*({\bf r'})\psi_i({\bf r'})\psi_j({\bf r}) = \epsilon_i \psi_i({\bf r})
\end{split}
$$

* Explain terms.

## Electron correlation
* The Hartree-Fock method generates solutions to the Schrodinger equation where the real electron-electron interaction is replaced by an average interaction.
* In a sufficiently large basis set, the HF wave function accounts for ~99% of the total energy.
* The remaining 1% is often very important for describing chemical phenomena such as chemical bond formation or breaking.
* The difference in energy between the HF and the lowest possible energy in the given basis set is called the **electron correlation energy**.
* As the HF solution usually gives ~99% of the correct answer, electron correlation methods normally use the HF wave function as a starting point for improvements.

## Density Functional Theory
* A *function* is a prescription for producing a number from a set of *variables*.
* A *functional* is a prescription for producing a number from a *function*, which in turn depends on variables.
* A wave function and the electron density are thus *functions*, while the energy depending on a wave function or an electron density is a *functional*.
* We will denote a function depending on a set of variables ${\bf x}$ with $f({\bf x})$, while a functional depending on a function $f$ is denoted as $F[f]$.

## Orbital-free DFT
* The electronic energy functional of electron density $\rho$ can be divided into three parts
    * kinetic energy functional $T[\rho]$
    * the nuclei-electron attraction energy functional $E_{ne}[\rho]$
    * the electron-electron repulsion energy functional $E_{ee}[\rho]$.
* With reference to the Hartree-Fock theory, $E_{ee}[\rho]$ may be divided into Coulomb and exchange parts, $J[\rho]$ and $K[\rho]$.
* Among these energy functionals, $E_{ne}[\rho]$ and $J[\rho]$ can be interpreted by the classical electrodynamics, 
$$
E_{ne}[\rho] = -\sum_i^{N_{nuc}}\int\frac{Z_a(R_a)\rho({\bf r})}{|{\bf R}_a - {\bf r}|}d{\bf r} \\
J[\rho] = \frac{1}{2}\int\int\frac{\rho({\bf r})\rho({\bf r'})}{|{\bf r} - {\bf r'}|}d{\bf r}d{\bf r'}
$$

##

* Here, the factor of 1/2 in $J[\rho]$ allows the integration to be done over all space fo both ${\bf r}$ and ${\bf r'}$ variables. Unlike these energy components, exchange part $K[\rho]$ can only be interpreted by the quantum mechanics.
* Early attempts of deducing functionals for the kinetic and exchange energies considered a uniform electrons gas where it may be shown that $T[\rho]$ and $K[\rho]$ are given by Thomas, Fermi, and Dirac as
$$
T_{TF}[\rho] = C_F \int \rho^{5/3}({\bf r})d{\bf r} \\
K_D[\rho] = -C_X \int \rho^{4/3}({\bf r})d{\bf r} \\
C_F = \frac{3}{10}(3\pi^2)^{2/3}, \ C_X = \frac{3}{4}\left( \frac{3}{\pi} \right)^{1/3}
$$

* Since $T[\rho]$ and $K[\rho]$ functionals are depending directly on the electron density, these methods are called *orbital-free DFT*, as opposed to the Kohn-Sham theory discussed in the next section.
* Unfortunately, the accuracy of the orbital-free DFT is too low to be of general use.

## Kohn-Sham theory
* The main drawback in the orbital-free DFT is the poor representation of the kinetic energy.
* The idea in the Kohn-Sham formalism is to split the kinetic energy functional into two parts, one which can be calculated exactly, and a small correction term.
* Assume for the moment a Hamiltonian operator of the form with $0 \le \lambda \le 1$.
$$
H_{\lambda} = {\bf T} + {\bf V}_{ext}(\lambda) + \lambda {\bf V}_{ee}
$$

* The external potential operator ${\bf V}_{ext}$ is equal to ${\bf V}_{ee}$ for $\lambda = 1$, but for intermediate $\lambda$ value it is assumed that ${\bf V}_{ext}(\lambda)$ is adjusted such that the same density is obtained for $\lambda = 1$ (the real system), for $\lambda = 0$.

##

* For the $\lambda = 0$ case, the electrons are non-interacting, and the exact solution to the Schrodinger equation is given as a Slater determinant.
* The exact kinetic energy functional is
$$
T_{KS} = \sum_i^{N_{el}}\braket{\phi_i|-\frac{1}{2}\nabla^2|\phi_i}
$$

* The $\lambda = 1$ corresponds to interacting electrons, and Eq.X is therefore only an approximation to the real kinetic energy.
* The key to Kohn-Sham theory is to calculate the kinetic energy under the assumption of non-interacting electrons.
* The remaining kinetic energy is absorbed into an exchange-correlation term, and a general DFT energy expression can be written as
$$
E_{DFT}[\rho] = T_{KS}[\rho] + E_{ne}[\rho] + J[\rho] + E_{XC}[\rho]
$$

## Gradient methods and molecular property (from Atkins)
* Once the electronic energy is obtained by solving the electronic Schrodinger equation, a number of molecular properties, perhaps the most important being the equilibrium molecular geometry, can be determined.
* The calculation of molecular or crystal structures is a variable supplement to experimental data in areas of structural chemistry such as X-ray crystallography, electron diffreaction, and microwave spectroscopy.
* Calculation of derivatives of the potential energy with respect to nuclar coordinates is crucial to the efficient determination of equilibrium structures.
* The derivatives can be computed numerically by calculating the potential energy at many geometries and determining the chnge in energy as each nuclear coordiate is varied.

## Energy derivatives
* For a diatomic molecule, the molecular potential energy $E$ depends only on the internuclar distance $R$; therefore, to find the potential minimum (or in general the energy stationary point) we need to locate a zero in $dE/dR$.
* The search is more cpmplicated for polyatomic molecules because the potential energy is a function of many nuclear coordinates, $q_i$.
* At the equilibrium geometry, each of the forced $f_i$ exerted on a nucleus by electrons and other nuclei must vanish:
$$
f_i = -\frac{\partial E}{\partial q_i} = 0 \ \text{(for all i)}
$$
* Therefore, in principle, the equilibrium geometry can be found by computing all the forces at a given molecular geometry and seeing if they vanish.

## Hessian
* A zero gradient characterizes a stationary point on the surface, but does not tell whether it is minima, maxima, or saddle points.
* To distinguish the types of stationary points, it is necessary to consider the second derivatives of the energy with respect to the nuclar coordinates.
* The quantities $\partial^2 E/ \partial q_i \partial q_j$ comprise the **Hessian or Hesse matrix**.
* A maximum (minimum) of a multi-dimensional potential energy surface is characterized by the eigenvalues of the Hessian all being positive (negative).
* A transition state (a first-order saddle point) correspond to one negative eigenvalue and all the rest positive.

## Transition state
* The transition state (TS) is a point on the PES connecting the two energy minima.
* A clear example of this is that the bond dissociation or formation process (Fig.X).
* The minimum at right corresponds to the state that has the O-H bond in this example, while the left minimum corresponds to the state with N-H bond.
* There is a saddle point between them, and the activation energy ($E_a$) can be measured by the energy difference between minima and the saddle point.

## Activation energy 
* When wa have the $E_a$, we can calculate the rate constant of the chemical reaction by Arrhenius equation as
$$
k = A\exp\left(-\frac{E_a}{RT}\right)
$$
where $A$ is the pre-exponential factor and $R$ is the gas constant.
* $A$ is less dependent to molecular species, but $E_a$ is highly dependent.
* Therefore obtaining the transition state structure is critically important topic in the chemical reaction.

## Thermodynamics
* The important thermodynamics quantities such as enthalpy, entropy, Gibbs free energy has contributions from translational, electronic, rotational, and vibrational motions of molecules.
* To calculate them, the partition function $q(V,T)$ is needed.
* From $q(V,T)$ to these quantities, the entropy $S$, internal energy $E$, and heat capacity $C_V$ is obtained as
$$
\begin{align*}
S   &= R + R \ln q + RT \left(\frac{\partial \ln q}{\partial T}\right)_V & \\
E   &= RT^2\left(\frac{\partial \ln q}{\partial T}\right)_V & \\
C_V &= \left(\frac{\partial E}{\partial T}\right)_{N,V} &
\end{align*}
$$

## Partition functions
* The translation partition function is
$$
q_{\rm trans} = \left(\frac{2\pi m k_B T}{h^2}\right)^{3/2}V
$$
* For the non-linear polyatomic molecule, the rotational partition function is
$$
q_{\rm rot} = \frac{\pi^{1/2}}{\sigma_{\rm rot}}\left[ \frac{T^{3/2}}{\Theta_{\rm rot,x}^{1/2}\Theta_{\rm rot,y}^{1/2}\Theta_{\rm rot,z}^{1/2}} \right]
$$
* The vibrational paritition funciton (for mode $i$) and overall one is
$$
\begin{align*}
q_{\rm{vib},i} &= \frac{\exp(-\Theta_i/2T)}{1-\exp(-\Theta_i/T)} & \\
q_{\rm{vib}} &= \prod_i \frac{\exp(-\Theta_i/2T)}{1-\exp(-\Theta_i/T)} &
\end{align*}
$$
