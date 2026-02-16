# hoomd-blue source map: Inputs and Modeling

Generated from source roots:
- `CMake`
- `hoomd`

Use this map only after exhausting the topic docs in `doc_map.md`.

## Topic query tokens
- `bonded`
- `geometry`
- `input`
- `modeling`
- `mpcd`
- `snapshot`
- `topology`
- `wall`

## Fast source navigation
- `rg -n "<symbol_or_keyword>" CMake hoomd`
- `rg -n "Snapshot|Wall|Geometry|fill|bond|angle|dihedral" hoomd`
- Validate model setup with small-system tests before scaling.

## Suggested source entry points
- `hoomd/snapshot.py` | why: input state/snapshot structure.
- `hoomd/wall.py` | why: wall geometry semantics and constraints.
- `hoomd/md/bond.py` | why: bonded interaction setup.
- `hoomd/md/angle.py` | why: angle interaction setup.
- `hoomd/md/dihedral.py` | why: dihedral interaction setup.
- `hoomd/md/improper.py` | why: improper interaction setup.
- `hoomd/mpcd/geometry.py` | why: MPCD geometry object behavior.
- `hoomd/mpcd/fill.py` | why: virtual-particle filling behavior.
- `hoomd/mpcd/stream.py` | why: boundary coupling and streaming behavior.
- `hoomd/mpcd/methods.py` | why: MPCD method constraints near walls.
- `hoomd/pytest/test_snapshot.py` | why: snapshot behavior checks.
- `hoomd/md/pytest/test_bond.py` | why: bond-model behavior checks.
- `hoomd/md/pytest/test_angle.py` | why: angle-model behavior checks.
- `hoomd/md/pytest/test_wall_potential.py` | why: wall interaction behavior checks.
- `hoomd/mpcd/pytest/test_geometry.py` | why: geometry behavior checks.
- `hoomd/mpcd/pytest/test_fill.py` | why: filler behavior checks.
