# UnrealEngine HTML5 (WebGL) ES3

<img src="Images/ThirdPerson.PNG" style="width:600px"/>

This is work which builds upon the [Epic Games HTML5 (WebGL) platform plugin](https://github.com/UnrealEngineHTML5/Documentation) to add:
- Support for the **latest/final version of UE4 (4.27)**.
- Support for a **recent version of emscripten** (will try to keep this up to date).
- Support for **ES3 shaders** (WebGL2).

Some other changes are also made to try and make a better out of the box experience:

- **Build compression of assets (to .gz files) is enabled by default**. You can still disable this if you prefer.
- **All required scripts/assets (e.g. Bootstrap) are included in built project** (no more third party JS/font downloads).
- **Web browser IndexedDB usage is enabled by default** to prevent having to download all the assets on each page refresh. You can still turn this off if you prefer it disabled. Any IndexedDB notices/warnings are now sent to console rather than a banner to avoid visual clutter.
- **Uses single-threaded by default** as it seems to work better at the moment (tried this after seeing @ufna's decision). Multi-threaded is still available but currently only works properly when packing for Development (will be looking into this issue).
- **Web socket networking plugin is enabled by default** should you wish to use multiplayer in HTML5.
- Added an [**optional way to pass command line options to the HTML5 application via session storage key**](Features/Feature-CommandLine.md). This feature is disabled by default.
- Added [**optional, experimental, support for websocket SSL and websocket URL override via session storage key**](Features/Feature-WebSocketSSL.md). This allows multiplayer to work when serving the HTML5 client via HTTPS. These features are disabled by default.

Tested on Windows 10 with latest Firefox and Chrome based browsers.

## Branches

**To access the links below you need to link your Epic Games account to GitHub - see your [Epic Games Account](https://www.epicgames.com/account/connected) - if you do not do this you will see 404 error.**

### 4.27 HTML5 ES3

https://github.com/SpeculativeCoder/UnrealEngine/tree/4.27-html5-es3

This is **UnrealEngine 4.27.2** with HTML5 platform support using **ES3** shaders and **emscripten 3.1.31**.

If you want to take a look at the full code here is a [diff](https://github.com/EpicGames/UnrealEngine/compare/4.27.2-release...SpeculativeCoder:4.27-html5-es3) of this branch against the pristine UE 4.27 release.

### 4.24 HTML5 ES2

https://github.com/SpeculativeCoder/UnrealEngine/tree/4.24-html5-es2

This is **UnrealEngine 4.24.3** with HTML5 platform support using **ES2** shaders and **emscripten 3.1.31** 

This may be useful as a fallback if you still need to use 4.24 or ES2 but want the other changes above - it also works as a reference of changes versus @nickshin's Epic Games HTML5 plugin development branch - see this [diff](https://github.com/UnrealEngineHTML5/UnrealEngine/compare/4.24.3-html5-1.39.18...SpeculativeCoder:4.24-html5-es2) for the comparison.

## Issues / Discussions

If you need to raise any technical issues / discussions regarding this fork and the code changes you can use [Issues](https://github.com/SpeculativeCoder/UnrealEngine/issues) / [Discussions](https://github.com/SpeculativeCoder/UnrealEngine/discussions).

If interested in a more in-depth discussion of the development / code etc. there are some notes in a [COMMENTARY](https://github.com/SpeculativeCoder/UnrealEngine/wiki/COMMENTARY) wiki page which I will aim to add to over time.

Also there could be discussion in Unreal Slackers Discord https://unrealslackers.org/ in the **#web** channel (but note this channel is also for the <a href="https://docs.unrealengine.com/5.1/en-US/pixel-streaming-in-unreal-engine/">Pixel Streaming</a> technology).

## Guide

### Requirements

- Git for Windows
- Visual Studio 2019 (install workloads "Game Development with C++" and ".NET desktop environment" with extra selection of ".NET Framework 4.6.2 development tools"). Visual Studio 2022 also works.
- CMake
- Python (3.*)

I have only built/tested on Windows 10 using the commands below. Other platforms may need further fixes/changes.

### Installation

Using a Git for Windows BASH shell.

Clone the repository branch (you can also download the branch as a ZIP and unpack it if you prefer that way):

    git clone -b 4.27-html5-es3 --single-branch https://github.com/SpeculativeCoder/UnrealEngine.git ue-4.27-html5-es3

Go into the folder:

    cd ue-4.27-html5-es3
    
Run:
    
    ./Setup.bat

This will download a lot of dependencies used by Unreal engine and perform some setup tasks. Then do:

    cd Engine/Platforms/HTML5
    ./HTML5Setup.sh

This patches the UnrealEngine source with a bunch of fixes, downloads emscripten SDK and builds the various support libraries (e.g. PhysX). It takes a while.

Now do:


    cd -
    ./GenerateProjectFiles.bat

Open ``UE4.sln`` in Visual Studio.

You first need to add the HTML5LauncherHelper project to the solution... to do this you can Right Click **Programs** then **Add -> Existing Project** then navigate to and select this project to add to the solution: ``Engine\Platforms\HTML5\Source\Programs\HTML5\HTML5LaunchHelper\HTML5LauncherHelper.csproj``

Now you can build all the programs. CTRL-Click the following projects to select them all at once:

- UE4
- AutomationTool
- AutomationToolLauncher
- HTML5LaunchHelper
- ShaderCompileWorker
- UnrealBuildTool
- UnrealFrontend
- UnrealHeaderTool
- UnrealLightmass
- UnrealPak

Now **Right Click -> Build Selection**

This will take a long time. UE4 will be built last and takes the longest.

### Usage

You can run the editor at ``Engine\Binaries\Win64\UE4Editor.exe``

First time though you will probably have to wait a while for shaders to compile.

Make a new (e.g. First Person, Third Person, or whatever you want) project. You should be able to build the project for HTML5 via **Package Project -> HTML5**

Once built, go to to where the build was packaged and run ``HTML5LaunchHelper.exe``

Navigate to http://localhost:8000

Select the .html file. You should see the running game.

## Troubleshooting

### When opening UE4.sln in Visual Studio you see "Target framework not supported" for each .NET program

For each of these you should be able to accept the default of "Update the target" to the newer version of .NET which seems to work fine.

### When building HTML5 you see: **error CS1519: Invalid token '(' in class, struct, or interface member declaration**

If you see this when trying to build HTML5 then in Visual Studio CTRL-Click these:
- AutomationTool
- AutomationToolLauncher
- HTML5LaunchHelper
- UnrealBuildTool

Then do **Right Click -> Rebuild Selection** to force rebuild the .NET programs. Seems to fix the issue.

### When running game you see in browser console an error with Assertion failure regarding multisampled textures

Try disabling Mobile MSAA (in Project Rendering options).

### When compiling, you see: "Detected compiler newer than Visual Studio 2019, please update min version checking"

This will happen when using Visual Studio 2022. It is just a warning and shouldn't cause any issues as VS2022 seems to be able to build the engine fine.

### When packaging HTML5 you see: "WARNING: Library XXX as not resolvable to a file when used in Module 'PhysX', assuming it is a filename and will search library paths for it. This is slow and dependency checking will not work for it. Please update reference to be fully qualified alternatively use PublicSystemLibraryPaths if you do intended to use this slow path to suppress this warning."

The XXX is usually PhysX. This seems to occur when HTML5Setup.sh randomly fails to properly build PhysX (or some other third party library) with an error like ```mingw32-make.exe[2]: write error: stdout``` but doesn't actually stop the ./HTML5Setup.sh build so it is easy to miss.

If you get this issue try running ./HTMLSetup.sh again.

NOTE: In the latest versions of the HTML5 branches the third party libraries are built using only a single thread (-j 1) to hopefully avoid this problem happening as much.

### When running HTML5Setup.sh you see: "fatal: not a git repository (or any of the parent directories): .git"

The HTML5Setup.sh script used to do a git checkout/restore to ensure the engine files were in a clean state before applying a patch, hence it didn't work if you downloaded a ZIP. However, I changed the HTML5Setup.sh to no longer require a git repository so if you have an older version of the code just do a new clone or ZIP download and you shouldn't run into this problem any more.

### When running HTML5Setup.sh you see: "zlib-1.2.8.tar.gz: Cannot open: No such file or directory"

This happens if you run HTML5Setup.sh without first having run the ./Setup.bat stage.
