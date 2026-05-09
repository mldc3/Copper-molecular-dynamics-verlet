# Copper Molecular Dynamics with Velocity-Verlet Integration

This repository presents a computational-physics molecular dynamics project for crystalline copper. The goal is to simulate an FCC copper crystal with a Lennard-Jones pair potential, periodic boundary conditions, the minimum image convention, and velocity-Verlet time integration.

## Scientific motivation

Molecular dynamics (MD) is a core tool in condensed matter physics and materials modelling because it links microscopic interatomic interactions to macroscopic thermodynamic behavior. This project is designed as a clean, professional implementation suitable for scientific-computing workflows and reproducible analysis.

## Physical model

The simulation implements:

- FCC lattice generation,
- Lennard-Jones pair potential,
- force calculation,
- periodic boundary conditions,
- minimum image convention,
- velocity-Verlet integration,
- kinetic, potential and total energy,
- instantaneous temperature,
- timestep stability,
- system-size dependence,
- radial distribution function,
- optional `.xyz` export for OVITO visualization.

### Lennard-Jones potential

\[
U(r)=4\,\epsilon\left[\left(\frac{\sigma}{r}\right)^{12}-\left(\frac{\sigma}{r}\right)^6\right]
\]

The repulsive term is proportional to \(r^{-12}\), while the attractive term is proportional to \(r^{-6}\).

Representative copper-like parameters used in this project:

- \(\sigma = 2.3151\) Å
- \(\epsilon = 0.167\) eV
- FCC lattice parameter \(a = 3.603\) Å
- copper atomic mass \(m = 63.546\) u

## Numerical method

The integrator is velocity-Verlet. For timestep \(\Delta t\):

\[
\mathbf{r}_{i}(t+\Delta t)=\mathbf{r}_{i}(t)+\mathbf{v}_{i}(t)\Delta t+\frac{1}{2}\mathbf{a}_{i}(t)\Delta t^2
\]

\[
\mathbf{v}_{i}(t+\Delta t)=\mathbf{v}_{i}(t)+\frac{1}{2}\left[\mathbf{a}_{i}(t)+\mathbf{a}_{i}(t+\Delta t)\right]\Delta t
\]

This second-order method is standard for NVE molecular dynamics due to its stability and good long-time energy behavior.

## Periodic boundary conditions

The simulation cell is treated as periodically repeated in all directions to mimic bulk copper. Distances are computed with the minimum image convention so each particle pair interacts through the closest periodic image.

## Main diagnostics

The analysis focuses on:

- force and potential-energy distributions,
- kinetic, potential and total energy trajectories,
- instantaneous temperature evolution,
- timestep sensitivity,
- system-size dependence,
- radial distribution function \(g(r)\),
- behavior with and without periodic boundary handling.

## Example results

Representative figures will be added after the raw project files are uploaded.

- **Forces:** placeholder pending uploaded simulation outputs.
- **Thermodynamics:** placeholder pending uploaded simulation outputs.
- **Timestep study:** placeholder pending uploaded simulation outputs.
- **System-size study:** placeholder pending uploaded simulation outputs.
- **Radial distribution:** placeholder pending uploaded simulation outputs.
- **Periodic boundary conditions:** placeholder pending uploaded simulation outputs.

## Running the code

1. Create and activate a Python environment.
2. Install dependencies:

```bash
pip install -r requirements.txt
```

3. Place or develop simulation scripts in `src/`.
4. Save generated figures in the corresponding subfolders under `figures/`.
5. Optionally export trajectories to `trajectories/examples/` in `.xyz` format for OVITO.

## Repository structure

```text
src/
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
README.md
requirements.txt
.gitignore
```

## Skills demonstrated

- Scientific programming in Python
- Numerical integration for Hamiltonian dynamics
- MD model design for crystalline solids
- Periodic-boundary and minimum-image implementations
- Thermodynamic and structural diagnostics from trajectory data
- Reproducible project organization and technical documentation

## Author

**María Lourdes Domínguez Cacho**  
Final-semester Physics student, University of Alicante  
GitHub: [mldc3](https://github.com/mldc3)
