<script>
export default {
  data() {
    return {
      r: 0,
      g: 0,
      b: 0,
      clr: "",
      showhex: false,
    };
  },
  methods: {
    random() {
      this.r = Math.floor(Math.random() * 256);
      this.g = Math.floor(Math.random() * 256);
      this.b = Math.floor(Math.random() * 256);
      this.clr = `rgb(${this.r}, ${this.g}, ${this.b})`;
      this.showhex = true;
    },
    rgbToHex(r, g, b) {
      return "#" + ((1 << 24) + (r << 16) + (g << 8) + b).toString(16).slice(1);
    },
  },
};
</script>

<template>
  <div style="background: {{clr}}" class="Color"></div>
  <h1 v-if="!showhex">Random RGB Color</h1>
  <h1 v-if="showhex">{{ `RGB: ${r} ${g} ${b}` }}</h1>
  <h2 v-if="showhex">{{ rgbToHex(r, g, b) }}</h2>
  <button @click="random">{{ showhex == false ? "Start" : "Random" }}</button>
</template>

<style>
:root {
  font-family: Inter, Avenir, Helvetica, Arial, sans-serif;
  font-size: 16px;
  line-height: 24px;
  font-weight: 400;

  color-scheme: light dark;
  color: rgba(255, 255, 255, 0.87);
  background-color: #242424;

  font-synthesis: none;
  text-rendering: optimizeLegibility;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  -webkit-text-size-adjust: 100%;
}

body {
  margin: 0;
  display: flex;
  place-items: center;
  min-width: 320px;
  min-height: 100vh;
}
h1 {
  font-size: 3.2em;
  line-height: 1.1;
}

button {
  border-radius: 8px;
  border: 1px solid transparent;
  padding: 0.6em 1.2em;
  font-size: 1em;
  font-weight: 500;
  font-family: inherit;
  background-color: #1a1a1a;
  cursor: pointer;
  transition: border-color 0.25s;
}
button:hover {
  border-color: #646cff;
}
button:focus,
button:focus-visible {
  outline: 4px auto -webkit-focus-ring-color;
}

#app {
  max-width: 1280px;
  margin: 0 auto;
  padding: 2rem;
  text-align: center;
  justify-content: center;
}
.Color {
  width: 100px;
  height: 100px;
  background: v-bind(clr);
}
</style>
