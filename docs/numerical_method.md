# Numerical Method

## Lattice initialization

Atoms are initialized on an FCC lattice by tiling a conventional unit cell in \(x\), \(y\), and \(z\). The lattice parameter sets the initial nearest-neighbor spacing and simulation-box length.

## Pair-distance computation

At each step, pair displacements are computed for all relevant atom pairs. Distances are derived from displacement vectors after applying periodic-image corrections.

## Cutoff radius

A finite cutoff radius \(r_c\) limits pair interactions for efficiency. Pairs with \(r_{ij}>r_c\) are excluded from force and potential calculations.

## Periodic wrapping

After each position update, coordinates are wrapped back into the principal simulation box. This keeps atom positions within the box while preserving periodic trajectories.

## Minimum image convention

For force evaluation, each displacement component is shifted by integer box lengths so that the pair separation corresponds to the nearest periodic image.

## Velocity initialization

Initial velocities are sampled from a distribution consistent with the chosen starting temperature (typically Maxwell-Boltzmann in practice).

## Center-of-mass velocity removal

The center-of-mass drift velocity is subtracted so total linear momentum is zero, preventing spurious system translation.

## Temperature rescaling

If required for initialization, velocities are uniformly rescaled so the instantaneous kinetic temperature matches the target initial value.

## Velocity-Verlet algorithm

The simulation advances with velocity-Verlet:

1. Update positions using current velocities and accelerations.
2. Apply periodic wrapping.
3. Recompute forces and accelerations from updated positions.
4. Update velocities with average old/new accelerations.

This provides second-order accuracy and robust behavior for conservative MD systems.

## Observables

The code records key observables over time:

- potential energy,
- kinetic energy,
- total energy,
- instantaneous temperature,
- optional structural metrics such as \(g(r)\).

## Stability checks

Numerical stability is assessed by timestep scans and monitoring of total-energy drift. Stable timestep choices should keep long-time energy fluctuations bounded.

## OVITO `.xyz` export

Trajectory snapshots can be exported to `.xyz` format for inspection in OVITO. This is optional and intended for qualitative structure and motion visualization.
