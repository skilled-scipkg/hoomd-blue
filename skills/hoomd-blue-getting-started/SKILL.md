---
name: hoomd-blue-getting-started
description: This skill should be used when users ask about getting started in hoomd-blue; it prioritizes documentation references and then source inspection only for unresolved details.
---

# hoomd-blue: Getting Started

## High-Signal Playbook
### Route conditions
- Use this skill to get a first successful HOOMD-blue run and basic environment sanity checks.
- Route to `hoomd-blue-build-and-install` when import/build prerequisites fail.
- Route to `hoomd-blue-simulation-workflows` once the user has a working baseline and needs production workflow details.

### Triage questions
- Is HOOMD already importable in the target environment?
- Does the user need CPU-only startup or GPU/MPI startup?
- Is the user creating a new script or migrating an older one (`sphinx-doc/migrating.rst`)?

### Canonical workflow
1. Verify install and version with a one-line import test.
2. Start from `hoomd.util.make_example_simulation()` to avoid external input-file dependencies.
3. Add a simple force + method + integrator stack.
4. Run a short segment and verify timestep progression.
5. Save the script as the baseline template for future runs.

### Minimal working example
```bash
python3 - <<'PY'
import hoomd

sim = hoomd.util.make_example_simulation(device=hoomd.device.CPU())
lj = hoomd.md.pair.LJ(nlist=hoomd.md.nlist.Cell(buffer=0.4))
lj.params[("A", "A")] = dict(epsilon=1.0, sigma=1.0)
lj.r_cut[("A", "A")] = 2.5
langevin = hoomd.md.methods.Langevin(filter=hoomd.filter.All(), kT=1.0)
sim.operations.integrator = hoomd.md.Integrator(
    dt=0.001, methods=[langevin], forces=[lj]
)
sim.run(1_000)
print("timestep", sim.timestep)
PY
```

### Validation checkpoints
- `import hoomd` succeeds without errors.
- Final printed timestep is `1000`.
- No `SimulationDefinitionError`/`IncompleteSpecificationError` is raised.

### Pitfalls and fixes
- Missing optional component errors: check whether GPU/MPI/MPCD was built before using related APIs.
- Re-initializing state in the same `Simulation` instance raises errors; initialize once.
- Migration issues from older scripts: check renamed/removed APIs in `sphinx-doc/migrating.rst`.

### High-value source entry links
- `hoomd/simulation.py` for state-creation lifecycle and run semantics.
- `hoomd/state.py` and `hoomd/operations.py` for runtime object wiring.
- `hoomd/device.py` for CPU/GPU device selection behavior.
- `hoomd/pytest/test_simulation.py` for baseline simulation behavior checks.

## Scope
- Handle questions about initial setup, quickstarts, and core concepts.
- Keep responses practical and startup-focused before diving into advanced tuning.

## Primary documentation references
- `README.md`
- `INSTALLING.rst`
- `sphinx-doc/getting-started.rst`
- `sphinx-doc/installation.rst`
- `sphinx-doc/how-to.rst`
- `sphinx-doc/howto/minimize-potential-energy.rst`
- `sphinx-doc/migrating.rst`

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
- `hoomd/__init__.py`
- `hoomd/device.py`
- `hoomd/simulation.py`
- `hoomd/state.py`
- `hoomd/operations.py`
- `hoomd/operation.py`
- `hoomd/util.py`
- `hoomd/pytest/test_simulation.py`
- `hoomd/pytest/test_state.py`
- `hoomd/pytest/test_operations.py`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" CMake hoomd`).
