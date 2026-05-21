<script setup>
import Sidebar from './Sidebar.vue'
import TopBar from './TopBar.vue'

// Props
defineProps({
  title: {
    type: String,
    required: true
  },
  subtitle: {
    type: String,
    default: ''
  }
})
</script>

<template>
  <div class="flex min-h-screen bg-[hsl(220,20%,97%)]">
    <Sidebar />

    <div class="flex-1 flex flex-col min-w-0">
      <TopBar :title="title" :subtitle="subtitle" />

      <main class="flex-1 p-6 overflow-auto">
        <slot />
      </main>
    </div>
  </div>
</template>
