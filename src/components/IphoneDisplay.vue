<template>
  <div class="app-container">
    <div class="header">
      <h1>L'iPhone 16</h1>
      <p>Titane, Graphite, Argent, Or, Bleu. Plus fort que jamais ! </p>
      <p>Un bond vers l'innovation. Personnalisez sa couleur pour l'adapter à votre style.</p>
    </div>


    <div ref="sceneContainer"></div>

    <div v-if="showInstructions" class="instructions">
      🖱️ Faites glisser pour tourner
    </div>
  </div>
</template>

<script>
import * as THREE from 'three';
import {GLTFLoader} from 'three/examples/jsm/loaders/GLTFLoader';
import {OrbitControls} from 'three/examples/jsm/controls/OrbitControls';

export default {
  name: "IphoneDisplay",
  data() {
    return {
      colors: ['#000000', '#ffffff', '#ff0000', '#00ff00', '#0000ff'],
      model: null,
      showInstructions: true,
    };
  },
  mounted() {
    this.initScene();
  },
  methods: {
    initScene() {
      const container = this.$refs.sceneContainer;


      const scene = new THREE.Scene();
      scene.background = new THREE.Color(0xf9f9f9);


      const camera = new THREE.PerspectiveCamera(
          75,
          window.innerWidth / window.innerHeight,
          0.1,
          1000
      );
      camera.position.set(8, 6, 12);
      camera.lookAt(0, 0, 0);

      // Rendu
      const renderer = new THREE.WebGLRenderer({antialias: true});
      renderer.setSize(window.innerWidth, window.innerHeight);
      container.appendChild(renderer.domElement);

      // Lumières
      const ambientLight = new THREE.AmbientLight(0xffffff, 1.2);
      scene.add(ambientLight);

      const directionalLight = new THREE.DirectionalLight(0xffffff, 1.5);
      directionalLight.position.set(7, 5, 15);
      scene.add(directionalLight);

      // Chargement modèle
      const loader = new GLTFLoader();
      loader.load(
          '/models/iphone_16_plus.glb',
          (gltf) => {
            const model = gltf.scene;
            model.scale.set(1, 1, 1); // Agrandissement modèle
            model.position.set(0, -0.3, 0);
            scene.add(model);
            this.model = model;

            setTimeout(() => {
              this.showInstructions = false;
            }, 5000);
          },
          undefined,
          (error) => {
            console.error('Erreur lors du chargement du modèle :', error);
          }
      );

      // Contrôles de la caméra
      const controls = new OrbitControls(camera, renderer.domElement);
      controls.enableDamping = true;
      controls.dampingFactor = 0.1;
      controls.target.set(0, 0, 0);
      controls.update();

      // Animation de la scène
      const animate = () => {
        requestAnimationFrame(animate);
        controls.update();
        renderer.render(scene, camera);
      };
      animate();

      // Mise à jour lors du redimensionnement
      window.addEventListener('resize', () => {
        camera.aspect = window.innerWidth / window.innerHeight;
        camera.updateProjectionMatrix();
        renderer.setSize(window.innerWidth, window.innerHeight);
      });
    },

    setColor(color) {
      if (this.model) {
        this.model.traverse((child) => {
          if (child.isMesh) {
            child.material.color.set(color);
          }
        });
      }
    },
  },
};
</script>

<style scoped>

.app-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  width: 100%;
  height: 100vh;
  background: #f9f9f9;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  color: #333;
  text-align: center;
  position: relative;
}


.header {
  margin-bottom: 20px;
}

.header h1 {
  font-size: 3rem;
  font-weight: 600;
  margin: 0;
  color: #111;
}

.header p {
  font-size: 1.2rem;
  color: #555;
  margin-top: 10px;
}

/* Palette de couleurs */
.color-palette {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.color-button {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  border: 2px solid #ddd;
  cursor: pointer;
  transition: transform 0.2s ease;
}

.color-button:hover {
  transform: scale(1.2);
  border: 2px solid #aaa;
}


.instructions {
  position: absolute;
  bottom: 30%;
  left: 50%;
  transform: translateX(-50%);
  padding: 10px 20px;
  background: rgba(255, 255, 255, 0.9);
  border-radius: 20px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  font-size: 1.1rem;
  color: #555;
  animation: fadeOut 1s ease-out 6s forwards;
}

@keyframes fadeOut {
  from {
    opacity: 1;
  }
  to {
    opacity: 0;
  }
}
</style>
