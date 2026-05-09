# Numerical Method and Computational Workflow

This document describes how the simulation is executed computationally, step by step.

## 1. FCC initialization
The system starts from an FCC crystal built by replicating the FCC basis over $(n_x,n_y,n_z)$ cells using lattice parameter $a$. This yields initial positions $\{\mathbf{r}_i\}_{i=1}^N$ and simulation box lengths from cell counts.

## 2. Pair-distance computation
At each force evaluation, pair displacements and pair distances $r_{ij}$ are computed. Distances on the diagonal are excluded to avoid self-interaction terms.

## 3. Self-interaction exclusion
Self terms $i=j$ are removed by setting diagonal distances to a non-interacting value (or by masks), so no atom interacts with itself.

## 4. Lennard-Jones energy
For interacting pairs, the pair potential is:

$$
U(r_{ij}) = 4\epsilon\left[\left(\frac{\sigma}{r_{ij}}\right)^{12} - \left(\frac{\sigma}{r_{ij}}\right)^6\right].
$$

Total potential energy is obtained from pair sums with double counting removed.

## 5. Force calculation
For each valid pair, forces are computed from the radial derivative of the Lennard-Jones potential and projected along the unit pair direction. Net force on atom $i$ is the sum over neighbors.

## 6. Cutoff handling
Interactions are restricted to $r_{ij}<r_c$ for efficiency. Under periodic boundaries, $r_c$ is limited to remain compatible with minimum-image assumptions.

## 7. Periodic wrapping
When periodic boundaries are enabled, displacements are wrapped with box lengths to represent periodic images.

## 8. Minimum image convention
Displacements are mapped to the nearest image before norm evaluation, giving the shortest periodic pair separation used in force/energy calculations.

## 9. Velocity initialization
Random initial velocities are assigned and then adjusted to match a target thermal state.

## 10. Center-of-mass velocity removal
The center-of-mass drift is removed so that:

$$
\sum_i m_i\mathbf{v}_i = \mathbf{0}.
$$

This prevents artificial translation of the whole crystal.

## 11. Temperature rescaling
Velocities are rescaled to match target initialization temperature using kinetic-energy-based scaling, with:

$$
T = \frac{2E_{\mathrm{kin}}}{3Nk_B}.
$$

## 12. Velocity-Verlet update
Each timestep performs:

1. position update from current $\mathbf{r}_n, \mathbf{v}_n, \mathbf{a}_n$,
2. force/acceleration recomputation at new positions,
3. velocity update using average acceleration.

This gives second-order time integration with good stability for suitable $\Delta t$.

## 13. Energy diagnostics
At each step, the workflow computes:
- kinetic energy $E_{\mathrm{kin}}$,
- potential energy $U$,
- total energy $E_{\mathrm{tot}}=E_{\mathrm{kin}}+U$,
- instantaneous temperature.

These trajectories are used to assess equilibration and numerical quality.

## 14. Timestep stability study
Multiple $\Delta t$ values are compared to identify stable integration ranges. Instability is detected through unphysical energy drift, overheating, or rapidly growing fluctuations.

## 15. System-size study
Runs with different atom counts evaluate finite-size effects, fluctuation scaling, and consistency of intensive observables such as temperature.

## 16. XYZ export
Selected trajectories are written in XYZ format for visualization and external structural diagnostics (for example in OVITO).
