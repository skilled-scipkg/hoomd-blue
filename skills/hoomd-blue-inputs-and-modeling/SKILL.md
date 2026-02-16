---
name: hoomd-blue-inputs-and-modeling
description: This skill should be used when users ask about inputs and modeling in hoomd-blue; it prioritizes documentation references and then source inspection only for unresolved details.
---

# hoomd-blue: Inputs and Modeling

## High-Signal Playbook
### Route conditions
- Use this skill for particle types/topology, wall and geometry definitions, and physical model setup (`sphinx-doc/howto/molecular.rst`, `sphinx-doc/hoomd/wall/wallgeometry.rst`, `sphinx-doc/hoomd/mpcd/module-geometry.rst`).
- Route to `hoomd-blue-simulation-workflows` for execution pipeline and logging/restart flow.
- Route to `hoomd-blue-theory-and-methods` when the question is primarily about algorithm/ensemble correctness.

### Triage questions
- Is this molecular MD topology setup, MPCD confined flow setup, or both?
- What confinement is needed (walls, parallel plates, pores, cylinders, sinusoidal channels)?
- Do you need slip or no-slip boundaries (`hoomd/mpcd/geometry.py`)?
- Are particles guaranteed to start inside the chosen geometry?
- Are you in 2D or 3D, and do wall vectors align with the intended simulation plane (`hoomd/wall.py` warnings)?

### Canonical workflow
1. Define particle types, bonded topology, and box in a snapshot/GSD (`sphinx-doc/howto/molecular.py`).
2. Configure force-field terms for the chosen model (`sphinx-doc/howto/molecular.rst`).
3. For confinement, instantiate wall or MPCD geometry objects and attach to methods/streaming.
4. If MPCD walls are used, add virtual particle filling where needed (`hoomd.mpcd.fill.GeometryFiller`).
5. Attach integrator and run short validation to ensure particles remain in valid regions.
6. Promote to production parameters only after geometry and topology checks pass.

### Minimal working example
```python
import hoomd
import gsd.hoomd

frame = gsd.hoomd.Frame()
frame.particles.N = 5
frame.particles.position = [[-2, 0, 0], [-1, 0, 0], [0, 0, 0], [1, 0, 0], [2, 0, 0]]
frame.particles.types = ["A"]
frame.particles.typeid = [0] * 5
frame.configuration.box = [20, 20, 20, 0, 0, 0]
frame.bonds.N = 4
frame.bonds.types = ["A-A"]
frame.bonds.typeid = [0] * 4
frame.bonds.group = [[0, 1], [1, 2], [2, 3], [3, 4]]

with gsd.hoomd.open(name="molecular.gsd", mode="x") as f:
    f.append(frame)

harmonic = hoomd.md.bond.Harmonic()
harmonic.params["A-A"] = dict(k=100, r0=1.0)
sim = hoomd.Simulation(device=hoomd.device.CPU(), seed=1)
sim.create_state_from_gsd(filename="molecular.gsd")
langevin = hoomd.md.methods.Langevin(filter=hoomd.filter.All(), kT=1.0)
sim.operations.integrator = hoomd.md.Integrator(dt=0.005, methods=[langevin], forces=[harmonic])
sim.run(1_000)
```

### Pitfalls and fixes
- 2D MD wall setups can push particles off-plane if `origin` / `axis` / `normal` are inconsistent (`hoomd/wall.py` warnings).
- `WallGeometry` objects are immutable; rebuild objects instead of mutating fields (`hoomd/wall.py`).
- `GeometryFiller` does not support triclinic boxes and has PlanarPore/non-cubic limits (`hoomd/mpcd/fill.py`).
- Setting `speed`/`angular_speed` with slip surfaces has no shear effect (`hoomd/mpcd/geometry.py`).
- Confined MPCD setups require particles initialized inside geometry; validate before production (`hoomd/mpcd/stream.py`, `hoomd/mpcd/methods.py`).

### Convergence/validation checks
- Confirm bonded geometry (bond lengths/angles) remains physically stable over equilibration.
- For confinement, assert all particles are inside boundaries after startup checks.
- For flow setups, verify expected qualitative profile (e.g., shear response for no-slip plates).
- Re-run with altered box padding to ensure no periodic self-interaction artifacts in confined setups.

### High-value source entry links
- `hoomd/wall.py` for wall geometry semantics, immutability, and 2D caveats.
- `hoomd/mpcd/geometry.py` for geometry definitions, no-slip/slip behavior, and examples.
- `hoomd/mpcd/fill.py` for virtual-particle filling rules and geometry limitations.
- `hoomd/mpcd/methods.py` and `hoomd/mpcd/stream.py` for inside-geometry checks and boundary coupling caveats.

## Scope
- Handle questions about inputs, system setup, models, and physical parameterization.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `sphinx-doc/howto/molecular.rst`
- `sphinx-doc/hoomd/wall/wallgeometry.rst`
- `sphinx-doc/hoomd/mpcd/module-geometry.rst`
- `sphinx-doc/hoomd/mpcd/geometry/sphere.rst`
- `sphinx-doc/hoomd/mpcd/geometry/planarpore.rst`
- `sphinx-doc/hoomd/mpcd/geometry/parallelplates.rst`
- `sphinx-doc/hoomd/mpcd/geometry/geometry.rst`
- `sphinx-doc/hoomd/mpcd/geometry/cosineexpansioncontraction.rst`
- `sphinx-doc/hoomd/mpcd/geometry/cosinechannel.rst`
- `sphinx-doc/hoomd/mpcd/geometry/concentriccylinders.rst`
- `sphinx-doc/hoomd/mpcd/fill/geometryfiller.rst`

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
- `hoomd/snapshot.py`
- `hoomd/wall.py`
- `hoomd/md/bond.py`
- `hoomd/md/angle.py`
- `hoomd/md/dihedral.py`
- `hoomd/md/improper.py`
- `hoomd/mpcd/geometry.py`
- `hoomd/mpcd/fill.py`
- `hoomd/mpcd/stream.py`
- `hoomd/mpcd/methods.py`
- `hoomd/pytest/test_snapshot.py`
- `hoomd/md/pytest/test_bond.py`
- `hoomd/md/pytest/test_angle.py`
- `hoomd/md/pytest/test_wall_potential.py`
- `hoomd/mpcd/pytest/test_geometry.py`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" CMake hoomd`).
