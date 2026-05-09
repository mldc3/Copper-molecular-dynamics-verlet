# Theory Background: Molecular Dynamics of FCC Copper

## Molecular dynamics framework
Molecular dynamics (MD) evolves atomic positions by integrating Newton's equations for interacting particles. For particle $i$ with mass $m_i$:

$$
m_i\frac{d^2\mathbf{r}_i}{dt^2} = \mathbf{F}_i,
$$

with force from the many-body potential energy surface:

$$
\mathbf{F}_i = -\nabla_i U.
$$

The method produces deterministic trajectories and connects microscopic interactions to thermodynamic observables.

## Integration methods and why Verlet is used
Time is discretized as $t_n = n\Delta t$. Classical methods such as Euler or Runge-Kutta can integrate the equations, but long MD trajectories benefit from time-reversible, symplectic-like schemes in the Verlet family.

In velocity-Verlet, positions and velocities are updated as:

$$
\mathbf{r}_{n+1} = \mathbf{r}_n + \mathbf{v}_n\Delta t + \frac{1}{2}\mathbf{a}_n\Delta t^2,
$$

$$
\mathbf{v}_{n+1} = \mathbf{v}_n + \frac{\Delta t}{2}(\mathbf{a}_n + \mathbf{a}_{n+1}).
$$

This method is second-order accurate and typically has good long-time energy behavior for conservative interactions when $\Delta t$ is sufficiently small.

## FCC copper lattice
Copper crystallizes in an FCC structure in standard conditions. The FCC unit cell has four basis positions and reproduces close-packed neighbor shells that define early peaks in structural correlation functions.

## Lennard-Jones interaction model
The simulation uses the pair potential:

$$
U(r) = 4\epsilon\left[\left(\frac{\sigma}{r}\right)^{12} - \left(\frac{\sigma}{r}\right)^6\right].
$$

The radial force magnitude comes from:

$$
F(r) = -\frac{dU}{dr}.
$$

The $r^{-12}$ term models short-range repulsion and the $r^{-6}$ term models attraction.

## Cutoff radius
Pair interactions are truncated at a finite cutoff $r_c$ to reduce computational cost. Only pairs with $r_{ij}<r_c$ contribute to force and potential. The choice of $r_c$ affects both efficiency and quantitative accuracy.

## Periodic boundary conditions
To approximate bulk material and reduce free-surface artifacts, the finite simulation box is periodically tiled. When a particle exits one side, its image re-enters on the opposite side.

## Minimum image convention
For each pair, the interaction uses the nearest periodic image. In a cubic box of length $L$, displacement components are wrapped as:

$$
\Delta x \leftarrow \Delta x - L\,\mathrm{round}(\Delta x/L),
$$

and analogously for $y,z$. Distances are then computed from the wrapped displacement vector.

## Temperature and energy in MD
Kinetic energy is:

$$
E_{\mathrm{kin}} = \sum_i \frac{1}{2}m_i v_i^2.
$$

Instantaneous temperature is estimated by equipartition:

$$
T = \frac{2E_{\mathrm{kin}}}{3Nk_B}.
$$

Total energy is:

$$
E_{\mathrm{tot}} = E_{\mathrm{kin}} + U.
$$

In ideal NVE dynamics, $E_{\mathrm{tot}}$ should remain approximately constant up to bounded numerical fluctuations.

## NVE and NVT context
NVE keeps particle number, volume, and total energy fixed. NVT introduces temperature control through a thermostat. This project interprets trajectories primarily in an NVE-style conservation context after initialization and velocity scaling.

## Radial distribution function
The radial distribution function $g(r)$ measures shell structure and short/medium-range order. Crystalline systems show pronounced peaks at characteristic neighbor distances; higher temperature broadens peaks and can reduce long-range order.

## Limitations of Lennard-Jones for copper
Lennard-Jones is useful for learning MD workflows but it is not a production-quality metallic potential for copper. Real copper bonding is better represented by many-body metallic models (for example EAM-type formulations), so quantitative predictions from this simplified model should be interpreted cautiously.
