---
name: hoomd-blue-theory-and-methods
description: This skill should be used when users ask about theory and methods in hoomd-blue; it prioritizes documentation references and then source inspection only for unresolved details.
---

# hoomd-blue: Theory and Methods

## High-Signal Playbook
### Route conditions
- Use this skill for selecting and parameterizing integration/collision/thermostat methods (`sphinx-doc/hoomd/md/module-methods.rst`, `sphinx-doc/hoomd/mpcd/module-methods.rst`).
- Route to `hoomd-blue-simulation-workflows` for execution sequencing, checkpointing, and run management.
- Route to `hoomd-blue-inputs-and-modeling` when the blocker is topology/geometry definition instead of method choice.

### Triage questions
- Which ensemble is required (NVE/NVT/NPT/NPH, HPMC, MPCD)?
- Is the system underdamped (MD) or overdamped/stochastic (Brownian/Langevin)?
- Are rotational DOFs needed and initialized?
- Are barostat / thermostat time constants (`tau`, `tauS`) chosen relative to `dt`?
- Are manifold constraints or NEC/HPMC-specific limitations relevant?

### Canonical workflow
1. Choose the method family that matches target physics (MD, HPMC, MPCD).
2. Select ensemble controls (thermostat/barostat/collision thermostat).
3. Set numerically conservative starting parameters (`dt`, `tau`, `tauS`, collision period).
4. Initialize particle momenta consistently with temperature control strategy.
5. Run short equilibration and inspect method-specific diagnostics.
6. Tighten or relax controls based on drift/acceptance and rerun.

### Minimal working example
```python
import hoomd

simulation = hoomd.util.make_example_simulation()
bussi = hoomd.md.methods.thermostats.Bussi(kT=1.5)
nvt = hoomd.md.methods.ConstantVolume(filter=hoomd.filter.All(), thermostat=bussi)
lj = hoomd.md.pair.LJ(nlist=hoomd.md.nlist.Cell(buffer=0.4))
lj.params[("A", "A")] = dict(epsilon=1.0, sigma=1.0)
lj.r_cut[("A", "A")] = 2.5
simulation.operations.integrator = hoomd.md.Integrator(
    dt=0.001, methods=[nvt], forces=[lj]
)
simulation.state.thermalize_particle_momenta(filter=hoomd.filter.All(), kT=1.5)
simulation.run(5_000)
```

### Pitfalls and fixes
- Zero initial velocities with thermostats can stall intended thermalization behavior; thermalize momenta first (`hoomd/md/methods/thermostats.py`).
- `Berendsen` does not sample the correct kinetic-energy distribution; use Bussi/MTTK for canonical sampling (`hoomd/md/methods/thermostats.py`).
- Poor `tauS` / `gamma` choices cause unstable or overdamped barostat dynamics (`hoomd/md/methods/methods.py`).
- RATTLE methods require particles initialized near the manifold (`hoomd/md/methods/rattle.py` warning).
- NEC integrators do not support GPU/MPI and have restricted potential/wall support (`hoomd/hpmc/nec/integrate.py`).
- For small MPCD mean free path, keep collision cell shifting enabled (`hoomd/mpcd/collide.py`).

### Convergence/validation checks
- Perform a `dt` sensitivity check (e.g., halve `dt`) and compare observables.
- For NVE/NPH, verify expected conservation behavior over long windows.
- For NVT/NPT, check temperature/pressure stationarity and relaxation time vs chosen `tau`/`tauS`.
- For HPMC/NEC, inspect move/collision counters and acceptance trends.
- For MPCD, verify collision/stream periods and thermostat settings are physically consistent.

### High-value source entry links
- `hoomd/md/methods/methods.py` for integrator equations and method constraints.
- `hoomd/md/methods/thermostats.py` for thermostat algorithms and caveats.
- `hoomd/md/methods/rattle.py` for manifold-constrained dynamics behavior.
- `hoomd/hpmc/integrate.py` and `hoomd/hpmc/nec/integrate.py` for HPMC/NEC details and limits.
- `hoomd/mpcd/stream.py` and `hoomd/mpcd/collide.py` for MPCD algorithm controls.

## Scope
- Handle questions about theoretical background and algorithmic methods.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `sphinx-doc/hoomd/mpcd/module-methods.rst`
- `sphinx-doc/hoomd/md/module-methods.rst`
- `sphinx-doc/hoomd/mpcd/stream/streamingmethod.rst`
- `sphinx-doc/hoomd/mpcd/methods/bounceback.rst`
- `sphinx-doc/hoomd/mpcd/collide/collisionmethod.rst`
- `sphinx-doc/hoomd/md/methods/thermostatted.rst`
- `sphinx-doc/hoomd/md/methods/overdampedviscous.rst`
- `sphinx-doc/hoomd/md/methods/module-thermostats.rst`
- `sphinx-doc/hoomd/md/methods/module-rattle.rst`
- `sphinx-doc/hoomd/md/methods/method.rst`
- `sphinx-doc/hoomd/md/methods/langevin.rst`
- `sphinx-doc/hoomd/md/methods/displacementcapped.rst`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for the complete topic document list.
- Use tutorials/examples as executable usage patterns when available.
- Use tests as behavior or regression references when available.
- If ambiguity remains after docs, inspect `references/source_map.md` and start with the ranked source entry points.
- Cite exact documentation file paths in responses.

## Tutorials and examples
- `sphinx-doc/howto`
- `sphinx-doc/tutorial`

## Test references
- `hoomd/pytest`
- `hoomd/test`
- `hoomd/hpmc/pytest`
- `hoomd/hpmc/test`
- `hoomd/md/pytest`
- `hoomd/md/test`
- `hoomd/mpcd/pytest`
- `hoomd/mpcd/test`

## Optional deeper inspection
- `CMake`
- `hoomd`

## Source entry points for unresolved issues
- `hoomd/mpcd/StreamingMethod.h`
- `hoomd/mpcd/StreamingMethod.cc`
- `hoomd/md/methods/thermostats.py`
- `hoomd/mpcd/BounceBackStreamingMethodGPU.h`
- `hoomd/mpcd/BounceBackStreamingMethod.h`
- `hoomd/mpcd/methods.py`
- `hoomd/mpcd/CollisionMethod.h`
- `hoomd/mpcd/CollisionMethod.cc`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" CMake hoomd`).
