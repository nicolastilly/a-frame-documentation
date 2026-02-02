# Lights

![screen capture](images/lights.gif)

By default, A-Frame scenes inject default lighting, an ambient light and a directional light.

In this example, i add two lights: one ambient and one spot.

``` { .html .copy linenums="1"}
<!DOCTYPE html>
<html lang="en">

    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>A-Frame lights</title>
        <script src="https://aframe.io/releases/1.7.1/aframe.min.js"></script>
    </head>

    <body>
        <a-scene>

            <!--the ambient light with a color-->
            <a-entity light="type: ambient; color: #EF5D8B"></a-entity>

            <!--the sport light with color, intensity and position-->
            <a-entity light="type: spot; color: #89cc24; intensity: 8" position="0 3.5 -0.6"></a-entity>


            <!-- Primitive shapes -->
            <a-box position="-1 0.5 -3" rotation="0 45 0" color="#4CC3D9"></a-box>
            <a-sphere position="0 1.25 -5" radius="1.25" color="#FEEE91"></a-sphere>
            <a-cylinder position="1 0.75 -3" radius="0.5" height="1.5" color="#FFA239"></a-cylinder>
            <a-plane position="0 0 -4" rotation="-90 0 0" width="40" height="40" color="#FEEE91"></a-plane>

            <!--add a sky to your scene-->
            <a-sky color="#8CE4FF"></a-sky>

        </a-scene>
    </body>

</html>
```