<script setup lang="ts">
import {
  BalApp,
  BalNavbar,
  BalNavbarBrand,
  BalIcon,
  BalText,
  BalNavbarMenu,
  BalNavbarMenuStart,
  BalFooter,
} from '@baloise/design-system-components-vue'
import { onMounted } from 'vue'
import { useI18nStore } from './stores/i18n.store'

const i18nStore = useI18nStore()

const hideLinks = import.meta.env.MODE === 'e2e'

onMounted(() => i18nStore.init())
</script>

<template>
  <BalApp class="has-sticky-footer has-background">
    <main className="container-fluid">
      <router-view />
    </main>
  </BalApp>
</template>
