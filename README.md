<div align="center">
  <img src="assets/outno-icon.png" alt="OUTNO" width="180">
</div>

# OUTNO

### Build. Package. Install.

**A Rust-powered Windows application installer builder.**

Create self-contained Windows setup executables from your application
files, with a familiar installation wizard and an accompanying
uninstaller.

[![Rust](https://img.shields.io/badge/Built%20with-Rust-orange?logo=rust)](https://www.rust-lang.org/)
[![Platform](https://img.shields.io/badge/Platform-Windows-0078D4?logo=windows)](https://www.microsoft.com/windows)
[![Status](https://img.shields.io/badge/Status-Early%20development-orange)](#project-status)
[![License:
MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

**[Releases](https://github.com/floringhitun223/outno/releases) ·
[Report a bug](https://github.com/floringhitun223/outno/issues) ·
[Request a feature](https://github.com/floringhitun223/outno/issues)**
:::

------------------------------------------------------------------------

## Overview

OUTNO is a Windows installer creation tool written in Rust. It takes an
application directory and packages its files into a standalone setup
executable.

The generated installer contains the application payload and the
installer runtime in one executable. When launched, it presents an
installation wizard, extracts the application and creates the selected
shortcuts. An uninstaller is also installed alongside the application.

OUTNO is being developed as an independent, modern alternative to
traditional Windows installer-building workflows.

## Project status

OUTNO is in early development. The features below describe the
functionality currently documented in the project; the roadmap contains
ideas that are not yet implemented.

## Features

-   **Self-contained setup executable** --- the generated installer
    carries the application archive and its metadata.
-   **Graphical installer wizard** --- Welcome, License, Destination,
    Tasks, Ready, Installing and Finish stages.
-   **Per-user installation by default** --- installs under
    `%LOCALAPPDATA%\Programs` without requiring administrator
    privileges.
-   **Application shortcuts** --- creates Desktop and Start Menu
    shortcuts.
-   **Uninstaller generation** --- writes an uninstaller executable and
    its data file into the installation directory.
-   **Windows app registration** --- adds an Add/Remove Programs entry
    for the current user.
-   **Path traversal protection** --- blocks archive paths that attempt
    to escape the installation directory.
-   **Rust implementation** --- a native Windows application built with
    Rust.

## How it works

OUTNO currently uses three operating modes:

  -----------------------------------------------------------------------
  Mode                                Purpose
  ----------------------------------- -----------------------------------
  **Compiler**                        Select the application folder and
                                      configure the installer.

  **Installer**                       Run the generated setup executable
                                      and install the packaged
                                      application.

  **Uninstaller**                     Confirm and remove an installed
                                      application.
  -----------------------------------------------------------------------

The generated setup file uses an appended-payload format:

``` text
[installer executable]
[application ZIP archive]
[manifest.json]
[ZIP length]
[manifest length]
["RSINST01"]
```

At startup, the executable checks for the payload trailer. If it is
present, it starts the installation wizard and extracts the application
archive.

## Getting started

### Requirements

-   Windows
-   [Rust](https://www.rust-lang.org/tools/install) and Cargo
-   The MSVC build tools for the `x86_64-pc-windows-msvc` target

### Build from source

If you are building on Windows and the MSVC target is not installed, add
it first:

``` powershell
rustup target add x86_64-pc-windows-msvc
```

Build the project in release mode:

``` powershell
cargo build --release
```

Run the resulting executable from the project directory:

``` powershell
.\target\release\rusty-setup.exe
```

> **Note:** The current binary name may still be `rusty-setup.exe`; it
> can be renamed as the Cargo package and binary configuration are
> updated.

## Creating an installer

1.  Launch the OUTNO compiler.
2.  Browse to the **Application folder** containing the files you want
    to distribute.
3.  Review the detected application name and main executable.
4.  Optionally select a `.txt` license file.
5.  Configure the shortcut options and output folder.
6.  Select **Build installer**.
7.  Find the generated setup executable in the selected output
    directory.

The output filename follows this pattern:

``` text
<App>-setup-<version>.exe
```

## Installation behavior

The currently documented installer behavior is:

-   Installs per-user by default to `%LOCALAPPDATA%\Programs\<App>`.
-   Creates Desktop and Start Menu `.lnk` shortcuts.
-   Writes `unins000.exe` and `unins000.dat` into the installation
    directory.
-   Creates an Add/Remove Programs entry under `HKCU`.
-   Blocks `../` path traversal during archive extraction.

An all-users installation mode and additional installer customization
are planned, not currently documented as implemented.

## Roadmap

Potential next steps for OUTNO include:

-   [ ] Embed an application icon and Windows UAC manifest.
-   [ ] Add an all-users installation option targeting `Program Files`.
-   [ ] Add installer language packs.
-   [ ] Support custom wizard images.
-   [ ] Render rich-text (`.rtf`) license files.
-   [ ] Save and load compiler projects.
-   [ ] Introduce a script-based project format.
-   [ ] Support code-signing workflows for generated installers.

The roadmap is subject to change as development progresses.

## Contributing

Contributions, bug reports and feature suggestions are welcome.

Before opening a pull request:

1.  Check existing issues to avoid duplicate reports.
2.  Describe the problem or proposed change clearly.
3.  Include reproduction steps for bug fixes where possible.
4.  Keep changes focused and document user-facing behavior.

Use the repository's [Issues
page](https://github.com/floringhitun223/outno/issues) for bug reports
and feature requests.

## Security

Installer software handles executable files, archives and filesystem
operations. Please report suspected security vulnerabilities privately
to the project maintainer rather than publishing exploit details in a
public issue.

Do not use OUTNO to package software you do not have permission to
distribute.

## License

OUTNO is licensed under the [MIT License](LICENSE).

------------------------------------------------------------------------


**OUTNO** · Build. Package. Install.
:::
