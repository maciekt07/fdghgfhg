<script>
import { ref } from "@vue/reactivity";
import Footer from "./components/Footer.vue";
import Message from "./components/Message.vue";
import Points from "./components/Points.vue";

const main = {
  data() {
    return {
      r: Number,
      g: Number,
      b: Number,
      points: 0,
      start: false,
      messageVisible: false,
      inputDisabled: false,
      tooShort: false,
      clr: "",
      userInput: String,
      message1: String,
      message2: String,
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
      if (
        this.userInput == correctAnswer ||
        this.userInput == correctAnswer.toUpperCase()
      ) {
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
        if (
          this.userInput == this.rgbToHex(this.r, this.g, this.b) ||
          this.userInput == this.rgbToHex(this.r, this.g, this.b).toUpperCase()
        ) {
          this.points++;
        }
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
  components: { Footer, Message, Points },
};
export default main;
</script>

<template>
  <div class="container" :class="{ start: !start }">
    <Points v-if="start" :Points="points" />
    <div class="Color"></div>

    <h1 v-if="!start">RGB To HEX Game</h1>
    <h1 v-if="start">{{ `RGB: ${r} ${g} ${b}` }}</h1>
    <input
      :class="{
        success: userInput.length == 7,
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
    ><br v-if="start" />

    <button v-if="(messageVisible && !tooShort) || !start" @click="random">
      <span v-if="!start"> <fa icon="play" /> &nbsp;Start</span>
      <span v-else><fa icon="rotate-right" /> &nbsp;Play Again</span>
    </button>

    <Message
      :Message1="message1"
      :Message2="message2"
      :BorderColor="clr"
      v-if="messageVisible"
    />
  </div>

  <Footer v-if="!start" />
</template>

<style lang="scss" scoped>
@use "style" as *;

input[type="text"] {
  font-size: 20px;
  padding: 8px;
  border-radius: 8px;
  border: none;
  transition: 0.2s;
  &.error:focus,
  &.error:focus-visible {
    outline: 3px solid $error;
    box-shadow: 0px 0px 10px 1px $error;
    &::selection {
      background: $error;
    }
  }
  &.success:focus,
  &.success:focus-visible {
    outline: 3px solid $success;
    box-shadow: 0px 0px 10px 1px $success;
    &::selection {
      background: $success;
    }
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
.start {
  height: 250px;
  padding: 30px;
  border: 8px solid $linkColor;
  button {
    margin-bottom: 30px;
  }
  // h1 {
  //   background-color: red;
  //   background-image: linear-gradient(45deg, #566dff, #8a50f6);
  //   background-size: 100%;
  //   background-repeat: repeat;
  //   -webkit-background-clip: text;
  //   -webkit-text-fill-color: transparent;
  //   -moz-background-clip: text;
  //   -moz-text-fill-color: transparent;
  // }
  *::selection {
    background: lighten($color: $linkColor, $amount: 15);
    color: rgb(119, 0, 255);
  }
}

.Color {
  width: 100px;
  height: 100px;
  background: v-bind(clr);
  border-radius: 18px;
  transition: 0.3s;
}
</style>
