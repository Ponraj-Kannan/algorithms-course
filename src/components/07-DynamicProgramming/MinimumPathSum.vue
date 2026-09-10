<script setup>
import MinimumPathSumDP from './MinimumPathSumDP.vue';

defineProps({
  topic: { type: String, default: 'Dynamic Programming' },
  subTopic: { type: String, default: 'Minimum Path Sum' }
});
</script>

<template>
  <MinimumPathSumDP :topic="topic" :subTopic="subTopic" />
</template>
