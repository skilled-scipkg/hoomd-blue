---
name: hoomd-blue-troubleshooting
description: This skill should be used when users ask about troubleshooting in hoomd-blue; it prioritizes documentation references and then source inspection only for unresolved details.
---

# hoomd-blue: Troubleshooting

## High-Signal Playbook
### Route conditions
- Use this skill for runtime/setup errors, API validation failures, and unexpected simulation behavior.
- Route to `hoomd-blue-build-and-install` for compile/install/linker/toolchain failures.
- Route to `hoomd-blue-parallel-hpc` for MPI/GPU-only failures.

### Triage questions
- What exact exception/warning type and message is reported?
- Does the failure happen on object creation, attach, or during `run()`?
- Is the issue reproducible in a minimal script with `make_example_simulation()`?

### Canonical workflow
1. Reproduce with the smallest script that still fails.
2. Classify error family (type conversion, mutability, incomplete specification, device/mpi availability).
3. Inspect relevant API docs and source validator path.
4. Confirm fix with a short run and targeted pytest.

### Minimal working example
```bash
python3 - <<'PY'
import hoomd

sim = hoomd.util.make_example_simulation()
try:
    # Intentional misuse: second state initialization should fail.
    sim.create_state_from_snapshot(hoomd.Snapshot())
except Exception as err:
    print(type(err).__name__)
    print(err)
PY
```

### Validation checkpoints
- The issue reproduces in a minimal script.
- The candidate fix removes the error without introducing new warnings.
- Relevant targeted tests pass after the fix.

### Pitfalls and fixes
- Debugging with full production scripts hides root causes; reduce to minimal reproducer first.
- Attach-time errors often come from incomplete parameterization (missing `params` or required types).
- Mutability/typing errors are usually deterministic; inspect parameter dictionaries and operation order.

### High-value source entry links
- `hoomd/error.py` for Python exception classes and messaging.
- `hoomd/data/typeconverter.py` and `hoomd/data/parameterdicts.py` for validation errors.
- `hoomd/operation.py` / `hoomd/operations.py` for attach-time ordering and lifecycle errors.
- `hoomd/pytest/test_operation.py` and `hoomd/pytest/test_parameter_dict.py` for reproducible failure patterns.

## Scope
- Handle questions about known issues, diagnostics, and debugging patterns.
- Prioritize reproducible diagnosis and quick convergence to validated fixes.

## Primary documentation references
- `sphinx-doc/testing.rst`
- `sphinx-doc/migrating.rst`
- `sphinx-doc/hoomd/module-error.rst`
- `sphinx-doc/hoomd/error/typeconversionerror.rst`
- `sphinx-doc/hoomd/error/mutabilityerror.rst`
- `sphinx-doc/hoomd/error/isolationwarning.rst`
- `sphinx-doc/hoomd/error/incompletespecificationerror.rst`
- `sphinx-doc/hoomd/error/dataaccesserror.rst`

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
- `hoomd/error.py`
- `hoomd/data/typeconverter.py`
- `hoomd/data/parameterdicts.py`
- `hoomd/operation.py`
- `hoomd/operations.py`
- `hoomd/simulation.py`
- `hoomd/state.py`
- `hoomd/device.py`
- `hoomd/communicator.py`
- `hoomd/pytest/test_operation.py`
- `hoomd/pytest/test_parameter_dict.py`
- `hoomd/pytest/test_type_parameter_dict.py`
- `hoomd/pytest/test_simulation.py`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" CMake hoomd`).
