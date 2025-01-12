# Unreal Engine 4.27 HTML5 ES3 (WebGL 2) & Unreal Engine 4.24 HTML5 ES2 (WebGL 1)

## Caveats

There are various issues/caveats to be aware of (some already existed, some are new in this fork):

- **Project Settings -> Packaging -> Pak File Compression Format(s)** is now set to **Zlib** by default, and you should have it set to this in your projects. Oodle compression (which was default in Epic UE4.27) is not supported.
- **Project Settings -> HTML5 -> Emscripten -> Multithreading support** is set to **False** by default, and you should have it set to this in your projects. HTML5 multithreading is not fully supported in this fork and currently only seems to work when packaging in Development mode (Test/Shipping renders a black screen).
- **Debug/DebugGame packaging is not supported**. Only Development, Testing, and Shipping packaging is supported.
- **MacOS with dedicated graphics (e.g. Intel Macs with NVIDIA/AMD cards) may not automatically use dedicated graphics** (reason unknown at the moment) which can result in much worse performance. Users with external displays plugged in will already be using dedicated graphics so should be fine. Other users can temporarily [disable Automatic graphics switching](https://support.apple.com/en-us/HT202043) to force use of dedicated graphics.
- Safari does not currently allow pointerlock (mouse cursor capture) once in fullscreen due to an [issue](https://bugs.webkit.org/show_bug.cgi?id=272136). As a workaround, an experimental option is available in **Project Settings -> HTML5 -> Emscripten -> Request pointerlock before fullscreen (Experimental)** which, although not reliable, will try to capture the pointer before switching to full screen, which sometimes works OK. This will likely be fixed soon in an upcoming Safari release so is only a temporary feature.
- **Safari mouse sensitivity is dramatically different to Firefox/Chrome-based browsers**. This may be due to how Safari reports mouse events and/or mouse movementX/Y - see [MDN](https://developer.mozilla.org/en-US/docs/Web/API/MouseEvent/movementX) and [issue](https://github.com/w3c/pointerlock/issues/42). 
- **Project Settings -> Rendering -> Mobile -> Mobile MSAA** should always be set to **No MSAA** (the default) as mobile MSAA is not supported (if enabled it will cause an "Assertion failed" error on startup).
- **Video playing, and likely anything related to Unreal [Media Framework](https://docs.unrealengine.com/4.27/en-US/WorkingWithMedia/IntegratingMedia/MediaFramework/) does not work**.
- **Niagara particles are not available**. You will need to use Cascade (legacy) particle systems instead.

For all features you may wish to use, a good rule of thumb is: anything that didn't work in Epic's last supported version of HTML5 packaging (4.23) or the community supported plugin (4.24) probably won't work in this fork. Anything that needs compute shaders won't work as that isn't supported in WebGL 1 or WebGL 2. See [this page](https://docs.unrealengine.com/4.27/en-US/RenderingAndGraphics/SupportedRenderingFeatures/) for an indication of what features may be supported (see the Android ES3.1 column which will be most useful indication as to what _may_ work in the ES3 branch of this fork).

See [TROUBLESHOOTING](TROUBLESHOOTING.md) for more detail on typical issues / troubleshooting / workarounds.