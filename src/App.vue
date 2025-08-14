<template>
  <StarBackground :config="starConfig" />
  <ControlPanel :config="starConfig" @update:config="updateStarConfig" />
</template>

<script>
import StarBackground from "./components/StarBackground.vue";
import ControlPanel from "./components/ControlPanel.vue";

export default {
  name: "App",
  components: {
    StarBackground,
    ControlPanel,
  },
  data() {
    return {
      starConfig: {
        numStars: 120,
        speedMin: 0.05,
        speedMax: 0.2,
        radiusMin: 0.3,
        radiusMax: 1.5,
        alphaMin: 0.3,
        alphaMax: 0.8,
        spikeChance: 0.1,
        spikeLength: 3,
        glowIntensity: 8,
      },
    };
  },
  methods: {
    updateStarConfig(newConfig) {
      this.starConfig = { ...newConfig };
    },
  },
};
</script>

<style>
#app {
  font-family: "Inter", Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;

  width: 100%;
}

body {
  margin: 0;
  padding: 0;
  background: #01030e;
}
</style>
