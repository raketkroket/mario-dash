<script setup lang="ts">
import {
  Chart as ChartJS,
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Filler,
  Tooltip,
  Legend,
} from 'chart.js'

import { Line } from 'vue-chartjs'

ChartJS.register(
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Filler,
  Tooltip,
  Legend
)

const data = [
  { maand: 'Jan', Frontend: 42, Backend: 58, API: 24 },
  { maand: 'Feb', Frontend: 48, Backend: 62, API: 28 },
  { maand: 'Mrt', Frontend: 55, Backend: 70, API: 32 },
  { maand: 'Apr', Frontend: 51, Backend: 65, API: 30 },
  { maand: 'Mei', Frontend: 63, Backend: 78, API: 38 },
  { maand: 'Jun', Frontend: 72, Backend: 85, API: 44 },
  { maand: 'Jul', Frontend: 68, Backend: 80, API: 41 },
  { maand: 'Aug', Frontend: 74, Backend: 88, API: 47 },
  { maand: 'Sep', Frontend: 82, Backend: 95, API: 52 },
  { maand: 'Okt', Frontend: 88, Backend: 102, API: 58 },
  { maand: 'Nov', Frontend: 91, Backend: 108, API: 61 },
  { maand: 'Dec', Frontend: 96, Backend: 115, API: 65 },
]

const chartData = {
  labels: data.map((item) => item.maand),

  datasets: [
    {
      label: 'Frontend',
      data: data.map((item) => item.Frontend),
      borderColor: '#0ea5e9',
      backgroundColor: 'rgba(14, 165, 233, 0.15)',
      borderWidth: 2,
      pointRadius: 0,
      tension: 0.4,
      fill: true,
    },
    {
      label: 'Backend',
      data: data.map((item) => item.Backend),
      borderColor: '#8b5cf6',
      backgroundColor: 'rgba(139, 92, 246, 0.15)',
      borderWidth: 2,
      pointRadius: 0,
      tension: 0.4,
      fill: true,
    },
    {
      label: 'API',
      data: data.map((item) => item.API),
      borderColor: '#10b981',
      backgroundColor: 'rgba(16, 185, 129, 0.15)',
      borderWidth: 2,
      pointRadius: 0,
      tension: 0.4,
      fill: true,
    },
  ],
}

const options = {
  responsive: true,
  maintainAspectRatio: false,

  plugins: {
    legend: {
      position: 'bottom',
      labels: {
        boxWidth: 10,
        boxHeight: 10,
        font: {
          size: 12,
        },
      },
    },

    tooltip: {
      backgroundColor: '#fff',
      titleColor: '#0f172a',
      bodyColor: '#475569',
      borderColor: '#e2e8f0',
      borderWidth: 1,
      padding: 10,
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
  },
}
</script>

<template>
  <div class="bg-white border border-slate-200 rounded-xl p-6">
    <div class="flex items-center justify-between mb-6">
      <div>
        <h2 class="font-heading font-bold text-slate-800 text-base">
          Vacatureontwikkeling 2025
        </h2>

        <p class="text-xs text-slate-500 mt-0.5">
          Openstaande vacatures per maand
        </p>
      </div>

      <span
        class="text-xs text-slate-400 bg-slate-100 px-2 py-1 rounded-md font-mono"
      >
        Bron: UWV
      </span>
    </div>

    <div class="h-[260px]">
      <Line
        :data="chartData"
        :options="options"
      />
    </div>
  </div>
</template>