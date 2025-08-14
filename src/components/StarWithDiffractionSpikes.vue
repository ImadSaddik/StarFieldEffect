<template>
  <canvas id="star-bg"></canvas>
</template>

<script>
export default {
  name: "StarWithDiffractionSpikes",
  data() {
    return {
      width: window.innerWidth,
      height: window.innerHeight,
      context: null,
      radius: 50,
      alpha: 1,
      spikeLength: 5,
      glowIntensity: 0,
      starColors: [
        "#ffffff", // White
        "#ffe9c4", // Pale yellow
        "#b5d0ff", // Blue-white
        "#f7f7a8", // Yellowish
        "#c4e1ff", // Light blue
        "#ffd700", // Gold
        "#ffb347", // Orange
      ],
    };
  },
  methods: {
    drawStar() {
      this.context.clearRect(0, 0, this.width, this.height);
      this.context.save();

      // Draw diffraction spikes (JWST style)
      const numSpikes = 6;
      const spikeLength = this.radius * this.spikeLength + Math.random() * 2;
      const spikeWidth = Math.max(0.5, this.radius * 0.5);
      const color = this.starColors[Math.floor(Math.random() * this.starColors.length)];
      const positionX = this.width / 2;
      const positionY = this.height / 2;

      for (let i = 0; i < numSpikes; i++) {
        this.context.save();

        const angle = (i * Math.PI) / (numSpikes / 2);
        this.context.translate(positionX, positionY);
        this.context.rotate(angle);
        this.context.beginPath();
        this.context.moveTo(0, 0);
        this.context.lineTo(0, -spikeLength);
        this.context.strokeStyle = color;
        this.context.shadowColor = color;
        this.context.shadowBlur = this.glowIntensity;
        this.context.lineWidth = spikeWidth;
        this.context.globalAlpha = this.alpha * 0.7;
        this.context.stroke();

        this.context.restore();
      }

      // Draw the star circle
      this.context.globalAlpha = this.alpha;
      this.context.beginPath();
      this.context.arc(positionX, positionY, this.radius, 0, 2 * Math.PI);
      this.context.fillStyle = color;
      this.context.shadowColor = color;
      this.context.shadowBlur = this.glowIntensity;
      this.context.fill();

      this.context.restore();
    },
    handleResize() {
      this.width = window.innerWidth;
      this.height = window.innerHeight;
      const canvas = this.$el;
      canvas.width = this.width;
      canvas.height = this.height;
      this.drawStar();
    },
    setupCanvas() {
      const canvas = this.$el;
      this.context = canvas.getContext("2d");
      canvas.width = this.width;
      canvas.height = this.height;
      canvas.style.position = "fixed";
      canvas.style.top = 0;
      canvas.style.left = 0;
      canvas.style.zIndex = -1;
      canvas.style.width = "100vw";
      canvas.style.height = "100vh";
      canvas.style.pointerEvents = "none";
    },
  },
  mounted() {
    this.setupCanvas();
    this.drawStar();
    window.addEventListener("resize", this.handleResize);
  },
  beforeUnmount() {
    window.removeEventListener("resize", this.handleResize);
  },
};
</script>

<style scoped>
#star-bg {
  position: fixed;
  top: 0;
  left: 0;
  z-index: -1;
  width: 100vw;
  height: 100vh;
  pointer-events: none;
}
</style>
