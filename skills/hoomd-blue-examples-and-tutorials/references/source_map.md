# hoomd-blue source map: Examples and Tutorials

Generated from source roots:
- `CMake`
- `hoomd`

Use this map only after exhausting the topic docs in `doc_map.md`.

## Topic query tokens
- `example`
- `fire`
- `howto`
- `hpmc`
- `minimize`
- `neighbor-list`
- `tune`
- `tutorial`

## Fast source navigation
- `rg -n "<symbol_or_keyword>" CMake hoomd`
- `rg -n "make_example_simulation|FIRE|move_size|nlist_buffer" hoomd`
- Confirm behavior with the nearest pytest before extending tutorial logic.

## Suggested source entry points
- `hoomd/util.py` | why: `make_example_simulation()` used by many docs/examples.
- `hoomd/md/minimize/fire.py` | why: FIRE minimization behavior.
- `hoomd/md/tune/nlist_buffer.py` | why: neighbor-list buffer tuner logic.
- `hoomd/hpmc/tune/move_size.py` | why: HPMC move-size tuning core.
- `hoomd/hpmc/tune/mc_move_tune.py` | why: HPMC move-size tuner scaffolding.
- `hoomd/hpmc/tune/boxmc_move_size.py` | why: BoxMC move tuning behavior.
- `hoomd/tune/custom_tuner.py` | why: custom tuning extension pattern.
- `hoomd/write/gsd.py` | why: checkpoint/output behavior for tutorial scripts.
- `hoomd/md/pytest/test_minimize_fire.py` | why: minimization correctness checks.
- `hoomd/md/pytest/test_nlist_tuner.py` | why: buffer tuning behavior checks.
- `hoomd/hpmc/pytest/test_move_size_tuner.py` | why: HPMC move-size tuning checks.
- `hoomd/hpmc/pytest/test_boxmc_move_tuner.py` | why: BoxMC move-size tuning checks.
- `hoomd/hpmc/pytest/test_boxmc.py` | why: BoxMC integration behavior checks.
- `hoomd/pytest/test_custom_writer.py` | why: lightweight scripted output checks.
