# Sky & environments

## Change the sky of the scene

<figure markdown="span">
![video capture](images/changethesky.gif)
<figcaption>Example of a modified sky </figcaption>
</figure>

Create the file `skyGradient.js` and add the code:

``` { .html .copy linenums="1"}
/* global AFRAME */
AFRAME.registerShader('skyGradient', {
  schema: {
    colorTop: { type: 'color', default: 'black', is: 'uniform' },
    colorBottom: { type: 'color', default: 'red', is: 'uniform' }
  },

  vertexShader: [
    'varying vec3 vWorldPosition;',

    'void main() {',

      'vec4 worldPosition = modelMatrix * vec4( position, 1.0 );',
      'vWorldPosition = worldPosition.xyz;',

      'gl_Position = projectionMatrix * modelViewMatrix * vec4( position, 1.0 );',

    '}'

  ].join('\n'),

  fragmentShader: [
    'uniform vec3 colorTop;',
    'uniform vec3 colorBottom;',

    'varying vec3 vWorldPosition;',

    'void main()',

    '{',
      'vec3 pointOnSphere = normalize(vWorldPosition.xyz);',
      'float f = 1.0;',
      'if(pointOnSphere.y > - 0.2){',

        'f = sin(pointOnSphere.y * 2.0);',

      '}',
      'gl_FragColor = vec4(mix(colorBottom,colorTop, f ), 1.0);',

    '}'
  ].join('\n')
});
```

Then add the file and environment to index.html.

``` { .html .copy linenums="1"}
<!-- dans le head -->
<script src="/skyGradient.js"></script>

<!-- dans le body -->
<a-entity
  id="sky"
  geometry="primitive: sphere; radius: 65;"
  material="shader: skyGradient; colorTop: #353449; colorBottom: #BC483E; side: back"
></a-entity>
```
Full code for the exemple:
??? Code
    ``` { .html .copy linenums="1"}
    <html>
        <head>
            <script src="https://aframe.io/releases/1.7.1/aframe.min.js"></script>
            <script src="/skyGradient.js"></script>
        </head>
        <body>
            <a-scene>
                <a-entity id="sky" geometry="primitive: sphere; radius: 65;"
                    material="shader: skyGradient; colorTop: #353449; colorBottom: #BC483E; side: back"><a-entity>
                        <a-sphere position="0 1.25 -5" radius="1.25" color="#CCCCCC"></a-sphere>
            </a-scene>
        </body>
    </html>
    ```



## Add a terrain

<div class="grid cards" markdown>

- **Be careful in Blender to position the terrain correctly in the center of the scene.**

- ![Blender screen capture](images/blenderterrainlocation.png)

</div>


To add a terrain in .glb format to the A-Frame scene:

- Create the `ground.js` file:
??? Code
    ``` { .js .copy linenums="1"}
    /* global AFRAME, THREE */

    /**
    * Loads and setup ground model.
    */
    AFRAME.registerComponent('ground', {
    init: function () {
        this.onModelLoaded = this.onModelLoaded.bind(this);
        this.el.addEventListener('model-loaded', this.onModelLoaded);
        this.el.setAttribute('gltf-model', '#ground');
    },
    onModelLoaded: function (evt) {
        var model = evt.detail.model;
        model.children.forEach(function (value) {
        value.receiveShadow = true;
        value.material.flatShading = THREE.FlatShading;
        });
    }
    });
    ```
- Link the file in index.html:
??? Code
    ``` { .html .copy linenums="1"}
    <!-- dans le head -->
    <script src="/ground.js"></script>
    ```
- Add the .glb file to index.html
??? Code
    ``` { .html .copy linenums="1"}
    <a-assets>
    <a-asset-item
        id="ground"
        src="https://cdn.glitch.global/daa3d4b3-f117-42b3-904f-3f14e4a78ba9/ground2.glb?v=1746539192732"><!-- changer le lien vers le fichier glb -->
        </a-asset-item>
    </a-assets>

    <a-entity ground></a-entity>
    ```

## Alternative solution for using pre-designed environments

The [aframe-environment-component](https://github.com/supermedium/aframe-environment-component) library is very useful for using several very different terrains in an A-frame project.

<figure markdown="span">
![video](https://github.com/feiss/aframe-environment-component/raw/master/assets/aframeenvironment.gif?raw=true)
<figcaption>Image: a-frame-environment-component repository</figcaption>
</figure>

To use aframe-environment-component:

- Use version 1.7.0 of A-Frame
- Then include the file: `<script src="https://unpkg.com/aframe-environment-component@1.5.x/dist/aframe-environment-component.min.js"></script>`
- Finally, add the environment to the scene: `<a-entity environment="preset: <name of the preset>"></a-entity>`

List of environments:
`none`, `default`, `contact`, `egypt`, `checkerboard`, `forest`, `goaland`, `yavapai`, `goldmine`, `threetowers`, `poison`, `arches`, `tron`, `japan`, `dream`, `volcano`, `starry`, `osiris`, `moon`

For example, with the `forest` environment, it gives:
`<a-entity environment="preset: forest"></a-entity>`