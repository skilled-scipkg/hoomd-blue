---
name: hoomd-blue-examples-and-tutorials
description: This skill should be used when users ask about examples and tutorials in hoomd-blue; it prioritizes documentation references and then source inspection only for unresolved details.
---

# hoomd-blue: Examples and Tutorials

## High-Signal Playbook
### Route conditions
- Use this skill when the fastest path is adapting a known how-to/tutorial pattern (`sphinx-doc/how-to.rst`, `sphinx-doc/tutorials.rst`).
- Route to `hoomd-blue-build-and-install` when examples fail due to environment/build issues.
- Route to `hoomd-blue-simulation-workflows` when the user needs a productionized pipeline beyond a tutorial pattern.

### Triage questions
- Is the goal benchmarking, minimization, parameter tuning, custom force logic, or free-energy workflow?
- Is the example MD, HPMC, or MPCD?
- Do you already have required input files like `spheres.gsd` / `init.gsd`?
- Is the target metric physical correctness or throughput (TPS / MPS)?
- Do you need a minimal example only, or a version ready for batch production?

### Canonical workflow
1. Pick the closest how-to page in `sphinx-doc/howto`.
2. Run the script unchanged once to establish a known-good baseline.
3. Change one control parameter at a time (buffer, move-size target, thermostat schedule, etc.).
4. Add lightweight logging for the key metric before broader sweeps.
5. Scale system size and hardware only after baseline correctness is confirmed.
6. Promote tuned constants into production scripts/workflows.

### Minimal working example
```python
import hoomd

simulation = hoomd.util.make_example_simulation()
constant_volume = hoomd.md.methods.ConstantVolume(filter=hoomd.filter.All())
fire = hoomd.md.minimize.FIRE(
    dt=0.001,
    force_tol=1e-3,
    angmom_tol=1e-3,
    energy_tol=1e-6,
    methods=[constant_volume],
)
lj = hoomd.md.pair.LJ(nlist=hoomd.md.nlist.Cell(buffer=0.4))
lj.params[("A", "A")] = dict(epsilon=1.0, sigma=1.0)
lj.r_cut[("A", "A")] = 2.5
simulation.operations.integrator = fire
simulation.operations.integrator.forces = [lj]
while not fire.converged:
    simulation.run(100)
```

### Pitfalls and fixes
- Benchmarking before autotuning/warm-up gives misleading performance (`sphinx-doc/howto/determine-the-most-efficient-device.rst`).
- Neighbor-list `buffer` sweeps with too few steps mis-rank candidates (`sphinx-doc/howto/choose-the-neighbor-list-buffer-distance.rst`).
- Multitype HPMC tuning without `ignore_statistics` staging skews move-size tuning (`sphinx-doc/howto/tune-mc-move-sizes-binary-system.rst`).
- Custom MD/HPMC potentials expecting high performance in pure Python: prefer component templates (`sphinx-doc/howto/custom-md-potential.rst`, `sphinx-doc/howto/custom-hpmc-potential.rst`).
- HPMC performance comparisons should use move attempts metric (`mps`) where relevant (`sphinx-doc/howto/determine-the-most-efficient-device.rst` note).

### Convergence/validation checks
- For minimization, require `fire.converged` and stable final energy under stricter tolerances.
- For buffer tuning, verify chosen `buffer` stays near-optimal on repeated runs.
- For HPMC move-size tuning, check post-tuning acceptance behavior per type is stable.
- For benchmarks, confirm ranking persists after increasing step count/system size.

### High-value source entry links
- `hoomd/md/tune/nlist_buffer.py` for neighbor-list buffer tuning behavior.
- `hoomd/hpmc/tune/move_size.py` and `hoomd/hpmc/tune/mc_move_tune.py` for HPMC move-size logic.
- `hoomd/hpmc/pytest/test_move_size_tuner.py` for expected tuner behavior.
- `hoomd/tune/custom_tuner.py` for custom tuning extension patterns.

## Scope
- Handle questions about worked examples, tutorials, and cookbook usage.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `sphinx-doc/index.rst`
- `sphinx-doc/howto/determine-the-most-efficient-device.rst`
- `sphinx-doc/tutorials.rst`
- `sphinx-doc/how-to.rst`
- `sphinx-doc/howto/tune-mc-move-sizes-binary-system.rst`
- `sphinx-doc/howto/minimize-potential-energy.rst`
- `sphinx-doc/howto/custom-md-potential.rst`
- `sphinx-doc/howto/custom-hpmc-potential.rst`
- `sphinx-doc/howto/continuously-vary-potential-parameters.rst`
- `sphinx-doc/howto/choose-the-neighbor-list-buffer-distance.rst`

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
- `hoomd/hpmc/tune/move_size.py`
- `hoomd/hpmc/tune/mc_move_tune.py`
- `hoomd/hpmc/tune/boxmc_move_size.py`
- `hoomd/md/tune/nlist_buffer.py`
- `hoomd/hpmc/tune/CMakeLists.txt`
- `hoomd/hpmc/pytest/test_move_size_tuner.py`
- `hoomd/hpmc/pytest/test_boxmc_move_tuner.py`
- `hoomd/hpmc/tune/__init__.py`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" CMake hoomd`).
