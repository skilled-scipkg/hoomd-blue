# hoomd-blue source map: Troubleshooting

Generated from source roots:
- `CMake`
- `hoomd`

Use this map only after exhausting the topic docs in `doc_map.md`.

## Topic query tokens
- `dataaccesserror`
- `failure`
- `incompletespecificationerror`
- `mutabilityerror`
- `simulationdefinitionerror`
- `traceback`
- `typeconversionerror`
- `warning`

## Fast source navigation
- `rg -n "<symbol_or_keyword>" CMake hoomd`
- `rg -n "raise .*Error|warning|validate|attach" hoomd`
- Build a minimal reproducer and trace the exact raise/validation path.

## Suggested source entry points
- `hoomd/error.py` | why: exception classes and warning types.
- `hoomd/data/typeconverter.py` | why: type-conversion failure paths.
- `hoomd/data/parameterdicts.py` | why: specification/validation failure paths.
- `hoomd/operation.py` | why: attach-time failure paths for operations.
- `hoomd/operations.py` | why: operation ordering and lifecycle issues.
- `hoomd/simulation.py` | why: simulation definition and run errors.
- `hoomd/state.py` | why: state-creation and access errors.
- `hoomd/device.py` | why: runtime device availability failures.
- `hoomd/communicator.py` | why: communicator/rank setup issues.
- `hoomd/pytest/test_operation.py` | why: operation error-path regression tests.
- `hoomd/pytest/test_parameter_dict.py` | why: parameter validation error tests.
- `hoomd/pytest/test_type_parameter_dict.py` | why: type-parameter error tests.
- `hoomd/pytest/test_simulation.py` | why: simulation lifecycle error tests.
- `hoomd/pytest/test_device.py` | why: device availability error tests.
- `hoomd/pytest/test_local_snapshot.py` | why: data-access edge-case tests.
