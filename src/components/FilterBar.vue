<script setup>
import { ref, computed } from 'vue'
import { Filter, X } from 'lucide-vue-next'

// Emit filter changes
const emit = defineEmits(['filterChange'])

const filters = ref({
  regio: 'all',
  type: 'all',
  periode: '2025',
  niveau: 'all'
})

const showFilters = ref(false)

function update(key, value) {
  filters.value[key] = value
  emit('filterChange', { ...filters.value })
}

function reset() {
  filters.value = {
    regio: 'all',
    type: 'all',
    periode: '2025',
    niveau: 'all'
  }
  emit('filterChange', { ...filters.value })
}

const hasActive = computed(() =>
  Object.entries(filters.value).some(([k, v]) =>
    k === 'periode' ? v !== '2025' : v !== 'all'
  )
)
</script>

<template>
  <div class="bg-white border border-slate-200 rounded-xl p-4 mb-6">
    <!-- Header -->
    <div class="flex items-center justify-between">
      <div class="flex items-center gap-2">
        <Filter :size="16" class="text-sky-600" />
        <span class="text-sm font-semibold text-slate-700">Filters</span>

        <span
          v-if="hasActive"
          class="px-2 py-0.5 bg-sky-100 text-sky-700 text-xs rounded-full font-medium"
        >
          Actief
        </span>
      </div>

      <div class="flex items-center gap-2">
        <button
          v-if="hasActive"
          @click="reset"
          class="flex items-center gap-1 text-xs text-slate-500 hover:text-red-500 transition-colors duration-200"
        >
          <X :size="12" />
          Reset
        </button>

        <button
          @click="showFilters = !showFilters"
          class="text-xs text-sky-600 hover:text-sky-700 font-medium transition-colors duration-200 sm:hidden"
        >
          {{ showFilters ? 'Verbergen' : 'Tonen' }}
        </button>
      </div>
    </div>

    <!-- Filter grid -->
    <div
      :class="[
        'grid grid-cols-2 md:grid-cols-4 gap-3 mt-4',
        showFilters ? 'grid' : 'hidden sm:grid'
      ]"
    >
      <!-- Regio -->
      <div>
        <label class="block text-xs text-slate-500 mb-1 font-medium">Regio</label>
        <select
          v-model="filters.regio"
          @change="update('regio', filters.regio)"
          class="w-full text-sm border border-slate-200 rounded-lg px-3 py-2 bg-white focus:outline-none focus:ring-2 focus:ring-sky-500/40 focus:border-sky-400 transition-all duration-200 text-slate-700"
        >
          <option value="all">Heel Flevoland</option>
          <option value="almere">Almere</option>
          <option value="lelystad">Lelystad</option>
          <option value="emmeloord">Emmeloord</option>
          <option value="dronten">Dronten</option>
          <option value="zeewolde">Zeewolde</option>
          <option value="urk">Urk</option>
        </select>
      </div>

      <!-- Type -->
      <div>
        <label class="block text-xs text-slate-500 mb-1 font-medium">Type Developer</label>
        <select
          v-model="filters.type"
          @change="update('type', filters.type)"
          class="w-full text-sm border border-slate-200 rounded-lg px-3 py-2 bg-white focus:outline-none focus:ring-2 focus:ring-sky-500/40 focus:border-sky-400 transition-all duration-200 text-slate-700"
        >
          <option value="all">Alle types</option>
          <option value="frontend">Frontend Developer</option>
          <option value="backend">Backend Developer</option>
          <option value="api">API Developer</option>
          <option value="fullstack">Full-stack Developer</option>
        </select>
      </div>

      <!-- Periode -->
      <div>
        <label class="block text-xs text-slate-500 mb-1 font-medium">Periode</label>
        <select
          v-model="filters.periode"
          @change="update('periode', filters.periode)"
          class="w-full text-sm border border-slate-200 rounded-lg px-3 py-2 bg-white focus:outline-none focus:ring-2 focus:ring-sky-500/40 focus:border-sky-400 transition-all duration-200 text-slate-700"
        >
          <option value="2025">2025</option>
          <option value="2024">2024</option>
          <option value="2023">2023</option>
          <option value="q4-2025">Q4 2025</option>
          <option value="q3-2025">Q3 2025</option>
        </select>
      </div>

      <!-- Niveau -->
      <div>
        <label class="block text-xs text-slate-500 mb-1 font-medium">Niveau</label>
        <select
          v-model="filters.niveau"
          @change="update('niveau', filters.niveau)"
          class="w-full text-sm border border-slate-200 rounded-lg px-3 py-2 bg-white focus:outline-none focus:ring-2 focus:ring-sky-500/40 focus:border-sky-400 transition-all duration-200 text-slate-700"
        >
          <option value="all">Alle niveaus</option>
          <option value="junior">Junior (0–2 jr)</option>
          <option value="medior">Medior (2–5 jr)</option>
          <option value="senior">Senior (5+ jr)</option>
          <option value="lead">Lead / Principal</option>
        </select>
      </div>
    </div>
  </div>
</template>
