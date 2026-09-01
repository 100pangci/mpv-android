# mpv-android-lib

[![Build Status](https://github.com/100pangci/mpv-android/actions/workflows/build.yml/badge.svg?branch=library)](https://github.com/100pangci/mpv-android/actions/workflows/build.yml)
[![Maven Central](https://img.shields.io/maven-central/v/io.github.100pangci/mpv-android-lib.svg)](https://central.sonatype.com/artifact/io.github.100pangci/mpv-android-lib)

A library version of [mpv-android](https://github.com/mpv-android/mpv-android), providing [libmpv](https://github.com/mpv-player/mpv) for Android applications.
Initially made for [mpvKt](https://github.com/abdallahmehiz/mpvKt) by [@abdallahmehiz](https://github.com/abdallahmehiz/);
this fork is maintained by [mpvKt](https://github.com/100pangci/mpvKt) and aligned with the
upstream [mpv-android](https://github.com/mpv-android/mpv-android) master buildscripts.

## "New" Features

* **Multiple MPV instances**
* **`mpv_node` support**
* **DASH support**

## Installation

Add the dependency to your `build.gradle`:

```groovy
dependencies {
    implementation "io.github.100pangci:mpv-android-lib:<version>"
}
```

## Getting Started

### Using BaseMPVView

`BaseMPVView` is a plain `SurfaceView` shell that attaches/detaches the
video surface. Create the `MPV` instance yourself and hand it to the view:

```kotlin
class MyPlayerView(context: Context, attrs: AttributeSet?) : BaseMPVView(context, attrs)

val playerView = MyPlayerView(context, null)
playerView.mpv = MPV(
    context,
    configDir = filesDir.path, // mpv.conf/input.conf are read from here
    cacheDir = cacheDir.path,
)
```

### Using MPV() Directly

The `MPV` constructor creates and initializes libmpv in one go:

```kotlin
val mpv = MPV(context, configDir = filesDir.path, cacheDir = cacheDir.path)

// Attach to a view surface
mpv.attachSurface(surface)

// Load and play a file
mpv.command("loadfile", "/path/to/video.mp4")

// Access props
val paused: Boolean? = mpv.prop["pause"]
mpv.prop["pause"] = false

// access and set nodes
val node: MPVNode? = mpv.getPropertyNode("track-list")
mpv.setPropertyNode("chapter-list", myCustomChaptersList)

// observe as kotlin flows
val pauseState: StateFlow<Boolean?> = mpv.propFlow["pause"]

// cleanup
mpv.detachSurface()
mpv.close()
```

### Multiple Instances

Each `MPV` instance is independent:

```kotlin
val player1 = MPV(context)
val player2 = MPV(context)

// Each player can play different content simultaneously
```

## Building from source

Take a look at the [README](buildscripts/README.md) inside the `buildscripts` directory.

Some other documentation can be found at this [link](http://mpv-android.github.io/mpv-android/).
