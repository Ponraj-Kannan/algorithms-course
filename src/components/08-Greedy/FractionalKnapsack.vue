<script setup>
import FractionalKnapsackProblem from './FractionalKnapsackProblem.vue';

defineProps({
  topic: { type: String, default: 'Greedy Algorithms' },
  subTopic: { type: String, default: 'Fractional Knapsack Problem' }
});
</script>

<template>
  <FractionalKnapsackProblem :topic="topic" :subTopic="subTopic" />
</template>
