# Unreal Engine 4.27 HTML5 ES3 (WebGL 2) & Unreal Engine 4.24 HTML5 ES2 (WebGL 1)

## Test Report

Most recent test run completed around **2025-06-08** for a release with emscripten **4.0.10**

## Test ES3 branch for https://github.com/SpeculativeCoder/UnrealEngine/tree/4.27.2-html5-es3-4.0.10

Versions:
- Windows **11**
- Git for Windows: **2.49.0**
- CMake: **4.0.2**
- Python: **3.13.4**
- Visual Studio **2022**: **17.14.4**
- Visual Studio toolchain: **14.44.35209**
- Windows SDK: **10.0.26100**

Steps:
- Built AdhocCombat (personal project) C++ project Development regularly and tested locally in Chromium.
- Built AdhocCombat (personal project) C++ project Shipping, deployed to https://adhoccombat.com, and ran in Chromium and/or Firefox. This ensures multiplayer works when the client is served over HTTPS.
- Loaded https://adhoccombat.com on iPhone (to trigger ASTC data variant usage). Used touch input to move view around. 
- Built FirstPerson Blueprint project in Development and ran in Chromium.
- Built FirstPerson Blueprint project in Shipping and ran in Chromium.
- Built FirstPerson Blueprint project in Development multithreaded and ran in Chromium.
- Tested multiplayer using enabled websocket plugin with at least one of the above.

## Test ES2 branch for https://github.com/SpeculativeCoder/UnrealEngine/tree/4.24.3-html5-es2-4.0.10

Versions:
- Windows **11**
- Git for Windows: **2.49.0**
- CMake: **4.0.2**
- Python: **3.13.4**
- Visual Studio **2022**: **17.14.4**
- Visual Studio toolchain: **14.44.35209**
- Windows SDK: **10.0.26100**

Steps:
- Built FirstPerson Blueprint project in Development and ran in Chromium.
- Built FirstPerson Blueprint project in Shipping and ran in Chromium.
- Built FirstPerson Blueprint project in Development multithreaded and ran in Chromium.
- Tested multiplayer using enabled websocket plugin with at least one of the above.
