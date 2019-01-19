<template>
  <div id="input-container">
    <input type="text" v-model="text" @keydown.13="regenerate">
    <button @click="regenerate">GO</button>
  </div>
</template>

<script>
import Matrix from '@/utils/matrix';

export default {
  name: 'app',
  data() {
    return {
      text: '01',
    };
  },
  mounted() {
    this.canvas = document.createElement('canvas');
    this.resize();
    this.init();
    document.body.appendChild(this.canvas);
    window.addEventListener('resize', this.resize);
  },
  methods: {
    init() {
      const { canvas, text } = this;
      const context = canvas.getContext('2d');
      this.$matrix = new Matrix({ canvas, context, text });
    },
    resize() {
      this.canvas.width = window.innerWidth;
      this.canvas.height = window.innerHeight;
      this.regenerate();
    },
    regenerate() {
      if (this.$matrix) this.$matrix.generate(this.text);
    },
  },
  beforeDestroy() {
    window.removeEventListener('resize', this.resize);
    this.$matrix.destroy();
  },
};
</script>

<style lang="less">
body {
  background-color: #000;
}
#input-container {
  display: flex;
  position: absolute;
  padding: 10px 20px;
  top: 0;
  left: 50%;
  transform: translateX(-50%) translateZ(0);
  background-color: #000;

  input {
    display: block;
    border: none;
    outline: 0;
    border-radius: 4px;
    line-height: 36px;
    padding: 0 8px;
    margin-right: 8px;
  }
  button {
    display: block;
    height: 36px;
    padding: 0 10px;
    margin: 0;
    line-height: 36px;
    border-radius: 20px;
    outline: 0;
    border: none;
    cursor: pointer;
    background-color: #00ff00;
  }
}
</style>
