# LWJGL NanoVG native libraries

![Minecraft](https://img.shields.io/badge/Minecraft-26.1.2-00af9c)
![Mod loader](https://img.shields.io/badge/Mod%20Loader-Fabric-1976d2)
![LWJGL](https://img.shields.io/badge/LWJGL-3.4.1-333333)
![Platforms](https://img.shields.io/badge/Platforms-Android%20ARM64%20%7C%20ARM32%20%7C%20Windows%20ARM64-4caf50)

![Release](https://img.shields.io/github/v/release/menearmenear/LWJGL-NanoVG-native-libraries)
![Downloads (latest release)](https://img.shields.io/github/downloads/menearmenear/LWJGL-NanoVG-native-libraries/latest/total?label=downloads%20%28latest%29)
![Downloads (total)](https://img.shields.io/github/downloads/menearmenear/LWJGL-NanoVG-native-libraries/total)
![Release date](https://img.shields.io/github/release-date/menearmenear/LWJGL-NanoVG-native-libraries?label=release%20date)
![Last commit](https://img.shields.io/github/last-commit/menearmenear/LWJGL-NanoVG-native-libraries)
![License](https://img.shields.io/badge/license-BSD--3--Clause-959da5)

Fabric library mod for **Minecraft 26.1.2** that ships the **LWJGL NanoVG native
libraries** (LWJGL **3.4.1**) for platforms that are not bundled with the game or with
other mods: Android `linux-arm64`, `linux-arm32` (Bionic builds) and `windows-arm64`.

Any mod that uses `org.lwjgl:lwjgl-nanovg` (e.g. custom NanoVG-rendered GUIs) can use this.

> Note: desktop Linux arm64 natives from Maven Central are built against **glibc**
> (`libm.so.6`) and will fail to `dlopen` on Android. This mod ships the **Bionic**
> builds used by the Pojav/Mojo Android launchers, so they load on Android devices.

## Why this exists

Most mods that bundle `lwjgl-nanovg` natives only shade desktop x86_64 variants
(`windows`, `linux`, `macos`, `macos-arm64`). On mobile launchers (Android aarch64) or ARM
desktops, LWJGL looks for the resource `linux/arm64/org/lwjgl/nanovg/liblwjgl_nanovg.so`,
does not find it, and the game crashes when the GUI initializes:

```
java.lang.UnsatisfiedLinkError: Failed to locate library: liblwjgl_nanovg.so
  at org.lwjgl.nanovg.LibNanoVG.<clinit>
```

LWJGL loads natives from *classpath resources* of any jar, so this mod simply places the
missing platform natives on the game classpath. No mixins, no code, no changes to any mod.

## Install

Drop `lwjgl-nanovg-natives-1.0.2.jar` from the latest
[release](https://github.com/menearmenear/LWJGL-NanoVG-native-libraries/releases) into the
mods folder. That's it.

## How it works / layout

The natives are packaged under the exact resource paths LWJGL 3.4.1's loader searches:

```
linux/arm64/org/lwjgl/nanovg/liblwjgl_nanovg.so
linux/arm32/org/lwjgl/nanovg/liblwjgl_nanovg.so
windows/arm64/org/lwjgl/nanovg/lwjgl_nanovg.dll
META-INF/linux/{arm64,arm32}/org/lwjgl/nanovg/liblwjgl_nanovg.so.sha1
META-INF/windows/arm64/org/lwjgl/nanovg/lwjgl_nanovg.dll.sha1
```

Sources: `org.lwjgl:lwjgl-nanovg:3.4.1` natives from
[Maven Central](https://repo1.maven.org/maven2/org/lwjgl/lwjgl-nanovg/3.4.1/) (windows-arm64)
and the Android (Bionic) builds from the
[MojoLauncher/unilwjgl3-builder](https://github.com/MojoLauncher/unilwjgl3-builder/releases/tag/v3.4.1-5)
release (linux-arm64, linux-arm32), BSD-3-Clause license.

## Requirements

- The game must already have a desktop-ish OpenGL environment (this mod only fixes native
  *loading*, not missing desktop GL). A GL 3.3+ context is required for the NanoVG GL3
  backend to render.
- The consuming mod's own bytecode/JDK requirements still apply; this is purely a native
  resource pack.

## Build

No Java code, so no Loom needed:

```
gradle buildJars     # produces both the mod jar and the sources jar
```

## Previous versions

| Release | Game | LWJGL | Natives |
| ------- | ---- | ----- | ------- |
| [v1.0.2](https://github.com/menearmenear/LWJGL-NanoVG-native-libraries/releases/tag/v1.0.2) | 26.1.2 | 3.4.1 | Android arm64/arm32 (Bionic), windows-arm64 |
| [v1.0.1](https://github.com/menearmenear/LWJGL-NanoVG-native-libraries/releases/tag/v1.0.1) | 26.1.2 | 3.4.1 | linux-arm64, linux-arm32, windows-arm64 |
| [v1.0.0](https://github.com/menearmenear/LWJGL-NanoVG-native-libraries/releases/tag/v1.0.0) | 1.21.11 | 3.3.3 | linux-arm64, linux-arm32, windows-arm64 |
