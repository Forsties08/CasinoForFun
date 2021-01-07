<template>
  <ul class="top-0 left-0 w-40 card-stack">
    <li
      v-for="n in 50"
      :key="n"
      :style="{ 'margin-top': n / 40 + 'em' }"
      class="w-32 h-56 bg-white border rounded-lg"
      :class="n % 2 === 0 ? 'border-gray-200' : 'border-gray-100'"
    />
    <li class="w-32 h-56 border border-gray-200 rounded-lg" :style="{ 'margin-top': 50 / 40 + 'em' }" />
  </ul>
</template>

<script setup lang="ts">
import { defineProps } from 'vue'

const props = defineProps({
  stack: {
    type: Number,
    require: true,
  },
})

console.log(props.stack)
</script>
