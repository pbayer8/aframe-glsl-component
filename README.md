## aframe-GLSL-component

Aframe component to inject custom GLSL into the THREE.js MeshStandardMaterial Shader.

Have a look at the [example](https://pbayer8.github.io/aframe-glsl-component/examples/index.html)

### API

| Property   | Description | Default Value |
| ---------- | ----------- | ------------- |
|fragUniformsGLSL | selectorAll - selects the script tag to inject in the beggining of the fragment shader|#fragUniformsGLSL|
|fragModsGLSL | selectorAll - selects the script tag to inject in main() of the fragment shader|#fragModsGLSL|
|vertUniformsGLSL | selectorAll - selects the script tag to inject in the beggining of the vertex shader|#vertUniformsGLSL|
|vertModsGLSL | selectorAll - selects the script tag to inject in main() of the fragment shader|#vertModsGLSL|
Note: you can also set almost every property accessible in the [materials component](https://github.com/aframevr/aframe/blob/a46c1b133634de39e5f40c35bf28823a0960c7b5/docs/components/material.md)

### Installation
Include as a `<script>` tag:
```
<script src="https://cdn.rawgit.com/pbayer8/aframe-glsl-component/master/main.js"></script>
```

### Usage
Add the `glsl-standard` component to an entity with geometry. Note that it works better with more segments:
```
<a-scene fog="type:linear;near:2;far:15;">
    <a-assets>
        <img src="images/floral.jpg" id="floral"/>
        <img src="images/couchnormal.png" id="couch"/>
        <img src="images/scene.jpg" id="sky"/>
        <img src="images/grass.png" id="grass"/>
    </a-assets>
    <a-sphere position="0 1.4 -2"
              glsl-standard="
                  envMap:#sky;
                  map:#floral;
                  normalMap:#couch;
                  metalness:.5;"
              segments-height="128"
              segments-width="128">
    </a-sphere>
</a-scene>
```

Then, write GLSL. This component give you access to two vec3 type variables you can modify: `finalPosition` in the vertex shader, and `finalColor` in the fragment shader. It also creates and updates two floats variables `time` (mS) and `seconds` that you can use. You can specify alternate script tags, otherwise it defaults to what is listed in the API section above:
```
<script id="vertUniformsGLSL" type="x-shader/x-fragment">
    //add vertex shader uniforms, varyings and functions here:
    varying float wave1;
    varying float wave2;
</script>
<script id="vertModsGLSL" type="x-shader/x-fragment">
    //modify or reassign the finalPosition vec3 here:
    wave1 = sin(finalPosition.y*5.+seconds*2.);
    wave2 = cos(finalPosition.x*9.+seconds*3.);
    finalPosition.x += .1*wave1;
    finalPosition.z += .1* wave2;
</script>
<script id="fragUniformsGLSL" type="x-shader/x-fragment">
    //add fragment shader uniforms, varyings and functions here:
    varying float wave1;
    varying float wave2;
</script>
<script id="fragModsGLSL" type="x-shader/x-fragment">
    //modify or reassign the finalColor vec3 here:
    finalColor.r += min(wave1,0.);
    finalColor.b += wave2;
</script>
```

The result will look like:
[this Image](https://imgur.com/a/QevHIjf.gif)