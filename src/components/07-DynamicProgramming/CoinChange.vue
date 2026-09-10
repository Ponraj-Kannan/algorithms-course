<script setup>
import CoinChangeDP from './CoinChangeDP.vue';
defineProps({
  topic: { type: String, default: 'Dynamic Programming' },
  subTopic: { type: String, default: 'Coin Change (LeetCode 322)' }
});
</script>

<template>
  <CoinChangeDP :topic="topic" :subTopic="subTopic" />
</template>
