# UnrealEngine HTML5 ES3 (WebGL2)

<img src="Images/ThirdPerson.jpg" style="width:600px"/>

This is documentation for a fork of UnrealEngine 4 which builds upon the [community-supported HTML5 (WebGL) platform plugin](https://github.com/UnrealEngineHTML5/Documentation) to add:
- Support for **ES3 shaders** (WebGL2).
- Support for the **latest/final version of UE4 (4.27)**.
- Support for a **recent version of emscripten** (will try to keep this up to date).

Packaged HTML5 projects should work in **Firefox** or **Chrome-based** browsers on **Windows 10/11** or **MacOS**. Safari on MacOS also appears to work for now but there may be subtle issues or future problems. Other browsers/platforms may either not work or have graphical/performance issues. Mobile does not work.

Development/packaging of HTML5 projects (i.e. building and using this fork of Unreal Editor) is only tested on Windows 10 (but 11 should also work). 

Live Example: [**AdhocCombat** (https://adhoccombat.com)](https://adhoccombat.com) - personal project, work in progress

### Other Features

Some other changes have also made to try and make a better out of the box experience:

- **Build compression of assets (to .gz files) is enabled by default**. You can still disable this if you prefer. Compressed (.gz suffix) assets need to be served using `Content-Type: gzip`. If you are unable to set this header in your hosting environment, a fallback decompression attempt will now be made (using [DecompressionStream](https://developer.mozilla.org/en-US/docs/Web/API/DecompressionStream)). This fallback is currently only supported/tested in Chrome-based browsers.
- **All required scripts/assets (e.g. Bootstrap) are included in built project** (no more third party JS/font downloads).
- **Web browser IndexedDB usage is enabled by default** to prevent having to download all the assets on each page refresh. You can still turn this off if you prefer the user to download the data every time.
- **Uses single-threaded HTML5 mode by default** as it seems to work better at the moment (tried this after seeing [@ufna](https://github.com/ufna/UE-HTML5)'s decision). You can still package a HTML5 build with multithreading support but it has issues (see Caveats section below).
- **Web socket networking plugin is enabled by default** should you wish to use multiplayer in HTML5.
- Added [**optional experimental support for websocket SSL**](Features/Feature-WebSocketSSL.md), including the ability to connect to a hostname rather than just an IP address. This allows multiplayer to work when serving the HTML5 client via HTTPS. This feature is disabled by default.
- Added an [**optional way to pass command line options to the HTML5 application**](Features/Feature-CommandLine.md) e.g. to select different maps etc. This feature is disabled by default.

Also available is an alternative branch with UE 4.24 using ES2 shaders (WebGL1) - but with the above new features - for those who wish to remain on that version.

### Caveats / Known Issues

- Only Development, Testing, and Shipping packaging is supported. Debug/DebugGame packaging is not supported.
- In the ES3 branch, stationary directional light does not properly cast dynamic shadows from moveable meshes. For now you could either work without these shadows or switch to moveable directional light (which does cast shadows but with worse quality).
- In both ES2 and ES3 branches, HTML5 multithreading only works for Development builds and may have subtle issues. Test/Shipping has graphical corruption / black screen. Single-threaded should be used to avoid this issue (make sure **Project Settings -> HTML5 -> Emscripten -> Multithreading support** is set to **False**).

## Git Repository / Branches

**To access the links below you need to link your Epic Games account to GitHub - see your [Epic Games Account](https://www.epicgames.com/account/connected) - if you do not do this you will see 404 error.**

### 4.27 HTML5 ES3

https://github.com/SpeculativeCoder/UnrealEngine/tree/4.27-html5-es3

This is **UnrealEngine 4.27.2** with HTML5 platform support using **ES3** shaders (WebGL2) and **emscripten 3.1.35**

If you want to look at the code here is a [diff](https://github.com/EpicGames/UnrealEngine/compare/4.27.2-release...SpeculativeCoder:4.27-html5-es3) of this branch against the pristine UE 4.27.2 release (you can see the changes are all _new_ files in the Platforms/HTML5 folder).

An alternative way to view the code is a [diff](https://github.com/SpeculativeCoder/UnrealEngine/compare/4.27.2-release_with_4.24.3-html5-1.39.18_plugin..4.27-html5-es3) of this branch against UE 4.27.2 release 
with @nickshin's last community supported UE4.24 HTML5 plugin code as the starting point. This shows the actual differences in the plugin code which were needed to get 4.27.2 working rather than all new files.

### 4.24 HTML5 ES2

https://github.com/SpeculativeCoder/UnrealEngine/tree/4.24-html5-es2

This is **UnrealEngine 4.24.3** with HTML5 platform support using **ES2** shaders (WebGL1) and **emscripten 3.1.35**

This may be useful as a fallback if you still need to use UE 4.24 and/or ES2 but want the other changes above. If you want to look at the code see this see this [diff](https://github.com/UnrealEngineHTML5/UnrealEngine/compare/4.24.3-html5-1.39.18..SpeculativeCoder:4.24-html5-es2) against @nickshin's last community supported UE4.24 HTML5 plugin code.

## Requirements

- Windows 10 (11 should also work)
- Git for Windows
- Visual Studio 2019 or 2022 (install workloads "Game Development with C++" and ".NET desktop environment" with extra selection of ".NET Framework 4.6.2 development tools" - also ensure you select/use a Windows 10 SDK).
- CMake
- Python (3.*)

I have only built/tested on Windows 10 using the commands below. Other platforms may need further fixes/changes.

## Installation

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

This will take a long time. UE4 will be built last and takes the longest. If you see any failures at the end, try again at least once in case there was any ordering issue etc.

## Usage

You can run the editor at ``Engine\Binaries\Win64\UE4Editor.exe``

First time though you will probably have to wait a while for shaders to compile.

Make a new (e.g. First Person, Third Person, or whatever you want) project. You should be able to build the project for HTML5 via **Package Project -> HTML5**

Once built, go to to where the build was packaged and run ``HTML5LaunchHelper.exe``

Navigate to http://localhost:8000

Select the .html file. You should see the running game.

## Troubleshooting

See [TROUBLESHOOTING](TROUBLESHOOTING.md) for typical troubleshooting / workarounds.

## Issues / Discussions

If you need to raise any technical issues / discussions regarding this fork and the code changes you can use the [Issues](https://github.com/SpeculativeCoder/UnrealEngine/issues) or [Discussions](https://github.com/SpeculativeCoder/UnrealEngine/discussions) sections for the fork (you need your GitHub linked to your Epic Games account to see these or you will see 404 error).

If interested in a more technical commentary of the development / code etc. there are some notes in the wiki under [COMMENTARY](https://github.com/SpeculativeCoder/UnrealEngine/wiki/COMMENTARY) which I will aim to add to over time (you need your GitHub linked to your Epic Games account to see this or you will see 404 error).

Also there could be some general HTML5 plugin discussion in Unreal Slackers Discord https://unrealslackers.org/ in the **#web** channel (but note this channel is for general discussion of Unreal on the web, including the <a href="https://docs.unrealengine.com/5.1/en-US/pixel-streaming-in-unreal-engine/">Pixel Streaming</a> technology).

## Documentation Copyright / License

Copyright (c) 2022-2023 SpeculativeCoder (https://github.com/SpeculativeCoder)

<a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by/4.0/">Creative Commons Attribution 4.0 International License</a>.

See [LICENSE](LICENSE) (CC-BY-4.0). The license only applies to the files in this repository.

**IMPORTANT: Any other repositories that are referred to / linked to are under their own copyright / license. For example https://github.com/SpeculativeCoder/UnrealEngine is under the Unreal Engine EULA etc. which you accept when you link your GitHub account to your Epic Games account.**