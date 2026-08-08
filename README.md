# LWJGL NanoVG native libraries

A generic Fabric library mod that ships the **LWJGL NanoVG native libraries** for platforms
that are not bundled with the game or with other mods: `linux-arm64`, `linux-arm32`,
`windows-arm64`.

Any mod that uses `org.lwjgl:lwjgl-nanovg` (e.g. custom NanoVG-rendered GUIs) can use this.

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

Drop `lwjgl-nanovg-natives-1.0.0.jar` into the mods folder. That's it.

## How it works / layout

The natives are packaged under the exact resource paths LWJGL 3.4.x's loader searches:

```
linux/arm64/org/lwjgl/nanovg/liblwjgl_nanovg.so
linux/arm32/org/lwjgl/nanovg/liblwjgl_nanovg.so
windows/arm64/org/lwjgl/nanovg/lwjgl_nanovg.dll
META-INF/linux/{arm64,arm32}/org/lwjgl/nanovg/liblwjgl_nanovg.so.sha1
META-INF/windows/arm64/org/lwjgl/nanovg/lwjgl_nanovg.dll.sha1
```

Sources: `org.lwjgl:lwjgl-nanovg:3.4.1` natives jars from Maven Central
(https://repo1.maven.org/maven2/org/lwjgl/lwjgl-nanovg/3.4.1/), BSD-3-Clause license.

## Requirements

- The game must already have a desktop-ish OpenGL environment (this mod only fixes native
  *loading*, not missing desktop GL). A GL 3.3+ context is required for the NanoVG GL3
  backend to render.
- The consuming mod's own bytecode/JDK requirements still apply; this is purely a native
  resource pack.

## Rebuild

No Java code, so no Loom needed:

```
gradle build
# or, without gradle: zip the src/main/resources contents and rename to .jar
```
