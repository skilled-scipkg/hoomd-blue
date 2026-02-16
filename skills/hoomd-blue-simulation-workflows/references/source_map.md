# hoomd-blue source map: Simulation Workflows

Generated from source roots:
- `CMake`
- `hoomd`

Use this map only after exhausting the topic docs in `doc_map.md`.

## Topic query tokens
- `collide`
- `integrator`
- `operations`
- `run`
- `simulation`
- `state`
- `stream`
- `workflow`

## Fast source navigation
- `rg -n "<symbol_or_keyword>" CMake hoomd`
- `rg -n "create_state_from|operations|Integrator|run\(" hoomd`
- For workflow bugs, trace state creation -> operation attach -> run loop.

## Suggested source entry points
- `hoomd/simulation.py` | why: lifecycle, seed, and run-step semantics.
- `hoomd/state.py` | why: state creation/access and snapshot interaction.
- `hoomd/operations.py` | why: operation composition and ordering.
- `hoomd/operation.py` | why: operation interface/attach behavior.
- `hoomd/md/integrate.py` | why: MD integrator runtime behavior.
- `hoomd/hpmc/integrate.py` | why: HPMC integrator runtime behavior.
- `hoomd/hpmc/nec/integrate.py` | why: NEC-specific execution constraints.
- `hoomd/mpcd/integrate.py` | why: MPCD integrator behavior and coupling.
- `hoomd/mpcd/stream.py` | why: MPCD streaming schedule behavior.
- `hoomd/mpcd/collide.py` | why: MPCD collision schedule behavior.
- `hoomd/write/gsd.py` | why: restart/checkpoint writer behavior.
- `hoomd/pytest/test_simulation.py` | why: simulation lifecycle behavior checks.
- `hoomd/md/pytest/test_integrate.py` | why: MD integration behavior checks.
- `hoomd/hpmc/pytest/test_nec.py` | why: NEC runtime behavior checks.
- `hoomd/mpcd/pytest/test_integrator.py` | why: MPCD integrator behavior checks.
- `hoomd/mpcd/pytest/test_collide.py` | why: MPCD collide behavior checks.
