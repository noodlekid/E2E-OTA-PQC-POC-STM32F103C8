# For Developers

## Quick Start (for the experienced)

1.  Ensure all **[Prerequisites](#️-prerequisites)** are installed and in your system's PATH.
2.  Clone the repository: `git clone https://github.com/noodlekid/E2E-OTA-PQC-POC-STM32F103C8.git`
3.  Open the project folder in VS Code. When prompted, install the recommended extensions.
4.  VS Code will automatically run the CMake configuration.
5.  Press **`Ctrl+Shift+B`** to build the project.
6.  Press **`F5`** to flash and start a debug session.

-----

## Project Architecture

The project structure is designed to enforce a clean separation between application logic, vendor libraries, and system configuration.

  - **/src**: Your application source code (`.c` files).
  - **/inc**: Your application header files (`.h` files).
  - **/lib**: Third-party libraries.
  - **/Drivers**: STM32 HAL/LL drivers, managed by STM32CubeMX. **Do not modify.**
  - **/Core**: STM32CubeMX generated system files (`syscalls.c`, etc.). Treat as read-only.
  - **PQC-POC-STM32-F103C8.ioc**: The STM32CubeMX configuration file. This is the source of truth for hardware configuration.

-----

## Prerequisites

The following tools are required to build and debug this project. Please ensure they are installed and that the command-line executables are available in your system's PATH.

  - **Visual Studio Code**: The code editor.
  - **ARM GCC Toolchain**: The cross-compiler for ARM targets.
  - **CMake**: The build system generator.
  - **Ninja**: A fast, modern build tool used by CMake.
  - **OpenOCD**: The on-chip debugger server that communicates with the ST-LINK.
  - **STM32CubeMX**: The graphical tool for configuring the microcontroller peripherals.
  - **Clang Toolchain**: Provides `clang-format` and `clang-tidy` for code styling and linting.
  - **Python**: Required for the `pre-commit` hook manager.

-----

## Environment Setup

Follow the instructions for your operating system.

### <img src="https://img.icons8.com/color/24/000000/linux--v1.png"/> Linux (Debian/Ubuntu) & WSL2

```bash
# Install core build tools, git, and python
sudo apt update
sudo apt install -y build-essential git python3 python3-pip

# Install ARM toolchain, OpenOCD, and CMake/Ninja
sudo apt install -y gcc-arm-none-eabi openocd cmake ninja-build

# Install Clang tools for linting/formatting
sudo apt install -y clang clang-format clang-tidy clangd

# Install pre-commit for git hooks
pip install pre-commit
```

### <img src="https://img.icons8.com/ios-filled/24/000000/mac-os.png"/> macOS (using Homebrew)

```bash
# Install Homebrew if you haven't already
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install core tools
brew install git python cmake ninja

# Install ARM toolchain and OpenOCD
brew tap ArmMbed/homebrew-formulae
brew install arm-none-eabi-gcc openocd

# Install Clang tools (often included with Xcode Command Line Tools, but this ensures they are up to date)
brew install llvm

# Install pre-commit
pip3 install pre-commit
```

### <img src="https://img.icons8.com/?id=uIXgLv5iSlLJ&format=png&color=000000"/> Arch Linux

```bash
# Install core build tools, git, and python
sudo pacman -Syu --needed base-devel git python python-pip

# Install ARM toolchain, OpenOCD, and CMake/Ninja
sudo pacman -Syu --needed arm-none-eabi-gcc openocd cmake ninja

# Install Clang tools for linting/formatting
sudo pacman -Syu --needed clang

# Install pre-commit for git hooks
sudo pacman -Syu python-pre-commit
```

### <img src="https://img.icons8.com/color/24/000000/windows-10.png"/> Windows

On Windows, it's highly recommended to use a package manager like **Chocolatey** or **Scoop** to simplify installation and PATH management.

```powershell
# Using Chocolatey
choco install -y git python cmake ninja

# Install ARM toolchain and OpenOCD
# It is recommended to install the "xPack" versions for Windows
# See: https://xpack.github.io/dev-tools/

# Install Clang/LLVM
choco install -y llvm

# Install pre-commit
pip install pre-commit
```

-----

## Development Workflow

### 1\. First-Time Setup

1.  **Clone** the repository.
2.  **Open the project folder** in Visual Studio Code.
3.  A notification will appear prompting you to install the **recommended workspace extensions**. Click **Install**.
4.  VS Code and the CMake Tools extension will automatically configure the project.
5.  Run `pre-commit install` in the terminal to set up the Git hooks.

### 2\. Building the Code

  - Press **`Ctrl+Shift+B`** (or `Cmd+Shift+B` on Mac).
  - *or* Open the Command Palette (`Ctrl+Shift+P`) and run **`Tasks: Run Build Task`**.

### 3\. Flashing & Debugging

  - Connect your ST-LINK programmer to the target board and your computer.
  - Press **`F5`** to automatically build, flash, and start a debug session.
  - Execution will be paused at the beginning of the `main()` function.
  - You can now use breakpoints, step through code, and inspect variables and peripheral registers.

### 4\. Modifying Hardware Configuration

All hardware configuration (pinouts, clock speeds, peripheral setup) is managed by STM32CubeMX.

1.  Open the **`PQC-POC-STM32-F103C8.ioc`** file with the standalone STM32CubeMX application.
2.  Make your desired changes in the graphical interface.
3.  Click **"Generate Code"** in CubeMX. This will update the HAL drivers and necessary system files.
4.  Your custom application code in `/src` and `/inc` will be untouched.
5.  Rebuild the project (`Ctrl+Shift+B`) to apply the changes.

-----

## Code Style & Linting

This project enforces a consistent code style and performs static analysis to catch common bugs before they are committed.

  - **Formatter**: `clang-format` automatically formats your code on save according to the rules in `.clang-format`.
  - **Linter**: `clang-tidy` performs static analysis based on the rules in `.clang-tidy`.
  - **Automation**: **Pre-commit hooks** are configured in `.pre-commit-config.yaml`. Before any commit is finalized, these hooks will automatically format your changed files and run the linter. **A commit will fail if the linter finds any errors.**

This automated workflow ensures that all code in the repository remains clean, readable, and robust.
