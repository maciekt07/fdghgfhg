<script>
import Footer from "./components/Footer.vue";
const main = {
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
  components: { Footer },
};
export default main;
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
    <button v-if="start && !messageVisible" @click="submit"><fa icon="check" />&nbsp;Submit</button
    ><br />

    <button v-if="messageVisible || !start" @click="random">
      <span v-if="!start"> <fa icon="play" /> &nbsp;Start</span>
      <span v-else><fa icon="rotate-right" /> &nbsp;Play Again</span></button
    ><br />
    <p v-if="messageVisible">
      {{ submit() }}
    </p>
  </div>

  <Footer :Hidden="!start" />
</template>

<style lang="scss" scoped>
@use "style" as *;

input[type="text"] {
  font-size: 20px;
  padding: 8px;
  border-radius: 8px;
  border: none;
  &.error:focus,
  &.error:focus-visible {
    outline: 3px solid red;
  }
  &.sucess:focus,
  &.sucess:focus-visible {
    outline: 3px solid green;
  }
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
</style>
