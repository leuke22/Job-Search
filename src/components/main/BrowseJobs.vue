<script setup lang="ts">
import JobsCard from '../cards/JobsCard.vue'
import { reactive, defineProps, onMounted } from 'vue'
import { RouterLink } from 'vue-router'
// @ts-expect-error: No type declarations available for vue-spinner PulseLoader
import PulseLoader from 'vue-spinner/src/PulseLoader.vue'

import axios from 'axios'

interface Job {
  id: number
  title: string
  type: string
  description: string
  location: string
  salary: string
  company: {
    name: string
    description: string
    contactEmail: string
    contactPhone: string
  }
}

const state = reactive<{
  jobs: Job[]
  isLoading: boolean
}>({
  jobs: [],
  isLoading: true,
})

defineProps<{
  limit?: number
  showButton?: boolean
}>()

onMounted(async () => {
  try {
    const response = await axios.get('/api/jobs')
    state.jobs = response.data
  } catch (error) {
    console.log('Error Fetching Data', error)
  } finally {
    state.isLoading = false
  }
})
</script>

<template>
  <section class="bg-green-50 px-4 py-10">
    <div class="container-xl lg:container m-auto">
      <h2 class="text-3xl font-bold text-green-500 mb-6 text-center">Browse Jobs</h2>
      <div v-if="state.isLoading" class="text-center text-gray-500 py-6">
        <PulseLoader />
      </div>
      <div v-else class="grid grid-cols-1 md:grid-cols-3 gap-6">
        <JobsCard
          v-for="job in state.jobs.slice(0, limit || state.jobs.length)"
          :key="job.id"
          :job="job"
        />
      </div>
    </div>
  </section>

  <section v-if="showButton" class="m-auto max-w-lg my-10 px-6">
    <RouterLink
      to="/jobs"
      class="block bg-black text-white text-center py-4 px-6 rounded-xl hover:bg-gray-700"
      >View All Jobs</RouterLink
    >
  </section>
</template>
