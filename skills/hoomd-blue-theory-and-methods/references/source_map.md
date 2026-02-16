# hoomd-blue source map: Theory and Methods

Generated from source roots:
- `CMake`
- `hoomd`

Use this map only after exhausting the topic docs in `doc_map.md`.

## Topic query tokens
- `barostat`
- `collide`
- `integrator`
- `langevin`
- `method`
- `rattle`
- `stream`
- `thermostat`

## Fast source navigation
- `rg -n "<symbol_or_keyword>" CMake hoomd`
- `rg -n "ConstantVolume|ConstantPressure|Langevin|RATTLE|Collision|Streaming" hoomd`
- For method questions, pair implementation files with matching pytest cases.

## Suggested source entry points
- `hoomd/md/methods/methods.py` | why: core MD method constraints and options.
- `hoomd/md/methods/thermostats.py` | why: thermostat implementation details.
- `hoomd/md/methods/rattle.py` | why: manifold-constrained dynamics behavior.
- `hoomd/md/alchemy/methods.py` | why: alchemical method behavior.
- `hoomd/md/integrate.py` | why: MD integration stack behavior.
- `hoomd/hpmc/integrate.py` | why: HPMC integrator behavior.
- `hoomd/hpmc/nec/integrate.py` | why: NEC method limitations/constraints.
- `hoomd/mpcd/methods.py` | why: MPCD methods behavior.
- `hoomd/mpcd/stream.py` | why: streaming method behavior.
- `hoomd/mpcd/collide.py` | why: collision method behavior.
- `hoomd/mpcd/StreamingMethod.h` | why: low-level streaming algorithm interface.
- `hoomd/mpcd/CollisionMethod.h` | why: low-level collision algorithm interface.
- `hoomd/md/pytest/test_methods.py` | why: MD method behavior checks.
- `hoomd/hpmc/pytest/test_nec.py` | why: NEC method behavior checks.
- `hoomd/mpcd/pytest/test_methods.py` | why: MPCD method behavior checks.
- `hoomd/mpcd/pytest/test_stream.py` | why: streaming behavior checks.
- `hoomd/mpcd/pytest/test_collide.py` | why: collision behavior checks.
