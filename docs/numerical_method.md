# Numerical Method

## FCC initialization
The simulation begins by generating a copper FCC crystal with user-defined cell replication counts \((n_x,n_y,n_z)\) and lattice parameter \(a\). The full coordinate array is assembled from the standard 4-site FCC basis.

## Pair-distance computation
Interatomic separations are evaluated through matrix-based pair-distance calculations. For periodic runs, displacement vectors are first wrapped with box lengths and then converted to distances.

## Exclusion of self-interaction
Diagonal entries \(r_{ii}\) are excluded from pairwise sums and force evaluation to avoid singular self-interaction terms.

## Cutoff handling
A fixed cutoff is applied so only near-neighbor interactions are included. In periodic mode, the cutoff is constrained by half the smallest box length to preserve minimum-image consistency.

## Periodic wrapping
Coordinates are wrapped back into the simulation box after each integration step using modulo operations with box lengths.

## Minimum image convention
Pair displacement vectors are corrected by subtracting integer box translations, ensuring shortest-image displacements are used in both force and energy calculations.

## Velocity initialization
Initial velocities are sampled from a random normal distribution and scaled according to a target temperature and copper atomic mass in simulation units.

## Center-of-mass velocity removal
The mean velocity vector is subtracted from all atoms so total linear momentum is approximately zero.

## Temperature rescaling
A scalar rescaling factor aligns the initial kinetic temperature with the requested setpoint.

## Velocity-Verlet equations
At each step, the algorithm updates positions using current velocities and accelerations, recomputes forces at new positions, and then updates velocities using average old/new accelerations.

## Calculation of observables
The implementation evaluates potential energy from pair interactions, kinetic energy from masses and velocities, total energy from their sum, and temperature from kinetic-energy-based relations.

## Timestep stability
A dedicated experiment compares multiple \(\Delta t\) values to assess numerical stability by monitoring drift or deviation in relative total energy.

## Trajectory export
Selected trajectories are exported in XYZ format for external visualization and structural analysis (for example, in OVITO).
