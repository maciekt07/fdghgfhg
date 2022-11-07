<script setup>
import Footer from "./components/Footer.vue";
import Message from "./components/Message.vue";
import Points from "./components/Points.vue";
import { ref } from "vue";

let r = ref(Number);
let g = ref(Number);
let b = ref(Number);
let points = ref(0);
let start = ref(false);
let messageVisible = ref(false);
let inputDisabled = ref(false);
let tooShort = ref(false);
let clr = ref(String);
let userInput = ref(String);
let message1 = ref(String);
let message2 = ref(String);

const rgbToHex = (r, g, b) => {
  return "#" + ((1 << 24) + (r << 16) + (g << 8) + b).toString(16).slice(1);
};

const random = () => {
  r.value = Math.floor(Math.random() * 256);
  g.value = Math.floor(Math.random() * 256);
  b.value = Math.floor(Math.random() * 256);
  clr.value = `rgb(${r.value}, ${g.value}, ${b.value})`;
  document
    .querySelector('meta[name="theme-color"]')
    .setAttribute("content", clr.value);
  start.value = ref(false);
  messageVisible.value = false;
  inputDisabled.value = false;
  tooShort.value = false;
  userInput.value = "";
  console.log(`Correct answer: ${rgbToHex(r.value, g.value, b.value)}`);
};

const submit = () => {
  const correctAnswer = rgbToHex(r.value, g.value, b.value);
  inputDisabled.value = true;
  messageVisible.value = true;
  if (
    userInput.value == correctAnswer ||
    userInput.value == correctAnswer.toUpperCase()
  ) {
    message1.value = "Answer is correct!";
    return true;
  } else {
    message1.value = "Answer isn't correct! The correct answer was";
    message2.value = rgbToHex(r.value, g.value, b.value);
    return false;
  }
};

const submitCheck = () => {
  console.log(`User Input: ${userInput.value}`);
  if (userInput.value.length == 7) {
    if (
      userInput.value == rgbToHex(r.value, g.value, b.value) ||
      userInput.value == rgbToHex(r.value, g.value, b.value).toUpperCase()
    ) {
      points.value++;
    }
    message2.value = `Points: ${points.value}`;
    submit();
    tooShort.value = false;
  } else {
    tooShort.value = true;
    messageVisible.value = true;
    message1.value = "Answer is too short!";
    message2.value = `${userInput.value.length}/7`;
  }
};

const back = () => {
  start.value = false;
  clr.value = "";
  document
    .querySelector('meta[name="theme-color"]')
    .setAttribute("content", clr.value);
};
</script>
<template>
  <button v-if="start" class="back" @click="back">
    <fa icon="chevron-left" />&nbsp;Back
  </button>
  <div class="container" :class="{ start: !start }">
    <Points v-if="start" :Points="points" />
    <div v-if="start" class="Color"></div>
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
      @keyup.enter="submitCheck"
      :placeholder="!inputDisabled ? 'Enter hex code...' : ''"
    />
    <br />
    <button
      v-if="(start && !messageVisible) || (tooShort && start)"
      @click="submitCheck"
    >
      <fa icon="check" />&nbsp;Submit
    </button>

    <button v-if="(messageVisible && !tooShort) || !start" @click="random">
      <span v-if="!start"> <fa icon="play" /> &nbsp;Start</span>
      <span v-else><fa icon="rotate-right" /> &nbsp;Play Again</span>
    </button>

    <Message
      :Message1="message1"
      :Message2="message2"
      :BorderColor="clr"
      v-if="messageVisible && start"
    />
  </div>

  <Footer v-if="!start" />
</template>
<style lang="scss" scoped>
@use "style" as *;

.back {
  top: 0;
  left: 0;
  position: absolute;
  margin: 20px;
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
  background: #242424;
}
.start {
  height: 250px;
  padding: 30px;
  border: 8px solid $linkColor;
  button {
    margin-bottom: 30px;
    &:hover {
      border-color: $linkColor;
    }
  }

  *::selection {
    background: lighten($color: $linkColor, $amount: 15);
  }
}

.Color {
  width: 100px;
  height: 100px;
  background: v-bind(clr);
  border-radius: 25px;
  transition: 0.3s;
}
</style>
