# Copper Molecular Dynamics with Velocity-Verlet Integration

## 1. Scientific motivation
This repository documents a computational-physics molecular dynamics study of FCC copper using a Lennard-Jones model, with emphasis on numerical stability, thermodynamic diagnostics, and physically meaningful trajectory analysis.

## 2. FCC copper crystal construction
The simulation initializes copper atoms on a face-centered-cubic (FCC) lattice by replicating a 4-atom FCC basis over a 3D grid of unit cells.

## 3. Lennard-Jones potential
Interatomic interactions are modeled with a Lennard-Jones pair potential, using fixed parameters defined in the original coursework code.

## 4. Force calculation
Forces are obtained from the radial derivative of the pair potential and combined into net Cartesian force vectors for each atom.

## 5. Periodic boundary conditions and minimum image convention
Finite-size artifacts are reduced using periodic boundary conditions (PBC), with pair separations computed through the minimum image convention to enforce the shortest periodic displacement.

## 6. Velocity initialization and temperature rescaling
Atomic velocities are initialized from a random thermal distribution, corrected to remove center-of-mass drift, and rescaled to match a target temperature.

## 7. Velocity-Verlet integration
Time evolution is performed with the velocity-Verlet scheme, combining stability and second-order accuracy for conservative molecular dynamics trajectories.

## 8. Thermodynamic diagnostics
The workflow tracks kinetic energy, potential energy, total energy, and instantaneous temperature across simulation steps for physical and numerical assessment.

## 9. Timestep stability
A timestep comparison study evaluates energy-conservation behavior as the integration step is varied.

![Timestep stability](figures/timestep/timestep_stability_comparison.png)

## 10. System-size dependence
A system-size study compares thermodynamic behavior across different FCC supercell sizes.

![System size study](figures/system_size/temperature_energy_vs_system_size.png)

## 11. Optional OVITO / XYZ trajectory visualization
The script includes XYZ export routines for post-processing and structure analysis in OVITO, including radial-distribution-function (RDF) inspection.

![RDF at 300 K](figures/radial_distribution/ovito_rdf_300K.png)

## 12. Repository structure
```text
src/
  copper_md_verlet.py
docs/
  theory.md
  numerical_method.md
  results_summary.md
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
requirements.txt
.gitignore
README.md
```

## 13. Skills demonstrated
- Molecular dynamics modeling in Python
- FCC crystal generation and periodic simulation-domain handling
- Pair-potential and force evaluation
- Velocity-Verlet integration and stability analysis
- Thermodynamic post-processing and visualization
- Scientific repository organization and reproducible structure design

## 14. Author
**María Lourdes Domínguez Cacho**  
Final-semester Physics student, University of Alicante  
GitHub: [mldc3](https://github.com/mldc3)
