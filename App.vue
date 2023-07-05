<script setup lang="ts">
import { BurstTimer, ComponentB } from "./src/components";

const onTimesUp = () => {
    console.log("times up")
};
</script>

<template>
    <BurstTimer time="20" @timesup="onTimesUp" />
    <ComponentB />
</template>

<style scoped></style>
