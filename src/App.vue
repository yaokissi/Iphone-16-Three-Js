<template>
  <div class="app">

    <IphoneDisplay ref="iphoneDisplay" />
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
      colors: ['#ffffff', '#181e27', '#ff0000', '#bbe4e6', '#899ee6'],
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
/* Conteneur pastilles couleur */
*{
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
}
.color-picker {
  position: absolute;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  gap: 10px;
}

/* Style pastille  couleur */
.color-picker button {
  width: 40px;
  height: 40px;
  border: none;
  border-radius: 50%;
  cursor: pointer;
  border: 1px solid whitesmoke;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
}
</style>
