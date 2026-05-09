# Theory Background: Molecular Dynamics of FCC Copper

This document summarizes the theoretical background behind the molecular dynamics project implemented in this repository. The goal is to connect several ingredients of computational physics:

1. numerical integration of Newton's equations,
2. construction of crystalline lattices,
3. interatomic pair potentials,
4. periodic boundary conditions,
5. force calculation,
6. thermodynamic observables,
7. structural analysis through the radial distribution function.

The simulation focuses on a crystalline copper system initialized in a face-centered cubic (FCC) structure and evolved using a Lennard-Jones pair potential and the velocity-Verlet algorithm.

---

## 1. Molecular dynamics as a deterministic simulation method

Molecular dynamics is a numerical method used to follow the time evolution of a system of interacting particles. Instead of sampling configurations randomly, as in Monte Carlo methods, molecular dynamics constructs an explicit trajectory by solving Newton's equations of motion.

For a system of \(N\) particles with positions \(\mathbf{r}_i(t)\), velocities \(\mathbf{v}_i(t)\), masses \(m_i\), and total potential energy \(U(\mathbf{r}_1,\ldots,\mathbf{r}_N)\), the equations of motion are

\[
m_i \frac{d^2 \mathbf{r}_i}{dt^2} = \mathbf{F}_i,
\]

where the force on particle \(i\) is obtained from the gradient of the potential energy,

\[
\mathbf{F}_i = -\nabla_i U.
\]

Equivalently, by introducing the velocity,

\[
\frac{d\mathbf{r}_i}{dt} = \mathbf{v}_i,
\qquad
\frac{d\mathbf{v}_i}{dt} = \frac{\mathbf{F}_i}{m_i}.
\]

This formulation reduces the problem to integrating a coupled system of ordinary differential equations.

In this project, the relevant physical picture is that atoms in a crystal are initially placed near equilibrium positions. At finite temperature, they do not remain static: they vibrate around those positions. Molecular dynamics makes this microscopic motion explicit.

---

## 2. Why numerical integration is needed

For realistic many-particle systems, the coupled equations of motion cannot usually be solved analytically. The trajectory must therefore be approximated at discrete times

\[
t_n = n\Delta t,
\]

where \(\Delta t\) is the integration time step.

The choice of numerical integrator is essential. A poor method can introduce artificial energy drift, unstable trajectories, or unphysical heating. For conservative systems, such as atoms interacting through a time-independent potential, a good integrator should preserve the total energy as well as possible over long times.

---

## 3. Overview of common integration schemes

### 3.1 Euler method

The simplest method is the explicit Euler scheme. For a first-order equation

\[
\frac{dx}{dt}=f(x,t),
\]

Euler integration gives

\[
x_{n+1}=x_n+\Delta t\,f(x_n,t_n).
\]

The method is easy to implement, but it is only first order in the time step. Its global error scales as \(O(\Delta t)\), and it can be unstable unless the time step is very small.

In molecular dynamics, Euler integration is generally not preferred because it tends to accumulate energy errors quickly.

---

### 3.2 Runge-Kutta methods

Runge-Kutta methods improve on Euler's method by evaluating the derivative at intermediate points inside each time interval.

A common second-order method is the midpoint method:

\[
k_1 = \Delta t\, f(x_n,t_n),
\]

\[
k_2 = \Delta t\, f\left(x_n+\frac{k_1}{2},t_n+\frac{\Delta t}{2}\right),
\]

\[
x_{n+1}=x_n+k_2.
\]

The fourth-order Runge-Kutta method evaluates four slopes:

\[
k_1 = \Delta t\, f(x_n,t_n),
\]

\[
k_2 = \Delta t\, f\left(x_n+\frac{k_1}{2},t_n+\frac{\Delta t}{2}\right),
\]

\[
k_3 = \Delta t\, f\left(x_n+\frac{k_2}{2},t_n+\frac{\Delta t}{2}\right),
\]

\[
k_4 = \Delta t\, f(x_n+k_3,t_n+\Delta t),
\]

and combines them as

\[
x_{n+1}=x_n+\frac{1}{6}(k_1+2k_2+2k_3+k_4).
\]

Runge-Kutta methods are accurate and general, but they are not the standard choice for long molecular dynamics simulations because they do not preserve the geometric structure of Hamiltonian dynamics as well as symplectic methods.

---

### 3.3 Leapfrog method

The leapfrog method is designed for second-order mechanical systems. It uses staggered positions and velocities:

\[
\mathbf{v}_{n+\frac{1}{2}}
=
\mathbf{v}_n
+
\frac{\Delta t}{2}\mathbf{a}_n,
\]

\[
\mathbf{r}_{n+1}
=
\mathbf{r}_n
+
\Delta t\,\mathbf{v}_{n+\frac{1}{2}},
\]

\[
\mathbf{v}_{n+\frac{3}{2}}
=
\mathbf{v}_{n+\frac{1}{2}}
+
\Delta t\,\mathbf{a}_{n+1}.
\]

Its main advantage is that it is time-reversible and has good long-time energy behavior for conservative systems.

---

## 4. Verlet and velocity-Verlet integration

The Verlet family of algorithms is widely used in molecular dynamics because it is explicit, second order, time-reversible, and stable for long simulations when the time step is chosen properly.

The position-Verlet method follows from the Taylor expansions of the position at \(t+\Delta t\) and \(t-\Delta t\). Adding the two expansions cancels the odd derivatives and gives

\[
\mathbf{r}_{n+1}
=
2\mathbf{r}_n
-
\mathbf{r}_{n-1}
+
\mathbf{a}_n \Delta t^2
+
O(\Delta t^4).
\]

This updates positions using the current position, previous position and current acceleration. However, molecular dynamics also needs velocities to compute kinetic energy and temperature. For this reason, this project uses the velocity-Verlet formulation.

The velocity-Verlet algorithm is:

\[
\mathbf{r}_{n+1}
=
\mathbf{r}_n
+
\mathbf{v}_n\Delta t
+
\frac{1}{2}\mathbf{a}_n\Delta t^2,
\]

\[
\mathbf{a}_{n+1}
=
\frac{\mathbf{F}_{n+1}}{m},
\]

\[
\mathbf{v}_{n+1}
=
\mathbf{v}_n
+
\frac{\Delta t}{2}
\left(
\mathbf{a}_n+\mathbf{a}_{n+1}
\right).
\]

This is the central time integration method used in the project.

The time step must be small enough to resolve atomic vibrations. A typical molecular dynamics time step is on the order of

\[
\Delta t \sim 1\,\mathrm{fs}=10^{-15}\,\mathrm{s}.
\]

If the time step is too large, the force changes too much between steps and the integrator can inject artificial energy into the system, causing numerical instability.

---

## 5. Crystalline lattices and FCC copper

A crystalline solid is described by a periodic arrangement of atoms. Mathematically, a Bravais lattice is generated by integer combinations of three primitive vectors:

\[
\mathbf{R}
=
n_1\mathbf{a}_1
+
n_2\mathbf{a}_2
+
n_3\mathbf{a}_3,
\qquad
n_1,n_2,n_3 \in \mathbb{Z}.
\]

A crystal structure is obtained by attaching a basis of atoms to each lattice point.

Copper crystallizes in a face-centered cubic structure. The conventional FCC unit cell contains atoms at:

\[
(0,0,0),
\]

\[
\left(\frac{1}{2},\frac{1}{2},0\right),
\]

\[
\left(\frac{1}{2},0,\frac{1}{2}\right),
\]

\[
\left(0,\frac{1}{2},\frac{1}{2}\right),
\]

in units of the lattice parameter \(a\).

Therefore, each conventional FCC cell contributes four atoms. A simulation box built from \(n_x \times n_y \times n_z\) conventional FCC cells contains

\[
N = 4 n_x n_y n_z
\]

atoms.

For the copper-like system used in this project, the representative lattice parameter is

\[
a = 3.603\,\text{\AA}.
\]

The FCC structure is a natural starting point for copper molecular dynamics because it represents the equilibrium crystalline arrangement before thermal motion is introduced.

---

## 6. Interatomic interactions and pair potentials

The total potential energy of a many-particle system can be approximated using a pair potential, where the interaction energy depends only on the distance between two atoms:

\[
U = \frac{1}{2}\sum_{i\neq j} U(r_{ij}).
\]

The factor \(1/2\) avoids double-counting pair interactions, because the pair \(i,j\) and the pair \(j,i\) represent the same physical interaction.

The distance between two atoms is

\[
r_{ij}=|\mathbf{r}_j-\mathbf{r}_i|.
\]

In this project, the interaction is described using the Lennard-Jones potential.

---

## 7. Lennard-Jones potential

The Lennard-Jones potential is

\[
U(r)
=
4\epsilon
\left[
\left(\frac{\sigma}{r}\right)^{12}
-
\left(\frac{\sigma}{r}\right)^6
\right].
\]

It contains two physical contributions:

- the \(r^{-12}\) term is strongly repulsive at short distances,
- the \(r^{-6}\) term is attractive at longer distances and represents dispersion / van der Waals attraction.

The parameter \(\epsilon\) controls the depth of the potential well, while \(\sigma\) is the distance at which the potential crosses zero:

\[
U(\sigma)=0.
\]

The minimum of the Lennard-Jones potential occurs at

\[
r_{\mathrm{min}} = 2^{1/6}\sigma.
\]

This is the preferred pair separation in the isolated two-body interaction.

For copper-like simulations, the representative parameters used are

\[
\epsilon = 0.167\,\mathrm{eV},
\]

\[
\sigma = 2.3151\,\text{\AA},
\]

\[
a = 3.603\,\text{\AA}.
\]

The Lennard-Jones potential is a simplified empirical potential. It captures basic repulsion and attraction, but it is not a fully realistic metallic potential. More advanced descriptions of metals often use many-body potentials such as the Embedded Atom Method (EAM), where the energy of each atom depends on the local electronic density generated by its neighbors.

Nevertheless, Lennard-Jones is useful for educational molecular dynamics because it allows the complete workflow to be implemented transparently: lattice generation, pair distances, forces, energy, temperature and time integration.

---

## 8. Force from the Lennard-Jones potential

For a central pair potential \(U(r)\), the force between particles is obtained from

\[
\mathbf{F}_{ij}
=
-
\frac{dU}{dr}
\hat{\mathbf{r}}_{ij}.
\]

Here,

\[
\hat{\mathbf{r}}_{ij}
=
\frac{\mathbf{r}_{ij}}{r_{ij}}
\]

is the unit vector pointing along the separation vector.

For the Lennard-Jones potential,

\[
U(r)
=
4\epsilon
\left[
\sigma^{12}r^{-12}
-
\sigma^6 r^{-6}
\right],
\]

so

\[
\frac{dU}{dr}
=
4\epsilon
\left[
-12\frac{\sigma^{12}}{r^{13}}
+
6\frac{\sigma^6}{r^7}
\right].
\]

Equivalently, the scalar force magnitude can be written as

\[
-\frac{dU}{dr}
=
24\epsilon
\left[
2\frac{\sigma^{12}}{r^{13}}
-
\frac{\sigma^6}{r^7}
\right].
\]

The net force on atom \(i\) is the sum over all neighbors:

\[
\mathbf{F}_i
=
\sum_{j\neq i}\mathbf{F}_{ij}.
\]

A useful consistency check is that the total force over all atoms should be approximately zero:

\[
\sum_i \mathbf{F}_i \approx 0.
\]

For pair forces satisfying Newton's third law, the force exerted by \(j\) on \(i\) is opposite to the force exerted by \(i\) on \(j\).

---

## 9. Cutoff radius

The Lennard-Jones potential decays rapidly with distance. Computing all pair interactions for all atoms scales as \(O(N^2)\), which becomes expensive for large systems. Therefore, a cutoff radius \(r_c\) is introduced:

\[
U(r_{ij}) \approx 0
\qquad
\text{for}
\qquad
r_{ij}>r_c.
\]

A common educational choice is

\[
r_c \approx 3\sigma.
\]

The cutoff must be chosen carefully:

- if \(r_c\) is too small, important attractive interactions are missed;
- if \(r_c\) is too large, the simulation becomes expensive;
- under periodic boundary conditions, \(r_c\) must be smaller than half the smallest box length.

The last condition is essential for the minimum image convention:

\[
r_c < \frac{1}{2}\min(L_x,L_y,L_z).
\]

If this condition is violated, an atom may interact with more than one periodic image of another atom, producing unphysical results.

---

## 10. Periodic boundary conditions

A real macroscopic material contains an enormous number of atoms. A direct simulation of all of them is impossible, so molecular dynamics uses a finite simulation cell. If the finite cell had open boundaries, atoms at the surface would have fewer neighbors than atoms in the bulk. This would introduce strong surface effects.

Periodic boundary conditions solve this problem by replicating the simulation box infinitely in all directions. If a particle leaves the box through one side, it re-enters from the opposite side.

For a cubic or orthorhombic box with lengths

\[
\mathbf{L} = (L_x,L_y,L_z),
\]

particle positions can be wrapped back into the box using

\[
\mathbf{r}_i \leftarrow \mathbf{r}_i \bmod \mathbf{L}.
\]

Periodic boundary conditions make the finite simulation cell behave like a small representative piece of an infinite bulk material.

---

## 11. Minimum image convention

When periodic boundary conditions are used, each particle has infinitely many periodic images. To avoid counting multiple copies, the minimum image convention is applied: each particle interacts only with the nearest periodic image of every other particle.

For a separation vector

\[
\Delta \mathbf{r}_{ij}
=
\mathbf{r}_j-\mathbf{r}_i,
\]

the minimum image correction can be written component-wise as

\[
\Delta \mathbf{r}_{ij}
\leftarrow
\Delta \mathbf{r}_{ij}
-
\mathbf{L}
\cdot
\mathrm{round}
\left(
\frac{\Delta \mathbf{r}_{ij}}{\mathbf{L}}
\right).
\]

After this correction, the effective distance is

\[
r_{ij}=|\Delta \mathbf{r}_{ij}|.
\]

This ensures that the force and energy are computed using the closest periodic image.

---

## 12. Initial velocities and temperature

To start a molecular dynamics simulation at a target temperature, each atom must be assigned an initial velocity.

At thermal equilibrium, velocity components follow the Maxwell-Boltzmann distribution. For one Cartesian component,

\[
f(v)
=
\sqrt{\frac{m}{2\pi k_B T}}
\exp\left(
-\frac{m v^2}{2k_B T}
\right).
\]

In practice, this project uses a simple initialization procedure:

1. assign random velocities,
2. remove center-of-mass drift,
3. compute the initial temperature,
4. rescale velocities to the desired target temperature.

The kinetic energy is

\[
E_{\mathrm{kin}}
=
\sum_i
\frac{1}{2}m_i|\mathbf{v}_i|^2.
\]

For \(N\) atoms in three dimensions, the equipartition theorem gives

\[
E_{\mathrm{kin}}
=
\frac{3}{2}Nk_BT.
\]

Therefore, the instantaneous temperature can be estimated as

\[
T
=
\frac{2E_{\mathrm{kin}}}{3Nk_B}.
\]

If the initial random velocities produce a temperature \(T_0\), they can be rescaled to a desired temperature \(T_{\mathrm{target}}\) using

\[
s
=
\sqrt{\frac{T_{\mathrm{target}}}{T_0}},
\]

\[
\mathbf{v}_i
\leftarrow
s\mathbf{v}_i.
\]

---

## 13. Removing center-of-mass drift

Randomly initialized velocities may give the entire system a nonzero center-of-mass velocity. This is not desirable when simulating equilibrium vibrations of a crystal, because the whole simulation box would drift through space.

The center-of-mass velocity is

\[
\mathbf{v}_{\mathrm{CM}}
=
\frac{\sum_i m_i \mathbf{v}_i}{\sum_i m_i}.
\]

It is removed by applying

\[
\mathbf{v}_i
\leftarrow
\mathbf{v}_i
-
\mathbf{v}_{\mathrm{CM}}.
\]

After this correction, the system has no global translational motion, and the kinetic energy corresponds to internal thermal motion rather than motion of the whole sample.

---

## 14. Thermodynamic observables

During the simulation, several observables are monitored.

### Kinetic energy

\[
E_{\mathrm{kin}}(t)
=
\sum_i
\frac{1}{2}m_i|\mathbf{v}_i(t)|^2.
\]

### Potential energy

\[
U(t)
=
\frac{1}{2}
\sum_{i\neq j}
U(r_{ij}(t)).
\]

### Total energy

\[
E_{\mathrm{tot}}(t)
=
E_{\mathrm{kin}}(t)+U(t).
\]

In a microcanonical molecular dynamics simulation, where \(N\), \(V\) and \(E\) are fixed, the total energy should remain approximately constant. Small fluctuations can appear due to numerical discretization, but a systematic drift usually indicates that the time step is too large or that the force calculation is inconsistent.

### Temperature

The instantaneous temperature is computed from the kinetic energy:

\[
T(t)
=
\frac{2E_{\mathrm{kin}}(t)}{3Nk_B}.
\]

In finite systems, temperature fluctuates. These fluctuations are physical and become smaller as the system size increases.

---

## 15. Ensembles: NVE and NVT

Two important statistical ensembles appear in molecular dynamics.

### Microcanonical ensemble: NVE

In the microcanonical ensemble,

\[
N,V,E=\mathrm{constant}.
\]

This is the natural ensemble for a standard velocity-Verlet simulation without a thermostat. The total energy is conserved, while the kinetic and potential energies exchange with each other.

### Canonical ensemble: NVT

In the canonical ensemble,

\[
N,V,T=\mathrm{constant}.
\]

The system is coupled to a heat bath at fixed temperature. In simulations, this requires a thermostat or periodic velocity rescaling.

Velocity rescaling is useful to prepare the initial temperature, but if applied too aggressively at every step, it can distort the physical velocity distribution. More realistic thermostat methods include Langevin dynamics and Nosé-Hoover dynamics.

This repository focuses on the deterministic velocity-Verlet molecular dynamics workflow and uses velocity scaling mainly as an initialization tool.

---

## 16. Langevin and Nosé-Hoover thermostats as context

Although the core simulation uses velocity-Verlet dynamics, it is useful to understand the role of thermostats.

### Langevin thermostat

A Langevin thermostat adds friction and random forces:

\[
m_i\frac{d^2\mathbf{r}_i}{dt^2}
=
\mathbf{F}_i
-
\gamma m_i\mathbf{v}_i
+
\mathbf{G}_i(t).
\]

The friction term removes energy, while the random force injects thermal fluctuations. The balance between both drives the system toward a canonical distribution.

### Nosé-Hoover thermostat

A Nosé-Hoover thermostat introduces an additional deterministic degree of freedom that exchanges energy with the physical system. If tuned properly, it can reproduce canonical sampling without random forces.

These methods are important in advanced molecular dynamics, but they are not required for the basic NVE simulation presented here.

---

## 17. Energy minimization

Before running finite-temperature dynamics, it is often useful to relax a structure by minimizing its potential energy. This finds a stable configuration close to a local minimum of \(U\).

A simple gradient-based idea is to move atoms along the force direction, because

\[
\mathbf{F}_i=-\nabla_i U.
\]

If the forces are small,

\[
|\mathbf{F}_i| < \mathrm{tolerance},
\]

the system is close to mechanical equilibrium.

More advanced minimization algorithms include conjugate gradient and quasi-Newton methods. In Python, these can be accessed through optimization tools such as `scipy.optimize.minimize`.

In the present project, the FCC lattice is already close to an equilibrium crystalline configuration for copper-like parameters, so the main focus is time evolution rather than energy minimization.

---

## 18. Radial distribution function

The radial distribution function \(g(r)\) is used to characterize the structure of the system. It measures the probability of finding another particle at distance \(r\) from a reference particle, compared with a completely uniform distribution of the same density.

For a system with number density

\[
\rho=\frac{N}{V},
\]

the radial distribution function is obtained by counting pairs in spherical shells of radius \(r\) and thickness \(\Delta r\), and normalizing by the shell volume:

\[
4\pi r^2\Delta r.
\]

Qualitatively:

- for a gas, \(g(r)\to 1\) at large distances and has little long-range structure;
- for a liquid, \(g(r)\) has broad peaks at short distances, showing local order;
- for a crystal, \(g(r)\) has sharp peaks at well-defined neighbor distances, showing long-range order.

For a crystalline FCC copper system at low temperature, \(g(r)\) should display sharp peaks corresponding to ordered neighbor shells. As temperature increases, atoms vibrate more strongly around their equilibrium positions, so the peaks broaden and their height decreases.

Thus, \(g(r)\) provides a structural diagnostic complementary to the energy and temperature curves.

---

## 19. Physical interpretation of temperature effects

At \(T=0\), atoms occupy equilibrium positions in the potential energy landscape. The potential energy is minimized, and thermal motion is absent.

At finite temperature, atoms vibrate around their lattice sites. The kinetic energy increases, and the potential energy also fluctuates because atoms move away from their equilibrium separations.

At higher temperatures:

- atomic vibrations become larger,
- instantaneous forces increase,
- the peaks of \(g(r)\) broaden,
- energy and temperature fluctuations become larger,
- sufficiently high temperatures may eventually destroy crystalline order.

In this educational project, the aim is not to reproduce a quantitatively exact copper melting transition. Instead, the aim is to show how molecular dynamics captures the qualitative connection between temperature, atomic motion, energy exchange and structural order.

---

## 20. Numerical consistency checks

A molecular dynamics implementation should be checked using several diagnostics:

### Energy conservation

For an NVE simulation,

\[
E_{\mathrm{tot}}(t)
=
E_{\mathrm{kin}}(t)+U(t)
\]

should remain approximately constant. A strong drift suggests that the time step is too large or the force calculation is inconsistent.

### Force consistency

The net force should be close to zero:

\[
\sum_i\mathbf{F}_i \approx 0.
\]

This tests whether pair forces are applied symmetrically.

### Cutoff validity

The cutoff radius must obey

\[
r_c < \frac{1}{2}\min(L_x,L_y,L_z).
\]

This ensures the minimum image convention remains valid.

### Time-step stability

A small time step improves energy conservation but increases computational cost. A large time step is faster but can produce artificial heating or unstable trajectories. A practical simulation must balance accuracy and efficiency.

### System-size dependence

Temperature is an intensive quantity and should not depend systematically on the number of atoms once the simulation is properly initialized. However, fluctuations decrease as the number of particles increases, approximately following a \(1/\sqrt{N}\) trend.

---

## 21. Limitations of the model

The Lennard-Jones potential is not a fully realistic potential for metallic copper. Copper bonding is metallic and depends on the local electronic environment. A more realistic description would use many-body potentials such as the Embedded Atom Method.

However, the Lennard-Jones model is valuable for demonstrating the computational workflow of molecular dynamics:

- generate a crystal,
- compute pair distances,
- apply boundary conditions,
- compute forces,
- integrate Newton's equations,
- monitor thermodynamic quantities,
- analyze structural order.

This makes it an appropriate model for a compact computational physics portfolio project.

---

## 22. Summary

This project combines several fundamental ideas in computational physics:

- deterministic molecular dynamics,
- FCC crystal generation,
- Lennard-Jones pair interactions,
- force calculation from a potential,
- cutoff radii,
- periodic boundary conditions,
- minimum image convention,
- velocity initialization at a target temperature,
- center-of-mass drift removal,
- velocity-Verlet integration,
- kinetic, potential and total energy diagnostics,
- instantaneous temperature,
- finite-size effects,
- radial distribution analysis.

Together, these elements form a complete molecular dynamics workflow for studying the qualitative thermal and structural behavior of an FCC copper-like crystal.
