<script setup lang="ts">
import { computed } from 'vue'
import { Motion } from 'motion-v'

import {
  TrendingUp,
  TrendingDown,
  Minus,
} from 'lucide-vue-next'

interface Props {
  title: string
  value: string
  change?: number
  changeLabel?: string
  icon: any
  color: 'sky' | 'emerald' | 'violet' | 'amber' | 'rose'
  delay?: number
}

const props = withDefaults(
  defineProps<Props>(),
  {
    delay: 0,
  }
)

const colorMap = {
  sky: {
    icon: 'bg-sky-100 text-sky-600',
  },
  emerald: {
    icon: 'bg-emerald-100 text-emerald-600',
  },
  violet: {
    icon: 'bg-violet-100 text-violet-600',
  },
  amber: {
    icon: 'bg-amber-100 text-amber-600',
  },
  rose: {
    icon: 'bg-rose-100 text-rose-600',
  },
}

const c = computed(() => colorMap[props.color])

const isPositive = computed(
  () =>
    props.change !== undefined &&
    props.change > 0
)

const isNegative = computed(
  () =>
    props.change !== undefined &&
    props.change < 0
)
</script>

<template>
  <Motion
    :initial="{ opacity: 0, y: 20 }"
    :animate="{ opacity: 1, y: 0 }"
    :transition="{
      duration: 0.4,
      delay,
    }"
    class="bg-white border border-slate-200 rounded-xl p-6 hover:shadow-md hover:-translate-y-0.5 transition-all duration-300"
  >
    <div class="flex items-start justify-between mb-4">

      <div
        :class="[
          'p-2.5 rounded-lg',
          c.icon,
        ]"
      >
        <component
          :is="icon"
          :size="20"
        />
      </div>

      <div
        v-if="change !== undefined"
        :class="[
          'flex items-center gap-1 px-2 py-1 rounded-full text-xs font-semibold',
          isPositive
            ? 'bg-emerald-100 text-emerald-700'
            : isNegative
            ? 'bg-rose-100 text-rose-700'
            : 'bg-slate-100 text-slate-600',
        ]"
      >
        <TrendingUp
          v-if="isPositive"
          :size="11"
        />

        <TrendingDown
          v-else-if="isNegative"
          :size="11"
        />

        <Minus
          v-else
          :size="11"
        />

        {{ Math.abs(change) }}%
      </div>

    </div>

    <p
      class="text-2xl font-bold text-slate-800 font-heading mb-1"
    >
      {{ value }}
    </p>

    <p class="text-sm text-slate-500">
      {{ title }}
    </p>

    <p
      v-if="changeLabel"
      class="text-xs text-slate-400 mt-1"
    >
      {{ changeLabel }}
    </p>
  </Motion>
</template>