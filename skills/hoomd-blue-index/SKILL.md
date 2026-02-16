---
name: hoomd-blue-index
description: This skill should be used when users ask how to use hoomd-blue and the correct generated documentation skill must be selected before going deeper into source code.
---

# hoomd-blue Skills Index

## Route the request
- Classify the request into one of the generated topic skills listed below.
- Prefer abstract, workflow-level guidance for large scientific packages; do not attempt full function-by-function coverage unless explicitly requested.

## Immediate Startup Path
1. Start with `hoomd-blue-getting-started` to validate import and run a minimal 1,000-step simulation.
2. Move to `hoomd-blue-simulation-workflows` to structure restart/logging/production flow.
3. Switch to a specialized skill (`inputs-and-modeling`, `theory-and-methods`, `parallel-hpc`, etc.) once baseline behavior is confirmed.

## Generated topic skills
- `hoomd-blue-sphinx-doc`: Sphinx Doc (documentation grouped under the 'sphinx-doc' theme)
- `hoomd-blue-theory-and-methods`: Theory and Methods (theoretical background and algorithmic methods)
- `hoomd-blue-build-and-install`: Build and Install (build, installation, compilation, and environment setup)
- `hoomd-blue-simulation-workflows`: Simulation Workflows (simulation setup, execution flow, and runtime controls)
- `hoomd-blue-inputs-and-modeling`: Inputs and Modeling (inputs, system setup, models, and physical parameterization)
- `hoomd-blue-examples-and-tutorials`: Examples and Tutorials (worked examples, tutorials, and cookbook usage)
- `hoomd-blue-parallel-hpc`: Parallel and HPC (MPI/OpenMP/GPU execution, scaling, and batch systems)
- `hoomd-blue-troubleshooting`: Troubleshooting (known issues, diagnostics, and debugging patterns)
- `hoomd-blue-getting-started`: Getting Started (initial setup, quickstarts, and core concepts)
- `hoomd-blue-api-and-scripting`: API and Scripting (language bindings, APIs, and programmatic interfaces)
- `hoomd-blue-developer-guide`: Developer Guide (developer architecture, extension points, and contribution workflow)

## Documentation-first inputs
- `sphinx-doc`

## Tutorials and examples roots
- `sphinx-doc/howto`
- `sphinx-doc/tutorial`

## Test roots for behavior checks
- `hoomd/pytest`
- `hoomd/test`
- `hoomd/hpmc/pytest`
- `hoomd/hpmc/test`
- `hoomd/md/pytest`
- `hoomd/md/test`
- `hoomd/mpcd/pytest`
- `hoomd/mpcd/test`

## Escalate only when needed
- Start from topic skill primary references.
- If those references are insufficient, search the selected topic skill doc map.
- If documentation still leaves ambiguity, open the selected topic skill source map and inspect the suggested source entry points.
- Use targeted symbol search while inspecting source (e.g., `rg -n "<symbol_or_keyword>" CMake hoomd`).

## Source directories for deeper inspection
- `CMake`
- `hoomd`
