<template>
  <nav class="mt-6 text-xl">
    <router-link class="mx-2 icon-btn" to="/" :title="t('button.home')">
      <carbon-campsite />
    </router-link>

    <a class="mx-2 icon-btn" :title="t('button.toggle_dark')" @click="isDark = !isDark">
      <carbon-moon v-if="isDark" />
      <carbon-sun v-else />
    </a>

    <a class="mx-2 icon-btn" :title="t('button.toggle_langs')" @click="toggleLocales">
      <carbon-language />
    </a>

    <router-link class="mx-2 icon-btn" to="/about" :title="t('button.about')">
      <carbon-dicom-overlay />
    </router-link>

    <a class="mx-2 icon-btn" rel="noreferrer" href="https://github.com/antfu/vitesse" target="_blank" title="GitHub">
      <carbon-logo-github />
    </a>
  </nav>
</template>

<script setup lang="ts">
import { useI18n } from 'vue-i18n'
import { locales } from '/~/messages'
import { isDark } from '/~/logics'

const i18n = useI18n()

const toggleLocales = () => {
  // change to some real logic
  i18n.locale.value = locales[(locales.indexOf(i18n.locale.value) + 1) % locales.length]
}

const t = i18n.t
</script>
