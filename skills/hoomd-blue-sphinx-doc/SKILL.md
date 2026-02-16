---
name: hoomd-blue-sphinx-doc
description: This skill should be used when users ask about sphinx doc in hoomd-blue; it prioritizes documentation references and then source inspection only for unresolved details.
---

# hoomd-blue: Sphinx Doc

## High-Signal Playbook
### Route conditions
- Use this skill when the user needs documentation navigation across the full API/guides corpus (`sphinx-doc/index.rst`, `sphinx-doc/documentation.rst`, `sphinx-doc/indices.rst`).
- Route to topic skills (`build-and-install`, `simulation-workflows`, `inputs-and-modeling`, `theory-and-methods`, `examples-and-tutorials`) once request scope is clear.
- Keep this skill as the "entry map" when the user asks broad "where do I start?" questions.

### Triage questions
- Is the user asking for a workflow answer or a specific API symbol?
- Is the question version-sensitive (`changes`, `deprecated`)?
- Does the user need units/notation clarification before parameter advice?
- Is a quick runnable example enough, or is deep implementation behavior required?

### Canonical workflow
1. Start at `sphinx-doc/index.rst` and classify into Guides vs Python API vs Reference.
2. Resolve to a specific topic doc page (or skill) before answering details.
3. If symbol semantics are unclear in RST stubs, jump to topic `references/source_map.md`.
4. Validate with a short runnable snippet when giving procedural advice.
5. Cite exact doc/source paths in the final answer.

### Minimal working example
```python
import hoomd

sim = hoomd.util.make_example_simulation(device=hoomd.device.CPU())
lj = hoomd.md.pair.LJ(nlist=hoomd.md.nlist.Cell(buffer=0.4))
lj.params[("A", "A")] = dict(epsilon=1.0, sigma=1.0)
lj.r_cut[("A", "A")] = 2.5
cv = hoomd.md.methods.ConstantVolume(filter=hoomd.filter.All())
sim.operations.integrator = hoomd.md.Integrator(
    dt=0.001, methods=[cv], forces=[lj]
)
sim.run(1_000)
```

### Pitfalls and fixes
- Treating this broad doc skill as a final endpoint leads to vague answers; route to a narrower topic skill quickly.
- Missing version/deprecation context: check `sphinx-doc/changes.rst` and `sphinx-doc/deprecated.rst` first.
- Parameter advice without unit context: anchor to `sphinx-doc/units.rst` and `sphinx-doc/notation.rst`.
- Autodoc pages may omit behavioral nuance; inspect source entry points when ambiguity remains.

### Convergence/validation checks
- Run a small smoke test (`run(0)` or short run) for any suggested script adaptation.
- Confirm API names against module index pages (`sphinx-doc/module-hoomd.rst` and submodule pages).
- Re-check assumptions when docs indicate deprecations or behavior changes (`sphinx-doc/changes.rst`).

### High-value source entry links
- `hoomd/simulation.py`, `hoomd/state.py`, `hoomd/operations.py` for lifecycle/runtime semantics.
- `hoomd/write/gsd.py` and `hoomd/write/hdf5.py` for output/logging implementation details.
- `hoomd/variant/box.py` and `hoomd/Variant.h` for variant behavior and low-level semantics.
- `hoomd/version.py` and `hoomd/version_config.py` for version/feature reporting.

## Scope
- Handle questions about documentation grouped under the 'sphinx-doc' theme.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `sphinx-doc/units.rst`
- `sphinx-doc/license.rst`
- `sphinx-doc/deprecated.rst`
- `sphinx-doc/open-source.rst`
- `sphinx-doc/notation.rst`
- `sphinx-doc/module-hoomd.rst`
- `sphinx-doc/logo.rst`
- `sphinx-doc/indices.rst`
- `sphinx-doc/documentation.rst`
- `sphinx-doc/credits.rst`
- `sphinx-doc/citing.rst`
- `sphinx-doc/changes.rst`

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
- `hoomd/simulation.py`
- `hoomd/state.py`
- `hoomd/operations.py`
- `hoomd/box.py`
- `hoomd/wall.py`
- `hoomd/variant/scalar.py`
- `hoomd/variant/box.py`
- `hoomd/write/gsd.py`
- `hoomd/write/hdf5.py`
- `hoomd/write/dcd.py`
- `hoomd/version.py`
- `hoomd/version_config.py`
- `hoomd/module.cc`
- `hoomd/pytest/test_variant.py`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" CMake hoomd`).
