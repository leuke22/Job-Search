<script setup lang="ts">
import BrowseJobs from '@/components/main/BrowseJobs.vue'
import HomeCard from '@/components/main/HomeCard.vue'
import MainHero from '@/components/main/MainHero.vue'
</script>

<template>
  <MainHero />
  <HomeCard />
  <BrowseJobs :limit="3" :showButton="true" />
</template>
