<script setup>
import { Database, Clock } from 'lucide-vue-next'

const props = defineProps({
  source: { type: String, required: true },
  lastUpdated: { type: String, required: true },
  status: {
    type: String,
    default: 'live',
    validator: (v) => ['live', 'cached', 'error'].includes(v)
  }
})

const statusConfig = {
  live: { color: 'text-emerald-600', dot: 'bg-emerald-400', label: 'Live' },
  cached: { color: 'text-amber-600', dot: 'bg-amber-400', label: 'Gecached' },
  error: { color: 'text-rose-600', dot: 'bg-rose-400', label: 'Fout' }
}

const s = statusConfig[props.status]
</script>

<template>
  <div class="flex flex-wrap items-center gap-4 text-xs text-slate-500 bg-slate-50 border border-slate-200 rounded-lg px-4 py-2.5">
    
    <!-- Source -->
    <div class="flex items-center gap-1.5">
      <Database :size="13" class="text-slate-400" />
      <span>
        Bron:
        <span class="font-semibold text-slate-700">{{ source }}</span>
      </span>
    </div>

    <div class="w-px h-3 bg-slate-200 hidden sm:block" />

    <!-- Last updated -->
    <div class="flex items-center gap-1.5">
      <Clock :size="13" class="text-slate-400" />
      <span>
        Update:
        <span class="font-semibold text-slate-700">{{ lastUpdated }}</span>
      </span>
    </div>

    <div class="w-px h-3 bg-slate-200 hidden sm:block" />

    <!-- Status -->
    <div class="flex items-center gap-1.5">
      <span
        class="w-1.5 h-1.5 rounded-full"
        :class="[s.dot, props.status === 'live' ? 'animate-pulse' : '']"
      />
      <span :class="['font-semibold', s.color]">
        {{ s.label }}
      </span>
    </div>

  </div>
</template>
