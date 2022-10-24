<script setup>
defineProps({
  Points: Number,
});
</script>
<template>
  <div v-if="Points > 0">Points: {{ Points }}</div>
</template>

<style lang="scss" scoped>
@use "../style.scss" as *;
div {
  transition: 0.3s;
  text-align: left;
  font-size: 18px;
  margin-right: 350px;
  background: $inputBackground;
  border-radius: 6px;
  padding: 8px;
}
</style>
