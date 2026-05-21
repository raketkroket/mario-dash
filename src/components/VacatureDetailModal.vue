<script setup lang="ts">
import { onMounted, onUnmounted } from 'vue'
import { Motion, AnimatePresence } from 'motion-v'
import { useToast } from 'vue-toastification'

import {
  X,
  MapPin,
  Clock,
  Briefcase,
  Euro,
  Building2,
  CheckCircle,
  Send,
} from 'lucide-vue-next'

export interface Vacature {
  id: number
  titel: string
  bedrijf: string
  stad: string
  type: string
  niveau: string
  salaris: string
  geplaatst: string
  remote: boolean
}

interface Props {
  vacature: Vacature | null
}

const props = defineProps<Props>()

const emit = defineEmits<{
  close: []
}>()

const toast = useToast()

const close = () => emit('close')

const handler = (e: KeyboardEvent) => {
  if (e.key === 'Escape') {
    close()
  }
}

onMounted(() => {
  window.addEventListener('keydown', handler)
})

onUnmounted(() => {
  window.removeEventListener('keydown', handler)
})

const handleApply = () => {
  if (!props.vacature) return

  toast.success(
    `Sollicitatie verstuurd naar ${props.vacature.bedrijf}`
  )

  close()
}

const handleSave = () => {
  toast.success(
    'Vacature opgeslagen in jouw favorieten'
  )
}

const highlights = [
  'Ontwerpen en bouwen van schaalbare applicaties',
  'Samenwerken met designers, product owners en collega-developers',
  'Code reviews uitvoeren en kwaliteit bewaken',
  'Bijdragen aan technische roadmap en architectuur',
]
</script>

<template>
  <AnimatePresence>

    <Motion
      v-if="vacature"
      :initial="{ opacity: 0 }"
      :animate="{ opacity: 1 }"
      :exit="{ opacity: 0 }"
      class="fixed inset-0 bg-slate-900/50 backdrop-blur-sm z-50 flex items-center justify-center p-4"
      @click="close"
    >

      <Motion
        :initial="{
          opacity: 0,
          scale: 0.95,
          y: 20,
        }"

        :animate="{
          opacity: 1,
          scale: 1,
          y: 0,
        }"

        :exit="{
          opacity: 0,
          scale: 0.95,
          y: 20,
        }"

        :transition="{
          duration: 0.2,
        }"

        class="bg-white rounded-2xl shadow-2xl max-w-2xl w-full max-h-[90vh] overflow-auto"

        @click.stop
      >

        <!-- Header -->

        <div
          class="sticky top-0 bg-white border-b border-slate-200 px-6 py-4 flex items-start justify-between gap-4"
        >

          <div class="flex items-start gap-3 flex-1 min-w-0">

            <div
              class="w-12 h-12 rounded-xl bg-gradient-to-br from-sky-100 to-sky-200 flex items-center justify-center text-base font-bold text-sky-700 shrink-0"
            >
              {{ vacature.bedrijf.charAt(0) }}
            </div>

            <div class="flex-1 min-w-0">

              <h2
                class="font-heading font-bold text-slate-800 text-lg leading-tight"
              >
                {{ vacature.titel }}
              </h2>

              <p
                class="text-sm text-slate-500 mt-0.5"
              >
                {{ vacature.bedrijf }}
              </p>

            </div>

          </div>

          <button
            class="p-2 rounded-lg text-slate-400 hover:text-slate-700 hover:bg-slate-100"
            @click="close"
          >
            <X :size="18" />
          </button>

        </div>

        <!-- Content -->

        <div class="px-6 py-5 space-y-5">

          <div
            class="grid grid-cols-2 md:grid-cols-4 gap-3"
          >

            <div
              class="bg-slate-50 border border-slate-100 rounded-lg p-3"
            >

              <MapPin
                :size="14"
                class="text-sky-600 mb-1.5"
              />

              <p
                class="text-[10px] text-slate-500 uppercase"
              >
                Locatie
              </p>

              <p
                class="text-sm font-semibold mt-0.5"
              >
                {{ vacature.stad }}
              </p>

            </div>

            <div
              class="bg-slate-50 border border-slate-100 rounded-lg p-3"
            >

              <Briefcase
                :size="14"
                class="text-sky-600 mb-1.5"
              />

              <p class="text-[10px]">
                Type
              </p>

              <p class="text-sm font-semibold">
                {{ vacature.type }}
              </p>

            </div>

            <div
              class="bg-slate-50 border border-slate-100 rounded-lg p-3"
            >

              <Euro
                :size="14"
                class="text-sky-600 mb-1.5"
              />

              <p class="text-[10px]">
                Salaris
              </p>

              <p class="text-sm font-semibold">
                {{ vacature.salaris }}
              </p>

            </div>

            <div
              class="bg-slate-50 border border-slate-100 rounded-lg p-3"
            >

              <Clock
                :size="14"
                class="text-sky-600 mb-1.5"
              />

              <p class="text-[10px]">
                Geplaatst
              </p>

              <p class="text-sm font-semibold">
                {{ vacature.geplaatst }}
              </p>

            </div>

          </div>

          <div class="flex flex-wrap gap-2">

            <span class="text-xs px-2.5 py-1 rounded-full bg-sky-100 text-sky-700">
              {{ vacature.type }}
            </span>

            <span class="text-xs px-2.5 py-1 rounded-full bg-violet-100 text-violet-700">
              {{ vacature.niveau }}
            </span>

            <span
              v-if="vacature.remote"
              class="text-xs px-2.5 py-1 rounded-full bg-teal-100 text-teal-700"
            >
              Remote mogelijk
            </span>

          </div>

          <section>

            <h3 class="font-bold mb-2">
              Functieomschrijving
            </h3>

            <p class="text-sm text-slate-600">

              Als
              {{ vacature.titel.toLowerCase() }}
              bij
              {{ vacature.bedrijf }}
              werk je aan moderne software.

            </p>

          </section>

          <section>

            <h3 class="font-bold mb-2">
              Wat je gaat doen
            </h3>

            <ul class="space-y-2">

              <li
                v-for="item in highlights"
                :key="item"
                class="flex gap-2"
              >

                <CheckCircle
                  :size="14"
                  class="text-emerald-500 mt-0.5"
                />

                {{ item }}

              </li>

            </ul>

          </section>

          <section
            class="bg-sky-50 border border-sky-100 rounded-lg p-4 flex gap-3"
          >

            <Building2
              :size="16"
              class="text-sky-600 mt-0.5"
            />

            <div>

              <p class="font-semibold">
                Over {{ vacature.bedrijf }}
              </p>

              <p class="text-xs text-slate-600 mt-1">
                Toonaangevende werkgever met focus op innovatie.
              </p>

            </div>

          </section>

        </div>

        <!-- Footer -->

        <div
          class="sticky bottom-0 bg-white border-t border-slate-200 px-6 py-4 flex justify-between"
        >

          <button
            class="px-4 py-2.5 border rounded-lg"
            @click="handleSave"
          >
            Opslaan
          </button>

          <button
            class="flex items-center gap-2 px-5 py-2.5 bg-sky-600 text-white rounded-lg hover:bg-sky-700"
            @click="handleApply"
          >
            <Send :size="14" />
            Solliciteer direct
          </button>

        </div>

      </Motion>

    </Motion>

  </AnimatePresence>
</template>