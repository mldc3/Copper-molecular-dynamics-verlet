# Results Summary and Physical Interpretation

This document summarizes the main numerical results obtained in the molecular dynamics simulation of an FCC copper-like crystal using a Lennard-Jones pair potential and velocity-Verlet time integration.

The purpose of the results is not only to show that the code runs, but to verify that the simulation reproduces the expected qualitative behavior of a crystalline solid:

- stable thermal vibrations around FCC lattice sites,
- exchange between kinetic and potential energy,
- approximate conservation of total energy in an NVE simulation,
- sensitivity to the integration time step,
- reduced fluctuations for larger systems,
- structural order visible through radial distribution analysis,
- improved bulk behavior when periodic boundary conditions are used.

The results are interpreted using the physical framework described in `docs/theory.md`.

---

## 1. System studied

The simulated system is initialized as a copper-like crystal with FCC geometry. The atoms interact through the Lennard-Jones potential,

\[
U(r)
=
4\epsilon
\left[
\left(\frac{\sigma}{r}\right)^{12}
-
\left(\frac{\sigma}{r}\right)^6
\right],
\]

using representative parameters for copper-like educational simulations:

\[
\sigma = 2.3151\,\text{\AA},
\qquad
\epsilon = 0.167\,\mathrm{eV},
\qquad
a = 3.603\,\text{\AA}.
\]

The FCC lattice provides the initial crystalline configuration. Once velocities are assigned, the atoms evolve according to Newton's equations. Their motion is integrated with the velocity-Verlet algorithm.

The main observables are:

\[
E_{\mathrm{kin}}(t),
\qquad
U(t),
\qquad
E_{\mathrm{tot}}(t)=E_{\mathrm{kin}}(t)+U(t),
\qquad
T(t).
\]

The instantaneous temperature is obtained from the kinetic energy through

\[
T(t)
=
\frac{2E_{\mathrm{kin}}(t)}{3Nk_B}.
\]

---

## 2. Potential-energy and force distribution

The first diagnostic is the spatial distribution of potential energy per atom and the corresponding force field. These plots are important because they test whether the local environment of each atom is physically reasonable.

At very low temperature, atoms remain close to their ideal FCC equilibrium positions. The potential energy per atom is therefore nearly homogeneous in the bulk of the crystal. In an ideal infinite crystal, every atom would have an equivalent environment and therefore the same local energy.

In a finite simulation without perfect bulk equivalence, small differences may appear near boundaries or in regions where the neighbor environment is not exactly identical. This is physically expected: atoms with fewer effective neighbors are less strongly bound and can have a slightly less negative potential energy.

The force vectors provide a second consistency check. In a well-prepared configuration close to equilibrium, forces should not show a large systematic drift in one direction. Instead, they should reflect local restoring forces produced by deviations from the ideal Lennard-Jones equilibrium separation.

---

## 3. Effect of temperature on local forces and potential energy

At finite temperature, atoms acquire kinetic energy and begin to vibrate around their equilibrium lattice positions. As a result, instantaneous interatomic distances are no longer exactly equal to their equilibrium values. Since the Lennard-Jones force depends strongly on distance, even small thermal displacements produce variations in the force field and in the local potential energy.

At moderate temperatures such as 300 K, the crystal remains ordered, but the distribution of potential energy broadens. This is not a failure of the simulation: it is the expected consequence of thermal vibrations. Atoms explore the bottom of the potential well instead of remaining exactly at its minimum.

At higher temperature, atoms explore a wider region of the potential-energy landscape. The kinetic energy is larger, so atoms move farther away from their equilibrium positions. The local potential-energy distribution becomes more heterogeneous, and the force magnitudes generally increase because atoms sample steeper regions of the Lennard-Jones potential.

At very high temperature, the system shows stronger thermal disorder. The FCC arrangement can still be recognizable over short simulation times, but the displacements are larger and the local environment becomes less uniform. This is consistent with the physical expectation that high temperature weakens the sharpness of crystalline order.

The important point is that temperature does not merely increase particle speed. It also changes the potential-energy landscape sampled by the system. Higher temperature means larger excursions away from equilibrium positions, stronger fluctuations in interatomic distances, and broader distributions of forces and local energies.

---

## 4. Base molecular dynamics run at 300 K

The base simulation initializes the FCC copper-like system at 300 K and follows its evolution under velocity-Verlet dynamics.

The key features expected in this kind of simulation are:

1. an initial transient or thermalization stage,
2. exchange between kinetic and potential energy,
3. stabilization around a fluctuating equilibrium regime,
4. approximate conservation of total energy.

During the first part of the simulation, the random initial velocities are redistributed through interatomic interactions. Some kinetic energy is converted into potential energy because the initially perfect FCC lattice is no longer exactly at the finite-temperature dynamical equilibrium. The atoms begin to vibrate, and the system relaxes from an artificially prepared initial state into a more natural dynamical state.

This explains why the measured temperature can settle below the nominal initialization temperature. The target temperature fixes the initial kinetic energy, but once the system evolves, part of that energy is stored as potential energy in collective vibrational modes.

The kinetic and potential energies are expected to show an anticorrelated behavior:

- when atoms move faster, kinetic energy increases;
- when atoms move farther from equilibrium separations, potential energy increases;
- in a conservative simulation, energy is exchanged between both forms.

The total energy should remain much more stable than either contribution separately. This is a central validation of the velocity-Verlet integrator. If the total energy remains approximately constant, the numerical time step and force calculation are consistent enough for the simulation.

Physically, the oscillations in \(E_{\mathrm{kin}}\), \(U\), and \(T\) can be interpreted as collective vibrational modes of the crystal. In a solid, atoms do not move independently; they oscillate in coupled modes similar to phonons.

---

## 5. Temperature-dependence study

The temperature-dependence study compares simulations initialized at different target temperatures. This test checks whether the code responds physically when the initial kinetic energy is changed.

The expected behavior is:

- higher initial temperature gives higher initial kinetic energy;
- higher kinetic energy leads to larger atomic displacements;
- larger displacements produce stronger potential-energy fluctuations;
- the final equilibrium temperature increases with the initial energy assigned to the system.

This trend is consistent with the equipartition relation,

\[
E_{\mathrm{kin}}
=
\frac{3}{2}Nk_BT.
\]

When \(T\) increases, the mean kinetic energy per atom increases. Since the atoms are interacting, this also affects the potential energy because the atoms move farther from the minimum of the Lennard-Jones potential.

At low temperature, the crystal behaves almost harmonically: atoms oscillate weakly around equilibrium positions. The energy curves are smoother and fluctuations are smaller.

At higher temperature, anharmonic effects become more visible. The Lennard-Jones potential is not a perfect parabola; it is steeply repulsive at short distances and more slowly attractive at longer distances. Therefore, as atoms explore larger displacements, the energy response becomes more nonlinear.

This explains why high-temperature curves show stronger fluctuations and less regular behavior.

---

## 6. Interpretation of thermalization

A subtle point in the results is that the initial temperature is not necessarily equal to the long-time average temperature.

The initialization procedure assigns velocities and rescales them so that the initial kinetic energy corresponds to the desired target temperature. However, the atoms are placed in an ideal FCC geometry, which is a static lattice configuration. Once the simulation begins, the system converts part of the kinetic energy into potential energy as the crystal starts vibrating.

Therefore, the simulation can show a decrease from the target initial temperature to a lower equilibrium value. This does not necessarily indicate an error. It reflects the redistribution of energy between kinetic and potential degrees of freedom.

In an NVE simulation, the conserved quantity is not temperature but total energy:

\[
E_{\mathrm{tot}} = E_{\mathrm{kin}} + U.
\]

Temperature is an instantaneous quantity derived from kinetic energy, so it naturally fluctuates.

---

## 7. Time-step stability

The time-step study is one of the most important numerical validations. Molecular dynamics requires a time step small enough to resolve the fastest relevant atomic vibrations.

The velocity-Verlet method is stable and accurate only if \(\Delta t\) is sufficiently small. If the time step is too large, the algorithm evaluates forces too infrequently. The atoms can move too far between force updates, so the computed acceleration no longer represents the true trajectory accurately.

For small time steps, the simulation shows:

- stable temperature evolution,
- bounded energy fluctuations,
- good total-energy conservation,
- physically meaningful atomic vibrations.

For large time steps, especially values around \(10\,\mathrm{fs}\), the simulation becomes unreliable. Typical symptoms include:

- artificial heating,
- growth of total energy,
- unstable temperature curves,
- poor resolution of force changes,
- unphysical atomic displacements.

This occurs because the integrator injects numerical error into the trajectory. The energy drift is not a physical effect; it is a discretization artifact.

A time step around

\[
\Delta t = 1\,\mathrm{fs}
\]

is a standard compromise for atomistic simulations: small enough to resolve atomic motion, but not so small that the computation becomes unnecessarily expensive.

The result demonstrates an important principle of computational physics: numerical stability is not only about whether the code runs without crashing. A simulation can run and still be physically wrong if the time step is too large.

---

## 8. System-size dependence

The system-size study compares simulations with different numbers of atoms. This test separates intensive and extensive behavior.

Temperature is an intensive quantity. Once the velocities are initialized consistently and the system is sufficiently large, the average equilibrium temperature should not depend strongly on the number of atoms:

\[
T \sim \frac{E_{\mathrm{kin}}}{N}.
\]

The total kinetic and potential energies, on the other hand, are extensive quantities: they scale approximately with the number of atoms.

The most important observed effect is the reduction of fluctuations with increasing system size. In finite systems, temperature fluctuates because it is computed from a finite number of particle velocities. For larger systems, the average is taken over more degrees of freedom, so fluctuations are reduced.

This is consistent with the statistical scaling

\[
\delta T \propto \frac{1}{\sqrt{N}}.
\]

Small systems show stronger fluctuations because each atom contributes significantly to the total kinetic energy. Large systems have smoother curves because individual atomic fluctuations average out.

This result is a useful check that the code captures the expected statistical behavior of many-particle systems.

---

## 9. Radial distribution function and OVITO analysis

The radial distribution function \(g(r)\) is a structural diagnostic. It measures how likely it is to find another atom at distance \(r\) from a reference atom, compared with a uniform distribution.

For a perfect or nearly perfect crystal at low temperature, \(g(r)\) should show sharp peaks. Each peak corresponds to a shell of neighbors in the crystal structure.

For FCC copper at low temperature, the peaks are expected to be narrow and well separated because atoms remain close to well-defined lattice positions.

At 300 K, the peaks broaden because atoms vibrate around their equilibrium positions. The important point is that the peaks do not disappear. Their persistence indicates that the system still has crystalline order.

This is exactly the expected behavior of a solid at moderate temperature:

- local order is preserved,
- atoms fluctuate around lattice sites,
- peaks broaden due to thermal motion,
- the long-range periodic structure remains visible.

At higher temperature, the peaks become broader and less intense. This indicates stronger thermal disorder. The first-neighbor peak can remain visible, but higher-neighbor shells become less sharply defined.

This behavior reflects the gradual loss of positional precision as atomic vibrations increase.

At still higher temperature, the radial distribution function may show a more pronounced reduction in crystalline sharpness. If peaks remain visible, the system still retains significant solid-like order during the simulated time. If peaks become broad and damped, the structure approaches liquid-like behavior.

In this project, the RDF analysis is used qualitatively: it verifies that thermal motion broadens crystalline order without requiring the simulation to reproduce an exact experimental phase transition.

---

## 10. Periodic boundary conditions

Periodic boundary conditions are essential for approximating bulk behavior with a finite number of atoms.

Without periodic boundaries, atoms at the surface have fewer neighbors than atoms in the interior. This produces surface effects:

- higher local energy near surfaces,
- less uniform force distribution,
- stronger dependence on simulation-box size,
- behavior less representative of an infinite material.

With periodic boundary conditions, the simulation box is repeated in all directions. Each atom sees an environment closer to that of a bulk crystal. This improves the physical interpretation of the simulation.

The minimum image convention ensures that each atom interacts only with the closest periodic image of every other atom. This is valid only when the cutoff radius satisfies

\[
r_c &lt; \frac{1}{2}\min(L_x,L_y,L_z).
\]

When this condition is satisfied, the simulation avoids double-counting periodic images and gives a consistent approximation to an extended bulk system.

Periodic boundary conditions are especially important at high temperature, where atoms move more and surface artifacts would otherwise become stronger.

---

## 11. Energy conservation as a validation test

A central result of the project is that the total energy remains much more stable than kinetic or potential energy separately.

This matters because the simulation is effectively an NVE molecular dynamics calculation after initialization. In NVE dynamics,

\[
N,V,E = \mathrm{constant}.
\]

The kinetic and potential energies are allowed to fluctuate, but their sum should remain approximately constant:

\[
E_{\mathrm{tot}}(t)
=
E_{\mathrm{kin}}(t)+U(t)
\approx \mathrm{constant}.
\]

If the total energy drifts systematically, one of the following problems may be present:

- the time step is too large,
- the force expression is inconsistent with the potential,
- periodic boundary conditions are applied incorrectly,
- the cutoff radius violates the minimum image condition,
- units are inconsistent.

In the stable simulations, the total-energy behavior supports the correctness of the implemented workflow.

---

## 12. Physical meaning of kinetic-potential anticorrelation

The anticorrelation between kinetic and potential energy is a characteristic signature of conservative vibrational dynamics.

When atoms move away from their equilibrium positions, the potential energy increases. As they slow down near turning points, kinetic energy decreases. When they move back toward equilibrium, potential energy is converted back into kinetic energy.

This is analogous to a collection of coupled oscillators. In a crystal, these coupled vibrations correspond to phonon-like modes.

Thus, the observed oscillations are not random numerical noise. They reflect the microscopic energy exchange expected in a solid.

---

## 13. Interpretation of high-temperature behavior

As the initial temperature increases, the simulation shows larger fluctuations and stronger disorder. This can be understood directly from the Lennard-Jones potential.

At low temperature, atoms explore only the bottom of the potential well, which is approximately harmonic. The motion is regular and the structure remains close to FCC.

At high temperature, atoms explore more anharmonic parts of the potential. Larger displacements mean that atoms sample both the steep repulsive wall and the weaker attractive tail more strongly. This produces:

- larger force fluctuations,
- broader potential-energy distributions,
- larger temperature fluctuations,
- broader RDF peaks,
- reduced structural sharpness.

The persistence or loss of FCC order depends on temperature, simulation time, boundary conditions and model quality. Since the Lennard-Jones potential is a simplified model for copper, the high-temperature behavior should be interpreted qualitatively rather than as a quantitatively accurate prediction of copper melting.

---

## 14. Limitations of the results

The results demonstrate a complete molecular dynamics workflow, but they should be interpreted with the limitations of the model in mind.

### Lennard-Jones approximation

Copper is a metal, and metallic bonding is not fully described by a pairwise Lennard-Jones potential. A more realistic copper simulation would use a many-body potential such as the Embedded Atom Method.

### Finite-size effects

Even with periodic boundary conditions, the simulated system is finite. Small systems show stronger fluctuations and may not represent all long-wavelength vibrational modes of a macroscopic crystal.

### Time-scale limitations

The simulated time is short compared with many real thermodynamic processes. The results describe short-time thermal motion and qualitative structural stability, not long-time phase evolution.

### Temperature control

The project uses velocity initialization and deterministic evolution. This is appropriate for NVE-style dynamics, but a true NVT simulation would require a thermostat such as Langevin or Nosé-Hoover.

### Qualitative rather than quantitative objective

The goal is to show physical and numerical consistency, not to reproduce exact experimental copper data.

---

## 15. Main conclusions

The results support the following conclusions:

1. The FCC lattice initialization produces a physically reasonable copper-like crystalline configuration.

2. The Lennard-Jones potential generates local restoring forces that keep atoms vibrating around equilibrium positions at moderate temperatures.

3. At 0 K or very low temperature, atoms remain close to equilibrium and the local potential-energy distribution is relatively homogeneous.

4. Increasing temperature produces larger vibrations, broader energy distributions and stronger force fluctuations.

5. In the 300 K base simulation, the system undergoes an initial transient before reaching a fluctuating equilibrium regime.

6. Kinetic and potential energies are anticorrelated, as expected for conservative vibrational dynamics.

7. The total energy is approximately conserved when the time step is sufficiently small, validating the velocity-Verlet implementation.

8. A time step near \(1\,\mathrm{fs}\) provides a good compromise between stability and computational efficiency.

9. Large time steps, such as \(10\,\mathrm{fs}\), lead to numerical artifacts such as artificial heating and loss of stability.

10. Larger systems show smoother temperature and energy curves because finite-size fluctuations decrease approximately as \(1/\sqrt{N}\).

11. Radial distribution analysis confirms solid-like order through sharp or broadened neighbor-shell peaks.

12. Periodic boundary conditions reduce surface artifacts and provide a better approximation to bulk crystalline behavior.

Overall, the project demonstrates a complete computational-physics pipeline: from lattice construction and interatomic forces to time integration, thermodynamic diagnostics and structural interpretation.
