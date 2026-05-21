<script setup>
import { ref } from 'vue'
import { RouterLink, useRoute } from 'vue-router'

import {
  LayoutDashboard,
  Briefcase,
  TrendingUp,
  MapPin,
  Building2,
  Users,
  Settings,
  ChevronLeft,
  ChevronRight,
  BarChart3,
  Download,
} from 'lucide-vue-next'

const route = useRoute()

const collapsed = ref(false)

const navItems = [
  { to: '/', icon: LayoutDashboard, label: 'Dashboard' },
  { to: '/vacatures', icon: Briefcase, label: 'Vacatures' },
  { to: '/trends', icon: TrendingUp, label: 'Trends' },
  { to: '/regio', icon: MapPin, label: "Regio's" },
  { to: '/bedrijven', icon: Building2, label: 'Bedrijven' },
  { to: '/arbeidsmarkt', icon: Users, label: 'Arbeidsmarkt' },
  { to: '/statistieken', icon: BarChart3, label: 'Statistieken' },
  { to: '/export', icon: Download, label: 'Export' },
  { to: '/admin', icon: Settings, label: 'Admin' },
]

const isActive = (path) => {
  return path === '/'
    ? route.path === '/'
    : route.path.startsWith(path)
}
</script>

<template>
  <aside
    :class="[
      'relative flex flex-col bg-[hsl(220,25%,10%)] border-r border-white/5 transition-all duration-300 min-h-screen shrink-0',
      collapsed ? 'w-16' : 'w-64',
    ]"
  >
    <!-- Header -->
    <div class="flex items-center justify-between px-4 py-5 border-b border-white/5">
      <div v-if="!collapsed">
        <span
          class="font-heading text-white font-bold text-base tracking-tight leading-tight"
        >
          Flevoland
        </span>

        <p
          class="text-[10px] text-slate-400 font-mono uppercase tracking-widest mt-0.5"
        >
          Dev Arbeidsmarkt
        </p>
      </div>

      <button
        @click="collapsed = !collapsed"
        :aria-label="
          collapsed
            ? 'Sidebar uitklappen'
            : 'Sidebar inklappen'
        "
        class="ml-auto p-1.5 rounded-md text-slate-400 hover:text-white hover:bg-white/10 transition-all duration-200 focus:outline-none focus:ring-2 focus:ring-sky-500/50"
      >
        <ChevronRight v-if="collapsed" :size="16" />
        <ChevronLeft v-else :size="16" />
      </button>
    </div>

    <!-- Navigation -->
    <nav
      class="flex-1 py-4 px-2 space-y-0.5"
      aria-label="Hoofdnavigatie"
    >
      <RouterLink
        v-for="item in navItems"
        :key="item.to"
        :to="item.to"
        :title="collapsed ? item.label : ''"
        :class="[
          'flex items-center gap-3 px-3 py-2.5 rounded-lg text-sm font-medium transition-all duration-200 group',
          isActive(item.to)
            ? 'bg-sky-500/15 text-sky-400 border border-sky-500/20'
            : 'text-slate-400 hover:text-white hover:bg-white/5',
        ]"
      >
        <component
          :is="item.icon"
          :size="18"
          class="shrink-0"
        />

        <span v-if="!collapsed">
          {{ item.label }}
        </span>
      </RouterLink>
    </nav>

    <!-- Footer -->
    <div class="px-3 py-4 border-t border-white/5">
      <div
        v-if="!collapsed"
        class="bg-white/5 rounded-lg p-3"
      >
        <p
          class="text-[10px] text-slate-500 uppercase tracking-widest font-mono mb-1"
        >
          Laatste update
        </p>

        <p class="text-xs text-slate-300 font-medium">
          15 jan 2026, 08:00
        </p>

        <div class="flex items-center gap-1.5 mt-2">
          <span
            class="w-1.5 h-1.5 rounded-full bg-emerald-400 animate-pulse"
          />

          <span
            class="text-[10px] text-emerald-400"
          >
            Live data actief
          </span>
        </div>
      </div>

      <div
        v-else
        class="flex justify-center"
      >
        <span
          class="w-2 h-2 rounded-full bg-emerald-400 animate-pulse"
        />
      </div>
    </div>
  </aside>
</template>