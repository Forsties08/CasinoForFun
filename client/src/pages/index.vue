<template>
  <div class="px-10 m-auto">
    <Blackjack />
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import { useI18n } from 'vue-i18n'

const name = ref('')

const router = useRouter()
const go = () => {
  if (name.value) router.push(`/hi/${name.value}`)
}

const { t } = useI18n()
</script>
