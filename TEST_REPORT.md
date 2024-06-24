# Unreal Engine 4.27 HTML5 ES3 (WebGL 2) / Unreal Engine 4.24 HTML5 ES2 (WebGL 1)

## Test Report

Most recent test run completed around **2024-06-22** for a release with emscripten **3.1.61**

## VS2022

- Windows 10
- Git for Windows: **2.45.2**
- CMake: **3.29.6**
- Python: **3.12.4**
- Visual Studio 2022: **17.10.3**
- Visual Studio toolchain: **14.40.33811**
- Windows 10 SDK: **10.0.20348**

### ES3 branch - tested with AdhocCombat (personal project)

- Build AdhocCombat C++ project Development regularly and tested locally in Chromium.
- Built AdhocCombat C++ project Shipping, deployed to https://adhoccombat.com, and ran in Chromium and/or Firefox.

### ES2 branch - tested with Unreal FirstPerson project

- Built FirstPerson Blueprint project in Development and ran in Chromium.
- Built FirstPerson Blueprint project in Shipping and ran in Chromium.
- Built FirstPerson Blueprint project in Development multithreaded and ran in Chromium.
- Tested multiplayer using enabled websocket plugin with at least one of the above.

## VS2019

- Windows 10
- Git for Windows: **2.45.2**
- CMake: **3.29.6**
- Python: **3.12.4**
- Visual Studio 2019: **16.11.37**
- Visual Studio toolchain: **14.29.30154**
- Windows 10 SDK: **10.0.20348**

### ES3 branch - tested with Unreal FirstPerson project

- Built FirstPerson Blueprint project in Development and ran in Chromium.
- Built FirstPerson Blueprint project in Shipping and ran in Chromium.
- Built FirstPerson Blueprint project in Development multithreaded and ran in Chromium.
- Tested multiplayer using enabled websocket plugin with at least one of the above.