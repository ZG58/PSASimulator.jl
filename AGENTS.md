# Repository Guidelines

## Project Structure & Module Organization

This is a Julia package for pressure swing adsorption simulation. Package metadata and dependencies live in `Project.toml`. Source code is under `src/`, with `src/PSASimulator.jl` defining the root module and including the implementation modules:

- `PSAInput.jl` and `PSAUtils.jl` for inputs, constants, and shared utilities.
- `StepModels.jl` plus `Func*.jl` files for the PSA process step right-hand sides.
- `PSACycle.jl` for the high-level `psacycle` driver and cycle convergence logic.

Examples and validation scripts are in `demo/`. Static assets, currently the logo, are in `assets/`. There is no `test/` directory yet.

## Build, Test, and Development Commands

- `julia --project=. -e 'using Pkg; Pkg.instantiate()'`: install package dependencies for the local environment.
- `julia --project=. -e 'using PSASimulator'`: smoke-test that the package loads.
- `julia --project=. demo/demo_psa_simulator.jl`: run the demonstration scenarios against literature data.
- `julia --project=. -e 'using Pkg; Pkg.test()'`: run the package test suite once `test/runtests.jl` exists.

The demo script activates the parent project itself, but running it with `--project=.` keeps the environment explicit.

## Coding Style & Naming Conventions

Use idiomatic Julia with 4-space indentation. Keep modules and exported public APIs in PascalCase or lowercase according to existing usage, such as `PSACycleDriver`, `StepModels`, `psacycle`, and `step_rhs`. Prefer descriptive names for physical variables and preserve established scientific notation where it maps directly to the PSA model, for example `P_0`, `T_0`, `q_s0`, and `t_ads`.

Add docstrings for public functions and concise comments only where the numerical method or model mapping is not obvious. Keep generated outputs, logs, notebooks, and `Manifest.toml` out of commits per `.gitignore`.

## Testing Guidelines

When adding tests, use Julia's standard `Test` framework in `test/runtests.jl`. Place deterministic unit tests near utility behavior and smaller PSA step interfaces before adding long-running integration cases. Name test sets by behavior, for example `@testset "Isotherm loading" begin`.

For simulation changes, include a package load check and either a focused unit test or a documented demo comparison.

## Commit & Pull Request Guidelines

The current history uses short, direct commit messages such as `Update logo` and `first commit`; continue with concise imperative or descriptive subjects under about 72 characters.

Pull requests should explain the modeling or API change, list commands run, and mention any numerical or performance impact. Link related issues when available. Include screenshots only for asset or documentation rendering changes.
