<template>
  <!-- <Card color="heart" symbol="K" scale="scale-75" /> -->

  <div class="grid h-screen grid-cols-1 grid-rows-3">
    <div id="dealer" class="text-right">
      <CardStack count="40" class="mt-5 ml-auto" />
    </div>
    <div><a id="start" href="#" class="text-blue-400" @click="start">START</a></div>
    <div id="players" class="grid grid-cols-5 gap-10">
      <div v-for="(player, playerIndex) in players" :key="playerIndex" class="relative grid grid-cols-12">
        <div class="absolute inset-x-0 bottom-0 z-50 font-bold text-white">Forsties08</div>
        <div
          v-for="(item, index) in player.hands"
          :key="index"
          class="relative col-span-6 h-60"
          :class="player.hands.length <= 1 ? 'col-start-4' : ''"
        >
          <!-- CLASS TO ADD AFTER ANIMATION: inset-x-0 & -top-<14*index> -->
          <div
            v-for="(card, i) in item"
            :id="'card-' + playerIndex + '-' + index + '-' + i"
            :key="i"
            class="absolute inset-x-0 z-10 invisible player-card"
            :style="{ top: -(3.5 * i) + 'rem' }"
          >
            <Card :symbol="card[0]" :color="card[1]" class="flipped" />
          </div>
          <div class="absolute inset-x-0 z-50 font-bold text-white -bottom-16">20</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, nextTick } from 'vue'

const players = ref([
  {
    name: 'Player #1',
    hands: [
      [
        ['A', 'diamond'],
        ['J', 'diamond'],
        ['J', 'diamond'],
      ],
      [['K', 'diamond']],
    ],
  },
  {
    name: 'Player #2',
    hands: [
      [],
      // [
      //   ['K', 'diamond'],
      //   ['J', 'diamond'],
      // ],
      // [['K', 'diamond']],
    ],
  },
  {
    name: 'Player #3',
    hands: [
      // [['1', 'diamond']]
    ],
  },
  {
    name: 'Player #4',
    hands: [
      // [['2', 'diamond']]
    ],
  },
  {
    name: 'Player #5',
    hands: [
      // [['K', 'diamond']]
    ],
  },
])

function getNumberOrElse(value: unknown, orElse: number) {
  if (!value) return orElse

  if (Number.isFinite(value)) return orElse

  return Number(value)
}

async function goTo(element: HTMLElement, targetX: number, targetY: number): Promise<void> {
  await new Promise<void>((resolve, reject) => {
    let left = getNumberOrElse(element.style.left.replace('px', ''), 0)
    let top = getNumberOrElse(element.style.top.replace('px', ''), 0)

    const interval = setInterval(() => {
      const { x, y } = element.getBoundingClientRect()
      let finished = true

      if (Math.abs(x - targetX) >= 1) {
        finished = false
        left += Math.max(-10, Math.min(targetX - x, 10))
        element.style.left = `${left}px`
      }

      if (Math.abs(y - targetY) >= 1) {
        finished = false
        top += Math.max(-10, Math.min(targetY - y, 10))
        element.style.top = `${top}px`
      }

      if (finished) {
        clearInterval(interval)
        resolve()
      }
    }, 1)
  })
}

function transitionEnd(element: Element): Promise<void> {
  return new Promise<void>((resolve, reject) => {
    element.addEventListener('transitionend', () => {
      resolve()
    })
  })
}

function wait(ms: number): Promise<void> {
  return new Promise<void>((resolve, reject) => {
    setTimeout(() => resolve(), ms)
  })
}

async function addCard(
  playerIndex: number,
  symbol: string,
  color: 'spade' | 'diamond' | 'club' | 'heart'
): Promise<HTMLElement> {
  players.value[playerIndex].hands[0].push([symbol, color])
  const cardIndex = players.value[playerIndex].hands[0].length - 1
  return new Promise<HTMLElement>((resolve, reject) => {
    setTimeout(() => {
      return resolve(document.getElementById(`card-${playerIndex}-0-${cardIndex}`))
    }, 10)
  })
}

const start = async () => {
  const { x, y } = document.querySelector('#dealer .card-stack li:last-child').getBoundingClientRect()
  const card = await addCard(1, 'K', 'spade')

  const { x: cardX, y: cardY } = card.getBoundingClientRect()
  const left = getNumberOrElse(card.style.left.replace('rem', ''), 0)
  const top = getNumberOrElse(card.style.top.replace('rem', ''), 0)

  const leftPixel = left * 16
  const topPixel = top * 16

  card.style.left = `${leftPixel + x - cardX}px`
  card.style.top = `${topPixel + y - cardY}px`

  card.classList.remove('invisible')

  await wait(10)

  card.classList.add('card-animated')

  card.style.left = `${left}rem`
  card.style.top = `${top}rem`

  await transitionEnd(card)

  card.querySelector('.card').classList.remove('flipped')
}
</script>
