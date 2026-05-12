# Results Summary and Physical Interpretation

This document interprets the uploaded simulation figures for FCC copper-like molecular dynamics using Lennard-Jones interactions and velocity-Verlet integration.

## Key observables
The analysis tracks kinetic, potential, and total energy:

$$
E_{\mathrm{tot}}(t)=E_{\mathrm{kin}}(t)+U(t),
$$

and instantaneous temperature:

$$
T(t)=\frac{2E_{\mathrm{kin}}(t)}{3Nk_B}.
$$

## 1. General behaviour of the molecular dynamics simulation

The simulation starts from an FCC crystal structure. This initial configuration is ordered and close to the expected structure of a metallic solid. After assigning initial velocities, the atoms begin to move around their equilibrium positions due to thermal motion.

At low temperature, the atoms only oscillate weakly around their lattice sites. The crystal remains highly ordered and the local environment of each atom changes only slightly. At higher temperature, the amplitude of the vibrations increases. The atoms explore wider regions of the Lennard-Jones potential well, causing larger changes in force, potential energy and instantaneous temperature.

The general behaviour observed in the figures is consistent with this picture:

- At low temperature, the system remains close to the ideal FCC structure.
- At intermediate temperature, the atoms vibrate more strongly but the structure is still mostly crystalline.
- At high temperature, disorder increases and the local force and potential distributions become broader.
- The radial distribution function confirms the gradual loss of structural order as temperature increases.
- The timestep study shows that too large a timestep introduces numerical instability.
- The system-size study shows that larger systems have smaller relative fluctuations.

Overall, the results are consistent with the expected qualitative behaviour of a finite atomic solid simulated using molecular dynamics.



## 2. Force and potential distribution at different temperatures

The first group of figures shows the distribution of potential energy and forces for configurations at different temperatures.

![Potential/forces at 0 K](../figures/forces/potential_forces_0K.png)

![Potential/forces at 300 K](../figures/forces/potential_forces_300K.png)

![Potential/forces at 1600 K](../figures/forces/potential_forces_1600K.png)

### 2.1 Behaviour at 0 K

At 0 K, the atoms are placed close to their ideal FCC lattice positions. Since there is no thermal motion, the atoms should remain near the minimum-energy configuration. For a perfectly relaxed crystal, the forces on the atoms would ideally be very small because each atom would be located at an equilibrium position.

In the potential-energy map, the distribution is expected to be relatively uniform because most atoms have similar local environments. If periodic boundary conditions are used, the surface effect is removed and the atoms should have almost the same number of neighbours. Therefore, the potential energy per atom should be nearly homogeneous.

If small variations appear at 0 K, they can come from numerical or modelling factors such as the cutoff radius, the finite simulation size, incomplete relaxation of the initial lattice parameter, or the fact that the Lennard-Jones parameters do not perfectly reproduce real copper bonding. Therefore, small residual forces or small energy differences are not necessarily a problem, but they should remain much smaller than at high temperature.

Physically, the 0 K case represents the reference state of the crystal. It allows us to check whether the FCC initialization, Lennard-Jones potential and neighbour calculation are working correctly.

### 2.2 Behaviour at 300 K

At 300 K, the system has thermal energy. The atoms are no longer fixed exactly at their ideal FCC positions, but they oscillate around them. This produces a broader distribution of interatomic distances.

Because the Lennard-Jones force depends strongly on distance, especially at short range, even moderate displacements change the local force and potential energy. Therefore, the 300 K map shows more variation than the 0 K map.

However, the structure should still remain crystalline. The atoms should not move randomly through the box; instead, they should vibrate around their lattice positions. This is the expected behaviour for a solid at room temperature.

The potential-energy distribution at 300 K therefore indicates thermal vibrations, not necessarily melting or structural failure. The important point is that the FCC order is still visible and the system remains stable.

### 2.3 Behaviour at 1600 K

At 1600 K, the thermal energy is much larger. The atoms move further from their equilibrium positions and sample more anharmonic regions of the Lennard-Jones potential. As a result, the force and potential-energy distributions become much broader.

This is physically expected because higher temperature means larger kinetic energy, larger atomic displacements and stronger fluctuations in local environments.

At this temperature, the system may approach a highly disordered solid or a liquid-like regime depending on the simulation parameters, system size and potential model. Since the Lennard-Jones model is only a simplified representation of copper, the exact melting behaviour should not be interpreted quantitatively. However, the qualitative trend is correct: increasing temperature increases disorder and broadens the energy and force distributions.

The 1600 K case therefore shows that the system is much more thermally activated. The atoms no longer behave as small harmonic oscillators around fixed lattice sites, and the local structure becomes less uniform.

## 3. Additional potential-energy map at 800 K

![Potential energy at 800 K final state](../figures/forces/potential_energy_800K_final.png)

The 800 K final-state map represents an intermediate case between the room-temperature simulation and the very high-temperature simulation.

Compared with 300 K, the potential-energy distribution is broader because the atoms vibrate with larger amplitude. Some atoms move closer together or further apart than in the ideal FCC lattice, which changes their local Lennard-Jones energy.

However, compared with 1600 K, the disorder is less extreme. The system still appears more ordered and less heterogeneous than in the high-temperature case.

This result is important because it shows a gradual temperature-dependent trend rather than a sudden numerical artifact. If the simulation were unstable, one might observe abrupt unphysical changes. Instead, the results show that as the temperature increases, the potential-energy landscape becomes progressively more heterogeneous.

This is consistent with the expected physical picture:

- At 0 K, atoms are close to equilibrium.
- At 300 K, atoms vibrate moderately.
- At 800 K, vibrations are stronger and local disorder increases.
- At 1600 K, the system becomes much more disordered.

## 4. Base 300 K thermodynamic simulation

![Base 300 K thermodynamics](../figures/thermodynamics/base_temperature_energy_300K.png)

The base 300 K simulation is one of the most important tests because it shows whether the molecular dynamics algorithm behaves correctly under normal conditions.

The plot tracks the temperature and energies as a function of simulation step. In a stable simulation, the kinetic energy and potential energy should fluctuate, but the total energy should remain approximately conserved.

### 4.1 Temperature evolution

The temperature is calculated from the kinetic energy using $T(t)=\frac{2E_{\mathrm{kin}}(t)}{3Nk_B}$.

At the beginning of the simulation, the temperature may not remain exactly equal to the assigned value. This happens because the initial velocities are assigned artificially and the initial crystal is not necessarily in perfect thermal equilibrium with those velocities.

During the first part of the simulation, the system relaxes. Some kinetic energy can be converted into potential energy as the lattice adjusts. After this initial transient, the temperature fluctuates around an equilibrium value.

These fluctuations are normal. In a finite system, the instantaneous temperature is not perfectly constant because it depends on the instantaneous kinetic energy. A small system shows larger fluctuations, while a larger system gives a smoother temperature curve.

### 4.2 Kinetic and potential energy exchange

The kinetic energy and potential energy usually show opposite trends. When atoms move faster, the kinetic energy increases. As they move through the potential well, the potential energy can decrease or increase depending on their positions.

In a bound solid, the atoms oscillate around equilibrium positions. This naturally produces an exchange between kinetic and potential energy, similar to coupled oscillators.

Therefore, oscillations in $E_{\mathrm{kin}}$ and $U$ are not a problem. They are expected in a conservative system.

### 4.3 Total energy conservation

The key quantity is the total energy, $E_{\mathrm{tot}}=E_{\mathrm{kin}}+U$.

If the velocity-Verlet integrator and force calculation are correct, the total energy should remain approximately constant, with only small bounded fluctuations.

A strong monotonic increase or decrease in total energy would indicate numerical problems. Possible causes would include:

- timestep too large,
- incorrect force sign,
- inconsistent units,
- wrong periodic boundary implementation,
- double counting or missing forces,
- cutoff radius problems,
- atoms starting too close together.

The base 300 K result shows the expected molecular dynamics behaviour if the total energy remains bounded and comparatively stable.

## 5. Effect of initial temperature

![Temperature-energy comparison](../figures/thermodynamics/temperature_energy_comparison.png)

The temperature comparison plot shows how the system behaves when initialized at different temperatures.

Increasing the initial temperature increases the initial kinetic energy because $E_{\mathrm{kin}}\propto T$. Since the atoms move faster, they travel further from their equilibrium positions. This produces larger variations in potential energy.

At low temperature, the atoms remain close to the bottom of the Lennard-Jones well. In this regime, the potential is approximately harmonic, and the crystal behaves like a set of coupled oscillators. The energy fluctuations are smaller and the structure remains ordered.

At higher temperature, the atoms explore more anharmonic parts of the potential. This means that the approximation of small oscillations becomes less valid. The potential-energy fluctuations increase, the force distribution becomes broader, and the structure becomes more disordered.

The observed trend can be interpreted as follows:

- Low temperature gives small oscillations and stable crystalline behaviour.
- Medium temperature gives stronger vibrations but still preserves much of the FCC order.
- High temperature gives large fluctuations and possible loss of long-range order.

It is also expected that the equilibrium temperature may be different from the initial assigned temperature. This is because the system redistributes energy between kinetic and potential forms during the relaxation stage.

Therefore, the temperature comparison confirms that the simulation responds physically to changes in thermal energy.

## 6. Timestep stability study

![Timestep stability comparison](../figures/timestep/timestep_stability_comparison.png)

The timestep stability plot is a numerical test of the integration method.

In molecular dynamics, the timestep $\Delta t$ must be small enough to resolve atomic vibrations. If $\Delta t$ is too large, the atoms move too far in a single step, and the force used by the integrator is no longer a good approximation over that interval.

Velocity-Verlet is much more stable than Euler, but it is not unconditionally stable. It still requires a physically reasonable timestep.

### 6.1 Small timestep

For small timesteps, the integration is more accurate. The total energy should fluctuate only slightly around a constant value. The motion is smooth and the atomic vibrations are well resolved.

The disadvantage is computational cost. A smaller timestep means more steps are needed to simulate the same physical time.

### 6.2 Intermediate timestep

An intermediate timestep, such as a value around the femtosecond scale, usually gives the best compromise. It is small enough to maintain energy conservation but large enough to make the simulation computationally efficient.

In molecular dynamics, this is normally the preferred regime.

### 6.3 Large timestep

For large timesteps, the simulation can become unstable. The total energy may drift upward, which corresponds to artificial numerical heating. The temperature may increase even though no physical heat source is present.

This happens because the integrator no longer follows the real trajectory of the system accurately. The discretization error becomes large enough to inject or remove energy artificially.

A large timestep can also cause atoms to move too close to each other. Since the Lennard-Jones repulsion grows very rapidly at short distances, this can produce extremely large forces and make the simulation explode numerically.

The timestep study therefore confirms that the choice of $\Delta t$ is essential. A stable simulation requires bounded total energy and physically reasonable temperature fluctuations.

## 7. System-size dependence

![Temperature and energy vs system size](../figures/system_size/temperature_energy_vs_system_size.png)

The system-size comparison studies how the number of atoms affects the thermodynamic behaviour.

In statistical mechanics, intensive quantities such as temperature should not depend strongly on system size once the system is large enough. However, the size of fluctuations does depend on the number of particles.

For a finite system, the relative size of fluctuations usually decreases approximately as $\frac{1}{\sqrt{N}}$.

This means that small systems show stronger fluctuations in temperature and energy, while larger systems give smoother curves.

### 7.1 Small systems

Small systems have fewer atoms and therefore fewer degrees of freedom. Each atomic motion has a larger relative effect on the total kinetic energy and temperature.

As a result, the temperature curve can look noisy. The energy exchange between kinetic and potential energy is also more visible.

Small systems are useful because they are fast to simulate, but their results can be strongly affected by finite-size effects.

### 7.2 Large systems

Larger systems contain more atoms and more degrees of freedom. The fluctuations of individual atoms average out more effectively.

Therefore, the temperature and energy curves become smoother. The equilibrium behaviour is more representative of a macroscopic material.

The system-size plot is consistent with this expectation: increasing the number of atoms reduces relative fluctuations and improves statistical stability.

This does not mean that larger systems remove all numerical errors, but they give better thermodynamic averages.

## 8. Radial distribution function analysis

The radial distribution function $g(r)$ is used to analyse the structure of the system. It measures how likely it is to find another atom at a distance $r$ from a reference atom, compared with a completely random distribution at the same density.

For a crystal, $g(r)$ has sharp peaks because atoms are located at well-defined neighbour distances. For a liquid, the peaks become broader and disappear at long distances. For a gas, $g(r)$ approaches 1 quickly after the excluded-volume region.

The RDF is therefore a structural diagnostic. It tells us whether the system is crystalline, liquid-like or disordered.

![OVITO RDF at 0 K](../figures/radial_distribution/ovito_rdf_0K.png)

![OVITO RDF at 300 K](../figures/radial_distribution/ovito_rdf_300K.png)

![OVITO RDF at 800 K](../figures/radial_distribution/ovito_rdf_800K.png)

![OVITO RDF at 1300 K](../figures/radial_distribution/ovito_rdf_1300K.png)

### 8.1 RDF at 0 K

At 0 K, the RDF should show very sharp peaks. These peaks correspond to the neighbour shells of the FCC lattice.

Because atoms are almost exactly at their lattice positions, the interatomic distances are well defined. Therefore, the probability of finding neighbours is concentrated at specific radii.

This is the strongest signature of crystalline order.

The 0 K RDF therefore confirms that the initial FCC structure is correctly generated and that the system has long-range order.

### 8.2 RDF at 300 K

At 300 K, the peaks are still present, but they are broader than at 0 K.

This broadening is caused by thermal vibration. Atoms are no longer fixed exactly at the ideal lattice distances, so each neighbour shell covers a small range of distances instead of a single value.

The important point is that the peaks remain clearly visible. This means that the system is still solid-like and retains FCC order.

The 300 K RDF therefore shows a thermally vibrating crystal, not a disordered gas or liquid.

### 8.3 RDF at 800 K

At 800 K, the peaks become wider and lower. This indicates stronger atomic motion and increased structural disorder.

The first-neighbour peak may still be visible because atoms still have preferred local distances. However, medium-range and long-range peaks are expected to weaken.

This means that the local structure is partly preserved, but the lattice is more distorted than at room temperature.

The 800 K RDF is therefore consistent with an intermediate regime where the system still contains solid-like order but with stronger thermal disorder.

### 8.4 RDF at 1300 K

At 1300 K, the RDF shows a stronger loss of crystalline order. Peaks are expected to be broader and less sharp, especially at larger distances.

This indicates that long-range order is weakening. The system may be approaching a highly disordered solid or a liquid-like configuration depending on the exact simulation conditions.

The first peak can remain visible because even liquids preserve short-range order: atoms still cannot overlap and still prefer certain separations due to the potential minimum. However, the disappearance or weakening of later peaks indicates loss of periodic structure.

Therefore, the RDF trend from 0 K to 1300 K supports the interpretation that increasing temperature progressively destroys crystalline order.

## 9. Interpretation of energy and RDF together

The energy plots and RDF plots provide complementary information.

The energy plots show whether the simulation is numerically stable and how the system exchanges kinetic and potential energy.

The RDF plots show whether the atomic structure remains ordered or becomes disordered.

A physically consistent result should satisfy both conditions:

1. The total energy should be reasonably conserved in NVE simulations.
2. The RDF should evolve consistently with temperature.

For example, at low temperature, we expect stable energy behaviour and sharp RDF peaks. At high temperature, we expect larger energy fluctuations and broader RDF peaks.

The results follow this qualitative trend. This supports the conclusion that the simulation is capturing the expected physical behaviour, at least at a qualitative level.

## 10. Role of periodic boundary conditions

Periodic boundary conditions are important because they remove artificial surfaces.

Without periodic boundaries, atoms at the edge of the crystal have fewer neighbours. Their potential energy is less negative, and they experience a different environment from atoms in the interior.

With periodic boundaries, the simulation box is repeated in space. This makes all atoms behave more like atoms inside an infinite bulk crystal.

This improves the interpretation of the energy and RDF results because the system is not dominated by surface effects.

However, periodic boundaries require the minimum image convention and a correct cutoff condition. The cutoff must satisfy $r_c<\frac{1}{2}\min(L_x,L_y,L_z)$. If not, atoms can interact incorrectly with multiple images.

Therefore, the use of periodic boundary conditions improves the physical realism of the simulation, but it also requires careful numerical implementation.

## 11. Limitations of the simulation

First, the Lennard-Jones potential is a simplified model for copper. Real metallic bonding is better described by potentials such as the Embedded Atom Method. Therefore, the numerical values of melting temperature, energy and defect behaviour should not be interpreted as quantitatively accurate for real copper.

Second, finite-size effects can influence the results. Small systems show larger fluctuations and may not reproduce bulk behaviour perfectly.

Third, the cutoff radius affects the potential energy and forces. If the cutoff is too small, attractive interactions are lost. If it is too large relative to the box, periodic boundary conditions can be violated.

Fourth, high-temperature behaviour should be interpreted carefully. The system may appear disordered, but precise phase-transition conclusions would require longer simulations, larger systems, equilibration checks and a more realistic potential.

Fifth, velocity rescaling is useful for initialization, but it is not a full physical thermostat if applied repeatedly. For true NVT simulations, Langevin or Nose-Hoover thermostats would be more appropriate.

## 13. Final conclusion

The molecular dynamics results show the expected behaviour for an FCC copper-like crystal simulated with Lennard-Jones interactions.

At 0 K, the system remains close to the ideal FCC configuration, with a relatively uniform potential-energy distribution and sharp radial distribution peaks. This confirms that the crystal initialization is correct.

At 300 K, atoms vibrate around their lattice positions. The force and potential distributions broaden slightly, and the RDF peaks become wider, but the crystalline structure is still preserved.

At intermediate temperatures such as 800 K, the system shows stronger thermal disorder. The potential-energy distribution becomes more heterogeneous and the RDF peaks broaden further.

At high temperatures such as 1300 K or 1600 K, the system becomes much more disordered. The atoms explore wider regions of the Lennard-Jones potential, force fluctuations increase and the RDF shows a clear weakening of long-range crystalline order.

The thermodynamic plots show the expected exchange between kinetic and potential energy. The total energy is the key indicator of numerical stability: a good simulation should keep it approximately conserved.

The timestep study confirms that large timesteps reduce integration quality and can introduce artificial heating. The system-size study confirms that larger systems produce smoother thermodynamic curves, consistent with the expected reduction of relative fluctuations as $1/\sqrt{N}$.

Overall, the simulation reproduces the main qualitative features expected from molecular dynamics: thermal vibrations, energy exchange, temperature fluctuations, finite-size effects, timestep sensitivity and progressive structural disorder with increasing temperature. The results are physically consistent as an educational molecular dynamics model, although quantitative predictions for real copper would require a more realistic metallic potential and more extensive convergence tests.













