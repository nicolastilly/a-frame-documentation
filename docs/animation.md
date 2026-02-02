# Animating a 3D object in the scene

[Link to details of the animation component in the A-Frame documentation.](https://aframe.io/docs/1.7.0/components/animation.html)

<figure markdown="span">
![screen capture](images/moveacube.gif)
<figcaption>Moving some primitives</figcaption>
</figure>

## Move a cube

``` { .html .copy linenums="1"}
<a-box position="-1 1.6 -5" animation="property: position; to: 1 8 -10; dur: 2000; easing: linear; loop: true" color="tomato"></a-box>
```

**Animation settings details:**

``` { .html .copy linenums="1"}
  animation="
    property: position;
    from: -1 42 0;      <!-- position de départ -->
    to:    1 42 0;      <!-- position d’arrivée -->
    dur:   3000;        <!-- durée aller ou retour -->
    dir:   alternate;   <!-- jouer aller puis retour -->
    loop:  true;        <!-- répéter indéfiniment -->
    easing: easeInOutSine;" <!-- courbe d'animation -->
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
                      material="shader: skyGradient; colorTop: yellow; colorBottom: #BC483E; side: back"><a-entity>
                          <a-box position="-1 1.6 -5"
                              animation="property: position; to: 1 8 -10; dur: 2000; dir: alternate;  easing: linear; loop: true"
                              color="black"></a-box>
                          <a-sphere position="-5 1.6 -5"
                              animation="property: position; to: 1 8 -10; dur: 4000; dir: alternate;  easing: linear; loop: true"
                              color="white"></a-sphere>
              </a-scene>
          </body>
      </html>
    ```
