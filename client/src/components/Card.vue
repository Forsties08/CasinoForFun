<template>
  <div
    class="inline-block w-32 h-56 m-auto text-3xl font-bold transfor card"
    :class="'text-' + color + (scale ? ' transform origin-' + origin + ' ' + scale : '')"
    @click="toggle"
  >
    <div class="w-full h-full bg-white border border-gray-200 rounded-lg card-inner">
      <div class="grid grid-cols-1 front-face place-content-between">
        <div class="grid grid-cols-2 text-left">
          <span class="ml-1">{{ symbol }}</span>
          <div class="mt-2 text-right">
            <fxemoji-blackdiamondsuit v-if="color === 'diamond'" class="inline-block" />
            <fxemoji-blackspadesuit v-if="color === 'spade'" class="inline-block -mt-3" />
            <fxemoji-blackclubsuit v-if="color === 'club'" class="inline-block -mt-3" />
            <fxemoji-blackheartsuit v-if="color === 'heart'" class="inline-block -mt-3" />
          </div>
        </div>
        <fxemoji-blackdiamondsuit v-if="color === 'diamond'" class="inline-block m-auto text-6xl" />
        <fxemoji-blackspadesuit v-if="color === 'spade'" class="inline-block m-auto text-6xl" />
        <fxemoji-blackclubsuit v-if="color === 'club'" class="inline-block m-auto text-6xl" />
        <fxemoji-blackheartsuit v-if="color === 'heart'" class="inline-block m-auto text-6xl" />
        <div class="grid grid-cols-2 text-left transform rotate-180">
          <span class="ml-1">{{ symbol }}</span>
          <div class="mt-2 text-right">
            <fxemoji-blackdiamondsuit v-if="color === 'diamond'" class="inline-block" />
            <fxemoji-blackspadesuit v-if="color === 'spade'" class="inline-block -mt-3" />
            <fxemoji-blackclubsuit v-if="color === 'club'" class="inline-block -mt-3" />
            <fxemoji-blackheartsuit v-if="color === 'heart'" class="inline-block -mt-3" />
          </div>
        </div>
      </div>

      <div class="p-1 bg-white rounded-lg h-80 back-face">
        <div class="w-full h-full bg-black rounded-lg bg-opacity-80" />
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { defineProps } from 'vue'

const props = defineProps({
  color: {
    type: String,
    require: true,
  },
  symbol: {
    type: String,
    require: true,
  },
  scale: {
    type: String,
    require: false,
  },
  origin: {
    type: String,
    require: false,
    default: 'top left',
  },
})

const toggle = (e) => {
  const card = e.target.closest('.card')
  card.classList.toggle('flipped')

  console.log('flipped: ', card.classList.contains('flipped'))
}
</script>
