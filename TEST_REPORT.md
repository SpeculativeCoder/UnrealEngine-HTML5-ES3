# Unreal Engine 4.27 HTML5 ES3 (WebGL 2) & Unreal Engine 4.24 HTML5 ES2 (WebGL 1)

## Test Report

Most recent test run completed around **2025-02-16** for a release with emscripten **4.0.3**

## Test ES3 branch, Windows 10, VS2022 -- AdhocCombat (personal project)

Versions:
- Windows **10**
- Git for Windows: **2.47.1**
- CMake: **3.31.5**
- Python: **3.13.1**
- Visual Studio **2022**: **17.13.0**
- Visual Studio toolchain: **14.43.34808**
- Windows SDK: **10.0.26100**

Steps:
- Built AdhocCombat C++ project Development regularly and tested locally in Chromium.
- Built AdhocCombat C++ project Shipping, deployed to https://adhoccombat.com, and ran in Chromium and/or Firefox. This ensures multiplayer works when the client is served over HTTPS.
- Loaded https://adhoccombat.com on iPhone (to trigger ASTC data variant usage). Used touch input to move view around. 

## Test ES2 branch, Windows 10, VS2022 -- Unreal FirstPerson project

Versions:
- Windows **10**
- Git for Windows: **2.47.1**
- CMake: **3.31.5**
- Python: **3.13.1**
- Visual Studio **2022**: **17.13.0**
- Visual Studio toolchain: **14.43.34808**
- Windows SDK: **10.0.26100**

Steps:
- Built FirstPerson Blueprint project in Development and ran in Chromium.
- Built FirstPerson Blueprint project in Shipping and ran in Chromium.
- Built FirstPerson Blueprint project in Development multithreaded and ran in Chromium.
- Tested multiplayer using enabled websocket plugin with at least one of the above.

## Test ES3 branch, Windows 10, VS2019 -- Unreal FirstPerson project

Versions:
- Windows **10**
- Git for Windows: **2.48.1**
- CMake: **3.31.5**
- Python: **3.13.1**
- Visual Studio **2019**: **16.11.44**
- Visual Studio toolchain: **14.29.30158**
- Windows 11 SDK: **10.0.22000**

Steps:
- Built FirstPerson Blueprint project in Development and ran in Chromium.
- Built FirstPerson Blueprint project in Shipping and ran in Chromium.
- Built FirstPerson Blueprint project in Development multithreaded and ran in Chromium.
- Tested multiplayer using enabled websocket plugin with at least one of the above.