<script setup>
import CoinChange2DP from './CoinChange2DP.vue';
defineProps({
  topic: { type: String, default: 'Dynamic Programming' },
  subTopic: { type: String, default: 'Coin Change II (Number of Ways)' }
});
</script>

<template>
  <CoinChange2DP :topic="topic" :subTopic="subTopic" />
</template>
