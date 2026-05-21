<script setup lang="ts">
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import { Motion, AnimatePresence } from 'motion-v'
import { useToast } from 'vue-toastification'

import {
  Search,
  Bell,
  RefreshCw,
  ChevronDown,
  X,
} from 'lucide-vue-next'

interface Props {
  title: string
  subtitle?: string
}

defineProps<Props>()

const router = useRouter()
const toast = useToast()

const searchValue = ref('')
const showNotifications = ref(false)
const showProfile = ref(false)
const refreshing = ref(false)

const notifications = [
  {
    id: 1,
    titel: 'Nieuwe vacature',
    tekst: '34 nieuwe vacatures gepubliceerd in Almere',
    tijd: '5 min geleden',
    kleur: 'sky',
  },
  {
    id: 2,
    titel: 'Tekort gestegen',
    tekst: 'Backend tekort steeg met +7% deze week',
    tijd: '1 uur geleden',
    kleur: 'amber',
  },
  {
    id: 3,
    titel: 'Data sync',
    tekst: 'UWV API succesvol gesynchroniseerd',
    tijd: '3 uur geleden',
    kleur: 'emerald',
  },
]

const kleurMap: Record<string, string> = {
  sky: 'bg-sky-500',
  amber: 'bg-amber-500',
  emerald: 'bg-emerald-500',
}

const handleRefresh = () => {
  refreshing.value = true

  setTimeout(() => {
    refreshing.value = false
    toast.success('Data succesvol vernieuwd')
  }, 1500)
}

const handleSearch = () => {
  const query = searchValue.value.trim()

  if (!query) return

  router.push(`/vacatures?q=${encodeURIComponent(query)}`)
  toast.info(`Zoeken naar "${query}"`)
}

const toggleNotifications = () => {
  showNotifications.value = !showNotifications.value
  showProfile.value = false
}

const toggleProfile = () => {
  showProfile.value = !showProfile.value
  showNotifications.value = false
}
</script>

<template>
  <header
    class="sticky top-0 z-30 bg-[hsl(220,20%,97%)]/90 backdrop-blur-md border-b border-slate-200 px-6 py-3 flex items-center gap-4"
  >
    <div class="flex-1 min-w-0">
      <h1
        class="font-heading text-slate-800 font-bold text-lg leading-tight truncate"
      >
        {{ title }}
      </h1>

      <p
        v-if="subtitle"
        class="text-xs text-slate-500 mt-0.5"
      >
        {{ subtitle }}
      </p>
    </div>

    <div class="flex items-center gap-3">
      <!-- Search -->
      <form
        class="relative hidden sm:block"
        @submit.prevent="handleSearch"
      >
        <Search
          :size="15"
          class="absolute left-3 top-1/2 -translate-y-1/2 text-slate-400"
        />

        <input
          v-model="searchValue"
          type="search"
          placeholder="Zoek vacatures, bedrijven..."
          class="pl-9 pr-4 py-2 text-sm bg-white border border-slate-200 rounded-lg w-56 focus:outline-none focus:ring-2 focus:ring-sky-500/40 focus:border-sky-400 transition-all duration-200 placeholder:text-slate-400"
        />
      </form>

      <!-- Refresh -->
      <button
        aria-label="Data vernieuwen"
        class="p-2 rounded-lg text-slate-500 hover:text-sky-600 hover:bg-sky-50 transition-all duration-200 focus:outline-none focus:ring-2 focus:ring-sky-500/40"
        @click="handleRefresh"
      >
        <RefreshCw
          :size="16"
          :class="refreshing ? 'animate-spin' : ''"
        />
      </button>

      <!-- Notifications -->
      <div class="relative">
        <button
          aria-label="Meldingen"
          class="relative p-2 rounded-lg text-slate-500 hover:text-sky-600 hover:bg-sky-50 transition-all duration-200 focus:outline-none focus:ring-2 focus:ring-sky-500/40"
          @click="toggleNotifications"
        >
          <Bell :size="16" />
          <span
            class="absolute top-1.5 right-1.5 w-1.5 h-1.5 bg-sky-500 rounded-full"
          />
        </button>

        <AnimatePresence>
          <Motion
            v-if="showNotifications"
            :initial="{ opacity: 0, y: -8 }"
            :animate="{ opacity: 1, y: 0 }"
            :exit="{ opacity: 0, y: -8 }"
            class="absolute right-0 mt-2 w-80 bg-white border border-slate-200 rounded-xl shadow-xl overflow-hidden z-40"
          >
            <div
              class="flex items-center justify-between px-4 py-3 border-b border-slate-100"
            >
              <p class="text-sm font-semibold text-slate-800">
                Meldingen
              </p>

              <button
                class="text-slate-400 hover:text-slate-700"
                @click="showNotifications = false"
              >
                <X :size="14" />
              </button>
            </div>

            <div class="max-h-80 overflow-auto">
              <button
                v-for="n in notifications"
                :key="n.id"
                class="w-full flex items-start gap-3 px-4 py-3 hover:bg-slate-50 transition-colors duration-200 text-left border-b border-slate-50 last:border-0"
                @click="
                  toast.info(n.titel);
                  showNotifications = false;
                "
              >
                <span
                  :class="[
                    'w-2 h-2 rounded-full mt-1.5 shrink-0',
                    kleurMap[n.kleur],
                  ]"
                />

                <div class="flex-1 min-w-0">
                  <p class="text-sm font-semibold text-slate-800">
                    {{ n.titel }}
                  </p>

                  <p class="text-xs text-slate-500 mt-0.5">
                    {{ n.tekst }}
                  </p>

                  <p class="text-[10px] text-slate-400 mt-1">
                    {{ n.tijd }}
                  </p>
                </div>
              </button>
            </div>
          </Motion>
        </AnimatePresence>
      </div>

      <!-- Profile -->
      <div class="relative">
        <button
          class="flex items-center gap-2 pl-3 pr-2 py-1.5 rounded-lg border border-slate-200 bg-white hover:bg-slate-50 transition-all duration-200 focus:outline-none focus:ring-2 focus:ring-sky-500/40"
          @click="toggleProfile"
        >
          <div
            class="w-6 h-6 rounded-full bg-sky-500 flex items-center justify-center text-white text-[10px] font-bold"
          >
            A
          </div>

          <span class="text-sm text-slate-700 font-medium hidden sm:block">
            Admin
          </span>

          <ChevronDown
            :size="14"
            class="text-slate-400"
          />
        </button>

        <AnimatePresence>
          <Motion
            v-if="showProfile"
            :initial="{ opacity: 0, y: -8 }"
            :animate="{ opacity: 1, y: 0 }"
            :exit="{ opacity: 0, y: -8 }"
            class="absolute right-0 mt-2 w-56 bg-white border border-slate-200 rounded-xl shadow-xl overflow-hidden z-40"
          >
            <div class="px-4 py-3 border-b border-slate-100">
              <p class="text-sm font-semibold text-slate-800">
                Admin Beheerder
              </p>

              <p class="text-xs text-slate-500">
                admin@flevoland.nl
              </p>
            </div>

            <div class="py-1">
              <button
                class="w-full text-left px-4 py-2 text-sm text-slate-700 hover:bg-slate-50 transition-colors duration-200"
                @click="router.push('/profile'); showProfile = false"
              >
                Profiel
              </button>

              <button
                class="w-full text-left px-4 py-2 text-sm text-slate-700 hover:bg-slate-50 transition-colors duration-200"
                @click="router.push('/admin'); showProfile = false"
              >
                Instellingen
              </button>

              <button
                class="w-full text-left px-4 py-2 text-sm text-slate-700 hover:bg-slate-50 transition-colors duration-200"
                @click="router.push('/export'); showProfile = false"
              >
                Exporteer data
              </button>
            </div>

            <div class="border-t border-slate-100 py-1">
              <button
                class="w-full text-left px-4 py-2 text-sm text-rose-600 hover:bg-rose-50 transition-colors duration-200"
                @click="
                  toast.success('Uitgelogd');
                  showProfile = false;
                "
              >
                Uitloggen
              </button>
            </div>
          </Motion>
        </AnimatePresence>
      </div>
    </div>
  </header>
</template>