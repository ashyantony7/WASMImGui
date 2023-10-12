# WASM Imgui Template project

This is a template project for building a webassembly application using imgui and emscripten.

## Setup

The following dependencies must be installed

1. CMake, Ninja and Conan:
   These dependencies are handled using pip in `requirements.txt`. To install them (preferably in a virtual environment) run:

```bash
pip install -r requirements.txt
```

2. OpenGL:

We use openGL for rendering. The following dependencies must be installed:

- Mac Os X

```bash
brew install glfw3
brew install glew
```
3. Emscripten:

Setup a conan profile for emscripten. Save the following file to `~/.conan/profiles/emscripten`:

- Mac Os:

```
include(default)

[settings]
os=Emscripten
arch=wasm
compiler=clang
compiler.version=16
compiler.libcxx=libc++
build_type=Release

[tool_requires]
emsdk/3.1.23
```

Build the application:

```bash
mkdir build && cd build
conan install ../conanfile.txt  --build missing --profile:build=default --profile:host=emscripten -s build_type=Release
source ./conanbuild.sh
cmake -G Ninja -DCMAKE_TOOLCHAIN_FILE=conan_toolchain.cmake -DCMAKE_BUILD_TYPE=Release ..
cmake --build . --config Release
```
