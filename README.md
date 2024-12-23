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