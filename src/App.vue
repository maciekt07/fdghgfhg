<script stetup>
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
      points: 0,
      inputDisabled: false,
    };
  },
  methods: {
    rgbToHex(r, g, b) {
      return "#" + ((1 << 24) + (r << 16) + (g << 8) + b).toString(16).slice(1);
    },

    random() {
      this.r = Math.floor(Math.random() * 256);
      this.g = Math.floor(Math.random() * 256);
      this.b = Math.floor(Math.random() * 256);
      this.clr = `rgb(${this.r}, ${this.g}, ${this.b})`;
      this.start = true;
      this.messageVisible = false;
      this.inputDisabled = false;
      this.message = "";
      console.log(`Correct answer: ${this.rgbToHex(this.r, this.g, this.b)}`);
    },

    submit() {
      const correctAnswer = this.rgbToHex(this.r, this.g, this.b);
      this.inputDisabled = true;
      this.messageVisible = true;
      if (this.message == correctAnswer || this.message == correctAnswer.toUpperCase()) {
        return "Answer is correct!";
      } else {
        return `Answer isn't correct! The correct answer was ${this.rgbToHex(
          this.r,
          this.g,
          this.b
        )}`;
      }
    },
  },
};
</script>

<template>
  <div class="container">
    <div class="Color"></div>
    <h1 v-if="!start">RGB To HEX Game</h1>
    <h1 v-if="start">{{ `RGB: ${r} ${g} ${b}` }}</h1>
    <input
      :class="{ sucess: message.length == 7, error: message.length < 7 && message.length > 0 }"
      :disabled="inputDisabled"
      type="text"
      v-if="start"
      maxlength="7"
      v-model="message"
      :placeholder="!inputDisabled ? 'Enter hex code...' : ''"
    />
    <br />
    <button v-if="start && !messageVisible" @click="submit">Submit</button><br />

    <button v-if="messageVisible || !start" @click="random">
      {{ start == false ? "Start" : "Play Again" }}</button
    ><br />
    <p v-if="messageVisible">
      {{ submit() }}
    </p>
  </div>
  <footer v-if="!start">
    Made with <span class="heart">❤</span> By
    <a target="_blank" href="https://github.com/maciekt07">github.com/maciekt07</a>
  </footer>
</template>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap");

.heart {
  transition: 0.3s;
}
.heart:hover {
  color: #3abdff;
}
a {
  cursor: pointer;
  transition: 0.4s all;
  color: #3abdff;
  display: inline-block;
  position: relative;
  text-decoration: none;
}
a::after {
  content: "";
  position: absolute;
  width: 100%;
  transform: scaleX(0);
  height: 2px;
  bottom: 0;
  left: 0;
  background-color: #3abdff;
  transform-origin: bottom right;
  transition: transform 0.25s ease-out;
  border-radius: 100px;
}

a:hover {
  text-shadow: 0px 0px 20px #3abdff;
}

a:hover::after {
  transform: scaleX(1);
  transform-origin: bottom left;
}

footer {
  width: 100%;
  color: white;
  text-align: center;
  font-size: 20px;
}

input[type="text"] {
  font-size: 20px;
  padding: 8px;
  border-radius: 8px;
  border: none;
}

input[type="text"].error:focus,
input[type="text"].error:focus-visible {
  outline: 3px solid red;
}

input[type="text"].sucess:focus,
input[type="text"].sucess:focus-visible {
  outline: 3px solid green;
}

p {
  background: rgb(77, 77, 77);
  padding: 10px;
  border-radius: 6px;
}
.container {
  font-family: poppins;
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
  font-size: 1.15em;
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
