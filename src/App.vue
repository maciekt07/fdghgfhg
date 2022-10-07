<script stetup>
import { ref } from "vue";
export default {
  data() {
    return {
      r: 0,
      g: 0,
      b: 0,
      clr: "",
      start: false,
      messageVisible: false,
      message: "",
    };
  },
  methods: {
    random() {
      this.r = Math.floor(Math.random() * 256);
      this.g = Math.floor(Math.random() * 256);
      this.b = Math.floor(Math.random() * 256);
      this.clr = `rgb(${this.r}, ${this.g}, ${this.b})`;
      this.start = true;
      this.messageVisible = false;
      this.message = "";
    },
    rgbToHex(r, g, b) {
      return "#" + ((1 << 24) + (r << 16) + (g << 8) + b).toString(16).slice(1);
    },
    submit() {
      console.log(this.message);
      this.messageVisible = true;
    },
  },
};
</script>

<template>
  <div class="container">
    <div class="Color"></div>
    <h1 v-if="!start">Random RGB Color</h1>
    <h1 v-if="start">{{ `RGB: ${r} ${g} ${b}` }}</h1>
    <h3 v-if="start">{{ userInput }}</h3>

    <input
      type="text"
      v-if="start"
      v-model="message"
      placeholder="enter hex code"
    />
    <br />
    <button v-if="start && !messageVisible" @click="submit">Submit</button
    ><br />

    <button v-if="messageVisible || !start" @click="random">
      {{ start == false ? "Start" : "Play Agaim" }}</button
    ><br />
    <p v-if="messageVisible">
      {{
        message == rgbToHex(r, g, b)
          ? "Answer is correct!"
          : `Answer isn't correct.
     The correct answer was ${rgbToHex(r, g, b)}`
      }}
    </p>
  </div>
</template>

<style>
input[type="text"] {
  font-size: 20px;
  padding: 8px;
  border-radius: 8px;
  border: none;
}
p {
  background: rgb(77, 77, 77);
  padding: 10px;
  border-radius: 6px;
}
.container {
  justify-content: center;
  align-items: center;
  display: flex;
  flex-direction: column;
  flex-wrap: nowrap;
  border: 8px solid v-bind(clr);
  border-radius: 35px;
  width: 500px;
  height: 650px;
  transition: 0.3s;
  box-shadow: 0px 0px 50px 1px v-bind(clr);
}
.Color {
  width: 100px;
  height: 100px;
  background: v-bind(clr);
  border-radius: 18px;
  /* border: 3px solid white; */
  transition: 0.3s;
}
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
  border-color: v-bind(clr);
}
button:focus,
button:focus-visible {
  /* outline: 4px auto -webkit-focus-ring-color; */
  outline: none;
}

#app {
  max-width: 1280px;
  margin: 0 auto;
  padding: 2rem;
  text-align: center;
  justify-content: center;
}
</style>
