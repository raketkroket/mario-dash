<script setup lang="ts">
import { ref } from 'vue'
import { Motion } from 'motion-v'
import { ExternalLink } from 'lucide-vue-next'

import BedrijfDetailModal from './BedrijfDetailModal.vue'
import type { Bedrijf } from './BedrijfDetailModal.vue'

const selected = ref<Bedrijf | null>(null)

const bedrijven: Bedrijf[] = [
  { id: 1, naam: 'Accenture Almere', type: 'Consultancy', stad: 'Almere', vacatures: 34, medewerkers: 420, groei: 22, techStack: ['React', 'Java', 'AWS'], opgericht: 2008 },
  { id: 2, naam: 'Capgemini', type: 'IT Services', stad: 'Lelystad', vacatures: 28, medewerkers: 310, groei: 15, techStack: ['Python', 'Azure', 'Node.js'], opgericht: 2001 },
  { id: 3, naam: 'Sogeti', type: 'IT Services', stad: 'Almere', vacatures: 24, medewerkers: 280, groei: 18, techStack: ['Java', 'Docker', 'TypeScript'], opgericht: 2005 },
  { id: 4, naam: 'Gemeente Almere', type: 'Overheid', stad: 'Almere', vacatures: 19, medewerkers: 3200, groei: 8, techStack: ['Vue.js', 'PHP', 'PostgreSQL'], opgericht: 1984 },
  { id: 5, naam: 'Wehkamp', type: 'E-commerce', stad: 'Almere', vacatures: 17, medewerkers: 1100, groei: 31, techStack: ['React', 'Kotlin', 'Kubernetes'], opgericht: 1952 },
  { id: 6, naam: 'Flevoland ICT', type: 'IT Services', stad: 'Emmeloord', vacatures: 14, medewerkers: 95, groei: 12, techStack: ['PHP', 'MySQL', 'Vue.js'], opgericht: 2012 },
  { id: 7, naam: 'Provincie Flevoland', type: 'Overheid', stad: 'Lelystad', vacatures: 11, medewerkers: 850, groei: 5, techStack: ['Angular', 'Java', 'Oracle'], opgericht: 1986 },
  { id: 8, naam: 'Lelystad Airport', type: 'Transport', stad: 'Lelystad', vacatures: 9, medewerkers: 240, groei: 44, techStack: ['Python', 'React', 'AWS'], opgericht: 1990 },
]

const typeColors: Record<string, string> = {
  Consultancy: 'bg-sky-100 text-sky-700',
  'IT Services': 'bg-violet-100 text-violet-700',
  Overheid: 'bg-amber-100 text-amber-700',
  'E-commerce': 'bg-emerald-100 text-emerald-700',
  Transport: 'bg-rose-100 text-rose-700',
}
</script>

<template>
  <div class="bg-white border border-slate-200 rounded-xl p-6">
    <div class="flex items-center justify-between mb-6">
      <div>
        <h2 class="font-heading font-bold text-slate-800 text-base">
          Top werkgevers
        </h2>

        <p class="text-xs text-slate-500 mt-0.5">
          Meeste openstaande developer vacatures
        </p>
      </div>
    </div>

    <div class="space-y-3">
      <Motion
        v-for="(b, i) in bedrijven"
        :key="b.naam"
        as="button"
        :initial="{ opacity: 0, x: -12 }"
        :animate="{ opacity: 1, x: 0 }"
        :transition="{ delay: i * 0.06 }"
        class="w-full text-left flex items-center gap-3 p-3 rounded-lg hover:bg-slate-50 transition-all duration-200 group focus:outline-none focus:ring-2 focus:ring-sky-500/40"
        @click="selected = b"
      >
        <span class="text-xs font-mono text-slate-400 w-5 shrink-0">
          {{ String(i + 1).padStart(2, '0') }}
        </span>

        <div
          class="w-8 h-8 rounded-lg bg-gradient-to-br from-slate-100 to-slate-200 flex items-center justify-center text-xs font-bold text-slate-600 shrink-0"
        >
          {{ b.naam.charAt(0) }}
        </div>

        <div class="flex-1 min-w-0">
          <p class="text-sm font-semibold text-slate-700 truncate">
            {{ b.naam }}
          </p>

          <span
            :class="[
              'text-[10px] px-1.5 py-0.5 rounded font-medium',
              typeColors[b.type] ?? 'bg-slate-100 text-slate-600',
            ]"
          >
            {{ b.type }}
          </span>
        </div>

        <div class="text-right shrink-0">
          <p class="text-sm font-bold text-slate-800">
            {{ b.vacatures }}
          </p>

          <p class="text-[10px] text-emerald-600 font-medium">
            +{{ b.groei }}%
          </p>
        </div>

        <ExternalLink
          :size="14"
          class="text-slate-300 group-hover:text-sky-500 transition-colors duration-200 shrink-0"
        />
      </Motion>
    </div>

    <BedrijfDetailModal
      :bedrijf="selected"
      @close="selected = null"
    />
  </div>
</template>