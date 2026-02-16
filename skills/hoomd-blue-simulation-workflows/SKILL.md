---
name: hoomd-blue-simulation-workflows
description: This skill should be used when users ask about simulation workflows in hoomd-blue; it prioritizes documentation references and then source inspection only for unresolved details.
---

# hoomd-blue: Simulation Workflows

## High-Signal Playbook
### Route conditions
- Use this skill for end-to-end simulation setup/run/restart/logging flow (`sphinx-doc/hoomd/simulation.rst`, `sphinx-doc/hoomd/md/integrator.rst`, `sphinx-doc/hoomd/mpcd/integrator.rst`).
- Route to `hoomd-blue-inputs-and-modeling` for topology, geometry, and wall-model construction details.
- Route to `hoomd-blue-theory-and-methods` for ensemble/method selection and equation-level choices.
- Route to `hoomd-blue-troubleshooting` for exception-driven debugging.

### Triage questions
- Are you running MD, HPMC, MPCD, or a coupled MPCD+MD workflow?
- Is initial state coming from `create_state_from_gsd` or `create_state_from_snapshot` (`hoomd/simulation.py`)?
- Which particles should move vs remain fixed (`sphinx-doc/howto/prevent-particles-from-moving.rst`)?
- What are target observables (TPS, pressure, free energy, acceptance rate)?
- Are you in single-rank mode or MPI/GPU mode during validation?

### Canonical workflow
1. Pick device and set `Simulation.seed` for reproducibility (`hoomd/simulation.py`).
2. Create state from GSD/snapshot exactly once (`hoomd/simulation.py`).
3. Construct integrator stack (MD/HPMC/MPCD) and attach operations.
4. Add logging/writers for the observables needed for validation.
5. Run short equilibration and verify operation counters / error-free stepping.
6. Run production segment; checkpoint/log at stable intervals.
7. For benchmarks, warm up autotuning first (`sphinx-doc/howto/determine-the-most-efficient-device.rst`).

### Minimal working example
```python
import hoomd

simulation = hoomd.util.make_example_simulation()
lj = hoomd.md.pair.LJ(nlist=hoomd.md.nlist.Cell(buffer=0.4))
lj.params[("A", "A")] = dict(epsilon=1.0, sigma=1.0)
lj.r_cut[("A", "A")] = 2.5

langevin = hoomd.md.methods.Langevin(filter=hoomd.filter.All(), kT=1.5)
simulation.operations.integrator = hoomd.md.Integrator(
    dt=0.001, methods=[langevin], forces=[lj]
)
simulation.run(1_000)
```

### Pitfalls and fixes
- Re-initializing an already initialized simulation state raises errors: create state only once (`hoomd/simulation.py`).
- Unset seed causes default-seed behavior; set `Simulation.seed` explicitly (`hoomd/simulation.py`).
- Stationary particles still moving in MD: remove them from method filters (`sphinx-doc/howto/prevent-particles-from-moving.rst`).
- MPCD data appears at unexpected times: particle data updates asynchronously vs MD steps (`hoomd/mpcd/integrate.py` docstring).
- MPCD period mismatch: collision period must align with streaming period (`hoomd/mpcd/collide.py`, `hoomd/mpcd/stream.py`).

### Convergence/validation checks
- Halve `dt` and confirm key observables change within tolerance.
- NVE/NPH sanity check: monitor drift in conserved quantities over fixed windows.
- HPMC sanity check: monitor `translate_moves`/`rotate_moves` and acceptance behavior (`hoomd/hpmc/integrate.py`).
- MPCD sanity check: verify stream/collide operations execute at intended periods.
- Performance check: TPS stabilizes only after autotuning and warm-up (`sphinx-doc/howto/determine-the-most-efficient-device.rst`).

### High-value source entry links
- `hoomd/simulation.py` for lifecycle (`seed`, state creation, timestep semantics).
- `hoomd/md/integrate.py`, `hoomd/hpmc/integrate.py`, `hoomd/mpcd/integrate.py` for integrator behavior.
- `hoomd/mpcd/stream.py` and `hoomd/mpcd/collide.py` for coupled MPCD scheduling and constraints.
- `hoomd/hpmc/nec/integrate.py` for NEC-specific runtime limitations and counters.

## Scope
- Handle questions about simulation setup, execution flow, and runtime controls.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `sphinx-doc/features.rst`
- `sphinx-doc/testing.rst`
- `sphinx-doc/howto/compute-the-free-energy-of-solids.rst`
- `sphinx-doc/howto/prevent-particles-from-moving.rst`
- `sphinx-doc/hoomd/simulation.rst`
- `sphinx-doc/hoomd/operation/integrator.rst`
- `sphinx-doc/hoomd/mpcd/integrator.rst`
- `sphinx-doc/hoomd/md/integrator.rst`
- `sphinx-doc/hoomd/error/simulationdefinitionerror.rst`
- `sphinx-doc/hoomd/mpcd/collide/stochasticrotationdynamics.rst`
- `sphinx-doc/hoomd/hpmc/integrate/hpmcintegrator.rst`
- `sphinx-doc/hoomd/hpmc/nec/integrate/hpmcnecintegrator.rst`

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
- `hoomd/simulation.py`
- `hoomd/state.py`
- `hoomd/operations.py`
- `hoomd/operation.py`
- `hoomd/md/integrate.py`
- `hoomd/hpmc/integrate.py`
- `hoomd/hpmc/nec/integrate.py`
- `hoomd/mpcd/integrate.py`
- `hoomd/mpcd/stream.py`
- `hoomd/mpcd/collide.py`
- `hoomd/write/gsd.py`
- `hoomd/pytest/test_simulation.py`
- `hoomd/md/pytest/test_integrate.py`
- `hoomd/hpmc/pytest/test_nec.py`
- `hoomd/mpcd/pytest/test_integrator.py`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" CMake hoomd`).
