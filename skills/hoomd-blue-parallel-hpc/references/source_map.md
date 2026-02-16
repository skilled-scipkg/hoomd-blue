# hoomd-blue source map: Parallel and HPC

Generated from source roots:
- `CMake`
- `hoomd`

Use this map only after exhausting the topic docs in `doc_map.md`.

## Topic query tokens
- `communicator`
- `cuda`
- `device`
- `gpu`
- `mpi`
- `parallel`
- `rank`
- `scaling`

## Fast source navigation
- `rg -n "<symbol_or_keyword>" CMake hoomd`
- `rg -n "MPI|GPU|communicator|ExecutionConfiguration|local_access_gpu" hoomd CMake`
- When debugging rank/device issues, inspect both build setup and Python runtime wiring.

## Suggested source entry points
- `hoomd/device.py` | why: Python device selection and availability checks.
- `hoomd/communicator.py` | why: rank/partition communicator behavior.
- `hoomd/error.py` | why: MPI/GPU availability exception definitions.
- `hoomd/data/local_access_gpu.py` | why: GPU local data access behavior.
- `hoomd/md/data/local_access_gpu.py` | why: MD-specific GPU local access behavior.
- `hoomd/ExecutionConfiguration.h` | why: low-level execution backend configuration.
- `hoomd/ExecutionConfiguration.cc` | why: backend selection/runtime setup implementation.
- `hoomd/Communicator.h` | why: domain communication internals.
- `hoomd/Communicator.cc` | why: communicator implementation details.
- `CMake/hoomd/HOOMDMPISetup.cmake` | why: MPI build-time configuration.
- `CMake/hoomd/HOOMDCUDASetup.cmake` | why: CUDA build-time configuration.
- `CMake/hoomd/HOOMDHIPSetup.cmake` | why: HIP build-time configuration.
- `hoomd/pytest/test_communicator.py` | why: communicator behavior tests.
- `hoomd/pytest/test_device.py` | why: device selection behavior tests.
- `hoomd/mpcd/pytest/test_snapshot.py` | why: GPU/local snapshot access behavior checks.
- `hoomd/md/pytest/test_array_view.py` | why: low-level data access behavior checks.
