# Cabal-Stack Rosetta Stone

A comprehensive guide for translating between Cabal and Stack commands and concepts.

## Table of Contents

- [Project Initialization](#project-initialization)
- [Building and Running](#building-and-running)
- [Dependency Management](#dependency-management)
- [Testing](#testing)
- [Cleaning and Maintenance](#cleaning-and-maintenance)
- [Advanced Commands](#advanced-commands)

## Project Initialization

| Cabal | Stack |
|-------|-------|
| [`cabal init`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-init) | [`stack new`](https://docs.haskellstack.org/en/stable/GUIDE/#start-your-new-project) |
| [`cabal update`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-update) | [`stack update`](https://docs.haskellstack.org/en/stable/GUIDE/#updating-your-package-index) |

**Explanation:**
- `cabal init` creates a new Cabal project with a `.cabal` file, while `stack new` creates a new Stack project with a complete project structure including `stack.yaml`, `package.yaml` (or `.cabal` file), and project templates.
- `cabal update` updates the package index from Hackage, while `stack update` updates Stack's package index. Stack typically manages its own snapshots and doesn't require frequent updates.

## Building and Running

| Cabal | Stack |
|-------|-------|
| [`cabal build`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-build) | [`stack build`](https://docs.haskellstack.org/en/stable/build_command/) |
| [`cabal run <target>`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-run) | [`stack run <target>`](https://docs.haskellstack.org/en/stable/build_command/#target-syntax) |
| [`cabal repl`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-repl) | [`stack repl`](https://docs.haskellstack.org/en/stable/GUIDE/#the-repl) or [`stack ghci`](https://docs.haskellstack.org/en/stable/GUIDE/#the-repl) |
| [`cabal exec <command>`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-exec) | [`stack exec <command>`](https://docs.haskellstack.org/en/stable/GUIDE/#exec) |
| [`cabal install`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-install) | [`stack install`](https://docs.haskellstack.org/en/stable/GUIDE/#the-stack-install-command-and-copy-bins-option) |

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
| [`cabal build`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-build) (auto-installs dependencies) | [`stack build`](https://docs.haskellstack.org/en/stable/build_command/) (auto-installs dependencies) |
| [`cabal freeze`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-freeze) | Use [resolver in `stack.yaml`](https://docs.haskellstack.org/en/stable/yaml_configuration/#resolver) |
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
| [`cabal test`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-test) | [`stack test`](https://docs.haskellstack.org/en/stable/GUIDE/#test) |
| [`cabal bench`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-bench) | [`stack bench`](https://docs.haskellstack.org/en/stable/GUIDE/#benchmark) |
| [`cabal test --enable-coverage`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-test) | [`stack test --coverage`](https://docs.haskellstack.org/en/stable/coverage/) |

**Explanation:**
- Both `cabal test` and `stack test` run your test suites defined in the package configuration.
- Both `cabal bench` and `stack bench` run benchmark suites.
- Both tools support code coverage reporting with their respective flags.

## Cleaning and Maintenance

| Cabal | Stack |
|-------|-------|
| [`cabal clean`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-clean) | [`stack clean`](https://docs.haskellstack.org/en/stable/GUIDE/#cleaning) |
| `rm -rf dist-newstyle/` | [`stack purge`](https://docs.haskellstack.org/en/stable/GUIDE/#stack-purge) |
| No direct equivalent | [`stack upgrade`](https://docs.haskellstack.org/en/stable/GUIDE/#upgrading-stack) |

**Explanation:**
- `cabal clean` removes build artifacts from the current project, while `stack clean` does the same for Stack projects.
- Removing `dist-newstyle/` completely cleans Cabal's build directory. `stack purge` removes all local work directories and Stack's snapshot packages.
- `stack upgrade` upgrades Stack itself; Cabal doesn't have a self-upgrade command (use your system's package manager or `ghcup`).

## Advanced Commands

| Cabal | Stack |
|-------|-------|
| [`cabal configure`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-configure) | Configuration in [`stack.yaml`](https://docs.haskellstack.org/en/stable/yaml_configuration/) |
| [`cabal haddock`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-haddock) | [`stack haddock`](https://docs.haskellstack.org/en/stable/GUIDE/#haddock) |
| [`cabal sdist`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-sdist) | [`stack sdist`](https://docs.haskellstack.org/en/stable/GUIDE/#sdist) |
| [`cabal upload`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-upload) | [`stack upload`](https://docs.haskellstack.org/en/stable/GUIDE/#upload) |
| [`cabal check`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-check) | No direct equivalent (use `cabal check`) |
| [`cabal get <package>`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-get) | [`stack unpack <package>`](https://docs.haskellstack.org/en/stable/GUIDE/#unpack) |
| [`cabal list <package>`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-list) | No direct equivalent |
| [`cabal info <package>`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-info) | No direct equivalent |
| [`cabal user-config`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-user-config) | Edit [`~/.stack/config.yaml`](https://docs.haskellstack.org/en/stable/yaml_configuration/#non-project-specific-config) manually |
| [`cabal path`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-path) | [`stack path`](https://docs.haskellstack.org/en/stable/GUIDE/#path) |
| [`cabal v2-build --enable-profiling`](https://cabal.readthedocs.io/en/stable/cabal-commands.html#cabal-build) | [`stack build --profile`](https://docs.haskellstack.org/en/stable/GUIDE/#debugging) |

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
