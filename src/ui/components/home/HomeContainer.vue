<script setup lang="ts">
import { useCounterStore } from '@/ui/stores/counter.store'
import HeaderSection from './HeaderSection.vue'
import IntroductionSection from './IntroductionSection.vue'
import SearchBar from './SearchBar.vue'

const counterStore = useCounterStore()
</script>

<template>
  <HeaderSection />
    <IntroductionSection />
    <SearchBar />
</template>
