<!-- TODO: Replace or remove this logo/link. -->
[![Project logo](logo.png)](https://example.com)

# [YOUR INSTALLER NAME]

<!-- TODO: Describe your installer and what it installs. -->
[One-line description of your installer and supported software.]

This project is built from **Sourcegen**, a customizable installer template forked from Nitrogen. Its download sources and package catalog are defined by the developer, not by Nitrogen.

## Installation

<!-- TODO: Replace with the actual distribution name and invocation for your installer. -->
> `pip install your-package-name`

## Dependencies

- Python 3.12+
- `pipx` (required for the `install` and `uninstall` commands)

<!-- TODO: Replace `your-installer` in all examples with your CLI command, e.g. `python -m your_package`. -->
<!-- TODO: Replace `example-package` and version numbers with packages supported by your installer. -->

# Commands

## General

Commands for downloading, removing, installing, and inspecting packages provided by your configured source. Package names and releases are determined by your implementation. Optional aliases can be defined in `PUBLICATION_CACHE`.

### `get <package> [release]`

Download a package into the installer's internal cache. The download URL and archive layout are controlled by the template's `resolve_download_url()` and `resolve_extracted_source()` hooks, or by the optional `custom_install_package()` hook. When omitted, the release defaults to `latest`.

> `your-installer get example-package 1.2.0`

### `rm <package> [release]`

Remove one cached release, or all cached releases of a package if no release is specified. Use `rm all` to clear the package cache.

> `your-installer rm example-package 1.2.0`

### `install <package> [release]`

Install an already cached package as a command-line application through `pipx`. This is separate from `get`, which only caches the package. The installed package must provide a usable Python entry point; optional `.sourcepkg` metadata can describe it.

> `your-installer install example-package 1.2.0`

### `uninstall <package> [release]`

Uninstall a previously installed application through `pipx`. The argument is resolved to the application's configured command/package name; removing the cached files is handled separately by `rm`.

> `your-installer uninstall example-package`

### `list`

List packages in the installer's internal cache.

> `your-installer list`

### `cache`

Show total cache size and cached package storage usage.

> `your-installer cache`

## Documentation

### `readme`

Display the README bundled with the installer.

> `your-installer readme`

### `license`

Display the license bundled with the installer.

> `your-installer license`

### `help`

Show the complete command reference.

> `your-installer help`

# Python API

<!-- TODO: Replace `your_package` with the actual importable Python module. -->

The installer also exposes a Python API. Its module name depends on how you package your Sourcegen-based project.

### `your_package.require(pub: str, rel: str | None = None) -> object`

Load a package from the internal cache, downloading it through the configured install flow if needed. Use dotted names to select submodules, such as `example_package.utils`. An omitted release uses `latest`.

> `Tools = require("example_package.utils", "1.2.0")`

### `your_package.require_async(pub: str, rel: str | None = None) -> object`

Asynchronous version of `require()` for applications already using `asyncio`.

> `Tools = await require_async("example_package.utils", "1.2.0")`

### `your_package.SourcegenDependencyError`

Intended error for failures to load an uncached package when its remote source is unavailable. **TODO:** Ensure this exception is defined in your Sourcegen implementation before using it; the current template references it but does not define it.

### `your_package.cleanup() -> None`

Remove the installer's internal temporary files and cached packages. Subsequent `require()` calls may need to download them again.

> `cleanup()`

### `your_package.entrypoint() -> None`

Start the installer's command-line interface.

> `entrypoint()`

# Developer Configuration

Sourcegen does not ship a predefined package catalog or a working download URL. **Complete the template's `# TODO` sections before releasing your installer.**

| Setting or hook | Purpose |
| --- | --- |
| `NAME`, `DESCRIPTION`, `VERSION`, `COMMAND` | Set branding, release information, and CLI usage. |
| `PUBLICATION_CACHE` | Add optional short aliases for package names. |
| `resolve_download_url(package, release)` | Return a direct ZIP archive download URL. |
| `resolve_extracted_source(extract_root, package, release)` | Choose which extracted directory becomes the cached package. |
| `custom_install_package(package, release, install_root, work_dir)` | Optionally replace the default ZIP download workflow altogether. |
| `INSTALLER_PACKAGE_DEPENDENCY` | Optionally add your installer package as a dependency of installed applications. |
| `_publication_leaf(package, release)` | Customize the cached folder naming convention if necessary. |

**Do not modify `SOURCEGEN_VERSION`** just to version your project: use `VERSION` for your installer's release number.

## Optional `.sourcepkg` metadata

The pipx installation path can read a `.sourcepkg` JSON file from a cached package directory. Metadata may specify a command name, entry script, version, description, Python requirement, and dependencies. The template can also infer some metadata if the file is absent.

<!-- TODO: Adapt metadata and packaging conventions to your actual supported applications. -->

---

<!-- TODO: Replace with your own project/license information. -->
[YOUR INSTALLER NAME] is based on Sourcegen, itself forked from Nitrogen. Review the upstream license and attribution requirements before publishing your fork.
