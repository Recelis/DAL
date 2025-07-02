# OpenCL

[docs](https://github.com/KhronosGroup/OpenCL-Guide/tree/main?tab=readme-ov-file)

OpenCL (Open Computing Language) is a standard for cross-platform parallel programming (but does not run on MacOS).

## Installing on Linux

[docs](https://github.com/KhronosGroup/OpenCL-Guide/blob/main/chapters/getting_started_linux.md)

I'm running this on my old Ubuntu machine (we will see how it goes).

To do this, will need

- C/C++ Compiler (from `sudo apt install build-essential -y`)
- Git
- CMake (as a `build system`)
- OpenCL-SDK

### CMake Build System

This is a system that helps define your packages in low level languages like C.

It outputs a `make` file that you can then call to compile everything into an executable.
