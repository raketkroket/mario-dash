<script setup lang="ts">
import {
  Chart as ChartJS,
  CategoryScale,
  LinearScale,
  BarElement,
  Tooltip,
} from 'chart.js'

import { Bar } from 'vue-chartjs'

ChartJS.register(
  CategoryScale,
  LinearScale,
  BarElement,
  Tooltip
)

const data = [
  { tech: 'React', count: 187 },
  { tech: 'TypeScript', count: 162 },
  { tech: 'Node.js', count: 148 },
  { tech: 'Python', count: 124 },
  { tech: 'Vue.js', count: 98 },
  { tech: 'Java', count: 87 },
  { tech: 'Docker', count: 76 },
  { tech: 'AWS', count: 71 },
  { tech: 'GraphQL', count: 54 },
  { tech: 'Kubernetes', count: 43 },
]

const colors = [
  '#0ea5e9',
  '#38bdf8',
  '#7dd3fc',
  '#0284c7',
  '#0369a1',
  '#8b5cf6',
  '#a78bfa',
  '#10b981',
  '#34d399',
  '#6ee7b7',
]

const chartData = {
  labels: data.map((i) => i.tech),

  datasets: [
    {
      data: data.map((i) => i.count),
      backgroundColor: colors,
      borderRadius: 6,
      borderSkipped: false,
      barThickness: 18,
    },
  ],
}

const options = {
  responsive: true,
  maintainAspectRatio: false,
  indexAxis: 'y',

  plugins: {
    legend: {
      display: false,
    },

    tooltip: {
      backgroundColor: '#fff',
      titleColor: '#0f172a',
      bodyColor: '#475569',
      borderColor: '#e2e8f0',
      borderWidth: 1,
      padding: 10,

      callbacks: {
        label(context: any) {
          return `${context.raw} vacatures`
        },
      },
    },
  },

  scales: {
    x: {
      grid: {
        color: '#f1f5f9',
      },

      border: {
        display: false,
      },

      ticks: {
        color: '#94a3b8',
        font: {
          size: 11,
        },
      },
    },

    y: {
      grid: {
        display: false,
      },

      border: {
        display: false,
      },

      ticks: {
        color: '#64748b',
        font: {
          size: 11,
        },
      },
    },
  },
}
</script>

<template>
  <div class="bg-white border border-slate-200 rounded-xl p-6">

    <div class="flex items-center justify-between mb-6">

      <div>
        <h2
          class="font-heading font-bold text-slate-800 text-base"
        >
          Meest gevraagde technologieën
        </h2>

        <p
          class="text-xs text-slate-500 mt-0.5"
        >
          Aantal vacatures per technologie
        </p>
      </div>

      <span
        class="text-xs text-slate-400 bg-slate-100 px-2 py-1 rounded-md font-mono"
      >
        Bron: CBS
      </span>

    </div>

    <div class="h-[260px]">
      <Bar
        :data="chartData"
        :options="options"
      />
    </div>

  </div>
</template>