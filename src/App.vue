<script>
import Footer from "./components/Footer.vue";
import Message from "./components/Message.vue";
const main = {
  data() {
    return {
      r: 0,
      g: 0,
      b: 0,
      clr: "",
      start: false,
      messageVisible: false,
      userInput: "",
      points: 0,
      inputDisabled: false,
      tooShort: false,
      message1: "",
      message2: "",
      points: 0,
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
      this.tooShort = false;
      this.userInput = "";
      console.log(`Correct answer: ${this.rgbToHex(this.r, this.g, this.b)}`);
    },
    submit() {
      const correctAnswer = this.rgbToHex(this.r, this.g, this.b);
      this.inputDisabled = true;
      this.messageVisible = true;
      if (this.userInput == correctAnswer || this.userInput == correctAnswer.toUpperCase()) {
        this.message1 = "Answer is correct!";
        return true;
      } else {
        this.message1 = "Answer isn't correct! The correct answer was";
        this.message2 = this.rgbToHex(this.r, this.g, this.b);
        return false;
      }
    },
    submitCheck() {
      if (this.userInput.length == 7) {
        this.points++;
        this.message2 = `Points: ${this.points}`;
        this.submit();
        this.tooShort = false;
      } else {
        this.tooShort = true;
        this.messageVisible = true;
        this.message1 = "Answer is too short!";
        this.message2 = this.userInput;
      }
    },
  },
  components: { Footer, Message },
};
export default main;
</script>

<template>
  <div class="container">
    <h1 v-if="start" class="points">Points: {{ points }}</h1>
    <div class="Color"></div>

    <h1 v-if="!start">RGB To HEX Game</h1>
    <h1 v-if="start">{{ `RGB: ${r} ${g} ${b}` }}</h1>
    <input
      :class="{
        sucess: userInput.length == 7,
        error: userInput.length < 7 && userInput.length > 0,
      }"
      :disabled="inputDisabled"
      type="text"
      v-if="start"
      maxlength="7"
      v-model="userInput"
      :placeholder="!inputDisabled ? 'Enter hex code...' : ''"
    />
    <br />
    <button v-if="(start && !messageVisible) || tooShort" @click="submitCheck">
      <fa icon="check" />&nbsp;Submit</button
    ><br />

    <button v-if="messageVisible || !start || tooShort" @click="random">
      <span v-if="!start"> <fa icon="play" /> &nbsp;Start</span>
      <span v-else><fa icon="rotate-right" /> &nbsp;Play Again</span></button
    ><br />

    <Message :Message1="message1" :Message2="message2" :BorderColor="clr" v-if="messageVisible" />
  </div>

  <Footer :Hidden="!start" />
</template>

<style lang="scss" scoped>
@use "style" as *;

.points {
  text-align: left;
  font-size: 18px;
  margin-right: 350px;
}

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
