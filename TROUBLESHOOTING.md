# UnrealEngine HTML5 ES3 (WebGL2)

## Troubleshooting

### When running HTML5Setup.sh you see: "fatal: not a git repository (or any of the parent directories): .git"

The HTML5Setup.sh script in the above branches used to do a git checkout/restore to ensure the engine files were in a clean state before applying a patch, hence it didn't work if you downloaded a ZIP. However, I changed the HTML5Setup.sh to no longer require a git repository so if you have an older version of the code just do a new clone or ZIP download and you shouldn't run into this problem any more.

### When running HTML5Setup.sh you see: "zlib-1.2.8.tar.gz: Cannot open: No such file or directory"

This happens if you run HTML5Setup.sh without first having run the ./Setup.bat stage. Running ./Setup.bat to ensure all the dependencies Unreal needs are downloaded (including this zlib tar) should avoid this error.

### When running HMTL5Setup.sh you see: "Python" (and/or things simply stop after seeing "Resolving deltas: 100% (XXX/XXX), done." and nothing more happens)

Even if you have Python properly installed, Windows may have some [App installer python.exe and python3.exe](https://stackoverflow.com/questions/57485491/python-python3-executes-in-command-prompt-but-does-not-run-correctly) that could possibly be interfering with Python usage.

### When opening UE4.sln in Visual Studio you see "Target framework not supported" for each .NET program

For each of these you should be able to accept the default of "Update the target" to the newer version of .NET which seems to work fine.

### When compiling, you see: "Detected compiler newer than Visual Studio XXX, please update min version checking"

XXX is just whatever version of Visual Studio the branch was last successfully tested with. This can appear if you are using an even more recent version of Visual Studio (i.e. 2022 latest versions). It is just a warning and shouldn't cause any issues as VS2022 seems to be able to build the engine fine.

### When packaging HTML5 you see: **error CS1519: Invalid token '(' in class, struct, or interface member declaration**

If you see this when trying to package for HTML5 then in Visual Studio CTRL-Click these:
- AutomationTool
- AutomationToolLauncher
- HTML5LaunchHelper
- UnrealBuildTool

Then do **Right Click -> Rebuild Selection** to force rebuild the .NET programs. Seems to fix the issue.

### When packaging HTML5 you see: "WARNING: Library XXX as not resolvable to a file when used in Module 'XXX'"

The XXX is usually PhysX. This seems to occur when HTML5Setup.sh randomly fails to properly build PhysX (or some other third party library) with an error like ```mingw32-make.exe[2]: write error: stdout``` but doesn't actually stop the ./HTML5Setup.sh build so it is easy to miss and you will only notice when then trying to package for HTML5.

If you get this issue try running ./HTMLSetup.sh again.

NOTE: In the latest versions of the above branches the third party libraries are built using only a single thread (-j 1) to hopefully avoid this problem happening as much.

### When running game you see in browser console an error with: "Assertion failed" regarding multisampled textures

Try disabling Mobile MSAA (in Project Rendering options).

### When running game you see in browser console an error with: "Assertion failed" relating to a compression method and "file needs to be forced to use zlib compression"

This can happen if you have PAK file compression format set to **Oodle**. This HTML5 plugin requires you to use **Zlib** compression. You can change this as follows:

Project Settings -> Packaging -> Pak File Compression Format(s): Change this from **Oodle** to **Zlib** 

NOTE: This is an advanced setting so you need to click the down arrow to unhide the setting.

Once you have done this you can package the project again as HTML5 and see if the issue is fixed. You may need to to clear the IndexedDB and/or browser cache so you get the newly generated PAK file.

### When running the game you only see a blank white page with buttons at the top and/or in the console log you see: "Uncaught SyntaxError: illegal character U+001F" and/or you see error/warning message: "Downloaded a compressed file XXX without the necessary HTTP response header "Content-Encoding: gzip""

If you are using compressed packaging for HTML5 (which is the default), you need to ensure the files ending in `.gz' (i.e. the compressed files) are served with the following HTTP header:

    Content-Encoding: gzip
    
This signals to the web browser that it needs to decompress the file before trying to use it.

The way the header can be set depends on how you are hosting the files. It will differ per solution - check the documentation for your web hosting solution. 

However, if you need to temporarily work around this, you can disable compressed packaging:

Project Settings -> HTML5 -> Packaging -> Compress files during packaging: Set to **false**

Most of the increase in size due to turning off this compression will be in the size of the Unreal binary code which will no longer be compressed. The PAK data file actually has its own compression (which should be turned on by default) handled by Unreal so depending on how big your project the increase may or may not be a significant versus the overall size. 

Also some web hosting solutions may _automatically_ do all the gzip compression of .css / .js assets on the fly (and set appropriate headers too) so in those cases you may be better off not using compressed packaging.

### When running the game with the ES3 branch you see a blue tint / incorrect reflections

There is something wrong with the ES3 fork with regard to reflections (cause is unknown at the moment). One clear issue is that the sphere reflection capture somehow causes a blue color tint on materials where reflections occur. You can see this in the following screenshot where the water and some of the character clothing has reflections which appear blue (rather than properly showing the environment in the reflection):

<img src="Images/ActionRPG_BlueTintIssue.jpg" style="width:400px"/>

For now, a workaround you could try is to pick a gray texture for your sphere reflection spheres (rather than using scene captures). Go to your SphereReflectionCapture's and/or your SkyLight etc. and set the Reflection Capture -> Reflection Source Type to **Specified Cubemap** and then choose Reflection Capture -> Cubemap as a gray texture e.g. **GrayTextureCube** (you can choose lighter / darker version depending on how visible you want the reflections i.e. whatever looks best for your particular project). This is not ideal but seems to be a way to mitigate this blue tint issue for now:

<img src="Images/ActionRPG_GrayTextureCubeWorkaround.jpg" style="width:400px"/>

[Original report thread with discussion](https://github.com/SpeculativeCoder/UnrealEngine/issues/19) (NOTE: this link requires your GitHub account to be linked to Epic Games account or you will see 404). Thanks to [@MackeyK24](https://github.com/MackeyK24) for reporting this issue.

### Why is the download so big? Can I reduce the size of the generated application?

By default, Unreal includes all assets used by _any_ map in the project. So if you have enabled starter content, or some Marketplace assets, these may have maps in them that are dragging in a lot of assets you didn't actually intend to use.

You should set the maps which will be included the packaging via this setting (make sure it has only the maps you make use of):

Project Settings -> Packaging -> List of maps to include in packaged build

This is an advanced setting so you may need to click the down arrow to show it.