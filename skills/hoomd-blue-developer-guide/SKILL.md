---
name: hoomd-blue-developer-guide
description: This skill should be used when users ask about developer guide in hoomd-blue; it prioritizes documentation references and then source inspection only for unresolved details.
---

# hoomd-blue: Developer Guide

## High-Signal Playbook
### Route conditions
- Use this skill for internal architecture questions, extension points, contribution flow, and test strategy.
- Route to `hoomd-blue-build-and-install` for compiler/toolchain configuration blockers.
- Route to topic runtime skills when the request is end-user simulation setup instead of development internals.

### Triage questions
- Is the change in Python API surface, C++ backend, or both?
- Is the user adding a new operation/force/method or changing existing behavior?
- Which test layer is needed: unit-like Python tests, component tests, or MPI/GPU coverage?

### Canonical workflow
1. Read architecture/contribution docs to scope change boundaries.
2. Locate owning module(s) and nearest existing tests.
3. Implement minimal change with explicit API constraints.
4. Run targeted pytest first, then broader suite as needed.
5. Confirm documentation and style expectations before finalizing.

### Minimal working example
```bash
# Fast developer feedback loop for custom operation changes
python3 -m pytest hoomd/pytest/test_custom_updater.py hoomd/pytest/test_custom_tuner.py
python3 -m pytest hoomd/pytest/test_operation.py hoomd/pytest/test_operations.py
```

### Validation checkpoints
- Relevant targeted tests pass before wider test selection.
- New/changed API parameters are validated through ParameterDict/TypeParameter checks.
- Operation attach/detach lifecycle behaves as expected in `simulation.operations`.

### Pitfalls and fixes
- Editing C++/Python interface boundaries without corresponding tests causes regressions.
- Missing module registration updates can hide new functionality from Python.
- Skipping attach-time lifecycle checks often leaves latent runtime errors.

### High-value source entry links
- `hoomd/module.cc`, `hoomd/md/module-md.cc`, `hoomd/hpmc/module.cc`, `hoomd/mpcd/module.cc` for Python module bindings.
- `hoomd/custom/custom_action.py` and `hoomd/custom/custom_operation.py` for extension mechanics.
- `hoomd/data/parameterdicts.py` and `hoomd/data/typeconverter.py` for API validation contracts.
- `hoomd/pytest_plugin_validate.py` and `hoomd/conftest.py` for test harness behavior.

## Scope
- Handle questions about developer architecture, extension points, and contribution workflow.
- Prioritize actionable guidance for implementing and validating changes.

## Primary documentation references
- `ARCHITECTURE.md`
- `CONTRIBUTING.rst`
- `BUILDING.rst`
- `sphinx-doc/developers.rst`
- `sphinx-doc/contributing.rst`
- `sphinx-doc/components.rst`
- `sphinx-doc/building.rst`
- `sphinx-doc/testing.rst`

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
- `hoomd/module.cc`
- `hoomd/md/module-md.cc`
- `hoomd/hpmc/module.cc`
- `hoomd/mpcd/module.cc`
- `hoomd/operation.py`
- `hoomd/custom/custom_action.py`
- `hoomd/custom/custom_operation.py`
- `hoomd/data/parameterdicts.py`
- `hoomd/data/typeconverter.py`
- `hoomd/pytest_plugin_validate.py`
- `hoomd/conftest.py`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" CMake hoomd`).
