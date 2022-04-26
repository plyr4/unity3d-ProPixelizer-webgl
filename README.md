## unity3d-ProPixelizer-webgl

This repository contains a sample Unity project for running the [ProPixelizer](https://assetstore.unity.com/packages/vfx/shaders/fullscreen-camera-effects/propixelizer-177877?aid=1011lGbg&pubref=am&utm_source=aff#description) ExampleScene using the Universal Render Pipeline.

The purpose of this project is to provide a working scene that describes an issue I'm encountering with [ProPixelizer](https://assetstore.unity.com/packages/vfx/shaders/fullscreen-camera-effects/propixelizer-177877?aid=1011lGbg&pubref=am&utm_source=aff#description) and WebGL builds in Unity 2020.3.32f1.

### Preface

Let me start this by saying I love ProPixelizer and everything that it provides and aims to accomplish. 
It's providing me the exact tool I need to bridge the gap between my love for the pixelized aesthetic and 3D games. I've been working tirelessly on a body of work using low-polygon 3d models, ProPixelizer, and Unity 3D physics and it's going great so far. Once I have a working demo I will return to the Asset Store immediately to give a raving 5/5 review.

However, I encountered an issue with the `Object Render Snapable` script component and WebGL builds running in the browser. See below.

### The Problem

When **building the ExampleScene using WebGL**, any object with an attached `Object Render Snapable` script component **will disappear from view**.

**_NOTE:_** This only occurs when built through WebGL. Windows builds render `Object Render Snapable` objects accurately.

See below for a description of the issue, how to reproduce using this GitHub repository, and the exact specs that the problem is reproduced under.

### Runtime Specs

You can find most of the Unity and WebGL runtime information from the project files, but here are the specs that the problem is being reproduced with:

```
Unity engine version: 2020.3.32f1 (12f8b0834f07)
webgl Renderer: WebKit WebGL
webgl Vendor:   WebKit
webgl Version:  OpenGL ES 3.0 (WebGL 2.0 (OpenGL ES 3.0 Chromium))
webgl GLES:     3
```

### Tested Browser Versions

The problem was reproduced in the following browser versions:
- Google Chrome Version 100.0.4896.127 (Official Build) (64-bit)
- Edge Version 100.0.1185.50 (Official build) (64-bit)
- Firefox 99.0.1 (64-bit)


### Reproducing the Problem via Provided Scene

1. Open the project in Unity. (I am currently running `2020.3.32f1`)

2. Open `Assets/Scenes/ExampleSceneCopyAndBuild.unity`.

    - The name refers to how the scene was created, by copying the ExampleScene and setting Build Platform to WebGL.

3. Hit play in the editor, to see all four objects in the MainCamera (looks good!).

4. Go to File > Build Settings > ensure Platform is set to WebGL then click Build and Run.

5. Once the game builds and runs the fourth object disappears from view.

6. In the scene inspector, select the object `Cube with 4x4 pixelisation, alpha cutout and Object Snap` (long name, but its accurate).

7. Remove the `Object Render Snapable` script component.

8. Go to File > Build Settings > ensure Platform is set to WebGL then click Build and Run.


### How the Scene Was Configured

The project was configured by strictly following the [ProPixelizer User Guide (v1.6)](https://sites.google.com/view/propixelizer/user-guide).

- Set up URP.
- Set up Camera.
- Duplicate the ExampleScene, open Scene.
- Duplicate the Alpha Example object, move it "down".
- Add `Object Render Snapable` to the duplicated object.
- Build and Run via WebGL.

Only one change was required to get the ExampleScene to render properly:

- added `PixelizedWithOutline` shader to `Always Included Shaders` (the green sphere was rendering gray)

See below for a description of the issue, how to reproduce using this GitHub repository, and the exact specs that the problem is reproduced under.

### WebGL Runtime Logs

These are the logs produced by the browser when running the WebGL application:

```
[UnityCache] 'http://localhost:62027/Build/webgl.data.gz' successfully downloaded and stored in the indexedDB cache
webgl.framework.js.gz:2 Loading player data from data.unity3d

webgl.framework.js.gz:2 Initialize engine version: 2020.3.32f1 (12f8b0834f07)

webgl.framework.js.gz:2 [Subsystems] Discovering subsystems at path UnitySubsystems

webgl.loader.js:1 Creating WebGL 2.0 context.
webgl.framework.js.gz:2 Renderer: WebKit WebGL

webgl.framework.js.gz:2 Vendor:   WebKit

webgl.framework.js.gz:2 Version:  OpenGL ES 3.0 (WebGL 2.0 (OpenGL ES 3.0 Chromium))

webgl.framework.js.gz:2 GLES:     3

webgl.framework.js.gz:2  EXT_color_buffer_float GL_EXT_color_buffer_float EXT_color_buffer_half_float GL_EXT_color_buffer_half_float EXT_disjoint_timer_query_webgl2 GL_EXT_disjoint_timer_query_webgl2 EXT_float_blend GL_EXT_float_blend EXT_texture_compression_bptc GL_EXT_texture_compression_bptc EXT_texture_compression_rgtc GL_EXT_texture_compression_rgtc EXT_texture_filter_anisotropic GL_EXT_texture_filter_anisotropic EXT_texture_norm16 GL_EXT_texture_norm16 KHR_parallel_shader_compile GL_KHR_parallel_shader_compile OES_draw_buffers_indexed GL_OES_draw_buffers_indexed OES_texture_float_linear GL_OES_texture_float_linear WEBGL_compressed_texture_s3tc GL_WEBGL_compressed_texture_s3tc WEBGL_compressed_texture_s3tc_srgb GL_WEBGL_compressed_texture_s3tc_srgb WEBGL_debug_renderer_info GL_WEBGL_debug_renderer_info WEBGL_debug_shaders GL_WEBGL_debug_shaders WEBGL_lose_context GL_WEBGL_lose_context WEBGL_multi_draw GL_WEBGL_multi_draw OVR_multiview2 GL_OVR_multiview2
```
