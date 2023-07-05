<script setup lang="ts">
import { ref } from "vue";

const count = ref(0);
</script>

<template>
  <div class="flex align-content-center flex-wrap counter">
    <label class="flex align-items-center justify-content-center">Counter:</label>
    <button @click="count++">++</button>
    <span class="flex align-items-center justify-content-center count">{{
      count
    }}</span>
    <button @click="count--">--</button>
  </div>
</template>

<style lang="scss" scoped>
.counter {
  padding: 30px;
  background-color: rgb(189, 241, 224);

  label {
    padding-right: 10px;
    font-weight: bold;
  }

  .count {
    padding: 0 10px;
  }
}
</style>
