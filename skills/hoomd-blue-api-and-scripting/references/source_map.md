# hoomd-blue source map: API and Scripting

Generated from source roots:
- `CMake`
- `hoomd`

Use this map only after exhausting the topic docs in `doc_map.md`.

## Topic query tokens
- `api`
- `action`
- `custom`
- `logging`
- `operation`
- `parameterdict`
- `scripting`
- `typeparameter`

## Fast source navigation
- `rg -n "<symbol_or_keyword>" CMake hoomd`
- `rg -n "class|def|struct|namespace" CMake hoomd`
- If a doc mentions a function/class, search that exact symbol first, then inspect nearby implementation files.

## Suggested source entry points
- `hoomd/operation.py` | why: base operation lifecycle and attach/detach semantics.
- `hoomd/operations.py` | why: operation container orchestration.
- `hoomd/custom/custom_action.py` | why: user-defined action API.
- `hoomd/custom/custom_operation.py` | why: wrapper behavior for custom operations.
- `hoomd/logging.py` | why: logger API and category behavior.
- `hoomd/data/parameterdicts.py` | why: runtime parameter validation and mutability.
- `hoomd/data/typeparam.py` | why: type-parameter mapping behavior.
- `hoomd/data/typeconverter.py` | why: type conversion and validation errors.
- `hoomd/box.py` | why: box API objects used in scripted workflows.
- `hoomd/variant/box.py` | why: box variant API behavior.
- `hoomd/update/box_resize.py` | why: scripted box-resize operation behavior.
- `hoomd/pytest/test_operation.py` | why: operation-level behavior checks.
- `hoomd/pytest/test_operations.py` | why: container behavior and ordering checks.
- `hoomd/pytest/test_custom_writer.py` | why: custom action/writer integration checks.
- `hoomd/pytest/test_type_parameter_dict.py` | why: type-parameter validation behavior tests.
- `hoomd/pytest/test_box_variant.py` | why: scripted box-variant behavior tests.
