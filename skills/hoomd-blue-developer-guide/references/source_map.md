# hoomd-blue source map: Developer Guide

Generated from source roots:
- `CMake`
- `hoomd`

Use this map only after exhausting the topic docs in `doc_map.md`.

## Topic query tokens
- `architecture`
- `binding`
- `contributing`
- `custom`
- `internals`
- `module`
- `plugin`
- `testing`

## Fast source navigation
- `rg -n "<symbol_or_keyword>" CMake hoomd`
- `rg -n "PYBIND11_MODULE|export|class .*\(|def " hoomd`
- Follow each API symbol to its test file before changing behavior.

## Suggested source entry points
- `hoomd/module.cc` | why: core Python bindings registration.
- `hoomd/md/module-md.cc` | why: MD bindings and exported symbols.
- `hoomd/hpmc/module.cc` | why: HPMC bindings and exported symbols.
- `hoomd/mpcd/module.cc` | why: MPCD bindings and exported symbols.
- `hoomd/operation.py` | why: base operation contract developers extend.
- `hoomd/custom/custom_action.py` | why: user extension base class.
- `hoomd/custom/custom_operation.py` | why: custom operation wiring.
- `hoomd/data/parameterdicts.py` | why: parameter validation contract.
- `hoomd/data/typeconverter.py` | why: conversion path for API inputs.
- `hoomd/pytest_plugin_validate.py` | why: pytest plugin validation hooks.
- `hoomd/conftest.py` | why: shared pytest fixtures and configuration.
- `hoomd/pytest/test_custom_updater.py` | why: custom updater behavior tests.
- `hoomd/pytest/test_custom_tuner.py` | why: custom tuner behavior tests.
- `hoomd/pytest/test_custom_writer.py` | why: custom writer behavior tests.
- `hoomd/pytest/test_operation.py` | why: operation lifecycle regression checks.
