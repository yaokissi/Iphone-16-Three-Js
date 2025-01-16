<template>
  <!-- Le conteneur pour la scène 3D -->
  <div ref="sceneContainer" class="scene-container"></div>
</template>

<script>
import * as THREE from 'three';
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls';

export default {
  name: "IphoneDisplay",
  mounted() {
    this.initScene();
  },
  methods: {
    initScene() {
      const container = this.$refs.sceneContainer;

      // Création de la scène
      const scene = new THREE.Scene();
      scene.background = new THREE.Color(0xf0f0f0); // Fond gris clair

      // Création de la caméra
      const camera = new THREE.PerspectiveCamera(
          75,
          container.offsetWidth / container.offsetHeight,
          0.1,
          1000
      );
      camera.position.set(5, 3, 10); // Position initiale
      camera.lookAt(0, 0, 0);

      // Rendu
      const renderer = new THREE.WebGLRenderer({antialias: true});
      renderer.setSize(container.offsetWidth, container.offsetHeight);
      container.appendChild(renderer.domElement);

      // Lumière ambiante
      const ambientLight = new THREE.AmbientLight(0xffffff, 0.8); // Intensité plus élevée
      scene.add(ambientLight);

      // Lumière directionnelle principale
      const directionalLight = new THREE.DirectionalLight(0xffffff, 1);
      directionalLight.position.set(5, 5, 5);
      directionalLight.castShadow = true;
      scene.add(directionalLight);

      // Lumière directionnelle secondaire (éclairage opposé)
      const secondaryLight = new THREE.DirectionalLight(0xffffff, 0.5);
      secondaryLight.position.set(-5, -5, -5);
      scene.add(secondaryLight);

      // Lumière ponctuelle pour un éclairage supplémentaire
      const pointLight = new THREE.PointLight(0xffffff, 0.7, 100);
      pointLight.position.set(0, 5, 5);
      scene.add(pointLight);

      // Chargement du modèle 3D
      const loader = new GLTFLoader();
      loader.load(
          '/models/iphone_16_plus.glb',
          (gltf) => {
            const model = gltf.scene;
            model.scale.set(0.5, 0.5, 0.5); // Échelle
            model.position.set(0, -1, 0); // Position centré
            scene.add(model);
            this.model = model;
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
        camera.aspect = container.offsetWidth / container.offsetHeight;
        camera.updateProjectionMatrix();
        renderer.setSize(container.offsetWidth, container.offsetHeight);
      });
    },

    setColor(color) {
      if (this.model) {
        this.model.traverse((child) => {
          if (child.isMesh) {
            child.material.color.set(color); // Change la couleur
          }
        });
      }
    },
  },
};
</script>

<style scoped>
/* Style du conteneur pour la scène */
.scene-container {
  width: 100%;
  height: 100vh;
  overflow: hidden;
}
</style>
