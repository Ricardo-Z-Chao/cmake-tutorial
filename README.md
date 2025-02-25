# Overview

Based on CMake official [tutorial](https://cmake.org/cmake/help/v3.30/guide/tutorial/index.html) that uses Newton's method for finding roots.

# Formatter

If you want to format the `CMakeLists.txt` file, you can follow these steps:

- First, use pip to install `cmake_format`.

  ```shell
  python -m pip install cmake_format
  ```

- Then install the "cmake-format" extension from VS Code.

# Configuring the preset

You can generate the `CMakePresets.json` file using the CMake extension in VSCode and add a "configurePreset". For example, configure a file in Windows:

```json
{
  "version": 8,
  "configurePresets": [
    {
      "name": "default",
      "displayName": "Default Configure",
      "generator": "MinGW Makefiles",
      "binaryDir": "${sourceDir}/out/build/${presetName}",
      "cacheVariables": {
        "CMAKE_BUILD_TYPE": "Debug",
        "CMAKE_INSTALL_PREFIX": "${sourceDir}/out/install/${presetName}"
      }
    }
  ]
}
```

The `CMakeUserPresets.json` file can specify custom local build details, sush as complier paths or build tools. This file should not  be checked into VCS. For example:

```json
{
  "version": 8,
  "configurePresets": [
    {
      "name": "windows-only",
      "inherits": "default",
      "cacheVariables": {
        "CMAKE_C_COMPILER": "D:/msys64/ucrt64/bin/gcc.exe",
        "CMAKE_CXX_COMPILER": "D:/msys64/ucrt64/bin/g++.exe",
        "CMAKE_MAKE_PROGRAM": "D:/msys64/ucrt64/bin/mingw32-make.exe"
      }
    }
  ]
}
```

> The `CMakeUserPresets.json` file can implicitly includs the `CMakePresets.json` file, even with no `include` field. You can specify the `inherits` field to inherit all fileds from anothor preset.In this example, the "windows-only" preset inherits from the "default" preset defined in `CMakePresets.json`.

Add "buildPresets" preset in the `CMakeUserPresets.json` file:

```json
"buildPresets": [
    {
    	"name": "default",
    	"configurePreset": "windows-only"
    }
]
```

# Build project

Excute the following command in the command line:

1. Generate project Buildsystem.

   ```shell
   cmake --preset=windows-only
   ```

2. Build project.

   ```shell
   cmake --build --preset=default
   ```

# Install

Excute the command `cmake --install <dir>`, where `<dir>` corresponds to the `binaryDir` field configured in the "configurePresets" preset. The CMake variable `CMAKE_INSTALL_PREFIX` is used to determine the root of where the files will be installed. In this case, you can run the command as follows:

```shell
cmake --install out/build/windows-only
```

Afterwards, you can check the directory specified by the `CMAKE_INSTALL_PREFIX` variable to verify the installation location.

# Test

Add "testPresets" preset in the `CMakeUserPresets.json` file:

```json
"testPresets": [
    {
        "name": "default",
        "configurePreset": "windows-only"
    }
]
```

Excute the following command in the command line:

```shell
ctest --preset=default
```

If you want to display our CTest results with CDash, navigate to the build directory and run:

```shell
ctest -D Experimental
```

# Package

To build a binary distribution, from the binary directory run:

```shell
cpack
```

To create an archive of the full source tree you would type:

```shell
cpack --config CPackSourceConfig.cmake
```
