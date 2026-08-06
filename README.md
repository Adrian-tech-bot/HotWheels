# Hot_Wheels_RC

Welcome to the **Hot Wheels** project for Curtin Robotics club (CRoC)

This repository contains Arduino, ESP32, C++ codebases, hardware design, mechanical/electrical schematics.

---

## Project Structure

```
Hot_Wheels_RC/
├── archive/                    -> archival assests, may be still relevant
├── docs/                       -> Documentation for the project
├── hardware_design/            -> CAD and schematic files for the hardware components
│   ├── Hot_Wheels_3d_model/    -> 3D model exported files (e.g., .stl, .step) for manufacturing and prototyping
│   └── Hot_Wheels_3d_model.f3d -> Fusion 360 CAD file (requires Fusion 360 to open)
├── Hot_Wheels_arduino_firmware/ -> Arduino firmware source code
```

## Collaboration Guidelines

To ensure smooth teamwork:

- **Commit, pull, and push frequently** to keep everyone on the same page.
- **Create a branch** for any experimental or testing-specific changes. Once stable, we can discuss merging them into the main codebase.
- If you have questions, feel free to reach out to Aron or Joshan


---

Contributors:  
- **Shea, Aron**

---

## Nix Flake Quickstart (recommended)

This repo ships a `flake.nix` that pins the full Arduino toolchain (via Nix) and pins the *two ESP32 package indexes* needed for this project:

- Espressif Arduino-ESP32 index
- Bluepad32 Arduino-ESP32 fork index

That means a new contributor can build on macOS/Linux without installing Arduino IDE, platform packages, or toolchains manually.

### 1) Enter the dev shell

```sh
nix develop -c $SHELL
```

### 2) Confirm the ESP32 + Bluepad32 core is present

```sh
arduino-cli core list
```

You should see `esp32-bluepad32:esp32` installed.

### 3) Build the firmware

The target board for this project is **ESP32 Dev Module** from the Bluepad32 core:

- FQBN: `esp32-bluepad32:esp32:esp32`

Compile:

```sh
arduino-cli compile \
	--fqbn esp32-bluepad32:esp32:esp32 \
	Hot_Wheels_arduino_firmware/Hot_Wheels_arduino_firmware.ino
```

### Development checks

The flake provides formatting, linting, and firmware compilation checks:

```sh
# Format Nix files
nix fmt

# Check formatting, Nix linting, and firmware compilation
nix flake check

# Run an individual command
nix run .#lint
nix run .#firmware
```

The development shell also installs a pre-commit hook that runs `nix flake check`.

### Notes on Bluepad32 library

The sketch includes `#include <Bluepad32.h>`. In this setup, the header is provided by the **esp32-bluepad32 platform package** (it bundles the library), so `arduino-cli lib list` may still show “No libraries installed” even though the include resolves.
