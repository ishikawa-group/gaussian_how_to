<!--
---
marp: true
math: katex
---
-->

<!-- headingDivider: 2 -->

## Why quantum chemistry?
* The behavior of the particles is governed by the equation of motion, and its classical mechanical version is known as the Newton's law.
* The proper description of atoms, molecules, and electrons is given by the laws of quantum mechanics. For this reason, we need to consider the **Schrödinger equation**, which is a quantum-mechanical equation of motion.
* If the solutions of the Schrödinger equations are generated without reference to experimental data, the methods are usually called "ab initio" (latin: "from the beginning") or "first principle".

## The Schrödinger equation
* In chemistry or solid state physics, the fundamental interaction we are interested is the electrostatic interaction.
* Here we introduce three assumptions to the SE
    1. time-independent
    2. non-relativistic
    3. Born-Oppenheimer approximation
* Under these approximations, the system of nuclei and electrons is described with a Hamiltonian below
```math
\hat{H} = T_{\rm nuc} + T_{\rm el} + V_{\rm nuc-nuc} + V_{\rm nuc-el} + V_{\rm el-el}
```

##
* These terms are written in the atomic unit as
```math
\begin{align*}
T_{\rm nuc} &= \sum_{I=1}^L\frac{\nabla_I^2}{2M_I} &\text{(kinetic energy of nuclei)} \\
T_{\rm el}  &= \sum_{i=1}^N\frac{\nabla_i^2}{2} &\text{(kinetic energy of electrons)} \\
V_{\rm nuc-nuc} &= \frac{1}{2}\sum_{I \ne J}\frac{Z_I Z_J}{|{\bf R}_I - {\bf R}_J|} &\text{(nuclei-nuclei repulsion)} \\
V_{\rm nuc-el} &= -\sum_{i,I}\frac{Z_I}{|{\bf r}_i - {\bf R}_I|} &\text{(nuclei-electron attraction)} \\
V_{\rm el-el} &= \frac{1}{2}\sum_{i \ne j}\frac{1}{|{\bf r}_i - {\bf r}_j|} &\text{(electron-electron repulsion)}
\end{align*}
```

##
* In this case, the Schrödinger equation becomes
```math
\hat{H}_{\rm el}(\left\{ {\bf R} \right\}) \Psi({\bf r}, \left\{ {\bf R} \right\}) = E_{\rm el}(\left\{ {\bf R} \right\}) \Psi({\bf r}, \left\{ {\bf R} \right\})
```
* Here, one can make the direct functional relationship (or "mapping") between the nuclear coordinates $\{{\bf R}\}$ and the electronic energy $E_{\rm el}$.
* It is quite informative to visualize $E_{\rm el}(\lbrace{\bf R}\rbrace)$ as the function of $\lbrace{\bf R}\rbrace$, which is often called **potential energy surface**.

## Dirac's braket notation
* It is convenient to use the Dirac's "bra-ket notation" for wave functions and multi-dimensional integrals in electronic structure theory in order to simplify the notation. The equivalences are defined as
```math
\begin{align*}
\ket{\Psi} \equiv \Psi, &\hspace{5pt} \bra{\Psi} \equiv \Psi^{*} \\
\int{d{\bf r} \Psi^{*}\Psi} &= \braket{\Psi|\Psi} \\
\int{d{\bf r} \Psi^{*}\hat{H}\Psi } &= \braket{\Psi|\hat{H}|\Psi}
\end{align*}
```
* The ket $\ket{\Psi}$ denotes a wave function while the bra $\bra{\Psi}$ denotes a complex conjugate wave function $\Psi^{*}$. The combined braket denotes that the whole expression should be integrated over all coordinates.

## Hartree-Fock theory - Introduction
* Except for the simplest cases, there are no simple way to solve the SE in a closed analytical form so we have to solve it numerically.
* The SE is a second-order partial differential equation (PDE), so in principle it can be directly solve. However, this needs integration over a large number of dimensions ($3\times N_{\rm elec}$), which is impossible.
* This difficulty can be solved by two approaches
    1. Approximate the electron-electron interaction by the effective one-electron problem. This reduces the $3N$ dim. integration to a sum of 3 dim. integrations.
    2. Expanding the wave function in some suitable basis set, as this will convert the PDE into a set of algebraic equations.
* The methodology based on above approaches is the **Hartree-Fock-Roothaan equation**, and is the basis of modern quantum chemistry calculations.

## Hartree product
* We are mainly interested in the electronic ground state energy $E_0$.
* There is an important quantum mechanical principle - the *Rayleigh-Ritz variational principle* - that provides a route to find approximate solutions for $E_0$.
* It states that the expectation value of $\hat{H}$ of any $\Psi$ is always higher than or equal to the exact $E_0$, i.e.
```math
E_0 \le \frac{\braket{\Psi|\hat{H}|\Psi}}{\braket{\Psi|\Psi}}
```
* So, the expectation value calculated by the wave function at your hand $\Psi$  will always be an upper bound for the true ground state energy. By improving $\ket{\Psi}$, you will have a lower expectation value and that is closer to the true ground state energy.

##
* Since $V_{\rm nuc-el}$ is an effective external potential for an electron, we write it as function of $r$ and also define the one-electron Hamiltonian $\hat{h}$ as
```math
\hat{v}_{\rm ext}({\bf r}) = -\sum_I \frac{Z_I}{|{\bf r} - {\bf R}_I|}, \hspace{2mm} \hat{h}({\bf r}) = -\frac{\nabla^2}{2} + \hat{v}_{\rm ext}({\bf r})
```
* Then one can form the one-electron SE as
```math
\hat{h}({\bf r})\psi_i({\bf r}) = \epsilon_i\psi_i({\bf r})
```
* In this case, the $N$-electron wave function can be expressed by the product of $\psi_i$ as
```math
\Psi_{\rm HP}({\bf r}_1, {\bf r}_2, \cdots, {\bf r}_N) = \psi_1({\bf r}_1)\psi_2({\bf r}_2)\cdots\psi_N({\bf r}_N)
```
* This wave function is called **Hartree product**, and it is a first crude guess for the true $N$-electron wave function.
* Note that $\psi_i$ is orthonormal thus $\braket{\psi_i({\bf r})|\psi_j({\bf r})} = \delta_{ij}$.

## Spatial and spin orbitals
* The one-electron wave function $\psi_i({\bf r})$ is called the *orbital*, and the Hartree product means that $N$-electron wave function is expressed by the product of orbitals.
* Up to now we assumed that the orbital depends only on ${\bf r}$, but an electron has the spin degree of freedom. We write this spin variable by $\omega$, and combine it with the spatial coordinate ${\bf r}$ as ${\bf x} = ({\bf r}, \omega)$.

##
* Let the one-electron wave function in ${\bf x}$ as $\chi_i({\bf x})$.
* Assuming that ${\bf r}$ and $\omega$ are independent, we have $\chi_i({\bf x}) = \psi_i({\bf r})\sigma_i(\omega)$, where $\psi$ and $\sigma$ denote the spatial and spin parts.
* $\chi$, $\phi$, $\sigma$ are a spin orbital, spatial orbital, and spin function.
* Since an electron have no chance to take both $\alpha$ and $\beta$ spin simultaneously, following integration over the spin variable holds.
```math
\begin{align*}
\int d\omega \alpha^{*}(\omega)\alpha(\omega) = 1 \\
\int d\omega \beta^{*}(\omega)\beta(\omega) = 1 \\
\int d\omega \alpha^{*}(\omega)\beta(\omega) = 0 \\
\int d\omega \beta^{*}(\omega)\alpha(\omega) = 0
\end{align*}
```

## Hartree equation
* Using the spin orbital $\chi$ above, we determine the expectation value of the Hamiltonian
```math
\hat{H} = \hat{h}({\bf r}) + \frac{1}{2}\sum_{i\ne j}^N\frac{1}{|{\bf r}_i - {\bf r}_j|}
```
with respect to the Hartree product. This becomes
```math
\begin{align*}
\braket{\Psi_{\rm HP}|\hat{H}|\Psi_{\rm HP}} = & \sum_{i=1}^N\int d{\bf x} \chi_i^{*}({\bf x}) \hat{h({\bf r})} \chi_i({\bf x}) \\
&+ \frac{1}{2}\sum_{i=1}^N\int d{\bf x} d{\bf x}' \chi_i^{*}({\bf x})\chi_j^{*}({\bf x}') \frac{1}{|{\bf r} - {\bf r}'|} \chi_i({\bf x})\chi_j({\bf x}') \\
&+ V_{\rm nuc-nuc}
\end{align*}
```

##
* Now we minimize this w.r.t. $\chi_i({\bf x})$ under the constraint that $\chi_i^{*}({\bf x})$ is normalized.
* This is a typical variational problem with the constraint taken into account via *Lagrange multipliers*, which gives
```math
\frac{\delta}{\delta\chi_i^{*}}\left[\braket{\Psi_{\rm HP}|\hat{H}|\Psi_{\rm HP}}-\sum_{i=1}^N\left\{\epsilon_i\left(1-\braket{\chi_i|\chi_i}\right)\right\}\right] = 0
```

##
* The $\epsilon_i$ act as Lagrange multipliers ensuring the normalization of $\chi_i({\bf x})$. This leads to the so-called **Hartree equation** as
```math
\left[\hat{h} + \sum_{j=1}^N\int d{\bf x}'\chi_j^{*}({\bf x}')\chi_j({\bf x}')\frac{1}{|{\bf r}-{\bf r}'|}\right]\chi_i({\bf x}) = \epsilon_i\chi_i({\bf x})
```
* This shows that an effective one-electron SE is solved for an electron embedded in the electrostatic field of all electrons (including itself).

## Hartree potential
* Using the electron density
```math
\rho({\bf r}) = \sum_{i=1}^N \psi_i^{*}({\bf r})\psi_i({\bf r})
```
the *Hartree potential* $\hat{v}_H$ can be defined as
```math
\hat{v}_H({\bf r}) = \int d{\bf r}' \frac{\rho({\bf r}')}{|{\bf r} - {\bf r}'|}
```
which corresponds to the electrostatic potential of all electrons. With $\hat{v}_H$, the Hartree equation can be written as
```math
\left[\hat{h} + \hat{v}_H \right]\chi_i({\bf x}) = \epsilon_i\chi_i({\bf x})
```

## Self-consistent field
* The Hartree equation have the form of one-electron SE. However, the solutions $\chi_i({\bf x})$ enter the effective one-particle Hamiltonian via $\hat{v}_H$.
* This dilemma can be resolved in an iterative fashion: One starts with some initial guess for the wave functions which enter the effective one particle Hamiltonian. The Hartree equations are then solved and a new set of solutions are determined.
* This cycle is repeated so often until the iterations no longer modify the solutions, i.e. self-consistency is reached. Such method is known as **self-consistent field (SCF)** method.

## Hartree-Fock method
* The Hartree product obeys the Pauli' exclusion principle only to some extent.
* In the Hartree product wave function, each electronic state is occupied by one electron. However, it does not take into account the anti-symmetry character of the wave function, which is also required by the Pauli principle. 
* This requires that the sign of wave function should change when two electrons are exchanged; this is anti-symmetric charactor of the electronic wave function.

## Slater determinant
* The anti-symmetric problem can be fixed by replacing the product of the one-electron wave function by the determinant of them. This is called a **Slater determinant**, and it has the form of
```math
\Psi({\bf x}_1, {\bf x}_2, \cdots {\bf x}_N) 
= \frac{1}{\sqrt{N!}}
\begin{vmatrix}
\chi_1({\bf x}_1) & \cdots & \chi_1({\bf x}_N) \\
\vdots            & \ddots & \vdots \\
\chi_N({\bf x}_1) & \cdots & \chi_N({\bf x}_N) \\
\end{vmatrix}
```
* For example in two-electron case,
```math
\begin{align*}
\Psi({\bf x}_1,{\bf x}_2) &= \chi_1({\bf x}_1)\chi_2({\bf x}_2) & &\text{(Hatree product)} \\
\Psi({\bf x}_1,{\bf x}_2) &= \chi_1({\bf x}_1)\chi_2({\bf x}_2) - \chi_1({\bf x}_2)\chi_1({\bf x}_2) & &\text{(Slater determinant)}
\end{align*}
```

##
* Then the expectation value of the Hamiltonian with the Slater determinant is
```math
\begin{split}
\braket{\Psi|H|\Psi} 
=& \sum_{i=1}^N\int d{\bf r} \psi_i^*({\bf r}) \hat{h} \psi_i({\bf r}) \\
 &+ \frac{1}{2}\sum_{i,j=1}^N\int d{\bf r} d{\bf r}' \frac{1}{|{\bf r}-{\bf r}'|}
\psi_i^*({\bf r})\psi_i({\bf r})\psi_j^*({\bf r'})\psi_j({\bf r'}) \\
 &- \frac{1}{2}\sum_{i,j=1}^N\int d{\bf r} d{\bf r}' \frac{1}{|{\bf r}-{\bf r}'|}
\delta_{\omega_i, \omega_j}\psi_i^*({\bf r})\psi_i({\bf r'})\psi_j^*({\bf r'})\psi_j({\bf r}) \\
 &+ V_{\rm nuc-nuc}
\end{split}
```
where Kronecker delta $\delta_{\omega_i, \omega_j}$ is coming from the integration in the spin variable.

##
* Just like the Hartree equation, we minimize the expectation value with respect to $\psi^{*}$ under the constraint of normalization. This gives the **Hartree-Fock equation** as
```math
\begin{align*}
\left[ \hat{h} + \hat{v}_H({\bf r}) \right] \psi_i({\bf r}) -\sum_{j \ne i}\int d{\bf r}' \frac{1}{|{\bf r} - {\bf r}'|}\psi_j^*({\bf r}')\psi_i({\bf r}')\psi_j({\bf r}) = \epsilon_i \psi_i({\bf r})
\end{align*}
```
* The last term of the left-hand side corresponds to the *exchange interaction* of electrons, as it exchanges $\psi_i$ into $\psi_j$ when it applies to $\phi_i$. This term arises by replacing the Hartree product into the determinant, and has purely quantum-mechanical character (no classical-mechanics interpretation)
* $\delta_{\omega_i, \omega_j}$ in this term means that the exchange interaction is only present among the electrons of same spin.
* By using the *Fock operator* $\hat{f}$, the HF equation becomes
```math
\hat{f}({\bf x})\chi({\bf x}) = \epsilon_i \chi({\bf x})
```

## Hartree-Fock-Roothaan equation
* The HF equation is a very complicated integro-differential equation, so we expand $\chi_i$ with a suitable basis set $\tilde{\chi_i}$ as
```math
\chi_i({\bf x}) = \sum_{\mu=1}^K C_{\mu i}\tilde{\chi}_{\mu}({\bf x})
```
* Where $C_{\mu i}$ is the expansion coefficient. Lets' introduce the overlap and Fock integrals
```math
S_{\mu \nu} = \int d{\bf x} \tilde{\chi}_{\mu}({\bf x}) \tilde{\chi}_{\nu}({\bf x}), \ \  
F_{\mu \nu} = \int d{\bf x} \tilde{\chi}_{\mu}({\bf x}) \hat{f}({\bf x}) \tilde{\chi}_{\nu}({\bf x})
```
* Then the HF equation becomes the **Hartree-Fock-Roothaan** equation
```math
\sum_{\nu}F_{\mu\nu}C_{\nu i} = \epsilon_i \sum_{\nu}S_{\mu\nu}C_{\nu i} \hspace{5pt}\text{or}\hspace{5pt} {\bf FC} = {\bf SC\epsilon}
```
* This is a general eigenvalue problem, and easily solved.
* As ${\bf F}$ depends on ${\bf C}$, it should be solved by the SCF manner.

## Basis set
* Quantum chemical methods usually describe the electrons by a localized basis set.
* Historically, two localized basis set is commonly known
    1. Slater type functions: $\exp(-\alpha r_{iI})$
    2. Gaussian type functions: $\exp(-\alpha r_{iI}^2)$. 
* $\alpha$ is the exponent of the function and $r_{iI}$ is the distance between the electron $i$ and the nucleus $I$.
* The Gaussian type function is more often used because they allow the analytic evaluation of the matrix elements necessary to perform an electronic structure calculation.
* In GAUSSIAN, only the Gaussian type function is used.

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
```math
\begin{align*}
E_{ne}[\rho] = -\sum_i^{N_{nuc}}\int\frac{Z_a(R_a)\rho({\bf r})}{|{\bf R}_a - {\bf r}|}d{\bf r} \\
J[\rho] = \frac{1}{2}\int\int\frac{\rho({\bf r})\rho({\bf r'})}{|{\bf r} - {\bf r'}|}d{\bf r}d{\bf r'}
\end{align*}
```

##

* Here, the factor of 1/2 in $J[\rho]$ allows the integration to be done over all space fo both ${\bf r}$ and ${\bf r'}$ variables. Unlike these energy components, exchange part $K[\rho]$ can only be interpreted by the quantum mechanics.
* Early attempts of deducing functionals for the kinetic and exchange energies considered a uniform electrons gas where it may be shown that $T[\rho]$ and $K[\rho]$ are given by Thomas, Fermi, and Dirac as
```math
T_{TF}[\rho] = C_F \int \rho^{5/3}({\bf r})d{\bf r} \\
K_D[\rho] = -C_X \int \rho^{4/3}({\bf r})d{\bf r} \\
C_F = \frac{3}{10}(3\pi^2)^{2/3}, \ C_X = \frac{3}{4}\left( \frac{3}{\pi} \right)^{1/3}
```

* Since $T[\rho]$ and $K[\rho]$ functionals are depending directly on the electron density, these methods are called *orbital-free DFT*, as opposed to the Kohn-Sham theory discussed in the next section.
* Unfortunately, the accuracy of the orbital-free DFT is too low to be of general use.

## Kohn-Sham theory
* The main drawback in the orbital-free DFT is the poor representation of the kinetic energy.
* The idea in the Kohn-Sham formalism is to split the kinetic energy functional into two parts, one which can be calculated exactly, and a small correction term.
* Assume for the moment a Hamiltonian operator of the form with $0 \le \lambda \le 1$.
```math
H_{\lambda} = {\bf T} + {\bf V}_{ext}(\lambda) + \lambda {\bf V}_{ee}
```
* The external potential operator $`{\bf V}_{ext}`$ is equal to $`{\bf V}_{ee}`$ for $\lambda = 1$, but for intermediate $\lambda$ value it is assumed that ${\bf V}_{ext}(\lambda)$ is adjusted such that the same density is obtained for $\lambda = 1$ (the real system), for $\lambda = 0$.

##

* For the $\lambda = 0$ case, the electrons are non-interacting, and the exact solution to the Schrodinger equation is given as a Slater determinant.
* The exact kinetic energy functional is
```math
T_{KS} = \sum_i^{N_{el}}\braket{\phi_i|-\frac{1}{2}\nabla^2|\phi_i}
```

* The $\lambda = 1$ corresponds to interacting electrons, and Eq.X is therefore only an approximation to the real kinetic energy.
* The key to Kohn-Sham theory is to calculate the kinetic energy under the assumption of non-interacting electrons.
* The remaining kinetic energy is absorbed into an exchange-correlation term, and a general DFT energy expression can be written as
```math
E_{DFT}[\rho] = T_{KS}[\rho] + E_{ne}[\rho] + J[\rho] + E_{XC}[\rho]
```

## Gradient methods and molecular property
* Once the electronic energy is obtained by solving the electronic Schrodinger equation, a number of molecular properties, perhaps the most important being the equilibrium molecular geometry, can be determined.
* The calculation of molecular or crystal structures is a variable supplement to experimental data in areas of structural chemistry such as X-ray crystallography, electron diffraction, and microwave spectroscopy.
* Calculation of derivatives of the potential energy with respect to nuclear coordinates is crucial to the efficient determination of equilibrium structures.
* The derivatives can be computed numerically by calculating the potential energy at many geometries and determining the change in energy as each nuclear coordinate is varied.

## Energy derivatives
* For a diatomic molecule, the molecular potential energy $E$ depends only on the internuclear distance $R$; therefore, to find the potential minimum (or in general the energy stationary point) we need to locate a zero in $dE/dR$.
* The search is more complicated for polyatomic molecules because the potential energy is a function of many nuclear coordinates, $q_i$.
* At the equilibrium geometry, each of the forced $f_i$ exerted on a nucleus by electrons and other nuclei must vanish:
```math
f_i = -\frac{\partial E}{\partial q_i} = 0 \ \text{(for all i)}
```
* Therefore, in principle, the equilibrium geometry can be found by computing all the forces at a given molecular geometry and seeing if they vanish.

## Hessian
* A zero gradient characterizes a stationary point on the surface, but does not tell whether it is minima, maxima, or saddle points.
* To distinguish the types of stationary points, it is necessary to consider the second derivatives of the energy with respect to the nuclear coordinates.
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
```math
k = A\exp\left(-\frac{E_a}{RT}\right)
```
where $A$ is the pre-exponential factor and $R$ is the gas constant.
* $A$ is less dependent to molecular species, but $E_a$ is highly dependent.
* Therefore obtaining the transition state structure is critically important topic in the chemical reaction.

## Thermodynamics
* The important thermodynamics quantities such as enthalpy, entropy, Gibbs free energy has contributions from translational, electronic, rotational, and vibrational motions of molecules.
* To calculate them, the partition function $q(V,T)$ is needed.
* From $q(V,T)$ to these quantities, the entropy $S$, internal energy $E$, and heat capacity $C_V$ is obtained as
```math
\begin{align*}
S   &= R + R \ln q + RT \left(\frac{\partial \ln q}{\partial T}\right)_V & \\
E   &= RT^2\left(\frac{\partial \ln q}{\partial T}\right)_V & \\
C_V &= \left(\frac{\partial E}{\partial T}\right)_{N,V} &
\end{align*}
```

## Partition functions
* The translation partition function is
```math
q_{\rm trans} = \left(\frac{2\pi m k_B T}{h^2}\right)^{3/2}V
```
* For the non-linear polyatomic molecule, the rotational partition function is
```math
q_{\rm rot} = \frac{\pi^{1/2}}{\sigma_{\rm rot}}\left[ \frac{T^{3/2}}{\Theta_{\rm rot,x}^{1/2}\Theta_{\rm rot,y}^{1/2}\Theta_{\rm rot,z}^{1/2}} \right]
```
* The vibrational partition function (for mode $i$) and overall one is
```math
\begin{align*}
q_{\rm{vib},i} &= \frac{\exp(-\Theta_i/2T)}{1-\exp(-\Theta_i/T)} & \\
q_{\rm{vib}} &= \prod_i \frac{\exp(-\Theta_i/2T)}{1-\exp(-\Theta_i/T)} &
\end{align*}
```
