# Copper Molecular Dynamics with Velocity-Verlet Integration

## 1. Project description
This repository contains an educational molecular dynamics (MD) simulation of copper-like atoms initialized on an FCC lattice and evolved with the velocity-Verlet integrator under a Lennard-Jones pair interaction.

## 2. Scientific motivation
The project studies how atomistic simulations connect microscopic interactions to macroscopic observables such as temperature, energy exchange, structural order, and numerical stability.

## 3. Physical model
Atoms are treated as classical particles with mass $m$, positions $\mathbf{r}_i$, and velocities $\mathbf{v}_i$, evolving under Newton's equations:

$$
m\frac{d^2\mathbf{r}_i}{dt^2} = \mathbf{F}_i.
$$

## 4. Lennard-Jones potential
Pair interactions are modeled with:

$$
U(r) = 4\epsilon\left[\left(\frac{\sigma}{r}\right)^{12} - \left(\frac{\sigma}{r}\right)^6\right].
$$

The force is obtained from $\mathbf{F}_{ij} = -\nabla U(r_{ij})$.

## 5. FCC copper lattice
Initial positions are generated from an FCC unit-cell basis replicated in $x,y,z$ to produce finite crystal samples used in temperature and system-size studies.

## 6. Periodic boundary conditions
The simulation uses periodic boundaries to reduce finite-size boundary artifacts and approximate bulk behavior.

## 7. Minimum image convention
Distances are computed with minimum-image wrapping so each pair interacts through the shortest periodic separation.

## 8. Velocity-Verlet integration
Time evolution uses velocity-Verlet:

$$
\mathbf{r}_{n+1} = \mathbf{r}_n + \mathbf{v}_n\Delta t + \frac{1}{2}\mathbf{a}_n\Delta t^2,
$$

$$
\mathbf{v}_{n+1} = \mathbf{v}_n + \frac{\Delta t}{2}(\mathbf{a}_n + \mathbf{a}_{n+1}).
$$

## 9. Thermodynamic diagnostics
Main observables are kinetic, potential, and total energy plus instantaneous temperature:

$$
T = \frac{2E_{\mathrm{kin}}}{3Nk_B}.
$$

These diagnostics are used to evaluate equilibration, energy conservation, and timestep quality.

## 10. Result figures
### Thermodynamics
![Base 300 K thermodynamics](figures/thermodynamics/base_temperature_energy_300K.png)
![Temperature-energy comparison](figures/thermodynamics/temperature_energy_comparison.png)

### Timestep stability
![Timestep stability comparison](figures/timestep/timestep_stability_comparison.png)

### System-size dependence
![Temperature and energy vs system size](figures/system_size/temperature_energy_vs_system_size.png)

### Force and potential maps
![Potential/forces at 0 K](figures/forces/potential_forces_0K.png)
![Potential/forces at 300 K](figures/forces/potential_forces_300K.png)
![Potential/forces at 1600 K](figures/forces/potential_forces_1600K.png)
![Potential energy at 800 K final state](figures/forces/potential_energy_800K_final.png)

### Radial distribution / OVITO
![OVITO RDF at 0 K](figures/radial_distribution/ovito_rdf_0K.png)
![OVITO RDF at 300 K](figures/radial_distribution/ovito_rdf_300K.png)
![OVITO RDF at 800 K](figures/radial_distribution/ovito_rdf_800K.png)
![OVITO RDF at 1300 K](figures/radial_distribution/ovito_rdf_1300K.png)

## 11. Running instructions
1. Create and activate a Python environment.
2. Install dependencies:

```bash
pip install -r requirements.txt
```

3. Run the simulation script:

```bash
python src/copper_md_verlet.py
```

## 12. Repository structure
```text
src/
  copper_md_verlet.py
docs/
  theory.md
  results_summary.md
  numerical_method.md
  sources_and_notes.md
figures/
  forces/
  thermodynamics/
  timestep/
  system_size/
  radial_distribution/
  periodic_boundary_conditions/
trajectories/
  examples/
raw_upload/
```
