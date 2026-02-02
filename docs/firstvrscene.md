# The first VR scene

## This A-Frame scene is configured in VR

It is to be used with a VR headset (in my case, the Meta Quest 2). To do this, open the headset's browser and go to the page's URL. Once open, a message will appear stating that the site is immersive. You must confirm to open the page in full screen.

This scene allows you to confirm:

- Opening a web page in the Meta Quest 2 browser and launching full-screen navigation.
- Moving around the scene with the two controllers (left joystick to move and right controller to move with teleportation).

![screen capture of the scene](images/firstvrscene.gif)


??? code
    ``` { .html .copy linenums="1"}
    <!DOCTYPE html>
    <html>
    <head>
        <script src="https://aframe.io/releases/1.7.1/aframe.min.js"></script>
        <!-- Ajout des composants pour la navigation -->
        <script src="https://cdn.jsdelivr.net/gh/n5ro/aframe-physics-system@v4.0.1/dist/aframe-physics-system.min.js"></script>
        <script src="https://cdn.jsdelivr.net/gh/donmccurdy/aframe-extras@v6.1.1/dist/aframe-extras.min.js"></script>
        
        <script>
        // Attendez que tous les composants soient chargés
        window.addEventListener('load', function() {
            console.log('Page chargée, initialisation des contrôleurs VR');
        });
        
        // Enregistrement d'un composant personnalisé pour le joystick
        AFRAME.registerComponent('thumbstick-controls', {
            schema: {
            speed: {default: 3.0}
            },
            
            init: function() {
            this.moving = false;
            this.velocity = new THREE.Vector3(0, 0, 0);
            this.cameraRig = document.querySelector('#cameraRig');
            
            // Debug pour vérifier l'initialisation
            console.log('Composant thumbstick-controls initialisé');
            
            // Écouteurs d'événements pour le joystick
            this.el.addEventListener('thumbstickmoved', this.handleThumbstickMoved.bind(this));
            this.el.addEventListener('thumbstickdown', function() {
                console.log('Joystick enfoncé!');
            });
            
            // Affichage des événements du contrôleur pour le débogage
            this.el.addEventListener('controllerconnected', function(evt) {
                console.log('Contrôleur connecté:', evt.detail.name);
            });
            },
            
            handleThumbstickMoved: function(evt) {
            console.log('Joystick déplacé:', evt.detail.x, evt.detail.y);
            
            // Enregistrez le mouvement du joystick
            // Inversion de l'axe Y pour correspondre à l'orientation intuitive du joystick
            if (Math.abs(evt.detail.x) > 0.1 || Math.abs(evt.detail.y) > 0.1) {
                this.velocity.x = evt.detail.x * this.data.speed;
                this.velocity.z = evt.detail.y * this.data.speed; // Valeur positive maintenant pour corriger l'inversion
                this.moving = true;
            } else {
                this.moving = false;
            }
            },
            
            tick: function(time, delta) {
            if (!this.moving) return;
            
            // Convertir delta en secondes pour une vitesse cohérente
            const deltaSeconds = delta / 1000;
            
            // Obtenir la direction de la caméra
            const rotation = document.querySelector('#head').object3D.rotation;
            const direction = new THREE.Vector3();
            direction.set(
                this.velocity.x,
                0,
                this.velocity.z
            );
            
            // Appliquer la rotation de la caméra au mouvement
            direction.applyAxisAngle(new THREE.Vector3(0, 1, 0), rotation.y);
            
            // Mettre à jour la position du rig
            this.cameraRig.object3D.position.x += direction.x * deltaSeconds;
            this.cameraRig.object3D.position.z += direction.z * deltaSeconds;
            }
        });
        
        // Composant pour la téléportation
        AFRAME.registerComponent('teleport-controls', {
            schema: {
            cameraRig: {default: '#cameraRig'},
            button: {default: 'trigger'}
            },
            
            init: function () {
            this.cameraRig = document.querySelector(this.data.cameraRig);
            
            // Création d'un raycaster pour la téléportation
            this.raycaster = new THREE.Raycaster();
            
            // Debug
            console.log('Composant teleport-controls initialisé');
            
            // Configuration de l'événement de téléportation
            this.el.addEventListener('triggerdown', () => {
                console.log('Trigger enfoncé, préparation à la téléportation');
                this.el.setAttribute('raycaster', 'showLine', true);
            });
            
            this.el.addEventListener('triggerup', this.teleport.bind(this));
            },
            
            teleport: function () {
            console.log('Tentative de téléportation');
            
            // Raycasting pour détecter le point de téléportation
            const direction = new THREE.Vector3(0, 0, -1);
            direction.applyQuaternion(this.el.object3D.quaternion);
            
            // Mise à jour de la position du rig
            this.cameraRig.object3D.position.x += direction.x * 3;
            this.cameraRig.object3D.position.z += direction.z * 3;
            
            console.log('Téléportation effectuée à:', this.cameraRig.object3D.position);
            
            this.el.setAttribute('raycaster', 'showLine', false);
            }
        });
        </script>
    </head>
    <body>
        <a-scene 
        physics="debug: true"      
        >
        
        <!-- Spécification pour les contrôleurs VR -->
        <a-entity id="cameraRig">
            <!-- Caméra -->
            <a-entity id="head" camera position="0 1.6 0" look-controls wasd-controls></a-entity>
            
            <!-- Contrôleurs mains -->
            <a-entity id="leftHand" 
                    hand-controls="hand: left; handModelStyle: lowPoly;" 
                    thumbstick-controls="speed: 3.0"
                    visible="true">
            <!-- Modèle visuel pour le contrôleur gauche -->
            <a-box scale="0.02 0.02 0.08" position="0 0 -0.04" color="red"></a-box>
            </a-entity>
            
                    
                    
            <a-entity id="rightHand" 
                    hand-controls="hand: right; handModelStyle: lowPoly;" 
                    raycaster="showLine: true; far: 20; interval: 0; objects: .clickable, [static-body]" 
                    teleport-controls
                    visible="true">
            <!-- Modèle visuel pour le contrôleur droit -->
            <a-box scale="0.02 0.02 0.08" position="0 0 -0.04" color="green"></a-box>
            </a-entity>
        </a-entity>
        
        <!-- Vos objets existants -->
        <a-box position="-1 0.5 -3" rotation="0 45 0" color="#4CC3D9" class="clickable"></a-box>
        <a-sphere position="0 1.25 -5" radius="1.25" color="#EF2D5E" class="clickable"></a-sphere>
        <a-cylinder position="1 0.75 -3" radius="0.5" height="1.5" color="#FFC65D" class="clickable"></a-cylinder>
        <a-plane position="0 0 -4" rotation="-90 0 0" width="20" height="20" color="#7BC8A4" 
                static-body></a-plane>
        <a-sky color="#ECECEC"></a-sky>
        
        <!-- Message d'information pour le débogage -->
        <a-entity position="0 2.5 -4" text="value: Utilisez le joystick pour vous deplacer\nAppuyez sur la gachette pour vous teleporter; color: black; align: center; width: 2;"></a-entity>
        
        </a-scene>
    </body>
    </html>

    ```