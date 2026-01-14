# Cabal-Stack Rosetta Stone

A comprehensive guide for translating between Cabal and Stack commands and concepts.

## Table of Contents

- [Project Initialization](#project-initialization)
- [Building and Running](#building-and-running)
- [Dependency Management](#dependency-management)
- [Testing](#testing)
- [Cleaning and Maintenance](#cleaning-and-maintenance)
- [Common Command Options](#common-command-options)
- [Advanced Commands](#advanced-commands)

## Project Initialization

| Cabal | Stack |
|-------|-------|
| [`cabal init`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-init) | [`stack new`](https://docs.haskellstack.org/en/stable/commands/new_command/) |
| [`cabal update`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-update) | [`stack update`](https://docs.haskellstack.org/en/stable/commands/update_command/) |

**Explanation:**
- `cabal init` creates a new Cabal project with a `.cabal` file, while `stack new` creates a new Stack project with a complete project structure including `stack.yaml`, `package.yaml` (or `.cabal` file), and project templates.
- `cabal update` updates the package index from Hackage, while `stack update` updates Stack's package index. Stack typically manages its own snapshots and doesn't require frequent updates.

## Building and Running

| Cabal | Stack |
|-------|-------|
| [`cabal build`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-build) | [`stack build`](https://docs.haskellstack.org/en/stable/commands/build_command/) |
| [`cabal run <target>`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-run) | [`stack run <target>`](https://docs.haskellstack.org/en/stable/commands/run_command/) |
| [`cabal repl`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-repl) | [`stack repl`](https://docs.haskellstack.org/en/stable/commands/repl_command/) or [`stack ghci`](https://docs.haskellstack.org/en/stable/commands/ghci_command/) |
| [`cabal exec <command>`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-exec) | [`stack exec <command>`](https://docs.haskellstack.org/en/stable/commands/exec_command/) |
| [`cabal install`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-install) | [`stack install`](https://docs.haskellstack.org/en/stable/commands/install_command/) |

**Explanation:**
- Both `cabal build` and `stack build` compile your project, but Stack builds against a specific resolver/snapshot defined in `stack.yaml`, ensuring reproducible builds.
- `cabal run` and `stack run` execute a target executable from your project.
- `cabal repl` and `stack repl`/`stack ghci` start an interactive GHCi session with your project loaded.
- `cabal exec` and `stack exec` run commands in an environment with your project's dependencies available.
- `cabal install` builds and installs executables to `~/.cabal/bin`, while `stack install` copies executables to `~/.local/bin` (by default).

## Dependency Management

| Cabal | Stack |
|-------|-------|
| Edit `*.cabal` file's `build-depends` | Edit `package.yaml` or `*.cabal` file's `build-depends` |
| [`cabal build`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-build) (auto-installs dependencies) | [`stack build`](https://docs.haskellstack.org/en/stable/commands/build_command/) (auto-installs dependencies) |
| [`cabal freeze`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-freeze) | Use [resolver in `stack.yaml`](https://docs.haskellstack.org/en/stable/yaml_configuration/) |
| [`cabal outdated`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-outdated) | No direct equivalent |
| [`cabal.project`](https://cabal.readthedocs.io/en/stable/cabal-project.html) for multi-package projects | [`stack.yaml`](https://docs.haskellstack.org/en/stable/yaml_configuration/) for multi-package projects |

**Explanation:**
- Dependencies are specified in `.cabal` files (or `package.yaml` for Stack projects using Hpack). Both tools automatically fetch and install dependencies during build.
- `cabal freeze` creates a `cabal.project.freeze` file to lock dependency versions. Stack achieves this through its resolver system, which references curated package sets (Stackage snapshots).
- `cabal outdated` checks for newer versions of dependencies; Stack doesn't have a direct equivalent as it uses curated snapshots.
- Multi-package projects use `cabal.project` (Cabal) or `stack.yaml` (Stack) to define the workspace.

## Testing

| Cabal | Stack |
|-------|-------|
| [`cabal test`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-test) | [`stack test`](https://docs.haskellstack.org/en/stable/commands/test_command/) |
| [`cabal bench`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-bench) | [`stack bench`](https://docs.haskellstack.org/en/stable/commands/bench_command/) |
| [`cabal test --enable-coverage`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-test) | [`stack test --coverage`](https://docs.haskellstack.org/en/stable/coverage/) |

**Explanation:**
- Both `cabal test` and `stack test` run your test suites defined in the package configuration.
- Both `cabal bench` and `stack bench` run benchmark suites.
- Both tools support code coverage reporting with their respective flags.

## Cleaning and Maintenance

| Cabal | Stack |
|-------|-------|
| [`cabal clean`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-clean) | [`stack clean`](https://docs.haskellstack.org/en/stable/commands/clean_command/) |
| `rm -rf dist-newstyle/` | [`stack purge`](https://docs.haskellstack.org/en/stable/commands/purge_command/) |
| No direct equivalent | [`stack upgrade`](https://docs.haskellstack.org/en/stable/commands/upgrade_command/) |

**Explanation:**
- `cabal clean` removes build artifacts from the current project, while `stack clean` does the same for Stack projects.
- Removing `dist-newstyle/` completely cleans Cabal's build directory. `stack purge` removes all local work directories and Stack's snapshot packages.
- `stack upgrade` upgrades Stack itself; Cabal doesn't have a self-upgrade command (use your system's package manager or `ghcup`).

## Common Command Options

| Cabal | Stack |
|-------|-------|
| [`--program-suffix`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cmdoption-program-suffix) | No direct equivalent (manually rename after [`stack install`](https://docs.haskellstack.org/en/stable/commands/install_command/)) |
| [`--allow-newer`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cmdoption-allow-newer) | Use [`allow-newer: true`](https://docs.haskellstack.org/en/stable/yaml_configuration/) in `stack.yaml` |
| [`--test-options`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cmdoption-test-options) | [`--test-arguments`](https://docs.haskellstack.org/en/stable/commands/test_command/) or `--ta` |
| [`--ghc-options`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cmdoption-ghc-options) | [`--ghc-options`](https://docs.haskellstack.org/en/stable/yaml_configuration/) (same flag) |
| [`--disable-documentation`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cmdoption-disable-documentation) | No direct equivalent (use [`skip-ghc-docs: true`](https://docs.haskellstack.org/en/stable/yaml_configuration/) in `stack.yaml`) |
| [`--publish`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-upload) | No direct equivalent (part of [`stack upload`](https://docs.haskellstack.org/en/stable/commands/upload_command/)) |
| [`-O0`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cmdoption-O) (disable optimization) | [`--fast`](https://docs.haskellstack.org/en/stable/commands/build_command/) or [`--ghc-options="-O0"`](https://docs.haskellstack.org/en/stable/yaml_configuration/) |
| [`--enable-tests`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cmdoption-enable-tests) | Tests are enabled by default, use [`stack test`](https://docs.haskellstack.org/en/stable/commands/test_command/) to build and run |

**Explanation:**
- `--program-suffix` in Cabal adds a suffix to installed executables. Stack doesn't have a direct command-line equivalent; you can achieve similar results through configuration or manual renaming after `stack install`.
- `--allow-newer` relaxes upper bounds on dependencies. In Stack, you set `allow-newer: true` in `stack.yaml` to allow newer versions than specified in the Stackage snapshot.
- `--test-options` (Cabal) passes options to test executables; Stack uses `--test-arguments` (or the shorter `--ta`) for the same purpose.
- Both tools support `--ghc-options` to pass flags directly to GHC.
- Cabal's `--disable-documentation` skips building Haddock docs during installation. Stack has configuration options in `stack.yaml` but no direct command-line flag.
- `--publish` in Cabal controls whether a package upload is published immediately. Stack's `upload` command handles this differently.
- `-O0` disables optimizations in Cabal. Stack's `--fast` flag does the same, or you can pass `-O0` via `--ghc-options`.
- `--enable-tests` in Cabal ensures test suites are built. Stack builds tests when you run `stack test`, making this option implicit.

## Advanced Commands

| Cabal | Stack |
|-------|-------|
| [`cabal configure`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-configure) | Configuration in [`stack.yaml`](https://docs.haskellstack.org/en/stable/yaml_configuration/) |
| [`cabal haddock`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-haddock) | [`stack haddock`](https://docs.haskellstack.org/en/stable/commands/haddock_command/) |
| [`cabal sdist`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-sdist) | [`stack sdist`](https://docs.haskellstack.org/en/stable/commands/sdist_command/) |
| [`cabal upload`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-upload) | [`stack upload`](https://docs.haskellstack.org/en/stable/commands/upload_command/) |
| [`cabal check`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-check) | No direct equivalent (use `cabal check`) |
| [`cabal get <package>`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-get) | [`stack unpack <package>`](https://docs.haskellstack.org/en/stable/commands/unpack_command/) |
| [`cabal list <package>`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-list) | No direct equivalent |
| [`cabal info <package>`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-info) | No direct equivalent |
| [`cabal user-config`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-user-config) | Edit [`~/.stack/config.yaml`](https://docs.haskellstack.org/en/stable/yaml_configuration/) manually |
| [`cabal path`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-path) | [`stack path`](https://docs.haskellstack.org/en/stable/commands/path_command/) |
| [`cabal build --enable-profiling`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-build) | [`stack build --profile`](https://docs.haskellstack.org/en/stable/commands/build_command/) |

**Explanation:**
- `cabal configure` was primarily used in v1 commands; v2 commands (nix-style builds) configure automatically. Stack uses `stack.yaml` for configuration.
- Both tools support generating Haddock documentation, creating source distributions (`sdist`), and uploading packages to Hackage.
- `cabal check` validates your package description for common issues; Stack doesn't have an equivalent (use `cabal check` even in Stack projects).
- `cabal get` downloads and unpacks a package source; Stack uses `stack unpack` for the same purpose.
- `cabal list` and `cabal info` query package information from Hackage; Stack doesn't have direct equivalents.
- Both tools provide commands to query paths (`cabal path` and `stack path`).
- Both tools support profiling builds with their respective flags.

## Additional Resources

- [Cabal User Guide](https://cabal.readthedocs.io/)
- [Stack User Guide](https://docs.haskellstack.org/)
- [Differences between Cabal and Stack](https://docs.haskellstack.org/en/stable/stack_yaml_vs_cabal_package_file/)

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request if you notice any errors or would like to add more command comparisons.
