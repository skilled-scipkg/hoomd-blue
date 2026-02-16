# hoomd-blue source map: Getting Started

Generated from source roots:
- `CMake`
- `hoomd`

Use this map only after exhausting the topic docs in `doc_map.md`.

## Topic query tokens
- `baseline`
- `first-run`
- `getting-started`
- `integrator`
- `quickstart`
- `simulation`
- `state`
- `timestep`

## Fast source navigation
- `rg -n "<symbol_or_keyword>" CMake hoomd`
- `rg -n "make_example_simulation|create_state_from|run\(" hoomd`
- Confirm a startup answer with a minimal script and one short run.

## Suggested source entry points
- `hoomd/__init__.py` | why: top-level API imports and feature exposure.
- `hoomd/device.py` | why: CPU/GPU device selection entry point.
- `hoomd/simulation.py` | why: simulation lifecycle and run semantics.
- `hoomd/state.py` | why: state creation and snapshot access.
- `hoomd/operations.py` | why: operation container basics.
- `hoomd/operation.py` | why: operation interface behavior.
- `hoomd/util.py` | why: minimal example simulation helper.
- `hoomd/pytest/test_simulation.py` | why: baseline simulation behavior tests.
- `hoomd/pytest/test_state.py` | why: state-management behavior tests.
- `hoomd/pytest/test_operations.py` | why: operation wiring behavior tests.
- `hoomd/md/pytest/test_integrate.py` | why: minimal MD integrator behavior checks.
