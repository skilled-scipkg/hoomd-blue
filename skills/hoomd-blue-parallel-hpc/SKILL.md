---
name: hoomd-blue-parallel-hpc
description: This skill should be used when users ask about parallel and hpc in hoomd-blue; it prioritizes documentation references and then source inspection only for unresolved details.
---

# hoomd-blue: Parallel and HPC

## High-Signal Playbook
### Route conditions
- Use this skill for MPI rank layout, GPU selection, distributed execution, and HPC-oriented validation.
- Route to `hoomd-blue-build-and-install` when MPI/GPU support is missing at build time.
- Route to `hoomd-blue-simulation-workflows` for non-HPC-specific simulation pipeline questions.

### Triage questions
- Is the target environment single GPU, multi-GPU, MPI multi-rank, or hybrid?
- Was HOOMD built with `ENABLE_MPI` / GPU support for this environment?
- Is the user optimizing correctness first or throughput/scaling first?

### Canonical workflow
1. Confirm device and communicator setup in a short script.
2. Run a short single-rank baseline.
3. Run a short MPI/GPU variant with identical physics settings.
4. Compare timestep progression and key observables for consistency.
5. Benchmark only after autotuning warm-up.

### Minimal working example
```bash
python3 - <<'PY'
import hoomd

device = hoomd.device.auto_select()
sim = hoomd.util.make_example_simulation(device=device)
print("ranks", device.communicator.num_ranks)
print("rank", device.communicator.rank)
sim.run(500)
print("timestep", sim.timestep)
PY
```

### Validation checkpoints
- `device.communicator.num_ranks` matches launcher intent.
- No `MPINotAvailableError` / `GPUNotAvailableError` in target environment.
- Short run reaches expected timestep on each rank without divergence.

### Pitfalls and fixes
- Launching with MPI when HOOMD is built without MPI triggers immediate runtime errors.
- Selecting GPU-specific APIs in CPU-only builds fails at attach time.
- Comparing performance before warm-up/autotuning leads to misleading throughput decisions.

### High-value source entry links
- `hoomd/device.py` and `hoomd/communicator.py` for runtime device/rank behavior.
- `hoomd/error.py` for MPI/GPU availability exceptions.
- `hoomd/data/local_access_gpu.py` and `hoomd/md/data/local_access_gpu.py` for GPU local-access semantics.
- `CMake/hoomd/HOOMDMPISetup.cmake` and `CMake/hoomd/HOOMDCUDASetup.cmake` for build-side feature enablement.

## Scope
- Handle questions about MPI/OpenMP/GPU execution, scaling, and batch systems.
- Keep guidance centered on reproducible correctness and then performance.

## Primary documentation references
- `sphinx-doc/testing.rst`
- `sphinx-doc/howto/determine-the-most-efficient-device.rst`
- `sphinx-doc/hoomd/device/auto_select.rst`
- `sphinx-doc/hoomd/device/gpu.rst`
- `sphinx-doc/hoomd/communicator/communicator.rst`
- `sphinx-doc/hoomd/error/mpinotavailableerror.rst`
- `sphinx-doc/hoomd/error/gpunotavailableerror.rst`
- `sphinx-doc/hoomd/data/localsnapshotgpu.rst`
- `sphinx-doc/hoomd/data/hoomdgpuarray.rst`
- `sphinx-doc/hoomd/md/data/neighborlistlocalaccessgpu.rst`
- `sphinx-doc/hoomd/md/data/forcelocalaccessgpu.rst`

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
- `hoomd/device.py`
- `hoomd/communicator.py`
- `hoomd/error.py`
- `hoomd/data/local_access_gpu.py`
- `hoomd/md/data/local_access_gpu.py`
- `hoomd/ExecutionConfiguration.h`
- `hoomd/ExecutionConfiguration.cc`
- `hoomd/Communicator.h`
- `hoomd/Communicator.cc`
- `CMake/hoomd/HOOMDMPISetup.cmake`
- `CMake/hoomd/HOOMDCUDASetup.cmake`
- `hoomd/pytest/test_communicator.py`
- `hoomd/pytest/test_device.py`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" CMake hoomd`).
