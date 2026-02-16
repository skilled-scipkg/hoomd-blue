# Evidence: hoomd-blue-simulation-workflows

## Primary docs
- `sphinx-doc/features.rst`
- `sphinx-doc/testing.rst`
- `sphinx-doc/howto/compute-the-free-energy-of-solids.rst`
- `sphinx-doc/howto/prevent-particles-from-moving.rst`
- `sphinx-doc/hoomd/simulation.rst`
- `sphinx-doc/hoomd/operation/integrator.rst`
- `sphinx-doc/hoomd/mpcd/integrator.rst`
- `sphinx-doc/hoomd/md/integrator.rst`
- `sphinx-doc/hoomd/error/simulationdefinitionerror.rst`
- `sphinx-doc/hoomd/mpcd/collide/stochasticrotationdynamics.rst`
- `sphinx-doc/hoomd/hpmc/integrate/hpmcintegrator.rst`
- `sphinx-doc/hoomd/hpmc/nec/integrate/hpmcnecintegrator.rst`

## Primary source entry points
- `skills/hoomd-blue-simulation-workflows/references/doc_map.md`
- `hoomd/hpmc/nec/integrate.py`
- `hoomd/hpmc/pytest/test_compute_free_volume.py`
- `hoomd/hpmc/pair/step.py`
- `hoomd/hpmc/pair/angular_step.py`
- `hoomd/hpmc/pytest/test_pair_step.py`
- `hoomd/hpmc/pytest/test_pair_angular_step.py`
- `hoomd/hpmc/nec/__init__.py`
- `hoomd/mpcd/Integrator.h`
- `hoomd/mpcd/Integrator.cc`
- `hoomd/mpcd/integrate.py`
- `hoomd/mpcd/collide.py`
- `hoomd/hpmc/module_sphinx.cc`
- `hoomd/hpmc/integrate.py`
- `hoomd/hpmc/compute.py`
- `hoomd/hpmc/nec/tune.py`
- `hoomd/hpmc/nec/CMakeLists.txt`
- `hoomd/mpcd/pytest/test_integrator.py`
- `hoomd/mpcd/pytest/test_collide.py`
- `hoomd/hpmc/pytest/test_nec.py`

## Extracted headings
- (none extracted)

## Executable command hints
- Python package
- Python ecosystem and to be extendable in user scripts. To enable interoperability, all operations
- mpirun -n 2 build/hoomd/hoomd/pytest/pytest-openmpi.sh -v -x build/hoomd
- $ python3 -m pytest build/hoomd --validate -m validate
- $ mpirun -n 2 hoomd/pytest/pytest-openmpi.sh build/hoomd -v -x -ra --validate -m validate

## Warnings and pitfalls
- .. warning::
- .. important::
