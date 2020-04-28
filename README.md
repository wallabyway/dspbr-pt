# Enterprise PBR Sample Renderer

A WebGL2 based [glTF](https://www.khronos.org/gltf/) path-tracer which implements the [Enterprise PBR](https://github.com/DassaultSystemes-Technology/EnterprisePBRShadingModel) material model.

The renderer fully supports the glTF 2.0 metallic/roughness model. The additional Enterprise PBR material features are supported as 
additional material extensions as defined by following pull requests to the official glTF 2.0 spec.

Summary of the extensions and change proposal to the original core spec
https://github.com/KhronosGroup/glTF/pull/1738

* [Specular Extensions](https://github.com/KhronosGroup/glTF/pull/1719)
* [IoR Extension](https://github.com/KhronosGroup/glTF/pull/1718)
* [Sheen Extension](https://github.com/KhronosGroup/glTF/pull/1688)
* [Clearcoat Extension](https://github.com/KhronosGroup/glTF/pull/1756)
* [Transmission Extension](https://github.com/KhronosGroup/glTF/pull/1698) (Not yet implemeted)

**NOTE**

When disabling path-tracing, the three.js native rasterizer is used as fallback. This renderer just supports the standard glTF 2.0 material model, the extensions mentioned above will are ignored.

Please have a look at [this overview](https://k0mplex.uber.space/reports/threejs/) for comparison renderings between the two renderers for several material configurations.


## Enterprise PBR - Sample Implementation
This renderer should serve as a sample implementation for Dassault Syst�mes Enterprise PBR material model. Please have a look at 
[dspbr.glsl](./lib/shader/dspbr.glsl) for the full material implementation.

## Setup

```
npm install 
npm run build
```

## Run

```
npm run dev
```

Launches the viewer in a browser with attached file watcher for auto refresh on file edits.

### Electron

Alternatively, use electron

```
npm start
```

or with file watcher for development

```
npm run dev:electron
```

## CLI Renderer

```
npm run render -- -- <scene_path> --ibl <hdr_path> --res <width> <height> --samples <num_samples>
```

For now, this writes the output image to ./output.png

### Example

```
npm run render -- -- assets/scenes/DamagedHelmet/DamagedHelmet.gltf --ibl assets/env/analytical_chromatic.hdr --res 1024 1024 --samples 64
```

## Using the Library 

The renderer can be easily used as stand-alone component.
Please note that you need to have three.js setup as dependency in your scene. 

```
npm install three 
```

Using the renderer
```javascript

 import { PathtracingRenderer } from './lib/renderer.js';

 let ibl = <path_to_ibl>
 let scene = <path_to_gltf>
 let renderer = new PathtracingRenderer({ "canvas": canvas });
 renderer.loadScene(scene, ibl, function () {
            renderer.render(-1, () => {
                stats.update();
            });
        });
```

Where the interface of the render function is as

```javascript
 function render(num_samples, frameFinishedCB, renderingFinishedCB)
```

Please have a look at src/index.html and lib/renderer.js for the details.


## Todo

### Material
- [ ] Implement proper transparency and alpha mask (cutout) handling
- [ ] Implement importance sampling for all components (clearcoat, sheen, ...)


### General
- [ ] Move path-tracing renderer to plain WebGL 


## Reference 

* This project was inspired by [Three.js glTF Viewer](https://github.com/donmccurdy/three-gltf-viewer) and the official [Khronos glTF Sample Viewer](https://github.com/KhronosGroup/glTF-Sample-Viewer).
* Env maps are rescaled versions of the [glTF Sample Viewer environment maps](https://github.com/ux3d/glTF-Sample-Environments).