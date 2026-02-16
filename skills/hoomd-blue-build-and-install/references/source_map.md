# hoomd-blue source map: Build and Install

Generated from source roots:
- `CMake`
- `hoomd`

Use this map only after exhausting the topic docs in `doc_map.md`.

## Topic query tokens
- `build`
- `cmake`
- `cuda`
- `hip`
- `install`
- `mpi`
- `ninja`
- `pytest`

## Fast source navigation
- `rg -n "<symbol_or_keyword>" CMake hoomd`
- `rg -n "option\(|ENABLE_|find_package|add_subdirectory" CMakeLists.txt CMake hoomd`
- If a doc mentions a build flag, search that exact flag first.

## Suggested source entry points
- `CMakeLists.txt` | why: top-level project options and dependency wiring.
- `CMake/CMakeLists.txt` | why: shared CMake include logic.
- `CMake/hoomd/hoomd-macros.cmake` | why: common CMake macros used across modules.
- `CMake/hoomd/HOOMDPythonSetup.cmake` | why: Python module build/install behavior.
- `CMake/hoomd/HOOMDMPISetup.cmake` | why: MPI feature gating and setup.
- `CMake/hoomd/HOOMDCUDASetup.cmake` | why: CUDA compiler/toolkit setup.
- `CMake/hoomd/HOOMDHIPSetup.cmake` | why: HIP/ROCm setup path.
- `hoomd/CMakeLists.txt` | why: core HOOMD module build graph.
- `hoomd/md/CMakeLists.txt` | why: MD component build options.
- `hoomd/hpmc/CMakeLists.txt` | why: HPMC component build options.
- `hoomd/mpcd/CMakeLists.txt` | why: MPCD component build options.
- `hoomd/pytest/CMakeLists.txt` | why: Python test packaging in build tree.
- `hoomd/md/pytest/CMakeLists.txt` | why: MD pytest packaging and discovery.
- `hoomd/hpmc/pytest/CMakeLists.txt` | why: HPMC pytest packaging and discovery.
- `hoomd/mpcd/pytest/CMakeLists.txt` | why: MPCD pytest packaging and discovery.
- `hoomd/pytest/pytest-openmpi.sh` | why: MPI pytest invocation wrapper.
