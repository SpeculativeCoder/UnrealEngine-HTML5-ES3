# Unreal Engine 4.27 HTML5 ES3 (WebGL 2) / 4.24 HTML5 ES2 (WebGL 1)

<img src="Images/ThirdPerson.jpg" style="width:600px"/>

This is documentation for a fork of Unreal Engine 4 which builds upon the last version of the [community-supported HTML5 (WebGL) platform plugin](https://github.com/UnrealEngineHTML5/Documentation) to add:
- Support for **ES3 shaders** (WebGL 2).
- Support for the **latest/final version of UE4 (4.27)**.
- Support for a **recent version of [emscripten](https://emscripten.org/)** (will try to keep this up to date).
- A number of other features and improvements (see below). 

Also available is an **alternative branch with UE 4.24 using ES2 shaders (WebGL 1)** for those who wish to remain on that version.

_NOTE: To access the [fork](https://github.com/SpeculativeCoder/UnrealEngine) and the associated [Issues](https://github.com/SpeculativeCoder/UnrealEngine/issues?q=) and [Discussions](https://github.com/SpeculativeCoder/UnrealEngine/discussions?discussions_q=) sections you need your GitHub linked to your Epic Games account or you will see 404 errors._

Packaged HTML5 projects work best in Firefox or Chrome-based browsers on Windows 10/11. They also work for now in Firefox, Safari and Chrome-based browsers on MacOS. Other browsers/platforms may either not work or have graphical/performance issues. Mobile does not work (only checked iPhone, though).

Development/packaging of HTML5 projects (i.e. building and using this fork of Unreal Editor) is done on Windows 10 (but 11 should also be OK).

Live Example: [**AdhocCombat** (https://adhoccombat.com)](https://adhoccombat.com) - personal project, work in progress

### Other Features / Caveats

Other changes have also made to try and make a better out of the box experience, and there are also various issues/caveats to be aware of (some already existed, some are new in this fork):

- **Web socket networking plugin is enabled by default** which is needed for multiplayer in HTML5 should you wish to use it. You would still need to run separate server(s) (e.g. Windows / Linux servers) to connect to as web browser HTML5 clients cannot act as servers.
- Added [**optional, experimental, support for websocket SSL**](Features/Feature-WebSocketSSL.md), including the ability to connect to a hostname rather than just an IP address. This allows multiplayer to work when serving the HTML5 client via HTTPS.
- Added an [**optional way to pass command line options to the HTML5 application**](Features/Feature-CommandLine.md) e.g. to select different maps and/or modes etc.
- **Build compression of assets (to .gz files) is enabled by default**. Compressed (.gz suffix) assets need to be served using `Content-Type: gzip` HTTP header. If the header is missing, this fork will try to use the browser's built-in [DecompressionStream](https://developer.mozilla.org/en-US/docs/Web/API/DecompressionStream) to work around it.
- **All required scripts/assets (e.g. Bootstrap) are included in built project** (no more third party JS/font downloads).
- **Web browser IndexedDB usage is enabled by default** to prevent having to download all the assets on each page refresh. You can still turn this off if you prefer the user to download the data every time.
- **Uses single-threaded HTML5 mode by default** as it works better at the moment (tried this after seeing [@ufna](https://github.com/ufna/UE-HTML5)'s decision). You should make sure **Project Settings -> HTML5 -> Emscripten -> Multithreading support** is set to **False** in your projects. HTML5 multithreading is not fully supported and currently only works when packaging in Development mode (Test/Shipping renders a black screen).
- Only Development, Testing, and Shipping packaging is supported. Debug/DebugGame packaging is not supported.
- MacOS with dedicated graphics (e.g. Intel Macs with NVIDIA/AMD cards) may not automatically use dedicated graphics (reason unknown at the moment) which can result in much worse performance. Users with external displays plugged in will already be using dedicated graphics so should be fine. Other users can temporarily [disable Automatic graphics switching](https://support.apple.com/en-us/HT202043) to force use of dedicated graphics.
- Currently, Safari mouse sensitivity is dramatically different to Firefox/Chrome-based. This is likely due to how Safari reports mouse movementX - see [MDN](https://developer.mozilla.org/en-US/docs/Web/API/MouseEvent/movementX) and associated [bug](https://github.com/w3c/pointerlock/issues/42).
- Mobile MSAA is not supported. It should be disabled by default for new projects but if your packaged HTML5 project doesn't work and you see "Assertion failed" regarding multisampled textures in your browser console then set **Project Settings -> Rendering -> Mobile -> Mobile MSAA** to **No MSAA** and package your project again.
- Video playing, and likely anything related to Unreal [Media Framework](https://docs.unrealengine.com/4.27/en-US/WorkingWithMedia/IntegratingMedia/MediaFramework/) does not work.

For all features you may wish to use, a good rule of thumb is: anything that didn't work in ES2 likely won't work in the ES3 fork, except maybe some graphical features which specifically needed ES3. Anything that needs compute shaders won't work as that isn't supported in WebGL 1 or WebGL 2. See [this page](https://docs.unrealengine.com/4.27/en-US/RenderingAndGraphics/SupportedRenderingFeatures/) for an indication of what features may be supported (see the Android ES3.1 column which will be most useful indication as to what may work or not work in the ES3 fork branch).

See [TROUBLESHOOTING](TROUBLESHOOTING.md) for more detail on typical issues / troubleshooting / workarounds.

## Git Repository / Branches

**To access the links below you need to link your Epic Games account to GitHub - see your [Epic Games Account](https://www.epicgames.com/account/connected) - if you do not do this you will see 404 error.**

### 4.27 HTML5 ES3 (WebGL 2)

https://github.com/SpeculativeCoder/UnrealEngine/tree/4.27-html5-es3

This is **Unreal Engine 4.27.2** with HTML5 platform support using **ES3** shaders (WebGL 2) and **emscripten 3.1.50**

If you want to look at the code here is a [diff](https://github.com/EpicGames/UnrealEngine/compare/4.27.2-release...SpeculativeCoder:4.27-html5-es3) of this branch against the pristine UE 4.27.2 release (you can see the changes are all _new_ files in the Platforms/HTML5 folder).

An alternative way to view the code is a [diff](https://github.com/SpeculativeCoder/UnrealEngine/compare/4.27.2-release_with_4.24.3-html5-1.39.18_plugin..4.27-html5-es3) of this branch against UE 4.27.2 release 
with @nickshin's last community supported UE4.24 HTML5 plugin code as the starting point. This shows the actual differences in the plugin code which were needed to get 4.27.2 working rather than all new files.

### 4.24 HTML5 ES2 (WebGL 1)

https://github.com/SpeculativeCoder/UnrealEngine/tree/4.24-html5-es2

This is **Unreal Engine 4.24.3** with HTML5 platform support using **ES2** shaders (WebGL 1) and **emscripten 3.1.50**

This may be useful as a fallback if you still need to use UE 4.24 and/or ES2 but want the other changes above. If you want to look at the code see this see this [diff](https://github.com/UnrealEngineHTML5/UnrealEngine/compare/4.24.3-html5-1.39.18..SpeculativeCoder:4.24-html5-es2) against @nickshin's last community supported UE4.24 HTML5 plugin code.

## Requirements

- Windows 10 (11 should also work)
- Git for Windows
- Visual Studio 2019 or 2022 - install workloads "Game Development with C++" and ".NET desktop environment" with extra selection of ".NET Framework 4.6.2 development tools" - also ensure you select/use a Windows 10 SDK.
- CMake (make sure you select to add it to PATH during installation or manually after)
- Python 3.* (watch out for Windows Python app installer "app execution aliases" which may cause problems - recommend setting these to disabled - see [this Stack Overflow post](https://stackoverflow.com/a/61958044))

I have only built/tested on Windows 10 using the commands below. Other platforms may need further fixes/changes. See [TEST_REPORT](TEST_REPORT.md) for the last test run I have done including versions of the above requirements at the time of testing. 

## Installation

These are the steps I use/consult when installing/testing the plugin. These steps are basically the same as those in the [original documentation for the community-supported plugin](https://github.com/UnrealEngineHTML5/Documentation/blob/master/Platforms/HTML5/HowTo/README.0.building.UE4.Editor.md) which has even more detail so it can sometimes be useful to consult those too.

Using a Git for Windows BASH shell.

Clone the appropriate repository branch (you can also download the branch as a ZIP and unpack it if you prefer that way).

For the 4.27 ES3 (WebGL 2) branch you can do:

    `git clone -b 4.27-html5-es3 --single-branch https://github.com/SpeculativeCoder/UnrealEngine.git ue-4.27-html5-es3`

Or, alternatively, for the 4.24 ES2 (WebGL 1) branch you would instead do:

    `git clone -b 4.24-html5-es2 --single-branch https://github.com/SpeculativeCoder/UnrealEngine.git ue-4.24-html5-es2`

This should download the branch (it will take a while depending on your connection as the source is quite large).

Now, as per an [announcement from Epic Games](https://forums.unrealengine.com/t/upcoming-disruption-of-service-impacting-unreal-engine-users-on-github/1155880), you will need to replace the `Commit.gitdeps.xml` in the `Engine/Build` folder with a version newly provided by Epic depending on the engine version you are using. You can download the latest Commit.gitdeps.xml from the *Assets* section of the relevant Unreal Engine release below and replace the file in your `Engine/Build` folder with it:
- https://github.com/EpicGames/UnrealEngine/releases/tag/4.27.2-release if you are using 4.27 ES3 (WebGL 2) branch
- https://github.com/EpicGames/UnrealEngine/releases/tag/4.24.3-release if you are using 4.24 ES2 (WebGL 1) branch

Note: this was a recent breaking change so in future I hope to include this fix in the branches but want to do it in a clean way so for now we rely on the official fix/source.

Back in Git Bash, go into the folder:

    cd ue-4.27-html5-es3
    
Run:
    
    ./Setup.bat

This will download a lot of dependencies used by Unreal engine and perform some setup tasks. *If you see an error about remote server returned error or 403 / Forbidden then you may not have properly applied the Commit.gitdeps.xml fix above.* 

Now do:

    cd Engine/Platforms/HTML5
    ./HTML5Setup.sh

This patches the Unreal Engine source with a bunch of fixes, downloads emscripten SDK and builds the various support libraries (e.g. PhysX). It takes a while. At the end of this some notification sounds will be played to try and let you know it's finished and you should see the line `Success!` after a bunch of green messages. *If you do not see the 'Success!' line then something has gone wrong and any further steps will encounter problems so you should investigate/resolve the issue first. Issues with the HTML5Setup.sh step can also often leave Engine/Platforms/HTML/Build/emsdk in a broken state so deleting that directory is often a necessary part of trying again after you have chased down whatever the problem was.*

Now do:


    cd -
    ./GenerateProjectFiles.bat

Open ``UE4.sln`` in Visual Studio. You will probably see a popup asking if it is OK to upgrade the .NET programs from 4.5 to 4.8 (or similar). You can accept this in each case (I typically click the "do this for all" checkbox to get through this quicker).

You first need to add the HTML5LauncherHelper project to the solution... to do this you can Right Click **Programs** then **Add -> Existing Project** then navigate to and select this project to add to the solution: ``Engine\Platforms\HTML5\Source\Programs\HTML5\HTML5LaunchHelper\HTML5LauncherHelper.csproj``. You may see the .NET 4.5 to 4.8 (or similar) version upgrade again which you can accept.

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

Make a new (e.g. First Person, Third Person, or whatever you want) project. You should be able to build the project for HTML5 via **Package Project -> HTML5**. 

To see the build process / messages, you can show the Output Log via **Window -> Developer Tools -> Output Log**. This is useful for chasing down any problems. *If you see a packaging error about AutomationTool failing with messages about "invalid tokens" etc. [you may need to rebuild the .NET programs and try to package again.](https://github.com/SpeculativeCoder/UnrealEngine-HTML5-ES3/blob/main/TROUBLESHOOTING.md#when-packaging-html5-you-see-error-cs1519-invalid-token--in-class-struct-or-interface-member-declaration)*

Once built, go to to where the build was packaged and run ``HTML5LaunchHelper.exe``

Navigate to http://localhost:8000

Select the .html file. You should see the running game.

## Releasing your Project

**IMPORTANT: Everything is done at your own risk! Also see this documentation's [LICENSE](LICENSE) which includes a disclaimer.**

Here are some things to consider when releasing your project. However, it should not be considered a fully detailed or complete list as this is a huge topic. It is merely intended to be helpful and may be added to over time.

### Legal requirements

Consider the legal requirements of distributing Unreal Engine, content etc. (e.g. users will typically be downloading the packaged engine/project to their machine and running it). You should adhere to all legal requirements. Legal advice is not provided here - it is your responsibility. I am not a lawyer. This is not legal advice.

Some relevant links:
- https://www.unrealengine.com/en-US/release
- https://www.unrealengine.com/en-US/eula/unreal
- https://www.unrealengine.com/en-US/eula/content

### Crypto

Since all packaged data is given to the user in the download (i.e. both encrypted data _and_ the the code/information to decode it), **you must assume all encryption keys and packaged data can always be obtained/cracked by the user**. However, using this functionality could help to further signal the _intent_ on your part to associate the executable code and content more closely together. You would have to come up with your own approach if you wanted anything more than this.

**Project Settings -> Crypto**

Click **Generate Project Key** to generate a key (use different keys for each platform you package for as some platforms may be easier/harder to crack).

You can enable all four:
- **Encrypt PAK Ini Files** = true
- **Encrypt PAK Index** = true
- **Encrypt UAsset Files** = true
- **Encrypt All Asset Files** = true

### Ensure only needed maps are packaged

By default, Unreal includes all assets used by _any_ map in the project. So if you have enabled starter content (which is typical), or some Marketplace assets, these may have maps in them that are dragging in a lot of assets you didn't actually intend to use.

You should set the maps which will be included the packaging via this setting (make sure it has only the maps you make use of): **Project Settings -> Packaging -> List of maps to include in packaged build**

This is an advanced setting so you may need to click the down arrow to show it.

### Set appropriate texture sizes

See community plugin documentation entry: https://github.com/UnrealEngineHTML5/Documentation/blob/master/Platforms/HTML5/HowTo/README.2.advanced.UE4.HTML5.md#smash-texture-sizes

### IndexedDB on or off

**Project Settings -> Platforms -> HTML5 -> Emscripten -> IndexedDB storage**

IndexedDB support is enabled by default for new projects in this fork.

When IndexedDB support is enabled, the downloaded data files will be "cached" in the user's IndexedDB, which is a database managed by the user's browser. When the user visits your site again, the data will instead be obtained from their IndexedDB. This can be particularly useful for projects that the user will regularly visit/use.

However, if you feel your project will only be visited as a one-off, or you are happy for the download to be done every time, you can disable IndexedDB support and the user's browser will not be asked to cache the data.

### Asset compression on or off

There are two compression mechanisms in place right now:
- **PAK file compression** (**Project Settings -> Project -> Packaging -> Packaging -> Create compressed cooked packages**) (you can see this by clicking the down arrow as it is an advanced setting). **This should always be left enabled** and dramatically reduces the size of your data PAK (which will include all the cooked content for the project).
- **Asset compression** (**Project Settings -> Platforms -> HTML5 -> Packaging -> Compress files during packaging**). This helps reduce the size of _all_ the files in your packaged project (including compiled WASM and the CSS/JS). When this is enabled, many of the packaged files will now have a `.gz` extension (i.e. gzipped). This compression means that when the .gz files are served from your hosting environment you must ensure the HTTP header `Content-Type: gzip` is set so the user's browser knows to decompress them (HTML5LaunchHelper.exe already does this for you when testing locally). Each hosting environment may do this in a different way - check the relevant documentation for your web server / hosting environment. If the header is not set your project may not load correctly in the user's browser (this fork may make an attempt with DecompressionStream to work around this in Chrome-based and Safari browsers but you shouldn't rely on this). Thus, **asset compression should be enabled/disabled depending on your circumstances.**

Asset compression's most useful contribution is that it significantly reduces the size of the WASM file (this is the compiled engine / project code). However, it does not actually reduce the PAK file much further (which is _already_ itself compressed via PAK file compression). If you encounter any issues where the packaged project works locally but doesn't work in your hosting environment, you should try turning off asset compression, clear the packaged files folder and then package/upload your project again to see if the issue is resolved. This lets you know that the missing header is probably the problem.

### Customizing the controls / web page

HTML5 projects use a set of template files in `Engine/Platforms/HTML5/Build/TemplateFiles` to produce the packaged project HTML/JS/CSS etc. This is what defines the appearance of the buttons at the bottom of the screen, for instance. You may wish to customize these files, either by editing them in the engine folder (not ideal as it is easy to lose the changes when a new version of this fork is released etc.) or by creating a copy of them in your project's `Build/HTML5` folder which will then be picked up during the HTML5 packaging process.

## Fork Updates / Fresh Start

If a new version of the fork is released (i.e. a new commit to the branch), it is best to rebuild Unreal entirely to avoid any issues.

If you downloaded as ZIP, just delete the old extracted directory, download the branch again and start from scratch.

If you cloned the git repository / branch, go into the folder where you cloned the git repository e.g.

    cd ue-4.27-html5-es3
    
Then do:

     rm -fr Engine/Platforms/HTML5/Build/emsdk/emsdk-* && git clean -fdx && git -c core.hooksPath=/dev/null restore .

This will clear everything out to almost exactly as you originally downloaded (the hooksPath argument stops the Unreal git hooks from downloading any new files).

Now do

    git -c core.hooksPath=/dev/null pull
    
This will bring in the latest version of the branch.

Now follow the original [Installation](#Installation) guide starting with the fix for `Commit.gitdeps.xml` and the `./Setup.bat` step and so on.

## Issues / Discussions

If you need to raise any technical issues / discussions regarding this fork and the code changes you can use the [Issues](https://github.com/SpeculativeCoder/UnrealEngine/issues?q=) or [Discussions](https://github.com/SpeculativeCoder/UnrealEngine/discussions?discussions_q=) sections for the fork (you need your GitHub linked to your Epic Games account to see these or you will see 404 error).

Also there could be some general HTML5 plugin discussion in Unreal Slackers Discord https://unrealslackers.org/ in the **#web** channel (but note this channel is for general discussion of Unreal on the web, including the <a href="https://docs.unrealengine.com/5.1/en-US/pixel-streaming-in-unreal-engine/">Pixel Streaming</a> technology).

If interested in a more technical commentary of the development / code etc. (could be useful if you are also working on the HTML5 plugin) there are some notes in the wiki under [COMMENTARY](https://github.com/SpeculativeCoder/UnrealEngine/wiki/COMMENTARY) which I will aim to add to over time (you need your GitHub linked to your Epic Games account to see this or you will see 404 error).

## Documentation Copyright / License

Copyright (c) 2022-2023 SpeculativeCoder (https://github.com/SpeculativeCoder)

<a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by/4.0/">Creative Commons Attribution 4.0 International License</a>.

See [LICENSE](LICENSE) (CC-BY-4.0). The license only applies to the files in this repository.

**IMPORTANT: Any other repositories that are referred to / linked to are under their own copyright / license. For example https://github.com/SpeculativeCoder/UnrealEngine is under the Unreal Engine EULA etc. which you accept when you link your GitHub account to your Epic Games account.**
