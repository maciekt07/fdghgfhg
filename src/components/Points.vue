<script setup>
defineProps({
  Points: Number,
});
</script>
<template>
  <h1 v-if="Points > 0" class="points">Points: {{ Points }}</h1>
</template>

<style lang="scss" scoped>
.points {
  text-align: left;
  font-size: 18px;
  margin-right: 350px;
  background: rgb(77, 77, 77);
  border-radius: 6px;
  padding: 8px;
}
</style>
