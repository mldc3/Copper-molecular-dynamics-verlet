# Theory

## Molecular dynamics framework

Molecular dynamics (MD) simulates the time evolution of a many-particle system by integrating Newton's equations of motion. For atom \(i\),

\[
m_i\frac{d^2\mathbf{r}_i}{dt^2}=\mathbf{F}_i
\]

where \(m_i\) is mass, \(\mathbf{r}_i\) position, and \(\mathbf{F}_i\) net force from interactions with other atoms.

## Pair potentials and interatomic interactions

In classical MD, interactions are commonly described by pair potentials \(U(r_{ij})\), where \(r_{ij}=|\mathbf{r}_i-\mathbf{r}_j|\). The total potential energy is the sum over unique pairs,

\[
U_{\text{tot}}=\sum_{i<j} U(r_{ij})
\]

and forces are obtained from the potential gradient.

## FCC crystal construction

Copper crystallizes in a face-centered cubic (FCC) structure. An FCC conventional cell contains atoms at the cube corners and face centers. A bulk crystal model is built by repeating the FCC unit cell in three dimensions.

## Lennard-Jones potential

The Lennard-Jones (LJ) model used here is

\[
U(r)=4\,\epsilon\left[\left(\frac{\sigma}{r}\right)^{12}-\left(\frac{\sigma}{r}\right)^6\right]
\]

The \(r^{-12}\) part represents short-range repulsion and the \(r^{-6}\) part represents longer-range attraction.

## Force from the potential

For a central pair potential, force is

\[
\mathbf{F}_{ij}=-\nabla U(r_{ij})= -\frac{dU}{dr}\hat{\mathbf{r}}_{ij}
\]

For the LJ form,

\[
\frac{dU}{dr}=4\epsilon\left[-12\frac{\sigma^{12}}{r^{13}}+6\frac{\sigma^6}{r^7}\right]
\]

which determines pairwise forces used in the equations of motion.

## Periodic boundary conditions

To represent bulk matter with finite atom count, periodic boundary conditions (PBC) replicate the simulation box in all directions. Atoms leaving one side re-enter from the opposite side.

## Minimum image convention

Under PBC, each pair interaction uses the closest periodic image displacement. This minimum image convention avoids unphysical long-distance choices and is central to consistent short-range MD with periodic boxes.

## Kinetic energy and temperature

Kinetic energy is

\[
K=\sum_i \frac{1}{2}m_i v_i^2
\]

Instantaneous temperature in classical MD is estimated from kinetic energy via equipartition,

\[
T=\frac{2K}{f k_B}
\]

where \(f\) is the number of active degrees of freedom.

## NVE energy conservation

In microcanonical (NVE) dynamics, total energy

\[
E=K+U
\]

should remain approximately constant, with small bounded fluctuations due to numerical discretization.

## Radial distribution function

The radial distribution function \(g(r)\) measures structural order by comparing local particle density at distance \(r\) to the ideal-gas reference at the same mean density. Peaks in \(g(r)\) indicate preferred neighbor shells characteristic of crystalline order.
