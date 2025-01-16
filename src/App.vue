<template>
  <div class="app">
    <!-- Composant pour afficher l'iPhone en 3D -->
    <IphoneDisplay ref="iphoneDisplay" />

    <!-- Barre de sélection des couleurs -->
    <div class="color-picker">
      <button
          v-for="color in colors"
          :key="color"
          :style="{ backgroundColor: color }"
      @click="changeColor(color)"
      ></button>
    </div>
  </div>
</template>

<script>
import IphoneDisplay from '../src/components/IphoneDisplay.vue';

export default {
  name: 'App',
  components: { IphoneDisplay }, // Inclusion du composant iPhone 3D
  data() {
    return {
      colors: ['#ffffff', '#181e27', '#ff0000', '#bbe4e6', '#899ee6'], // Liste des couleurs disponibles
    };
  },
  methods: {
    // Méthode pour transmettre la couleur choisie au composant enfant
    changeColor(color) {
      this.$refs.iphoneDisplay.setColor(color); // Appelle la méthode setColor dans IphoneDisplay
    },
  },
};
</script>

<style>
/* Conteneur des pastilles de couleur */
.color-picker {
  position: absolute;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  gap: 10px;
}

/* Style des boutons (pastilles) de couleur */
.color-picker button {
  width: 40px;
  height: 40px;
  border: none;
  border-radius: 50%; /* Forme ronde */
  cursor: pointer;
  border: 1px solid whitesmoke;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2); /* Effet d'ombre */
}
</style>
