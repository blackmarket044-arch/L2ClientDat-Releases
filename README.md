# L2ClientDat Editor - Lunara Edition

L2ClientDat is a tool designed for opening and saving Lineage 2 .dat files.<br>
This software is intended to assist developers and players in managing, modifying,<br>
and understanding the data used by Lineage 2.


## Features

- Open Lineage 2 .dat files.
- Save modifications back to .dat format.


## Recommended software

- Liberica JDK 24
- Eclipse IDE for Java Developers

## Portable Windows build

Run `build-portable.ps1` from PowerShell on a machine with JDK 24 on `PATH`.
The script creates a self-contained Windows application image and a ZIP archive
under `..\build\portable`. The target computer does not need Java installed;
distribute the ZIP and run `L2ClientDat.exe` from the extracted folder.

The EXE metadata identifies the publisher as `L2JLunara`. On startup, the
application displays `images\splash.png` while it initializes and closes the
splash after the main window is ready. The splash background is transparent and
the displayed image is scaled proportionally to a maximum width of 520 pixels.

The EXE uses `dist\images\l2jlunara_128x128.png` as its icon by default.
When a PNG is used, the script generates an ICO with 16, 24, 32, 48, 64,
128, and 256 pixel variants for Windows Explorer's different view modes.
To use another square PNG (up to 256 x 256) or an ICO file, pass its path:

```powershell
powershell -ExecutionPolicy Bypass -File .\build-portable.ps1 -SourceIcon "D:\path\to\my-icon.png"
```

## Updates and releases

The portable application checks the public GitHub Releases page for
`blackmarket044-arch/L2ClientDat-Releases` when it starts. Keep the source
repository private and use this separate public repository only for release
assets. Do not commit application source code to the public releases repository;
GitHub's automatically generated source archives include files committed there.
If a newer stable release is available, the user can install it or be reminded
on the next run.
The updater verifies the ZIP's SHA-256 digest from the GitHub Releases API,
checks the archive contents, preserves the user's window and debug settings,
and restarts the application after installation. The updater is only enabled
for the portable application image.

The first release using the separate updater repository is version 1.0.1.
Existing 1.0.0 installations still check the old repository, so users on 1.0.0
must manually install 1.0.1 once. After that, their application will check the
public releases repository for later updates.

To prepare a release, build the portable archive with its matching version:

```powershell
powershell -ExecutionPolicy Bypass -File .\build-portable.ps1 -Version 1.0.1
```

The build creates `L2ClientDat-portable.zip` under `..\build\portable`. Create
a GitHub Release tagged `v1.0.1` in the public releases repository and upload
that ZIP using the exact asset name
`L2ClientDat-portable.zip`. Use a higher `MAJOR.MINOR.PATCH` version for each
subsequent release. GitHub must finish generating the asset's SHA-256 digest
before clients can install it.

The build downloads the ProGuard 7.10.0 distribution from the official
Guardsquare release and verifies its SHA-256 digest before use. It obfuscates
private implementation symbols while preserving application class names and
public/protected APIs needed by dynamically compiled format plugins. The
mapping file is written beside the build output as
`L2ClientDat-mapping-<version>.txt`; keep it private and do not upload it with
the release.

Obfuscation makes code analysis harder but cannot prevent decompilation or
modification of a distributed Java application. Only distribute releases
through the configured public releases repository; the updater trusts that
repository's release assets.
