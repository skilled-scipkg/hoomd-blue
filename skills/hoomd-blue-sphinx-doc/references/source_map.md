# hoomd-blue source map: Sphinx Doc

Generated from source roots:
- `CMake`
- `hoomd`

Use this map only after exhausting the topic docs in `doc_map.md`.

## Topic query tokens
- `documentation`
- `module`
- `notation`
- `state`
- `units`
- `variant`
- `version`
- `write`

## Fast source navigation
- `rg -n "<symbol_or_keyword>" CMake hoomd`
- `rg -n "class|def|__doc__|loggables|TypeParameter" hoomd`
- Use source only when docs are ambiguous or omit behavioral nuance.

## Suggested source entry points
- `hoomd/__init__.py` | why: exported top-level API mapping to docs.
- `hoomd/simulation.py` | why: simulation semantics referenced by docs.
- `hoomd/state.py` | why: state API implementation behind docs.
- `hoomd/operations.py` | why: operation container implementation details.
- `hoomd/box.py` | why: box API behavior behind docs.
- `hoomd/wall.py` | why: wall API behavior and constraints.
- `hoomd/variant/scalar.py` | why: scalar variant behavior.
- `hoomd/variant/box.py` | why: box variant behavior.
- `hoomd/write/gsd.py` | why: GSD writer behavior.
- `hoomd/write/hdf5.py` | why: HDF5 log writer behavior.
- `hoomd/write/dcd.py` | why: DCD writer behavior.
- `hoomd/version.py` | why: runtime version reporting behavior.
- `hoomd/version_config.py` | why: compiled feature/version config values.
- `hoomd/module.cc` | why: pybind-exposed symbol organization.
- `hoomd/pytest/test_variant.py` | why: documented variant behavior checks.
- `hoomd/pytest/test_simulation.py` | why: documented simulation behavior checks.
