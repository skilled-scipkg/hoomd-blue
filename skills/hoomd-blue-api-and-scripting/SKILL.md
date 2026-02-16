---
name: hoomd-blue-api-and-scripting
description: This skill should be used when users ask about api and scripting in hoomd-blue; it prioritizes documentation references and then source inspection only for unresolved details.
---

# hoomd-blue: API and Scripting

## High-Signal Playbook
### Route conditions
- Use this skill for Python API usage, custom actions/operations, parameter dictionaries, and script architecture.
- Route to `hoomd-blue-simulation-workflows` when the question is about full run pipelines instead of API object semantics.
- Route to `hoomd-blue-theory-and-methods` for ensemble/method selection questions.

### Triage questions
- Is the user writing plain scripts or custom `Action` / `CustomUpdater` / `CustomWriter` logic?
- Are they debugging API validation errors (`TypeConversionError`, mutability, invalid parameters)?
- Do they need box variants/resizing, custom logging, or operation scheduling?

### Canonical workflow
1. Define simulation state and integrator first.
2. Add custom actions/writers/updaters through `simulation.operations`.
3. Validate parameter dictionaries/types before long runs.
4. Add lightweight logging to observe API effects at runtime.
5. Confirm behavior with short test runs before scaling.

### Minimal working example
```bash
python3 - <<'PY'
import hoomd

class StepPrinter(hoomd.custom.Action):
    def act(self, timestep):
        if timestep % 200 == 0:
            print("step", timestep)

sim = hoomd.util.make_example_simulation()
lj = hoomd.md.pair.LJ(nlist=hoomd.md.nlist.Cell(buffer=0.4))
lj.params[("A", "A")] = dict(epsilon=1.0, sigma=1.0)
lj.r_cut[("A", "A")] = 2.5
cv = hoomd.md.methods.ConstantVolume(filter=hoomd.filter.All())
sim.operations.integrator = hoomd.md.Integrator(dt=0.001, methods=[cv], forces=[lj])
sim.operations.writers.append(
    hoomd.write.CustomWriter(
        action=StepPrinter(),
        trigger=hoomd.trigger.Periodic(100),
    )
)
sim.run(1_000)
PY
```

### Validation checkpoints
- Custom action executes on expected trigger cadence.
- No parameter/type validation errors are raised at attach time.
- `sim.operations` contains the expected integrator and writer objects.

### Pitfalls and fixes
- Adding custom operations before defining a valid simulation state can lead to attach-time errors.
- Writing directly to immutable API fields raises `MutabilityError`; rebuild objects when required.
- Type mismatch in parameter dictionaries is usually caught immediately; verify key/value types and tuple keys.

### High-value source entry links
- `hoomd/operation.py` and `hoomd/operations.py` for operation lifecycle and scheduling.
- `hoomd/custom/custom_action.py` and `hoomd/custom/custom_operation.py` for user extension points.
- `hoomd/data/parameterdicts.py` and `hoomd/data/typeconverter.py` for validation behavior.
- `hoomd/pytest/test_custom_writer.py` and `hoomd/pytest/test_operation.py` for concrete behavior checks.

## Scope
- Handle questions about language bindings, APIs, and programmatic interfaces.
- Keep responses practical for writing robust scripts and custom extensions.

## Primary documentation references
- `sphinx-doc/module-hoomd.rst`
- `sphinx-doc/hoomd/operations.rst`
- `sphinx-doc/hoomd/operation/operation.rst`
- `sphinx-doc/hoomd/operation/integrator.rst`
- `sphinx-doc/hoomd/module-custom.rst`
- `sphinx-doc/hoomd/custom/action.rst`
- `sphinx-doc/hoomd/custom/customoperation.rst`
- `sphinx-doc/hoomd/logging/logger.rst`
- `sphinx-doc/hoomd/data/typeparameter.rst`
- `sphinx-doc/hoomd/box/boxinterface.rst`
- `sphinx-doc/style.rst`

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
- `hoomd/operation.py`
- `hoomd/operations.py`
- `hoomd/custom/custom_action.py`
- `hoomd/custom/custom_operation.py`
- `hoomd/logging.py`
- `hoomd/data/parameterdicts.py`
- `hoomd/data/typeparam.py`
- `hoomd/data/typeconverter.py`
- `hoomd/pytest/test_operation.py`
- `hoomd/pytest/test_operations.py`
- `hoomd/pytest/test_custom_writer.py`
- `hoomd/pytest/test_type_parameter_dict.py`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" CMake hoomd`).
