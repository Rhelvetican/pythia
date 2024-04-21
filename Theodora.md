# Theodora

- Pythia's ver own package manager.

## Manifest Keys

### Package Options

- `name`: Define a package's name.
- `ver`: Define a package's version.
    - Pythia follows [Semantic Versioning](https://semver.org/). Where:
        - Take `"X.Y.Z"` for example:
            - `X` is the *MAJOR* version. It should only be incremented when you are introducing breaking changes.
            - `Y` is the *MINOR* version. It should only be incremented when you are introducing new features in a backward compatible manner.
            - `Z` is the *PATCH* version. It should only be incremented when you are introducing backward compatible bugfixes.
- `authors`: Define a package's authors.
- `build`: Define a package's build script.
- `type`: Define a package's type. It could be either `"bin"` or `"lib"`.

### Dependencies

- Dependencies are loaded from the central Theodora registry by default. You can however use external registries as needed.
- Only the name and a version string are required in the case you are using the central Theodora registry.

    ```toml
    time = "1"          # Import the time package with version 1.X.X
    pyrite = "0.5"      # Import the pyrite package with version 0.5.X
    c-ffi = "1.0.66"    # Import the c-ffi package with version 1.0.66
    ```

- To specify more details about the dependency. You can defined the dependency as a table instead.
    - In such case, here are the list of possible manifest keys:
        - `ver`: Define the version of the package.
        - `url`: Define the URL of the registry to get the package.
        - `git`: Define the git repository of the package.
        - `feat`: Define a list of features to enable from the library.
        - `def-feat`: Specify whether to enable default features or not.

- You can also override dependencies with other copies of it in the `override` section.

### Profiles Options

These options control the LLVM flags.

- `opt-level` :
    - Possible values:
        - `0`: No optimizations are applied. Recommended for debugging.
        - `1`: Minimal optimizations are applied.
        - `2`: Basic optimizations are applied, default for release.
        - `3`: All optimizations are applied. Recommended for releasing.
        - `"s"`: Optimize for Size.
        - `"z"`: Optimize for Size, but also turn off loop vectorization.
- `debug`:
    - Possible values:
        - `0`, `false`, or `"none"`: no debug info at all, default for release.
        - `"line-directives-only"`: line info directives only. For the nvptx* targets this enables profiling. For other use cases, `line-tables-only` is the better, more compatible choice.
        - `"line-tables-only"`: line tables only. Generates the minimal amount of debug info for backtraces with filename/line number info, but not anything else, i.e. no variable or function parameter info.
        - `1` or `"limited"`: debug info without type or variable-level information. Generates more detailed module-level info than line-tables-only.
        - `2`, `true`, or `"full"`: full debug info, default for dev.
- `strip`:
    - Possible values:
        - `"none"`, `"debuginfo"`, and `"symbols"`.
- ETC.
