<script setup>
defineProps({
  Message1: String,
  Message2: String,
  BorderColor: String,
});
</script>
<template>
  <p>
    {{ Message1 }} <br />
    <span v-if="Message2 != ''">
      {{ Message2 }}
    </span>
  </p>
</template>

<style lang="scss" scoped>
@use "../style.scss" as *;

p {
  background: rgb(77, 77, 77);
  padding: 12px;
  border-radius: 8px;
  line-height: 30px;
  border-left: 5px solid v-bind(BorderColor);
}
span {
  padding: 4px;
  border-radius: 4px;
  background: gray;
  margin-top: 25px;
  &::selection {
    background: v-bind(BorderColor);
  }
}
</style>
