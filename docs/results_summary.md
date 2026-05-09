# Results Summary

## Force and potential-energy distribution
The force/potential visualizations show non-uniform local interaction environments across atoms, with stronger interactions for closer neighbors and weaker contributions at larger separations within cutoff.

## Base simulation at 300 K
The base 300 K run provides a stable reference trajectory for monitoring temperature and energy components under velocity-Verlet time integration.

## Comparison between temperatures
Cross-temperature simulations illustrate how thermodynamic trajectories change between low and high thermal regimes while preserving the same interaction model and numerical setup.

## Timestep stability
The timestep comparison demonstrates the expected sensitivity of energy conservation to integration step size: smaller timesteps remain more stable, while overly large timesteps degrade numerical behavior.

## System-size dependence
The system-size study highlights finite-size effects in thermodynamic traces and supports the use of larger supercells for smoother bulk-like statistics.

## Radial distribution / OVITO visualization
RDF-related images and OVITO workflows provide structure-focused interpretation across temperatures, including sharper solid-like ordering at lower temperature and broader features at higher temperature.

## Main conclusions
The organized dataset supports a coherent computational-physics workflow for FCC copper MD: physically interpretable force/energy behavior, reliable integrator trends under proper timestep control, and useful structure diagnostics through trajectory visualization.
