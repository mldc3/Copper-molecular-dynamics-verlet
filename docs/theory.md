# Theory

## Molecular dynamics
Molecular dynamics (MD) simulates atomistic motion by numerically integrating Newton's equations of motion for many interacting particles. Given an interaction model and initial conditions, MD produces trajectories from which structural and thermodynamic observables can be computed.

## Newton's equations
For each atom \(i\),
\[
m_i \frac{d^2 \mathbf{r}_i}{dt^2} = \mathbf{F}_i,
\]
where \(m_i\) is mass, \(\mathbf{r}_i\) position, and \(\mathbf{F}_i\) the net force from all other atoms.

## Pair potentials
In pairwise models, total potential energy is written as
\[
U = \frac{1}{2}\sum_{i\neq j} V(r_{ij}),
\]
with \(r_{ij}=|\mathbf{r}_i-\mathbf{r}_j|\). The factor \(1/2\) avoids double counting.

## FCC lattice
Face-centered-cubic (FCC) copper is represented by a cubic unit cell with four basis atoms. Repeating this basis along \(x,y,z\) generates the simulation supercell used as the initial crystal configuration.

## Lennard-Jones potential
The Lennard-Jones form is
\[
V(r)=4\varepsilon\left[\left(\frac{\sigma}{r}\right)^{12}-\left(\frac{\sigma}{r}\right)^6\right],
\]
where \(\sigma\) controls the length scale and \(\varepsilon\) the interaction depth.

## Force from potential derivative
The radial force magnitude is derived from
\[
F(r)=-\frac{dV}{dr},
\]
and converted to vector force along the interatomic direction. Net force on each atom is the vector sum over all interacting neighbors.

## Cutoff radius
To reduce computational cost, interactions are truncated beyond a cutoff radius \(r_c\). Only pairs with \(r_{ij}<r_c\) contribute to forces and potential energy.

## Periodic boundary conditions
Periodic boundary conditions replicate the simulation box in all directions, approximating bulk material behavior and suppressing free-surface artifacts in finite systems.

## Minimum image convention
Under periodicity, pair separations are computed using the nearest periodic image of each atom pair so that the shortest physically equivalent displacement is used.

## Kinetic energy and temperature
Kinetic energy is
\[
K=\sum_i \frac{1}{2}m_i v_i^2.
\]
In classical MD, instantaneous temperature is estimated from kinetic energy after removing center-of-mass drift.

## NVE energy conservation
With conservative forces and a stable timestep, microcanonical (NVE-like) trajectories should show near-conservation of total energy, up to bounded numerical fluctuations.

## Radial distribution function
The radial distribution function \(g(r)\) quantifies local structure by measuring how atomic density varies with distance from a reference atom. In solids, \(g(r)\) shows sharp coordination peaks; in liquids, peaks broaden and short-range order dominates.
