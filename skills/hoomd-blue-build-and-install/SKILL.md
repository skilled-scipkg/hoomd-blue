---
name: hoomd-blue-build-and-install
description: This skill should be used when users ask about build and install in hoomd-blue; it prioritizes documentation references and then source inspection only for unresolved details.
---

# hoomd-blue: Build and Install

## High-Signal Playbook
### Route conditions
- Use this skill for install/build commands, dependency setup, CMake options, and build/test failures (`sphinx-doc/installation.rst`, `sphinx-doc/building.rst`, `BUILDING.rst`).
- Route to `hoomd-blue-simulation-workflows` once the package imports and the user is asking about runtime setup.
- Route to `hoomd-blue-parallel-hpc` for MPI rank layout, GPU efficiency tuning, and scheduler launch strategy.

### Triage questions
- Is a binary install sufficient, or is source required for MPI / native CUDA / HIP (`INSTALLING.rst`)?
- Which Python environment is targeted for install (isolated dev env vs system env)?
- Are `ENABLE_MPI`, `ENABLE_GPU`, precision flags, or optional modules needed (`BUILDING.rst`)?
- Was the repository cloned with submodules (`--recursive`)?
- Is the goal a developer build with tests, or an end-user install only?

### Canonical workflow
1. Install prerequisites and test dependencies (`BUILDING.rst`).
2. Clone source with submodules (`BUILDING.rst`).
3. Configure with CMake/Ninja and required feature flags (`BUILDING.rst`).
4. Build with `ninja`.
5. Run tests (`ctest` and `python3 -m pytest hoomd`; MPI wrapper in `sphinx-doc/testing.rst`).
6. Install with `ninja install` or use `PYTHONPATH=<repo>/build` for non-invasive testing (`BUILDING.rst`).
7. Smoke-test import and a short run.

### Minimal working example
```bash
micromamba install cmake eigen git ninja numpy pybind11 python pytest rowan
git clone --recursive git@github.com:glotzerlab/hoomd-blue.git
cd hoomd-blue
cmake -B build -S . -GNinja -DENABLE_GPU=off -DENABLE_MPI=off
cd build
ninja
python3 -m pytest hoomd
```

### Pitfalls and fixes
- Missing submodules after checkout: run `git submodule update --init` (`BUILDING.rst`).
- CMake ignores new library/tool paths: delete the build directory CMake cache file, then reconfigure (`BUILDING.rst`).
- Conda toolchain conflicts in dev envs: remove `clang` / `gcc` from the env (`BUILDING.rst` warning).
- `ninja install` replaces another HOOMD install: validate from `PYTHONPATH=<repo>/build` first (`BUILDING.rst` warning).
- CUDA pin mismatch on binary installs: align `CONDA_OVERRIDE_CUDA` with available package builds (`INSTALLING.rst`).

### Convergence/validation checks
- `python3 -c "import hoomd; print(hoomd.version.version)"` works in the intended environment.
- `python3 -m pytest hoomd` passes for serial builds (`BUILDING.rst`).
- MPI build test wrapper succeeds (`mpirun -n 2 build/hoomd/hoomd/pytest/pytest-openmpi.sh ...`, `sphinx-doc/testing.rst`).
- Optional doc build succeeds when needed: `sphinx-build -b html sphinx-doc html` (`BUILDING.rst`).

### High-value source entry links
- `hoomd/CMakeLists.txt` for top-level build options and module wiring.
- `hoomd/pytest/CMakeLists.txt` and `hoomd/md/pytest/CMakeLists.txt` for Python test packaging in build trees.
- `hoomd/hpmc/CMakeLists.txt` and `hoomd/mpcd/CMakeLists.txt` for feature-specific build gates.
- `hoomd/md/long_range/CMakeLists.txt` for long-range (PPPM) build dependencies.

## Scope
- Handle questions about build, installation, compilation, and environment setup.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `sphinx-doc/components.rst`
- `sphinx-doc/requirements.txt`
- `hoomd/test/CMakeLists.txt`
- `hoomd/mpcd/test/CMakeLists.txt`
- `hoomd/md/test/CMakeLists.txt`
- `sphinx-doc/installation.rst`
- `sphinx-doc/building.rst`
- `hoomd/pytest/CMakeLists.txt`
- `hoomd/hpmc/test/CMakeLists.txt`
- `sphinx-doc/hoomd/util/make_example_simulation.rst`
- `hoomd/mpcd/pytest/CMakeLists.txt`
- `hoomd/md/pytest/CMakeLists.txt`

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
- `hoomd/md/long_range/CMakeLists.txt`
- `hoomd/mpcd/pytest/CMakeLists.txt`
- `hoomd/hpmc/pytest/CMakeLists.txt`
- `hoomd/mpcd/CMakeLists.txt`
- `hoomd/hpmc/CMakeLists.txt`
- `hoomd/hpmc/tune/CMakeLists.txt`
- `hoomd/hpmc/pair/CMakeLists.txt`
- `hoomd/hpmc/nec/CMakeLists.txt`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" CMake hoomd`).
