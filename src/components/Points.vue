<script setup>
defineProps({
  Points: Number,
});
</script>
<template>
  <div v-if="Points > 0">Points: {{ Points }}</div>
</template>

<style lang="scss" scoped>
div {
  text-align: left;
  font-size: 18px;
  margin-right: 350px;
  background: rgb(77, 77, 77);
  border-radius: 6px;
  padding: 8px;
}
</style>
