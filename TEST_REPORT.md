# Unreal Engine 4.27 HTML5 ES3 (WebGL 2) / Unreal Engine 4.24 HTML5 ES2 (WebGL 1)

## Test Report

Most recent test run completed around **2023-02-09** for a release with emscripten **3.1.53**

## VS2022

- Windows 10
- Git for Windows: **2.43.0**
- CMake: **3.27.9**
- Python: **3.12.1**
- Visual Studio 2022: **17.8.6**
- Visual Studio toolchain: **14.38.33135**
- Windows 10 SDK: **10.0.20348**

### ES3 branch - tested with AdhocCombat (personal project)

- Build AdhocCombat Development regularly and tested locally in Chromium.
- Built AdhocCombat Shipping, deployed to https://adhoccombat.com, and ran in Chromium and/or Firefox.

### ES2 branch - tested with Unreal FirstPerson project

- Built FirstPerson project in Development and ran in Chromium and tested multiplayer. 
- Built FirstPerson project in Shipping and ran in Chromium.
- Built FirstPerson project in Development multithreaded and ran in Chromium.

## VS2019

- Windows 10
- Git for Windows: **2.43.0**
- CMake: **3.27.9**
- Python: **3.12.2**
- Visual Studio 2019: **16.11.33**
- Visual Studio toolchain: **14.29.30133**
- Windows 10 SDK: **10.0.19041**

### ES3 branch - tested with Unreal FirstPerson project

- Built FirstPerson project in Development and ran in Chromium and tested multiplayer. 
- Built FirstPerson project in Shipping and ran in Chromium.
- Built FirstPerson project in Development multithreaded and ran in Chromium.