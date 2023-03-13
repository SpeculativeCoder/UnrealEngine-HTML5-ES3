# UnrealEngine HTML5 ES3 (WebGL2)

<img src="Images/ThirdPerson.jpg" style="width:600px"/>

This is work which builds upon the [UnrealEngine community-supported HTML5 (WebGL) platform plugin](https://github.com/UnrealEngineHTML5/Documentation) to add:
- Support for the **latest/final version of UE4 (4.27)**.
- Support for a **recent version of emscripten** (will try to keep this up to date).
- Support for **ES3 shaders** (WebGL2).

Supported/tested on Windows 10 with latest Firefox and Chrome based browsers. Other platforms/browsers may either not work or have performance/graphical issues.

Some other changes were also made to try and make a better out of the box experience:

- **Build compression of assets (to .gz files) is enabled by default**. You can still disable this if you prefer.
- **All required scripts/assets (e.g. Bootstrap) are included in built project** (no more third party JS/font downloads).
- **Web browser IndexedDB usage is enabled by default** to prevent having to download all the assets on each page refresh. You can still turn this off if you prefer the user to download the data every time.
- **Uses single-threaded by default** as it seems to work better at the moment (tried this after seeing [@ufna](https://github.com/ufna/UE-HTML5)'s decision). Multi-threaded is still available but currently only works properly when packing for Development (will be looking into this issue).
- **Web socket networking plugin is enabled by default** should you wish to use multiplayer in HTML5.
- Added [**optional experimental support for websocket SSL**](Features/Feature-WebSocketSSL.md). This allows multiplayer to work when serving the HTML5 client via HTTPS. This feature is disabled by default.
- Added an [**optional way to pass command line options to the HTML5 application**](Features/Feature-CommandLine.md) e.g. to select different maps etc. This feature is disabled by default.

Live Example: [**AdhocCombat** (https://adhoccombat.com)](https://adhoccombat.com) - work in progress

## Git Repository / Branches

**To access the links below you need to link your Epic Games account to GitHub - see your [Epic Games Account](https://www.epicgames.com/account/connected) - if you do not do this you will see 404 error.**

### 4.27 HTML5 ES3

https://github.com/SpeculativeCoder/UnrealEngine/tree/4.27-html5-es3

This is **UnrealEngine 4.27.2** with HTML5 platform support using **ES3** shaders and **emscripten 3.1.32**.

If you want to take a look at the full code here is a [diff](https://github.com/EpicGames/UnrealEngine/compare/4.27.2-release...SpeculativeCoder:4.27-html5-es3) of this branch against the pristine UE 4.27.2 release (you can see the changes are all _new_ files in the Platforms/HTML5 folder). An alternative way to view the changes is to take UE 4.27.2 _along with_ @nickshin's last community supported UE4.24 HTML5 plugin code and compare this against the ES3 branch above - see this [diff](https://github.com/SpeculativeCoder/UnrealEngine/compare/4.27.2-release_with_4.24.3-html5-1.39.18_plugin..4.27-html5-es3). This shows the actual differences in the plugin code which were needed to get 4.27.2 working (along with the other changes) rather than all new files.

### 4.24 HTML5 ES2

https://github.com/SpeculativeCoder/UnrealEngine/tree/4.24-html5-es2

This is **UnrealEngine 4.24.3** with HTML5 platform support using **ES2** shaders and **emscripten 3.1.32** 

This may be useful as a fallback if you still need to use 4.24 or ES2 but want the other changes above - it also works as a reference of changes versus @nickshin's UE4.24 HTML5 plugin branch - see this [diff](https://github.com/UnrealEngineHTML5/UnrealEngine/compare/4.24.3-html5-1.39.18..SpeculativeCoder:4.24-html5-es2) for the comparison.


## Requirements

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

This will take a long time. UE4 will be built last and takes the longest.

## Usage

You can run the editor at ``Engine\Binaries\Win64\UE4Editor.exe``

First time though you will probably have to wait a while for shaders to compile.

Make a new (e.g. First Person, Third Person, or whatever you want) project. You should be able to build the project for HTML5 via **Package Project -> HTML5**

Once built, go to to where the build was packaged and run ``HTML5LaunchHelper.exe``

Navigate to http://localhost:8000

Select the .html file. You should see the running game.

## Troubleshooting

### When opening UE4.sln in Visual Studio you see "Target framework not supported" for each .NET program

For each of these you should be able to accept the default of "Update the target" to the newer version of .NET which seems to work fine.

### When packaging HTML5 you see: **error CS1519: Invalid token '(' in class, struct, or interface member declaration**

If you see this when trying to package for HTML5 then in Visual Studio CTRL-Click these:
- AutomationTool
- AutomationToolLauncher
- HTML5LaunchHelper
- UnrealBuildTool

Then do **Right Click -> Rebuild Selection** to force rebuild the .NET programs. Seems to fix the issue.

### When running game you see in browser console an error with: "Assertion failed" regarding multisampled textures

Try disabling Mobile MSAA (in Project Rendering options).

### When running game you see in browser console an error with: "Assertion failed" relating to a compression method and "file needs to be forced to use zlib compression"

This can happen if you have PAK file compression format set to **Oodle**. This HTML5 plugin requires you to use **Zlib** compression. You can change this as follows:

Project Settings -> Packaging -> Pak File Compression Format(s): Change this from **Oodle** to **Zlib** 

NOTE: This is an advanced setting so you need to click the down arrow to unhide the setting.

Once you have done this you can package the project again as HTML5 and see if the issue is fixed. You may need to to clear the IndexedDB and/or browser cache so you get the newly generated PAK file.

### When compiling, you see: "Detected compiler newer than Visual Studio XXX, please update min version checking"

XXX is just whatever version of Visual Studio the branch was last successfully tested with. This can appear if you are using an even more recent version of Visual Studio (i.e. 2022 latest versions). It is just a warning and shouldn't cause any issues as VS2022 seems to be able to build the engine fine.

### When packaging HTML5 you see: "WARNING: Library XXX as not resolvable to a file when used in Module 'XXX'"

The XXX is usually PhysX. This seems to occur when HTML5Setup.sh randomly fails to properly build PhysX (or some other third party library) with an error like ```mingw32-make.exe[2]: write error: stdout``` but doesn't actually stop the ./HTML5Setup.sh build so it is easy to miss and you will only notice when then trying to package for HTML5.

If you get this issue try running ./HTMLSetup.sh again.

NOTE: In the latest versions of the above branches the third party libraries are built using only a single thread (-j 1) to hopefully avoid this problem happening as much.

### When running HTML5Setup.sh you see: "fatal: not a git repository (or any of the parent directories): .git"

The HTML5Setup.sh script in the above branches used to do a git checkout/restore to ensure the engine files were in a clean state before applying a patch, hence it didn't work if you downloaded a ZIP. However, I changed the HTML5Setup.sh to no longer require a git repository so if you have an older version of the code just do a new clone or ZIP download and you shouldn't run into this problem any more.

### When running HTML5Setup.sh you see: "zlib-1.2.8.tar.gz: Cannot open: No such file or directory"

This happens if you run HTML5Setup.sh without first having run the ./Setup.bat stage. Running ./Setup.bat to ensure all the dependencies Unreal needs are downloaded (including this zlib tar) should avoid this error.

### When running HMTL5Setup.sh you see: "Python" (and/or things simply stop after seeing "Resolving deltas: 100% (XXX/XXX), done." and nothing more happens)

Even if you have Python properly installed, Windows may have some [App installer python.exe and python3.exe](https://stackoverflow.com/questions/57485491/python-python3-executes-in-command-prompt-but-does-not-run-correctly) that could possibly be interfering with Python usage.

### When running the game you only see a blank white page with buttons at the top and/or in the console log you see: "Uncaught SyntaxError: illegal character U+001F" and/or you see error/warning message: "Downloaded a compressed file XXX without the necessary HTTP response header "Content-Encoding: gzip""

If you are using compressed packaging for HTML5 (which is the default), you need to ensure the files ending in `.gz' (i.e. the compressed files) are served with the following HTTP header:

    Content-Encoding: gzip
    
This signals to the web browser that it needs to decompress the file before trying to use it.

The way the header can be set depends on how you are hosting the files. It will differ per solution - check the documentation for your web hosting solution. 

However, if you need to temporarily work around this, you can disable compressed packaging:

Project Settings -> HTML5 -> Packaging -> Compress files during packaging: Set to **false**

Most of the increase in size due to turning off this compression will be in the size of the Unreal binary code which will no longer be compressed. The PAK data file actually has its own compression (which should be turned on by default) handled by Unreal so depending on how big your project the increase may or may not be a significant versus the overall size. 

Also some web hosting solutions may _automatically_ do all the gzip compression of .css / .js assets on the fly (and set appropriate headers too) so in those cases you may be better off not using compressed packaging.

### Why is the download so big? Can I reduce the size of the generated application?

By default, Unreal includes all assets used by _any_ map in the project. So if you have enabled starter content, or some Marketplace assets, these may have maps in them that are dragging in a lot of assets you didn't actually intend to use.

You should set the maps which will be included the packaging via this setting (make sure it has only the maps you make use of):

Project Settings -> Packaging -> List of maps to include in packaged build

This is an advanced setting so you may need to click the down arrow to show it.

## Issues / Discussions

If you need to raise any technical issues / discussions regarding this fork and the code changes you can use [Issues](https://github.com/SpeculativeCoder/UnrealEngine/issues) / [Discussions](https://github.com/SpeculativeCoder/UnrealEngine/discussions) (as above, you need your GitHub linked to your Epic Games account to use these).

If interested in a more in-depth discussion of the development / code etc. there are some notes in a [COMMENTARY](https://github.com/SpeculativeCoder/UnrealEngine/wiki/COMMENTARY) wiki page which I will aim to add to over time.

Also there could be some general HTML5 plugin discussion in Unreal Slackers Discord https://unrealslackers.org/ in the **#web** channel (but note this channel is for general discussion of Unreal on the web, including the <a href="https://docs.unrealengine.com/5.1/en-US/pixel-streaming-in-unreal-engine/">Pixel Streaming</a> technology).

## Copyright / License

Copyright (c) 2022-2023 SpeculativeCoder (https://github.com/SpeculativeCoder)

<a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by/4.0/">Creative Commons Attribution 4.0 International License</a>.

See [LICENSE](LICENSE) (CC-BY-4.0). The license only applies to the files in this repository.

**IMPORTANT: Any other repositories that are referred to / linked to are under their own copyright / license. 
For example https://github.com/SpeculativeCoder/UnrealEngine is under the Unreal Engine EULA etc. which you accept when you link your GitHub account to your Epic Games account.**