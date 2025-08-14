<template>
  <div class="star-control-panel">
    <div class="control-header">
      <h2 class="control-title" :style="{ marginRight: isOpen ? '0px' : '32px' }">Star Field Controls</h2>
      <button
        class="toggle-btn icon-btn"
        @click="isOpen = !isOpen"
        :aria-label="isOpen ? 'Hide controls' : 'Show controls'"
      >
        <i v-if="isOpen" class="fa-solid fa-chevron-up"></i>
        <i v-else class="fa-solid fa-chevron-down"></i>
      </button>
    </div>
    <div v-if="isOpen">
      <div v-for="(value, key) in localConfig" :key="key" class="control-row">
        <div class="control-label">{{ formatLabel(key) }}</div>
        <div class="control-slider">
          <input
            type="range"
            :id="key + '-slider'"
            :min="getMin(key)"
            :max="getMax(key)"
            :step="getStep(key)"
            v-model.number="localConfig[key]"
            @input="emitUpdate"
          />
        </div>
        <div class="control-value">
          <input
            type="number"
            :id="key + '-number'"
            :min="getMin(key)"
            :max="getMax(key)"
            :step="getStep(key)"
            v-model="localConfig[key]"
            @input="onNumberInput(key, $event)"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ControlPanel",
  props: {
    config: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      localConfig: { ...this.config },
      isOpen: true,
      minMax: {
        numStars: [10, 500],
        speedMin: [0.01, 0.5],
        speedMax: [0.01, 0.5],
        radiusMin: [0.1, 3],
        radiusMax: [0.1, 3],
        alphaMin: [0.1, 1],
        alphaMax: [0.1, 1],
        spikeChance: [0, 1],
        spikeLength: [1, 10],
        glowIntensity: [0, 20],
      },
      step: {
        speedMin: 0.01,
        speedMax: 0.01,
        radiusMin: 0.1,
        radiusMax: 0.1,
        alphaMin: 0.01,
        alphaMax: 0.01,
        spikeChance: 0.01,
        spikeLength: 0.1,
        glowIntensity: 1,
        default: 1,
      },
    };
  },
  watch: {
    config: {
      handler(newVal) {
        this.localConfig = { ...newVal };
      },
      deep: true,
    },
  },
  methods: {
    emitUpdate() {
      this.$emit("update:config", { ...this.localConfig });
    },
    onNumberInput(key, event) {
      const value = event.target.value;
      if (value === "") {
        this.localConfig[key] = this.getMin(key);
      } else {
        let number = Number(value);
        const min = this.getMin(key);
        const max = this.getMax(key);
        if (number < min) number = min;
        if (number > max) number = max;
        this.localConfig[key] = number;
      }
      this.emitUpdate();
    },
    isNumber(value) {
      return typeof value === "number";
    },
    getMin(key) {
      return this.minMax[key][0];
    },
    getMax(key) {
      return this.minMax[key][1];
    },
    getStep(key) {
      return this.step[key] || this.step.default;
    },
    formatLabel(key) {
      const labelMap = {
        numStars: "Number of Stars",
        speedMin: "Min Speed",
        speedMax: "Max Speed",
        radiusMin: "Min Radius",
        radiusMax: "Max Radius",
        alphaMin: "Min Opacity",
        alphaMax: "Max Opacity",
        spikeChance: "Spike Probability",
        spikeLength: "Spike Length",
        glowIntensity: "Glow Intensity",
      };
      return labelMap[key] || key;
    },
  },
};
</script>

<style scoped>
.star-control-panel {
  position: fixed;
  bottom: 24px;
  left: 24px;
  background: rgba(30, 30, 30, 1);
  color: white;
  padding: 16px 40px 0 24px;
  border-radius: 10px;
  z-index: 10;
  font-size: 14px;
}

.control-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 20px;
  margin-right: -40px;
  padding-right: 24px;
}

.control-title {
  margin: 0;
}

.icon-btn {
  width: 48px;
  height: 48px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  font-size: 24px;
  cursor: pointer;
  transition: background 0.2s, border 0.2s;
}

.icon-btn:hover {
  background: #444;
  border-color: #bbb;
}

.toggle-btn {
  background: none;
  border: 2px solid #e0e0e0;
  border-radius: 6px;
}

.control-row {
  display: grid;
  grid-template-columns: 1.1fr 1.5fr 0.4fr;
  align-items: center;
  margin-bottom: 16px;
  gap: 20px;
}

.control-label {
  text-transform: none;
  font-weight: 500;
  color: #e0e0e0;
}

.control-slider input[type="range"] {
  width: 100%;
  margin: 0;
}

.control-value input[type="number"] {
  width: 100%;
  height: 24px;
  padding: 0 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
  background-color: white;
  color: black;
}
</style>
